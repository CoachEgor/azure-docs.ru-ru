---
title: Сопоставление обогащенных полей входных данных, поддерживающих когнитивный поиск, с полями выходных данных в службе "Поиск Azure"
description: Извлечение и обогащение полей источников данных, а также сопоставление их с полями выходных данных в индекс поиска Azure.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.subservice: cognitive-search
ms.openlocfilehash: 1d1ef7be83601a1ccf09ac52f87d0fcc953c605c
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2019
ms.locfileid: "69639185"
---
# <a name="how-to-map-enriched-fields-to-a-searchable-index"></a>Сопоставление обогащенных полей с индексом, поддерживающим поиск

В этой статье вы узнаете, как сопоставлять обогащенные поля входных данных с полями выходных данных в индексе, поддерживающем поиск. После [определения набора навыков](cognitive-search-defining-skillset.md) вы должны сопоставить поля выходных данных любого навыка, который напрямую вносит значения в заданное поле в ваш индекс поиска. Сопоставление полей является обязательным этапом для перемещения содержимого из обогащенных документов в индекс.


## <a name="use-outputfieldmappings"></a>Использование outputFieldMappings
Чтобы сопоставить поля, добавьте `outputFieldMappings` в определение индексатора, как показано ниже.

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

Текст запроса имеет следующую структуру:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```
Для каждого сопоставления поля выходных данных, задайте имя обогащенного поля (sourceFieldName) и имя поля, как указано в индексе (targetFieldName).

Путь в sourceFieldName может представлять один элемент или несколько элементов. В приведенном выше примере ```/document/content/sentiment``` представляет одно числовое значение, а ```/document/content/organizations/*/description``` представляет несколько описаний организации. В случаях с несколькими элементами они преобразуются в массив, содержащий все элементы. Более конкретно, данные для примера ```/document/content/organizations/*/description``` в поле *описаний* будут выглядеть как плоский массив описаний до его индексации:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Следующие шаги
После сопоставления обогащенных полей с полями, поддерживающими поиск, для каждого поля, поддерживающего поиск, можно задать атрибуты поля [в качестве части определения индекса](search-what-is-an-index.md).

Дополнительные сведения о сопоставлении полей см. в статье [Сопоставление полей в индексаторах Поиска Azure](search-indexer-field-mappings.md).
