---
title: Перенос рабочих нагрузок Hive Azure HDInsight 3,6 в HDInsight 4,0
description: Узнайте, как перенести рабочие нагрузки Apache Hive в HDInsight 3,6 в HDInsight 4,0.
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: ec96189185a06c1fcbd95eed6216ade47f3089c3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "79214644"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Перенос рабочих нагрузок Hive Azure HDInsight 3,6 в HDInsight 4,0

В этом документе показано, как перенести рабочие нагрузки Apache Hive и LLAP в HDInsight 3,6 в HDInsight 4,0. HDInsight 4,0 предоставляет более новые функции Hive и LLAP, такие как материализованные представления и кэширование результатов запросов. При переносе рабочих нагрузок в HDInsight 4,0 можно использовать множество новых функций Hive 3, которые недоступны в HDInsight 3,6.

В этой статье рассматриваются следующие темы:

* Миграция метаданных Hive в HDInsight 4,0
* Безопасность переноса таблиц ACID и не ACID
* Сохранение политик безопасности Hive в версиях HDInsight
* Выполнение запросов и отладка из HDInsight 3,6 в HDInsight 4,0

Одним из преимуществ Hive является возможность экспорта метаданных во внешнюю базу данных (хранилище метаданных Hive). **Хранилище метаданных Hive** отвечает за хранение статистики таблицы, включая расположение хранилища таблиц, имена столбцов и сведения об индексах таблиц. Схема базы данных хранилище метаданных различается в разных версиях Hive. Для безопасного обновления хранилище метаданных Hive рекомендуется создать копию и обновить копию, а не текущую рабочую среду.

## <a name="copy-metastore"></a>Копировать хранилище метаданных

Для HDInsight 3,6 и HDInsight 4,0 требуются разные схемы хранилище метаданных и не могут совместно использовать один хранилище метаданных.

### <a name="external-metastore"></a>Внешние хранилище метаданных

