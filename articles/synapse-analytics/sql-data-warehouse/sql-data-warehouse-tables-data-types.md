---
title: Определение типов данных
description: Рекомендации по определению типов данных таблицы в хранилище данных SQL Azure.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 020b6fc08dad0dacb51215eca6f7baf127a9dba6
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351312"
---
# <a name="table-data-types-in-azure-sql-data-warehouse"></a>Типы данных таблицы в хранилище данных SQL Azure
Рекомендации по определению типов данных таблицы в хранилище данных SQL Azure. 

## <a name="what-are-the-data-types"></a>Какие типы данных используются?

Хранилище данных SQL поддерживает самые распространенные типы данных. Список поддерживаемых типов данных см. в [типах данных](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) в инструкции CREATE TABLE. 

## <a name="minimize-row-length"></a>Уменьшение длины строки
Уменьшение размера типов данных сокращает длину строки, что улучшает производительность запросов. Используйте наименьший тип данных для данных. 

- Не рекомендуется использовать по умолчанию длинные значения столбцов. Например, если самое длинное значение состоит из 25 знаков, столбец необходимо определить как VARCHAR(25). 
- Не нужно использовать [NVARCHAR][NVARCHAR], если вам требуется только VARCHAR.
- По возможности используйте NVARCHAR(4000) или VARCHAR(8000) вместо NVARCHAR(MAX) или VARCHAR(MAX).

Если для загрузки таблиц используются внешние таблицы PolyBase, определенная длина таблицы не должна превышать 1 МБ. Если строка с данными переменной длины превышает 1 МБ, можно загрузить строку с помощью BCP, а не PolyBase.

## <a name="identify-unsupported-data-types"></a>Определение неподдерживаемых типов данных
Если вы переносите базу данных из другой базы данных SQL, вы можете обнаружить некоторые типы данных, неподдерживаемые в хранилище данных SQL. Используйте этот запрос для определения неподдерживаемых типов данных в существующей схеме SQL.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>Обходные решения для неподдерживаемых типов данных

Ниже перечислены типы данных, неподдерживаемые хранилищем данных SQL, а также варианты, доступные для использования вместо неподдерживаемых типов данных.

| Неподдерживаемые типы данных | Обходной путь |
| --- | --- |
| [Геометрии](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [geography](/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [Hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)(4000) |
| [Изображения](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [text](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[Varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [Ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql) |
| [Sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql) |Разделите столбец на несколько строго типизированных столбцов. |
| [Таблице](/sql/t-sql/data-types/table-transact-sql) |Преобразуйте во временные таблицы. |
| [Timestamp](/sql/t-sql/data-types/date-and-time-types) |Для использования [datetime2](/sql/t-sql/data-types/datetime2-transact-sql) и функции [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql) внесите изменения в код. По умолчанию поддерживаются только константы, поэтому нельзя использовать current_timestamp как ограничение по умолчанию. Если нужно перенести значения версии строки из типизированного столбца timestamp, используйте [BINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) или [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) в качестве значений версии строки NOT NULL или NULL. |
| [xml](/sql/t-sql/xml/xml-transact-sql) |[Varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [определяемый пользователем тип](/sql/relational-databases/native-client/features/using-user-defined-types) |По возможности выполните преобразование в исходный тип данных. |
| Значения по умолчанию | Значения по умолчанию поддерживают только литералы и константы. |


## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о разработке таблиц см. в статье [Общие сведения о проектировании таблиц в хранилище данных SQL Azure](sql-data-warehouse-tables-overview.md).
