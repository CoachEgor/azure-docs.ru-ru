---
title: Краткое руководство.  Извлечение печатного и рукописного текста через REST, Java с помощью Компьютерного зрения версии 2.0 и 2.1
titleSuffix: Azure Cognitive Services
description: Из этого краткого руководства вы узнаете, как использовать API компьютерного зрения для извлечения печатного и рукописного текста из изображения с помощью Java.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 12/05/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 15c84b0c4cd4311300b951c3bf86b2bd62d48bfd
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2020
ms.locfileid: "77566137"
---
# <a name="quickstart-extract-printed-and-handwritten-text-using-the-computer-vision-20-and-21-rest-api-and-java"></a>Краткое руководство. Извлечение печатного и рукописного текста с помощью REST API "Компьютерное зрение" версии 2.0 и 2.1 и Java

Из этого краткого руководства вы узнаете, как извлечь печатный и рукописный текст из изображения с помощью REST API Компьютерного зрения. С помощью методов [Batch Read](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) и [Read Operation Result](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/5be108e7498a4f9ed20bf96d) вы можете обнаружить текст на изображении и извлечь распознанные символы в поток машиночитаемых символов. API определит, какую модель распознавания следует использовать для каждой строки текста, так как он поддерживает изображения с печатным и рукописным текстом.

В сравнении с Компьютерным зрением версий 2.0 и 2.1 общедоступная предварительная версия Компьютерного зрения 3.0 предоставляет следующие преимущества:

* еще большую точность;
* измененный формат вывода;
* оценка достоверности для машинных слов;
* поддержка испанского и английского языков с дополнительным языковым параметром.

#### <a name="version-2"></a>[Версия 2](#tab/version-2)

> [!IMPORTANT]
> Метод [Batch Read](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) выполняется асинхронно. Этот метод не возвращает никаких данных в текст успешного ответа. Вместо этого метод Batch Read возвращает URI в значение поля заголовка ответа `Operation-Location`. Затем можно вызвать этот URI, который представляет API [Read Operation Result](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/5be108e7498a4f9ed20bf96d), чтобы проверить состояние и вернуть результаты вызова метода Batch Read.

#### <a name="version-3-public-preview"></a>[Версия 3 (общедоступная предварительная версия)](#tab/version-3)

> [!IMPORTANT]
> Метод [Batch Read](https://westus2.dev.cognitive.microsoft.com/docs/services/5d98695995feb7853f67d6a6/operations/5d986960601faab4bf452005) выполняется асинхронно. Этот метод не возвращает никаких данных в текст успешного ответа. Вместо этого метод Batch Read возвращает URI в значение поля заголовка ответа `Operation-Location`. Затем можно вызвать этот URI, который представляет API [Read Operation Result](https://westus2.dev.cognitive.microsoft.com/docs/services/5d98695995feb7853f67d6a6/operations/5d9869604be85dee480c8750), чтобы проверить состояние и вернуть результаты вызова метода Batch Read.

---

## <a name="prerequisites"></a>Предварительные требования

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services), прежде чем начинать работу.

