---
title: Использование конечных точек службы "Машинное обучение" в Azure Stream Analytics
description: В этой статье объясняется, как использовать определяемые пользователем функции машинного обучения в Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 0c74c0f85ea1851bc50ee20dbde9336f4f7b757a
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164389"
---
# <a name="azure-machine-learning-studio-integration-in-stream-analytics-preview"></a>Интеграция Azure студии машинного обучения в Stream Analytics (Предварительная версия)
Stream Analytics поддерживает определяемые пользователем функции, которые требуют использования в студии машинного обучения Azure конечные точки. Поддержка этой возможности в REST API описана в статье [Библиотека REST API Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx). В этой статье приведены дополнительные сведения, необходимые для успешной реализации этой возможности в Stream Analytics. Также было размещено руководство, которое доступно [здесь](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-studio-terminology"></a>Рассматриваемые действия: Терминология студии машинного обучения Azure
Студия машинного обучения Microsoft Azure предоставляет средство совместной работы, и перетащите, которые можно использовать для создания, тестирования и развертывания решений прогнозной аналитики данных. Этот инструмент — *Студия машинного обучения Azure*. Студия используется для взаимодействия с ресурсами машинного обучения, а также для компилирования и тестирования прототипа с последующими итерациями. Эти ресурсы и их определения приведены ниже.

* **Рабочая область**. *Рабочая область* — это контейнер, содержащий все ресурсы машинного обучения для управления и контроля.
* **Эксперимент**. *Эксперименты* создаются специалистами по обработке и анализу данных для использования наборов данных и обучения моделей машинного обучения.
* **Конечная точка**. *Конечные точки* являются объектом студии машинного обучения Azure, используемые для принятия функций в качестве входных данных, применения выбранной модели машинного обучения и возвращения результата оценки.
* **Оценивающая веб-служба**. *Оценивающая веб-служба* — это коллекция конечных точек, упомянутых выше.

Каждая конечная точка имеет API для пакетного и синхронного выполнения. В Stream Analytics используется синхронное выполнение. В Студии машинного обучения Azure соответствующая служба называется [службой обработки запросов и ответов](../machine-learning/studio/consume-web-services.md).

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Ресурсы машинного обучения, необходимые для заданий Stream Analytics
Для успешной обработки задания Stream Analytics требуются конечная точка типа "запрос — ответ", [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md)и определение swagger. В Stream Analytics есть дополнительная конечная точка, которая создает URL-адрес конечной точки swagger, ищет интерфейс, а затем возвращает пользователю определение определяемой пользователем функции по умолчанию.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Настройка Stream Analytics и определяемой пользователем функции машинного обучения с помощью REST API
С помощью интерфейсов REST API можно настроить задание для вызова функций машинного обучения Azure. Для этого необходимо выполнить следующие шаги:

1. Создание задания Stream Analytics
2. Определение входных данных.
3. Определение выходных данных.
4. Создание определяемой пользователем функции
5. Создание преобразования Stream Analytics, которое вызывает определяемую пользователем функцию.
6. Запустите задание

## <a name="creating-a-udf-with-basic-properties"></a>Создание определяемой пользователем функции с базовыми свойствами
Например, в следующем примере кода создает определяемой пользователем скалярной функции с именем *newudf* , которая выполняет привязку к конечной точке студии машинного обучения Azure. Обратите внимание, что *конечную точку* (URI службы) можно найти на странице справки по API для выбранной службы, а *apiKey* — на главной странице служб.

```
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

Пример тела запроса:

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
```

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Вызов конечной точки RetrieveDefaultDefinition для определяемой пользователем функции по умолчанию
После создания схемы определяемой пользователем функции требуется получить ее полное определение. Конечной точки RetrieveDefaultDefinition помогает получить определение по умолчанию для скалярной функции, которая привязана к конечной точке студии машинного обучения Azure. Для приведенных ниже полезных данных требуется получить определение указанной пользователем функции по умолчанию для скалярной функции, которая привязана к конечной точке машинного обучения Azure. Фактическая конечная точка не указывается, так как она задана во время запроса PUT. Stream Analytics вызывает указанную в запросе конечную точку, если она задана явно. В противном случае будет использоваться конечная точка, указанная изначально. Здесь определяемая пользователем функция (UDF) принимает один строковой параметр (предложение) и возвращает один вывод строкового типа, который указывает на метку "тональность" для этого предложения.

```
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
```

Пример тела запроса:

```json
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
```

Вывод будет выглядеть примерно так, как показано ниже.

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="patch-udf-with-the-response"></a>Исправление для определяемой пользователем функции с помощью ответа
Теперь необходимо исправить определяемую пользователем функцию с использованием предыдущего ответа, как показано ниже.

```
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

Текст запроса (вывод из RetrieveDefaultDefinition):

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Реализация преобразования Stream Analytics для вызова определяемой пользователем функции
Теперь выполните запрос к определяемой пользователем функции (здесь с именем scoreTweet) для каждого входного события и запишите ответ для этого события в вывод.

```json
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
```


## <a name="get-help"></a>Получение справки
Дополнительную помощь и поддержку вы можете получить на нашем [форуме Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Дальнейшие действия
* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
