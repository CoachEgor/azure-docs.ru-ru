---
title: Краткое руководство. Анализ удаленного изображения — REST, PHP
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как проанализировать изображение с помощью API компьютерного зрения на PHP.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 432ad21ac856f4abde26d30b8f1ef8b34cc6dc0e
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995648"
---
# <a name="quickstart-analyze-a-remote-image-using-the-rest-api-and-php-in-computer-vision"></a>Краткое руководство. Анализ удаленного изображения с помощью REST API компьютерного зрения и PHP

Из этого краткого руководства вы узнаете, как анализировать удаленное изображение с помощью REST API в API компьютерного зрения, чтобы извлечь визуальные признаки. С помощью метода [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) можно извлечь визуальные функции на основе содержимого изображения.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

- Необходимо установить [PHP](https://secure.php.net/downloads.php).
- Необходимо установить [Pear](https://pear.php.net).
- У вас должен быть ключ подписки для Компьютерного зрения. На странице [Пробная версия Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) можно получить бесплатную пробную версию ключа. Или следуйте инструкциям из статьи [Create a Cognitive Services account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) (Создание учетной записи Cognitive Services), чтобы получить подписку Content Moderator и свой ключ.

## <a name="create-and-run-the-sample"></a>Создание и выполнение примера кода

Чтобы создать и запустить пример, сделайте следующее.

1. Установите пакет [`HTTP_Request2`](https://pear.php.net/package/HTTP_Request2) для PHP5.
   1. Откройте окно командной строки с правами администратора.
   1. Выполните следующую команду:

      ```console
      pear install HTTP_Request2
      ```

   1. После успешной установки пакета закройте окно командной строки.

1. Скопируйте приведенный ниже код в текстовый редактор.
1. При необходимости внесите в код следующие изменения.
    1. Замените значение `subscriptionKey` своим ключом подписки.
    1. Замените значение `uriBase` URL-адресом конечной точки для метода [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) из региона Azure, где вы получили ключи подписки, если это необходимо.
    1. При необходимости замените значение `imageUrl` URL-адресом другого изображения, анализ которого следует выполнить.
    1. При необходимости замените значение параметра запроса `language` на другой язык.
1. Сохраните код как файл с расширением `.php`. Например, `analyze-image.php`.
1. Откройте окно браузера с поддержкой PHP.
1. Перетащите файл в окно браузера.

```php
<html>
<head>
    <title>Analyze Image Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

$imageUrl = 'https://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . '/analyze');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'visualFeatures' => 'Categories,Description',
    'details' => '',
    'language' => 'en'
);
$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body parameters
$body = json_encode(array('url' => $imageUrl));

// Request body
$request->setBody($body);

try
{
    $response = $request->send();
    echo "<pre>" .
        json_encode(json_decode($response->getBody()), JSON_PRETTY_PRINT) . "</pre>";
}
catch (HttpException $ex)
{
    echo "<pre>" . $ex . "</pre>";
}
?>
</body>
</html>
```

## <a name="examine-the-response"></a>Изучите ответ.

Успешный ответ будет возвращен в формате JSON. После этого запустится синтаксический анализ примера веб-сайта и в окне браузера отобразится успешный ответ, аналогичный следующему.

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "requestId": "ebf5a1bc-3ba2-4c56-99b4-bbd20ba28705",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если файл больше не нужен, удалите его, а затем удалите пакет `HTTP_Request2` на PHP5. Чтобы удалить пакет, сделайте следующее:

1. Откройте окно командной строки с правами администратора.
2. Выполните следующую команду:

   ```console
   pear uninstall HTTP_Request2
   ```

3. После успешного удаления пакета закройте окно командной строки.

## <a name="next-steps"></a>Дополнительная информация

Ознакомьтесь с API компьютерного зрения, который позволяет анализировать изображения, обнаруживать знаменитостей и достопримечательности, создавать эскизы, извлекать печатный и рукописный текст. Для быстрых экспериментов с API компьютерного зрения можно использовать [открытую консоль тестирования API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Обзор API компьютерного зрения](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
