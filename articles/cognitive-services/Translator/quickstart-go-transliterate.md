---
title: Краткое руководство. Транслитерация текста, Go — API перевода текстов
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве вы узнаете, как с помощью API перевода текстов и Go преобразовать текст на одном языке из одного набора символов в другой.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 65a873744a55539be220560d8d8d062a4d5adaa7
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445175"
---
# <a name="quickstart-use-the-translator-text-api-to-transliterate-text-using-go"></a>Краткое руководство. Транслитерация текста с помощью Go и API перевода текстов

Из этого краткого руководства вы узнаете, как с помощью API перевода текстов преобразовать текст на одном языке из одного набора символов в другой.

Для этого краткого руководства требуется [учетная запись Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с ресурсом API перевода текстов. Если у вас нет учетной записи, можно использовать [бесплатную пробную версию](https://azure.microsoft.com/try/cognitive-services/), чтобы получить ключ подписки.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам понадобится:

* [GO](https://golang.org/doc/install)
* ключ подписки Azure для API перевода текстов.

## <a name="create-a-project-and-import-required-modules"></a>Создание проекта и импорт обязательных модулей

Создайте проект Go, используя предпочитаемые IDE или текстовый редактор. Затем скопируйте в файл проекта с именем `transliterate-text.go` этот фрагмент кода.

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
     * want to use env variables. If so, be sure to delete the "os" import.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our transliterate function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    transliterate(subscriptionKey)
}
```

## <a name="create-a-function-to-transliterate-text"></a>Создание функции для транслитерации текста

Давайте создадим функцию для транслитерации текста. Эта функция будет принимать один аргумент — ключ подписки API перевода текстов.

```go
func transliterate(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Теперь давайте создадим URL-адрес. URL-адрес создается с помощью методов `Parse()` и `Query()`. При этом параметры добавляются с помощью метода `Add()`. В этом примере японский текст транслитерируется в текст на латинице.

Скопируйте этот код в функцию `transliterate`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0")
q := u.Query()
q.Add("language", "ja")
q.Add("fromScript", "jpan")
q.Add("toScript", "latn")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Дополнительные сведения о конечных точках, маршрутах и параметрах запросов см. в руководстве по [API перевода текстов 3.0: транслитерация](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate).

## <a name="create-a-struct-for-your-request-body"></a>Создание структуры для текста запроса

Далее создайте анонимную структуру для текста запроса и закодируйте его как объект JSON с использованием `json.Marshal()`. Добавьте этот код в функцию `transliterate`.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "こんにちは"},
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

Добавьте следующий код в функцию `transliterate`, чтобы декодировать ответ JSON, а затем отформатировать и вывести результат.

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
go run transliterate-text.go
```

Если вы хотите сравнить свой код с нашей версией, см. [пример кода на сайте GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Пример ответа

```json
[
  {
    "script": "latn",
    "text": "konnichiwa"
  }
]
```

## <a name="next-steps"></a>Дополнительная информация

Сведения о пакетах Go для API-интерфейсов Cognitive Services из [Azure SDK для Go](https://github.com/Azure/azure-sdk-for-go) см. на сайте GitHub.

> [!div class="nextstepaction"]
> [Подробнее о пакетах Go на сайте GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>См. также

Узнайте, как с помощью API перевода текстов выполнять следующие задачи:

* [перевод текста](quickstart-go-translate.md);
* [определение языка по входным данным](quickstart-go-detect.md);
* [получение вариантов перевода](quickstart-go-dictionary.md);
* [получение списка поддерживаемых языков](quickstart-go-languages.md);
* [определение длины предложения на основе входных данных](quickstart-go-sentences.md).
