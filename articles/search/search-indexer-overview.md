---
title: Индексаторы для обхода данных при импорте
titleSuffix: Azure Cognitive Search
description: Просканировать базу данных SQL Azure, SQL Управляемый экземпляр, Azure Cosmos DB или службу хранилища Azure, чтобы извлечь данные для поиска и заполнить индекс Azure Когнитивный поиск.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: a728eff7addc7f835f82e795457e722fd60d2e30
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564597"
---
# <a name="indexers-in-azure-cognitive-search"></a>Indexers in Azure Cognitive Search (Индексаторы в службе "Когнитивный поиск Azure")

*Индексатор* в когнитивный Поиск Azure — это средство, которое извлекает данные и метаданные с возможностью поиска из внешнего источника данных Azure и заполняет индекс на основе сопоставления полей с полями между индексом и источником данных. Этот подход иногда называют "моделью извлечения", так как служба извлекает данные в, не требуя написания кода, добавляющего данные в индекс.

Индексаторы основаны на типах источников данных или платформах с отдельными индексаторами для SQL Server в Azure, Cosmos DB, хранилище таблиц Azure и хранилище BLOB-объектов. Индексаторы хранилища BLOB-объектов имеют дополнительные свойства, относящиеся к типам содержимого больших двоичных объектов.

Индексатор можно использовать как отдельное средство для приема данных. Кроме того, можно использовать комбинацию методов, предусматривающих использование индексатора для загрузки только некоторых полей в индексе.

Индексаторы можно запускать по требованию или по регулярному расписанию обновления данных, которое выполняется каждые пять минут. Для более частых обновлений требуется модель отправки, которая одновременно обновляет данные в Когнитивный поиск Azure и внешнем источнике данных.

## <a name="approaches-for-creating-and-managing-indexers"></a>Способы создания индексаторов и управления ими

Далее представлены способы создания индексаторов и управления ими.

* [Мастер импорта данных > портала](search-import-data-portal.md)
* [API REST службы](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* [Пакет SDK для .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations)

Изначально новый индексатор объявляется в качестве предварительной версии функции. Предварительные версии функций добавляют в API (REST и .NET), а затем после выхода общедоступной версии интегрируются на портале. При оценке нового индексатора следует запланировать написание кода.

## <a name="permissions"></a>Разрешения

Для всех операций, связанных с индексаторами, включая запросы на получение состояния или определений, требуется [Административный ключ API](search-security-api-keys.md). 

<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Поддерживаемые источники данных

Данные о сканировании, выполненном индексаторами, хранятся в Azure.

* [Хранилище BLOB-объектов Azure](search-howto-indexing-azure-blob-storage.md)
* [Azure Data Lake Storage 2-го поколения](search-howto-index-azure-data-lake-storage.md) (Предварительная версия)
* [хранилище таблиц Azure](search-howto-indexing-azure-tables.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [База данных SQL Azure и Управляемый экземпляр SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [SQL Server на виртуальных машинах Azure](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md)
* [Управляемый экземпляр SQL](search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md)

## <a name="basic-configuration-steps"></a>Основные этапы настройки
В индексаторах реализованы уникальные функции для работы с источниками данных. Поэтому тип индексатора будет определять особенности настройки источника данных или индексатора. Тем не менее всем индексаторам присущи сходные структура и требования. Ниже описаны действия, общие для всех индексаторов.

### <a name="step-1-create-a-data-source"></a>Шаг 1. Создание источника данных
Индексатор получает соединение с источником данных из объекта *источника данных* . Определение источника данных предоставляет строку подключения и, возможно, учетные данные. Чтобы создать ресурс, вызовите REST API для [создания источника данных](https://docs.microsoft.com/rest/api/searchservice/create-data-source) или [класс DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource).

Источники данных настраиваются и управляются независимо от индексаторов, которые их используют. Это означает, что источник данных может использоваться несколькими индексаторами для одновременной загрузки нескольких индексов.

### <a name="step-2-create-an-index"></a>Шаг 2. Создание индекса
Хотя индексатор автоматизирует некоторые задачи, связанные с приемом данных, это обычно не распространяется на создание индекса. Перед началом работы у вас должен быть стандартный индекс с полями, которые соответствуют полям во внешнем источнике данных. Поля должны совпадать по имени и типу данных. Дополнительные сведения о структурировании индекса см. в статье [Создание индекса (когнитивный Поиск Azure REST API)](https://docs.microsoft.com/rest/api/searchservice/Create-Index) или [класса индекса](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index). Справку по сопоставлению полей см. [в разделе сопоставления полей в Azure когнитивный Поиск индексаторы](search-indexer-field-mappings.md).

> [!Tip]
> Хотя индексаторы не могут создавать индексы, вы можете это сделать с помощью мастера **импорта данных**. В большинстве случаев мастер может определить схему индекса на основе существующих метаданных в источнике, предоставляя предварительную схему, которую можно изменить в мастере во время его работы. После создания индекса в службе последующие изменения на портале ограничиваются добавлением новых полей. Используйте мастер для создания индекса, но не для изменения. Чтобы пройти практическое обучение, перейдите к [пошаговому руководству по порталу](search-get-started-portal.md).

### <a name="step-3-create-and-schedule-the-indexer"></a>Шаг 3. Создание и планирование индексатора
Определение индексатора — это конструкция, которая объединяет все элементы, связанные с приемом данных. Обязательные элементы включают источник данных и индекс. Необязательные элементы включают в себя расписание и сопоставления полей. Сопоставление полей необязательно, если поля источника и поля индекса четко соответствуют друг другу. Дополнительные сведения о структурировании индексатора см. в статье [Создание индексатора (REST API когнитивный Поиск Azure)](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

<a id="RunIndexer"></a>

## <a name="run-indexers-on-demand"></a>Выполнение индексаторов по запросу

Хотя можно запланировать индексирование, для вызова индексатора также может быть вызвано использование [команды Run](https://docs.microsoft.com/rest/api/searchservice/run-indexer):

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2020-06-30
    api-key: [Search service admin key]

> [!NOTE]
> При успешном возвращении API планируется вызов индексатора, но фактическая обработка происходит асинхронно. 

Вы можете отслеживать состояние индексатора на портале или с помощью API получения состояния индексатора. 

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Получение состояния индексатора

Состояние и журнал выполнения индексатора можно получить с помощью [команды получить состояние индексатора](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status):


    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2020-06-30
    api-key: [Search service admin key]

Ответ содержит сведения об общем состоянии индексатора, последнем (или текущем) вызове индексатора, а также журнал последних вызовов индексатора.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

История выполнения включает не более 50 последних завершенных выполнений, которые сортируются в обратном хронологическом порядке (то есть в ответе первым отображается последнее выполнение).

## <a name="next-steps"></a>Дальнейшие шаги
Теперь, когда вы ознакомились с основной идеей, можно переходить к изучению требований и задач, связанных с каждым типом источника данных.

* [База данных SQL Azure, SQL Управляемый экземпляр или SQL Server на виртуальной машине Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Хранилище BLOB-объектов Azure](search-howto-indexing-azure-blob-storage.md)
* [хранилище таблиц Azure](search-howto-indexing-azure-tables.md)
* [Индексирование больших двоичных объектов CSV с помощью индексатора больших двоичных объектов Azure Когнитивный поиск](search-howto-index-csv-blobs.md)
* [Индексирование больших двоичных объектов JSON с помощью индексатора BLOB-объектов Когнитивный поиск Azure](search-howto-index-json-blobs.md)
