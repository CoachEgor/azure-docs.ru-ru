---
title: Публикации устойчивых функций в службе "Сетка событий Azure" (предварительная версия)
description: Узнайте, как настроить автоматическую публикацию в службе "Сетка событий Azure" для устойчивых функций.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: glenga
ms.openlocfilehash: c07a42349fbd81a46b1b7cd9bcad1978f891a6b2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60733771"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Публикации устойчивых функций в службе "Сетка событий Azure" (предварительная версия)

В этой статье показано, как настроить Устойчивые функции для публикации событий жизненного цикла оркестрации (например, события создания, завершения и сбоя) в пользовательском [разделе Сетки событий Azure](https://docs.microsoft.com/azure/event-grid/overview).

Ниже приведены некоторые сценарии, где эта функция полезна:

* **Сценарии DevOps, такие как "синие" или "зеленые" развертывания**. Вам может потребоваться определить, выполняются ли какие-то задачи, перед реализацией [стратегии параллельного развертывания](durable-functions-versioning.md#side-by-side-deployments).

* **Поддержка расширенного мониторинга и диагностики**. Вы можете отслеживать сведения о состоянии оркестрации во внешнем хранилище, оптимизированном для запросов, например, базы данных SQL или Cosmos DB.

* **Продолжительное фоновое действие**. При использовании Устойчивых функций для продолжительного фонового действия эта функция позволяет вам узнать текущее состояние.

## <a name="prerequisites"></a>Технические условия

* Установите [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 1.3.0-rc или более поздней версии в проекте устойчивых функций.
* Установите [эмулятор хранения Azure](https://docs.microsoft.com/azure/storage/common/storage-use-emulator).
* Установите [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) или используйте [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

## <a name="create-a-custom-event-grid-topic"></a>Создание раздела сетки пользовательское событие

Создайте раздел сетки событий для отправки событий из устойчивых функций. Ниже показано, как создать раздел с помощью Azure CLI. Сведения о том, как сделать это с помощью PowerShell или портала Azure, см. в следующих статьях:

* [Краткие руководства по EventGrid: создание пользовательского события с помощью PowerShell](https://docs.microsoft.com/azure/event-grid/custom-event-quickstart-powershell)
* [Краткие руководства по EventGrid: создание пользовательского события с помощью портала Azure](https://docs.microsoft.com/azure/event-grid/custom-event-quickstart-portal)

### <a name="create-a-resource-group"></a>Создание группы ресурсов

Создайте группу ресурсов с помощью команды `az group create`. В настоящее время "Сетка событий Azure" не поддерживает все регионы. Сведения о поддерживаемых регионах см. в разделе ["Сетка событий Azure" Обзор](https://docs.microsoft.com/azure/event-grid/overview).

```bash
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Создание пользовательской темы

Раздел сетки событий содержит определяемую пользователем конечную точку, которой можно размещать вашему событию. Замените `<topic_name>` уникальным именем для вашей темы. Имя раздела должно быть уникальным, так как оно становится записью службы доменных имен (DNS).

```bash
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup
```

## <a name="get-the-endpoint-and-key"></a>Получение конечной точки и ключа

Получите конечную точку раздела. Замените `<topic_name>` на выбранное имя.

```bash
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Получите ключ раздела. Замените `<topic_name>` на выбранное имя.

```bash
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Теперь можно отправлять события в раздел.

## <a name="configure-azure-event-grid-publishing"></a>Настройка публикации в службе "Сетка событий Azure"

Найдите файл `host.json` в вашем проекте устойчивых функций.

Добавьте `eventGridTopicEndpoint` и `eventGridKeySettingName` в свойство `durableTask`.

```json
{
    "durableTask": {
        "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "eventGridKeySettingName": "EventGridKey"
    }
}
```

Возможные свойства конфигурации "Сетка событий Azure" можно найти в [документации по host.json](../functions-host-json.md#durabletask). После настройки `host.json` файл, ваше приложение-функция отправляет события жизненного цикла для раздела сетки событий. Это работает при запуске приложения-функции, как локально, так и в Azure.'' "

Установите параметр приложения для ключа раздела в приложении-функции и `local.setting.json`. Следующий код JSON является примером `local.settings.json` для локальной отладки. Замените `<topic_key>` на ключ раздела.  

```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
        "EventGridKey": "<topic_key>"
    }
}
```

Убедитесь, что [эмулятор хранилища](https://docs.microsoft.com/azure/storage/common/storage-use-emulator) работает. Рекомендуется запустить команду `AzureStorageEmulator.exe clear all` перед выполнением.

## <a name="create-functions-that-listen-for-events"></a>Создание функций, которые прослушивают события

Создайте приложение-функцию. Лучше найти его в том же регионе, что раздел сетки событий.

### <a name="create-an-event-grid-trigger-function"></a>Создание функции триггера сетки событий

Создайте функцию для получения событий жизненного цикла. Выберите **Пользовательская функция**.

![Выберите создание пользовательской функции.](./media/durable-functions-event-publishing/functions-portal.png)

Выберите триггер сетки событий, а затем щелкните `C#`.

![Выберите триггер сетки событий.](./media/durable-functions-event-publishing/eventgrid-trigger.png)

Введите имя функции, а затем выберите `Create`.

![Создайте триггер сетки событий.](./media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Будет создана функция со следующим кодом:

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

public static void Run(JObject eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
}
```

Выберите `Add Event Grid Subscription`. Эта операция Добавляет подписку на событие сетка для раздела сетки событий, который вы создали. Дополнительные сведения см. в разделе [Основные понятия в службе "Сетка событий Azure"](https://docs.microsoft.com/azure/event-grid/concepts).

![Выберите ссылку на триггер сетки событий.](./media/durable-functions-event-publishing/eventgrid-trigger-link.png)

Выберите `Event Grid Topics` в качестве **типа раздела**. Выберите группу ресурсов, созданную для раздела сетки событий. Затем выберите экземпляр раздела сетки событий. Нажмите кнопку `Create`.

![создание подписки в службе "Сетка событий";](./media/durable-functions-event-publishing/eventsubscription.png)

Теперь все готово для получения событий жизненного цикла.

## <a name="create-durable-functions-to-send-the-events"></a>Создание Устойчивых функций для отправки событий

В проекте устойчивых функций начните отладку на локальном компьютере.  Следующий код совпадает с кодом шаблона для устойчивых функций. Вы уже настроили `host.json` и `local.settings.json` на локальном компьютере.

```csharp
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace LifeCycleEventSpike
{
    public static class Sample
    {
        [FunctionName("Sample")]
        public static async Task<List<string>> RunOrchestrator(
            [OrchestrationTrigger] DurableOrchestrationContext context)
        {
            var outputs = new List<string>();

            // Replace "hello" with the name of your Durable Activity Function.
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Tokyo"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Seattle"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "London"));

            // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
            return outputs;
        }

        [FunctionName("Sample_Hello")]
        public static string SayHello([ActivityTrigger] string name, ILogger log)
        {
            log.LogInformation($"Saying hello to {name}.");
            return $"Hello {name}!";
        }

        [FunctionName("Sample_HttpStart")]
        public static async Task<HttpResponseMessage> HttpStart(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
            [OrchestrationClient]DurableOrchestrationClient starter,
            ILogger log)
        {
            // Function input comes from the request content.
            string instanceId = await starter.StartNewAsync("Sample", null);
            log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

Устойчивые функции начнут отправлять события жизненного цикла, если вы вызовите `Sample_HttpStart` с помощью Postman или браузера. Конечная точка для локальной отладки обычно такая: `http://localhost:7071/api/Sample_HttpStart`.

Просмотрите журналы из функции, которую вы создали на портале Azure.

```
2018-04-20T09:28:21.041 [Info] Function started (Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d)
2018-04-20T09:28:21.104 [Info] {
    "id": "054fe385-c017-4ce3-b38a-052ac970c39d",
    "subject": "durable/orchestrator/Running",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Running"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:19.6492068Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}

2018-04-20T09:28:21.104 [Info] Function completed (Success, Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d, Duration=65ms)
2018-04-20T09:28:37.098 [Info] Function started (Id=36fadea5-198b-4345-bb8e-2837febb89a2)
2018-04-20T09:28:37.098 [Info] {
    "id": "8cf17246-fa9c-4dad-b32a-5a868104f17b",
    "subject": "durable/orchestrator/Completed",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Completed"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2018-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>Схема событий

В приведенном ниже списке описана схема событий жизненного цикла:

* **`id`** . Уникальный идентификатор для события сетки событий.
* **`subject`** . Путь к субъекту событий. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}` будет `Running`, `Completed`, `Failed` и `Terminated`.  
* **`data`** . Определенные параметры Устойчивых функций.
  * **`hubName`** . Имя [TaskHub](durable-functions-task-hubs.md).
  * **`functionName`** . Имя функции оркестратора.
  * **`instanceId`** . Идентификатор экземпляра Устойчивых функций.
  * **`reason`** . Дополнительные данные, связанные с событием отслеживания. Дополнительные сведения см. в статье [Диагностика в устойчивых функциях (Функции Azure)](durable-functions-diagnostics.md).
  * **`runtimeStatus`** . Состояние среды выполнения оркестрации. "Running", "Completed", "Failed", "Canceled".
* **`eventType`** : «orchestratorEvent»
* **`eventTime`** . Время события (UTC).
* **`dataVersion`** . Версия схемы события жизненного цикла.
* **`metadataVersion`** .  Версия метаданных.
* **`topic`** . Ресурс раздела сетки событий.

## <a name="how-to-test-locally"></a>Локальное тестирование

Чтобы выполнить тестирование локально, используйте [ngrok](../functions-bindings-event-grid.md#local-testing-with-ngrok).

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Управление экземплярами в устойчивых функциях (Функции Azure)](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Управление версиями в устойчивых функциях (Функции Azure)](durable-functions-versioning.md)
