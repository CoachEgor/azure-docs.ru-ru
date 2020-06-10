---
title: Руководство по подготовке данных для обучения прогнозной модели на языке R
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: В первой части этого учебника из трех частей вы подготовите данные из базы данных SQL Azure для обучения прогнозной модели на языке R с помощью Служб машинного обучения в Базе данных SQL Azure (предварительная версия).
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 07/26/2019
ROBOTS: NOINDEX
ms.openlocfilehash: a82467a097c50314e8f26f4a5cc4507f867ad504
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2020
ms.locfileid: "84024794"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Руководство по подготовке данных для обучения прогнозной модели на языке R с помощью Служб машинного обучения в Базе данных SQL Azure (предварительная версия)
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

В первой части этого руководства, состоящего из трех частей, объясняется, как импортировать и подготовить данные из базы данных SQL Azure с помощью R. Далее в этой серии эти данные будут использоваться для обучения и развертывания прогнозной модели машинного обучения на языке R с помощью Служб машинного обучения в Базе данных SQL Azure (предварительная версия).

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

Для этой серии учебников представим, что вы владелец бизнеса по прокату лыж, и вам нужно спрогнозировать количество прокатов в будущем. Эта информация поможет вам подготовить инвентарь, персонал и пункты проката.

В первой и второй части из этой серии описывается, как создать некоторые скрипты R в RStudio для подготовки данных и обучения модели машинного обучения. Из третьей части вы узнаете, как выполнить эти скрипты R в базе данных SQL с помощью хранимых процедур.

В этой статье вы узнаете, как выполнять следующие задачи.

> [!div class="checklist"]
>
> * Импорт примера базы данных в Базу данных SQL Azure с помощью R.
> * Загрузка данных из Базы данных SQL Azure в кадр данных на языке R.
> * Подготовка данных на языке R путем определения некоторых столбцов как категориальных.

Из [второй части](predictive-model-build-compare-tutorial.md) вы узнаете, как создать и обучить несколько моделей машинного обучения на языке R, а затем выбрать наиболее точную из них.

Из [третьей части](predictive-model-deploy-tutorial.md) вы узнаете, как сохранить модель в базе данных, а затем создать хранимые процедуры с помощью скриптов R, разработанных в первых двух частях. Хранимые процедуры будут выполняться в базе данных SQL для составления прогнозов на основе новых данных.

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас еще нет подписки Azure, [создайте учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.

* [База данных SQL Azure с включенными Службами машинного обучения](machine-learning-services-overview.md) (с использованием R).

* Пакет RevoScaleR. Сведения о вариантах локальной установки этого пакета см. в разделе [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms).

* IDE R. В этом учебнике используется [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* Инструмент SQL-запроса. В этом учебнике предполагается, что вы используете [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) или [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/).

## <a name="import-the-sample-database"></a>Импорт образца базы данных

Пример набора данных, используемый в этом учебнике, сохранен в **BACPAC-файле** резервной копии базы данных, доступный для скачивания и использования.

1. Скачайте файл [TutorialDB.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bacpac).

1. Следуйте указаниям в статье [Quickstart: Import a BACPAC file to a database in Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-import) (Краткое руководство. Импорт BACPAC-файла в базу данных в Базе данных SQL Azure), используя следующие сведения:

   * Импортируйте из скачанного файла **TutorialDB.bacpac**.
   * В общедоступной предварительной версии выберите конфигурацию **виртуального ядра 5-го поколения** для новой базы данных.
   * Присвойте новой базе данных имя TutorialDB.

## <a name="load-the-data-into-a-data-frame"></a>Загрузка данных в кадр данных

Чтобы использовать данные в R, загрузите их из базы данных SQL Azure в кадр данных (`rentaldata`).

Создайте файл RScript в RStudio и выполните следующий сценарий. Замените **Server**, **UID** и **PWD** собственными данными для подключения.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Logical SQL server>",
               "; Database=TutorialDB",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");

#Get the data from the table
SQL_rentaldata <- RxSqlServerData(table = "dbo.rental_data", connectionString = connStr, returnDataFrame = TRUE);

#Import the data into a data frame
rentaldata <- rxImport(SQL_rentaldata);

#Take a look at the structure of the data and the top rows
head(rentaldata);
str(rentaldata);
```

Результат должен иметь следующий вид.

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>Подготовка данных

В этом образце базы данных большая часть подготовки уже выполнена, но можно сделать еще кое-что.
Используйте следующий сценарий R для определения трех столбцов как *категорий*, изменив типы данных на *factor*.

```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);

#Visualize the dataset after the change
str(rentaldata);
```

Результат должен иметь следующий вид.

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

Теперь данные готовы для обучения.

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы не собираетесь продолжать работу с этим руководством, удалите базу данных TutorialDB со своего сервера.

На портале Azure сделайте следующее:

1. В меню слева на портале Azure выберите **Все ресурсы** или **Базы данных SQL**.
1. В поле **Фильтровать по имени...** введите **TutorialDB** и выберите свою подписку.
1. Выберите базу данных TutorialDB.
1. На странице **Обзор** выберите **Удалить**.

## <a name="next-steps"></a>Дальнейшие действия

В первой части этого учебника вы выполнили следующие действия:

* Импорт примера базы данных в Базу данных SQL Azure с помощью R.
* Загрузка данных из Базы данных SQL Azure в кадр данных на языке R.
* Подготовка данных на языке R путем определения некоторых столбцов как категориальных.

Чтобы создать модель машинного обучения, использующую данные из базы данных TutorialDB, воспользуйтесь инструкциями из второй части этой серии учебников.

> [!div class="nextstepaction"]
> [Руководство по созданию прогнозной модели на языке R с помощью Служб машинного обучения в Базе данных SQL Azure (предварительная версия)](predictive-model-build-compare-tutorial.md)
