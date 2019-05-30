---
title: Изменение и обучение приложения с помощью Go
description: Из этого краткого руководства для языка Go вы узнаете, как добавить примеры высказываний в приложение домашней автоматизации и обучить это приложение.
titleSuffix: Language Understanding - Microsoft Cognitive Services
author: diberry
manager: nitinme
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: diberry
ms.openlocfilehash: 973323cd14d9472d9845f0709fc6d2924efc56f5
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357150"
---
# <a name="quickstart-change-model-using-go"></a>Краткое руководство. Изменение модели с помощью Go

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* Установленный язык программирования [Go](https://golang.org/).
* [Introductory Videos](https://code.visualstudio.com) (Вводные видео) 

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Файл JSON с примерами высказываний.

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Создание простого примера кода 

1. Создайте `add-utterances.go` с помощью VS Code. 

2. Добавьте зависимости. 

   [!code-go[Add dependencies.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=2-10 "Add dependencies.")]

3. Добавьте универсальную функцию HTTP-запроса с ключом разработки в заголовке. 

   [!code-go[Add HTTP request function which includes passing authoring key in header.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=12-36 "Add HTTP request function, which includes passing authoring key in header. ")]

4. Добавьте примеры высказываний из файла JSON.

   [!code-go[Add example utterances from JSON file.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=62-76 "Add example utterances from JSON file.")]

5. Выполните запрос на обучение. Запрос использует вспомогательную функцию, чтобы задать VERB для того же маршрута, что и в состоянии обучения. 

   [!code-go[Request training.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=77-86 "Request training. ")]

6. Выполните запрос на состояние обучения. Запрос использует вспомогательную функцию, чтобы задать VERB для того же маршрута, что и в запросе на обучение. 

   [!code-go[Request training status.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=87-90 "Request training status. ")]

7. Добавьте функцию main, чтобы обрабатывать анализ командной строки.

   [!code-go[Add main function to handle command line parsing.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=38-60 "Add main function to handle command-line parsing.")]

## <a name="add-an-utterance-from-the-command-line-train-and-get-status"></a>Добавление фразы из командной строки, обучение и получение состояния

1. В командной строке каталога, в котором вы создали файл Go, введите `go build add-utterances.go`, чтобы скомпилировать этот файл. Командная строка не возвращает никаких данных в случае успешной компиляции.

2. Запустите приложение Go из командной строки, введя следующий текст: 

    ```console
    add-utterances -appID <your-app-id> -authoringKey <add-your-authoring-key> -version <your-version-id> -region westus -utteranceFile utterances.json

    ```

    Замените `<add-your-authoring-key>` значением ключа разработки (начального ключа). Замените `<your-app-id>` значением идентификатора приложения. Замените `<your-version-id>` значением версии. Версия по умолчанию — `0.1`.

    В командной строке отобразятся результаты:

    ```console
    add example utterances requested
    [
        {
            "text": "go lang 1",
            "intentName": "None",
            "entityLabels": []
        }
    ,
        {
            "text": "go lang 2",
            "intentName": "None",
            "entityLabels": []
        }
    ]
    201
    [
        {
            "value": {
                "ExampleId": 77783998,
                "UtteranceText": "go lang 1"
            },
            "hasError": false
        },
        {
            "value": {
                "ExampleId": 77783999,
                "UtteranceText": "go lang 2"
            },
            "hasError": false
        }
    ]
    training selected
    202
    {"statusId":9,"status":"Queued"}
    training status selected
    200
    [
        {
            "modelId": "c52d6509-9261-459e-90bc-b3c872ee4a4b",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "5119cbe8-97a1-4c1f-85e6-6449f3a38d77",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "01e6b6bc-9872-47f9-8a52-da510cddfafe",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "33b409b2-32b0-4b0c-9e91-31c6cfaf93fb",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "1fb210be-2a19-496d-bb72-e0c2dd35cbc1",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "3d098beb-a1aa-423f-a0ae-ce08ced216d6",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "cce854f8-8f8f-4ed9-a7df-44dfea562f62",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "4d97bf0d-5213-4502-9712-2d6e77c96045",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        }
    ]
    ```

    В этом ответе содержится HTTP-код состояния для каждого из трех HTTP-вызовов, а также ответ в формате JSON в тексте ответа. 

## <a name="clean-up-resources"></a>Очистка ресурсов
Когда вы закончите работу с этим руководством, удалите все созданные при работе с ним файлы. 

## <a name="next-steps"></a>Дополнительная информация
> [!div class="nextstepaction"] 
> [Создание приложения с личным доменом](luis-quickstart-intents-only.md) 
