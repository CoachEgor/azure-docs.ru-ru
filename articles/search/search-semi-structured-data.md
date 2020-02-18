---
title: Учебник. индексирование струтуред данных в больших двоичных объектах JSON
titleSuffix: Azure Cognitive Search
description: Сведения об индексировании и поиске частично структурированных больших двоичных объектов JSON в Azure с помощью интерфейсов REST API службы "Когнитивный поиск Azure" и Postman.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 0603ad1fbecf33e5880fd7f18d35af51795f8e39
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2020
ms.locfileid: "77251997"
---
# <a name="rest-tutorial-index-and-search-semi-structured-data-json-blobs-in-azure-cognitive-search"></a>Руководство по работе с другими компонентами: индексирование и поиск частично структурированных данных (BLOB-объекты JSON) в Azure Когнитивный поиск

Служба "Когнитивный поиск Azure" может индексировать документы и массивы JSON в хранилище BLOB-объектов Azure с помощью [индексатора](search-indexer-overview.md), который умеет читать полуструктурированные данные. Частично структурированные данные содержат теги или метки, отделяющие содержимое в данных. Этим они отличаются от неструктурированных данных, которые должны быть полностью проиндексированы, и от формально структурированных данных, которые соответствуют модели данных, например схеме реляционной базы данных, и поддерживают индексирование по отдельным полям.

