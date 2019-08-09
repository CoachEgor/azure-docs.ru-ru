---
title: 'Хранилище данных SQL Azure: заметки о выпуске за июнь 2018 г. | Документация Майкрософт'
description: Заметки о выпуске для хранилища данных SQL Azure.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: ''
ms.date: 07/23/2018
ms.author: anjangsh
ms.reviewer: jrasnick
ms.openlocfilehash: 4348a634fd5b2b33f36d8e79f28caf659b82ccf4
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "67626155"
---
# <a name="whats-new-in-azure-sql-data-warehouse-june-2018"></a>Что нового в Хранилище данных SQL Azure? Июнь 2018 г.
Хранилище данных SQL Azure постоянно совершенствуется. В этой статье описаны новые возможности и изменения, вступившие в силу с июня 2018 г. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="user-defined-restore-points"></a>Определяемые пользователем точки восстановления
Хранилище данных SQL автоматически создает моментальные снимки хранилища данных каждые 8 часов, чтобы гарантировать 8-часовое значение целевой точки восстановления. Автоматизированное создание моментальных снимков упрощает ваши задачи по управлению хранилищем данных, но иногда нужно создавать дополнительные моментальные снимки в критические моменты в зависимости от потребностей вашего бизнеса. Например, моментальные снимки следует создавать перед загрузкой значительного объема данных или развертыванием новых скриптов в хранилище данных, чтобы получить точку восстановления непосредственно перед выполнением такой операции. 

Хранилище данных SQL теперь поддерживает [определенные пользователем точки восстановления](https://azure.microsoft.com/blog/quick-recovery-time-with-sql-data-warehouse-using-user-defined-restore-points/) с помощью командлета [New-азсклдатабасересторепоинт](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaserestorepoint) .

```powershell
New-AzSqlDatabaseRestorePoint
    -ResourceGroupName $ResourceGroupName
    -ServerName $ServerName
    -DatabaseName $DatabaseName
    -RestorePointLabel $RestorePointName
```

## <a name="column-level-security"></a>Безопасность на уровне столбцов
Управление доступом к данным и безопасностью в хранилище данных крайне важны для создания доверительных отношений с клиентами и партнерами. Хранилище данных SQL [теперь поддерживает безопасность на уровне столбцов (CLS)](https://azure.microsoft.com/blog/column-level-security-is-now-supported-in-azure-sql-data-warehouse/), которая позволяет настраивать разрешения на просмотр конфиденциальных данных, ограничивая доступ пользователей к определенным столбцам в таблицах, но при этом не изменяя структуру всего хранилища данных.

Функция CLS позволяет управлять доступом к столбцам таблиц в зависимости от контекста выполнения пользователя или членства в группах, используя стандартную инструкцию T-SQL [GRANT](https://docs.microsoft.com/azure/sql-data-warehouse/column-level-security). Логика ограничения доступа находится на уровне самой базы данных, а не на другом уровне приложения отдельно от данных. Это упрощает реализацию системы безопасности в целом.


```sql
CREATE USER Nurse WITHOUT LOGIN;   
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO Nurse;   
EXECUTE AS USER = 'Nurse';
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12 
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="object_schema_name"></a>OBJECT_SCHEMA_NAME
Функция [OBJECT_SCHEMA_NAME()](https://docs.microsoft.com/sql/t-sql/functions/object-schema-name-transact-sql) возвращает имя схемы базы данных для объектов в схеме. Эта функция стала стандартной в средствах извлечения, преобразования и загрузки при проверке схемы объектов. 

```sql
SELECT
    OBJECT_SCHEMA_NAME([object_id]) [table_schema]
    , [name]                        [table_name]
FROM
    [sys].[tables];
```

**Пример результатов**
```
table_schema    | table_name
-----------------------------
dbo               customer
dbo               lineitem
dbo               nation
dbo               orders
```

## <a name="support-for-the-systime_zone_info-view"></a>Поддержка представления sys.time_zone_info
Представление [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) возвращает сведения о поддерживаемых часовых поясах в Хранилище данных SQL Azure.

```sql
SELECT * FROM [sys].[time_zone_info];
```

**Пример результатов**
```
name                            | current_utc_offset | is_currently_dst
-------------------------------------------------------------------------
Pacific Standard Time (Mexico)    -07:00               1
US Mountain Standard Time         -07:00               0
Mountain Standard Time (Mexico)   -06:00               1
Central Standard Time             -05:00               1
```

## <a name="auto-stats-operations-appear-in-sysdm_pdw_exec_requests-behavior-change"></a>Отображение операций автоматического создания статистики в sys.dm_pdw_exec_requests (изменение в поведении)

С появлением [автоматического создания статистики](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics#automatic-creation-of-statistic) Хранилище данных SQL Azure будет создавать статистику для оптимизации выполнения запросов. В выпуске за июнь 2018 г. добавлена возможность отслеживать автоматическое создание статистики. Теперь в представление [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) добавляется новая запись при каждом выполнении операции [CREATE STATISTICS](https://docs.microsoft.com/sql/t-sql/statements/create-statistics-transact-sql).

```sql
SELECT
    [start_time]
    , [end_time]
    , [command]
FROM
    [sys].[dm_pdw_exec_requests]
WHERE
    [command] LIKE 'CREATE STATISTICS _WA_Sys%';
```
**Пример результатов**
```
start_time                | end_time                | command
------------------------------------------------------------------------------------------------------------------------------
2018-06-06 19:06:26.173    2018-06-06 19:06:26.173    CREATE STATISTICS _WA_Sys_00000001_63D998CC ON dbo.LineItem(l_orderkey);
```

## <a name="next-steps"></a>Следующие шаги
Теперь, когда вы уже знакомы с хранилищем данных SQL, Узнайте, как быстро [создать хранилище данных SQL][create a SQL Data Warehouse]. Если вы раньше не работали с Azure, используйте [глоссарий Azure Глоссарий][Azure glossary] , чтобы узнать значение новых терминов. Или ознакомьтесь со следующими ресурсами, посвященными хранилищу данных SQL.  

* [Истории успеха клиентов]
* [Блоги]
* [Запросы функций]
* [Видеоролики]
* [Блоги группы консультирования клиентов]
* [Форум Stack Overflow]
* [Twitter]


[Блоги]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Блоги группы консультирования клиентов]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Истории успеха клиентов]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Запросы функций]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Форум Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Видеоролики]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
