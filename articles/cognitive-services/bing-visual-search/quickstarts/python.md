---
title: Краткое руководство. Получение аналитических сведений об изображении с помощью REST API и Python — Визуальный поиск Bing
titleSuffix: Azure Cognitive Services
description: Узнайте, как отправить изображение с помощью API Визуального поиска Bing и Python, а затем получить аналитические сведения об этом изображении.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.author: scottwhi
ms.custom: devx-track-python
ms.openlocfilehash: ef265ab0cff9514695b40995e842518803e2e4c0
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91324592"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Краткое руководство. Получение аналитических сведений об изображении с помощью REST API и Python Визуального поиска Bing

Используйте это краткое руководство, чтобы выполнить вызов к API Визуального поиска Bing. Это приложение Python отправляет изображение в API и отображает возвращенные данные о нем. Это приложение создано на языке Python. Но API представляет собой веб-службу на основе REST, совместимую с большинством языков программирования.

## <a name="prerequisites"></a>Предварительные требования

* [Python 3.x](https://www.python.org/)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Инициализация приложения

1. Создайте файл Python в избранной интегрированной среде разработки или редакторе и добавьте следующую инструкцию `import`:

    ```python
    import requests, json
    ```

2. Создайте переменные для ключа подписки, конечной точки и путь к отправляемому изображению. Для значения `BASE_URI` можно использовать глобальную конечную точку в следующем коде или конечную точку [личного поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), отображаемую на портале Azure для вашего ресурса.

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```
    
3. При отправке локального изображения данные формы должны содержать заголовок `Content-Disposition`. Задайте для его параметра `name` значение "image", а для параметра `filename` — имя файла с изображением. Форма содержит двоичные данные изображения. Максимально допустимый размер отправляемого изображения — 1 МБ.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

4. Создайте объект словаря для хранения информации о заголовке запроса. Привяжите ключ подписки к строке `Ocp-Apim-Subscription-Key`.

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

5. Создайте еще один словарь, содержащий изображение, которое будет открываться и загружаться при отправке запроса.

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>Проанализируйте ответ JSON.

Создайте метод `print_json()` для получения ответа API и выведите на экран код JSON.

```python
def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
```

## <a name="send-the-request"></a>Отправка запроса

Используйте `requests.post()` для отправки запроса в Визуальный поиск Bing. Включите строку для конечной точки, заголовка и сведений о файле. Выведите на экран `response.json()` с помощью `print_json()`.

```python
try:
    response = requests.post(BASE_URI, headers=HEADERS, files=file)
    response.raise_for_status()
    print_json(response.json())
    
except Exception as ex:
    raise ex
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Create a Visual Search single-page web app](../tutorial-bing-visual-search-single-page-app.md) (Создание одностраничного веб-приложения Визуального поиска)
