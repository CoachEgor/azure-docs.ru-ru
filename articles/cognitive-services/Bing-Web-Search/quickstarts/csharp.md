---
title: Краткое руководство. Поиск в Интернете с помощью C# — REST API Bing для поиска в Интернете
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как отправлять запросы в REST API Bing для поиска в Интернете с помощью C# и получать ответы в формате JSON.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 10/19/2020
ms.author: aahi
ms.custom: seodec2018, devx-track-csharp
ms.openlocfilehash: 201fcf80e26aaaed78e1f6a78eb0dab5a345ba50
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2020
ms.locfileid: "96350608"
---
# <a name="quickstart-search-the-web-using-the-bing-web-search-rest-api-and-c"></a>Краткое руководство. Поиск в Интернете с помощью REST API Bing для поиска в Интернете и C#

> [!WARNING]
> API Поиска Bing будут перенесены из Cognitive Services в службы Поиска Bing. С **30 октября 2020 г.** подготовку всех новых экземпляров Поиска Bing необходимо будет выполнять в соответствии с процедурой, описанной [здесь](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> API-интерфейсы Поиска Bing, подготовленные с помощью Cognitive Services, будут поддерживаться в течение следующих трех лет или до завершения срока действия вашего Соглашения Enterprise (в зависимости от того, какой период окончится раньше).
> Инструкции по миграции см. в статье о [службах Поиска Bing](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Используйте это краткое руководство, чтобы вызвать API Поиска в Интернете Bing. Это приложение C# отправляет поисковый запрос к API и показывает ответ JSON. Хотя это приложение написано на C#, API представляет собой веб-службу RESTful, совместимую с большинством языков программирования.

В этом примере программы в этом кратком руководстве используются только классы .NET Core.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам потребуется следующее:

* Windows: [Visual Studio 2017 или более поздней версии](https://www.visualstudio.com/downloads/)
* Linux или MacOS: [Visual Studio Code](https://code.visualstudio.com/) и [.NET Core](https://dotnet.microsoft.com/download).
* [Бесплатная подписка Azure.](https://azure.microsoft.com/free/dotnet)

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="create-a-project-and-declare-dependencies"></a>Создание проекта и объявление зависимостей

Создайте консольный проект в Visual Studio или Visual Studio Code. Используйте следующий код, чтобы импортировать необходимые пространства имен и типы:

```csharp
using System;
using System.Text;
using System.Net;
using System.IO;
using System.Collections.Generic;
```

## <a name="declare-a-namespace-and-class-for-your-program"></a>Объявление пространства имен и класса для программы

При работе с этим кратким руководством большую часть кода мы поместим в класс `Program`. Для начала создайте в проекте пространство имен `BingSearchApiQuickstart` и класс `Program`.  

```csharp
namespace BingSearchApisQuickstart
{
    class Program
    {
        // The code in the following sections goes here.
    }
}
```

## <a name="define-variables"></a>Определение переменных

Прежде чем продолжить, необходимо задать несколько переменных. Добавьте этот код в класс `Program`, созданный в предыдущем разделе: 

1. Для значения `uriBase` вы можете использовать глобальную конечную точку, указанную в коде ниже, или конечную точку [личного поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), которая отображается на портале Azure для вашего ресурса. 

2. Убедитесь, что значение `uriBase` указано правильно, и замените значение `accessKey` ключом подписки из своей учетной записи Azure. 

3. Кроме того, вы можете настроить поисковый запрос, заменив значение параметра `searchTerm`. 

```csharp
// Enter a valid subscription key.
const string accessKey = "enter key here";
/*
 * If you encounter unexpected authorization errors, double-check this value
 * against the endpoint for your Bing Web search instance in your Azure
 * dashboard.
 */
const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/search";
const string searchTerm = "Microsoft Cognitive Services";
```

## <a name="declare-the-main-method"></a>Объявление метода main

Метод `Main()` является обязательным. Именно он всегда вызывается первым при запуске программы. В нашем приложении метод main проверяет `accessKey`, выполняет запрос и выводит ответ.

Метод `main()` зависит от методов, созданных в следующих разделах.

```csharp
static void Main()
{
    Console.OutputEncoding = System.Text.Encoding.UTF8;
    if (accessKey.Length == 32)
    {
        Console.WriteLine("Searching the Web for: " + searchTerm);
        SearchResult result = BingWebSearch(searchTerm);
        Console.WriteLine("\nRelevant HTTP Headers:\n");
        foreach (var header in result.relevantHeaders)
            Console.WriteLine(header.Key + ": " + header.Value);

        Console.WriteLine("\nJSON Response:\n");
        Console.WriteLine(JsonPrettyPrint(result.jsonResult));
    }
    else
    {
        Console.WriteLine("Invalid Bing Search API subscription key!");
        Console.WriteLine("Please paste yours into the source code.");
    }
    Console.Write("\nPress Enter to exit ");
    Console.ReadLine();
}
```

## <a name="create-a-struct-for-search-results"></a>Создание структуры для результатов поиска

Создайте структуру, которая возвращает результаты поиска с соответствующими заголовками. Она вызывается при выполнении запроса к API поиска в Интернете Bing, чтобы создать объект результата.

```csharp
// Returns search results with headers.
struct SearchResult
{
    public String jsonResult;
    public Dictionary<String, String> relevantHeaders;
}
```

## <a name="construct-a-request"></a>Создание запроса

Используйте этот код, чтобы создать запрос поиска, выполнить запрос GET и обработать ответ.

```csharp
/// <summary>
/// Makes a request to the Bing Web Search API and returns data as a SearchResult.
/// </summary>
static SearchResult BingWebSearch(string searchQuery)
{
    // Construct the search request URI.
    var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery);

    // Perform request and get a response.
    WebRequest request = HttpWebRequest.Create(uriQuery);
    request.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
    HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
    string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

    // Create a result object.
    var searchResult = new SearchResult()
    {
        jsonResult = json,
        relevantHeaders = new Dictionary<String, String>()
    };

    // Extract Bing HTTP headers.
    foreach (String header in response.Headers)
    {
        if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
            searchResult.relevantHeaders[header] = response.Headers[header];
    }
    return searchResult;
}
```

## <a name="format-the-response"></a>Форматирование ответа

Этот метод форматирует ответ JSON, например расставляет отступы и добавляет разрывы строк.

```csharp
/// <summary>
/// Formats the JSON string by adding line breaks and indents.
/// </summary>
/// <param name="json">The raw JSON string.</param>
/// <returns>The formatted JSON string.</returns>
static string JsonPrettyPrint(string json)
{
    if (string.IsNullOrEmpty(json))
        return string.Empty;

    json = json.Replace(Environment.NewLine, "").Replace("\t", "");

    StringBuilder sb = new StringBuilder();
    bool quote = false;
    bool ignore = false;
    char last = ' ';
    int offset = 0;
    int indentLength = 2;

    foreach (char ch in json)
    {
        switch (ch)
        {
            case '"':
                if (!ignore) quote = !quote;
                break;
            case '\\':
                if (quote && last != '\\') ignore = true;
                break;
        }

        if (quote)
        {
            sb.Append(ch);
            if (last == '\\' && ignore) ignore = false;
        }
        else
        {
            switch (ch)
            {
                case '{':
                    case '[':
                        sb.Append(ch);
                        sb.Append(Environment.NewLine);
                        sb.Append(new string(' ', ++offset * indentLength));
                        break;
                    case ']':
                    case '}':
                        sb.Append(Environment.NewLine);
                        sb.Append(new string(' ', --offset * indentLength));
                        sb.Append(ch);
                        break;
                    case ',':
                        sb.Append(ch);
                        sb.Append(Environment.NewLine);
                        sb.Append(new string(' ', offset * indentLength));
                        break;
                    case ':':
                        sb.Append(ch);
                        sb.Append(' ');
                        break;
                    default:
                        if (quote || ch != ' ') sb.Append(ch);
                        break;
            }
        }
        last = ch;
    }
    return sb.ToString().Trim();
}
```

## <a name="put-it-all-together"></a>Сборка

А теперь выполните созданный код. Если вы хотите сравнить свой код с нашей версией, см. [пример кода на сайте GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingWebSearchv7.cs).

## <a name="example-json-response"></a>Пример ответа в формате JSON

Ответы из API Bing для поиска в Интернете возвращаются в формате JSON. Представленный пример сокращен для отображения только одного результата.  

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Microsoft Cognitive Services"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q=Microsoft+cognitive+services",
    "totalEstimatedMatches": 22300000,
    "value": [
      {
        "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0",
        "name": "Microsoft Cognitive Services",
        "url": "https://www.microsoft.com/cognitive-services",
        "displayUrl": "https://www.microsoft.com/cognitive-services",
        "snippet": "Knock down barriers between you and your ideas. Enable natural and contextual interaction with tools that augment users' experiences via the power of machine-based AI. Plug them in and bring your ideas to life.",
        "deepLinks": [
          {
            "name": "Face",
            "url": "https://azure.microsoft.com/services/cognitive-services/face/",
            "snippet": "Add facial recognition to your applications to detect, identify, and verify faces using the Face service from Microsoft Azure. ... Cognitive Services; Face service;"
          },
          {
            "name": "Text Analytics",
            "url": "https://azure.microsoft.com/services/cognitive-services/text-analytics/",
            "snippet": "Cognitive Services; Text Analytics API; Text Analytics API . Detect sentiment, ... you agree that Microsoft may store it and use it to improve Microsoft services, ..."
          },
          {
            "name": "Computer Vision API",
            "url": "https://azure.microsoft.com/services/cognitive-services/computer-vision/",
            "snippet": "Extract the data you need from images using optical character recognition and image analytics with Computer Vision APIs from Microsoft Azure."
          },
          {
            "name": "Emotion",
            "url": "https://www.microsoft.com/cognitive-services/en-us/emotion-api",
            "snippet": "Cognitive Services Emotion API - microsoft.com"
          },
          {
            "name": "Bing Speech API",
            "url": "https://azure.microsoft.com/services/cognitive-services/speech/",
            "snippet": "Add speech recognition to your applications, including text to speech, with a speech API from Microsoft Azure. ... Cognitive Services; Bing Speech API;"
          },
          {
            "name": "Get Started for Free",
            "url": "https://azure.microsoft.com/services/cognitive-services/",
            "snippet": "Add vision, speech, language, and knowledge capabilities to your applications using intelligence APIs and SDKs from Cognitive Services."
          }
        ]
      }
    ]
  },
  "relatedSearches": {
    "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches",
    "value": [
      {
        "text": "microsoft bot framework",
        "displayText": "microsoft bot framework",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+bot+framework"
      },
      {
        "text": "microsoft cognitive services youtube",
        "displayText": "microsoft cognitive services youtube",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+youtube"
      },
      {
        "text": "microsoft cognitive services search api",
        "displayText": "microsoft cognitive services search api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+search+api"
      },
      {
        "text": "microsoft cognitive services news",
        "displayText": "microsoft cognitive services news",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+news"
      },
      {
        "text": "ms cognitive service",
        "displayText": "ms cognitive service",
        "webSearchUrl": "https://www.bing.com/search?q=ms+cognitive+service"
      },
      {
        "text": "microsoft cognitive services text analytics",
        "displayText": "microsoft cognitive services text analytics",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+text+analytics"
      },
      {
        "text": "microsoft cognitive services toolkit",
        "displayText": "microsoft cognitive services toolkit",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+toolkit"
      },
      {
        "text": "microsoft cognitive services api",
        "displayText": "microsoft cognitive services api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+api"
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0"
          }
        }
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "RelatedSearches",
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches"
          }
        }
      ]
    }
  }
}
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Руководство по одностраничным приложениям для API Поиска в Интернете Bing](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]