---
title: Краткое руководство. Получение аналитических сведений об изображениях с помощью REST API и Java — Визуальный поиск Bing
titleSuffix: Azure Cognitive Services
description: Узнайте, как отправить изображение в API визуального поиска Bing и получить аналитические сведения об этом изображении.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.custom: devx-track-java
ms.author: scottwhi
ms.openlocfilehash: 6e0e0cc2513b1c2a5f89e61984331399ebae269a
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "87320448"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-java"></a>Краткое руководство. Получение аналитических сведений об изображениях с помощью REST API визуального поиска Bing и Java

Используйте это краткое руководство, чтобы выполнить вызов к API "Визуальный поиск Bing". Это приложение Java отправляет изображение в API и отображает возвращенные данные о нем. Это приложение создано на языке Java. Но API представляет собой веб-службу RESTful, совместимую с большинством языков программирования.

## <a name="prerequisites"></a>Предварительные требования

* [Комплект SDK Java (JDK) 7 или 8](https://aka.ms/azure-jdks).
* [Библиотеки Gson Java](https://github.com/google/gson).
* [Apache HttpComponents](https://hc.apache.org/downloads.cgi).

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Создание и инициализация проекта

1. Создайте проект Java в любой интегрированной среде разработки или редакторе, а затем импортируйте в него следующие библиотеки:

    ```java
    import java.util.*;
    import java.io.*;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    
    // HttpClient libraries
    
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.ContentType;
    import org.apache.http.entity.mime.MultipartEntityBuilder;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    ```

2. Создайте переменные для конечной точки API, ключ подписки и путь к изображению. Для значения `endpoint` вы можете использовать глобальную конечную точку, указанную в коде ниже, или конечную точку [личного поддомена](../../../cognitive-services/cognitive-services-custom-subdomains.md), которая отображается на портале Azure для вашего ресурса.

    ```java
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

    
3. При отправке локального изображения данные формы должны содержать заголовок `Content-Disposition`. Задайте для его параметра `name` значение "image", а для параметра `filename` имя файла с изображением. Форма содержит двоичные данные изображения. Максимально допустимый размер отправляемого изображения — 1 МБ.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

## <a name="create-the-json-parser"></a>Создание средства синтаксического анализа JSON

Создайте метод, чтобы сделать ответ JSON от API более удобным для чтения с помощью `JsonParser`.

```java
public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
```

## <a name="construct-the-search-request-and-query"></a>Создание поискового запроса и запроса

1. В методе main приложения создайте клиент HTTP с помощью `HttpClientBuilder.create().build();`.

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. Создайте объект `HttpEntity` для отправки изображения в API.

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Создайте объект `httpPost` с конечной точкой и задайте заголовок, чтобы использовать ключ подписки.

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## <a name="receive-and-process-the-json-response"></a>Получение и обработка ответа в формате JSON

1. Используйте метод `HttpClient.execute()` для отправки запроса в API и сохраните ответ в объекте `InputStream`.
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. Сохраните строку JSON и выведите ответ.

    ```java
    String json = new Scanner(stream).useDelimiter("\\A").next();
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(json));
    ```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Build a Visual Search single-page web app](../tutorial-bing-visual-search-single-page-app.md) (Создание одностраничного веб-приложения Визуального поиска)
