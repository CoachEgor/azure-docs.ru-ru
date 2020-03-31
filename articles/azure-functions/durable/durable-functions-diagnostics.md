---
title: Диагностика в устойчивых функциях — Azure
description: Сведения о том, как диагностировать неполадки в расширении устойчивых функций для Функций Azure.
author: cgillum
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 4cb832f8fe11ac2581e97d9cdcc777eaff702ee9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278198"
---
# <a name="diagnostics-in-durable-functions-in-azure"></a>Диагностика в Устойчивых функциях в Azure

Существует несколько способов диагностики неполадок в [устойчивых функциях](durable-functions-overview.md). Некоторые из этих вариантов такие же, как и для обычных функций, а некоторые уникальны для устойчивых функций.

## <a name="application-insights"></a>Application Insights

[Application Insights](../../azure-monitor/app/app-insights-overview.md) является рекомендуемым средством для выполнения диагностики и мониторинга в Функциях Azure. То же относится и к устойчивым функциям. Общие сведения об использовании Application Insights в приложении-функции см. в статье [Мониторинг Функций Azure](../functions-monitoring.md).

Расширение устойчивых функций в службе "Функции Azure" также генерирует *события отслеживания* для выполнения трассировки на протяжении всего процесса оркестрации. Эти события отслеживания можно найти и задать с помощью инструмента [Analytics Application Insights на](../../azure-monitor/app/analytics.md) портале Azure.

### <a name="tracking-data"></a>Данные отслеживания

Каждое событие жизненного цикла экземпляра оркестрации инициирует запись события отслеживания в коллекцию **трассировки** в Application Insights. Это событие содержит полезные данные **customDimensions** с несколькими полями.  Все имена полей указаны с префиксом `prop__`.

* **hubName.** Имя центра задач, в котором выполняется оркестрация.
* **appName.** Имя приложения-функции. Это поле полезно, когда у вас есть несколько функциональных приложений, совместно используемых в одном и том же экземпляре Application Insights.
* **slotName.**[Слот развертывания](../functions-deployment-slots.md), в котором выполняется текущее приложение-функция. Это поле полезно, когда вы используете слоты развертывания для версии оркестровок.
* **functionName.** Имя оркестратора или функции действия.
* **functionType.** Тип функции, например функция **оркестратора** или **действия**.
* **InstanceId.** Уникальный идентификатор экземпляра оркестрации.
* **state.** Состояние выполнения жизненного цикла экземпляра. Допустимые значения:
  * **Scheduled.** Выполнение функции было запланировано, но еще не началось.
  * **Started.** Функция запущена, но еще не находится в состоянии ожидания и не была завершена.
  * **Awaited.** Оркестратор запланировал некоторые действия и ожидает их завершения.
  * **Listening.** Оркестратор прослушивает уведомления о внешних событиях.
  * **Completed.** Функция успешно выполнена.
  * **Failed.** Выполнение функции завершилось ошибкой.
* **reason.** Дополнительные данные, связанные с событием отслеживания. Например, если экземпляр ожидает уведомления о внешних событиях, это поле указывает имя события, которое он ожидает. Если функция не сработала, это поле будет содержать сведения об ошибке.
* **isReplay.** Логическое значение, указывающее, следует ли повторно выполнять событие отслеживания.
* **extensionVersion.** Версия расширения устойчивых задач. Информация о версии является особенно важной для сообщения о возможных ошибках в расширении. Долго выполняющиеся экземпляры могут сообщать о нескольких версиях, если возникает обновление при их выполнении.
* **sequenceNumber**: порядковый номер выполнения события. В сочетании с меткой времени позволяет упорядочить события по времени выполнения. *Обратите внимание на то, что это число будет сброшено до нуля при перезапуске узла, когда экземпляр работает. Поэтому важно всегда сначала сортировать по метке времени, а затем по значению sequenceNumber.*

Многословность данных отслеживания, испускаемых в Application `logger` Insights, может быть `logging` настроена в разделе (Функции 1.x) или (Функции 2.0) файла. `host.json`

#### <a name="functions-10"></a>Функции 1.0

```json
{
    "logger": {
        "categoryFilter": {
            "categoryLevels": {
                "Host.Triggers.DurableTask": "Information"
            }
        }
    }
}
```

