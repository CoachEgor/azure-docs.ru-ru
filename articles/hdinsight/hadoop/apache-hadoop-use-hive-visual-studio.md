---
title: Apache Hive со средствами Data Lake (Apache Hadoop) для Visual Studio — Azure HDInsight
description: Узнайте, как использовать Средства Data Lake для Visual Studio с целью выполнения запросов Apache Hive с Apache Hadoop в Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 3a2e81234702e1fcff0349a14a4bc2852d257ad6
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62095478"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Выполнение запросов Apache Hive с использованием средств Data Lake для Visual Studio

Узнайте, как использовать Средства Data Lake для Visual Studio с целью выполнения запросов Apache Hive. Средства Data Lake позволяют легко создавать, отправлять и отслеживать запросы Hive к Apache Hadoop в Azure HDInsight.

## <a id="prereq"></a>Предварительные требования

* Кластер Azure HDInsight (Apache Hadoop в HDInsight)

  > [!IMPORTANT]  
  > Linux — это единственная операционная система, используемая для работы с HDInsight 3.4 или более поздних версий. Дополнительные сведения см. в разделе [Приближается дата прекращения сопровождения HDI версии 3.3](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Одна из следующих версий Visual Studio:

    * Visual Studio 2013 Community, Professional, Premium или Ultimate с обновлением 4

    * Visual Studio 2015 (любой выпуск);

    * Visual Studio 2017 (любой выпуск).

* Средства HDInsight для Visual Studio или инструменты Azure Data Lake Tools для Visual Studio. Пошаговые указания по установке и настройке инструментов Visual Studio Hadoop см. в статье [Приступая к работе с инструментами Azure Data Lake (в HDInsight) для Visual Studio для выполнения запроса Hive](apache-hadoop-visual-studio-tools-get-started.md).

## <a id="run"></a> Выполнение запросов Apache Hive с помощью Visual Studio

1. Откройте **Visual Studio** и выберите **Создать** > **Проект** > **Azure Data Lake** > **HIVE** > **Hive Application** (Приложение Hive). Введите имя этого проекта.

2. Откройте файл **Script.hql** , созданный в этом проекте, и вставьте следующие операторы HiveQL:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Эти операторы выполняют следующие действия:

   * `DROP TABLE`: если таблица существует, эта инструкция удаляет ее.

   * `CREATE EXTERNAL TABLE`: создает "внешнюю" таблицу в Hive. Внешние таблицы хранят только определение самой таблицы в Hive, в то время как данные остаются в исходном расположении.

     > [!NOTE]  
     > Внешние таблицы следует использовать, если исходные данные должны обновляться с использованием внешних источников. Например, с помощью задания MapReduce или службы Azure.
     >
     > Удаление внешней таблицы **не** приводит к удалению данных, будет удалено только определение таблицы.

   * `ROW FORMAT`: инструкции по форматированию данных для Hive. В данном случае поля всех журналов разделены пробелом.

   * `STORED AS TEXTFILE LOCATION`: указывает Hive, что данные хранятся в каталоге example/data и их формат — "Текст".

   * `SELECT`: подсчитывает количество строк, в которых столбец `t4` содержит значение `[ERROR]`. Эта инструкция должна вернуть значение `3`, так как данное значение содержат три строки.

   * `INPUT__FILE__NAME LIKE '%.log'`: указывает Hive, что вернуть нужно только данные из файлов с расширением LOG. Это предложение ограничивает поиск до файла sample.log, который содержит данные.

3. На панели инструментов выберите **Кластер HDInsight**, который вы хотите использовать для этого запроса. Щелкните **Отправить**, чтобы выполнить инструкции как задание Hive.

   ![Панель отправки](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

4. Отобразится **сводка по заданию Hive** и информация о его выполнении. Воспользуйтесь ссылкой **Обновить**, чтобы обновить информацию о задании. Обновляйте ее до тех пор, пока **состояние задания** не изменится на **Завершено**.

   ![Сводка по завершенному заданию](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

5. Воспользуйтесь ссылкой **Выходные данные задания** , чтобы просмотреть выходные данные этого задания. Они содержат `[ERROR] 3`. Это значение, возвращенное данным запросом.

6. Запросы Hive можно также отправлять без создания проекта. С помощью **обозревателя сервера** разверните узлы **Azure** > **HDInsight**, щелкните правой кнопкой мыши сервер HDInsight, а затем выберите **Написать запрос Hive**.

7. Отобразится документ **temp.hql** , в который нужно добавить следующие операторы HiveQL:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Эти операторы выполняют следующие действия:

   * `CREATE TABLE IF NOT EXISTS`: создает таблицу, если ее нет. Так как не используется ключевое слово `EXTERNAL`, эта инструкция создает внутреннюю таблицу. Внутренние таблицы хранятся в хранилище данных Hive и управляются Hive.

     > [!NOTE]  
     > В отличие от таблиц `EXTERNAL`, удаление внутренней таблицы приводит к удалению ее базовых данных.

   * `STORED AS ORC`: позволяет сохранить данные в формате ORC. Это высокооптимизированный и эффективный формат для хранения данных Hive.

   * `INSERT OVERWRITE ... SELECT`: выбирает строки из таблицы `log4jLogs`, которые содержат `[ERROR]`, а затем вставляет эти данные в таблицу `errorLogs`.

8. На панели инструментов щелкните **Отправить** , чтобы выполнить задание. Определить, было ли задание выполнено успешно, можно по значению в поле **Состояние задания** .

9. Убедитесь в том, что задание создало таблицу. Для этого в **обозревателе сервера** разверните **Azure** > **HDInsight** > ваш кластер HDInsight > **Базы данных Hive** > **По умолчанию**. Должны быть отображены таблицы **errorLogs** и **log4jLogs**.

## <a id="nextsteps"></a>Дальнейшие действия

Как вы видите, средства HDInsight для Visual Studio упрощают работу с запросами Hive в HDInsight.

Общая информация о Hive в HDInsight:

* [Использование Apache Hive с Apache Hadoop в HDInsight](hdinsight-use-hive.md)

Дополнительная информация о других способах работы с Hadoop в HDInsight:

* [Использование Apache Pig с Apache Hadoop в HDInsight](hdinsight-use-pig.md)

* [Использование MapReduce в Apache Hadoop в HDInsight](hdinsight-use-mapreduce.md)

Дополнительная информация об инструментах HDInsight для Visual Studio:

* [Приступая к работе со средствами HDInsight для Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
