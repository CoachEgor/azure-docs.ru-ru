---
title: Перенос данных в учетную запись API таблиц в Azure Cosmos DB
description: Сведения о том, как перенести либо импортировать данные из локальной или облачной среды в учетную запись API таблиц Azure в Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 12/07/2017
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 3d1efc0a116a38686fa929a2058fa88e4c2cfa82
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2020
ms.locfileid: "85119483"
---
# <a name="migrate-your-data-to-azure-cosmos-db-table-api-account"></a>Перенос данных в учетную запись API таблиц в Azure Cosmos DB

Это руководство содержит инструкции по импорту данных для использования с помощью [API таблицы](table-introduction.md) Azure Cosmos DB. Данные, хранящиеся в хранилище таблиц Azure, можно импортировать в API таблиц в Azure Cosmos DB, используя средства переноса данных или средство AzCopy. Данные, хранящиеся в учетной записи API таблицы Azure Cosmos DB (предварительная версия), необходимо переносить с помощью средства переноса данных. 

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * импорт данных с помощью средства переноса данных;
> * импорт данных с помощью AzCopy.
> * миграция из API таблицы (предварительная версия) в API таблицы. 

## <a name="prerequisites"></a>Предварительные требования

* **Увеличьте пропускную способность.** Продолжительность переноса данных зависит от пропускной способности, настроенной для отдельного контейнера или набора контейнеров. Увеличьте пропускную способность для крупных миграций. После переноса уменьшите пропускную способность для экономии расходов. Дополнительные сведения об увеличении пропускной способности на портале Azure см. в статье об уровнях производительности и ценовых категориях в Azure Cosmos DB.

* **Создайте ресурсы Azure Cosmos DB**. Прежде чем переносить данные, создайте все таблицы на портале Azure. Если вы выполняете перенос в учетную запись Azure Cosmos DB, обладающую пропускной способностью уровня базы данных, обязательно укажите ключ раздела при создании таблиц Azure Cosmos DB.

## <a name="data-migration-tool"></a>Средство переноса данных

Импортировать имеющиеся данные хранилища таблиц Azure в учетную запись API таблицы (общедоступная версия) или перенести данные из учетной записи API таблицы (предварительная версия) в учетную запись API таблицы (общедоступная версия) можно с помощью средства переноса данных Azure Cosmos DB командной строки (dt.exe). Другие источники в настоящее время не поддерживаются. Средство переноса данных на основе пользовательского интерфейса (dtui.exe) в настоящее время не поддерживается для учетных записей API таблиц. 

Чтобы выполнить миграцию данных таблицы, сделайте следующее:

1. Скачайте средство миграции с сайта [GitHub](https://github.com/azure/azure-documentdb-datamigrationtool).
2. Запустите `dt.exe` с помощью аргументов командной строки для своего сценария. `dt.exe` принимает команду в следующем формате:

   ```bash
    dt.exe [/<option>:<value>] /s:<source-name> [/s.<source-option>:<value>] /t:<target-name> [/t.<target-option>:<value>] 
   ```

Существуют такие параметры для команды:

    /ErrorLog: Optional. Name of the CSV file to redirect data transfer failures
    /OverwriteErrorLog: Optional. Overwrite error log file
    /ProgressUpdateInterval: Optional, default is 00:00:01. Time interval to refresh on-screen data transfer progress
    /ErrorDetails: Optional, default is None. Specifies that detailed error information should be displayed for the following errors: None, Critical, All
    /EnableCosmosTableLog: Optional. Direct the log to a cosmos table account. If set, this defaults to destination account connection string unless /CosmosTableLogConnectionString is also provided. This is useful if multiple instances of DT are being run simultaneously.
    /CosmosTableLogConnectionString: Optional. ConnectionString to direct the log to a remote cosmos table account. 

### <a name="command-line-source-settings"></a>Параметры источника командной строки

Определить хранилище таблиц Azure или API таблицы (предварительная версия) как источник миграции можно с помощью следующих параметров источника.

    /s:AzureTable: Reads data from Azure Table storage
    /s.ConnectionString: Connection string for the table endpoint. This can be retrieved from the Azure portal
    /s.LocationMode: Optional, default is PrimaryOnly. Specifies which location mode to use when connecting to Azure Table storage: PrimaryOnly, PrimaryThenSecondary, SecondaryOnly, SecondaryThenPrimary
    /s.Table: Name of the Azure Table
    /s.InternalFields: Set to All for table migration as RowKey and PartitionKey are required for import.
    /s.Filter: Optional. Filter string to apply
    /s.Projection: Optional. List of columns to select

Чтобы получить строку подключения к источнику при импорте из хранилища таблиц Azure, откройте портал Azure, щелкните **Учетные записи хранения** > **Учетная запись** > **Ключи доступа**, а затем нажмите кнопку "Копировать", чтобы скопировать **строку подключения**.

:::image type="content" source="./media/table-import/storage-table-access-key.png" alt-text="Снимок экрана: параметры источника HBase":::

Чтобы получить строку подключения к источнику при импорте из учетной записи API таблицы Azure Cosmos DB (предварительная версия), откройте портал Azure, щелкните **Azure Cosmos DB** > **Учетная запись** > **Строка подключения**, а затем нажмите кнопку "Копировать", чтобы скопировать **строку подключения**.

:::image type="content" source="./media/table-import/cosmos-connection-string.png" alt-text="Снимок экрана: параметры источника HBase":::

[Пример команды хранилища таблиц Azure](#azure-table-storage)

[Пример команды API таблицы Azure Cosmos DB (предварительная версия)](#table-api-preview)

### <a name="command-line-target-settings"></a>Параметры целевого объекта командной строки

Определите API таблицы Azure Cosmos DB в качестве целевого объекта для переноса с помощью следующих параметров целевого объекта.

    /t:TableAPIBulk: Uploads data into Azure CosmosDB Table in batches
    /t.ConnectionString: Connection string for the table endpoint
    /t.TableName: Specifies the name of the table to write to
    /t.Overwrite: Optional, default is false. Specifies if existing values should be overwritten
    /t.MaxInputBufferSize: Optional, default is 1GB. Approximate estimate of input bytes to buffer before flushing data to sink
    /t.Throughput: Optional, service defaults if not specified. Specifies throughput to configure for table
    /t.MaxBatchSize: Optional, default is 2MB. Specify the batch size in bytes

<a id="azure-table-storage"></a>
### <a name="sample-command-source-is-azure-table-storage"></a>Пример команды: источник — хранилище таблиц Azure

Ниже приведен пример команды для импорта данных из хранилища таблиц Azure в API таблицы:

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Table storage account name>;AccountKey=<Account Key>;EndpointSuffix=core.windows.net /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```
<a id="table-api-preview"></a>
### <a name="sample-command-source-is-azure-cosmos-db-table-api-preview"></a>Пример команды: источник — API таблиц Azure Cosmos DB (предварительная версия)

Ниже приведен пример команды для импорта данных из API таблицы (предварительная версия) в API таблицы (общедоступная версия):

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Table API preview account name>;AccountKey=<Table API preview account key>;TableEndpoint=https://<Account Name>.documents.azure.com; /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

## <a name="migrate-data-by-using-azcopy"></a>Перенос данных с помощью AzCopy

С помощью служебной программы командной строки AzCopy можно также перенести данные из табличного хранилища Azure в API таблицы Azure Cosmos DB. Чтобы использовать AzCopy, сначала необходимо экспортировать данные, как описано в разделе [Экспорт данных из хранилища таблиц](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#export-data-from-table-storage), а затем импортировать их, как описано в разделе [Импорт данных в хранилище таблиц](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#import-data-into-table-storage).

При выполнении импорта в Azure Cosmos DB см. следующий пример. Обратите внимание, что значение /Dest использует cosmosdb, а не ядро.

Пример команды импорта:

```
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.cosmosdb.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

## <a name="migrate-from-table-api-preview-to-table-api"></a>перенос из API таблицы (предварительная версия) в API таблицы.

> [!WARNING]
> Если вы хотите воспользоваться преимуществами общедоступных таблиц, выполните перенос имеющихся таблиц в предварительной версии, как указано в этом разделе. В противном случае в течение нескольких недель будет выполнен автоматический перенос для имеющихся клиентов предварительной версии. Однако обратите внимание, что для автоматически перенесенных таблиц предварительной версии появятся определенные ограничения, в то время как для созданных таблиц ограничений не будет.
> 

Доступна общедоступная версия API таблицы. Ниже приведены различия между предварительной и общедоступной версиями таблиц в коде, выполняемом в облаке, а также в коде, выполняемом на клиенте. Поэтому мы не рекомендуем использовать клиентский пакет SDK предварительной версии с учетной записью общедоступной версии API таблицы и наоборот. Клиентам предварительной версии API таблицы, которые хотят продолжить использование имеющихся таблиц, необходимо перенести данные из предварительной версии в общедоступную среду или дождаться автоматического переноса. Если вы ждете автоматического переноса, вам придет уведомление об ограничениях для перенесенных таблиц. После переноса можно создавать таблицы в имеющейся учетной записи без ограничений (ограничения применяются только к перенесенным таблицам).

Для переноса данных из API таблицы (предварительная версия) в общедоступную версию API таблицы сделайте следующее:

1. Создайте учетную запись Azure Cosmos DB и задайте в качестве типа API таблицу Azure, как описано в разделе [Создание учетной записи базы данных](create-table-dotnet.md#create-a-database-account).

2. Измените клиентов для использования в общедоступной версии [пакетов SDK для API таблицы](table-sdk-dotnet.md).

3. Перенесите данные клиента из таблиц предварительной версии в общедоступную версию таблицы с помощью средства переноса данных. Инструкции по использованию средства переноса данных см. в [этом](#data-migration-tool) разделе. 

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]
> * импорт данных с помощью средства переноса данных;
> * импорт данных с помощью AzCopy.
> * перенос из API таблицы (предварительная версия) в API таблицы.

Теперь вы можете перейти к следующему руководству, из которого вы узнаете, как запрашивать данные с помощью API таблицы Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Как выполнять запросы с помощью SQL в базе данных Azure Cosmos DB](../cosmos-db/tutorial-query-table.md)