#### <a name="functions-20"></a>Функции 2.0

```json
{
    "logging": {
        "logLevel": {
          "Host.Triggers.DurableTask": "Information",
        },
    }
}
```

По умолчанию передаются все события отслеживания без ответа. Объем данных можно уменьшить, задав для `Host.Triggers.DurableTask` значение `"Warning"` или `"Error"`. В этом случае события отслеживания будут передаваться в исключительных ситуациях.

Чтобы включить создание событий воспроизведения для оркестрации в режиме подробного протоколирования, вы можете задать для `LogReplayEvents` значение `true` в разделе `durableTask` файла `host.json`, как показано ниже:

#### <a name="functions-10"></a>Функции 1.0

```json
{
    "durableTask": {
        "logReplayEvents": true
    }
}
```

#### <a name="functions-20"></a>Функции 2.0

```javascript
{
    "extensions": {
        "durableTask": {
            "logReplayEvents": true
        }
    }
}
```

> [!NOTE]
> По умолчанию телеметрия Application Insights осуществляется средой выполнения Функций Azure, чтобы избежать частой передачи данных. Это может привести к потере сведений об отслеживании, если много событий жизненного цикла происходят за короткий период времени. Сведения о настройке этого поведения см. в разделе [Настройка выборки](../functions-monitoring.md#configure-sampling).

### <a name="single-instance-query"></a>Одноэкземплярный запрос

Указанный ниже запрос содержит данные отслеживания журнала для одного экземпляра оркестрации функции [последовательности Hello](durable-functions-sequence.md). Он написан с помощью [языка запросов Application Insights (AIQL)](https://aka.ms/LogAnalyticsLanguageReference). Этот экземпляр фильтрует выполнение воспроизведения, поэтому показан только *логический* путь выполнения. События могут быть упорядочены путем сортировки по `timestamp` и `sequenceNumber`, как показано в следующем запросе.

```AIQL
let targetInstanceId = "ddd1aaa685034059b545eb004b15d4eb";
let start = datetime(2018-03-25T09:20:00);
traces
| where timestamp > start and timestamp < start + 30m
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = customDimensions["prop__functionName"]
| extend instanceId = customDimensions["prop__instanceId"]
| extend state = customDimensions["prop__state"]
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend sequenceNumber = tolong(customDimensions["prop__sequenceNumber"])
| where isReplay != true
| where instanceId == targetInstanceId
| sort by timestamp asc, sequenceNumber asc
| project timestamp, functionName, state, instanceId, sequenceNumber, appName = cloud_RoleName
```

Результатом является список событий отслеживания, который отображает путь выполнения оркестрации, включая любые функции действий, упорядоченные по времени выполнения в возрастающем порядке.

![Запрос Application Insights](./media/durable-functions-diagnostics/app-insights-single-instance-ordered-query.png)

### <a name="instance-summary-query"></a>Сводный запрос экземпляров

Следующий запрос отображает состояние всех экземпляров оркестрации, выполненных в указанном диапазоне времени.

```AIQL
let start = datetime(2017-09-30T04:30:00);
traces
| where timestamp > start and timestamp < start + 1h
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = tostring(customDimensions["prop__functionName"])
| extend instanceId = tostring(customDimensions["prop__instanceId"])
| extend state = tostring(customDimensions["prop__state"])
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend output = tostring(customDimensions["prop__output"])
| where isReplay != true
| summarize arg_max(timestamp, *) by instanceId
| project timestamp, instanceId, functionName, state, output, appName = cloud_RoleName
| order by timestamp asc
```

Результатом является список идентификаторов экземпляров и их текущее состояние выполнения.

![Запрос Application Insights](./media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Ведение журнала

Очень важно помнить о поведении воспроизведения оркестратора при записи журналов непосредственно из функции оркестратора. Например, рассмотрим следующую функцию оркестратора:

### <a name="precompiled-c"></a>Предкомпилированный код C#

```csharp
[FunctionName("FunctionChain")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context,
    ILogger log)
{
    log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

### <a name="c-script"></a>Скрипт C#

```csharp
public static async Task Run(
    IDurableOrchestrationContext context,
    ILogger log)
{
    log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

### <a name="javascript-functions-20-only"></a>JavaScript (только Функции 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    context.log("Calling F1.");
    yield context.df.callActivity("F1");
    context.log("Calling F2.");
    yield context.df.callActivity("F2");
    context.log("Calling F3.");
    yield context.df.callActivity("F3");
    context.log("Done!");
});
```

Полученные данные журнала будут выглядеть примерно на следующем выходе из следующего примера:

```txt
Calling F1.
Calling F1.
Calling F2.
Calling F1.
Calling F2.
Calling F3.
Calling F1.
Calling F2.
Calling F3.
Done!
```

> [!NOTE]
> Помните, что хотя в журналах указаны вызовы F1, F2 и F3, эти функции *фактически* вызываются, только когда они встречаются впервые. Последующие вызовы, которые происходят во время воспроизведения, пропускаются, и выходные данные воспроизводятся в логике оркестратора.

Если вы хотите протоколировать только выполнение без воспроизведения, можно написать логическое выражение, чтобы вносить запись, только когда `IsReplaying` имеет значение `false`. Рассмотрим указанный выше пример, но на этот раз с проверкой воспроизведения.

#### <a name="precompiled-c"></a>Предкомпилированный код C#

```csharp
[FunctionName("FunctionChain")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context,
    ILogger log)
{
    if (!context.IsReplaying) log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    if (!context.IsReplaying) log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    if (!context.IsReplaying) log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

#### <a name="c"></a>C#

```cs
public static async Task Run(
    IDurableOrchestrationContext context,
    ILogger log)
{
    if (!context.IsReplaying) log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    if (!context.IsReplaying) log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    if (!context.IsReplaying) log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

#### <a name="javascript-functions-20-only"></a>JavaScript (только Функции 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    if (!context.df.isReplaying) context.log("Calling F1.");
    yield context.df.callActivity("F1");
    if (!context.df.isReplaying) context.log("Calling F2.");
    yield context.df.callActivity("F2");
    if (!context.df.isReplaying) context.log("Calling F3.");
    yield context.df.callActivity("F3");
    context.log("Done!");
});
```

Начиная с функций Durable 2.0, функции оркестратора `ILogger` .NET также имеют возможность создавать, что автоматически отфильтровывает выписки из журнала во время воспроизведения. Эта автоматическая фильтрация `IDurableOrchestrationContext.CreateReplaySafeLogger(ILogger)` осуществляется с помощью API.

```csharp
[FunctionName("FunctionChain")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context,
    ILogger log)
{
    log = context.CreateReplaySafeLogger(log);
    log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

С ранее упомянутыми изменениями вывод журнала заключается в следующем:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

> [!NOTE]
> Предыдущие примеры СЗ относятся к долгосрочным функциям 2.x. Для долгосрочных функций 1.x необходимо использовать `DurableOrchestrationContext` вместо `IDurableOrchestrationContext`. Для получения дополнительной информации о [Durable Functions versions](durable-functions-versions.md) различиях между версиями см.

## <a name="custom-status"></a>Настраиваемое значение состояния

Эта возможность позволяет задать настраиваемое значение состояния для функции оркестратора. Такое значение состояния можно задать с помощью API HTTP-запросов состояния или API `IDurableOrchestrationClient.GetStatusAsync`. Настраиваемое состояние оркестрации предоставляет дополнительные возможности мониторинга для функций оркестратора. Например, в код функции оркестратора можно включить вызовы `IDurableOrchestrationContext.SetCustomStatus`, чтобы обновлять ход выполнения длительной операции. Клиент, например веб-страница или другая внешняя система, может периодически отправлять запрос в API HTTP-запросов состояния, чтобы получить расширенные сведения о ходе выполнения. Пример использования `IDurableOrchestrationContext.SetCustomStatus` приведен ниже.

### <a name="precompiled-c"></a>Предкомпилированный код C#

```csharp
[FunctionName("SetStatusTest")]
public static async Task SetStatusTest([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { completionPercentage = 90.0, status = "Updating database records" };
    context.SetCustomStatus(customStatus);

    // ...do more work...
}
```

> [!NOTE]
> Предыдущий пример сC —для прочных функций 2.x. Для долгосрочных функций 1.x необходимо использовать `DurableOrchestrationContext` вместо `IDurableOrchestrationContext`. Для получения дополнительной информации о [Durable Functions versions](durable-functions-versions.md) различиях между версиями см.

### <a name="javascript-functions-20-only"></a>JavaScript (только Функции 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    const customStatus = { completionPercentage: 90.0, status: "Updating database records", };
    context.df.setCustomStatus(customStatus);

    // ...do more work...
});
```

Когда выполняется оркестрация, внешние клиенты могут получить данные о таком настраиваемом значении состояния:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

Они получат следующий ответ:

```http
{
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": { "completionPercentage": 90.0, "status": "Updating database records" },
  "output": null,
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T19:40:30Z"
}
```

> [!WARNING]
> Полезные данные настраиваемого значения состояния не должны превышать 16 КБ JSON-текста в кодировке UTF-16, так как они должны поместиться в столбец Хранилища таблиц Azure. Для полезных данных большего размера можно использовать внешнее хранилище.

## <a name="debugging"></a>Отладка

Функции Azure поддерживают сценарии отладки кода функции напрямую. Та же поддержка реализуется в устойчивых функциях, работающих в Azure или локально. Однако следует помнить о некоторых особенностях поведения при выполнении отладки.

* **Воспроизведение**: Функции оркестратора регулярно [воспроизводить,](durable-functions-orchestrations.md#reliability) когда новые входы получены. Такое поведение означает, что одно *логическое* выполнение функции оркестратора может привести к многократному нажатию одной и той же точки разрыва, особенно если она установлена на ранних стадиях кода функции.
* **Ожидайте**: Всякий раз, когда возникает `await` функция оркестратора, он дает контроль обратно диспетчеру с постоянной целевой системой. Если это первый раз, когда конкретное `await` столкнулось, связанная задача *никогда не* возобновляется. Поскольку задача никогда не возобновляется, переступить *через* ожидание (F10 в Visual Studio) не представляется возможным. Вы можете обойти это действие, только когда задание воспроизводится.
* **Время отправления сообщений:** Долгосрочные функции внутренне используют сообщения очереди для управления выполнением функций оркестратора, деятельности и сущности. В среде со множеством виртуальных машин прерывание отладки на длительное время может привести к тому, что другая виртуальная машина получит сообщение, из-за чего произойдет дублирующее выполнение. Это поведение также существует для обычных функций триггера очереди, но его очень важно отметить в этом контексте, так как очереди относятся к тонкостям реализации.
* **Остановка и запуск**: Сообщения в функции длительного сохраняются между сеансами отладки. Если вы прекратите отладку и прекратите процесс локального узла во время выполнения функции прочного, эта функция может автоматически выполняться в будущем сеансе отладки. Такое поведение может ввести в заблуждение, когда не ожидается. Очистка всех сообщений от [внутренних очередей хранения](durable-functions-perf-and-scale.md#internal-queue-triggers) между сеансами отладки является одним из методов, чтобы избежать такого поведения.

> [!TIP]
> При настройке моментов разрыва в функциях оркестратора, если вы хотите только перерыв на не-повторвыполнения выполнения, вы можете установить условную точку разрыва, которая ломается только в том случае, если `IsReplaying` это `false`.

## <a name="storage"></a>Хранилище

По умолчанию устойчивые функции хранят состояние в службе хранилища Azure. Такое поведение означает, что вы можете проверить состояние ваших оркестровок с помощью таких инструментов, как [Microsoft Azure Storage Explorer.](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)

![Скриншот Azure Storage Explorer](./media/durable-functions-diagnostics/storage-explorer.png)

Это полезно для отладки, так как вы можете увидеть точное состояние оркестрации. Сообщения в очередях можно также проверить, чтобы узнать, какое действие находится в состоянии ожидания (или в некоторых случаях могло зависнуть).

> [!WARNING]
> Хотя просматривать журнал выполнения в службе таблиц более удобно, избегайте использования зависимостей в таблице. Они могут измениться при развитии расширения устойчивых функций.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Подробнее о мониторинге в функциях Azure](../functions-monitoring.md)
