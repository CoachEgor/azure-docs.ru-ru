---
title: Отправка заданий в кластер Spark в Azure HDInsight с помощью Livy Spark
description: Узнайте, как выполнить удаленную отправку заданий Spark в кластер Azure HDInsight с помощью Apache Spark REST API.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: c8504c6bf25b186a4bc87c4e7565444dd3e57209
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64570486"
---
# <a name="use-apache-spark-rest-api-to-submit-remote-jobs-to-an-hdinsight-spark-cluster"></a>Удаленная отправка заданий Spark в кластер Azure HDInsight с помощью Apache Spark REST API

Узнайте, как использовать [Apache Livy](https://livy.incubator.apache.org/), интерфейс [Apache Spark](https://spark.apache.org/) REST API, который применяется для удаленной отправки заданий в кластер Azure HDInsight Spark. Подробную документацию см. здесь: [https://livy.incubator.apache.org/](https://livy.incubator.apache.org/).

Вы можете использовать Livy для выполнения интерактивных оболочек Spark или отправки пакетных заданий для запуска в кластере Spark. В статье рассматривается использование Livy для отправки пакетных заданий. Во фрагментах кода в этой статье используется Curl для отправки вызовов REST API к конечной точке Livy Spark.

**Предварительные требования:**

* Кластер Apache Spark в HDInsight. Инструкции см. в статье [Начало работы. Создание кластера Apache Spark в HDInsight на платформе Linux и выполнение интерактивных запросов с помощью SQL Spark](apache-spark-jupyter-spark-sql.md).

* [cURL](https://curl.haxx.se/). В этой статье для демонстрации вызовов REST API к кластеру HDInsight Spark используется cURL.

## <a name="submit-an-apache-livy-spark-batch-job"></a>Отправка пакетного задания Apache Livy Spark
Перед отправкой пакетного задания необходимо загрузить JAR-файл приложения в хранилище кластеров, связанное с соответствующим кластером. Вы можете использовать для этого служебную программу командной строки [**AzCopy**](../../storage/common/storage-use-azcopy.md). Кроме того, для отправки данных вы можете использовать множество других клиентов. Дополнительные сведения о них см. в статье [Отправка данных для заданий Apache Hadoop в HDInsight](../hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H "Content-Type: application/json" -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches' -H "X-Requested-By: admin"

**Примеры**:

* Если JAR-файл находится в хранилище кластеров (WASB)
  
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
* Если имя JAR-файла и имя класса необходимо передать в составе входного файла (в данном примере input.txt).
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"

## <a name="get-information-on-livy-spark-batches-running-on-the-cluster"></a>Получение сведений о пакетах Livy Spark, выполняемых в кластере
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Примеры**:

* Если вы хотите получить все пакеты Livy Spark, выполняемые в кластере:
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches" 
* Если вы хотите получить пакет с определенным идентификатором
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a>Удаление пакетного задания Livy Spark
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Пример**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a>Livy Spark и высокая доступность
Livy обеспечивает высокую доступность заданий Spark, выполняемых в кластере. Вот несколько примеров.

* Если после удаленной отправки задания в кластер Spark служба Livy перестает работать, задание продолжает выполняться в фоновом режиме. Выполняя резервное копирование, служба Livy восстанавливает состояние задания и отправляет отчет.
* Служба Livy в серверной части обеспечивает работу записных книжек Jupyter для HDInsight. Если записная книжка выполняет задание Spark, а служба Livy перезапускается, записная книжка продолжит выполнять ячейки кода. 

## <a name="show-me-an-example"></a>Показать пример
В этом разделе мы рассмотрим примеры использования Livy Spark для отправки пакетного задания, проследим за ходом выполнения задания, а затем удалим это задание. В примере используется приложение, разработанное в статье [Создание автономного приложения Scala для работы в кластере HDInsight Spark (Linux)](apache-spark-create-standalone-application.md). При выполнении этих шагов предполагается, что:

* Вы уже скопировали JAR-файл приложения в учетную запись хранения, связанную с кластером.
* На компьютере, где выполняются эти действия, установлен CuRL.

Выполните следующие действия:

1. Для начала убедитесь в том, что в кластере выполняется Livy Spark. Для этого получите список выполняемых пакетов. Если Livy используется для выполнения задания в первый раз, то выходные данные должны содержать ноль.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    Должен отобразиться результат, аналогичный приведенному ниже фрагменту кода.
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    Обратите внимание на то, что последняя строка выходных данных содержит значение **total:0**, означающее, что никакие пакеты не выполняются.

2. Теперь отправим пакетное задание. В приведенном ниже фрагменте кода для передачи имени JAR-файла и имени класса в качестве параметров используется входной файл (input.txt). Если данные действия выполняются на компьютере Windows, то рекомендуется использовать входной файл.
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
   
    Параметры в файле **input.txt** определяются следующим образом:
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    Должен отобразиться результат, аналогичный приведенному ниже фрагменту кода.
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    Обратите внимание на то, что последняя строка выходных данных содержит значение **state:starting**. Кроме того, она включает параметр **id:0** — **0** означает идентификатор пакета.

3. Теперь вы можете извлечь состояние конкретного пакета по идентификатору соответствующего пакета.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Должен отобразиться результат, аналогичный приведенному ниже фрагменту кода.
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    Теперь выходные данные содержат параметр **state:success**, означающий, что задание выполнено.

4. При необходимости пакет можно удалить.
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Должен отобразиться результат, аналогичный приведенному ниже фрагменту кода.
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    Последняя строка выходных данных показывает, что пакет удален. При удалении запущенного задания оно также завершается. Если задание удаляется после завершения (независимо от успешности выполнения), удаляются все данные об этом задании.

## <a name="updates-to-livy-configuration-starting-with-hdinsight-35-version"></a>Обновления конфигурации Livy, начиная с версии HDInsight 3.5

Кластер HDInsight версии 3.5 и выше по умолчанию запрещает использование локальных путей для доступа к примерам файлов данных или JAR-файлам. Мы рекомендуем использовать путь `wasb://` для доступа к JAR-файлам или примерам файлов данных из кластера. Если вы хотите использовать локальный путь, необходимо соответствующим образом обновить конфигурацию Ambari. Для этого выполните следующие действия:

1. Перейдите на портал Ambari для кластера. Веб-интерфейс Ambari доступен по адресу https://**CLUSTERNAME**.azurehdidnsight.net, где CLUSTERNAME — это имя вашего кластера HDInsight.

2. В левой области навигации щелкните **Livy**, а затем щелкните **Configs** (Конфигурации).

3. В разделе **livy-default** добавьте имя свойства `livy.file.local-dir-whitelist` и присвойте ему значение **"/"**, если вы хотите разрешить полный доступ к файловой системе. Если вы хотите разрешить доступ только к конкретному каталогу, то в качестве значения свойства укажите путь к этому каталогу.

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a>Отправка задания Livy для кластера в пределах виртуальной сети Azure

При подключении к кластеру HDInsight Spark из виртуальной сети Azure можно напрямую подключаться к Livy в кластере. В этом случае URL-адрес для конечной точки Livy — `http://<IP address of the headnode>:8998/batches`. Здесь **8998** — это порт, на котором выполняется Livy на головном узле кластера. Дополнительные сведения о доступе к службам через порты, не являющиеся общедоступными, см. в разделе [Порты, используемые службами Hadoop в HDInsight](../hdinsight-hadoop-port-settings-for-services.md).





## <a name="next-step"></a>Дальнейшие действия

* [Документация по Apache Livy REST API](https://livy.incubator.apache.org/docs/latest/rest-api.html)
* [Управление ресурсами кластера Apache Spark в Azure HDInsight](apache-spark-resource-manager.md)
* [Отслеживание и отладка заданий в кластере Apache Spark в HDInsight на платформе Linux](apache-spark-job-debugging.md)

