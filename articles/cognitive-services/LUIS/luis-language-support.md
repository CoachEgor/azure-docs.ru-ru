---
title: Поддержка языков
titleSuffix: Azure Cognitive Services
description: Служба LUIS имеет широкий набор функций. Не все компоненты имеют одинаковую языковую функциональную совместимость. Убедитесь, что функции, которые вас интересуют, поддерживаются для используемого языка и региональных параметров. Приложение LUIS учитывает конкретный язык и региональные параметры, которые нельзя изменять после установки.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/19/2019
ms.author: diberry
ms.openlocfilehash: 8f067bc005c4de9ddc87ed598b1717f8fbb29a6a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65072381"
---
# <a name="language-and-region-support-for-luis"></a>Поддержка языков и регионов в LUIS

Служба LUIS имеет широкий набор функций. Не все компоненты имеют одинаковую языковую функциональную совместимость. Убедитесь, что функции, которые вас интересуют, поддерживаются для используемого языка и региональных параметров. Приложение LUIS учитывает конкретный язык и региональные параметры, которые нельзя изменять после установки.

## <a name="multi-language-luis-apps"></a>Многоязыковые приложения LUIS

Если вам требуется многоязыковое клиентское приложение LUIS, такое как чат-бот, у вас есть несколько вариантов. Если LUIS поддерживает все языки, можно разработать приложение LUIS для каждого языка. Каждое приложение LUIS получает уникальный идентификатор приложения и журнал конечной точки. Чтобы обеспечить распознавание речи для языка, не поддерживаемого LUIS, можно использовать [API Microsoft Translator](../Translator/translator-info-overview.md) для перевода фрагментов речи на поддерживаемый язык, отправки фрагментов в конечную точку LUIS и получения оценок.

## <a name="languages-supported"></a>Поддерживаемые языки

Служба LUIS распознает фрагменты речи на следующих языках:

