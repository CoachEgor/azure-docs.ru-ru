---
title: Применение пользовательских пакетов Maven с записной книжкой Jupyter в кластерах Spark в Azure HDInsight
description: Пошаговые инструкции по настройке записных книжек Jupyter, доступных с кластерами HDInsight Spark для использования пользовательских пакетов Maven.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: hrasheed
ms.openlocfilehash: 02b5b7a3673b3df3ba27e7814851e3519e473633
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448729"
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Использование внешних пакетов с записными книжками Jupyter в кластерах Apache Spark в HDInsight
> [!div class="op_single_selector"]
> * [Использование волшебных команд](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Использование действия сценария](apache-spark-python-package-installation.md)

Узнайте, как настроить [Jupyter Notebook](https://jupyter.org/) в кластере Apache Spark в HDInsight для использования внешних, предоставленных сообществом пакетов Apache **Maven**, которые не включены в готовую версию кластера. 

Полный список доступных пакетов можно найти в [репозитории Maven](https://search.maven.org/) . Его также можно получить из других источников. Например, полный список предоставленных сообществом пакетов можно найти в разделе [Пакеты Spark](https://spark-packages.org/).

В этой статье вы узнаете, как использовать пакет [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) с записной книжкой Jupyter.

## <a name="prerequisites"></a>Технические условия
Необходимо следующее:

* Кластер Apache Spark в HDInsight. Инструкции см. в статье [Начало работы. Создание кластера Apache Spark в HDInsight на платформе Linux и выполнение интерактивных запросов с помощью SQL Spark](apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Использование внешних пакетов с записными книжками Jupyter
1. На начальной панели [портала Azure](https://portal.azure.com/)щелкните элемент кластера Spark (если он закреплен на начальной панели). Кроме того, вы можете перейти к кластеру, последовательно щелкнув **Просмотреть все** > **Кластеры HDInsight**.   

1. В колонке кластера Spark щелкните **Быстрые ссылки**, затем в колонке **Панель мониторинга кластера** выберите **Записная книжка Jupyter**. При появлении запроса введите учетные данные администратора для кластера.

    > [!NOTE]  
    > Также можно открыть Jupyter Notebook для своего кластера, открыв следующий URL-адрес в браузере. Замените **CLUSTERNAME** именем кластера:
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

1. Создайте новую записную книжку. Щелкните **Создать**, а затем выберите **Spark**.
   
    ![Создание записной книжки Jupyter](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Создание записной книжки Jupyter")

1. Будет создана и открыта записная книжка с именем Untitled.pynb. Щелкните имя записной книжки в верхней части страницы сверху и введите понятное имя.
   
    ![Указание имени для записной книжки](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Указание имени для записной книжки")

1. Используйте волшебную команду `%%configure` , чтобы настроить записную книжку для использования внешнего пакета. В записных книжках, которые используют внешние пакеты, следует вызывать волшебную команду `%%configure` в первой ячейке кода. Она настраивает ядро для использования пакета до начала сеанса.

    >[!IMPORTANT]  
    >Если вы забыли настроить ядро в первой ячейке, можно использовать `%%configure` с параметром `-f`, но при этом сеанс будет перезапущен и все изменения будут утеряны.

    | Версия HDInsight | Команда |
    |-------------------|---------|
    |Для HDInsight 3.3 и HDInsight 3.4 | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | Для HDInsight 3.5 и HDInsight 3.6 | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

1. Приведенный выше фрагмент ожидает получить координаты Maven для внешнего пакета в центральном репозитории Maven. В этом фрагменте кода `com.databricks:spark-csv_2.10:1.4.0` — координата пакета **spark-csv** в Maven. Вот как сформировать координаты для пакета.
   
    a. Найдите пакет в репозитории Maven. В этой статье мы используем [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    2\. В репозитории найдите значения для параметров **GroupId**, **ArtifactId** и **Version**. Убедитесь, что полученные значения соответствуют кластеру. В этом случае мы используем Scala 2.10 и пакет Spark 1.4.0, но может потребоваться выбрать другие версии, соответствующие версиям Scala или Spark в вашем кластере. Выяснить версию Scala в кластере можно, выполнив `scala.util.Properties.versionString` для ядра Jupyter Spark или при отправке Spark. Выяснить версию Spark в кластере можно, выполнив `sc.version` для элементов Jupyter Notebook.
   
    ![Использование внешних пакетов с записными книжками Jupyter](./media/apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Использование внешних пакетов с записными книжками Jupyter")
   
    c. Объедините три значения, разделив их двоеточием ( **:** ).
   
        com.databricks:spark-csv_2.10:1.4.0

1. Запустите ячейку кода с помощью волшебной команды `%%configure` . После выполнения этой команды соответствующий сеанс Livy будет использовать указанный вами пакет. В последующих ячейках записной книжки теперь можно использовать этот пакет, как показано ниже.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    Для HDInsight 3.6 следует использовать фрагмент кода ниже.

        val df = spark.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

1. Затем можно запустить фрагменты кода, как показано ниже, чтобы просмотреть данные из таблицы данных, созданной на предыдущем шаге.
   
        df.show()
   
        df.select("Time").count()

## <a name="seealso"></a>Дополнительные материалы
* [Apache Spark в Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Сценарии
* [Руководство. Анализ данных Apache Spark с использованием Power BI в HDInsight](apache-spark-use-bi-tools.md)
* [Использование Apache Spark MLlib для Создание приложения машинного обучения Apache Spark в HDInsight](apache-spark-ipython-notebook-machine-learning.md)
* [Использование Apache Spark MLlib для создания приложения машинного обучения и анализа набора данных](apache-spark-machine-learning-mllib-ipython.md)
* [Анализ журналов веб-сайтов с помощью Apache Spark в HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Создание и запуск приложений
* [Создание автономного приложения с использованием Scala](apache-spark-create-standalone-application.md)
* [Удаленный запуск заданий с помощью Apache Livy в кластере Apache Spark](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Средства и расширения

* [Использование внешних пакетов Python с элементами Jupyter Notebook в кластерах Apache Spark в HDInsight на платформе Linux](apache-spark-python-package-installation.md)
* [Использование подключаемого модуля средств HDInsight для IntelliJ IDEA для создания и отправки приложений Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Удаленная отладка приложений Apache Spark с помощью подключаемого модуля средств HDInsight для IntelliJ IDEA](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Использование записных книжек Zeppelin с кластером Apache Spark в Azure HDInsight](apache-spark-zeppelin-notebook.md)
* [Ядра для записной книжки Jupyter в кластерах Apache Spark в Azure HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Установка записной книжки Jupyter на компьютере и ее подключение к кластеру Apache Spark в Azure HDInsight (предварительная версия)](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Управление ресурсами
* [Управление ресурсами кластера Apache Spark в Azure HDInsight](apache-spark-resource-manager.md)
* [Отслеживание и отладка заданий в кластере Apache Spark в HDInsight на платформе Linux](apache-spark-job-debugging.md)
