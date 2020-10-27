---
title: Руководства по устранению неполадок с Azure HDInsight
description: Устранение неполадок рабочих нагрузок Apache Hadoop с помощью Azure HDInsight. В документации с пошаговыми инструкциями показано, как с помощью HDInsight устранять распространенные неполадки с Apache Hive, Apache Spark, Apache YARN, Apache HBase, HDFS и Apache Storm.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/14/2019
ms.openlocfilehash: 12448d3e5e77d05f3b8fbd474179c4fcf7c57acc
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2020
ms.locfileid: "92535082"
---
# <a name="troubleshoot-by-using-azure-hdinsight"></a>Устранение неполадок при помощи Azure HDInsight

| Рабочая нагрузка Apache | Основные вопросы |
|---|---|
|![значок значка hdinsight Apache HBase](./media/hdinsight-troubleshoot-guide/hdinsight-apache-hbase.png)<br>[Устранение неполадок с Apache HBase]()|<br>[Неназначенные регионы](hbase/hbase-troubleshoot-unassigned-regions.md#scenario-unassigned-regions)<br><br>[Истечение времени ожидания при выполнении команды "hbase hbck" в Azure HDInsight](hbase/hbase-troubleshoot-timeouts-hbase-hbck.md)<br><br>[Проблемы с подключением к Apache Phoenix в Azure HDInsight](hbase/hbase-troubleshoot-phoenix-connectivity.md)<br><br>[Что вызывает сбой запуска главного сервера?](hbase/hbase-troubleshoot-start-fails.md)<br><br>[Биндексцептион-Address уже используется](hbase/hbase-troubleshoot-bindexception-address-use.md)|
|![значок значка hdinsight Apache HDFS](./media/hdinsight-troubleshoot-guide/hdinsight-apache-hdfs.png)<br>[Устранение неполадок с Apache Hadoop HDFS](hdinsight-troubleshoot-hdfs.md)|<br>[Разделы справки доступ к локальной системе HDFS из кластера?](hdinsight-troubleshoot-hdfs.md#how-do-i-access-local-hdfs-from-inside-a-cluster)<br><br>[Локальная служба HDFS зависает в защищенном режиме в кластере Azure HDInsight](hadoop/hdinsight-hdfs-troubleshoot-safe-mode.md)|
|![значок значка hdinsight Apache Hive](./media/hdinsight-troubleshoot-guide/hdinsight-apache-hive.png)<br>[Устранение неполадок Apache Hive](hdinsight-troubleshoot-hive.md)|<br>[Как экспортировать хранилище метаданных Hive и импортировать его в другой кластер?](hdinsight-troubleshoot-hive.md#how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster)<br><br>[Как найти журналы Apache Hive в кластере?](hdinsight-troubleshoot-hive.md#how-do-i-locate-hive-logs-on-a-cluster)<br><br>[Как запустить оболочку Apache Hive с определенными конфигурациями в кластере?](hdinsight-troubleshoot-hive.md#how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster)<br><br>[Как анализировать данные направленного ациклического графа Apache Tez по критическому пути кластера?](hdinsight-troubleshoot-hive.md#how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path)<br><br>[Как скачать данные направленного ациклического графа Apache Tez из кластера?](hdinsight-troubleshoot-hive.md#how-do-i-download-tez-dag-data-from-a-cluster)|
|![значок Apache Spark для hdinsight](./media/hdinsight-troubleshoot-guide/hdinsight-apache-spark.png)<br>[Устранение неполадок с Apache Spark](./spark/apache-troubleshoot-spark.md)|<br>[Как настроить приложение Apache Spark с помощью Apache Ambari в кластерах?](spark/apache-troubleshoot-spark.md#how-do-i-configure-an-apache-spark-application-by-using-apache-ambari-on-clusters)<br><br>[Как настроить приложение Apache Spark с помощью Jupyter Notebook в кластерах?](spark/apache-troubleshoot-spark.md#how-do-i-configure-an-apache-spark-application-by-using-a-jupyter-notebook-on-clusters)<br><br>[Как настроить приложение Apache Spark с помощью Apache Livy в кластерах?](spark/apache-troubleshoot-spark.md#how-do-i-configure-an-apache-spark-application-by-using-apache-livy-on-clusters)<br><br>[Как настроить приложение Apache Spark с помощью spark-submit в кластерах?](spark/apache-troubleshoot-spark.md#how-do-i-configure-an-apache-spark-application-by-using-spark-submit-on-clusters)<br><br>[Как настроить приложение Apache Spark с помощью IntelliJ?](spark/apache-spark-intellij-tool-plugin.md)<br><br>[Как настроить приложение Apache Spark с помощью Eclipse?](spark/apache-spark-eclipse-tool-plugin.md)<br><br>[Как настроить приложение Apache Spark с помощью VSCode?](hdinsight-for-vscode.md)<br><br>[Исключение OutOfMemoryError для Apache Spark](spark/apache-spark-troubleshoot-outofmemory.md#scenario-outofmemoryerror-exception-for-apache-spark)|
|![значок Apache в hdinsight](./media/hdinsight-troubleshoot-guide/hdinsight-apache-storm.png)<br>[Устранение неполадок с Apache Storm](./storm/apache-troubleshoot-storm.md)|<br>[Как получить доступ к пользовательскому интерфейсу Apache Storm в кластере?](storm/apache-troubleshoot-storm.md#how-do-i-access-the-storm-ui-on-a-cluster)<br><br>[Как передать сведения о контрольной точке spout концентратора событий Apache Storm из одной топологии в другую?](storm/apache-troubleshoot-storm.md#how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another)<br><br>[Как найти двоичные файлы Storm в кластере?](storm/apache-troubleshoot-storm.md#how-do-i-locate-storm-binaries-on-a-cluster)<br><br>[Как определить топологию развертывания кластера Storm?](storm/apache-troubleshoot-storm.md#how-do-i-determine-the-deployment-topology-of-a-storm-cluster)<br><br>[Как найти двоичные файлы объекта spout концентратора событий Apache Storm для разработки?](storm/apache-troubleshoot-storm.md#how-do-i-locate-storm-event-hub-spout-binaries-for-development)|
|![значок значка hdinsight Apache YARN](./media/hdinsight-troubleshoot-guide/hdinsight-apache-yarn.png)<br>[Устранение неполадок с Apache Hadoop YARN](hdinsight-troubleshoot-YARN.md)|<br>[Как создать очередь Apache Hadoop YARN в кластере?](hdinsight-troubleshoot-yarn.md#how-do-i-create-a-new-yarn-queue-on-a-cluster)<br><br>[Как скачать журналы Apache Hadoop YARN из кластера?](hdinsight-troubleshoot-yarn.md#how-do-i-download-yarn-logs-from-a-cluster)|

## <a name="hdinsight-troubleshooting-resources"></a>Ресурсы по устранению неполадок HDInsight

| Сведения о | Справочные материалы |
| --- | --- |
| HDInsight в Linux и оптимизация | - [Сведения об использовании HDInsight в Linux](hdinsight-hadoop-linux-information.md)<br>- [Указатель статей об устранении неполадок Apache Hadoop в HDInsight](hdinsight-hadoop-stack-trace-error-messages.md)<br>- [Производительность запросов Apache Hive](https://web.archive.org/web/20190217214250/https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/) |
| Журналы и дампы | - [Доступ к журналам приложений Apache Hadoop YARN в Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)<br>- [Включение дампов кучи для служб Apache Hadoop в Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)|
| ошибки | - [Понимание и устранение ошибок WebHCat](hdinsight-hadoop-templeton-webhcat-debug-errors.md)<br>- [Параметры Apache Hive для устранения ошибки OutofMemory (нехватка памяти)](hdinsight-hadoop-hive-out-of-memory-error-oom.md) |
| Инструменты | - [Оптимизация запросов Apache Hive](hdinsight-hadoop-optimize-hive-query.md)<br>- [Средство HDInsight для IntelliJ](./spark/apache-spark-intellij-tool-plugin.md)<br>- [Средство HDInsight для Eclipse](./spark/apache-spark-eclipse-tool-plugin.md)<br>- [Средство HDInsight для VSCode](hdinsight-for-vscode.md)<br>- [Средство HDInsight для Visual Studio](./hadoop/apache-hadoop-visual-studio-tools-get-started.md) |

## <a name="next-steps"></a>Дальнейшие действия

Если вы не видите своего варианта проблемы или вам не удается ее устранить, дополнительные сведения можно получить, посетив один из следующих каналов.

* Получите ответы специалистов Azure на [сайте поддержки сообщества пользователей Azure](https://azure.microsoft.com/support/community/).

* Подпишитесь на [@AzureSupport](https://twitter.com/azuresupport) — официальный канал Microsoft Azure для работы с клиентами. Вступайте в сообщество Azure для получения нужных ресурсов: ответов, поддержки и советов экспертов.

* Если вам нужна дополнительная помощь, отправьте запрос в службу поддержки на [портале Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Выберите **Поддержка** в строке меню или откройте центр **Справка и поддержка** . Дополнительные сведения см. в статье [Создание запроса на поддержку Azure](../azure-portal/supportability/how-to-create-azure-support-request.md). Доступ к управлению подписками и поддержкой выставления счетов уже включен в вашу подписку Microsoft Azure, а техническая поддержка предоставляется в рамках одного из [планов Службы поддержки Azure](https://azure.microsoft.com/support/plans/).
