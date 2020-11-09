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
ms.openlocfilehash: 712f86eeaa49c1afe281ad5ede7a6cf2cc0ada4b
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93074986"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Краткое руководство. Получение аналитических сведений об изображении с помощью REST API и Python Визуального поиска Bing

> [!WARNING]
> API Поиска Bing будут перенесены из Cognitive Services в службы Поиска Bing. С **30 октября 2020 г.** подготовку всех новых экземпляров Поиска Bing необходимо будет выполнять в соответствии с процедурой, описанной [здесь](https://aka.ms/cogsvcs/bingmove).
> API-интерфейсы Поиска Bing, подготовленные с помощью Cognitive Services, будут поддерживаться в течение следующих трех лет или до завершения срока действия вашего Соглашения Enterprise (в зависимости от того, какой период окончится раньше).
> Инструкции по миграции см. в статье о [службах Поиска Bing](https://aka.ms/cogsvcs/bingmigration).

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
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
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
