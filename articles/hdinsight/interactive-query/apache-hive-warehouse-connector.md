---
title: Apache Spark & Hive — соединитель хранилища Hive — Azure HDInsight
description: Узнайте, как интегрировать Apache Spark и Apache Hive с соединителем хранилища Hive в Azure HDInsight.
author: nakhanha
ms.author: nakhanha
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/08/2019
ms.openlocfilehash: 765bbc352c493124c1adec68eff456f4d0de3d49
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75744872"
---
# <a name="integrate-apache-spark-and-apache-hive-with-the-hive-warehouse-connector"></a>Интеграция Apache Spark и Apache Hive с соединителем хранилища Hive

Соединитель хранилища Apache Hive (HWC) — это библиотека, которая позволяет более легко работать с Apache Spark и Apache Hive с помощью вспомогательных задач, таких как перемещение данных между кадрами и таблицами Hive в Spark, а также направление данных потоковой передачи Spark в таблицы Hive. Соединитель хранилища Hive работает как мост между Spark и Hive. Он поддерживает Scala, Java и Python для разработки.

Соединитель хранилища Hive позволяет использовать преимущества уникальных функций Hive и Spark для создания мощных приложений для работы с большими данными. Apache Hive предлагает поддержку для транзакций базы данных, которые являются атомарными, постоянными, изолированными и устойчивыми (ACID). Дополнительные сведения о ACID и транзакциях в Hive см. в разделе [транзакции Hive](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions). Hive также предлагает подробные элементы управления безопасностью с помощью Apache Ranger и аналитической обработки с низкой задержкой, которая недоступна в Apache Spark.

Apache Spark имеет интерфейс API структурированной потоковой передачи, который предоставляет возможности потоковой передачи, недоступные в Apache Hive. Начиная с HDInsight 4,0, Apache Spark 2.3.1 и Apache Hive 3.1.0 имеют отдельные метахранилища, что может усложнить взаимодействие. Соединитель хранилища Hive упрощает совместное использование Spark и Hive. Библиотека HWC загружает данные из управляющих программ в исполнители Spark параллельно, делая ее более эффективной и масштабируемой, чем использование стандартного подключения JDBC из Spark к Hive.

