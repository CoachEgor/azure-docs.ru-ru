---
title: включить файл
description: включить файл
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/14/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 284aad7dd5b51268b1c8ff8a02f4489d6f1cd3d9
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279449"
---
Параметры конфигурации для [устойчивых функций](../articles/azure-functions/durable-functions-overview.md).

### <a name="durable-functions-1x"></a>Устойчивые функции 1. x

```json
{
  "durableTask": {
    "hubName": "MyTaskHub",
    "controlQueueBatchSize": 32,
    "partitionCount": 4,
    "controlQueueVisibilityTimeout": "00:05:00",
    "workItemQueueVisibilityTimeout": "00:05:00",
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
    "maxQueuePollingInterval": "00:00:30",
    "azureStorageConnectionStringName": "AzureWebJobsStorage",
    "trackingStoreConnectionStringName": "TrackingStorage",
    "trackingStoreNamePrefix": "DurableTask",
    "traceInputsAndOutputs": false,
    "logReplayEvents": false,
    "eventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "eventGridKeySettingName":  "EventGridKey",
    "eventGridPublishRetryCount": 3,
    "eventGridPublishRetryInterval": "00:00:30",
    "eventGridPublishEventTypes": ["Started", "Completed", "Failed", "Terminated"]
  }
}
```

### <a name="durable-functions-2-0-host-json"></a>Устойчивые функции 2. x

```json
{
  "durableTask": {
    "hubName": "MyTaskHub",
    "storageProvider": {
      "connectionStringName": "AzureWebJobsStorage",
      "controlQueueBatchSize": 32,
      "controlQueueBufferThreshold": 256,
      "controlQueueVisibilityTimeout": "00:05:00",
      "maxQueuePollingInterval": "00:00:30",
      "partitionCount": 4,
      "trackingStoreConnectionStringName": "TrackingStorage",
      "trackingStoreNamePrefix": "DurableTask",
      "workItemQueueVisibilityTimeout": "00:05:00",
    },
    "tracing": {
      "traceInputsAndOutputs": false,
      "traceReplayEvents": false,
    },
    "notifications": {
      "eventGrid": {
        "topicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
        "keySettingName": "EventGridKey",
        "publishRetryCount": 3,
        "publishRetryInterval": "00:00:30",
        "publishEventTypes": [
          "Started",
          "Pending",
          "Failed",
          "Terminated"
        ]
      }
    },
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
    "extendedSessionsEnabled": false,
    "extendedSessionIdleTimeoutInSeconds": 30,
    "useGracefulShutdown": false
  }
}
```

Имена центров задач должны начинаться с буквы и содержать только буквы и цифры. Если имя не указано, центру задач в приложении-функции назначается имя по умолчанию **DurableFunctionsHub**. Дополнительные сведения см. в статье о [центрах задач](../articles/azure-functions/durable-functions-task-hubs.md).

|Свойство  |По умолчанию | Description |
|---------|---------|---------|
|hubName|DurableFunctionsHub|Альтернативные имена [центра задач](../articles/azure-functions/durable-functions-task-hubs.md) позволяют изолировать несколько приложений устойчивых функций друг от друга, даже если они используют один и тот же интерфейс хранилища.|
|контролкуеуебатчсизе|32|Количество сообщений, одновременно извлекаемых из очереди управления.|
|контролкуеуебуфферсрешолд|256|Количество сообщений очереди управления, которые могут быть помещены в память за один раз, после чего диспетчер будет ожидать перед отправкой дополнительных сообщений.|
|partitionCount |4|Число разделов для очереди управления. Допускается целочисленное значение в диапазоне от 1 до 16.|
|контролкуеуевисибилититимеаут |5 мин|Время видимости для сообщений, выведенных из очереди управления.|
|воркитемкуеуевисибилититимеаут |5 мин|Время видимости для сообщений, выведенных из очереди рабочих элементов.|
|максконкуррентактивитифунктионс |10× количество процессоров на текущем компьютере|Максимальное число функции действия, которые могут параллельно обрабатываться на одном экземпляре узла.|
|максконкурренторчестраторфунктионс |10× количество процессоров на текущем компьютере|Максимальное количество функций оркестратора, которые могут быть обработаны параллельно в одном экземпляре узла.|
|макскуеуеполлингинтервал|30 секунд|Максимальный интервал опроса управления и очереди рабочих элементов в формате *чч: мм: СС* . Более высокие значения могут привести к увеличению задержек при обработке сообщений. Более низкие значения могут привести к более высоким затратам на хранение из-за увеличенных транзакций хранилища.|
|азуресторажеконнектионстрингнаме |AzureWebJobsStorage|Имя параметра приложения, в котором хранится строка подключения к службе хранилища Azure для управления базовыми ресурсами этой службы.|
|траккингстореконнектионстрингнаме||Имя строки подключения, используемой для таблиц журнала и экземпляров. Если не указано, используется подключение `azureStorageConnectionStringName`.|
|траккингсторенамепрефикс||Префикс, используемый для таблиц журнала и экземпляров при указании `trackingStoreConnectionStringName`. Если значение не задано, префикс по умолчанию будет `DurableTask`. Если `trackingStoreConnectionStringName` не указан, то таблицы журнала и экземпляров будут использовать в качестве префикса значение `hubName`, а любой параметр для `trackingStoreNamePrefix` будет проигнорирован.|
|трацеинпутсандаутпутс |false|Это значение указывает, нужно ли отслеживать входы и выходы вызовов функций. При трассировке событий выполнения функции по умолчанию для вызовов функций фиксируется количество байтов в сериализованных входных и выходных данных. Это поведение предоставляет минимальные сведения о том, как выглядят входные и выходные данные без чрезмерного перекрытия журналов или непреднамеренного предоставления конфиденциальной информации. Если вы присвоите этому свойству значение true, в журналы выполнения функций будет включаться полное содержимое их входов и выходов.|
|логреплайевентс|false|Значение, указывающее, следует ли записывать повторные события оркестрации в Application Insights.|
|евентгридтопицендпоинт ||URL-адрес конечной точки пользовательского раздела службы "Сетка событий Azure". Если это свойство задано, события уведомления о жизненном цикле оркестрации публикуются в этой конечной точке. Это свойство поддерживает разрешение параметров приложения.|
|евентгридкэйсеттингнаме ||Имя параметра приложения, содержащего ключ для аутентификации в пользовательском разделе службы "Сетка событий Azure" в `EventGridTopicEndpoint`.|
|евентгридпублишретрикаунт|0|Число повторных попыток, если публикация в разделе "Сетка событий" завершается сбоем.|
|евентгридпублишретринтервал|5 мин|Интервал повторных попыток для публикации в разделе "Сетка событий" указывается в формате *чч: мм:сс*.|
|евентгридпублишевенттипес||Список типов событий для публикации в сетке событий. Если не указано, будут опубликованы все типы событий. Допустимые значения: `Started`, `Completed`, `Failed`, `Terminated`.|
|усеграцефулшутдовн|false|Образца Включите корректное завершение работы, чтобы снизить вероятность того, что при завершении работы функции не удается завершить работу.|

Многие из этих параметров предназначены для оптимизации производительности. Дополнительные сведения см. в статье [о производительности и масштабируемости](../articles/azure-functions/durable-functions-perf-and-scale.md).
