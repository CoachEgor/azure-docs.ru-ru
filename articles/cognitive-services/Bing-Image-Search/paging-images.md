---
title: Разбивка на страницы списка изображений, возвращаемых API Bing для поиска изображений
titleSuffix: Azure Cognitive Services
description: Просмотр различных страниц с изображениями, возвращенными API Bing для поиска изображений.
services: cognitive-services
author: swhite-msft
manager: cgonlun
ms.assetid: 3C8423F8-41E0-4F89-86B6-697E840610A7
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 9368abe7d3b6ad6cf6e86b503dca4fca4f18739c
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388459"
---
# <a name="page-through-the-images-results"></a>Разбивка на страницы списка возвращаемых изображений

Когда вы вызываете API для поиска изображений, Bing возвращает список результатов. Список — это подмножество общего количества результатов, относящихся к запросу. Чтобы получить предполагаемое общее количество доступных результатов, необходимо обратиться к полю [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#totalestimatedmatches) объекта ответа.  

В следующем примере показано поле `totalEstimatedMatches`, содержащееся в результатах поиска изображений.  

```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8...",
    "totalEstimatedMatches" : 838,
    "value" : [...]  
}  
```  

Чтобы разбить на страницы список доступных изображений, используйте параметры запроса [count](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#count) и [offset](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#offset).  

Параметр `count` задает количество результатов, возвращаемых в ответе. Максимальное количество результатов, которые можно запросить в ответе, равно 150. Значение по умолчанию — 35. Фактическое число полученных результатов может быть меньше запрошенного.

Параметр `offset` задает количество пропускаемых результатов. Значение `offset` начинается с нуля и должно быть меньше (`totalEstimatedMatches` - `count`).  

Если вы хотите отображать 20 изображений на странице, необходимо задать для параметра `count` значение 20, а для параметра `offset` — значение 0, чтобы получить первую страницу результатов. В следующем примере показано, как запрашиваются 20 изображений с начальным смещением в 40 результатов.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Если значение по умолчанию `count` подходит для вашей реализации, необходимо указать только параметр запроса `offset`.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Можно предположить, что для разбиения списка на страницы по 35 изображений на каждой странице для параметра `offset` необходимо при первом запросе задать значение 0, а затем увеличивать `offset` на 35 при каждом последующем запросе. Но некоторые результаты из предыдущего ответа могут повторяться в последующем ответе. Например, первые два изображения в ответе могут совпадать с последними двумя изображениями из предыдущего ответа.

Чтобы исключить повторяющиеся результаты, используйте поле [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#nextoffset) объекта `Images`. В поле `nextOffset` указывается значение `offset` для следующего запроса. Например, для разбиения по 30 изображений на страницу при первом запросе необходимо задать для параметра `count` значение 30, а для `offset` — 0. При следующем запросе задайте для параметра `count` значение 30, а для `offset` — значение `nextOffset` из предыдущего ответа. Для перемещения по страницам в обратном направлении рекомендуется использовать такой же стек смещений и отображать самые последние результаты.

> [!NOTE]
> Разбиение на страницы доступно только для поиска изображений (/images/search) и не применимо к аналитическим сведениям об изображениях или изображениям, набирающим популярность (/images/trending).

> [!NOTE]
> Поле `TotalEstimatedAnswers` содержит приблизительное общее число результатов поиска, которые вы можете получить для текущего запроса.  При задании параметров `count` и `offset` число `TotalEstimatedAnswers` может измениться. 
