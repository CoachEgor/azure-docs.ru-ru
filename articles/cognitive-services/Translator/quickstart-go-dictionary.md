---
title: Краткое руководство. Поиск слов в двуязычном словаре с помощью Go — API перевода текстов
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве вы узнаете, как искать варианты перевода и контекстуальные термины, используя API перевода текстов и Go.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: bc92fb078c46c877f3b99389b1d14ad0f93e63b3
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445211"
---
# <a name="quickstart-look-up-words-with-bilingual-dictionary-using-go"></a>Краткое руководство. Поиск слов в двуязычном словаре с помощью Go

В этом кратком руководстве описано, как найти другие варианты перевода и использования для выбранного языка с помощью Go и REST API перевода текстов.

Для этого краткого руководства требуется [учетная запись Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с ресурсом API перевода текстов. Если у вас нет учетной записи, можно использовать [бесплатную пробную версию](https://azure.microsoft.com/try/cognitive-services/), чтобы получить ключ подписки.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам понадобится:

* [GO](https://golang.org/doc/install)
* ключ подписки Azure для API перевода текстов.

## <a name="create-a-project-and-import-required-modules"></a>Создание проекта и импорт обязательных модулей

Создайте проект Go в привычной вам интегрированной среде разработки или редакторе или создайте папку на рабочем столе. Затем скопируйте этот фрагмент кода в файл с именем `alt-translations.go` в своем проекте или папке.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>Создание функции main

В этом примере будет предпринята попытка считать ключ подписки API перевода текстов из переменной среды `TRANSLATOR_TEXT_KEY`. Если вы не знакомы с переменными среды, можно задать `subscriptionKey` в виде строки и закомментировать условный оператор.

Скопируйте в проект следующий код:

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables. Then, be sure to delete the "os" import.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our altTranslations function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    altTranslations(subscriptionKey)
}
```

## <a name="create-a-function-to-get-alternate-translations"></a>Создание функции для поиска вариантов перевода

Давайте создадим функцию для поиска вариантов перевода. Эта функция будет принимать один аргумент — ключ подписки API перевода текстов.

```go
func altTranslations(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Теперь давайте создадим URL-адрес. URL-адрес создается с помощью методов `Parse()` и `Query()`. При этом параметры добавляются с помощью метода `Add()`. В этом примере мы переводим с английского на испанский.

Скопируйте этот код в функцию `altTranslations`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0")
q := u.Query()
q.Add("from", "en")
q.Add("to", "es")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Дополнительные сведения о конечных точках, маршрутах и параметрах запросов см. в руководстве по [API перевода текстов 3.0: Поиск по словарю](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-dictionary-lookup).

## <a name="create-a-struct-for-your-request-body"></a>Создание структуры для текста запроса

Далее создайте анонимную структуру для текста запроса и закодируйте его как объект JSON с использованием `json.Marshal()`. Добавьте этот код в функцию `altTranslations`.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "Pineapples"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>Создание запроса

Теперь, когда вы закодировали текст запроса в формате JSON, можно создать запрос POST и вызвать API перевода текстов.

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers to the request
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

Если вы используете подписку на несколько служб Cognitive Services, необходимо также включить `Ocp-Apim-Subscription-Region` в параметрах запроса. [Дополнительные сведения об аутентификации с использованием подписки на несколько служб](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication). 

## <a name="handle-and-print-the-response"></a>Обработка и вывод ответа

Добавьте следующий код в функцию `altTranslations`, чтобы декодировать ответ JSON, а затем отформатировать и вывести результат.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Сборка

Вот и все. Вы собрали простую программу, которая вызовет API перевода текстов и вернет ответ JSON. Теперь пришло время запустить ее.

```console
go run alt-translations.go
```

Если вы хотите сравнить свой код с нашей версией, см. [пример кода на сайте GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Пример ответа

```json
[
  {
    "displaySource": "pineapples",
    "normalizedSource": "pineapples",
    "translations": [
      {
        "backTranslations": [
          {
            "displayText": "pineapples",
            "frequencyCount": 158,
            "normalizedText": "pineapples",
            "numExamples": 5
          },
          {
            "displayText": "cones",
            "frequencyCount": 13,
            "normalizedText": "cones",
            "numExamples": 5
          },
          {
            "displayText": "piña",
            "frequencyCount": 5,
            "normalizedText": "piña",
            "numExamples": 3
          },
          {
            "displayText": "ganks",
            "frequencyCount": 3,
            "normalizedText": "ganks",
            "numExamples": 2
          }
        ],
        "confidence": 0.7016,
        "displayTarget": "piñas",
        "normalizedTarget": "piñas",
        "posTag": "NOUN",
        "prefixWord": ""
      },
      {
        "backTranslations": [
          {
            "displayText": "pineapples",
            "frequencyCount": 16,
            "normalizedText": "pineapples",
            "numExamples": 2
          }
        ],
        "confidence": 0.2984,
        "displayTarget": "ananás",
        "normalizedTarget": "ananás",
        "posTag": "NOUN",
        "prefixWord": ""
      }
    ]
  }
]
```

## <a name="next-steps"></a>Дополнительная информация

Изучите примеры на языке Go для API-интерфейсов Cognitive Services из [Azure SDK для Go](https://github.com/Azure/azure-sdk-for-go) на сайте GitHub.

> [!div class="nextstepaction"]
> [Примеры на Go на сайте GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>См. также

Узнайте, как с помощью API перевода текстов выполнять следующие задачи:

* [перевод текста](quickstart-go-translate.md);
* [транслитерация текста](quickstart-go-transliterate.md);
* [определение языка по входным данным](quickstart-go-detect.md);
* [получение списка поддерживаемых языков](quickstart-go-languages.md);
* [определение длины предложения на основе входных данных](quickstart-go-sentences.md).