| Язык |Языковой стандарт  |  Предварительно созданная предметная область | Предварительно созданная сущность | Фраза список рекомендаций | **[Анализ текста](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)<br>(Тональность и<br>ключевые слова)|
|--|--|:--:|:--:|:--:|:--:|
| Английский (США) |`en-US` | ✔ | ✔  |✔|✔|
| *[Китайский](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Нидерландский |`nl-NL` |-|  -   |-|✔|
| французский (Франция) |`fr-FR` |-| ✔ |✔ |✔|
| французский (Канада) |`fr-CA` |-|   -   |-|✔|
| Немецкий |`de-DE` |-| ✔ |✔ |✔|
| Итальянский |`it-IT` |-| ✔ |✔|✔|
| *[Японский](#japanese-support-notes) |`ja-JP` |-| ✔ |✔|Только ключевая фраза|
| Корейский |`ko-KR` |-|   -   |-|Только ключевая фраза|
| Португальский (Бразилия) |`pt-BR` |-| ✔ |✔ |не все вложенные языки и региональные параметры|
| испанский (Испания) |`es-ES` |-| ✔ |✔|✔|
| Испанский (Мексика)|`es-MX` |-|  -   |✔|✔|
| Турецкий | `tr-TR` |-|-|-|Только тональность|


Языковая поддержка зависит от [готовых сущностей](luis-reference-prebuilt-entities.md) и [готовых предметных областей](luis-reference-prebuilt-domains.md).

### <a name="chinese-support-notes"></a>\* Примечания о поддержке китайского языка

 - В языке и региональных параметрах `zh-cn` служба LUIS ожидает упрощенную кодировку китайского языка вместо традиционной кодировки.
 - Имена намерений, сущностей, свойств и регулярных выражений можно указывать китайскими или латинскими символами.
 - См. в разделе [ссылку предварительно созданных доменов](luis-reference-prebuilt-domains.md) сведения, на котором предварительно созданных доменов поддерживаются в `zh-cn` языка и региональных параметров.
<!--- When writing regular expressions in Chinese, do not insert whitespace between Chinese characters.-->

### <a name="japanese-support-notes"></a>\* Примечания о поддержке японского языка

 - Поскольку служба LUIS не обеспечивает синтаксический анализ и не может понять разницу между почтительной и неформальной речью, необходимо включить различные уровни формальности в качестве примеров обучения для ваших приложений.
     - でございます не равно です.
     - です не равно だ.

### <a name="text-analytics-support-notes"></a>** Примечания о поддержке анализа текста
Текстовая аналитика включает в себя предварительно созданную сущность keyPhrase и анализ тональности. Вложенные языки и региональные параметры поддерживаются только для португальского языка: `pt-PT` и `pt-BR`. Все другие языки и региональные параметры поддерживаются на уровне основного языка. Дополнительные сведения о [поддерживаемых языках](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) анализа текста.

### <a name="speech-api-supported-languages"></a>Поддерживаемые языки API речи
Сведения о языках режима диктовки службы речи см. в разделе [Поддерживаемые языки](https://docs.microsoft.com/azure/cognitive-services/Speech/api-reference-rest/supportedlanguages##interactive-and-dictation-mode) для службы "Речь".

### <a name="bing-spell-check-supported-languages"></a>Поддерживаемые языки проверки орфографии Bing
Список поддерживаемых языков и состояний см. в разделе [Поддерживаемые языки](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) проверки орфографии Bing.

## <a name="rare-or-foreign-words-in-an-application"></a>Редкие или иностранные слова в приложении
В языке и региональных параметрах `en-us` служба LUIS обучается для определения большинства английских слов, включая сленг. В языке и региональных параметрах `zh-cn` служба LUIS обучается для определения большинства символов китайского языка. Если при использовании редких слов в `en-us` или символов в `zh-cn` вы понимаете, что служба LUIS не может распознать это слово или символ, их можно добавить в [список фраз](luis-how-to-add-features.md). Например, слова за пределами языка и региональных параметров приложения, то есть иностранные слова, следует добавить в свойство "Список фраз". Такой список фраз должен быть помечен как невзаимозаменяемый, чтобы указать, что набор слов формирует класс, который LUIS требуется научиться распознавать, но слова не являются синонимами или взаимозаменяемыми.

### <a name="hybrid-languages"></a>Гибридные языки
Гибридные языки объединяют слова из двух культур, например английского и китайского языка. Эти языки не поддерживаются в LUIS, так как приложение основано на одном языке.

## <a name="tokenization"></a>Разметка
В целях машинного обучения LUIS разбивает фрагменты речи на [маркеры](luis-glossary.md#token), в зависимости от языка и региональных параметров.

|Язык|  каждый пробел и специальный символ | уровень символа|составные слова|[возвращаемая размеченная сущность](luis-concept-data-extraction.md#tokenized-entity-returned)
|--|:--:|:--:|:--:|:--:|
|Китайский||✔||✔|
|Нидерландский|||✔|✔|
|Английский (en-us)|✔ ||||
|Французский (fr-FR)|✔||||
|Французский (fr-CA)|✔||||
|Немецкий|||✔|✔|
|Итальянский|✔||||
|Японский||||✔|
|Корейский||✔||✔|
|Португальский (Бразилия)|✔||||
|Испанский (es-ES)|✔||||
|Испанский (es-MX)|✔||||

### <a name="custom-tokenizer-versions"></a>Пользовательские разметчика версий

Следующие языки и региональные параметры имеют настраиваемые разметчика версии:

|Язык и региональные параметры|Version|Назначение|
|--|--|--|
|Немецкий<br>`de-de`|1.0.0|Размечает слова, разделяя их использование на основе обучения разметчика компьютера, который пытается разделить составные слова на их одного компонента.<br>Если пользователь вводит `Ich fahre einen krankenwagen` utterance он окажется `Ich fahre einen kranken wagen`. Позволяя маркировку `kranken` и `wagen` независимо друг от друга как другой сущности.|
|Немецкий<br>`de-de`|1.0.2|Размечает слова, разделяя их на пробелы.<br> Если пользователь вводит `Ich fahre einen krankenwagen` как utterance остается один токен. Таким образом `krankenwagen` помечается как единая сущность. |

### <a name="migrating-between-tokenizer-versions"></a>Миграция между версиями лексический анализатор
<!--
Your first choice is to change the tokenizer version in the app file, then import the version. This action changes how the utterances are tokenized but allows you to keep the same app ID. 

Tokenizer JSON for 1.0.0. Notice the property value for  `tokenizerVersion`. 

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.0",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.0",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 23,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```

Tokenizer JSON for version 1.0.1. Notice the property value for  `tokenizerVersion`. 

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.1",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.1",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 16,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```
-->

Разметки происходит на уровне приложения. Не поддерживается для версии на уровне разметки. 

[Импортировать файл как новое приложение](luis-how-to-start-new-app.md#import-an-app-from-file), вместо версии. Это действие означает, что новое приложение имеет идентификатор другое приложение, но использует разметчика версии, указанной в файле. 