Создайте новую копию внешнего хранилище метаданных. Если вы используете внешний хранилище метаданных, один из надежных и простых способов создания копии хранилище метаданных — [Восстановление базы данных](../../sql-database/sql-database-recovery-using-backups.md#point-in-time-restore) с другим именем с помощью функции восстановления базы данных SQL.  Дополнительные сведения о подключении внешних хранилище метаданных к кластеру HDInsight см. в статье [использование внешних хранилищ метаданных в Azure hdinsight](../hdinsight-use-external-metadata-stores.md) .

### <a name="internal-metastore"></a>Внутренняя хранилище метаданных

Если вы используете внутренний хранилище метаданных, можно использовать запросы для экспорта определений объектов в хранилище метаданных Hive и их импорта в новую базу данных.

После завершения этого сценария предполагается, что старый кластер больше не будет использоваться для доступа к любой из таблиц или баз данных, на которые ссылается скрипт.

> [!NOTE]
> В случае таблиц ACID будет создана новая копия данных, расположенных под таблицей.

1. Подключитесь к кластеру HDInsight с помощью [клиента Secure Shell (SSH)](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Подключитесь к HiveServer2 с помощью [клиента Beeline](../hadoop/apache-hadoop-use-hive-beeline.md) из открытого сеанса SSH, введя следующую команду:

    ```hiveql
    for d in `beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show databases;"`; 
    do
        echo "Scanning Database: $d"
        echo "create database if not exists $d; use $d;" >> alltables.hql; 
        for t in `beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show tables;"`;
        do
            echo "Copying Table: $t"
            ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;"`;

            echo "$ddl;" >> alltables.hql;
            lowerddl=$(echo $ddl | awk '{print tolower($0)}')
            if [[ $lowerddl == *"'transactional'='true'"* ]]; then
                if [[ $lowerddl == *"partitioned by"* ]]; then
                    # partitioned
                    raw_cols=$(beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;" | tr '\n' ' ' | grep -io "CREATE TABLE .*" | cut -d"(" -f2- | cut -f1 -d")" | sed 's/`//g');
                    ptn_cols=$(beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;" | tr '\n' ' ' | grep -io "PARTITIONED BY .*" | cut -f1 -d")" | cut -d"(" -f2- | sed 's/`//g');
                    final_cols=$(echo "(" $raw_cols "," $ptn_cols ")")

                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "create external table ext_$t $final_cols TBLPROPERTIES ('transactional'='false');";
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "insert into ext_$t select * from $t;";
                    staging_ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table ext_$t;"`;
                    dir=$(echo $staging_ddl | grep -io " LOCATION .*" | grep -m1 -o "'.*" | sed "s/'[^-]*//2g" | cut -c2-);

                    parsed_ptn_cols=$(echo $ptn_cols| sed 's/ [a-z]*,/,/g' | sed '$s/\w*$//g');
                    echo "create table flattened_$t $final_cols;" >> alltables.hql;
                    echo "load data inpath '$dir' into table flattened_$t;" >> alltables.hql;
                    echo "insert into $t partition($parsed_ptn_cols) select * from flattened_$t;" >> alltables.hql;
                    echo "drop table flattened_$t;" >> alltables.hql;
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "drop table ext_$t";
                else
                    # not partitioned
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "create external table ext_$t like $t TBLPROPERTIES ('transactional'='false');";
                    staging_ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table ext_$t;"`;
                    dir=$(echo $staging_ddl | grep -io " LOCATION .*" | grep -m1 -o "'.*" | sed "s/'[^-]*//2g" | cut -c2-);

                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "insert into ext_$t select * from $t;";
                    echo "load data inpath '$dir' into table $t;" >> alltables.hql;
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "drop table ext_$t";
                fi
            fi
            echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t;" >> alltables.hql;
        done;
    done
    ```

    Эта команда создает файл с именем **аллтаблес. HQL**.

1. Закройте сеанс SSH. Затем введите команду scp, чтобы загрузить **аллтаблес. HQL** локально.

    ```bash
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:alltables.hql c:/hdi
    ```

1. Отправьте **аллтаблес. HQL** в *Новый* кластер HDInsight.

    ```bash
    scp c:/hdi/alltables.hql sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/home/sshuser/
    ```

1. Затем с помощью SSH подключитесь к *новому* кластеру HDInsight. Запустите следующий код из сеанса SSH:

    ```bash
    beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" -i alltables.hql
    ```


## <a name="upgrade-metastore"></a>Обновление хранилище метаданных

После завершения **копирования** хранилище метаданных запустите сценарий обновления схемы в [действии сценария](../hdinsight-hadoop-customize-cluster-linux.md) в существующем кластере HDInsight 3,6, чтобы обновить схему хранилище метаданных до куста 3. Это позволяет подключить базу данных как HDInsight 4,0 хранилище метаданных.

Используйте значения в таблице ниже. Замените `SQLSERVERNAME DATABASENAME USERNAME PASSWORD` на соответствующие значения **копируемого** хранилище метаданных Hive, разделенные пробелами. При указании имени SQL Server не включайте ". database.windows.net".

|Свойство | Значение |
|---|---|
|Тип скрипта|- Custom|
|Имя|Обновление Hive|
|URI bash-скрипта|`https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/launch-schema-upgrade.sh`|
|Типы узлов|Head|
|Параметры|SQLSERVERNAME ИМЯ ПОЛЬЗОВАТЕЛЯ, ПАРОЛЬ|

> [!Warning]  
> Обновление, преобразующее схему метаданных HDInsight 3,6 в схему HDInsight 4,0, не может быть отменено.

Вы можете проверить обновление, выполнив следующий запрос SQL к базе данных:

```sql
select * from dbo.version
```

## <a name="migrate-hive-tables-to-hdinsight-40"></a>Перенос таблиц Hive в HDInsight 4,0

После выполнения предыдущего набора действий по переносу Hive хранилище метаданных в HDInsight 4,0 таблицы и базы данных, записанные в хранилище метаданных, будут отображаться в кластере HDInsight 4,0 путем выполнения `show tables` или `show databases` из кластера. Сведения о выполнении запросов в кластерах HDInsight 4,0 см. в разделе [выполнение запросов в разных версиях hdinsight](#query-execution-across-hdinsight-versions) .

Однако фактические данные из таблиц недоступны, пока кластер не будет иметь доступ к необходимым учетным записям хранения. Чтобы кластер HDInsight 4,0 мог получить доступ к тем же данным, что и ваш старый кластер HDInsight 3,6, выполните следующие действия.

1. Определите учетную запись хранения Azure для таблицы или базы данных.

1. Если кластер HDInsight 4,0 уже запущен, подключите учетную запись хранения Azure к кластеру через Ambari. Если вы еще не создали кластер HDInsight 4,0, убедитесь, что учетная запись хранения Azure указана как первичная или вторичная учетная запись хранения кластера. Дополнительные сведения о добавлении учетных записей хранения в кластеры HDInsight см. [в статье Добавление дополнительных учетных записей хранения в hdinsight](../hdinsight-hadoop-add-storage.md).

## <a name="deploy-new-hdinsight-40-and-connect-to-the-new-metastore"></a>Развертывание нового HDInsight 4,0 и подключение к новому хранилище метаданных

После завершения обновления схемы разверните новый кластер HDInsight 4,0 и подключите обновленный хранилище метаданных. Если вы уже развернули 4,0, настройте его так, чтобы вы могли подключиться к хранилище метаданных из Ambari.

## <a name="run-schema-migration-script-from-hdinsight-40"></a>Запуск скрипта миграции схемы из HDInsight 4,0

Таблицы в HDInsight 3,6 и HDInsight 4,0 обрабатываются по-разному. По этой причине нельзя использовать одни и те же таблицы для кластеров разных версий. Если вы хотите использовать HDInsight 3,6 в то же время, что и HDInsight 4,0, необходимо иметь отдельные копии данных для каждой версии.

Рабочая нагрузка Hive может включать в себя сочетание ACID и таблиц без ACID. Одно из ключевых различий между Hive в HDInsight 3,6 (Hive 2) и Hive в HDInsight 4,0 (Hive 3) является ACID для таблиц. В HDInsight 3,6 Включение соответствия Hive ACID требует дополнительной настройки, но в таблицах HDInsight 4,0 по умолчанию соответствуют ACID. Единственное действие, которое необходимо выполнить перед миграцией, — выполнение основной сжатия таблицы ACID в кластере 3,6. В представлении Hive или Beeline выполните следующий запрос:

```sql
alter table myacidtable compact 'major';
```

Это сжатие требуется, так как таблицы HDInsight 3,6 и HDInsight 4,0 ACID по-разному понимают различия ACID. Сжатие обеспечивает очистку, гарантирующую согласованность. Раздел 4 [документации по миграции Hive](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-upgrade-major/content/prepare_hive_for_upgrade.html) содержит рекомендации по массовым сжатию таблиц ACID в HDInsight 3,6.

После завершения процедуры миграции и сжатия хранилище метаданных можно перенести фактическое хранилище. После завершения миграции хранилища Hive хранилище HDInsight 4,0 будет иметь следующие свойства.

|3.6 |4,0 |
|---|---|
|Внешние таблицы|Внешние таблицы|
|Нетранзакционные управляемые таблицы|Внешние таблицы|
|Управляемые транзакционные таблицы|Управляемые таблицы|

Перед выполнением миграции может потребоваться настроить свойства хранилища. Например, если предполагается, что к некоторой таблице будет обращаться третья сторона (например, кластер HDInsight 3,6), эта таблица должна быть внешней по мере завершения миграции. В HDInsight 4,0 все управляемые таблицы являются транзакционными. Поэтому доступ к управляемым таблицам в HDInsight 4,0 должен осуществлять только кластеры HDInsight 4,0.

После правильной настройки свойств таблицы выполните средство миграции хранилища Hive из одного из головных узлах кластера с помощью оболочки SSH:

1. Подключитесь к кластеру головного узла с помощью SSH. Инструкции см. [в статье подключение к HDInsight с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) .
1. Откройте оболочку входа от имени пользователя Hive, выполнив`sudo su - hive`
1. Определите версию стека платформы данных, выполнив `ls /usr/hdp`. При этом отобразится строка версии, которую следует использовать в следующей команде.
1. Выполните следующую команду из оболочки. Замените `STACK_VERSION` на строку версии из предыдущего шага:

```bash
/usr/hdp/STACK_VERSION/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true -m automatic --modifyManagedTables
```

По завершении работы средства миграции хранилище Hive будет готово к HDInsight 4,0.

> [!Important]  
> Управляемые таблицы в HDInsight 4,0 (включая таблицы, перенесенные из 3,6) не должны быть доступны другим службам или приложениям, включая кластеры HDInsight 3,6.

## <a name="secure-hive-across-hdinsight-versions"></a>Защита Hive в версиях HDInsight

Начиная с HDInsight 3,6, HDInsight интегрируется с Azure Active Directory с помощью HDInsight Корпоративный пакет безопасности (ESP). ESP использует Kerberos и Apache Ranger для управления разрешениями конкретных ресурсов в кластере. Политики Ranger, развернутые с помощью Hive в HDInsight 3,6, можно перенести в HDInsight 4,0, выполнив следующие действия:

1. Перейдите на панель Service Manager Ranger в кластере HDInsight 3,6.
2. Перейдите к политике с именем **Hive** и экспортируйте политику в JSON-файл.
3. Убедитесь, что все пользователи, которые ссылаются на экспортированную политику JSON, существуют в новом кластере. Если пользователь упоминается в политике JSON, но не существует в новом кластере, добавьте пользователя в новый кластер или удалите ссылку из политики.
4. Перейдите на панель **Service Manager Ranger** в кластере HDInsight 4,0.
5. Перейдите к политике с именем **Hive** и импортируйте Ranger политику JSON из шага 2.

## <a name="check-compatibility-and-modify-codes-as-needed-in-test-app"></a>Проверка совместимости и изменение кодов при необходимости в тестовом приложении

При переносе рабочих нагрузок, таких как существующие программы и запросы, просмотрите заметки о выпуске и документацию, чтобы внести изменения и применить изменения по мере необходимости. Если кластер HDInsight 3,6 использует общий ресурс Spark и хранилище метаданных Hive, требуется [Дополнительная настройка с использованием соединителя хранилища Hive](./apache-hive-warehouse-connector.md) .

## <a name="deploy-new-app-for-production"></a>Развертывание нового приложения для рабочей среды

Чтобы переключиться на новый кластер, например, можно установить новое клиентское приложение и использовать его в качестве новой рабочей среды, или обновить существующее клиентское приложение и переключиться на HDInsight 4,0.

## <a name="switch-hdinsight-40-to-the-production"></a>Переключение HDInsight 4,0 в рабочую среду

Если во время тестирования были созданы различия в хранилище метаданных, необходимо обновить изменения непосредственно перед переключением. В этом случае можно экспортировать & импортировать хранилище метаданных, а затем снова выполнить обновление.

## <a name="remove-the-old-production"></a>Удаление старого рабочего

Убедившись, что выпуск завершен и полностью работоспособ, можно удалить версию 3,6 и предыдущую хранилище метаданных. Убедитесь, что все перенесено перед удалением среды.

## <a name="query-execution-across-hdinsight-versions"></a>Выполнение запросов в версиях HDInsight

Существует два способа выполнения и отладки запросов Hive/LLAP в кластере HDInsight 3,6. Хивекли предоставляет возможности командной строки, а представление представления/Hive Tez предоставляет рабочий процесс на основе графического пользовательского интерфейса.

В HDInsight 4,0 Хивекли был заменен на Beeline. Хивекли — это Thrift клиент для Hiveserver 1, а Beeline — клиент JDBC, предоставляющий доступ к Hiveserver 2. Beeline также можно использовать для подключения к любой другой конечной точке базы данных, совместимой с JDBC. Beeline доступен в HDInsight 4,0 без необходимости установки.

В HDInsight 3,6 клиент GUI для взаимодействия с сервером Hive является представлением Hive Ambari. HDInsight 4,0 не поставляется с Ambari представлением. Мы предоставили нашим клиентам способ использования Data Analytics Studio (DAS), который не является основной службой HDInsight. DAS не поставляется с кластерами HDInsight и не является официально поддерживаемым пакетом. Однако DAS можно установить в кластере с помощью [действия сценария](../hdinsight-hadoop-customize-cluster-linux.md) следующим образом:

|Свойство | Значение |
|---|---|
|Тип скрипта|- Custom|
|Имя|DAS|
|URI bash-скрипта|`https://hdiconfigactions.blob.core.windows.net/dasinstaller/LaunchDASInstaller.sh`|
|Типы узлов|Head|

Подождите 10 – 15 минут, а затем запустите Data Analytics Studio с помощью этого URL `https://CLUSTERNAME.azurehdinsight.net/das/`-адреса:.

Перед доступом к DAS может потребоваться обновить пользовательский интерфейс Ambari и (или) перезапустить все компоненты Ambari.

После установки DAS, если вы не видите запросы, которые вы выполняли в средстве просмотра запросов, выполните следующие действия.

1. Задайте конфигурации для Hive, TEZ и DAS, как описано в [этом разделе руководства по устранению неполадок установки Das](https://docs.hortonworks.com/HDPDocuments/DAS/DAS-1.2.0/troubleshooting/content/das_queries_not_appearing.html).
2. Убедитесь, что следующие конфигурации каталога службы хранилища Azure являются страничными BLOB-объектами и перечислены в разделе `fs.azure.page.blob.dirs`:
    * `hive.hook.proto.base-directory`
    * `tez.history.logging.proto-base-dir`
3. Перезапустите HDFS, Hive, TEZ и DAS в обоих головных узлах.

## <a name="next-steps"></a>Дальнейшие шаги

* [Объявление о HDInsight 4,0](../hdinsight-version-release.md)
* [HDInsight 4,0. подробное углубление](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/)
* [Таблицы ACID Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
