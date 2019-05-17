---
title: Подключение Excel к Apache Hadoop с помощью Power Query — Azure HDInsight
description: Узнайте, как пользоваться преимуществами компонентов бизнес-аналитики и применять Power Query для Excel для получения доступа к данным, хранящимся в Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.openlocfilehash: e35e86bee4dfee21d0a98867c3e0d1d7850ed0ec
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/16/2019
ms.locfileid: "65762010"
---
# <a name="connect-excel-to-apache-hadoop-by-using-power-query"></a>Подключение Excel к Apache Hadoop с помощью Power Query
Одной из ключевых особенностей решения Майкрософт для работы с большими данными является интеграция компонентов бизнес-аналитики Майкрософт с кластерами Apache Hadoop в службе Azure HDInsight. Важнейшим примером является возможность подключения Excel к учетной записи хранения Azure, в которой хранятся данные, связанные с кластером Hadoop, с помощью надстройки Microsoft Power Query для Excel. В этой статье приводится пошаговое руководство по настройке и использованию Power Query для запроса данных, связанных с кластером Hadoop, который управляется с помощью HDInsight.

### <a name="prerequisites"></a>Технические условия
Перед началом работы с этой статьей необходимо иметь следующее:

* **Кластер HDInsight**. Чтобы настроить кластер, ознакомьтесь с [руководством по Azure HDInsight][hdinsight-get-started].
* **Рабочая станция** под управлением Windows 7, Windows Server 2008 R2 или последующих версий операционной системы.
* **Office 2016, Office 2013 профессиональный плюс, Office 365 профессиональный плюс, Excel 2013 автономный или Office профессиональный плюс 2010**.

## <a name="install-power-query"></a>Установка Power Query
Power Query может импортировать данные, которые были выведены или созданы заданием Hadoop, выполняющимся в кластере HDInsight.

В Excel 2016 надстройка Power Query находится на вкладке "Данные" ленты в группе "Скачать & преобразовать". В предыдущих версиях Excel необходимо скачать надстройку Microsoft Power Query для Excel из [Центра загрузки Майкрософт][powerquery-download] и установить ее.

## <a name="import-hdinsight-data-into-excel"></a>Импорт данных HDInsight в Excel
Надстройка Power Query для Excel удобна для импорта данных из кластера HDInsight в Excel, где можно использовать средства бизнес-аналитики, такие как PowerPivot и Power Map, для изучения, анализа и представления данных.

**Импорт данных из кластера HDInsight**

1. Откройте Excel.
2. Создайте новую пустую книгу.
3. Выполните указанные ниже действия для вашей версии Excel.

   - Excel 2016

     - Откройте на ленте вкладку **Данные**, в разделе **Скачать & преобразовать** нажмите кнопку **Получение внешних данных**, выберите пункт **Из Azure**, а затем — **Из Azure HDInsight (HDFS)**.

       ![HDI.PowerQuery.SelectHdiSource](./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

   - Excel 2013 или 2010

     - Откройте меню **Power Query**, щелкните **Из Azure**, а затем — **Из Microsoft Azure HDInsight**.
   
       ![HDI.PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
       **Примечание.** Если вы не видите **Power Query** меню **файл** > **параметры** > **Add-ins**и выберите  **COM-надстройки** из раскрывающегося списка **управление** поле в нижней части страницы. Нажмите кнопку **Перейти...** и убедитесь, что установлен флажок «Power Query для Excel».
       
       **Примечание.** Power Query также позволяет импортировать данные из HDFS, щелкнув **Из других источников**.
4. В поле **Имя учетной записи** введите имя учетной записи хранения больших двоичных объектов Azure, связанной с вашим кластером, и нажмите кнопку **ОК**. Эта учетная запись может быть учетная запись хранения по умолчанию или связанная учетная запись хранения.  Формат имеет следующий вид: *https://&lt;имя_учетной_записи_хранения>.blob.core.windows.net/*.
5. В поле **Ключ учетной записи** введите ключ для учетной записи хранения больших двоичных объектов Azure, а затем нажмите кнопку **Сохранить**. (Вводить данные учетной записи требуется только при первом доступе к этому магазину.)
6. В области **Навигатор** слева от окна редактора запросов дважды щелкните имя контейнера хранилища больших двоичных объектов. По умолчанию имя контейнера совпадает с именем кластера.
7. Найдите **HiveSampleData.txt** в столбце **Имя** (путь папки: **../hive/warehouse/hivesampletable/**), а затем щелкните **Двоичный код** в левой части HiveSampleData.txt. HiveSampleData.txt поставляется вместе с кластером. При необходимости можно использовать собственный файл.
   
    ![HDI.PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. Если необходимо, можно переименовать имена столбцов. Когда будете готовы, щелкните **Закрыть и загрузить**.  Данные загружены в книгу.
   
    ![HDI.PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Дальнейшие действия
В этой статье было показано, как использовать Power Query для извлечения данных из HDInsight в Excel. Аналогичным образом можно извлекать данные из HDInsight в SQL Azure. Можно также передавать данные в HDInsight. Для получения дополнительных сведений ознакомьтесь со следующими статьями:

* [Визуализация данных Apache Hive с помощью Microsoft Power BI в Azure HDInsight](apache-hadoop-connect-hive-power-bi.md)
* [Visualize Interactive Query Hive data with Microsoft Power BI using DirectQuery in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) (Визуализация данных Hive из кластера Interactive Query с помощью Microsoft Power BI и DirectQuery в Azure HDInsight).
* [Выполнение запросов Apache Hive в Azure HDInsight с помощью Apache Zeppelin](../interactive-query/hdinsight-connect-hive-zeppelin.md)
* [Подключение Excel к Hadoop в Azure HDInsight с помощью Microsoft Hive ODBC Driver](apache-hadoop-connect-excel-hive-odbc-driver.md)
* [Подключение к Azure HDInsight и выполнение запросов Apache Hive с помощью Средств Data Lake для Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)
* [Использование средств Azure HDInsight для Visual Studio Code](../hdinsight-for-vscode.md).
* [Отправка данных в HDInsight](./../hdinsight-upload-data.md)

[image-hdi-powerquery-hdi-source]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: https://go.microsoft.com/fwlink/?LinkID=286689
