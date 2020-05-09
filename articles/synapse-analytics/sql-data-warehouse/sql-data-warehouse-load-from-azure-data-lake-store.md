---
title: Руководство по загрузке данных из Azure Data Lake Storage
description: Используйте внешние таблицы Polybase для загрузки данных из Azure Data Lake Storage синапсе SQL.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/08/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: f26aafc771998ea73d1a4f97f0e960a94f6775c3
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82626723"
---
# <a name="load-data-from-azure-data-lake-storage-for-sql-analytics"></a>Загрузка данных из Azure Data Lake Storage для SQL Analytics

В этом руководство описано, как использовать внешние таблицы Polybase для загрузки данных из Azure Data Lake Storage. Несмотря на то, что можно выполнять нерегламентированные запросы к данным, хранящимся в Data Lake Storage, рекомендуется импортировать данные для лучшей производительности.

> [!NOTE]  
> Альтернативой загрузке является [инструкция копирования](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) , которая сейчас находится в общедоступной предварительной версии.  Инструкция COPY обеспечивает наибольшую гибкость. Чтобы оставить отзыв о инструкции COPY, отправьте сообщение электронной почты по следующему списку рассылки: sqldwcopypreview@service.microsoft.com.
>
> [!div class="checklist"]
>
> * Создание объектов базы данных, необходимых для загрузки из Data Lake Storage.
> * Подключитесь к каталогу Data Lake Storage.
> * Загрузка данных в хранилище данных.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="before-you-begin"></a>Перед началом

