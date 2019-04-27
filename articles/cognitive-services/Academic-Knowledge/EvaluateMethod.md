---
title: Оценка метода — Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Используйте метод Evaluate для получения набора академических сущностей на основе выражения запроса.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: d2e628fb7fc502ef9ba81d20680d66f24fd7d138
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61339102"
---
# <a name="evaluate-method"></a>Метод Evaluate

**Evaluate** REST API используется для получения набора академических сущностей на основе выражения запроса.
<br>

**Конечная точка REST:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate? 
```   
<br>

## <a name="request-parameters"></a>Параметры запроса  

ИМЯ     | Value | Обязательный?  | ОПИСАНИЕ
-----------|-----------|---------|--------
**expr**       | Текстовая строка | Yes | Выражение запроса, которое указывает, какие сущности должны быть возвращены.
**model**      | Текстовая строка | Нет   | Имя модели, которую необходимо запросить.  В настоящее время значение по умолчанию соответствует *latest*.        
**attributes** | Текстовая строка | Нет <br>значение по умолчанию: Идентификатор | Список разделительных запятых, который определяет значения атрибутов, которые включены в ответ. Имена атрибутов с учетом регистра.
**count**        | Number | Нет <br>Значение по умолчанию: 10 | Количество результатов для возврата.
**offset**     | Number |   Нет <br>Значение по умолчанию: 0    | Индекс первого результата для возврата.
**OrderBy** |   Текстовая строка | Нет <br>По умолчанию: по снижению | Имя атрибута, который используется для сортировки списка сущностей. При желании можно указать восходящий или нисходящий порядок. Недопустимый формат: *name:asc* или *name:desc*.
  
 <br>

## <a name="response-json"></a>Ответ (JSON)

ИМЯ | ОПИСАНИЕ
-------|-----   
**expr** |  Параметр *expr* из запроса.
**entities** |  Массив из 0 или более сущностей, которые соответствуют выражению запроса. Каждая сущность содержит значение натурального логарифма вероятности и значения других запрашиваемых атрибутов.
**aborted** | Если истекло время ожидания запроса, то значение true.

<br>

#### <a name="example"></a>Пример:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate?expr=
Composite(AA.AuN=='jaime teevan')&count=2&attributes=Ti,Y,CC,AA.AuN,AA.AuId
```
<br>Как правило, выражение будет получено из ответа на метод **interpret**.  Но у вас есть возможность составить выражение запроса самостоятельно (см. [Синтаксис выражения запроса](QueryExpressionSyntax.md)).  
  
Используя параметры *count* и *offset*, можно получить большое количество результатов без отправки одного запроса, что приведет к огромному (и потенциально медленному) ответу.  В этом примере запрос использовал выражение для первой интерпретации из ответа API **interpret** как значения *expr*. Параметр *count=2* указывает, что запрашиваются результаты 2 сущностей. Параметр *attributes=Ti,Y,CC,AA.AuN,AA.AuId* указывает, что для каждого результата запрашивается название, год, показатель цитируемости, имя и идентификатор автора.  Список атрибутов см. в разделе [Атрибуты сущности](EntityAttributes.md).
  
```JSON
{
  "expr": "Composite(AA.AuN=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -15.08,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "CC": 372,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1968481722
        },
        {
          "AuN": "susan t dumais",
          "AuId": 676500258
        },
        {
          "AuN": "eric horvitz",
          "AuId": 1470530979
        }
      ]
    },
    {
      "logprob": -15.389,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "CC": 237,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1982462162
        },
        {
          "AuN": "christine alvarado",
          "AuId": 2163512453
        },
        {
          "AuN": "mark s ackerman",
          "AuId": 2055132526
        },
        {
          "AuN": "david r karger",
          "AuId": 2012534293
        }
      ]
    }
  ]
}
 ```
 
