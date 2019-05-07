---
title: Копирование данных в Управляемый экземпляр Базы данных SQL Azure и из него с помощью Фабрики данных Azure | Документация Майкрософт
description: Сведения о перемещении данных в Управляемый экземпляр Базы данных SQL Azure и из него с помощью Фабрики данных Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: jingwang
ms.openlocfilehash: 3f29db5786c682188b4eadec12275df46ae3b547
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153340"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Копирование данных в Управляемый экземпляр Базы данных SQL Azure и из него с помощью Фабрики данных Azure

В этой статье описывается, как с помощью действия копирования в Фабрике данных Azure скопировать данные в Управляемый экземпляр Базы данных SQL Azure и из него. Это продолжение [обзорной статьи о действии копирования](copy-activity-overview.md), в которой представлены общие сведения об этом действии.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Данные из Управляемого экземпляра Базы данных SQL Azure можно скопировать в любое хранилище данных, поддерживаемое в качестве приемника. И наоборот, данные из любого хранилища данных, поддерживаемого в качестве источника, можно скопировать в управляемый экземпляр. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

Этот соединитель Управляемого экземпляра Базы данных SQL Azure поддерживает:

- копирование данных с проверкой подлинности SQL или Windows;
- (в качестве источника) извлечение данных с использованием SQL-запроса или хранимой процедуры;
- (в качестве приемника) применение пользовательской логики для добавления данных в целевую таблицу или вызова хранимой процедуры во время копирования.

[Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) в SQL Server в настоящее время не поддерживается. 

## <a name="prerequisites"></a>Технические условия

Чтобы скопировать данные из Управляемого экземпляра Базы данных SQL Azure, который расположен в виртуальной сети, настройте локальную среду выполнения интеграции с доступом к нужной базе данных. Дополнительные сведения см. в статье [Создание и настройка локальной среды выполнения интеграции](create-self-hosted-integration-runtime.md).

Если вы подготавливаете локальную среду выполнения интеграции в той же виртуальной сети, что и сам управляемый экземпляр, подключите компьютер среды выполнения интеграции к другой подсети относительно управляемого экземпляра. Если локальная среда выполнения интеграции подготавливается в другой виртуальной сети, используйте пиринг между виртуальными сетями или подключение "виртуальная сеть — виртуальная сеть". Дополнительные сведения см. в статье [Подключение приложения к Управляемому экземпляру Базы данных SQL](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="get-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей Фабрики данных, характерных для соединителя Управляемого экземпляра Базы данных SQL Azure.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы Управляемого экземпляра Базы данных SQL поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type необходимо задать значение **SqlServer**. | Да. |
| connectionString |Это свойство определяет сведения о строке подключения, необходимые для подключения к Управляемому экземпляру с помощью проверки подлинности SQL или Windows. Дополнительные сведения представлены в примерах ниже. <br/>Пометьте это поле как SecureString, чтобы безопасно хранить его в Фабрике данных. Вы также можете поместить пароль в Azure Key Vault, и если это аутентификация SQL, извлеките конфигурацию `password` из строки подключения. Ознакомьтесь с примером JSON под таблицей и с подробными сведениями в статье [Хранение учетных данных в Azure Key Vault](store-credentials-in-key-vault.md). |Да. |
| userName |Это свойство задает имя пользователя для проверки подлинности Windows. Например, **domainname\\username**. |№ |
| password |Это свойство позволяет указать пароль для учетной записи пользователя, которую вы указали в параметре имени пользователя. Выберите **SecureString**, чтобы обеспечить защиту при хранении сведений о строке подключения в Фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). |№ |
| connectVia | Это [среда выполнения интеграции](concepts-integration-runtime.md) для подключения к хранилищу данных. Укажите здесь локальную среду выполнения интеграции из той же виртуальной сети, где находится управляемый экземпляр. |Да. |

>[!TIP]
>Может появиться код ошибки UserErrorFailedToConnectToSqlServer с сообщением вида "The session limit for the database is XXX and has been reached" (Достигнут лимит сеансов XXX, установленный для базы данных). Если возникает такая ошибка, добавьте `Pooling=false` в строку подключения и повторите попытку.

**Пример 1. Использование проверки подлинности SQL**.

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример 2. Использование аутентификации SQL с помощью пароля в Azure Key Vault**.

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример 3. Использование проверки подлинности Windows**.

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;"
            },
            "userName": "<domain\\username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о наборах данных. Этот раздел содержит список свойств, поддерживаемых набором данных Управляемого экземпляра Базы данных SQL.

