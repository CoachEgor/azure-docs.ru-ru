---
title: Обработка ошибок в устойчивых функциях — Azure
description: Сведения о том, как обрабатывать ошибки в расширении устойчивых функций для Функций Azure.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 447b3dcf5040835f5a853beff68bde794ece51f5
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371920"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Обработка ошибок в устойчивых функциях (Функции Azure)

Согласованность устойчивых функций реализована в коде и может использовать встроенные функции обработки ошибок языка программирования. В действительности нет никаких новых концепций, которые необходимо изучить, чтобы добавить в согласование обработку ошибок и компенсацию. Однако следует помнить о некоторых особенностях поведения.

## <a name="errors-in-activity-functions"></a>Ошибки в функциях действий

Любое исключение, которое вызывается в функции действия, передается обратно в функцию Orchestrator и выдается как `FunctionFailedException`. Вы можете написать код для обработки и компенсации ошибок, соответствующий вашим потребностям в функции оркестратора.

Например, рассмотрим следующую функцию оркестратора, которая перемещает средства с одного счета на другой:

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("TransferFunds")]
public static async Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var transferDetails = context.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        {
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

> [!NOTE]
> Предыдущие C# примеры предназначены для устойчивые функции 2. x. Для Устойчивые функции 1. x необходимо использовать `DurableOrchestrationContext` вместо `IDurableOrchestrationContext`. Дополнительные сведения о различиях между версиями см. в статье [устойчивые функции версии](durable-functions-versions.md) .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const transferDetails = context.df.getInput();

    yield context.df.callActivity("DebitAccount",
        {
            account = transferDetails.sourceAccount,
            amount = transferDetails.amount,
        }
    );

    try {
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.destinationAccount,
                amount = transferDetails.amount,
            }
        );
    }
    catch (error) {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.sourceAccount,
                amount = transferDetails.amount,
            }
        );
    }
});
```

---

Если первый вызов функции **кредитаккаунт** завершается неудачно, функция Orchestrator компенсирует денежные средства в исходной учетной записи.

## <a name="automatic-retry-on-failure"></a>Автоматическое повторение попыток при сбое

При вызове функций действий или функций суборкестрации можно указать автоматическую политику повтора. В следующем примере предпринимается попытка вызова функции до трех раз с ожиданием в течение 5 секунд между попытками:

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("TimerOrchestratorWithRetry")]
public static async Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await context.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);

    // ...
}
```

> [!NOTE]
> Предыдущие C# примеры предназначены для устойчивые функции 2. x. Для Устойчивые функции 1. x необходимо использовать `DurableOrchestrationContext` вместо `IDurableOrchestrationContext`. Дополнительные сведения о различиях между версиями см. в статье [устойчивые функции версии](durable-functions-versions.md) .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const firstRetryIntervalInMilliseconds = 5000;
    const maxNumberOfAttempts = 3;

    const retryOptions = 
        new df.RetryOptions(firstRetryIntervalInMilliseconds, maxNumberOfAttempts);

    yield context.df.callActivityWithRetry("FlakyFunction", retryOptions);

    // ...
});
```

---

Вызов функции действия в предыдущем примере принимает параметр для настройки политики автоматической повторной попытки. Существует несколько вариантов настройки политики автоматического повтора.

* **Max number of attempts** (Максимальное число попыток): максимальное число повторных попыток.
* **First retry interval** (Интервал до первого повтора): время ожидания перед первой повторной попыткой.
* **Backoff coefficient** (Коэффициент отсрочки): коэффициент, позволяющий определить степень увеличения отсрочки. По умолчанию равен 1.
* **Max retry interval** (Максимальный интервал повтора): максимальное время ожидания между повторными попытками.
* **Retry timeout** (Время ожидания повтора): максимальное время, отведенное на выполнение повторных попыток. Поведение по умолчанию — бесконечное повторение.
* **Handle**: можно указать определяемый пользователем обратный вызов, чтобы определить, следует ли повторить попытку выполнения функции.

## <a name="function-timeouts"></a>Время ожидания функций

Может потребоваться отменить вызов функции в функции Orchestrator, если выполнение займет слишком много времени. Правильный способ сделать это в настоящее время — создать [устойчивый таймер](durable-functions-timers.md), используя `context.CreateTimer` (.NET) или `context.df.createTimer` (JavaScript) в сочетании с `Task.WhenAny` (.NET) или `context.df.Task.any` (JavaScript), как показано в следующем примере:

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("TimerOrchestrator")]
public static async Task<bool> Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

> [!NOTE]
> Предыдущие C# примеры предназначены для устойчивые функции 2. x. Для Устойчивые функции 1. x необходимо использовать `DurableOrchestrationContext` вместо `IDurableOrchestrationContext`. Дополнительные сведения о различиях между версиями см. в статье [устойчивые функции версии](durable-functions-versions.md) .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("FlakyFunction");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    } else {
        // timeout case
        return false;
    }
});
```

---

> [!NOTE]
> Этот механизм фактически не прекращает текущее выполнение функции действия. Вместо этого он просто позволяет функции оркестратора игнорировать результат и двигаться дальше. Дополнительные сведения см. в документации по [таймерам](durable-functions-timers.md#usage-for-timeout).

## <a name="unhandled-exceptions"></a>Необработанные исключения

Если функция оркестратора завершается сбоем с необработанным исключением, сведения об этом исключении регистрируются в журнале, и экземпляр завершает работу с состоянием `Failed`.

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Сведения об оркестрации нескончаемые](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Дополнительные сведения для диагностики неполадок](durable-functions-diagnostics.md)
