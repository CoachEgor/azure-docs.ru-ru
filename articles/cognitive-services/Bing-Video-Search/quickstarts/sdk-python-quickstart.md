---
title: Краткое руководство. Поиск видео с помощью пакета SDK для Python — Поиск видео Bing
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как отправлять запросы с помощью пакета SDK Поиска видео Bing для Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: aahi
ms.openlocfilehash: c1afea4e6cacc1f6e9ac84ca20e638836540396e
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75448387"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-python"></a>Краткое руководство. Поиск видео с помощью пакета SDK Поиска видео Bing для Python

Используйте это краткое руководство, чтобы начать поиск новостей с помощью пакета SDK Поиска видео Bing для Python. Пока для Поиска видео Bing имеется REST API, совместимый с большинством языков, пакет SDK предоставляет простой способ интегрировать службу в приложения. Исходный код этого примера можно получить на [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) с дополнительными возможностями и заметками.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="prerequisites"></a>предварительные требования

- [Python](https://www.python.org/) 2.x или 3.x.
- Пакет SDK Поиска видео Bing для Python

Мы советуем использовать [виртуальное окружение](https://docs.python.org/3/tutorial/venv.html) Python. Вы можете установить и инициализировать виртуальное окружение с помощью [модуля venv](https://pypi.python.org/pypi/virtualenv). Установите virtualenv для Python 2.7 с помощью команды:

```console
python -m venv mytestenv
```

Установите пакет SDK Поиска видео Bing с помощью следующей команды:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```

## <a name="create-and-initialize-the-application"></a>Создание и инициализация приложения

1. Создайте файл Python в любой интегрированной среде разработки или редакторе и добавьте следующие инструкции импорта. 

    ```python
    from azure.cognitiveservices.search.videosearch import VideoSearchAPI
    from azure.cognitiveservices.search.videosearch.models import VideoPricing, VideoLength, VideoResolution, VideoInsightModule
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Создайте переменную для ключа подписки. 

    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    ```

## <a name="create-the-search-client"></a>Создание клиента для поиска

Создайте экземпляр `CognitiveServicesCredentials`, а затем создайте экземпляр клиента:

```python
client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))
```

## <a name="send-a-search-request-and-get-a-response"></a>Отправка поискового запроса и получение ответа

1. Используйте `client.videos.search()` в поисковом запросе, чтобы отправить запрос в API Поиска видео Bing и получить ответ.

    ```python
    video_result = client.videos.search(query="SwiftKey")
    ```

2. Если ответ содержит результаты поиска, извлеките первый и выведите его идентификатор, имя и URL-адрес.

    ```python
    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")
    ```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Руководство. Одностраничное приложение для поиска видео](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>См. также раздел 

- [Что такое API Bing для поиска видео?](../overview.md)
- [Примеры пакета SDK для .NET в Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
