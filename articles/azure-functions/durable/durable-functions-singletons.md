---
title: Одноэлементные экземпляры в устойчивых функциях — Azure
description: Инструкции по использованию одноэлементных экземпляров в расширении "Устойчивые функции" для Функций Azure.
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 4eff7c4c91ed664fcf1f4fc7a8be2d43d24e5c6b
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76262815"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Одноэлементные экземпляры в устойчивых функциях (Функции Azure)

Для фоновых заданий часто требуется обеспечить, чтобы в каждый момент времени выполнялся только один экземпляр определенной Orchestrator. Этот тип одноэлементного поведения можно обеспечить в [устойчивые функции](durable-functions-overview.md) , назначив идентификатору экземпляра Orchestrator при его создании.

## <a name="singleton-example"></a>Пример одноэлементного экземпляра

В следующем примере показана функция триггера HTTP, которая создает одноэлементное согласование фонового задания. Код обеспечивает наличие только одного экземпляра с указанным идентификатором.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

> [!NOTE]
> Предыдущий C# код предназначен для устойчивые функции 2. x. Для Устойчивые функции 1. x необходимо использовать атрибут `OrchestrationClient` вместо атрибута `DurableClient`, а вместо `IDurableOrchestrationClient`необходимо использовать тип параметра `DurableOrchestrationClient`. Дополнительные сведения о различиях между версиями см. в статье [устойчивые функции версии](durable-functions-versions.md) .

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

**function.json**

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}/{instanceId}",
      "methods": ["post"]
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

**index.js**

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionName;

    // Check if an instance with the specified ID already exists.
    const existingInstance = await client.getStatus(instanceId);
    if (!existingInstance) {
        // An instance with the specified ID doesn't exist, create one.
        const eventData = req.body;
        await client.startNew(functionName, instanceId, eventData);
        context.log(`Started orchestration with ID = '${instanceId}'.`);
        return client.createCheckStatusResponse(req, instanceId);
    } else {
        // An instance with the specified ID exists, don't create one.
        return {
            status: 409,
            body: `An instance with ID '${instanceId}' already exists.`,
        };
    }
};
```

---

По умолчанию идентификаторы экземпляров — это случайным образом сгенерированные GUID. Однако в предыдущем примере идентификатор экземпляра передается в данные маршрута из URL-адреса. Код вызывает `GetStatusAsync`(C#) или `getStatus` (JavaScript), чтобы проверить, запущен ли уже экземпляр с указанным идентификатором. Если такой экземпляр не работает, создается новый экземпляр с этим ИДЕНТИФИКАТОРом.

> [!NOTE]
> В этом примере содержится потенциальное состояние гонки. Если два экземпляра **HttpStartSingle** выполняются параллельно, оба вызова функции сообщат об успешном выполнении, но фактически запустится только один экземпляр оркестрации. В зависимости от применяемых требований это может привести к нежелательным побочным эффектам. Поэтому важно, чтобы два запроса не выполняли одновременно эту функцию триггера.

Сведения о реализации функции Orchestrator не имеют значения. Это может быть обычная функция оркестратора, которая начинает и завершает работу, или же выполняющаяся бесконечно (т. е. [вечная оркестрация](durable-functions-eternal-orchestrations.md)). Важно то, что в любой момент времени выполняется только один экземпляр.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Сведения о собственных функциях HTTP оркестрации](durable-functions-http-features.md)
