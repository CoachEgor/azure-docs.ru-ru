---
title: Краткое руководство. Поиск изображений с помощью REST API Bing для поиска изображений и C#
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как отправлять запросы в REST API Bing для поиска изображений с помощью C# и получать ответы в формате JSON.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: devx-track-csharp
ms.openlocfilehash: cf129e5059d7abc74a74983ce68f5a8ddc2bf18e
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2020
ms.locfileid: "94593558"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-c"></a>Краткое руководство. Поиск изображений с помощью REST API Bing для поиска изображений и C#

> [!WARNING]
> API Поиска Bing будут перенесены из Cognitive Services в службы Поиска Bing. С **30 октября 2020 г.** подготовку всех новых экземпляров Поиска Bing необходимо будет выполнять в соответствии с процедурой, описанной [здесь](https://aka.ms/cogsvcs/bingmove).
> API-интерфейсы Поиска Bing, подготовленные с помощью Cognitive Services, будут поддерживаться в течение следующих трех лет или до завершения срока действия вашего Соглашения Enterprise (в зависимости от того, какой период окончится раньше).
> Инструкции по миграции см. в статье о [службах Поиска Bing](https://aka.ms/cogsvcs/bingmigration).

Узнайте, как отправлять поисковые запросы к API Поиска изображений Bing. Это приложение на C# отправляет поисковый запрос к API и отображает URL-адрес первого возвращенного в результатах изображения. Хотя это приложение написано на C#, API представляет собой веб-службу RESTful, совместимую с большинством языков программирования.

Исходный код, используемый в данном примере, вместе с дополнительной обработкой ошибок и аннотациями можно получить на [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingImageSearchv7Quickstart.cs).

## <a name="prerequisites"></a>Предварительные требования
* Любой выпуск [Visual Studio 2017 или более поздней версии](https://www.visualstudio.com/downloads/).
* Платформа [Json.NET](https://www.newtonsoft.com/json), доступная в виде пакета NuGet.
* Если вы используете Linux или MacOS, это приложение можно запустить с помощью [Mono](https://www.mono-project.com/).

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Создание и инициализация проекта

1. Создайте консольное решение `BingSearchApisQuickStart` в Visual Studio. Затем добавьте следующие пространства имен в основной файл кода:

    ```csharp
    using System;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    using Newtonsoft.Json.Linq;
    ```

2. Создайте переменные для конечной точки API, ключ подписки и условие поиска. Для `uriBase` можно использовать глобальную конечную точку в следующем коде или конечную точку [личного поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), отображаемую на портале Azure для вашего ресурса.

    ```csharp
    //...
    namespace BingSearchApisQuickstart
    {
        class Program
        {
        // Replace the this string with your valid access key.
        const string subscriptionKey = "enter your key here";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/search";
        const string searchTerm = "tropical ocean";
    //...
    ```

## <a name="create-a-struct-to-format-the-bing-image-search-response"></a>Создание структуры для форматирования ответа API Bing для поиска изображений

Определите структуру `SearchResult`, которая будет содержать результаты поиска изображения и сведения о заголовке JSON.

```csharp
    namespace BingSearchApisQuickstart
    {
        class Program
        {
        //...
            struct SearchResult
            {
                public String jsonResult;
                public Dictionary<String, String> relevantHeaders;
            }
//...
```

## <a name="create-a-method-to-send-search-requests"></a>Создание метода для отправки поисковых запросов

Создайте метод `BingImageSearch` для выполнения вызовов к API и определите структуру `SearchResult`, созданную ранее, в качестве возвращаемого типа.

```csharp
//...
namespace BingSearchApisQuickstart
{
    //...
    class Program
    {
        //...
        static SearchResult BingImageSearch(string searchTerm)
        {
        }
//...
```

## <a name="create-and-handle-an-image-search-request"></a>Создание и обработка запроса на поиск изображения

В методе `BingImageSearch` сделайте следующее.

1. Создайте URI для поискового запроса. Отформатируйте условие поиска `SearchTerm`, прежде чем добавлять его в строку.

    ```csharp
    static SearchResult BingImageSearch(string SearchTerm){

        var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(SearchTerm);
    //...
    ```

2. Отправьте веб-запрос и получите ответ в виде строки JSON.

    ```csharp
    WebRequest request = WebRequest.Create(uriQuery);
    request.Headers["Ocp-Apim-Subscription-Key"] = subscriptionKey;
    HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
    string json = new StreamReader(response.GetResponseStream()).ReadToEnd();
    ```

3. Создайте объект результата поиска и извлеките заголовки HTTP Bing. Затем получите `searchResult`.

    ```csharp
    // Create the result object for return
    var searchResult = new SearchResult()
    {
        jsonResult = json,
        relevantHeaders = new Dictionary<String, String>()
    };

    // Extract Bing HTTP headers
    foreach (String header in response.Headers)
    {
        if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
            searchResult.relevantHeaders[header] = response.Headers[header];
    }
    return searchResult;
    ```

## <a name="process-and-view-the-response"></a>Обработка и просмотр ответа

1. В методе main вызовите `BingImageSearch()` и сохраните возвращенный ответ. Затем десериализируйте JSON в объект.

    ```csharp
    SearchResult result = BingImageSearch(searchTerm);
    //deserialize the JSON response from the Bing Image Search API
    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result.jsonResult);
    ```

2. Получите первое возвращенное изображение из `jsonObj`, выведите заголовок и URL-адрес изображения.
    ```csharp
    var firstJsonObj = jsonObj["value"][0];
    Console.WriteLine("Title for the first image result: " + firstJsonObj["name"]+"\n");
    //After running the application, copy the output URL into a browser to see the image.
    Console.WriteLine("URL for the first image result: " + firstJsonObj["webSearchUrl"]+"\n");
    ```  


## <a name="example-json-response"></a>Пример ответа в формате JSON

Ответы из API Bing для поиска изображений возвращаются в формате JSON. Представленный пример сокращен для отображения только одного результата.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }]
}
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Руководство по одностраничным приложениям для API Bing для поиска изображений](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>См. также раздел

* [Знакомство с API Bing для поиска изображений](../overview.md)  
* [Попробуйте API Bing для поиска изображений](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)
* [Узнайте о ценах на интерфейсы API Поиска Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)
* [Просмотрите документацию по Cognitive Services](../../index.yml)
* [Справочник по API Bing для поиска изображений](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)