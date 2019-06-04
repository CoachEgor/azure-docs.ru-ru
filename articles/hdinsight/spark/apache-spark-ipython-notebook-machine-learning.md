---
title: Руководство по Создание приложения машинного обучения Spark в Azure HDInsight
description: Пошаговые инструкции по созданию приложения машинного обучения Apache Spark в кластерах HDInsight Spark с помощью записной книжки Jupyter.
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/24/2019
ms.author: hrasheed
ms.openlocfilehash: ed6a8f83d2ef31513aeadbc6741dd77c30c30070
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2019
ms.locfileid: "66252883"
---
# <a name="tutorial-build-an-apache-spark-machine-learning-application-in-hdinsight"></a>Руководство по Создание приложения машинного обучения Apache Spark в HDInsight 

Из этого руководства вы узнаете, как с помощью [Jupyter Notebook](https://jupyter.org/) создать приложение машинного обучения [Apache Spark](https://spark.apache.org/) для Azure HDInsight. 

[MLlib](https://spark.apache.org/docs/latest/ml-guide.html) — масштабируемая библиотека машинного обучения Spark, состоящая из общих алгоритмов и служебных программ обучения, включая классификацию, регрессию, кластеризацию, совместную фильтрацию, сокращение размерности, а также базовые элементы оптимизации.

Из этого руководства вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> * разработка приложения машинного обучения Apache Spark.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

* Кластер Apache Spark в HDInsight. Ознакомьтесь со статьей [Краткое руководство. Создание кластера Apache Spark в HDInsight с помощью шаблона](./apache-spark-jupyter-spark-sql-use-portal.md).

* Опыт работы с записными книжками Jupyter с Spark в HDInsight. Дополнительные сведения см. в статье [Руководство. Загрузка данных и выполнение запросов в кластере Apache Spark в Azure HDInsight](./apache-spark-load-data-run-query.md).

## <a name="understand-the-data-set"></a>Общие сведения о наборе данных

Приложение использует данные из примера файла HVAC.csv, который по умолчанию доступен на всех кластерах. Файл находится в папке `\HdiSamples\HdiSamples\SensorSampleData\hvac`. Это данные о целевой температуре и фактической температуре некоторых зданий, в которых установлена система кондиционирования воздуха. В столбце **System** указан идентификатор системы, а в столбце **SystemAge** — срок эксплуатации системы кондиционирования в годах. Используя эти сведения, вы можете спрогнозировать, будет ли температура здания выше или ниже относительно целевой температуры на основе идентификатора системы и срока ее эксплуатации.

![Моментальный снимок данных, используемых для примера машинного обучения Spark](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Моментальный снимок данных, используемых для примера машинного обучения Spark")

## <a name="develop-a-spark-machine-learning-application-using-spark-mllib"></a>Разработка приложения машинного обучения Spark с помощью Spark MLlib

В этом приложении для классификации документов используется [конвейер машинного обучения](https://spark.apache.org/docs/2.2.0/ml-pipeline.html) Spark. Конвейеры машинного обучения предоставляют набор API-интерфейсов более высокого уровня, созданных на основе кадров данных, позволяющих создавать и настраивать действующие конвейеры машинного обучения. В конвейере вы разобьете документ на слова, преобразуете слова в вектор числового признака и, наконец, создадите модель прогнозирования с помощью вектора и меток признаков. Выполните следующие действия, чтобы создать приложение.

1. Создайте записную книжку Jupyter, используя ядро PySpark. Инструкции см. в разделе по [созданию записной книжки Jupyter](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).
2. Импортируйте типы, необходимые для этого сценария. Вставьте следующий фрагмент кода в пустую ячейку и нажмите клавиши **SHIFT + ВВОД**. 

    ```PySpark
    from pyspark.ml import Pipeline
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml.feature import HashingTF, Tokenizer
    from pyspark.sql import Row

    import os
    import sys
    from pyspark.sql.types import *

    from pyspark.mllib.classification import LogisticRegressionWithSGD
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array
    ```

3. Загрузите данные (hvac.csv), проанализируйте их и используйте эти данные для обучения модели. 

    ```PySpark
    # Define a type called LabelDocument
    LabeledDocument = Row("BuildingID", "SystemInfo", "label")

    # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
    def parseDocument(line):
        values = [str(x) for x in line.split(',')]
        if (values[3] > values[2]):
            hot = 1.0
        else:
            hot = 0.0        

        textValue = str(values[4]) + " " + str(values[5])

        return LabeledDocument((values[6]), textValue, hot)

    # Load the raw HVAC.csv file, parse it using the function
    data = sc.textFile("/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
    training = documents.toDF()
    ```

    В фрагменте кода определите функцию, которая сравнивает фактическую температуру с целевой. Если фактическая температура больше, то здание горячее (значение **1.0**). В противном случае в здании холодно (значение **0,0**). 

4. Настройте конвейер машинного обучения Spark, который состоит из трех частей: логического анализатора, hashingTF и lr. 

    ```PySpark
    tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    ```

    См. дополнительные сведения о [конвейере машинного обучения Apache Spark и том, как он работает](https://spark.apache.org/docs/latest/ml-pipeline.html).

5. Впишите конвейер в документ для обучения.
   
    ```PySpark
    model = pipeline.fit(training)
    ```

6. Проверьте, чтобы в документе для обучения имелась контрольная точка хода выполнения для приложения.
   
    ```PySpark
    training.show()
    ```
   
    Выходные данные должны быть следующего вида.

    ```
    +----------+----------+-----+
    |BuildingID|SystemInfo|label|
    +----------+----------+-----+
    |         4|     13 20|  0.0|
    |        17|      3 20|  0.0|
    |        18|     17 20|  1.0|
    |        15|      2 23|  0.0|
    |         3|      16 9|  1.0|
    |         4|     13 28|  0.0|
    |         2|     12 24|  0.0|
    |        16|     20 26|  1.0|
    |         9|      16 9|  1.0|
    |        12|       6 5|  0.0|
    |        15|     10 17|  1.0|
    |         7|      2 11|  0.0|
    |        15|      14 2|  1.0|
    |         6|       3 2|  0.0|
    |        20|     19 22|  0.0|
    |         8|     19 11|  0.0|
    |         6|      15 7|  0.0|
    |        13|      12 5|  0.0|
    |         4|      8 22|  0.0|
    |         7|      17 5|  0.0|
    +----------+----------+-----+
    ```

    Сравнение выходных данных со сведениями в необработанном CSV-файле. Например, в первой строке CSV-файла содержатся следующие данные:

    ![Моментальный снимок выходных данных для примера машинного обучения Spark](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Моментальный снимок выходных данных для примера машинного обучения Spark")

    Обратите внимание на то, насколько фактическая температура меньше целевой, что свидетельствует о том, что здание холодное. В выходных данных обучения видно, что значение в первой строке столбца **label** составляет **0,0**. Это означает, что в здании не тепло.

7. Подготовьте набор данных, в отношении которого необходимо выполнить обученную модель. Для этого передайте идентификатор системы и данные о сроке эксплуатации (значения в столбце **SystemInfo** в выходных данных обучения). После этого модель предскажет, станет ли в здании с таким идентификатором системы и сроком эксплуатации теплее (значение 1,0) или холоднее (значение 0,0).
   
    ```PySpark   
    # SystemInfo here is a combination of system ID followed by system age
    Document = Row("id", "SystemInfo")
    test = sc.parallelize([(1L, "20 25"),
                    (2L, "4 15"),
                    (3L, "16 9"),
                    (4L, "9 22"),
                    (5L, "17 10"),
                    (6L, "7 22")]) \
        .map(lambda x: Document(*x)).toDF() 
    ```
8. Наконец, создайте прогнозы на основе тестовых данных. 
   
    ```PySpark
    # Make predictions on test documents and print columns of interest
    prediction = model.transform(test)
    selected = prediction.select("SystemInfo", "prediction", "probability")
    for row in selected.collect():
        print row
    ```

    Выходные данные должны быть следующего вида.

    ```   
    Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
    Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
    Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
    Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
    Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
    Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
    ```
   
   В первой строке прогноза видно, что при использовании системы кондиционирования с идентификатором 20 и возрастом 25 лет температура в здании высокая (**прогноз = 1.0**). Первое значение параметра DenseVector (0,49999) соответствует прогнозу 0,0, а второе значение (0,5001) — прогнозу 1,0. В выходных данных видно, что, даже если второе значение лишь незначительно выше, модель показывает **прогноз = 1.0**.
10. Завершите работу записной книжки для освобождения ресурсов. Для этого в меню **File** (Файл) записной книжки выберите пункт **Close and Halt** (Закрыть и остановить). Записная книжка завершит работу и закроется.

## <a name="use-anaconda-scikit-learn-library-for-spark-machine-learning"></a>Использование библиотеки scikit-learn Anaconda для машинного обучения Spark
Кластеры Apache Spark в HDInsight включают библиотеки Anaconda. Они также включают библиотеку **scikit-learn** для машинного обучения. Кроме того, библиотека включает различные наборы данных, которые можно использовать для создания примеров приложений прямо в записной книжке Jupyter. Примеры использования библиотеки scikit-learn см. здесь: [https://scikit-learn.org/stable/auto_examples/index.html](https://scikit-learn.org/stable/auto_examples/index.html).

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как с помощью Jupyter Notebook создать приложение машинного обучения Apache Spark для Azure HDInsight. Перейдите к следующему руководству, чтобы научиться использовать IntelliJ IDEA для заданий Spark. 

> [!div class="nextstepaction"]
> [Создание приложения Scala Maven с помощью IntelliJ](./apache-spark-create-standalone-application.md)