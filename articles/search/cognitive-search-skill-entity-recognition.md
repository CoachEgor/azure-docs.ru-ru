---
title: Когнитивный навык распознавания сущностей
titleSuffix: Azure Cognitive Search
description: Извлекайте различные типы сущностей из текста в конвейере обогащения в Azure Cognitive Search.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 6ef5952b6413563b2c2e16ff2218f709b414fb84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297814"
---
#    <a name="entity-recognition-cognitive-skill"></a>Когнитивный навык распознавания сущностей

Навык **распознавания сущностей** извлекает сущности различных типов из текста. Этот навык использует модели машинного обучения, предоставляемые функцией [Анализ текста](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) в Cognitive Services.

> [!NOTE]
> По мере расширения сферы охвата за счет увеличения частоты обработки, добавления дополнительных документов или добавления большего числа алгоритмов ИИ необходимо [прикрепить ресурс Cognitive Services.](cognitive-search-attach-cognitive-services.md) Плата взимается при вызове API в Cognitive Services и извлечении изображений при распознавании документов в службе "Когнитивный поиск Azure". За извлечение текста из документов плата не взимается.
>
> Плата за выполнение встроенных навыков взимается в рамках существующей [модели оплаты Cognitive Services по мере использования](https://azure.microsoft.com/pricing/details/cognitive-services/). Плата за извлечение изображений указана на [странице с ценами на службу "Когнитивный поиск Azure"](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.EntityRecognitionSkill

## <a name="data-limits"></a>Ограничения данных
Максимальный размер записи должен соразмеровать 50 000 [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)символов. Если вам нужно разбить данные перед отправкой для извлечения ключевой фразы, можно воспользоваться [навыком разделения текста](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Параметры навыков

Все параметры чувствительны к регистру и являются необязательными.

| Имя параметра     | Описание |
|--------------------|-------------|
| Категории    | Массив категорий, который следует извлекать.  Возможные типы категорий: `"Person"`, `"Location"`, `"Organization"`, `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"`. Если категория не указана, возвращаются все типы.|
|defaultLanguageCode |    Код языка вводимого текста. Поддерживаются следующие `ar, cs, da, de, en, es, fi, fr, hu, it, ja, ko, nl, no, pl, pt-BR, pt-PT, ru, sv, tr, zh-hans`языки: . Не все категории сущностей поддерживаются для всех языков; см. примечание ниже.|
|minimumPrecision | Значение от 0 до 1. Если оценка достоверности `namedEntities` (на выходе) ниже этого значения, объект не возвращается. Значение по умолчанию равно 0. |
|includeTypelessEntities | Установите, `true` если вы хотите распознать известные объекты, которые не соответствуют текущим категориям. Признанные сущности `entities` возвращаются в сложном поле вывода. Например, "Windows 10" является хорошо известным объектом (продуктом), но так как "Продукты" не являются поддерживаемой категорией, эта сущность будет включена в поле вывода сущностей. Значение по умолчанию — `false`. |


## <a name="skill-inputs"></a>Входные данные навыков

| Ввод имени      | Описание                   |
|---------------|-------------------------------|
| languageCode    | Необязательный параметр. Значение по умолчанию — `"en"`.  |
| text          | Текст для анализа.          |

## <a name="skill-outputs"></a>Выходные данные навыка

> [!NOTE]
> Для некоторых языков категории сущностей не поддерживаются. Типы `"Person"`категорий `"Organization"` сущности поддерживаются для полного списка языков, приведенных выше. `"Location"` Только _де_, _en_, _es_, _fr_, `"Datetime"`и `"URL"` _zh-hans_ поддержки извлечения `"Quantity"`, , и `"Email"` типов. Для получения дополнительной информации [см.](https://docs.microsoft.com/azure/cognitive-services/text-analytics/language-support)  

| Имя вывода      | Описание                   |
|---------------|-------------------------------|
| Люди       | Массив строк, где каждая строка представляет имя человека. |
| Расположения  | Массив строк, где каждая строка представляет расположение. |
| organizations  | Массив строк, где каждая строка представляет организацию. |
| quantities  | Массив строк, где каждая строка представляет количество. |
| dateTimes  | Массив строк, где каждая строка представляет собой значение даты и времени (как оно отображается в тексте). |
| urls | Массив строк, где каждая строка представляет собой URL-адрес. |
| emails | Массив строк, где каждая строка представляет собой адрес электронной почты. |
| namedEntities | Массив сложных типов, содержащий следующие поля: <ul><li>категория</li> <li>значение (фактическое имя сущности)</li><li>смещение (расположение, где оно было найдено в тексте);</li><li>уверенность (более высокая стоимость означает, что это больше, чтобы быть реальной сущности)</li></ul> |
| Сущности | Массив сложных типов, содержащий подробные сведения о сущностях, извлеченных из текста, со следующими полями. <ul><li> name (Фактическое имя сущности. Представляет собой "нормализованную" форму);</li><li> wikipediaId;</li><li>wikipediaLanguage;</li><li>wikipediaUrl (ссылка на страницу Википедии для сущности);</li><li>bingId</li><li>type (категория распознанной сущности);</li><li>subType (доступно только для определенных категорий, что дает дополнительное представление о типе сущности);</li><li> matches (имеющаяся составная коллекция);<ul><li>text (необработанный текст для сущности);</li><li>offset (расположение, где было найдено смещение);</li><li>length (длина необработанного текста сущности).</li></ul></li></ul> |

##    <a name="sample-definition"></a>Пример определения

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en",
    "includeTypelessEntities": true,
    "minimumPrecision": 0.5,
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      },
      {
        "name": "emails",
        "targetName": "contact"
      },
      {
        "name": "entities"
      }
    ]
  }
