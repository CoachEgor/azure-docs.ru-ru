---
title: Начало работы с межбазовыми запросами (вертикальное секционирование)
description: Узнайте, как использовать запрос к эластичной базе данных в вертикально секционированных базах данных.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/25/2019
ms.openlocfilehash: b6d9ba0c57850c61626d7b6d99fcb1fffc2d3c25
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73690404"
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Приступая к работе с межбазовыми запросами (вертикальное секционирование) (предварительная версия)

Запрос (предварительная версия) к эластичной базе данных для Базы данных Azure SQL позволяет выполнять запросы T-SQL, охватывающие несколько баз данных, с помощью одной точки подключения. Эта статья относится к [вертикально секционированным базам данных](sql-database-elastic-query-vertical-partitioning.md).  

Будут представлены способы настройки и использования базы данных SQL Azure для выполнения запросов, охватывающих несколько связанных баз данных.

Дополнительные сведения о функции запросов к эластичной базе данных см. в [этой статье](sql-database-elastic-query-overview.md).

## <a name="prerequisites"></a>Предварительные требования

Требуется разрешение ALTER ANY EXTERNAL DATA SOURCE. Это разрешение включено в разрешение ALTER DATABASE. Для обращения к базовому источнику данных необходимы разрешения ALTER ANY EXTERNAL DATA SOURCE.

## <a name="create-the-sample-databases"></a>Создание образца базы данных

Для начала создайте две базы данных, **Клиенты** и **Заказы**, на одном или на разных серверах Базы данных SQL.

Отправьте в базу данных **Заказы** указанные ниже запросы, чтобы создать таблицу **OrderInformation** (Сведения о заказах) и заполнить ее образцами данных:

    CREATE TABLE [dbo].[OrderInformation](
        [OrderID] [int] NOT NULL,
        [CustomerID] [int] NOT NULL
        )
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)

Отправьте в базу данных **Клиенты** указанный ниже запрос, чтобы создать таблицу сведений о клиентах **CustomerInformation** (Сведения о клиентах) и заполнить ее примерами данных.

    CREATE TABLE [dbo].[CustomerInformation](
        [CustomerID] [int] NOT NULL,
        [CustomerName] [varchar](50) NULL,
        [Company] [varchar](50) NULL
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC)
    )
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC')
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ')
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO')

## <a name="create-database-objects"></a>Создание объектов базы данных

### <a name="database-scoped-master-key-and-credentials"></a>Главный ключ и учетные данные для конкретной базы данных

1. Откройте SQL Server Management Studio или SQL Server Data Tools в Visual Studio.
2. Подключитесь к базе данных «Заказы» и выполните следующие команды T-SQL:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master_key_password>';
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';  

    Вместо параметров username и password укажите имя пользователя и пароль, используемые для входа в базу данных "Клиенты".
    Проверка подлинности с помощью Azure Active Directory и эластичных запросов сейчас не поддерживается.

### <a name="external-data-sources"></a>Внешние источники данных

Для создания внешнего источника данных выполните следующую команду в базе данных «Заказы».

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
        (TYPE = RDBMS,
        LOCATION = '<server_name>.database.windows.net',
        DATABASE_NAME = 'Customers',
        CREDENTIAL = ElasticDBQueryCred,
    ) ;

### <a name="external-tables"></a>Внешние таблицы

Создайте в базе данных «Заказы» внешнюю таблицу, соответствующую определению таблицы CustomerInformation:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
    ( [CustomerID] [int] NOT NULL,
      [CustomerName] [varchar](50) NOT NULL,
      [Company] [varchar](50) NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc)

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Выполнение запроса T-SQL к примеру эластичной базы данных

Определив источник внешних данных и внешние таблицы, вы сможете использовать T-SQL для запроса внешних таблиц. Выполните следующий запрос в базе данных «Заказы»:

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company
    FROM OrderInformation
    INNER JOIN CustomerInformation
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID

## <a name="cost"></a>Стоимость

В настоящее время функция запроса к эластичной базе данных включена в стоимость Базы данных SQL Azure.  

Сведения о ценах см. на [странице с ценами на Базу данных SQL](https://azure.microsoft.com/pricing/details/sql-database).

## <a name="next-steps"></a>Дальнейшие действия

* Общие сведения об эластичных запросах см. в разделе [Обзор эластичных запросов к базе данных SQL Azure (предварительная версия)](sql-database-elastic-query-overview.md).
* Описание синтаксиса и примеры запросов вертикально секционированных данных см. в разделе [Запрос к нескольким облачным базам данных с разными схемами (предварительная версия)](sql-database-elastic-query-vertical-partitioning.md).
* Руководство по горизонтальному секционированию (сегментированию) см. в статье [Отчеты по масштабируемым облачным базам данных (предварительная версия)](sql-database-elastic-query-getting-started.md).
* Описание синтаксиса и примеры запросов горизонтально секционированных данных см. в разделе [Отчеты по масштабируемым облачным базам данных (предварительная версия)](sql-database-elastic-query-horizontal-partitioning.md).
* В описании [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) приведена хранимая процедура, которая выполняет инструкцию Transact-SQL для отдельной удаленной базы данных SQL Azure или набора баз данных, выступающих в качестве сегментов в схеме горизонтального секционирования.
