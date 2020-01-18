---
title: Центры задач в устойчивых функциях — Azure
description: Сведения о том, что такое центр задач в расширении устойчивых функций для Функций Azure. Узнайте, как настроить центры задач.
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: ffb3d590aebe80994de1e7e834a2eba5777df9a1
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76262492"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Центры задач в устойчивых функциях (Функции Azure)

*Центр задач* в [устойчивых функциях](durable-functions-overview.md) — это логический контейнер для ресурсов службы хранилища Azure, которые используются для оркестрации. Функции оркестраторов и действий могут взаимодействовать друг с другом, только когда они принадлежат к одному центру задач.

Если несколько приложений-функций совместно используют учетную запись хранения, каждому приложению-функции *необходимо* предоставить отдельное имя центра задач. Учетная запись хранения может содержать несколько центров задач. На следующей схеме показано, что в общей и отдельной учетных записях хранения на одно приложение-функцию приходится один центр задач.

![Схема с общей и отдельной учетными записями хранения.](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Ресурсы хранилища Azure

Центр задач состоит из следующих ресурсов хранилища:

* Одна или несколько очередей элементов управления.
* Одна очередь рабочих элементов.
* Одна таблица журнала.
* одна таблица экземпляров.
* Один контейнер хранилища, содержащий один или несколько арендуемых больших двоичных объектов.
* Контейнер хранилища, содержащий полезные данные больших сообщений, если применимо.

Все эти ресурсы создаются автоматически в учетной записи хранения Azure по умолчанию при выполнении или запланированном запуске функций Orchestrator, Entity или Activity. В статье [Производительность и масштабирование в устойчивых функциях (Функции Azure)](durable-functions-perf-and-scale.md) объясняется, как используются эти ресурсы.

## <a name="task-hub-names"></a>Имена центров задач

Центры задач определяются по имени, объявленном в файле *host.json*, как показано в следующем примере:

### <a name="hostjson-functions-20"></a>Host. JSON (функции 2,0)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "MyTaskHub"
    }
  }
}
```

### <a name="hostjson-functions-1x"></a>host.json (Функции, версия 1.x)

```json
{
  "durableTask": {
    "hubName": "MyTaskHub"
  }
}
```

Концентраторы задач можно также настроить с помощью параметров приложения, как показано в следующем `host.json` примере файла:

### <a name="hostjson-functions-10"></a>Host. JSON (функции 1,0)

```json
{
  "durableTask": {
    "hubName": "%MyTaskHub%"
  }
}
```

### <a name="hostjson-functions-20"></a>Host. JSON (функции 2,0)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "%MyTaskHub%"
    }
  }
}
```

Для имени центра задач будет установлено значение параметра приложения `MyTaskHub`. В следующем файле `local.settings.json` показано, как определить параметр `MyTaskHub` как `samplehubname`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" : "samplehubname"
  }
}
```

В следующем коде показано, как написать функцию, которая использует [привязку клиента оркестрации](durable-functions-bindings.md#orchestration-client) для работы с центром задач, настроенным в качестве параметра приложения:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
    [OrchestrationClient(TaskHub = "%MyTaskHub%")] IDurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

> [!NOTE]
> Предыдущий C# пример — для устойчивые функции 2. x. Для Устойчивые функции 1. x необходимо использовать `DurableOrchestrationContext` вместо `IDurableOrchestrationContext`. Дополнительные сведения о различиях между версиями см. в статье [устойчивые функции версии](durable-functions-versions.md) .

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Свойство центра задач в файле `function.json` задается с помощью параметра приложения:

```json
{
    "name": "input",
    "taskHub": "%MyTaskHub%",
    "type": "orchestrationClient",
    "direction": "in"
}
```

---

Имена центров задач должны начинаться с буквы и содержать только буквы и цифры. Если значение не указано, будет использоваться имя центра задач по умолчанию, как показано в следующей таблице.

| Версия устойчивого расширения | Имя центра задач по умолчанию |
| - | - |
| 2.x | При развертывании в Azure имя центра задач является производным от имени _приложения-функции_. При работе вне Azure имя центра задач по умолчанию — `TestHubName`. |
| 1.x | Имя центра задач по умолчанию для всех окружений — `DurableFunctionsHub`. |

Дополнительные сведения о различиях между версиями расширений см. в статье [устойчивые функции версии](durable-functions-versions.md) .

> [!NOTE]
> Имя — это то, что отличает один центр задач от другого, если в общей учетной записи хранения таких центров несколько. Если общую учетную запись хранения используют несколько приложений-функций, вам нужно задать разные имена для каждого центра задач в файлах *host.json*. В противном случае несколько приложений функций будут конкурировать друг с другом для сообщений, что может привести к неопределенному поведению, в том числе оркестрации в `Pending` или `Running` состоянии.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Узнайте, как управлять управлением версиями оркестрации](durable-functions-versioning.md)
