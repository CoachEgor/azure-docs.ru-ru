---
title: Устранение неполадок в Storm с помощью Azure HDInsight
description: Ответы на часто задаваемые вопросы об использовании Apache Storm с Azure HDInsight.
keywords: Azure HDInsight, Storm, вопросы и ответы, руководство по устранению неполадок, часто задаваемые вопросы
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 11/08/2019
ms.custom: seodec18
ms.openlocfilehash: e2cc9dd81a0bbefa5cf37facb6067bda07117eaf
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903720"
---
# <a name="troubleshoot-apache-storm-by-using-azure-hdinsight"></a>Устранение неполадок в Apache Storm с помощью Azure HDInsight

Ознакомьтесь с основными проблемами, которые возникают при работе с полезными данными [Apache Storm](https://storm.apache.org/) в [Apache Ambari](https://ambari.apache.org/), и способами их решения.

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a>Как получить доступ к пользовательскому интерфейсу Storm в кластере?

Есть два способа для получения доступа к пользовательскому интерфейсу Storm из браузера:

### <a name="apache-ambari-ui"></a>Пользовательский интерфейс Apache Ambari

1. Перейдите к панели мониторинга Ambari.
2. В списке служб выберите **Storm**.
3. В меню **Quick Links** (Быстрые ссылки) выберите **Storm UI** (Пользовательский интерфейс Storm).

### <a name="direct-link"></a>Прямая ссылка

Доступ к пользовательскому интерфейсу Storm можно получить по следующему URL-адресу:

`https://<cluster DNS name>/stormui`

Пример: `https://stormcluster.azurehdinsight.net/stormui`

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a>Как передать сведения о контрольной точке spout концентратора событий Storm из одной топологии в другую?

При разработке топологий, которые считывают данные из Центров событий Azure с помощью JAR-файла spout концентратора событий HDInsight Storm, необходимо развернуть топологию, которая имеет то же имя в новом кластере. Но нужно сохранить данные контрольных точек, которые были зафиксированы в [Apache ZooKeeper](https://zookeeper.apache.org/) на старом кластере.

### <a name="where-checkpoint-data-is-stored"></a>Где хранятся данные контрольных точек

Данные контрольных точек для смещений сохраняются объектом spout концентратора событий в Zookeeper. Имеется два корневых пути:

- Нетранзакционные контрольные точки spout хранятся в `/eventhubspout`.

- Данные контрольной точки spout транзакций хранятся в `/transactional`.

### <a name="how-to-restore"></a>Сведения о восстановлении

Чтобы получить скрипты и библиотеки, которые используются для экспорта данных из ZooKeeper, а затем импортируют данные обратно в ZooKeeper с новым именем, см. [примеры HDInsight Storm](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

В папке lib находятся JAR-файлы, содержащие реализацию для операции экспорта или импорта. Папка bash содержит пример скрипта, который демонстрирует, как экспортировать данные из сервера ZooKeeper в старом кластере, а затем импортировать его на сервер ZooKeeper в новом кластере.

Выполните скрипт [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) из узлов ZooKeeper для экспорта, а затем импорта данных. Обновите скрипт до правильной версии платформы данных Hortonworks Data Platform (HDP). (Мы работаем над универсальностью этих скриптов в HDInsight. Универсальные скрипты могут запускаться с любого узла кластера без необходимости внесения изменений пользователем.)

Команда экспорта записывает метаданные по пути распределенной файловой системы Apache Hadoop (HDFS) (в хранилище BLOB-объектов Azure или хранилище Azure Data Lake Storage) в указанном расположении.

### <a name="examples"></a>Примеры

#### <a name="export-offset-metadata"></a>Экспорт метаданных смещения

1. Используйте SSH для перехода в кластер ZooKeeper в кластере, из которого необходимо экспортировать смещение контрольной точки.
2. Выполните следующую команду (после обновления строки версии HDP), чтобы экспортировать данные смещения ZooKeeper в путь к `/stormmetadta/zkdata` HDFS:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Импорт метаданных смещения

1. Используйте SSH для перехода в кластер ZooKeeper в кластере, из которого необходимо импортировать смещение контрольной точки.
2. Выполните следующую команду (после обновления строки версии HDP), чтобы импортировать данные смещения ZooKeeper из пути HDFS `/stormmetadata/zkdata` на сервер ZooKeeper в целевом кластере:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```

#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a>Удалите метаданные смещения, чтобы топологии могли начать обработку данных с начала или метки времени (на усмотрение пользователя).

1. Используйте SSH для перехода в кластер ZooKeeper в кластере, из которого необходимо удалить смещение контрольной точки.
2. Выполните следующую команду (после обновления строки версии HDP) для удаления всех данных смещения ZooKeeper в текущем кластере:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Как найти двоичные файлы Storm в кластере?

Двоичные файлы с накоплением для текущего стека HDP находятся в `/usr/hdp/current/storm-client`. Расположение одинаковое как для головных узлов, так и для рабочих узлов.

Для конкретных версий HDP в/usr/HDP может существовать несколько двоичных файлов (например, `/usr/hdp/2.5.0.1233/storm`). Папка `/usr/hdp/current/storm-client` симлинкед до последней версии, которая работает в кластере.

Дополнительные сведения см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) и [Apache Storm](https://storm.apache.org/).

## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>Как определить топологию развертывания кластера Storm?

Сначала определяются все компоненты, установленные с помощью HDInsight Storm. Кластер Storm состоит из четырех категорий узлов:

* Узлы шлюза
* Головные узлы
* Узлы Zookeeper.
* Рабочие узлы

### <a name="gateway-nodes"></a>Узлы шлюза

Узел шлюза —это шлюз и служба обратного прокси-сервера, которая предоставляет общий доступ к активной службе управления Ambari. Он также управляет выбором лидера Ambari.

### <a name="head-nodes"></a>Головные узлы

Головные узлы Storm запускают следующие службы:
* Nimbus;
* сервер Ambari;
* сервер метрик Ambari;
* сборщик метрик Ambari.
 
### <a name="zookeeper-nodes"></a>Узлы Zookeeper.

HDInsight поставляется с кворумом Zookeeper, включающим три узла. Размер кворума является фиксированным и не может быть перенастроен.

Службы Storm в кластере настроены для автоматического использования кворума Zookeeper.

### <a name="worker-nodes"></a>Рабочие узлы

Рабочие узлы Storm запускают следующие службы:
* Supervisor;
* виртуальные машины Java (JVM) рабочей роли для выполнения топологий;
* агент Ambari.

## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Как найти двоичные файлы объекта spout концентратора событий Storm для разработки?

Дополнительные сведения об использовании JAR-файлов spout концентратора событий Storm с топологией см. на следующих ресурсах.

### <a name="java-based-topology"></a>Топология на основе Java

[Обработка событий из Центров событий Azure с помощью Apache Storm в HDInsight (Java)](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)

### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>Топология на основе C# (Mono в кластерах Linux Storm для HDInsight 3.4+)

[Обработка событий из Центров событий Azure с помощью Apache Storm в HDInsight (C#)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)

### <a name="latest-apache-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Последние двоичные файлы spout концентратора событий Apache Storm для кластеров Linux Storm для HDInsight 3.5+

Чтобы узнать, как использовать spout концентратора событий последних версий, который работает с кластерами кластеров HDInsight 3.5 + Linux, см. [файл readme mvn-](https://github.com/hdinsight/mvn-repo/blob/master/README.md)Repository.

### <a name="source-code-examples"></a>Примеры исходного кода

См. [примеры](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) того, как выполнять чтение и запись из концентратора событий Azure с помощью топологии Apache Storm (написанной на Java) в кластере Azure HDInsight.

## <a name="how-do-i-locate-storm-log4j-2-configuration-files-on-clusters"></a>Как найти файлы конфигурации Storm Log4J 2 в кластерах?

Ниже приведены сведения о том, как определить файлы конфигурации [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) для служб Storm.

### <a name="on-head-nodes"></a>На головных узлах

Конфигурация Log4J Nimbus считывается из `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`.

### <a name="on-worker-nodes"></a>На рабочих узлах

Конфигурация Log4Jа супервизора считывается из `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`.

Файл конфигурации Worker Log4J считывается из `/usr/hdp/\<HDP version>/storm/log4j2/worker.xml`.

Примеры: `/usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml`
`/usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml`

---

## <a name="not-a-leader-exception"></a>Не является исключением лидера

При отправке топологии пользователь может получить сообщение об ошибке следующего вида: `Topology submission exception, cause not a leader, the current leader is NimbusInfo`.

Чтобы устранить эту проблему, пользователю может потребоваться создать билет для перезапуска или перезагрузки узлов. Дополнительные сведения можно найти здесь: [https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html](https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html).

---

## <a name="next-steps"></a>Дополнительная информация

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

- Получите ответы от экспертов Azure через [службу поддержки сообщества Azure](https://azure.microsoft.com/support/community/).

- Подключайтесь с [@AzureSupport](https://twitter.com/azuresupport) — официальная учетная запись Microsoft Azure для улучшения качества взаимодействия с клиентами. Подключение сообщества Azure к нужным ресурсам: ответы, поддержка и эксперты.

- Если вам нужна дополнительная помощь, можно отправить запрос в службу поддержки из [портал Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите пункт **Поддержка** в строке меню или откройте центр **справки и поддержки** . Для получения более подробных сведений см. статью [о создании запроса на поддержку Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Доступ к поддержке в рамках управления подпиской и выставления счетов включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется через тот или иной [план поддержки Azure](https://azure.microsoft.com/support/plans/).
