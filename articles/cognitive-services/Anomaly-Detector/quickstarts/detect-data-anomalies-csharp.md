---
title: Краткое руководство. Обнаружение аномалий в данных временных рядов с использованием REST API Детектора аномалий и C#
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать API "Детектор аномалий" для обнаружения отклонений в ряде данных как в пакетном режиме, так и при потоковой передаче.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 06/30/2020
ms.author: aahi
ms.openlocfilehash: 585731212fa31be2757d5b5d4c4e0a2ef1212ca8
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/06/2020
ms.locfileid: "85980228"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Краткое руководство. Обнаружение аномалий в данных временных рядов с использованием REST API Детектора аномалий и C# 

Используйте это краткое руководство, чтобы узнать, как начать использовать две модели API Детектора аномалий для обнаружения аномалий в данных временных рядов. Это приложение C# отправляет два запроса API, содержащие данные временных рядов в формате JSON, и получает ответы.

| Запрос API                                        | Выходные данные приложения                                                                                                                                         |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Обнаружение аномалий в пакетном режиме                        | Ответ в формате JSON содержит информацию о состоянии аномалий (и другие данные) для каждой точки в данных временного ряда, а также положения всех обнаруженных аномалий. |
| Обнаружение состояний аномалии последней точки данных | В ответе JSON содержится состояние аномалии (и другие данные) для последней точки данных в данных временных рядов.                                        |

 Хотя это приложение создается на языке C#, API представляет собой веб-службу RESTful, совместимую с большинством языков программирования. Исходный код для этого краткого руководства можно найти на портале [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/csharp-detect-anomalies.cs).

## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure — [создайте бесплатную учетную запись](https://azure.microsoft.com/free/).
- Получив подписку Azure, перейдите к <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title="Создание ресурса Детектора аномалий"  target="_blank">созданию ресурса Детектора аномалий <span class="docon docon-navigate-external x-hidden-focus"></span></a> на портале Azure, чтобы получить ключ и конечную точку. Дождитесь, пока закончится развертывание, и нажмите кнопку **Перейти к ресурсу**.
    - Для подключения приложения к API "Детектор аномалий" потребуется ключ и конечная точка из созданного ресурса. Ключ и конечная точка будут вставлены в приведенный ниже код в кратком руководстве.
    Используйте бесплатную ценовую категорию (`F0`), чтобы опробовать службу, а затем выполните обновление до платного уровня для рабочей среды.
- Любой выпуск [Visual Studio 2017 или более поздней версии](https://visualstudio.microsoft.com/downloads/)
- Платформа [Json.NET](https://www.newtonsoft.com/json), доступная в виде пакета NuGet. Чтобы установить Newtonsoft.Json в качестве пакета NuGet в Visual Studio, сделайте следующее.
    
    1. Щелкните правой кнопкой мыши имя проекта в **обозревателе решений**.
    2. Выберите **Manage NuGet Packages...** (Управление пакетами NuGet...).
    3. Найдите пакет *Newtonsoft.Json* и установите его.

- Если вы используете Linux или MacOS, это приложение можно запустить с помощью [Mono](https://www.mono-project.com/).

- JSON-файл содержит точки данных временного ряда. Пример данных для этого краткого руководства можно найти на портале [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [anomaly-detector-environment-variables](../includes/environment-variables.md)]

## <a name="create-a-new-application"></a>Создание приложения

1. В Visual Studio создайте новое консольное решение и добавьте следующие пакеты. 

    [!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=usingStatements)]


2. Создайте переменные для ключа подписки и конечной точки. Далее приведены универсальные коды ресурса (URI), которые можно использовать для обнаружения аномалий. Для создания URL-адресов запроса API они будут добавлены в конечную точку службы позднее.

    | Метод обнаружения                   | URI                                              |
    |------------------------------------|--------------------------------------------------|
    | Пакетное обнаружение                    | `/anomalydetector/v1.0/timeseries/entire/detect` |
    | Обнаружение в последней точке данных | `/anomalydetector/v1.0/timeseries/last/detect`   |
        
    [!code-csharp[initial variables for endpoint, key and data file](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=vars)]

## <a name="create-a-function-to-send-requests"></a>Создание функции для отправки запросов

1. Создайте новую асинхронную функцию с именем `Request`, принимающую переменные, созданные выше.

2. Установите протокол безопасности клиента и информацию заголовка с помощью объекта `HttpClient`. Не забудьте добавить ключ подписки в заголовок `Ocp-Apim-Subscription-Key`. Затем создайте объект `StringContent` для запроса.

3. Отправьте запрос с `PostAsync()`, а затем верните ответ.

    [!code-csharp[Request method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=requestMethod)]

## <a name="detect-anomalies-as-a-batch"></a>Обнаружение аномалий в пакетном режиме

1. Создайте новую функцию с именем `detectAnomaliesBatch()`. Создайте запрос и отправьте его, вызвав функцию `Request()` с указанием вашей конечной точки, ключа подписки, URL-адреса для пакетного обнаружения аномалий и данных временного ряда.

2. Десериализируйте объект JSON и выведите его в консоль.

3. Если ответ содержит поле `code`, выведите код ошибки и сообщение об ошибке. 

4. В противном случае найдите положения аномалий в наборе данных. Поле `isAnomaly` ответа содержит массив логических значений, каждое из которых указывает, является ли точка данных аномалией. Преобразуйте его в строковый массив с помощью функции `ToObject<bool[]>()` объекта ответа. Пройдите по массиву, а затем выведите перечень всех `true` значений. Эти значения соответствуют индексу аномальных точек данных, если они были найдены.

    [!code-csharp[Detect anomalies batch](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesBatch)]


## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Обнаружение состояний аномалии последней точки данных

1. Создайте новую функцию с именем `detectAnomaliesLatest()`. Создайте запрос и отправьте его, вызвав функцию `Request()` с указанием конечной точки, ключа подписки, URL-адреса для обнаружения последней точки аномалии и данных временного ряда.

2. Десериализируйте объект JSON и выведите его в консоль.

    [!code-csharp[Detect anomalies latest](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesLatest)]
 
## <a name="load-your-time-series-data-and-send-the-request"></a>Загрузка данных временных рядов и отправка запроса

1. В методе main приложения загрузите данные временного ряда в формате JSON с помощью `File.ReadAllText()`. 

2. Вызовите функции обнаружения аномалий, созданные ранее. Чтобы после выполнения приложения оставить консоль открытой, используйте `System.Console.ReadKey()`.

    [!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=main)]

### <a name="example-response"></a>Пример ответа

Успешный ответ возвращается в формате JSON. Перейдите по ссылками ниже, чтобы просмотреть ответ JSON на GitHub:
* [Пример ответа при пакетном обнаружении](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Пример ответа при обнаружении последней точки](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
