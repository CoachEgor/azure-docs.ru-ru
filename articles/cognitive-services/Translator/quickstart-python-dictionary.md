---
title: Краткое руководство. Поиск слов в двуязычном словаре с помощью Python — API перевода текстов
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как найти другие варианты перевода и использования для выбранного языка с помощью Python и REST API перевода текстов.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: erhopf
ms.openlocfilehash: ef72e7f5a4974a9da96d03dc74bc7a8b01ff4d10
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515083"
---
# <a name="quickstart-look-up-words-with-bilingual-dictionary-using-python"></a>Краткое руководство. Поиск слов в двуязычном словаре с помощью Python

В этом кратком руководстве описано, как найти другие варианты перевода и использования для выбранного языка с помощью Python и REST API перевода текстов.

Для этого краткого руководства требуется [учетная запись Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с ресурсом API перевода текстов. Если у вас нет учетной записи, можно использовать [бесплатную пробную версию](https://azure.microsoft.com/try/cognitive-services/), чтобы получить ключ подписки.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам понадобится:

* Python 2.7.x или 3.x;
* ключ подписки Azure для API перевода текстов.

## <a name="create-a-project-and-import-required-modules"></a>Создание проекта и импорт обязательных модулей

Создайте проект Python в привычной вам интегрированной среде разработки или редакторе или создайте папку на рабочем столе. Затем скопируйте этот фрагмент кода в файл с именем `dictionary-lookup.py` в своем проекте или папке.

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> Если вы ранее не использовали эти модули, вам потребуется установить их перед запуском программы. Чтобы установить эти пакеты, выполните команду: `pip install requests uuid`.

Первый комментарий сообщает вашему интерпретатору Python об использовании кодировки UTF-8. Затем требуемые модули импортируются для считывания ключа подписки из переменной среды, создания запроса HTTP, создания уникального идентификатора и обработки ответа JSON, возвращаемого с помощью API перевода текстов.

## <a name="set-the-subscription-key-base-url-and-path"></a>Задание ключа подписки, базового URL-адреса и пути

В этом примере будет предпринята попытка считать ключ подписки API перевода текстов из переменной среды `TRANSLATOR_TEXT_KEY`. Если вы не знакомы с переменными среды, можно задать `subscriptionKey` в виде строки и закомментировать условный оператор.

Скопируйте в проект следующий код:

```python
# Checks to see if the Translator Text subscription key is available
# as an environment variable. If you are setting your subscription key as a
# string, then comment these lines out.
if 'TRANSLATOR_TEXT_KEY' in os.environ:
    subscriptionKey = os.environ['TRANSLATOR_TEXT_KEY']
else:
    print('Environment variable for TRANSLATOR_TEXT_KEY is not set.')
    exit()
# If you want to set your subscription key as a string, uncomment the line
# below and add your subscription key. Then, be sure to delete your "os" import.
# subscriptionKey = 'put_your_key_here'
```

Конечная точка для перевода текстов задается как `base_url`. Параметр `path` задает маршрут `dictionary/lookup` и определяет, что нужно использовать версию 3 API.

Параметры `params` используются для установки исходных и выходных языков. В этом примере мы используем английский и испанский языки: `en` и `es`.

>[!NOTE]
> Дополнительные сведения о конечных точках, маршрутах и параметрах запросов см. в руководстве [API перевода текстов 3.0: поиск по словарю](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-dictionary-lookup).

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/dictionary/lookup?api-version=3.0'
params = '&from=en&to=es';
constructed_url = base_url + path + params
```

## <a name="add-headers"></a>Добавление заголовков

Самый простой способ выполнить проверку подлинности запроса — это передать ключ подписки как заголовок `Ocp-Apim-Subscription-Key`, который мы используем в этом примере. В качестве альтернативы вы можете обменять свой ключ подписки на маркер доступа и передать маркер доступа в виде заголовка `Authorization` для проверки своего запроса. Дополнительные сведения см. в разделе [Authenticate to the Speech API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) (Аутентификация в API речи).

Скопируйте в проект следующий фрагмент кода:

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

## <a name="create-a-request-to-find-alternate-translations"></a>Создание запроса для поиска альтернативных способов перевода

Определите строку (или строки), которую вы хотите перевести:

```python
# You can pass more than one object in body.
body = [{
    'text': 'Elephants'
}]
```

Далее мы создадим запрос POST, используя модуль `requests`. Требуется три аргумента: сцепленный URL-адрес, заголовки запроса и текст запроса:

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>Вывод ответа

Последний шаг — вывести результаты. Этот фрагмент кода упорядочивает результаты, сортируя ключи, устанавливая отступы и объявляя разделители элементов и ключей.

```python
print(json.dumps(response, sort_keys=True, indent=4, ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>Сборка

Вот и все. Вы собрали простую программу, которая вызовет API перевода текстов и вернет ответ JSON. Теперь пришло время запустить ее.

```console
python alt-translations.py
```

Если вы хотите сравнить свой код с нашей версией, см. [пример кода на сайте GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python).

## <a name="sample-response"></a>Пример ответа

```json
[
    {
        "displaySource": "elephants",
        "normalizedSource": "elephants",
        "translations": [
            {
                "backTranslations": [
                    {
                        "displayText": "elephants",
                        "frequencyCount": 1207,
                        "normalizedText": "elephants",
                        "numExamples": 5
                    }
                ],
                "confidence": 1.0,
                "displayTarget": "elefantes",
                "normalizedTarget": "elefantes",
                "posTag": "NOUN",
                "prefixWord": ""
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы закодировали свой ключ подписки в программе, обязательно удалите его после завершения работы с этим кратким руководством.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Ознакомиться с примерами на Python на сайте GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)

## <a name="see-also"></a>См. также

Узнайте, как с помощью API перевода текстов выполнять следующие задачи:

* [перевод текста](quickstart-python-translate.md);
* [транслитерация текста](quickstart-python-transliterate.md);
* [определение языка по входным данным](quickstart-python-detect.md);
* [получение списка поддерживаемых языков](quickstart-python-languages.md);
* [определение длины предложения на основе входных данных](quickstart-python-sentences.md).
