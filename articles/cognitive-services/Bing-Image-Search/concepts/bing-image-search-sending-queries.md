---
title: Отправка запросов на получение изображений — API Bing для поиска изображений
titleSuffix: Azure Cognitive Services
description: Дополнительные сведения о настройке запросов поиска, отправляемых в API Bing для поиска изображений.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: C2862E98-8BCC-423B-9C4A-AC79A287BE38
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: b2d142783146edcaf40125ce58e43fe001909412
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60635624"
---
# <a name="send-queries-to-the-bing-image-search-api"></a>Отправка запросов в API Bing для поиска изображений

API Bing для поиска изображений предоставляет аналогичный поисковой системе Bing.com/Images функционал. Его можно использовать для отправки поискового запроса в Bing и получать список изображений, соответствующих запросу.

## <a name="use-and-suggest-search-terms"></a>Использование и предложение поисковых запросов

После ввода поискового запроса он будет закодирован в URL-адресе перед параметром запроса [**q**](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query). Например, если вы вводите *sailing dinghies*, установите для параметра `q` значение `sailing+dinghies` или `sailing%20dinghies`.

Если в приложении предусмотрено поле для ввода условий поиска, то для упрощения работы можно использовать [API автозаполнения Bing](../../bing-autosuggest/get-suggested-search-terms.md). Этот API может отображать предлагаемые поисковые запросы в режиме реального времени. API возвращает предложенные строки запроса на основе частичных условий поиска и службы Cognitive Services.

## <a name="pivot-the-query"></a>Сведение запросов

Если Bing может сегментировать исходный поисковый запрос, возвращаемый объект [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) содержит поле `pivotSuggestions`. Сводка предложений может отображаться в виде необязательных поисковых запросов для пользователя. Например, если исходный запрос был *Microsoft Surface*, Bing может разделить запрос на *Microsoft* и *Surface* и предложить сведения для каждого из них. Эти предложения могут отображаться в виде необязательных поисковых запросов для пользователя.

В следующем примере показаны сводные предложения для *Microsoft Surface*.  

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface&FORM=OIIARP",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions": [...],
    "pivotSuggestions": [{
        "pivot": "microsoft",
        "suggestions": [{
            "text": "Contoso Surface",
            "displayText": "Contoso",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=OtterBox+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Contoso...",
                    "searchLink": "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Contoso+Surface..."
            }
        },
        {
            "text": "Adatum Surface",
            "displayText": "Adatum",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Adatum+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Adatum+Surface&pid=Ap..."
            }
        },
        ...
        ]
    },
    {
        "pivot": "surface",
        "suggestions": [{
            "text": "Microsoft Surface4",
            "displayText": "Surface4",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface...",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft..."
            }
        },
        {
            "text": "Microsoft Tablet",
            "displayText": "Tablet",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Tablet&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Microsoft+Tablet..."
            }
        },
        ...
    ],
    "nextOffsetAddCount": 0
}
```

Поле `pivotSuggestions` содержит список сегментов (сводок), на которые был разбит исходный запрос. Для каждой сводки ответ содержит список объектов [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj), содержащих предлагаемые запросы. Поле `text` содержит предложенные поисковые запросы. Поле `displayText` содержит условие, заменяющее сводку в исходном запросе. Например, дата выпуска Surface.

Если строка сводного запроса является тем, что ищет пользователь, используйте поля `text` и `thumbnail` для отображения этих строк. Создайте эскиз и текст с помощью URL-адреса `webSearchUrl` или `searchLink`. Используйте поле `webSearchUrl` для отправки пользователю результаты поиска Bing. Если вы предоставляете собственную страницу результатов, используйте поле `searchLink`.

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expand-the-query"></a>Расширение запроса

Если Bing может расширить запрос, чтобы сузить исходный поисковый запрос, объект [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) будет содержать поле `queryExpansions`. Например, если запрос был *Microsoft Surface*, расширенные запросы могут иметь вид:
- Microsoft Surface **Pro 3**.
- Microsoft Surface **RT**.
- Microsoft Surface **Phone**.
- Microsoft Surface **Hub**.

В следующем примере показаны расширенные запросы для *Microsoft Surface*.

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface...",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions":  [{
        "text": "Microsoft Surface Pro 3",
        "displayText": "Pro 3",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Pro+3...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Microsoft...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Pro+3..."
        }
    },
    {
        "text": "Microsoft Surface RT",
        "displayText": "RT",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+RT...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+RT..."
        }
    },
    {
        "text": "Microsoft Surface Phone",
        "displayText": "Phone",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Phone",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Phone..."
        }
    }],
    "pivotSuggestions": [...],
    "nextOffsetAddCount": 0
}
```

Поле `queryExpansions` содержит список объектов [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj). Поле `text` содержит расширенный поисковый запрос. Поле `displayText` содержит термин, который расширяет запрос. Если строка расширенного запроса является тем, что ищет пользователь, используйте поля `text` и `thumbnail` для отображения этих строк. Создайте эскиз и текст с помощью URL-адреса `webSearchUrl` или `searchLink`. Используйте поле `webSearchUrl` для отправки пользователю результаты поиска Bing. Если вы предоставляете собственную страницу результатов, используйте поле `searchLink`.

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->


## <a name="throttling-requests"></a>Запросы на регулирование

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Дальнейшие действия

Если вы еще ни разу не использовали API Bing для поиска изображений, ознакомьтесь со следующей [статьей](../quickstarts/csharp.md). Если вы ищете что-нибудь посложнее, ознакомьтесь с руководством по созданию [одностраничного веб-приложения](../tutorial-bing-image-search-single-page-app.md).