```
##    <a name="sample-input"></a>Пример ввода

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Contoso corporation was founded by John Smith. They can be reached at contact@contoso.com",
             "languageCode": "en"
           }
      }
    ]
}
```

##    <a name="sample-output"></a>Пример выходных данных

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "persons": [ "John Smith"],
        "emails":["contact@contoso.com"],
        "namedEntities": 
        [
          {
            "category":"Person",
            "value": "John Smith",
            "offset": 35,
            "confidence": 0.98
          }
        ],
        "entities":  
        [
          {
            "name":"John Smith",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Person",
            "subType": null,
            "matches": [{
                "text": "John Smith",
                "offset": 35,
                "length": 10
            }]
          },
          {
            "name": "contact@contoso.com",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Email",
            "subType": null,
            "matches": [
            {
                "text": "contact@contoso.com",
                "offset": 70,
                "length": 19
            }]
          },
          {
            "name": "Contoso",
            "wikipediaId": "Contoso",
            "wikipediaLanguage": "en",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/Contoso",
            "bingId": "349f014e-7a37-e619-0374-787ebb288113",
            "type": null,
            "subType": null,
            "matches": [
            {
                "text": "Contoso",
                "offset": 0,
                "length": 7
            }]
          }
        ]
      }
    }
  ]
}
```

Обратите внимание, что смещения, возвращенные для сущностей в выходе этого навыка, непосредственно возвращаются из [API Text Analytics,](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)что означает, что если вы используете их для индексирования в исходную строку, вы должны использовать класс [StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) в .NET для извлечения правильного содержимого.  [Более подробную информацию можно найти здесь.](https://docs.microsoft.com/azure/cognitive-services/text-analytics/concepts/text-offsets)

## <a name="error-cases"></a>Варианты ошибок
Если код языка для документа не поддерживается, возвращается ошибка и сущности не извлекаются.

## <a name="see-also"></a>См. также

+ [Встроенные навыки](cognitive-search-predefined-skills.md)
+ [Определение набора навыков](cognitive-search-defining-skillset.md)
