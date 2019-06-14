---
title: Конечная точка службы предварительного просмотра URL-адресов в проектах
titlesuffix: Azure Cognitive Services
description: Сводные сведения о конечной точке службы предварительного просмотра URL-адресов.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: reference
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 7cc52493ec0e2b9c81d52da4bb22102c2c7e5e5c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60712499"
---
# <a name="project-url-preview-endpoint"></a>Конечная точка службы предварительного просмотра URL-адресов в проектах

API службы предварительного просмотра URL-адресов включает в себя одну конечную точку.

## <a name="endpoint"></a>Конечная точка
Чтобы получить службу предварительного просмотра URL-адресов, отправьте запрос к следующей конечной точке. Используйте заголовки и параметры URL-адреса для других спецификаций.

GET:
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

```

### <a name="query-parameters"></a>Параметры запроса
|Name|Значение|type|Обязательно для заполнения|  
|----------|-----------|----------|--------------|  
|q|URL-адрес для предварительного просмотра|String |Да|
|safeSearch|Недопустимое содержимое для взрослых или пиратское содержимое будет заблокировано с кодом ошибки 400, при этом флаг *isFamilyFriendly* не возвращается. <p>Ниже описано поведение, применяемое к разрешенному содержимому для взрослых. Возвращается код состояния 200, а флаг *isFamilyFriendly* имеет значение false.<ul><li>safeSearch=strict: не возвращаются заголовок, описание, URL-адрес и изображение.</li><li>safeSearch=moderate: возвращаются заголовок, URL-адрес и описание, но не изображение.</li><li>safeSearch=off: возвращаются все объекты и элементы ответа — заголовок, URL-адрес, описание и изображение.</li></ul> |String|Необязательно. </br> По умолчанию используется значение safeSearch=strict.| 

## <a name="response-object"></a>Объект ответа

Ответ включает в себя заголовки HTTP и объект WebPage с атрибутами, как показано в следующем примере: `name`, `url`, `description`, `isFamilyFriendly` и `primaryImageOfPage`.

```
BingAPIs-TraceId: 15AFE52A97AA422F960433A94803F6CE
BingAPIs-SessionId: 40587764F42142D3A8BA99F66B2B3BB6
X-MSEdge-ClientID: 0389E3EDED106B5E1424E82FEC436A56
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 15AFE52A97AA422F960433A94803F6CE Ref B: PAOEDGE0418 Ref C: 2018-03-30T16:36:27Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile ...",
  "url": "https://swiftkey.com/",
   "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

```

## <a name="next-steps"></a>Дальнейшие действия
- [Краткое руководство для C#](csharp.md)
- [Краткое руководство для Java](java-quickstart.md)
- [Краткое руководство для JavaScript](javascript.md)
- [Краткое руководство для Node](node-quickstart.md)
- [Краткое руководство для Python](python-quickstart.md)