![Архитектура соединителя хранилища Hive](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Ниже перечислены некоторые операции, поддерживаемые соединителем хранилища Hive.

* Описание таблицы
* Создание таблицы для данных в формате ORC
* Выбор данных Hive и получение таблицы
* Запись кадров данных в Hive в пакетной службе
* Выполняя инструкцию обновления Hive
* Чтение данных таблицы из Hive, их преобразование в Spark и запись в новую таблицу Hive
* Запись кадра данных или потока Spark в Hive с помощью Хивестреаминг

## <a name="hive-warehouse-connector-setup"></a>Установка соединителя хранилища Hive

Выполните следующие действия, чтобы настроить соединитель хранилища Hive между кластером Spark и интерактивным запросом в Azure HDInsight:

### <a name="create-clusters"></a>Создание кластеров

1. Создайте кластер HDInsight Spark **4,0** с учетной записью хранения и настраиваемой виртуальной сетью Azure. Сведения о создании кластера в виртуальной сети Azure см. в статье [Добавление HDInsight в существующую виртуальную сеть](../../hdinsight/hdinsight-plan-virtual-network-deployment.md#existingvnet).

1. Создайте кластер HDInsight Interactive query (LLAP) **4,0** с той же учетной записью хранения и виртуальной сетью Azure, что и кластер Spark.

### <a name="modify-hosts-file"></a>Изменение файла узлов

Скопируйте сведения об узле из файла `/etc/hosts` на headnode0 в кластере интерактивных запросов и объедините данные в файл `/etc/hosts` в headnode0 кластера Spark. Этот шаг позволит кластеру Spark разрешать IP-адреса узлов в кластере интерактивных запросов. Просмотрите содержимое обновленного файла с помощью `cat /etc/hosts`. Окончательный результат должен выглядеть примерно так, как показано на снимке экрана ниже.

![соединитель хранилища Hive файл hosts](./media/apache-hive-warehouse-connector/hive-warehouse-connector-hosts-file.png)

### <a name="gather-preliminary-information"></a>Сбор предварительных сведений

#### <a name="from-your-interactive-query-cluster"></a>Из кластера интерактивных запросов

1. Перейдите на домашнюю страницу Apache Ambari кластера, используя `https://LLAPCLUSTERNAME.azurehdinsight.net`, где `LLAPCLUSTERNAME` — имя кластера интерактивных запросов.

1. Перейдите в раздел > **конфигурации** **Hive** > **Дополнительно** > **Расширенный Hive — сайт** > **Hive. Zookeeper. Кворум** и запишите значение. Значение может быть похоже на: `zk0-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181,zk1-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181,zk4-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181`.

1. Выберите **куст** > **конфигурации** > **Дополнительно** > **Общие** > **Hive. хранилище метаданных. URI** и запишите значение. Значение может быть похоже на: `thrift://iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:9083,thrift://hn1-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:9083`.

#### <a name="from-your-apache-spark-cluster"></a>Из кластера Apache Spark

1. Перейдите на домашнюю страницу Apache Ambari кластера, используя `https://SPARKCLUSTERNAME.azurehdinsight.net`, где `SPARKCLUSTERNAME` — имя кластера Apache Spark.

1. Перейдите к разделу **Hive** > **конфигурации** > **Дополнительно** > **Расширенный куст-Interactive-сайт** > **Hive. llap. Демон. Service. Hosts** и запишите значение. Значение может быть похоже на: `@llap0`.

### <a name="configure-spark-cluster-settings"></a>Настройка параметров кластера Spark

В пользовательском веб-интерфейсе Spark Ambari перейдите к **Spark2** > **configs** > **Custom Spark2 — Defaults**.

![Настройка Spark2 Apache Ambari](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

Выберите **Добавить свойство...** при необходимости добавить или обновить следующее:

| Ключ | Значение |
|----|----|
|`spark.hadoop.hive.llap.daemon.service.hosts`|Значение, полученное ранее из **Hive. llap. Демон. Service. Hosts**.|
|`spark.sql.hive.hiveserver2.jdbc.url`|`jdbc:hive2://LLAPCLUSTERNAME.azurehdinsight.net:443/;user=admin;password=PWD;ssl=true;transportMode=http;httpPath=/hive2`. Укажите строку подключения JDBC, которая подключается к Hiveserver2 в кластере интерактивных запросов. Замените `LLAPCLUSTERNAME` именем кластера интерактивных запросов. Замените `PWD` фактическим паролем.|
|`spark.datasource.hive.warehouse.load.staging.dir`|`wasbs://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp`. Задайте подходящий HDFS-совместимый промежуточный каталог. При наличии двух разных кластеров промежуточный каталог должен быть папкой в промежуточном каталоге учетной записи хранения кластера LLAP, чтобы у HiveServer2 был доступ к нему.  Замените `STORAGE_ACCOUNT_NAME` именем учетной записи хранения, используемой кластером, и `STORAGE_CONTAINER_NAME` именем контейнера хранилища.|
|`spark.datasource.hive.warehouse.metastoreUri`|Значение, полученное ранее из **Hive. хранилище метаданных. URI**.|
|`spark.security.credentials.hiveserver2.enabled`|`false` для режима развертывания клиента YARN.|
|`spark.hadoop.hive.zookeeper.quorum`|Значение, полученное ранее из **Hive. Zookeeper. кворума**.|

При необходимости сохраните изменения и перезапустите компоненты.

## <a name="using-the-hive-warehouse-connector"></a>Использование соединителя хранилища Hive

### <a name="connecting-and-running-queries"></a>Подключение и выполнение запросов

Вы можете выбрать один из нескольких различных методов подключения к кластеру интерактивных запросов и выполнения запросов с помощью соединителя хранилища Hive. Поддерживаемые методы включают следующие средства:

* [оболочка Spark](../spark/apache-spark-shell.md)
* PySpark
* Spark — отправка
* [Zeppelin](../spark/apache-spark-zeppelin-notebook.md)
* [Livy](../spark/apache-spark-livy-rest-interface.md)

Все примеры, приведенные в этой статье, будут выполняться с помощью Spark-Shell.

Чтобы запустить сеанс Spark-Shell, выполните следующие действия.

1. Подключитесь по протоколу SSH к головного узла кластеру Apache Spark. Дополнительные сведения о подключении к кластеру с помощью SSH см. [в статье подключение к HDInsight (Apache Hadoop) с помощью SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Введите следующую команду, чтобы запустить оболочку Spark:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-<STACK_VERSION>.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

    Вы увидите приветственное сообщение и запрос `scala>`, где можно ввести команды.

1. После запуска Spark-Shell экземпляр соединителя хранилища Hive можно запустить с помощью следующих команд:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="connecting-and-running-queries-on-enterprise-security-package-esp-clusters"></a>Подключение и выполнение запросов в кластерах Корпоративный пакет безопасности (ESP)

Корпоративный пакет безопасности (ESP) предоставляет возможности корпоративного класса, такие как проверка подлинности на основе Active Directory, поддержка нескольких пользователей и управление доступом на основе ролей для кластеров Apache Hadoop в Azure HDInsight. Дополнительные сведения о ESP см. [в статье использование Корпоративный пакет безопасности в HDInsight](../domain-joined/apache-domain-joined-architecture.md).

1. Подключитесь по протоколу SSH к головного узла кластеру Apache Spark. Дополнительные сведения о подключении к кластеру с помощью SSH см. [в статье подключение к HDInsight (Apache Hadoop) с помощью SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Введите `kinit` и войдите с помощью пользователя домена.

1. Запустите Spark-Shell с полным списком параметров конфигурации, как показано ниже. Все значения всех прописных букв между угловыми скобками должны быть указаны на основе кластера. Если необходимо найти значения для ввода для любого из приведенных ниже параметров, см. раздел о [настройке соединителя хранилища Hive](#hive-warehouse-connector-setup).

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-<STACK_VERSION>.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    --conf spark.hadoop.hive.llap.daemon.service.hosts='<LLAP_APP_NAME>'
    --conf spark.sql.hive.hiveserver2.jdbc.url='jdbc:hive2://<ZOOKEEPER_QUORUM>;serviceDiscoveryMode=zookeeper;zookeeperNamespace=hiveserver2-interactive'
    --conf spark.datasource.hive.warehouse.load.staging.dir='<STAGING_DIR>'
    --conf spark.datasource.hive.warehouse.metastoreUri='<METASTORE_URI>'
    --conf spark.hadoop.hive.zookeeper.quorum='<ZOOKEEPER_QUORUM>'
   ```

### <a name="creating-spark-dataframes-from-hive-queries"></a>Создание кадров данных Spark из запросов Hive

Результаты всех запросов, использующих библиотеку HWC, возвращаются в виде кадров данных. В следующих примерах показано, как создать базовый запрос.

```scala
hive.setDatabase("default")
val df = hive.executeQuery("select * from hivesampletable")
df.filter("state = 'Colorado'").show()
```

Результаты запроса представляют собой фрагменты данных Spark, которые можно использовать с библиотеками Spark, такими как МЛИБ и SparkSQL.

### <a name="writing-out-spark-dataframes-to-hive-tables"></a>Запись кадров данных Spark в таблицы Hive

Spark изначально не поддерживает запись в управляемые вами Hive таблицы. Однако с помощью HWC можно записать все кадры данных в таблицу Hive. Эту функцию можно увидеть на работе в следующем примере:

1. Создайте таблицу с именем `sampletable_colorado` и укажите ее столбцы с помощью следующей команды:

    ```scala
    hive.createTable("sampletable_colorado").column("clientid","string").column("querytime","string").column("market","string").column("deviceplatform","string").column("devicemake","string").column("devicemodel","string").column("state","string").column("country","string").column("querydwelltime","double").column("sessionid","bigint").column("sessionpagevieworder","bigint").create()
    ```

1. Отфильтруйте таблицу `hivesampletable`, где `state` столбец равен `Colorado`. Этот запрос таблицы Hive возвращается в виде кадра данных Spark. Затем кадр данных сохраняется в таблице Hive `sampletable_colorado` с помощью функции `write`.

    ```scala
    hive.table("hivesampletable").filter("state = 'Colorado'").write.format(HiveWarehouseSession.HIVE_WAREHOUSE_CONNECTOR).option("table","sampletable_colorado").save()
    ```

1. Просмотрите результаты с помощью следующей команды:

    ```scala
    hive.table("sampletable_colorado").show()
    ```
    
    ![соединитель хранилища Hive "отобразить таблицу Hive"](./media/apache-hive-warehouse-connector/hive-warehouse-connector-show-hive-table.png)

### <a name="structured-streaming-writes"></a>Операции записи структурированной потоковой передачи

С помощью соединителя хранилища Hive можно использовать потоковую передачу Spark для записи данных в таблицы Hive.

Выполните следующие действия, чтобы создать пример соединителя хранилища Hive, который принимает данные из потока Spark по порту localhost 9999 в таблицу Hive.

1. Выполните действия, описанные в разделе [подключение и выполнение запросов](#connecting-and-running-queries).

1. Начните поток Spark с помощью следующей команды:

    ```scala
    val lines = spark.readStream.format("socket").option("host", "localhost").option("port",9999).load()
    ```

1. Создайте данные для созданного потока Spark, выполнив следующие действия.
    1. Откройте второй сеанс SSH в том же кластере Spark.
    1. В командной строке введите `nc -lk 9999`. Эта команда использует служебную программу неткат для отправки данных из командной строки в указанный порт.

1. Вернитесь к первому сеансу SSH и создайте новую таблицу Hive для хранения данных потоковой передачи. В Spark-Shell введите следующую команду:

    ```scala
    hive.createTable("stream_table").column("value","string").create()
    ```

1. Затем запишите данные потоковой передачи во вновь созданную таблицу с помощью следующей команды:

    ```scala
    lines.filter("value = 'HiveSpark'").writeStream.format(HiveWarehouseSession.STREAM_TO_STREAM).option("database", "default").option("table","stream_table").option("metastoreUri",spark.conf.get("spark.datasource.hive.warehouse.metastoreUri")).option("checkpointLocation","/tmp/checkpoint1").start()
    ```

    >[!Important]
    > В настоящее время параметры `metastoreUri` и `database` должны быть заданы вручную из-за известной проблемы в Apache Spark. Дополнительные сведения об этой ошибке см. в статье [Spark-25460](https://issues.apache.org/jira/browse/SPARK-25460).

1. Вернитесь к второму сеансу SSH и введите следующие значения:

    ```bash
    foo
    HiveSpark
    bar
    ```

1. Вернитесь к первому сеансу SSH и обратите внимание на краткое действие. Для просмотра данных используйте следующую команду:

    ```scala
    hive.table("stream_table").show()
    ```

**Нажмите клавиши CTRL + C** , чтобы прерывать неткат во втором сеансе SSH. Используйте `:q`, чтобы выйти из Spark-Shell в первом сеансе SSH.

### <a name="securing-data-on-spark-esp-clusters"></a>Защита данных в кластерах Spark ESP

1. Создайте таблицу `demo` с некоторыми образцами данных, введя следующие команды:

    ```scala
    create table demo (name string);
    INSERT INTO demo VALUES ('HDinsight');
    INSERT INTO demo VALUES ('Microsoft');
    INSERT INTO demo VALUES ('InteractiveQuery');
    ```

1. Просмотрите содержимое таблицы с помощью следующей команды. Перед применением политики в `demo` таблице отображается полный столбец.

    ```scala
    hive.executeQuery("SELECT * FROM demo").show()
    ```

    ![Демонстрационная таблица перед применением политики Ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-before-ranger-policy.png)

1. Примените политику маскирования столбцов, которая будет показывать только последние четыре символа столбца.  
    1. Перейдите в пользовательский интерфейс администратора Ranger по адресу `https://CLUSTERNAME.azurehdinsight.net/ranger/`.
    1. Щелкните службу Hive для кластера в разделе **Hive**.
        ![Ranger Service Manager](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. Щелкните вкладку **маскирование** , а затем — **Добавить новую политику** .

        ![Ranger соединителя хранилища Hive список политик Hive](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png)

    а. Укажите имя нужной политики. Выбор базы данных: **по умолчанию**, таблица Hive: **демонстрация**, столбец Hive: **имя**, пользователь: **Rsadmin2**, типы доступа: **Выбор**и **частичная маска: показывать последние 4** из меню **параметров выбора маски** . Нажмите кнопку **Добавить**.
                ![создать](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png) политики
1. Снова просмотрите содержимое таблицы. После применения политики Ranger можно увидеть только последние четыре символа столбца.

    ![Демонстрационная таблица после применения политики Ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Дальнейшие действия

* [Use Interactive Query with HDInsight](https://docs.microsoft.com/azure/hdinsight/interactive-query/apache-interactive-query-get-started) (Использование Interactive Query в HDInsight)
* [Примеры взаимодействия с соединителем хранилища Hive с помощью Zeppelin, Livy, Spark-Submit и pyspark](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)