Чтобы копировать данные из базы данных управляемого экземпляра SQL Azure, поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type для набора данных должно иметь значение **SqlServerTable**. | Да. |
| tableName |Это свойство содержит имя таблицы или представления в экземпляре базы данных, на которое ссылается связанная служба. | "Нет" для источника. "Да" для приемника. |

**Пример**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, доступных для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). Этот раздел содержит список свойств, поддерживаемых источником и приемником Управляемого экземпляра Базы данных SQL.

### <a name="azure-sql-database-managed-instance-as-a-source"></a>Управляемый экземпляр Базы данных SQL Azure в качестве источника

Чтобы скопировать данные из Управляемого экземпляра Базы данных SQL Azure, задайте тип источника **SqlSource** в действии копирования. В разделе source для действия копирования поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type для источника действия копирования должно иметь значение **SqlSource**. | Да. |
| sqlReaderQuery |Это свойство применяет пользовательский SQL-запрос для чтения данных. Например, `select * from MyTable`. |№ |
| sqlReaderStoredProcedureName |Это свойство содержит имя хранимой процедуры, которая считывает данные из исходной таблицы. Последней инструкцией SQL должна быть инструкция SELECT в хранимой процедуре. |№ |
| storedProcedureParameters |Это параметры для хранимой процедуры.<br/>Допустимые значения: пары имен или значений. Имена и регистр параметров должны совпадать с именами и регистром параметров хранимой процедуры. |№ |

Обратите внимание на следующие моменты.

- Если для параметра **SqlSource** указано значение **sqlReaderQuery**, то действие копирования направляет запрос для получения данных к управляемому экземпляру, указанному в качестве источника. Есть и другой вариант: создать хранимую процедуру, указав ее имя в **sqlReaderStoredProcedureName** и параметры в **storedProcedureParameters**, если она принимает параметры.
- Если свойства **sqlReaderQuery** или **sqlReaderStoredProcedureName** не указаны, то для построения запроса используются столбцы, определенные в разделе structure шаблона JSON для набора данных. Запрос `select column1, column2 from mytable` выполняется для управляемого экземпляра. Если в определении набора данных нет раздела structure, выбираются все столбцы из таблицы.

**Пример. Использование SQL-запроса**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Пример. Использование хранимой процедуры**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Определение хранимой процедуры**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-managed-instance-as-a-sink"></a>Управляемый экземпляр Базы данных SQL Azure в качестве приемника

Чтобы скопировать данные в Управляемый экземпляр Базы данных SQL Azure, задайте тип приемника **SqlSink** в действии копирования. В разделе sink для действия копирования поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type для приемника действия копирования должно иметь значение **SqlSink**. | Да. |
| writeBatchSize |Количество строк для вставки в таблицу SQL **в пакете**.<br/>Допустимые значения: целое число (количество строк). По умолчанию фабрика данных динамически определяет размер соответствующего пакета, в зависимости от размера строки.  |Нет  |
| writeBatchTimeout |Это свойство определяет время ожидания при выполнении операции пакетной вставки, по истечении которого она считается не выполненной.<br/>Допустимые значения: любой промежуток времени. Например, 00:30:00 определяет период 30 минут. |№ |
| preCopyScript |Это свойство определяет для действия копирования SQL-запрос, который выполняется перед записью данных в базу данных управляемого экземпляра. Он вызывается однократно при каждом запуске копирования. Это свойство можно использовать для очистки предварительно загруженных данных. |№ |
| sqlWriterStoredProcedureName |Это имя хранимой процедуры, которая определяет правила помещения исходных данных в целевую таблицу. Например, в таких процедурах можно определить собственную бизнес-логику на основе операций upsert или преобразований. <br/><br/>Эта хранимая процедура будет *вызываться для каждого пакета*. Чтобы создать однократно выполняемую операцию, в которой не используются исходные данные, например для удаления или усечения данных, примените свойство `preCopyScript`. |№ |
| storedProcedureParameters |Эти параметры используются для хранимой процедуры.<br/>Допустимые значения: пары имен или значений. Имена и регистр параметров должны совпадать с именами и регистром параметров хранимой процедуры. |№ |
| sqlWriterTableType |Это свойство определяет имя типа таблицы для использования в хранимой процедуре. Действие копирования предоставляет доступ к перемещаемым данным во временной таблице с указанным здесь типом. Это позволяет при выполнении хранимой процедуры объединить копируемые и существующие данные. |№ |

