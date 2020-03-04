---
title: Создание подключения к данным в сетке событий для обозреватель данных Azure с помощьюC#
description: Из этой статьи вы узнаете, как создать подключение к данным в службе "Сетка событий" для C#обозреватель данных Azure с помощью.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: 03963f60cc364dd36ad55c0a28e92e3b585bb38d
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2020
ms.locfileid: "78255077"
---
# <a name="create-an-event-grid-data-connection-for-azure-data-explorer-by-using-c"></a>Создание подключения к данным в сетке событий для обозреватель данных Azure с помощьюC#

> [!div class="op_single_selector"]
> * [Портал](ingest-data-event-grid.md)
> * [C#](data-connection-event-grid-csharp.md)
> * [Python](data-connection-event-grid-python.md)
> * [Шаблон Azure Resource Manager](data-connection-event-grid-resource-manager.md)


Обозреватель данных Azure — это быстрая и высокомасштабируемая служба для изучения данных журналов и телеметрии. Azure обозреватель данных обеспечивает прием (загрузку данных) из концентраторов событий, центров Интернета вещей и больших двоичных объектов, записанных в контейнеры больших двоичных объектов. В этой статье вы создадите подключение к данным в сетке событий для Azure обозреватель данных с C#помощью.

## <a name="prerequisites"></a>предварительные требования

* Если вы не установили Visual Studio 2019, вы можете скачать и использовать **бесплатную** [версию Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/). При установке Visual Studio необходимо включить возможность **разработки для Azure**.
* Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/) Azure, прежде чем начинать работу.
* Создание [кластера и базы данных](create-cluster-database-csharp.md)
* Создание [сопоставления таблиц и столбцов](net-standard-ingest-data.md#create-a-table-on-your-test-cluster)
* Задание [политик базы данных и таблиц](database-table-policies-csharp.md) (необязательно)
* Создайте [учетную запись хранения с подпиской службы "Сетка событий](ingest-data-event-grid.md#create-an-event-grid-subscription-in-your-storage-account)".

[!INCLUDE [data-explorer-data-connection-install-nuget-csharp](../../includes/data-explorer-data-connection-install-nuget-csharp.md)]

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-an-event-grid-data-connection"></a>Добавление подключения к данным в сетке событий

В следующем примере показано, как добавить подключение к данным в сетке событий программным способом. См. раздел [Создание подключения к данным в сетке событий в Azure обозреватель данных](ingest-data-event-grid.md#create-an-event-grid-data-connection-in-azure-data-explorer) для добавления подключения к данным сетки событий с помощью портал Azure.

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
var credential = new ClientCredential(clientId, clientSecret);
var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

var kustoManagementClient = new KustoManagementClient(credentials)
{
    SubscriptionId = subscriptionId
};

var resourceGroupName = "testrg";
//The cluster and database that are created as part of the Prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
var dataConnectionName = "myeventhubconnect";
//The event hub and storage account that are created as part of the Prerequisites
var eventHubResourceId = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.EventHub/namespaces/xxxxxx/eventhubs/xxxxxx";
var storageAccountResourceId = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.Storage/storageAccounts/xxxxxx";
var consumerGroup = "$Default";
var location = "Central US";
//The table and column mapping are created as part of the Prerequisites
var tableName = "StormEvents";
var mappingRuleName = "StormEvents_CSV_Mapping";
var dataFormat = DataFormat.CSV;

await kustoManagementClient.DataConnections.CreateOrUpdateAsync(resourceGroupName, clusterName, databaseName, dataConnectionName,
    new EventGridDataConnection(storageAccountResourceId, eventHubResourceId, consumerGroup, tableName: tableName, location: location, mappingRuleName: mappingRuleName, dataFormat: dataFormat));
```

|**Параметр** | **Рекомендуемое значение** | **Описание поля**|
|---|---|---|
| tenantId | *xxxxxxxx-XXXXX-XXXX-XXXX-XXXXXXXXX* | Идентификатор клиента. Также известен как идентификатор каталога.|
| subscriptionId | *xxxxxxxx-XXXXX-XXXX-XXXX-XXXXXXXXX* | Идентификатор подписки, используемый для создания ресурсов.|
| clientid | *xxxxxxxx-XXXXX-XXXX-XXXX-XXXXXXXXX* | Идентификатор клиента приложения, которое может получать доступ к ресурсам в клиенте.|
| clientSecret | *кскскскскскскскскскскскскскс* | Секрет клиента приложения, которое может получить доступ к ресурсам в клиенте. |
| имя_группы_ресурсов | *testrg* | Имя группы ресурсов, содержащей кластер.|
| clusterName | *mykustocluster* | Имя кластера.|
| databaseName | *mykustodatabase* | Имя целевой базы данных в кластере.|
| датаконнектионнаме | *мевенсубконнект* | Требуемое имя подключения к данным.|
| tableName | *стормевентс* | Имя целевой таблицы в целевой базе данных.|
| маппингруленаме | *StormEvents_CSV_Mapping* | Имя сопоставления столбцов, связанного с целевой таблицей.|
| Формат. | *-* | Формат данных сообщения.|
| евенсубресаурцеид | *Идентификатор ресурса* | Идентификатор ресурса концентратора событий, в котором сетка событий настроена для отправки событий. |
| сторажеаккаунтресаурцеид | *Идентификатор ресурса* | Идентификатор ресурса учетной записи хранения, в которой хранятся данные для приема. |
| Группы потребителей | *$Default* | Группа потребителей концентратора событий.|
| location | *Центральная часть США* | Расположение ресурса подключения к данным.|

## <a name="generate-sample-data"></a>Создание примера данных

Теперь, когда Azure Data Explorer и учетная запись хранения подключены, вы можете создать примеры данных и отправить их в хранилище BLOB-объектов.

Этот скрипт создает новый контейнер в вашей учетной записи хранения, передает существующий файл (как большой двоичный объект) в этот контейнер и выводит список больших двоичных объектов в контейнере.

```csharp
var azureStorageAccountConnectionString=<storage_account_connection_string>;

var containerName=<container_name>;
var blobName=<blob_name>;
var localFileName=<file_to_upload>;

// Creating the container
var azureStorageAccount = CloudStorageAccount.Parse(azureStorageAccountConnectionString);
var blobClient = azureStorageAccount.CreateCloudBlobClient();
var container = blobClient.GetContainerReference(containerName);
container.CreateIfNotExists();

// Set metadata and upload file to blob
var blob = container.GetBlockBlobReference(blobName);
blob.Metadata.Add("rawSizeBytes", "4096‬"); // the uncompressed size is 4096 bytes
blob.Metadata.Add("kustoIngestionMappingReference", "mapping_v2‬");
blob.UploadFromFile(localFileName);

// List blobs
var blobs = container.ListBlobs();
```

> [!NOTE]
> Azure обозреватель данных не удалит прием BLOB-объектов после приема. Храните большие двоичные объекты в течение трех – пять дней, используя [жизненный цикл хранилища BLOB-объектов Azure](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts?tabs=azure-portal) для управления удалением больших двоичных объектов.

[!INCLUDE [data-explorer-data-connection-clean-resources-csharp](../../includes/data-explorer-data-connection-clean-resources-csharp.md)]
