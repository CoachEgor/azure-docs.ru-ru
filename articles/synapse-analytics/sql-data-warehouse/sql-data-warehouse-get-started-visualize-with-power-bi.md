---
title: Визуализация данных с помощью Power BI Microsoft Azure
description: Визуализация данных хранилища данных SQL с помощью Power BI
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 7e764eeab6f681d90e1a602f02cdb03330d6fd3d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80350948"
---
# <a name="visualize-data-with-power-bi"></a>Визуализация данных с помощью Power BI
В этом руководстве показано, как использовать Power BI для подключения к хранилищу данных SQL и создания нескольких базовых визуализаций.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Предварительные условия
Для пошагового изучения этого учебника потребуется следующее:

* Хранилище данных SQL, в которое предварительно загружена база данных AdventureWorksDW. Для подготовки хранилища данных см. статью [Краткое руководство. Создание хранилища данных SQL Azure на портале Azure и отправка запросов к этому хранилищу данных](create-data-warehouse-portal.md) и выберите загрузку демонстрационных данных. Если хранилище данных уже существует, но в нем нет демонстрационных данных, вы можете [загрузить WideWorldImportersDW](load-data-wideworldimportersdw.md).

## <a name="1-connect-to-your-database"></a>1. подключение к базе данных
Чтобы открыть Power BI и подключиться к базе данных AdventureWorksDW, выполните следующие действия.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Щелкните **Базы данных SQL** и выберите базу данных хранилища данных SQL AdventureWorks.
   
    ![Поиск базы данных](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png)
3. Нажмите кнопку «Открыть в Power BI».
   
    ![Кнопка Power BI](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png)
4. Должна появиться страница подключения к хранилищу данных SQL с веб-адресом вашей базы данных. Нажмите кнопку «Далее».
   
    ![Power BI: подключение](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png)
5. Введите имя пользователя и пароль Azure SQL Server.
   
    ![Страница входа Power BI](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png)
6. Чтобы открыть базу данных, щелкните набор данных AdventureWorksDW на левой панели.
   
    ![Power BI: открытие AdventureWorksDW](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png)

## <a name="2-create-a-report"></a>2. Создание отчета
Теперь вы готовы к использованию Power BI для анализа демонстрационных данных AdventureWorksDW. Для выполнения анализа в AdventureWorksDW предусмотрено представление с именем AggregateSales. Это представление содержит несколько основных метрик для анализа продаж компании.

1. Чтобы создать карту суммы продаж по почтовому индексу, разверните представление AggregateSales, щелкнув его в области полей справа. Выберите столбцы PostalCode и SalesAmount.
   
    ![Power BI: выбор AggregateSales](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png)
   
    Power BI в автоматическом режиме распознает географические данные и наносит их на карту.
   
    ![Карта Power BI](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png)

2. На этом этапе создается линейчатая диаграмма, на которой отображается сумма продаж в зависимости от доходов клиентов. Чтобы создать гистограмму, перейдите к расширенному представлению AggregateSales. Щелкните поле SalesAmount. Перетащите поле «Доход клиентов» влево и поместите его на оси.
   
    ![Power BI: выбор оси](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png)
   
    Гистограмма слева.
   
    ![Гистограмма Power BI](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png)
3. На этом этапе создается график, на котором отображены продажи по дате заказа. Чтобы создать график, перейдите к расширенному представлению AggregateSales. Щелкните SalesAmount и OrderDate. В столбце «Визуализации» щелкните значок «График» (это первый значок во второй строке под заголовком «Визуализации»).
   
    ![Power BI: выбор графика](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png)
   
    Теперь у вас есть отчет, на котором отображены три разные визуализации.
   
    ![График Power BI](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png)

Ход выполнения можно сохранить в любой момент, выбрав в меню **Файл** пункт **Сохранить**.

## <a name="using-direct-connect"></a>Использование прямого соединения
Так же как при работе с Базой данных SQL Azure, функция прямого соединения хранилища данных SQL позволяет использовать аналитические возможности Power BI. Прямое соединение позволяет отправлять запросы в хранилище данных SQL Azure в режиме реального времени по мере исследования данных.  Благодаря этой функции в сочетании с масштабом хранилища данных SQL пользователи могут за считаные минуты создавать динамические отчеты на основе терабайтов данных. Кроме того, появление кнопки "Открыть в Power BI" дает возможность пользователям подключить Power BI напрямую к хранилищу данных SQL, не собирая сведений из других частей Azure.

При использовании прямого соединения:

* При подключении укажите полное имя сервера.
* Убедитесь, что правила брандмауэра для базы данных имеют настройку "Разрешить доступ к службам Azure".
* Каждое действие, например выбор столбца или добавление фильтра, отправляет запрос напрямую в хранилище данных.
* Плитки обновляются автоматически примерно через каждые 15 минут.
* Раздел вопросов и ответов недоступен для наборов данных прямого соединения.
* Изменения схемы применяются автоматически.
* Время ожидания любых запросов на прямое соединение истекает через две минуты.

Эти ограничения и примечания могут измениться по мере улучшения работы продукта.

## <a name="next-steps"></a>Дальнейшие шаги
Теперь, когда мы предоставили вам некоторое время, чтобы получить демонстрационные данные, см. статью как [разработать](sql-data-warehouse-overview-develop.md) или [загрузить](design-elt-data-loading.md). Кроме того, рекомендуем ознакомиться с [веб-сайтом Power BI](https://www.powerbi.com/).