> [!TIP]
> Когда вы копируете данные в Управляемый экземпляр Базы данных SQL, действие копирования по умолчанию добавляет данные в таблицу приемника. Для выполнения команды upsert или дополнительной бизнес-логики используйте хранимую процедуру в SqlSink. Дополнительные сведения см. в разделе [Вызов хранимой процедуры из приемника SQL](#invoke-a-stored-procedure-from-a-sql-sink).

**Пример 1. Добавление данных**.

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Пример 2. Вызов хранимой процедуры для выполнения операции upsert во время копирования**.

Дополнительные сведения см. в разделе [Вызов хранимой процедуры из приемника SQL](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>Столбцы идентификаторов в целевой базе данных

В следующем примере копируются данные из исходной таблицы без столбца идентификаторов в целевую таблицу со столбцом идентификаторов.

**Исходная таблица**

```sql
create table dbo.SourceTbl
(
    name varchar(100),
    age int
)
```

**Целевая таблица**

```sql
create table dbo.TargetTbl
(
    identifier int identity(1,1),
    name varchar(100),
    age int
)
```

Обратите внимание, что в целевой таблице есть столбец идентификаторов.

**Определение JSON исходного набора данных**

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

**Определение JSON целевого набора данных**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

Обратите внимание, что исходная и целевая таблицы имеют разные схемы. В целевой таблице есть столбец идентификаторов. В этом случае нужно указать в определении целевого набора данных свойство structure без столбца идентификаторов.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Вызов хранимой процедуры из приемника SQL

Для копирования данных в Управляемый экземпляр Базы данных SQL вы можете настроить хранимую процедуру, которая будет вызываться с указанными вами параметрами.

Вы можете использовать хранимую процедуру, когда встроенные механизмы копирования не подходят. Она обычно используется, когда перед окончательной вставкой исходных данных в целевую таблицу необходимо выполнить вставку с обновлением (операция upsert) или дополнительную обработку. Дополнительная обработка может включать такие задачи, как объединение столбцов, поиск дополнительных значений и вставка в несколько таблиц.

В следующем примере показано, как использовать хранимую процедуру для выполнения операции Upsert в таблице базы данных SQL Server. Предположим, что и входные данные, и таблица **Marketing** приемника состоят из трех столбцов: **ProfileID**, **State** и **Category**. Выполните операцию Upsert на основе данных столбца **ProfileID** только для определенной категории.

**Выходной набор данных:** «tableName» должно быть то же имя параметра типа таблицы в хранимой процедуре (см. ниже скрипт хранимой процедуры).

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Определение **приемника SQL** разделе в действии копирования следующим образом.

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

В своей базе данных определите хранимую процедуру с тем же именем, что и **SqlWriterStoredProcedureName**. Она обрабатывает входные данные из указанного источника и выполняет их слияние в выходную таблицу. Имя параметра типа таблицы в хранимой процедуре должно совпадать с именем **tableName**, заданным в наборе данных.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category);
END
```

В своей базе данных определите тип таблицы с тем же именем, что и SqlWriterTableType. Для типа таблицы укажите ту же схему, которая возвращается для входных данных.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

В этой хранимой процедуре используются [параметры с табличным значением](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Сопоставление типов данных для Управляемого экземпляра Базы данных SQL

При копировании данных в Управляемый экземпляр Базы данных SQL или из него используются следующие сопоставления между типами данных Управляемого экземпляра Базы данных SQL Azure и промежуточными типами данных Фабрики данных Azure. Дополнительные сведения о том, как действие копирования сопоставляет схему и типы данных источника и приемника, см. в статье [Сопоставление схем в действии копирования](copy-activity-schema-and-type-mapping.md).

| Тип данных Управляемого экземпляра Базы данных SQL Azure | Промежуточный тип данных Фабрики данных Azure |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |Datetime |
| Datetime |Datetime |
| datetime2 |Datetime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |Datetime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
|  timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| Xml |xml |

>[!NOTE]
> В настоящее время для типов данных, которые сопоставляются с промежуточными типом Decimal, Фабрика данных Azure поддерживает точность до 28. Если для ваших данных требуется точность больше 28, попробуйте преобразовать их в строковые данные в SQL-запросе.

## <a name="next-steps"></a>Дальнейшие действия
Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в Фабрике данных Azure см. в [этой таблице](copy-activity-overview.md##supported-data-stores-and-formats).
