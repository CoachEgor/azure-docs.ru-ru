---
title: Устранение неполадок со Spark в Azure HDInsight
description: Получите ответы на распространенные вопросы о работе с Apache Spark и Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: a4dc7293c00097c7a5752e29bf7c9a203cbb31a5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64721154"
---
# <a name="troubleshoot-apache-spark-by-using-azure-hdinsight"></a>Устранение неполадок в Apache Spark с помощью Azure HDInsight

Ознакомьтесь с основными проблемами, которые возникают при работе с полезными данными [Apache Spark](https://spark.apache.org/) в [Apache Ambari](https://ambari.apache.org/), и способами их решения.

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-ambari-on-clusters"></a>Как настроить приложение Apache Spark с помощью Apache Ambari в кластерах?

### <a name="resolution-steps"></a>Способы устранения

Значения конфигурации Spark, можно оптимизировать, что помогает избежать исключение OutofMemoryError приложения Apache Spark. Ниже показано по умолчанию значения конфигурации Spark в Azure HDInsight: 

1. В списке кластеров выберите **Spark2**.

    ![Выбор кластера из списка](./media/apache-troubleshoot-spark/update-config-1.png)

2. Выберите вкладку **Конфигурации** .

    ![Выбор вкладки "Конфигурации"](./media/apache-troubleshoot-spark/update-config-2.png)

3. В списке конфигураций выберите **Custom-spark2-defaults**.

    ![Выбор конфигурации custom-spark-defaults](./media/apache-troubleshoot-spark/update-config-3.png)

4. Найдите параметр значения, который необходимо настроить, например **spark.executor.memory**. В этом случае значение **4608m** слишком высокое.

    ![Выбор поля spark.executor.memory](./media/apache-troubleshoot-spark/update-config-4.png)

5. Задайте для этого параметра рекомендуемое значение. Рекомендуется использовать значение **2048m**.

    ![Изменение значения на 2048m](./media/apache-troubleshoot-spark/update-config-5.png)

6. Сохраните это значение, а затем сохраните конфигурацию. На панели инструментов нажмите кнопку **Сохранить**.

    ![Сохранение параметра и конфигурации](./media/apache-troubleshoot-spark/update-config-6a.png)

    Если нужно будет пересмотреть какую-либо конфигурацию, вы получите оповещение. Проверьте элементы, а затем нажмите кнопку **Proceed Anyway** (Продолжить). 

    ![Нажатие кнопки "Proceed Anyway" (Продолжить)](./media/apache-troubleshoot-spark/update-config-6b.png)

    Запишите примечание об изменениях конфигурации, а затем нажмите кнопку **Сохранить**.

    ![Примечание об изменениях](./media/apache-troubleshoot-spark/update-config-6c.png)

7. При каждом сохранении конфигурации вам будет предложено перезапустить службу. Нажмите кнопку **Перезапустить**.

    ![Нажатие кнопки "Перезапустить"](./media/apache-troubleshoot-spark/update-config-7a.png)

    Подтвердите перезапуск.

    ![Нажатие кнопки "Confirm Restart All" (Подтвердить перезапуск всех)](./media/apache-troubleshoot-spark/update-config-7b.png)

    Вы можете просмотреть запущенные процессы.

    ![Просмотр запущенных процессов](./media/apache-troubleshoot-spark/update-config-7c.png)

8. Вы можете добавить конфигурации. В списке конфигураций выберите **Custom-spark2-defaults**, а затем щелкните **Добавить свойство**.

    ![Выбор "Добавить свойство"](./media/apache-troubleshoot-spark/update-config-8.png)

9. Определите новое свойство. Вы можете определить отдельное свойство с помощью диалогового окна для определенных параметров, например тип данных. Или вы можете определить несколько свойств с одним определением на строку. 

    В этом примере свойство **spark.driver.memory** определяется со значением **4 ГБ**.

    ![Определение нового свойства](./media/apache-troubleshoot-spark/update-config-9.png)

10. Сохраните конфигурацию и перезапустите службу, как описано на шагах 6 и 7.

Эти изменения применяются на уровне кластера, но их можно переопределить при отправке задания Spark.

### <a name="additional-reading"></a>Дополнительные материалы

[Отправка заданий Apache Spark в кластерах HDInsight](https://web.archive.org/web/20190112152841/ https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Как настроить приложение Apache Spark с помощью Jupyter Notebook в кластерах?

### <a name="resolution-steps"></a>Способы устранения

1. Чтобы определить, какие конфигурации Spark нужно настроить и с какими значениями, см. раздел "Что вызывает в приложении Apache Spark исключение OutOfMemoryError?"

2. Укажите конфигурации Spark в допустимом формате JSON в первой ячейке Jupyter Notebook после директивы **%%configure**. При необходимости измените фактические значения:

    ![Добавление конфигурации](./media/apache-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Дополнительные материалы

[Отправка заданий Apache Spark в кластерах HDInsight](https://web.archive.org/web/20190112152841/ https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-livy-on-clusters"></a>Как настроить приложение Apache Spark с помощью Apache Livy в кластерах?

### <a name="resolution-steps"></a>Способы устранения

1. Чтобы определить, какие конфигурации Spark нужно настроить и с какими значениями, см. раздел "Что вызывает в приложении Apache Spark исключение OutOfMemoryError?" 

2. Отправьте приложение Spark в Livy с помощью клиента REST, например cURL. Используйте команду, аналогичную приведенной ниже. При необходимости измените фактические значения:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Дополнительные материалы

[Отправка заданий Apache Spark в кластерах HDInsight](https://web.archive.org/web/20190112152841/ https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-spark-submit-on-clusters"></a>Как настроить приложение Apache Spark с помощью spark-submit в кластерах?

### <a name="resolution-steps"></a>Способы устранения

1. Чтобы определить, какие конфигурации Spark нужно настроить и с какими значениями, см. раздел "Что вызывает в приложении Apache Spark исключение OutOfMemoryError?"

2. Запустите оболочку Spark с помощью команды, аналогичной приведенной ниже. При необходимости измените фактические значения конфигураций: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Дополнительные материалы

[Отправка заданий Apache Spark в кластерах HDInsight](https://web.archive.org/web/20190112152841/ https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-an-apache-spark-application-outofmemoryerror-exception"></a>Что вызывает в приложении Apache Spark исключение OutOfMemoryError?

### <a name="detailed-description"></a>Подробное описание

Сбой приложения Spark со следующими типами неперехваченных исключений:

```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a>Возможные причины

Наиболее вероятной причиной этого исключения является нехватка памяти кучи, выделенной для виртуальных машин Java. Эти виртуальные машины Java запускаются как исполнители или драйверы в рамках приложения Spark. 

### <a name="resolution-steps"></a>Способы устранения

1. Определите максимальный объем данных, которые обрабатывает приложение Spark. Это значение можно предугадать, исходя из максимального размера входных данных, промежуточных данных, создаваемых при преобразовании входных данных, и выходных данных, получаемых при дальнейшем преобразовании промежуточных данных приложением. Этот процесс может быть итеративным, если не удается предугадать начальное значение. 

2. Убедитесь, что в кластере HDInsight, который будет использоваться, достаточно ресурсов, таких как память и количество ядер, для работы приложения Spark. Это можно определить, просмотрев в разделе Cluster Metrics (Метрики кластера) пользовательского интерфейса YARN такие значения, как **Memory Used** (Используемая память) и **Memory Total** (Всего памяти), а также **VCores Used** (Используемые ядра VCore) и **VCores Total** (Всего ядер VCore).

3. Задайте для параметров конфигурации Spark соответствующие значения, не превышающие 90 % доступной памяти и количества ядер. Эти значения должны быть в пределах требований к объему памяти для приложения Spark: 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    Чтобы рассчитать общий объем памяти, используемый всеми исполнителями: 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
   Чтобы рассчитать общий объем памяти, используемый драйвером:
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a>Дополнительные материалы

- [Обзор управления памятью в Apache Spark](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [Отладка приложения Apache Spark в кластере HDInsight](https://web.archive.org/web/20190112152909/ https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)


### <a name="see-also"></a>См. также
[Устранение неполадок с помощью Azure HDInsight](../../hdinsight/hdinsight-troubleshoot-guide.md)
