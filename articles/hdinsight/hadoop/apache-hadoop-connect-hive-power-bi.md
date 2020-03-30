---
title: Визуализация данных Apache Hive с помощью Power BI в Azure HDInsight
description: Сведения о визуализации данных Hive, обрабатываемых Azure HDInsight, с помощью Microsoft Power BI.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/02/2020
ms.openlocfilehash: d9b64785dbd82842479eb3f313b8394f9f25b40b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370004"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Визуализация данных Apache Hive с Microsoft Power BI с использованием ODBC в Azure HDInsight

Узнайте, как подключить Microsoft Power BI Desktop к Azure HDInsight с помощью ODBC и визуализировать данные Apache Hive.

> [!IMPORTANT]
> Можно использовать драйвер Hive ODBC для импорта с помощью универсального соединителя ODBC в Power BI Desktop. Но этот драйвер не рекомендуется для рабочих нагрузок бизнес-аналитик, с учетом того что ядро запросов Hive не является интерактивным. Для лучшей производительности используйте [соединитель интерактивных запросов HDInsight ](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) и [соединитель HDInsight Spark](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect).

В этой статье вы загружаете данные со таблицы `hivesampletable` Hive в Power BI. Эта таблица содержит некоторые данные об использовании мобильного телефона. Затем вы отобразите эти данные на карте мира:

![Отчет карты HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

Эти сведения также относятся к новому типу кластера [интерактивных запросов](../interactive-query/apache-interactive-query-get-started.md). Сведения о подключении к HDInsight Interactive Query при помощи прямого запроса см. в статье [Visualize Interactive Query Hive data with Microsoft Power BI using direct query in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) (Визуализация данных Hive Interactive Query при помощи Microsoft Power BI с использованием прямого запроса в Azure HDInsight).

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить действия, указанные в этой статье, вам потребуется:

* Кластер HDInsight. Это может быть кластер HDInsight с Hive или новый кластер интерактивных запросов. Сведения о создании кластеров см. в [этом разделе](apache-hadoop-linux-tutorial-get-started.md).

* [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/). Копию этой программы можно скачать в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Создание источника данных Hive ODBC

Дополнительные сведения см. в разделе [Создание источника данных Hive ODBC](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Загрузка данных из HDInsight

Таблица **hivesampletable** Hive поставляется со всеми кластерами HDInsight.

1. Запустите Power BI Desktop.

1. Из верхнего меню, перейдите на **дом** > **Получить данные** > **Подробнее ...**.

    ![HDInsight Excel Power BI открытые данные](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)

1. Из диалога **Get Data** выберите **другие** слева, выберите **ODBC** справа, а затем выберите **Connect** в нижней части.

1. Из диалога **From ODBC** выберите имя источника данных, созданное в последнем разделе, из списка выпадающих данных. Затем выберите **OK**.

1. Для первого использования откроется диалог **драйверов ODBC.** Выберите **по умолчанию или пользовательские** из левого меню. Затем выберите **Подключите** для открытия **Navigator.**

1. Из диалога **Navigator** расширьте **ODBC > HIVE > по умолчанию,** выберите **hivesampletable,** а затем выберите **Load.**

## <a name="visualize-data"></a>Визуализируйте данные

Продолжите из последней процедуры.

1. Из панели визуализаций выберите **Карту,** это значок глобуса.

    ![Настройка отчета HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)

1. Из панели **Полей** выберите **страну** и **устройство**устройства. На карте отобразятся данные.

1. Разверните карту.

## <a name="next-steps"></a>Дальнейшие действия

Из этой статьи вы узнали, как визуализировать данные HDInsight с помощью Power BI.  Дополнительные сведения см. в следующих статьях:

* [Используйте Apache Цеппелин для запуска запросов Apache Hive в Azure HDInsight.](../interactive-query/hdinsight-connect-hive-zeppelin.md)
* [Подключение Excel к Hadoop в Azure HDInsight с помощью Microsoft Hive ODBC Driver](./apache-hadoop-connect-excel-hive-odbc-driver.md)
* [Подключите Excel к Apache Hadoop с помощью power Query](apache-hadoop-connect-excel-power-query.md).
* [Подключение к Azure HDInsight и выполнение запросов Apache Hive с помощью Средств Data Lake для Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)
* [Используйте инструмент Azure HDInsight для визуального кода studio.](../hdinsight-for-vscode.md)
* [Загрузка данных в HDInsight](./../hdinsight-upload-data.md).
