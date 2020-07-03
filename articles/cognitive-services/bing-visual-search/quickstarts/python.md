---
title: Краткое руководство. Получение аналитических сведений об изображении с помощью REST API и Python — Визуальный поиск Bing
titleSuffix: Azure Cognitive Services
description: Узнайте, как отправить изображение в API визуального поиска Bing и получить аналитические сведения об этом изображении.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: scottwhi
ms.openlocfilehash: b56f6743b642904349797ac5b6167194f7916b45
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "75446593"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Краткое руководство. Получение аналитических сведений об изображении с помощью REST API и Python Визуального поиска Bing

Из этого краткого руководства вы узнаете, как сделать первый вызов API Визуального поиска Bing и просмотреть результаты. Это приложение Python отправляет изображение в API и отображает возвращенные данные о нем. Хотя это приложение создано на языке Python, API представляет собой веб-службу RESTful, совместимую с большинством языков программирования.

## <a name="prerequisites"></a>Предварительные требования

* [Python 3.x](https://www.python.org/)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Инициализация приложения

1. Создайте файл Python в избранной интегрированной среде разработки или редакторе и добавьте следующую инструкцию `import`:

    ```python
    import requests, json
    ```

2. Создайте переменные для ключа подписки, конечной точки и путь к отправляемому изображению. В качестве `BASE_URI` может быть глобальная конечная точка, приведенная ниже, или конечная точка [пользовательского поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), отображаемая на портале Azure для вашего ресурса.

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```
    
    При отправке локального изображения данные формы должны содержать заголовок `Content-Disposition`. Вам необходимо задать для его параметра `name` значение "image", а для параметра `filename` — любую строку. Форма содержит двоичные данные изображения. Максимально допустимый размер отправляемого изображения — 1 МБ.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

3. Создайте объект словаря для хранения информации о заголовке запроса. Привяжите ключ подписки к строке `Ocp-Apim-Subscription-Key`, как показано ниже:

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. Создайте еще один словарь, содержащий изображение, которое будет открываться и загружаться при отправке запроса:

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>Проанализируйте ответ JSON.

1. Создайте метод с именем `print_json()` для получения ответа API и напечатайте JSON:

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>Отправка запроса

1. Используйте `requests.post()` для отправки запроса в Визуальный поиск Bing. Включите строку для конечной точки, заголовка и сведений о файле. Напечатайте `response.json()` с помощью `print_json()`:

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
