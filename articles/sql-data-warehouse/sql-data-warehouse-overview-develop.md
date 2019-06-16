---
title: Ресурсы для разработки хранилища данных в Azure | Документация Майкрософт
description: Основные понятия разработки, проектные решения, рекомендации и методики программирования для хранилища данных SQL.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 613bcb05dab993989a2ae00b71fef95794953ab8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65850730"
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Проектные решения и методики программирования для хранилища данных SQL
Ознакомьтесь со статьями о разработке, чтобы лучше понять основные проектные решения, рекомендации и методики программирования для хранилища данных SQL.

## <a name="key-design-decisions"></a>Основные проектные решения
Следующие статьи содержат основные понятия и проектные решения для разработки хранилища распределенных данных, использующего хранилище данных SQL:

* [подключения][connections];
* [параллелизм][concurrency];
* [транзакции][transactions];
* [пользовательские схемы][user-defined schemas];
* [распределение таблиц][table distribution];
* [индексы таблицы][table indexes];
* [секции таблицы][table partitions];
* [CTAS][CTAS];
* [статистика][statistics].

## <a name="development-recommendations-and-coding-techniques"></a>Рекомендации по разработке и методики программирования
В следующих статьях описаны методики программирования, советы и рекомендации по разработке для хранилища данных SQL:

* [хранимые процедуры][stored procedures];
* [метки][labels];
* [представления][views];
* [временные таблицы][temporary tables];
* [динамический код SQL][dynamic SQL];
* [циклы][looping];
* [группировка по параметрам][group by options];
* [присвоение значения переменной][variable assignment].

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные справочные сведения см. в статье [Темы, связанные с языком Transact-SQL](sql-data-warehouse-reference-tsql-statements.md).

<!--Image references-->

<!--Article references-->
[concurrency]: ./resource-classes-for-workload-management.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md


<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