В этом учебнике используются [интерфейсы REST API службы "Когнитивный поиск Azure"](https://docs.microsoft.com/rest/api/searchservice/) и клиент REST для выполнения следующих задач:

> [!div class="checklist"]
> * Настройка источника данных службы "Когнитивный поиск Azure" для контейнера больших двоичных объектов.
> * Создание индекса службы "Когнитивный поиск Azure", в котором будет находится содержимое для поиска.
> * Настройка и запуск индексатора для чтения контейнера и извлечения содержимого для поиска из хранилища BLOB-объектов Azure.
> * Поиск по индексу, который вы только что создали.

## <a name="prerequisites"></a>Предварительные требования

В этом кратком руководстве используются приведенные ниже службы, инструменты и данные. 

[Создайте службу "Когнитивный поиск Azure"](search-create-service-portal.md) или [найдите имеющуюся службу](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) в рамках текущей подписки. Вы можете использовать бесплатную службу для выполнения инструкций, описанных в этом учебнике. 

[Создайте учетную запись хранения Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) для хранения примера данных.

[Классическое приложение Postman](https://www.getpostman.com/) для отправки запросов к службе "Когнитивный поиск Azure".

Архив [clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip) содержит данные, используемые в этом учебнике. Скачайте этот файл и распакуйте его в собственной папке. Источником данных, преобразованных в JSON для этого учебника, является [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results).

## <a name="get-a-key-and-url"></a>Получение ключа и URL-адреса

Вызовам REST требуется URL-адрес службы и ключ доступа при каждом запросе. Служба поиска создана с обоими элементами, поэтому если вы добавили службу "Когнитивный поиск Azure" в подписку, выполните следующие действия для получения необходимых сведений:

1. [Войдите на портал Azure](https://portal.azure.com/) и на странице **обзора** службы поиска получите URL-адрес. Пример конечной точки может выглядеть так: `https://mydemo.search.windows.net`.

1. В разделе **Параметры** > **Ключи** получите ключ администратора, чтобы обрести полные права на службу. Существуют два взаимозаменяемых ключа администратора, предназначенных для обеспечения непрерывности бизнес-процессов на случай, если вам потребуется сменить один из них. Вы можете использовать первичный или вторичный ключ для выполнения запросов на добавление, изменение и удаление объектов.

![Получение конечной точки HTTP и ключа доступа](media/search-get-started-postman/get-url-key.png "Получение конечной точки HTTP и ключа доступа")

Для выполнения любого запроса к службе требуется использование ключа API. Если есть действительный ключ, для каждого запроса устанавливаются отношения доверия между приложением, которое отправляет запрос, и службой, которая его обрабатывает.

## <a name="prepare-sample-data"></a>Подготовка примера данных

1. [Войдите на портал Azure](https://portal.azure.com), перейдите к учетной записи хранения Azure, выберите **Большие двоичные объекты**, а затем щелкните **Контейнер**.

1. [Создайте контейнер больших двоичных объектов](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) для хранения примера данных. Можно задать любое из допустимых значений уровня общего доступа.

1. Откройте контейнер после создания и на панели команд выберите **Загрузить**.

   ![Отправка на панели команд](media/search-semi-structured-data/upload-command-bar.png "Отправка на панели команд")

1. Перейдите к папке, содержащей примеры файлов. Выберите их, а затем щелкните **Загрузить**.

   ![Передача файлов](media/search-semi-structured-data/clinicalupload.png "Upload files")

После завершения отправки файлы должны появиться в собственной вложенной папке внутри контейнера данных.

## <a name="set-up-postman"></a>Настройка Postman

Запустите Postman и настройте HTTP-запрос. Если вы не работали с этим инструментом, изучите статью о [работе с REST API службы "Когнитивный поиск Azure" с помощью Postman](search-get-started-postman.md).

Для каждого вызова в этом учебнике используется метод запроса **POST**. Заголовки ключей — Content-type и api-key. Значения ключей заголовков — application/json и admin key (ключ администратора — это заполнитель для первичного ключа поиска) соответственно. Тело — это расположение фактического содержимого вызова. В зависимости от используемого клиента возможны некоторые различия в способах создания запросов, однако эти являются основными.

  ![Частично структурированный поиск](media/search-semi-structured-data/postmanoverview.png)

Мы используем Postman для трех вызовов API к службе поиска для создания источника данных, индекса и индексатора. Источник данных содержит указатель к вашей учетной записи хранения и данным JSON. Служба поиска создает подключение при отправке данных.

Строки запроса должны указывать версию API. Каждый вызов должен возвращать результат **201 Created** (создан ресурс). Общедоступная версия API (api-version) для использования массивов JSON — `2019-05-06`.

Выполните следующие три вызова API из клиента REST.

## <a name="create-a-data-source"></a>Создание источника данных

[API создания источника данных](https://docs.microsoft.com/rest/api/searchservice/create-data-source) создает объект когнитивный Поиск Azure, указывающий, какие данные индексировать.

Конечная точка этого вызова — `https://[service name].search.windows.net/datasources?api-version=2019-05-06`. Замените `[service name]` именем службы поиска. 

Для этого вызова в тексте запроса необходимо указать имя и ключ учетной записи хранения, а также имя контейнера большого двоичного объекта. Ключ учетной записи хранения можно найти на портале Azure в разделе учетной записи хранения **Ключи доступа**. На следующем рисунке показано расположение:

  ![Частично структурированный поиск](media/search-semi-structured-data/storagekeys.png)

Обязательно замените `[storage account name]`, `[storage account key]` и `[blob container name]` в тексте вызова перед его выполнением.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "[blob container name]"}
}
```

Результат должен выглядеть следующим образом:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "[mycontainernamehere]",
        "query": null
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

## <a name="create-an-index"></a>Создание индекса
    
Второй вызов — [API создания индекса](https://docs.microsoft.com/rest/api/searchservice/create-index), создающий индекс службы "Когнитивный поиск Azure", в котором хранятся все доступные для поиска данные. Индекс указывает все параметры и их атрибуты.

URL-адрес этого вызова — `https://[service name].search.windows.net/indexes?api-version=2019-05-06`. Замените `[service name]` именем службы поиска.

Сначала замените URL-адрес. Затем скопируйте и вставьте следующий код в тело и выполните запрос.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

Результат должен выглядеть следующим образом:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

## <a name="create-and-run-an-indexer"></a>Создание и запуск индексатора

Индексатор подключает источник данных, импортирует данные к целевому индексу поиска и при необходимости предоставляет расписание для автоматизации обновления данных. REST API — [создание индексатора](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

URL-адрес этого вызова — `https://[service name].search.windows.net/indexers?api-version=2019-05-06`. Замените `[service name]` именем службы поиска.

Сначала замените URL-адрес. Затем скопируйте и вставьте следующий код в тело и отправьте запрос. Запрос обрабатывается немедленно. При возвращении ответа вы получите индекс с возможностью полнотекстового поиска.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Результат должен выглядеть следующим образом:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>Поиск JSON-файлов

Вы можете начать поиск сразу после загрузки первого документа. Для этой задачи используйте [**проводник поиска**](search-explorer.md) на портале.

На портале Azure откройте страницу**обзора** службы поиска и в списке **Индексы** найдите созданный индекс.

Обязательно выберите только что созданный индекс. 

  ![Неструктурированный поиск](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Поиск определяемых пользователем метаданных

Как и прежде, данные можно запрашивать несколькими способами: полнотекстовый поиск, системные свойства или определяемые пользователем метаданные. Поиск свойств системы и определяемых пользователем метаданных можно выполнить только с параметром `$select`, если они были помечены как **извлекаемые** во время создания целевого индекса. Параметры индекса невозможно изменить после создания, но можно добавить дополнительные параметры.

Пример простого запроса — `$select=Gender,metadata_storage_size`, который ограничивает возвращаемые значения этими двумя параметрами.

  ![Частично структурированный поиск](media/search-semi-structured-data/lastquery.png)

Пример более сложного запроса — `$filter=MinimumAge ge 30 and MaximumAge lt 75`, который возвращает только результаты, в которых значение параметра MinimumAge не меньше 30, а MaximumAge — не больше 75.

  ![Частично структурированный поиск](media/search-semi-structured-data/metadatashort.png)

Поэкспериментируйте и попробуйте создать несколько дополнительных запросов самостоятельно, если хотите. Вы можете использовать логические операторы (and, or, not) и операторы сравнения (eq, ne, gt, lt, ge, le). При сравнении строк учитывается регистр.

Параметр `$filter` работает только с метаданными, которые отмечены как фильтруемые при создании индекса.

## <a name="clean-up-resources"></a>Очистка ресурсов

Самый быстрый способ очистки по завершении работы с учебником — удалить группу ресурсов, содержащую службу "Когнитивный поиск Azure". Теперь можно удалить группу ресурсов вместе со всем ее содержимым без возможности восстановления. На портале имя группы ресурсов находится на странице "Обзор" службы "Когнитивный поиск Azure".

## <a name="next-steps"></a>Следующие шаги

Существует ряд подходов и несколько вариантов индексирования больших двоичных объектов JSON. Следующим шагом будет рассмотрение и тестирование различных вариантов. Это позволит понять, что лучше всего подходит для вашего сценария.

> [!div class="nextstepaction"]
> [How to index JSON blobs using Azure Search Blob indexer](search-howto-index-json-blobs.md) (Индексация больших двоичных объектов JSON с помощью индексатора больших двоичных объектов службы "Когнитивный поиск Azure")
