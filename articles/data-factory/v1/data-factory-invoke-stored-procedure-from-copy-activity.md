---
title: Выверанка сохраненной процедуры из деятельности по копированию фабрики данных Azure
description: Узнайте, как вызвать хранимую процедуру в базе данных SQL Azure или SQL Server из действия копирования в фабрике данных Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d05c2b03a0c498144f37c9b6205053120a596b09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "74924095"
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Вызов хранимой процедуры из действия копирования в фабрике данных Azure
> [!NOTE]
> В этой статье рассматривается служба "Фабрика данных Azure" версии 1. Если вы используете текущую версию Фабрики данных, см. руководство по [преобразованию данных с помощью действия хранимой процедуры в службе "Фабрика данных"](../transform-data-using-stored-procedure.md).


При копировании данных в базу данных [SQL Server](data-factory-sqlserver-connector.md) или [SQL Azure](data-factory-azure-sql-connector.md) можно настроить класс **SqlSink** в действии копирования для вызова хранимой процедуры. Хранимые процедуры можно использовать для дополнительной обработки (объединения столбцов, поиска значений, вставки в несколько таблиц и т. д.), которая может потребоваться перед вставкой данных в целевую таблицу. В этой функции используются [параметры с табличным значением](https://msdn.microsoft.com/library/bb675163.aspx). 

В следующем примере показано, как вызвать хранимую процедуру в базе данных SQL Server из конвейера фабрики данных (действие копирования):  

## <a name="output-dataset-json"></a>Определение JSON выходного набора данных
В определении JSON выходного набора данных задайте для свойства **type** значение **SqlServerTable**. При использовании с базой данных SQL Azure задайте для него значение **AzureSqlTable**. Значение свойства **tableName** должно соответствовать имени первого параметра хранимой процедуры.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>Раздел SqlSink в определении JSON действия копирования
Определите раздел **SqlSink** в JSON действия копирования следующим образом. Чтобы вызвать хранимую процедуру при вставке данных в целевую базу данных (приемник), необходимо указать значения для свойств **SqlWriterStoredProcedureName** и **SqlWriterTableType**. Описания этих свойств см. в разделе [SqlSink](data-factory-sqlserver-connector.md#sqlsink) статьи, посвященной соединителю SQL Server.

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Определение хранимой процедуры 
В своей базе данных определите хранимую процедуру с тем же именем, что и **SqlWriterStoredProcedureName**. Хранимая процедура обрабатывает входные данные из исходного хранилища данных и вставляет данные в таблицу в целевой базе данных. Имя первого параметра хранимой процедуры должно совпадать со значением свойства tableName, заданным в определении JSON (Marketing) набора данных.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Определение типа таблицы
В своей базе данных определите тип таблицы с тем же именем, что и **SqlWriterTableType**. Схема типа таблицы должна соответствовать схеме входного набора данных.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Дальнейшие действия
См. следующие статьи о соединителях, где есть полные примеры JSON: 

- [База данных SQL Azure](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
