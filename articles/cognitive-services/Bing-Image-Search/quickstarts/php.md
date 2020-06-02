---
title: Краткое руководство. Поиск изображений с помощью REST API Bing для поиска изображений и PHP
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как отправлять запросы в REST API Bing для поиска изображений с помощью PHP и получать ответы в формате JSON.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 50ce929904df7d4cf4e6ee83d77a9ad16c5f8cff
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83118878"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-php"></a>Краткое руководство. Поиск изображений с помощью REST API Bing для поиска изображений и PHP

Используйте это краткое руководство, чтобы вызвать API Bing для поиска изображений и получить ответ в формате JSON. Простое приложение в этой статье отправляет поисковый запрос и отображает необработанные результаты.

Хотя это приложение написано на PHP, API является веб-службой RESTful, совместимой с любым языком программирования, который может выполнять HTTP-запросы и анализировать JSON.

Исходный код этого примера доступен на [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/php/Search/BingWebSearchv7.php).

## <a name="prerequisites"></a>Предварительные требования

* [PHP 5.6.x или более поздней версии.](https://php.net/downloads.php)

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

См. сведения о [ценах на Cognitive Services (API Поиска Bing)](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-the-application"></a>Создание и инициализация приложения

Чтобы запустить это приложение, сделайте следующее:

1. Убедитесь, что поддержка безопасного HTTP включена в файле `php.ini`. В Windows этот файл находится в каталоге *C:\windows*.
2. Создайте проект PHP в используемой вами интегрированной среде разработки или редакторе.
3. Определите конечную точку API, ключ подписки и условие поиска. Эта конечная точка может быть глобальной конечной точкой в следующем коде или конечной точкой [личного поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), отображаемой на портале Azure для вашего ресурса.

    ```php
    $endpoint = 'https://api.cognitive.microsoft.com/bing/v7.0/images/search';
    // Replace the accessKey string value with your valid access key.
    $accessKey = 'enter key here';
    $term = 'tropical ocean';
    ```

## <a name="construct-and-perform-an-http-request"></a>Создание и выполнение HTTP-запроса

1. Используйте переменные из последнего шага, чтобы подготовить HTTP-запрос к API для поиска изображений.

    ```php
    $headers = "Ocp-Apim-Subscription-Key: $key\r\n";
    $options = array ( 'http' => array (
                            'header' => $headers,
                            'method' => 'GET' ));
    ```

2. Отправьте веб-запрос и получите ответ в формате JSON.

    ```php
    $context = stream_context_create($options);
    $result = file_get_contents($url . "?q=" . urlencode($query), false, $context);
    ```

## <a name="process-and-print-the-json"></a>Обработка и вывод JSON

Обработайте и выведите ответ в формате JSON.

```php
$headers = array();
    foreach ($http_response_header as $k => $v) {
        $h = explode(":", $v, 2);
        if (isset($h[1]))
            if (preg_match("/^BingAPIs-/", $h[0]) || preg_match("/^X-MSEdge-/", $h[0]))
                $headers[trim($h[0])] = trim($h[1]);
    }
    return array($headers, $result);
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

* [Знакомство с API Bing для поиска изображений](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Попробуйте интерактивную демонстрацию в подключенном режиме.](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) 
* [Узнайте о ценах на API Поиска Bing.](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) 
* [Получите бесплатный ключ доступа Cognitive Services.](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)
* [Просмотрите документацию по Cognitive Services.](https://docs.microsoft.com/azure/cognitive-services)
* [Просмотрите справочник по API Поиска изображений Bing.](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
