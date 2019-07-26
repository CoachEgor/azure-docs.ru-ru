---
title: Краткое руководство. Поиск изображений с помощью пакета SDK для поиска изображений Bing для Node.js
titleSuffix: Azure Cognitive Services
description: Воспользуйтесь сведениями из быстрого начала и выполните первый поиск изображения с помощью пакета SDK Bing для поиска изображений. Данный пакет является оболочкой для API и содержит те же функции. Это простое приложение Node.js отправляет запрос на поиск изображения, анализирует ответ JSON и отображает URL-адрес первого возвращенного изображения.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 24f6de759f233b82cb19a21401d3886a80bd746f
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423892"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-for-nodejs"></a>Краткое руководство. Поиск изображений с помощью пакета SDK для поиска изображений Bing для Node.js

Воспользуйтесь сведениями из быстрого начала и выполните первый поиск изображения с помощью пакета SDK Bing для поиска изображений. Данный пакет является оболочкой для API и содержит те же функции. Это простое приложение JavaScript отправляет запрос на поиск изображения, анализирует ответ JSON и отображает URL-адрес первого возвращенного изображения.

Исходный код, используемый в данном примере, вместе с дополнительной обработкой ошибок и аннотациями можно получить на [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js).

## <a name="prerequisites"></a>Предварительные требования
Получите [ключ доступа Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) с помощью **поиска**.  См. также [Цены на Cognitive Services. API-интерфейсы поиска Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).  См. также [Цены на Cognitive Services. API-интерфейсы поиска Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

* [Пакет SDK по поиску изображений Cognitive Services для Node.js](https://www.npmjs.com/package/azure-cognitiveservices-imagesearch)
    * Установка с помощью `npm install azure-cognitiveservices-imagesearch`
* Модуль [Node.js Azure Rest](https://www.npmjs.com/package/ms-rest-azure)
    * Установка с помощью `npm install ms-rest-azure`

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Создание и инициализация приложения

1. Создайте новый файл JavaScript в избранной IDE или редакторе и установите строгость, HTTPS и другие требования.

    ```javascript
    'use strict';
    const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    ```

2. В главном методе проекта необходимо создать переменные для действующего ключа подписки, после чего результаты поиска изображения и поиск термина будут возвращены Bing. Затем с помощью ключа создайте экземпляр клиента для поиска образа.

    ```javascript
    //replace this value with your valid subscription key.
    let serviceKey = "ENTER YOUR KEY HERE";

    //the search term for the request
    let searchTerm = "canadian rockies";

    //instantiate the image search client 
    let credentials = new CognitiveServicesCredentials(serviceKey);
    let imageSearchApiClient = new ImageSearchAPIClient(credentials);

    ```

## <a name="create-an-asynchronous-helper-function"></a>Создание асинхронной вспомогательной функции

1. Создайте функцию которая будет асинхронно вызывать клиента и возвращать ответ из службы поиска изображений Bing.  
    ```javascript
    //a helper function to perform an async call to the Bing Image Search API
    const sendQuery = async () => {
        return await imageSearchApiClient.imagesOperations.search(searchTerm);
    };
    ```
   ## <a name="send-a-query-and-handle-the-response"></a>Отправка запроса и обработка ответа

1. Чтобы проанализировать результаты поиска изображения, полученные в ответе, вызовите вспомогательную функцию и обработайте ее `promise`.

    Если в ответе содержатся результаты поиска, сохраните первый результат и распечатайте такие сведения, как URL-адрес эскиза, исходный URL-адрес и общее количество возвращенных изображений.  
    ```javascript
    sendQuery().then(imageResults => {
        if (imageResults == null) {
        console.log("No image results were found.");
        }
        else {
            console.log(`Total number of images returned: ${imageResults.value.length}`);
            let firstImageResult = imageResults.value[0];
            //display the details for the first image result. After running the application,
            //you can copy the resulting URLs from the console into your browser to view the image.
            console.log(`Total number of images found: ${imageResults.value.length}`);
            console.log(`Copy these URLs to view the first image returned:`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image content url: ${firstImageResult.contentUrl}`);
        }
      })
      .catch(err => console.error(err))
    ```

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Руководство по одностраничным приложениям для API Bing для поиска изображений](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>См. также

* [Что такое API Bing для поиска изображений?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Попробуйте API Bing для поиска изображений](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Get a free Cognitive Services access key](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api) (Получение бесплатного ключа доступа Cognitive Services)
* [Node.js samples for the Azure Cognitive Services SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) (Примеры Node.js для пакета SDK в Azure Cognitive Services)
* [Документация по службам Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Справочник по API Bing для поиска изображений](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