Перед началом работы с этим руководством скачайте и установите последнюю версию [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (SSMS).

Для работы с этим руководством необходимы указанные ниже компоненты.

* Пул SQL. См. раздел [Создание пула SQL и запрос данных](create-data-warehouse-portal.md).
* Учетная запись Data Lake Storage. См. статью [Приступая к работе с Azure Data Lake Storage](../../data-lake-store/data-lake-store-get-started-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Для этой учетной записи хранения необходимо настроить или указать один из следующих учетных данных для загрузки: ключ учетной записи хранения, пользователь приложения Azure Directory или пользователь AAD с соответствующей ролью RBAC для учетной записи хранения.

## <a name="create-a-credential"></a>Создание учетных данных

Вы можете пропустить этот раздел и перейти к статье "Создание внешнего источника данных" при проверке подлинности с помощью команды AAD Pass. Учетные данные для базы данных не обязательно создавать или указывать при использовании транзитного агента AAD, но убедитесь, что у пользователя AAD есть соответствующая роль RBAC (читатель данных BLOB-объекта хранилища, участник или роль владельца) в учетной записи хранения. Дополнительные сведения см. [здесь](https://techcommunity.microsoft.com/t5/Azure-SQL-Data-Warehouse/How-to-use-PolyBase-by-authenticating-via-AAD-pass-through/ba-p/862260).

Чтобы получить доступ к учетной записи Data Lake Storage, необходимо создать главный ключ базы данных для шифрования секрета учетных данных. Затем создайте учетные данные для базы данных, чтобы сохранить секрет. При проверке подлинности с помощью субъектов-служб (пользователь приложения Azure Directory) учетные данные субъекта базы данных хранят учетные данные участника-службы, настроенные в AAD. Для хранения ключа учетной записи хранения для Gen2 можно также использовать учетные данные уровня базы данных.

Чтобы подключиться к Data Lake Storage с помощью субъектов-служб, необходимо **сначала** создать приложение Azure Active Directory, создать ключ доступа и предоставить приложению доступ к учетной записи Data Lake Storage. Инструкции см. [в разделе аутентификация в Azure Data Lake Storage с помощью Active Directory](../../data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

Войдите в пул SQL с помощью пользователя, имеющего разрешения на уровне элемента управления, и выполните следующие инструкции SQL для базы данных:

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest

CREATE MASTER KEY;


-- B (for service principal authentication): Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    -- Always use the OAuth 2.0 authorization endpoint (v1)
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- B (for Gen2 storage key authentication): Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;

-- It should look something like this when authenticating using service principal:
CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```

## <a name="create-the-external-data-source"></a>Создание внешнего источника данных

Используйте команду [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), чтобы сохранить расположение данных. Если выполняется проверка подлинности в AAD, параметр CREDENTIAL не требуется. При проверке подлинности с помощью управляемого удостоверения для конечных точек службы следуйте этой [документации](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#azure-synapse-analytics-polybase) , чтобы настроить внешний источник данных.

```sql
-- C (for Gen1): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen1 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<datalakestoregen1accountname>.azuredatalakestore.net',
    CREDENTIAL = ADLSCredential
);

-- C (for Gen2): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen2 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION='abfs[s]://<container>@<AzureDataLake account_name>.dfs.core.windows.net', -- Please note the abfss endpoint for when your account has secure transfer enabled
    CREDENTIAL = ADLSCredential
);
```

## <a name="configure-data-format"></a>Настройка формата данных

Чтобы импортировать данные из Data Lake Storage, необходимо указать формат внешнего файла. Этот объект определяет, как файлы записываются в Data Lake Storage.
Полный список форматов, доступных для команды [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), см. в документации по T-SQL.

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store missing values as default for datatype.

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-the-external-tables"></a>Создание внешних таблиц

Указав источник данных и формат файла, можно перейти к созданию внешних таблиц. Внешние таблицы являются методом взаимодействия с внешними данными. В параметре расположения можно указать файл или каталог. Если указан каталог, загружаются все содержащиеся в нем файлы.

```sql
-- D: Create an External Table
-- LOCATION: Folder under the Data Lake Storage root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStorage
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Рекомендации по внешним таблицам

Создание внешней таблицы не представляет сложностей, но следует учитывать некоторые особенности.

Внешние таблицы строго типизированы. Это означает, что каждая строка принимаемых данных должна соответствовать определению в схеме таблицы.
Строка, которая не соответствует определению схемы, будет отклоняться при загрузке.

С помощью параметров REJECT_TYPE и REJECT_VALUE вы можете определить, сколько строк или какой процент данных должны попасть в итоговую таблицу. Если в ходе загрузки будет достигнуто указанное ограничение, загрузка завершится ошибкой. Чаще всего строки отклоняются из-за несоответствия определению схемы. Например, если для столбца в схеме ошибочно указан формат int (целое число), а сами данные в файле имеют строковый формат, будут отклонены все строки.

Хранилище Data Lake Storage 1-го поколения использует управление доступом на основе ролей (RBAC) для управления доступом к данным. Это означает, что субъект-служба должна иметь разрешения на чтение для каталогов, указанных в параметре расположения, и для дочерних элементов конечного каталога и файлов. Это позволяет PolyBase выполнять аутентификацию и загрузку данных.

## <a name="load-the-data"></a>Загрузка данных

Чтобы загрузить данные из Data Lake Storage используйте инструкцию [CREATE TABLE как SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) .

Компонент CTAS создает новую таблицу и заполняет ее результатам инструкции Select. CTAS определяет новую таблицу так, чтобы в ней содержались те же столбцы и типы данных, которые были выведены инструкцией Select. Если вы выбираете все столбцы из внешней таблицы, новая таблица получает точную копию всех столбцов и типов данных этой внешней таблицы.

В нашем примере мы создаем распределенную хэш-таблицу с именем DimProduct на основе внешней таблицы DimProduct_external.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```

## <a name="optimize-columnstore-compression"></a>Оптимизация сжатия columnstore

По умолчанию таблицы определяются как кластеризованный индекс columnstore. После завершения загрузки для некоторых строк данных может не выполняться сжатие в индекс columnstore.  Это может происходить по ряду причин. Чтобы узнать больше, ознакомьтесь с [управлением индексами columnstore](sql-data-warehouse-tables-index.md).

Чтобы оптимизировать производительность запросов и сжатие columnstore после загрузки, перестройте таблицу, чтобы настроить принудительное сжатие всех строк таблиц индексом columnstore.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>Оптимизация статистики

Одностолбцовую статистику рекомендуется создавать сразу после загрузки. Существует несколько вариантов статистики. Например, при создании одностолбцовой статистики для каждого столбца перестройка всех статистических данных может занять длительное время. Если вам известно, что некоторые столбцы не будут входить в предикаты запросов, можно пропустить создание статистики для этих столбцов.

Если вам потребуется создать одностолбцовую статистику для каждого столбца в каждой таблице, можно использовать пример кода хранимой процедуры `prc_sqldw_create_stats` из статьи, посвященной [статистике](sql-data-warehouse-tables-statistics.md).

Следующий пример кода — хорошая отправная точка для создания статистики. Он создает одностолбцовую статистику для каждого столбца в таблице измерения и для каждого соответствующего столбца в таблицах фактов. Одно- или многостолбцовую статистику в другие столбцы таблицы фактов можно добавить позже.

## <a name="achievement-unlocked"></a>Победа!

Вы успешно загрузили данные в хранилище данных. Отличная работа!

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы создали внешние таблицы для определения структуры данных, хранящихся в Data Lake Storage 1-го поколения, а затем использовали инструкцию PolyBase CREATE TABLE AS SELECT для загрузки данных в хранилище данных.

Вы выполнили такие действия:
> [!div class="checklist"]
>
> * Созданы объекты базы данных, необходимые для загрузки из Data Lake Storage.
> * Подключено к каталогу Data Lake Storage.
> * Загруженные данные в хранилище данных.
>

Загрузка данных — это первый этап разработки решения хранилища данных с помощью Azure синапсе Analytics. Ознакомьтесь с нашими ресурсами разработки.

> [!div class="nextstepaction"]
> [Узнайте, как разрабатывать таблицы для хранения данных](sql-data-warehouse-tables-overview.md)
