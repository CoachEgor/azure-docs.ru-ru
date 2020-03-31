---
title: Краткое руководство. Получение намерения с помощью браузера — LUIS
titleSuffix: Azure Cognitive Services
description: Из этого краткого руководства вы узнаете, как использовать общедоступное приложение LUIS для определения намерения пользователя на основе текста разговора в браузере.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 02/03/2020
ms.author: diberry
ms.openlocfilehash: e06bb4c09b3ebab25c0c0ef8ac5c51f6842f34cd
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "76987960"
---
# <a name="quickstart-get-intent-with-a-browser"></a>Краткое руководство. Получение намерения с помощью браузера

Чтобы понять, какие данные возвращает конечная точка прогнозирования LUIS, просмотрите результат прогнозирования в веб-браузере.

## <a name="prerequisites"></a>Предварительные требования

Чтобы запросить общедоступное приложение, вам потребуется перечисленные ниже средства.

* Ваш собственный ключ для Распознавания речи (LUIS) разработки или прогнозирования, который можно получить на портале [LUIS Portal (предварительная версия)](https://preview.luis.ai/). Если у вас еще нет подписки для создания ключа, вы можете зарегистрироваться, чтобы получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Идентификатор общедоступного приложения: `df67dcdb-c37d-46af-88e1-8b97951ca1c2`.

## <a name="use-the-browser-to-see-predictions"></a>Использование браузера для просмотра прогнозирования

1. Откройте веб-браузер.
1. Используйте полные URL-адреса, приведенные ниже, заменив `YOUR-KEY` своим собственным ключом разработки или прогнозирования LUIS. Запросы — это запросы GET, которые включает авторизацию с ключом разработки или прогнозирования LUIS в качестве параметра строки запроса.

    #### <a name="v3-prediction-request"></a>[Запрос на прогнозирование V3](#tab/V3-1-1)


    Формат URL-адреса V3 для запроса **GET** к конечной точке:

    `
    https://westus.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-KEY
    `

    #### <a name="v2-prediction-request"></a>[Запрос на прогнозирование V2](#tab/V2-1-2)

    Формат URL-адреса V2 для запроса **GET** к конечной точке:

    `
    https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=YOUR-KEY&q=turn on all lights
    `

1. Вставьте URL-адрес в окне браузера и нажмите клавишу ВВОД. В браузере отображается результат JSON, который указывает, что LUIS определяет намерение `HomeAutomation.TurnOn` как основное и сущность `HomeAutomation.Operation` со значением `on`.

    #### <a name="v3-prediction-response"></a>[Ответ на прогнозирование V3](#tab/V3-2-1)

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-response"></a>[Ответ на прогнозирование V2](#tab/V2-2-2)

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

    * * *

1. Чтобы увидеть все намерения, добавьте соответствующий параметр строки запроса.

    #### <a name="v3-prediction-endpoint"></a>[Конечная точка прогнозирования V3](#tab/V3-3-1)

    Добавьте `show-all-intents=true` в конец строки запроса, чтобы **показать все намерения**:

    `
    https://westus.api.cognitive.microsoft.com/luis/predict/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-KEY&show-all-intents=true
    `

    ```JSON
    {
        "query": "turn off the living room light",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-endpoint"></a>[Конечная точка прогнозирования V2](#tab/V2)

    Добавьте `verbose=true` в конец строки запроса, чтобы **показать все намерения**:

    `
    https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn on all lights&subscription-key={your-key}&verbose=true
    `

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "intents": [
            {
                "intent": "HomeAutomation.TurnOn",
                "score": 0.5375382
            },
            {
                "intent": "None",
                "score": 0.08687421
            },
            {
                "intent": "HomeAutomation.TurnOff",
                "score": 0.0207554
            }
        ],
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```


<!-- FIX - is the public app getting updated for the new prebuilt domain with entities? -->

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [конечной точке прогнозирования V3](luis-migration-api-v3.md).

> [!div class="nextstepaction"]
> [Создание приложения на портале LUIS](get-started-portal-build-app.md)
