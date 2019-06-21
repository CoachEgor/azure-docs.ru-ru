---
title: включение файла
description: включение файла
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 03/14/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d79d1bd5ec244ad4399a02c349e2504516d06ccd
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185856"
---
Параметры конфигурации для [устойчивых функций](../articles/azure-functions/durable-functions-overview.md).

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
    "eventGridPublishEventTypes": ["Started", "Pending", "Failed", "Terminated"]
  }
}
```

Имена центров задач должны начинаться с буквы и содержать только буквы и цифры. Если имя не указано, центру задач в приложении-функции назначается имя по умолчанию **DurableFunctionsHub**. Дополнительные сведения см. в статье о [центрах задач](../articles/azure-functions/durable-functions-task-hubs.md).

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------|
|hubName|DurableFunctionsHub|Альтернативные имена [центра задач](../articles/azure-functions/durable-functions-task-hubs.md) позволяют изолировать несколько приложений устойчивых функций друг от друга, даже если они используют один и тот же интерфейс хранилища.|
|controlQueueBatchSize|32|Количество сообщений, одновременно извлекаемых из очереди управления.|
|partitionCount |4\.|Число разделов для очереди управления. Допускается целочисленное значение в диапазоне от 1 до 16.|
|controlQueueVisibilityTimeout |5 мин|Время видимости для сообщений, выведенных из очереди управления.|
|workItemQueueVisibilityTimeout |5 мин|Время видимости для сообщений, выведенных из очереди рабочих элементов.|
|maxConcurrentActivityFunctions |10× количество процессоров на текущем компьютере|Максимальное число функции действия, которые могут параллельно обрабатываться на одном экземпляре узла.|
|maxConcurrentOrchestratorFunctions |10× количество процессоров на текущем компьютере|Максимальное количество функций оркестратора, которые могут быть обработаны параллельно в одном экземпляре узла.|
|maxQueuePollingInterval|30 секунд|Максимальный контроль и интервал опроса очереди рабочих элементов в *чч: мм:* формат. Более высокие значения может привести обработки задержки более поздней версии сообщения. Из-за транзакции с хранилищем, повысить более низкие значения может привести больше затрат на хранение.|
|azureStorageConnectionStringName |AzureWebJobsStorage|Имя параметра приложения, в котором хранится строка подключения к службе хранилища Azure для управления базовыми ресурсами этой службы.|
|trackingStoreConnectionStringName||Имя строки соединения для таблицы журнала и экземпляров. Если не указан, `azureStorageConnectionStringName` используется соединение.|
|trackingStoreNamePrefix||Префикс, используемый для журнала и экземпляров таблицы, если `trackingStoreConnectionStringName` указан. Если не задано, префикс по умолчанию будет `DurableTask`. Если `trackingStoreConnectionStringName` не указан, то будет использовать таблицы журнала и экземпляры `hubName` как префикс и любые параметры `trackingStoreNamePrefix` будет игнорироваться.|
|traceInputsAndOutputs |false|Это значение указывает, нужно ли отслеживать входы и выходы вызовов функций. При трассировке событий выполнения функции по умолчанию для вызовов функций фиксируется количество байтов в сериализованных входных и выходных данных. Такое поведение обеспечивает минимальные сведения о входных и выходных данных выглядеть без раздувания журналы или непреднамеренного раскрытия конфиденциальных сведений. Если вы присвоите этому свойству значение true, в журналы выполнения функций будет включаться полное содержимое их входов и выходов.|
|logReplayEvents|false|Значение, указывающее, следует ли записывать повторные события оркестрации в Application Insights.|
|eventGridTopicEndpoint ||URL-адрес конечной точки пользовательского раздела службы "Сетка событий Azure". Если это свойство имеет значение, события уведомления жизненного цикла оркестрации публикуются в эту конечную точку. Это свойство поддерживает разрешение параметров приложения.|
|eventGridKeySettingName ||Имя параметра приложения, содержащего ключ для аутентификации в пользовательском разделе службы "Сетка событий Azure" в `EventGridTopicEndpoint`.|
|eventGridPublishRetryCount|0|Число повторных попыток, если публикация в разделе "Сетка событий" завершается сбоем.|
|eventGridPublishRetryInterval|5 мин|Интервал повторных попыток для публикации в разделе "Сетка событий" указывается в формате *чч: мм:сс*.|
|eventGridPublishEventTypes||Список типов событий для публикации "Сетка событий". Если не указан, все типы событий будут опубликованы. Разрешены значения включают `Started`, `Completed`, `Failed`, `Terminated`.|

Многие из этих параметров, предназначены для оптимизации производительности. Дополнительные сведения см. в статье [о производительности и масштабируемости](../articles/azure-functions/durable-functions-perf-and-scale.md).