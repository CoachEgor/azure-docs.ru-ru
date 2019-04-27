---
title: Использование API Anomaly Finder с Python. Microsoft Cognitive Services | Документация Майкрософт
description: Информация и примеры кода, которые помогут вам приступить к работе с API Anomaly Finder с использованием Python в Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: df894fee3328b2199b8f7436adabfd3db62f4004
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61217298"
---
# <a name="use-the-anomaly-finder-api-with-python"></a>Использование API Anomaly Finder с Python

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

В этой статье содержатся сведения и примеры кода, которые помогут вам быстро приступить к работе с API Anomaly Finder с Python для получения аномальных результатов в данных временных рядов.

## <a name="prerequisites"></a>Технические условия

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-python"></a>Получение точек аномалий с помощью API Anomaly Finder с использованием Python 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Пример данных временных рядов

Ниже приведен пример точек данных по временным рядам.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-python-example"></a>Пример анализа данных и получения точек аномалий с помощью Python

Убедитесь, что вы установили Python 3, затем создайте исполняемый файл Python с именем detect.py. В файл detect.py следует включить приведенный ниже код. Перед выполнением кода не забудьте заменить значение `[YOUR_SUBSCRIPTION_KEY]` на действующий ключ подписки.
Замените `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` точками данных.

```python
import requests
import json


def detect(url, subscription_key, request_data):
    headers = {'Content-Type': 'application/json', 'Ocp-Apim-Subscription-Key': subscription_key}
    response = requests.post(url, data=json.dumps(request_data), headers=headers)
    if response.status_code == 200:
        return json.loads(response.content.decode("utf-8"))
    else:
        print(response.status_code)
        raise Exception(response.text)


sample_data = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]"
endpont = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
subscription_key = "[YOUR_SUBSCRIPTION_KEY]"

result = detect(endpont, subscription_key, sample_data)
print(result)

```

### <a name="example-response"></a>Пример ответа

Успешный ответ будет возвращен в формате JSON. Пример ответа приведен ниже.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Приложение Python](../tutorials/python-tutorial.md)
