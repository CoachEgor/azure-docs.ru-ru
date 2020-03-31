---
title: Краткое руководство. Получение аналитических сведений об изображениях с помощью REST API и Node.js — Визуальный поиск Bing
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
ms.openlocfilehash: 373d6fa5402ba703cbebe88ad562974ba97f3391
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75379714"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Краткое руководство. Получение аналитических сведений об изображениях с помощью REST API Визуального поиска Bing и Node.js

В этом кратком руководстве вы узнаете, как сделать первый вызов API визуального поиска Bing и просмотреть результаты поиска. Это простое приложение JavaScript отправляет изображение в API и отображает данные о нем. Хотя это приложение создается на языке JavaScript, API представляет собой веб-службу RESTful, совместимую с большинством языков программирования.

## <a name="prerequisites"></a>Предварительные требования

* [Node.js](https://nodejs.org/en/download/)
* Модуль запросов для JavaScript. Вы можете использовать команду `npm install request`, чтобы установить модуль.
* Модуль данных формы. Вы можете использовать команду `npm install form-data`, чтобы установить модуль. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Инициализация приложения

1. Создайте файл JavaScript в избранной интегрированной среде разработки или редакторе и установите следующие требования:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. Создайте переменные для конечной точки API, ключ подписки и путь к изображению. В качестве `baseUri` может быть глобальная конечная точка, приведенная ниже, или конечная точка [пользовательского поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), отображаемая на портале Azure для вашего ресурса.

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Создайте функцию с именем `requestCallback()`, чтобы вывести ответ, полученный от API:

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Создание и отправка поискового запроса

При отправке локального изображения данные формы должны содержать заголовок `Content-Disposition`. Необходимо задать для его параметра `name` значение "image", а для параметра `filename` можно присвоить любую строку. Форма содержит двоичные данные изображения. Максимально допустимый размер отправляемого изображения — 1 МБ.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

1. Создайте объект **FormData** с помощью `FormData()` и добавьте в него путь к своему изображению, используя `fs.createReadStream()`:
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. Используйте библиотеку запросов, чтобы отправить изображение, и вызовите `requestCallback()` для вывода ответа. Не забудьте добавить ключ подписки в заголовок запроса:

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Build a Visual Search single-page web app](../tutorial-bing-visual-search-single-page-app.md) (Создание одностраничного веб-приложения Визуального поиска)
