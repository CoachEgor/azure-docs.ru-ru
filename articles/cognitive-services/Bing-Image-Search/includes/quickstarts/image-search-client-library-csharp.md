---
title: Краткое руководство по использованию клиентской библиотеки Поиска изображений Bing (C#)
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/04/2020
ms.author: aahi
ms.openlocfilehash: 9c3bae9d2ad388409c40a8e8c89bcdd52f536cdb
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "85805803"
---
Воспользуйтесь сведениями из краткого руководства и выполните первый поиск изображения с помощью клиентской библиотеки Поиска изображений Bing. Эта библиотека является оболочкой для API и содержит те же функции. Это простое приложение C# отправляет запрос на поиск изображения, анализирует ответ JSON и отображает URL-адрес первого возвращенного изображения.

Исходный код, используемый в данном примере, вместе с дополнительной обработкой ошибок и аннотациями можно получить на [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch).

## <a name="prerequisites"></a>Предварительные требования
* Любой выпуск [Visual Studio 2017 или более поздней версии](https://visualstudio.microsoft.com/vs/whatsnew/).
* [Пакет NuGet для когнитивного поиска изображений](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch/).

Чтобы установить клиентскую библиотеку Поиска изображений Bing в Visual Studio, используйте параметр **Управление пакетами NuGet** в **обозревателе решений** в Visual Studio.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](~/includes/cognitive-services-bing-image-search-signup-requirements.md)]

См. также [Цены на Cognitive Services. API-интерфейсы поиска Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-the-application"></a>Создание и инициализация приложения

Сначала создайте в Visual Studio консольное приложение C#. Добавьте в проект следующие пакеты.

```csharp
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
```

В главном методе проекта необходимо создать переменные для действующего ключа подписки, после чего результаты поиска изображения и поиск термина будут возвращены Bing. Затем с помощью ключа создайте экземпляр клиента для поиска образа.

```csharp
//IMPORTANT: replace this variable with your Cognitive Services subscription key
string subscriptionKey = "ENTER YOUR KEY HERE";
//stores the image results returned by Bing
Images imageResults = null;
// the image search term to be used in the query
string searchTerm = "canadian rockies";

//initialize the client
//NOTE: If you're using version 1.2.0 or below for the Bing Image Search client library, 
// use ImageSearchAPI() instead of ImageSearchClient() to initialize your search client.

var client = new ImageSearchClient(new ApiKeyServiceClientCredentials(subscriptionKey));
```

## <a name="send-a-search-query-using-the-client"></a>Отправка поискового запроса с помощью клиента

Используйте клиент для выполнения поиска с таким текстом запроса:

```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Синтаксический анализ и просмотр первого результата изображений

Выполните синтаксический анализ результатов изображений, возвращенных в ответе.
Если в ответе содержатся результаты поиска, сохраните первый результат и распечатайте такие сведения, как URL-адрес эскиза, исходный URL-адрес и общее количество возвращенных изображений.  

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Руководство по одностраничным приложениям для API Bing для поиска изображений](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>См. также раздел

* [Что такое API Bing для поиска изображений?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Попробуйте API Bing для поиска изображений](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Примеры .NET для пакета SDK в Azure Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Документация по службам Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Справочник по API Bing для поиска изображений](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