- У вас должна быть установлена платформа [Java&trade;, комплект разработчика Java, выпуск "Стандартный" версии 7 или 8](https://aka.ms/azure-jdks) (JDK 7 или 8).
- У вас должен быть ключ подписки для Компьютерного зрения. На странице [Try Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) (Пробная версия Cognitive Services) можно получить ключ бесплатной пробной версии. Или следуйте инструкциям из статьи [Create a Cognitive Services account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) (Создание учетной записи Cognitive Services), чтобы получить подписку Content Moderator и свой ключ. Затем [создайте переменные среды](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) для строки ключа и конечной точки службы с именами `COMPUTER_VISION_SUBSCRIPTION_KEY` и `COMPUTER_VISION_ENDPOINT` соответственно.

## <a name="create-and-run-the-sample-application"></a>Создание и запуск примера приложения

#### <a name="version-2"></a>[Версия 2](#tab/version-2)

Чтобы создать и запустить пример, сделайте следующее.

1. Создайте проект Java в используемой вами интегрированной среде разработки или редакторе. Если этот параметр доступен, создайте проект Java из шаблона приложения командной строки.
1. Импортируйте следующие библиотеки в свой проект Java. Если вы используете Maven — его координаты предоставляются для каждой библиотеки.
   - [HTTP-клиент Apache](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.5)
   - [HTTP-ядро Apache](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.9)
   - [Библиотека JSON](https://github.com/stleary/JSON-java) (org.json:json:20180130)
1. Добавьте следующие операторы `import` в файл, содержащий общий класс `Main` для вашего проекта.  

   ```java
   import java.net.URI;
   import org.apache.http.HttpEntity;
   import org.apache.http.HttpResponse;
   import org.apache.http.client.methods.HttpGet;
   import org.apache.http.client.methods.HttpPost;
   import org.apache.http.client.utils.URIBuilder;
   import org.apache.http.entity.StringEntity;
   import org.apache.http.impl.client.CloseableHttpClient;
   import org.apache.http.impl.client.HttpClientBuilder;
   import org.apache.http.util.EntityUtils;
   import org.apache.http.Header;
   import org.json.JSONObject;
   ```

1. Замените открытый класс `Main` следующим кодом.
1. При необходимости замените значение `imageToAnalyze` URL-адресом другого изображения, из которого вы хотите извлечь текст.
1. Сохраните, а затем создайте проект Java.
1. Если вы используете интегрированную среду разработки, запустите `Main`. В противном случае откройте окно командной строки и затем используйте команду `java` для выполнения скомпилированного класса. Например, `java Main`.

```java
public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Add your Computer Vision subscription key and endpoint to your environment variables.
    // After setting, close and then re-open your command shell or project for the changes to take effect.
    String subscriptionKey = System.getenv("COMPUTER_VISION_SUBSCRIPTION_KEY");
    String endpoint = ("COMPUTER_VISION_ENDPOINT");

    private static final String uriBase = endpoint + 
            "vision/v2.1/read/core/asyncBatchAnalyze";

    private static final String imageToAnalyze =
        "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
        "Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg";

    public static void main(String[] args) {
        CloseableHttpClient httpTextClient = HttpClientBuilder.create().build();
        CloseableHttpClient httpResultClient = HttpClientBuilder.create().build();;

        try {
            // This operation requires two REST API calls. One to submit the image
            // for processing, the other to retrieve the text found in the image.

            URIBuilder builder = new URIBuilder(uriBase);

            // Prepare the URI for the REST API method.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"" + imageToAnalyze + "\"}");
            request.setEntity(requestEntity);

            // Two REST API methods are required to extract text.
            // One method to submit the image for processing, the other method
            // to retrieve the text found in the image.

            // Call the first REST API method to detect the text.
            HttpResponse response = httpTextClient.execute(request);

            // Check for success.
            if (response.getStatusLine().getStatusCode() != 202) {
                // Format and display the JSON error message.
                HttpEntity entity = response.getEntity();
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Error:\n");
                System.out.println(json.toString(2));
                return;
            }

            // Store the URI of the second REST API method.
            // This URI is where you can get the results of the first REST API method.
            String operationLocation = null;

            // The 'Operation-Location' response header value contains the URI for
            // the second REST API method.
            Header[] responseHeaders = response.getAllHeaders();
            for (Header header : responseHeaders) {
                if (header.getName().equals("Operation-Location")) {
                    operationLocation = header.getValue();
                    break;
                }
            }

            if (operationLocation == null) {
                System.out.println("\nError retrieving Operation-Location.\nExiting.");
                System.exit(1);
            }

            // If the first REST API method completes successfully, the second
            // REST API method retrieves the text written in the image.
            //
            // Note: The response may not be immediately available. Text
            // recognition is an asynchronous operation that can take a variable
            // amount of time depending on the length of the text.
            // You may need to wait or retry this operation.

            System.out.println("\nText submitted.\n" +
                    "Waiting 10 seconds to retrieve the recognized text.\n");
            Thread.sleep(10000);

            // Call the second REST API method and get the response.
            HttpGet resultRequest = new HttpGet(operationLocation);
            resultRequest.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            HttpResponse resultResponse = httpResultClient.execute(resultRequest);
            HttpEntity responseEntity = resultResponse.getEntity();

            if (responseEntity != null) {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(responseEntity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Text recognition result response: \n");
                System.out.println(json.toString(2));
            }
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```

#### <a name="version-3-public-preview"></a>[Версия 3 (общедоступная предварительная версия)](#tab/version-3)

Чтобы создать и запустить пример, сделайте следующее.

1. Создайте проект Java в используемой вами интегрированной среде разработки или редакторе. Если этот параметр доступен, создайте проект Java из шаблона приложения командной строки.
1. Импортируйте следующие библиотеки в свой проект Java. Если вы используете Maven — его координаты предоставляются для каждой библиотеки.
   - [HTTP-клиент Apache](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.5)
   - [HTTP-ядро Apache](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.9)
   - [Библиотека JSON](https://github.com/stleary/JSON-java) (org.json:json:20180130)
1. Добавьте следующие операторы `import` в файл, содержащий общий класс `Main` для вашего проекта.  

   ```java
    import java.net.URI;
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpGet;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.client.utils.URIBuilder;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    import org.apache.http.util.EntityUtils;
    import org.apache.http.Header;
    import org.json.JSONObject;
    ```

1. Замените открытый класс `Main` следующим кодом.
1. При необходимости замените значение `language` на язык, который необходимо распознать. Допустимые значения: EN — английский и ES — для испанского языка.
1. При необходимости замените значение `imageToAnalyze` URL-адресом другого изображения, из которого вы хотите извлечь текст.
1. Сохраните, а затем создайте проект Java.
1. Если вы используете интегрированную среду разработки, запустите `Main`. В противном случае откройте окно командной строки и затем используйте команду `java` для выполнения скомпилированного класса. Например, `java Main`.

```java

public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Add your Computer Vision subscription key and endpoint to your environment variables.
    // After setting, close and then re-open your command shell or project for the changes to take effect.
    private static String subscriptionKey = System.getenv("COMPUTER_VISION_SUBSCRIPTION_KEY");
    private static String endpoint = System.getenv("COMPUTER_VISION_ENDPOINT");

    // Set the language that you want to recognize
    private static String language = "en";  // Accepted values are "en" for English, or "es" for Spanish

    private static String uriBase = endpoint +
            "/vision/v3.0-preview/read/analyze";

    private static String imageToAnalyze =
            "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
                    "Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg";

    public static void main(String[] args) {
        CloseableHttpClient httpTextClient = HttpClientBuilder.create().build();
        CloseableHttpClient httpResultClient = HttpClientBuilder.create().build();;

        System.out.println("Endpoint:         " + endpoint);
        System.out.println("Subscription key: " + subscriptionKey);
        System.out.println("Language:         " + language);

        try {
            // This operation requires two REST API calls. One to submit the image
            // for processing, the other to retrieve the text found in the image.

            URIBuilder builder = new URIBuilder(uriBase);
            builder.setParameter("language", language);

            // Prepare the URI for the REST API method.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"" + imageToAnalyze + "\"}");
            request.setEntity(requestEntity);

            // Two REST API methods are required to extract text.
            // One method to submit the image for processing, the other method
            // to retrieve the text found in the image.

            // Call the first REST API method to detect the text.
            HttpResponse response = httpTextClient.execute(request);

            // Check for success.
            if (response.getStatusLine().getStatusCode() != 202) {
                // Format and display the JSON error message.
                HttpEntity entity = response.getEntity();
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Error:\n");
                System.out.println(json.toString(2));
                return;
            }

            // Store the URI of the second REST API method.
            // This URI is where you can get the results of the first REST API method.
            String operationLocation = null;

            // The 'Operation-Location' response header value contains the URI for
            // the second REST API method.
            Header[] responseHeaders = response.getAllHeaders();
            for (Header header : responseHeaders) {
                if (header.getName().equals("Operation-Location")) {
                    operationLocation = header.getValue();
                    break;
                }
            }

            if (operationLocation == null) {
                System.out.println("\nError retrieving Operation-Location.\nExiting.");
                System.exit(1);
            }

            // If the first REST API method completes successfully, the second
            // REST API method retrieves the text written in the image.
            //
            // Note: The response may not be immediately available. Text
            // recognition is an asynchronous operation that can take a variable
            // amount of time depending on the length of the text.
            // You may need to wait or retry this operation.

            System.out.println("\nText submitted.\n" +
                    "Waiting 10 seconds to retrieve the recognized text.\n");
            Thread.sleep(10000);

            // Call the second REST API method and get the response.
            HttpGet resultRequest = new HttpGet(operationLocation);
            resultRequest.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            HttpResponse resultResponse = httpResultClient.execute(resultRequest);
            HttpEntity responseEntity = resultResponse.getEntity();

            if (responseEntity != null) {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(responseEntity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Text recognition result response: \n");
                System.out.println(json.toString(2));
            }
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```

---

## <a name="examine-the-response"></a>Изучите ответ.

Успешный ответ будет возвращен в формате JSON. После этого запустится синтаксический анализ примера приложения и в окне консоли отобразится успешный ответ, аналогичный следующему.

#### <a name="version-2"></a>[Версия 2](#tab/version-2)

```json
Text submitted. Waiting 10 seconds to retrieve the recognized text.

Text recognition result response:

{
  "status": "Succeeded",
  "recognitionResults": [
    {
      "page": 1,
      "clockwiseOrientation": 349.59,
      "width": 3200,
      "height": 3200,
      "unit": "pixel",
      "lines": [
        {
          "boundingBox": [202,618,2047,643,2046,840,200,813],
          "text": "Our greatest glory is not",
          "words": [
            {
              "boundingBox": [204,627,481,628,481,830,204,829],
              "text": "Our"
            },
            {
              "boundingBox": [519,628,1057,630,1057,832,518,830],
              "text": "greatest"
            },
            {
              "boundingBox": [1114,630,1549,631,1548,833,1114,832],
              "text": "glory"
            },
            {
              "boundingBox": [1586,631,1785,632,1784,834,1586,833],
              "text": "is"
            },
            {
              "boundingBox": [1822,632,2115,633,2115,835,1822,834],
              "text": "not"
            }
          ]
        },
        {
          "boundingBox": [420,1273,2954,1250,2958,1488,422,1511],
          "text": "but in rising every time we fall",
          "words": [
            {
              "boundingBox": [423,1269,634,1268,635,1507,424,1508],
              "text": "but"
            },
            {
              "boundingBox": [667,1268,808,1268,809,1506,668,1507],
              "text": "in"
            },
            {
              "boundingBox": [874,1267,1289,1265,1290,1504,875,1506],
              "text": "rising"
            },
            {
              "boundingBox": [1331,1265,1771,1263,1772,1502,1332,1504],
              "text": "every"
            },
            {
              "boundingBox": [1812, 1263, 2178, 1261, 2179, 1500, 1813, 1502],
              "text": "time"
            },
            {
              "boundingBox": [2219, 1261, 2510, 1260, 2511, 1498, 2220, 1500],
              "text": "we"
            },
            {
              "boundingBox": [2551, 1260, 3016, 1258, 3017, 1496, 2552, 1498],
              "text": "fall"
            }
          ]
        },
        {
          "boundingBox": [1612, 903, 2744, 935, 2738, 1139, 1607, 1107],
          "text": "in never failing ,",
          "words": [
            {
              "boundingBox": [1611, 934, 1707, 933, 1708, 1147, 1613, 1147],
              "text": "in"
            },
            {
              "boundingBox": [1753, 933, 2132, 930, 2133, 1144, 1754, 1146],
              "text": "never"
            },
            {
              "boundingBox": [2162, 930, 2673, 927, 2674, 1140, 2164, 1144],
              "text": "failing"
            },
            {
              "boundingBox": [2703, 926, 2788, 926, 2790, 1139, 2705, 1140],
              "text": ",",
              "confidence": "Low"
            }
          ]
        }
      ]
    }
  ]
}
```

#### <a name="version-3-public-preview"></a>[Версия 3 (общедоступная предварительная версия)](#tab/version-3)

```json
{
  "analyzeResult": {
    "readResults": [{
      "unit": "pixel",
      "width": 800,
      "angle": 0.8206,
      "language": "en",
      "page": 1,
      "lines": [
        {
          "boundingBox": [
            6,
            4,
            774,
            14,
            773,
            61,
            5,
            49
          ],
          "words": [
            {
              "boundingBox": [
                14,
                5,
                76,
                6,
                74,
                49,
                12,
                48
              ],
              "confidence": 0.83,
              "text": "The"
            },
            {
              "boundingBox": [
                84,
                6,
                182,
                7,
                180,
                51,
                82,
                49
              ],
              "confidence": 0.762,
              "text": "quick"
            },
            {
              "boundingBox": [
                191,
                7,
                312,
                9,
                309,
                54,
                189,
                51
              ],
              "confidence": 0.67,
              "text": "brown"
            },
            {
              "boundingBox": [
                320,
                9,
                382,
                10,
                379,
                55,
                317,
                54
              ],
              "confidence": 0.849,
              "text": "fox"
            },
            {
              "boundingBox": [
                390,
                10,
                497,
                11,
                493,
                57,
                387,
                55
              ],
              "confidence": 0.703,
              "text": "jumps"
            },
            {
              "boundingBox": [
                506,
                11,
                596,
                12,
                591,
                59,
                502,
                57
              ],
              "confidence": 0.799,
              "text": "over"
            },
            {
              "boundingBox": [
                604,
                12,
                666,
                13,
                661,
                60,
                600,
                59
              ],
              "confidence": 0.923,
              "text": "the"
            },
            {
              "boundingBox": [
                674,
                13,
                773,
                14,
                768,
                62,
                670,
                60
              ],
              "confidence": 0.863,
              "text": "lazy"
            }
          ],
          "language": "en",
          "text": "The quick brown fox jumps over the lazy"
        },
        {
          "boundingBox": [
            5,
            53,
            79,
            56,
            77,
            95,
            4,
            92
          ],
          "words": [{
            "boundingBox": [
              6,
              53,
              74,
              56,
              72,
              95,
              5,
              92
            ],
            "confidence": 0.418,
            "text": "dog"
          }],
          "language": "en",
          "text": "dog"
        },
        {
          "boundingBox": [
            0,
            90,
            787,
            95,
            787,
            145,
            0,
            136
          ],
          "words": [
            {
              "boundingBox": [
                1,
                96,
                79,
                93,
                79,
                135,
                0,
                136
              ],
              "confidence": 0.835,
              "text": "Pack"
            },
            {
              "boundingBox": [
                87,
                93,
                151,
                92,
                151,
                135,
                87,
                135
              ],
              "confidence": 0.88,
              "text": "my"
            },
            {
              "boundingBox": [
                162,
                92,
                226,
                91,
                225,
                135,
                161,
                135
              ],
              "confidence": 0.301,
              "text": "box"
            },
            {
              "boundingBox": [
                234,
                91,
                335,
                90,
                335,
                135,
                233,
                135
              ],
              "confidence": 0.959,
              "text": "with"
            },
            {
              "boundingBox": [
                346,
                91,
                418,
                91,
                417,
                136,
                345,
                135
              ],
              "confidence": 0.489,
              "text": "five"
            },
            {
              "boundingBox": [
                426,
                91,
                527,
                93,
                527,
                138,
                425,
                136
              ],
              "confidence": 0.727,
              "text": "dozen"
            },
            {
              "boundingBox": [
                554,
                94,
                687,
                98,
                687,
                143,
                553,
                139
              ],
              "confidence": 0.377,
              "text": "liquor"
            },
            {
              "boundingBox": [
                701,
                99,
                787,
                103,
                787,
                146,
                700,
                143
              ],
              "confidence": 0.693,
              "text": "jugs"
            }
          ],
          "language": "en",
          "text": "Pack my box with five dozen liquor jugs"
        }
      ],
      "height": 154
    }],
    "version": "3.0.0"
  },
  "createdDateTime": "2020-02-11T21:21:14Z",
  "lastUpdatedDateTime": "2020-02-11T21:21:19Z",
  "status": "succeeded"
}
```

---

## <a name="clean-up-resources"></a>Очистка ресурсов

Если проект Java больше не нужен, удалите его вместе со скомпилированным классом и импортированными библиотеками.

## <a name="next-steps"></a>Дальнейшие действия

Изучите приложение Java Swing, которое использует API компьютерного зрения для оптического распознавания символов (OCR) и создания интеллектуально обрезанных эскизов, а также для обнаружения, классификации, добавления тегов и описания визуальных компонентов изображения, включая лица. Для быстрых экспериментов с API компьютерного зрения можно использовать [открытую консоль тестирования API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Руководство по использованию API компьютерного зрения для Java](../Tutorials/java-tutorial.md)
