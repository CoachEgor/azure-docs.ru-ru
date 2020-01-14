---
title: Краткое руководство. Получение аналитических сведений об изображении с помощью пакета SDK для C# — Визуальный поиск Bing
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как отправить изображение с помощью пакета SDK для визуального поиска Bing и получить аналитические сведения об этом изображении.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: aahi
ms.openlocfilehash: b1f5274bcae1f6e59f6dea94beee810a4613d739
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75446607"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-sdk-for-c"></a>Краткое руководство. Получение сведений изображений с помощью пакета SDK для визуального поиска Bing для C#

Ознакомьтесь с этим кратким руководством, чтобы начать получать сведения изображений из службы визуального поиска Bing с помощью пакета SDK для C#. Хотя визуальный поиск Bing имеет API REST, совместимый с большинством языков, пакет SDK предоставляет простой способ интегрировать службу в приложения. Исходный код для этого шаблона можно найти на портале [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVisualSearch).

## <a name="prerequisites"></a>предварительные требования

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Если вы используете Linux или MacOS, это приложение можно запустить с помощью [Mono](https://www.mono-project.com/).
* Пакет NuGet для визуального поиска. 
    - В обозревателе решений в Visual Studio щелкните правой кнопкой мыши проект и выберите `Manage NuGet Packages` в меню. Установите пакет `Microsoft.Azure.CognitiveServices.Search.VisualSearch`. При установке пакетов NuGet также устанавливаются следующие пакеты.
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json.


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

<a name="client"></a>

## <a name="create-and-initialize-the-application"></a>Создание и инициализация приложения

1. В Visual Studio создайте новый проект. Затем добавьте следующие директивы.
    
    ```csharp
    using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
    using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
    ```

2. Создайте экземпляр клиента с помощью ключа подписки.
    
    ```csharp
    var client = new VisualSearchClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```
    
## <a name="send-a-search-request"></a>Отправка поискового запроса 

1. Создайте `FileStream` для ваших изображений (в данном случае `TestImages/image.jpg`). Затем используйте клиент для отправки поискового запроса с помощью`client.Images.VisualSearchMethodAsync()`. 
    
    ```csharp
     System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open);
     // The knowledgeRequest parameter is not required if an image binary is passed in the request body
     var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
    ```
    
2. Проанализируйте результаты предыдущего запроса.

    ```csharp
    // Visual Search results
    if (visualSearchResults.Image?.ImageInsightsToken != null)
    {
        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
    }
    else
    {
        Console.WriteLine("Couldn't find image insights token!");
    }
    
    // List of tags
    if (visualSearchResults.Tags.Count > 0)
    {
        var firstTagResult = visualSearchResults.Tags[0];
        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");
    
        // List of actions in first tag
        if (firstTagResult.Actions.Count > 0)
        {
            var firstActionResult = firstTagResult.Actions[0];
            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
        }
        else
        {
            Console.WriteLine("Couldn't find tag actions!");
        }
    }
    ```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Руководство по одностраничным веб-приложениям для наглядного поиска](tutorial-bing-visual-search-single-page-app.md)
