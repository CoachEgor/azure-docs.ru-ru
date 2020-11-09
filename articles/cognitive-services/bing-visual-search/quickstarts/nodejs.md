---
title: Краткое руководство. Получение аналитических сведений об изображениях с помощью REST API и Node.js — Визуальный поиск Bing
titleSuffix: Azure Cognitive Services
description: Узнайте, как отправить изображение с помощью API Визуального поиска Bing и Node.js, а затем получить аналитические сведения об этом изображении.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.author: scottwhi
ms.custom: devx-track-js
ms.openlocfilehash: 393746eee09b8fc6c3518f6f864d742abc0476fc
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93096824"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Краткое руководство. Получение аналитических сведений об изображениях с помощью REST API Визуального поиска Bing и Node.js

> [!WARNING]
> API Поиска Bing будут перенесены из Cognitive Services в службы Поиска Bing. С **30 октября 2020 г.** подготовку всех новых экземпляров Поиска Bing необходимо будет выполнять в соответствии с процедурой, описанной [здесь](https://aka.ms/cogsvcs/bingmove).
> API-интерфейсы Поиска Bing, подготовленные с помощью Cognitive Services, будут поддерживаться в течение следующих трех лет или до завершения срока действия вашего Соглашения Enterprise (в зависимости от того, какой период окончится раньше).
> Инструкции по миграции см. в статье о [службах Поиска Bing](https://aka.ms/cogsvcs/bingmigration).

Используйте это краткое руководство, чтобы выполнить вызов API Визуального поиска Bing. Это простое приложение JavaScript отправляет изображение в API и отображает данные о нем. Хотя приложение написано на JavaScript, API представляет собой веб-службу RESTful и совместим с большинством языков программирования.

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

2. Создайте переменные для конечной точки API, ключ подписки и путь к изображению. Для значения `baseUri` вы можете использовать глобальную конечную точку, указанную в коде ниже, или конечную точку [личного поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), которая отображается на портале Azure для вашего ресурса.

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Создайте функцию с именем `requestCallback()`, чтобы выводить полученный от API ответ.

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Создание и отправка поискового запроса

1. При отправке локального изображения данные формы должны содержать заголовок `Content-Disposition`. Задайте для его параметра `name` значение "image", а для параметра `filename` — имя файла с изображением. Форма содержит двоичные данные изображения. Максимально допустимый размер отправляемого изображения — 1 МБ.

   ```
   --boundary_1234-abcd
   Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

   ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

   --boundary_1234-abcd--
   ```

2. Создайте новый объект `FormData` с помощью `FormData()` и добавьте к нему путь к изображению с помощью `fs.createReadStream()`.
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

3. Используйте библиотеку запросов, чтобы отправить изображение, и вызовите `requestCallback()` для вывода ответа. Добавьте ключ подписки в заголовок запроса.

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
