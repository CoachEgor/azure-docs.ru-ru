---
title: Предложение условий поиска с помощью API автозаполнения Bing
titleSuffix: Azure Cognitive Services
description: В этой статье рассматривается понятие предложения запроса с использованием API автозаполнения Bing и влияние длины запроса на релевантность.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: 060dbd29ee4ddb78e8ae9b2ed4e7814da3c4eebf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "74072891"
---
# <a name="suggesting-query-terms"></a>Предложение терминов запроса

API автозаполнения Bing обычно вызывается всякий раз, когда пользователь вводит новый символ в поле поиска вашего приложения. Полнота строки запроса влияет на релевантность предложений, возвращаемых API-интерфейсом. Чем ближе строка запроса к завершению, тем точнее будет список предложений для этого запроса. Например, возвращаемые API-интерфейсом предложения при вводе `s`, скорее всего, будут менее значимыми, чем результаты для фразы `sailing dinghies`.

## <a name="example-request"></a>Пример запроса

В следующем примере показано, как отправить запрос, который возвращает предлагаемые строки запроса для слова *sail*. Не забудьте закодировать в URL-адресе частичный запрос условия пользователя, когда устанавливаете параметр запроса [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference#query). Например, если пользователь вводит *sailing les*, установите параметр `q` в `sailing+les` или `sailing%20les`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Этот ответ содержит список объектов [SearchAction](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference#searchaction) с предлагаемыми условиями запроса.

```json
{
    "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
    "displayText" : "sailing lessons seattle",
    "query" : "sailing lessons seattle",
    "searchKind" : "WebSearch"
}, ...
```

## <a name="using-suggested-query-terms"></a>Использование предлагаемых условий запроса

Каждое из предложений содержит поля `displayText`, `query` и `url`. Поле `displayText` содержит предлагаемые запросы, которые вы можете поместить в раскрывающийся список для поля поиска. Должны отображаться все предоставленные предложения и строго в указанном порядке.

В примере ниже показан раскрывающийся список поля поиска с предложениями условий запроса из API "Автозаполнение Bing".

![Автозаполнение раскрывающегося списка поля поиска](../media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Если пользователь выбирает из раскрывающегося списка предложенный запрос, используйте условие запроса из поля `query`, чтобы вызвать раздел [Документация по API Bing для поиска в Интернете](../../bing-web-search/search-the-web.md) и отобразить полученные результаты. Или вы можете использовать URL-адрес из поля `url`, чтобы переадресовать пользователя на страницу результатов поиска Bing.

## <a name="next-steps"></a>Дальнейшие действия

* [Что такое API Автозаполнения Bing?](../get-suggested-search-terms.md)