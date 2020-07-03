---
title: Краткое руководство. Обнаружение лиц на изображении с помощью REST API и Java
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как определить лица на изображении с помощью REST API распознавания лиц Azure и Java.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 59c121f09858114cecfaeb45e257315d1ab92935
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81403424"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Краткое руководство. Обнаружение лиц на изображении с помощью REST API и Java

В этом кратком руководстве описано, как определить человеческие лица на изображении с помощью REST API распознавания лиц Azure и Java.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу. 

## <a name="prerequisites"></a>Предварительные требования

- Ключ подписки на службу "Распознавание лиц". Вы можете получить ключ бесплатной пробной подписки на странице [Пробная версия Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Или следуйте инструкциям в руководстве по [созданию учетной записи Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account), чтобы получить подписку на службу "Распознавание лиц" и свой ключ.
- Любая интегрированная среда разработки Java (Java IDE).

## <a name="create-the-java-project"></a>Создание проекта Java

1. Создайте приложение командной строки Java в своей среде IDE и добавьте класс **Main** с методом **main**.
1. Импортируйте следующие библиотеки в свой проект Java. Если вы используете Maven — его координаты предоставляются для каждой библиотеки.
   - [HTTP-клиент Apache](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.6)
   - [HTTP-ядро Apache](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.10)
   - [Библиотека JSON](https://github.com/stleary/JSON-java) (org.json:json:20180130)
   - [Ведение журнала Apache Commons](https://commons.apache.org/proper/commons-logging/download_logging.cgi) (commons-logging:commons-logging:1.1.2)

## <a name="add-face-detection-code"></a>Добавление кода определения лиц

Откройте класс main своего проекта. Здесь вы добавите код, необходимый для загрузки изображений и определения лиц.

### <a name="import-packages"></a>Импорт пакетов

Добавьте в начало файла следующие инструкции `import`.

```java
// This sample uses Apache HttpComponents:
// http://hc.apache.org/httpcomponents-core-ga/httpcore/apidocs/
// https://hc.apache.org/httpcomponents-client-ga/httpclient/apidocs/

import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONObject;
```

### <a name="add-essential-fields"></a>Добавление важных полей

Замените класс **Main** следующим кодом: Они указывают, как подключиться к службе распознавания лиц и где получить входные данные. Вам нужно будет обновить поле `subscriptionKey`, указав значение вашего ключа подписки, и изменить строку `uriBase`, добавив в нее требуемую строку конечной точки. Вы также можете задать для `imageWithFaces` путь, указывающий на файл другого изображения.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

Поле `faceAttributes` — это просто список некоторых типов атрибутов. Оно определяет, какую информацию о распознанных лицах нужно получить.

```Java
public class Main {
    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    private static final String uriBase =
        "https://<My Endpoint String>.com/face/v1.0/detect";

    private static final String imageWithFaces =
        "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}";

    private static final String faceAttributes =
        "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";
```

### <a name="call-the-face-detection-rest-api"></a>Вызов REST API распознавания лиц

Дополните метод **main** следующим кодом. Он создает вызов REST к API распознавания лиц, чтобы определить информацию о лицах на удаленном изображении (строка `faceAttributes` указывает, какие атрибуты лица нужно получить). Затем он записывает выходные данные в строку JSON.

```Java
    public static void main(String[] args) {
        HttpClient httpclient = HttpClientBuilder.create().build();

        try
        {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("returnFaceId", "true");
            builder.setParameter("returnFaceLandmarks", "false");
            builder.setParameter("returnFaceAttributes", faceAttributes);

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity reqEntity = new StringEntity(imageWithFaces);
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();
```

### <a name="parse-the-json-response"></a>Проанализируйте ответ JSON.

Прямо под предыдущим кодом добавьте следующий блок, который преобразует возвращенные данные JSON в более удобный для чтения формат, прежде чем вывести их на консоль. Наконец, закройте блок try-catch, метод **main** и класс **Main**.

```Java
            if (entity != null)
            {
                // Format and display the JSON response.
                System.out.println("REST Response:\n");

                String jsonString = EntityUtils.toString(entity).trim();
                if (jsonString.charAt(0) == '[') {
                    JSONArray jsonArray = new JSONArray(jsonString);
                    System.out.println(jsonArray.toString(2));
                }
                else if (jsonString.charAt(0) == '{') {
                    JSONObject jsonObject = new JSONObject(jsonString);
                    System.out.println(jsonObject.toString(2));
                } else {
                    System.out.println(jsonString);
                }
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="run-the-app"></a>Запустите приложение

Скомпилируйте код и запустите его. При успешном ответе в окне консоли отобразятся данные распознавания лиц в легко читаемом формате JSON. Пример:

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  },
  "faceAttributes": {
    "makeup": {
      "eyeMakeup": true,
      "lipMakeup": true
    },
    "facialHair": {
      "sideburns": 0,
      "beard": 0,
      "moustache": 0
    },
    "gender": "female",
    "accessories": [],
    "blur": {
      "blurLevel": "low",
      "value": 0.06
    },
    "headPose": {
      "roll": 0.1,
      "pitch": 0,
      "yaw": -32.9
    },
    "smile": 0,
    "glasses": "NoGlasses",
    "hair": {
      "bald": 0,
      "invisible": false,
      "hairColor": [
        {
          "color": "brown",
          "confidence": 1
        },
        {
          "color": "black",
          "confidence": 0.87
        },
        {
          "color": "other",
          "confidence": 0.51
        },
        {
          "color": "blond",
          "confidence": 0.08
        },
        {
          "color": "red",
          "confidence": 0.08
        },
        {
          "color": "gray",
          "confidence": 0.02
        }
      ]
    },
    "emotion": {
      "contempt": 0,
      "surprise": 0.005,
      "happiness": 0,
      "neutral": 0.986,
      "sadness": 0.009,
      "disgust": 0,
      "anger": 0,
      "fear": 0
    },
    "exposure": {
      "value": 0.67,
      "exposureLevel": "goodExposure"
    },
    "occlusion": {
      "eyeOccluded": false,
      "mouthOccluded": false,
      "foreheadOccluded": false
    },
    "noise": {
      "noiseLevel": "low",
      "value": 0
    },
    "age": 22.9
  },
  "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f"
}]
```

## <a name="next-steps"></a>Дальнейшие действия

В рамках этого краткого руководства вы создали простое консольное приложение Java, которое использует вызовы REST с API Azure "Распознавание лиц" для обнаружения лиц на изображении и возврата их атрибутов. Узнайте, что еще можно делать с помощью этой функции в приложении Android.

> [!div class="nextstepaction"]
> [Руководство. Создание приложения Android для обнаружения и выделения лиц на изображении](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)
