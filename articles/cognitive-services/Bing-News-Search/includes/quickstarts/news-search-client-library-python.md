---
title: Краткое руководство по использованию клиентских библиотек Поиска новостей Bing для Python
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.openlocfilehash: c1bd0d86a3fd9d19d67d84b9b05955421373e01e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "79503512"
---
Используйте это краткое руководство, чтобы начать поиск новостей с помощью клиентской библиотеки Поиска новостей Bing для Python. Поскольку REST API Поиска новостей Bing совместим с большинством языков программирования, клиентская библиотека обеспечивает простой способ интеграции службы в ваши приложения. Исходный код для этого шаблона можно найти на портале [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py).

## <a name="prerequisites"></a>Предварительные требования

* [Python](https://www.python.org/) 2.x или 3.x.

Мы рекомендуем для разработки на Python использовать [виртуальное окружение](https://docs.python.org/3/tutorial/venv.html). Установите и инициализируйте виртуальное окружение с помощью [модуля venv](https://pypi.python.org/pypi/virtualenv). Необходимо установить virtualenv для Python 2.7. Создайте виртуальное окружение:

```console
python -m venv mytestenv
```

Установите зависимости клиентской библиотеки для Поиска новостей Bing:
    
```console
python -m pip install azure-cognitiveservices-search-newssearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Создание и инициализация приложения

1. Создайте проект Python в любой интегрированной среде разработки или редакторе, а затем импортируйте в него следующие библиотеки. Создайте переменные для ключа подписки и термина для поиска.

    ```python
    from azure.cognitiveservices.search.newssearch import NewsSearchClient
    from msrest.authentication import CognitiveServicesCredentials
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    search_term = "Quantum Computing"
    ```

## <a name="initialize-the-client-and-send-a-request"></a>Инициализация клиента и отправка запроса

1. Создайте экземпляр `CognitiveServicesCredentials`.
    
    ```python
    client = NewsSearchClient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

2. Отправьте поисковой запрос к API поиска новостей и сохраните ответ.

    ```python
    news_result = client.news.search(query=search_term, market="en-us", count=10)
    ```

## <a name="parse-the-response"></a>Синтаксический анализ ответа

Получив результаты поиска, выведите первый результат из списка веб-страниц:

```python
if news_result.value:
    first_news_result = news_result.value[0]
    print("Total estimated matches value: {}".format(
        news_result.total_estimated_matches))
    print("News result count: {}".format(len(news_result.value)))
    print("First news name: {}".format(first_news_result.name))
    print("First news url: {}".format(first_news_result.url))
    print("First news description: {}".format(first_news_result.description))
    print("First published time: {}".format(first_news_result.date_published))
    print("First news provider: {}".format(first_news_result.provider[0].name))
else:
    print("Didn't see any news result data..")
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание одностраничного веб-приложения](../../tutorial-bing-news-search-single-page-app.md)
