---
title: Триггер разогрева функций Azure
description: Понять, как использовать триггер для разогрева в функциях Azure.
documentationcenter: na
author: alexkarcher-msft
manager: gwallace
keywords: лазурные функции, функции, обработка событий, разминка, холодный старт, премиум, динамические вычисления, без серверной архитектуры
ms.service: azure-functions
ms.topic: reference
ms.date: 11/08/2019
ms.author: alkarche
ms.openlocfilehash: c3ed780bc50b690b2f5c3285024695ec6426b9b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "77167316"
---
# <a name="azure-functions-warm-up-trigger"></a>Триггер разогрева функций Azure

В этой статье объясняется, как работать с триггером разогрева в Azure Functions. Триггер разминки поддерживается только для функциональных приложений, работающих в [плане Premium.](functions-premium-plan.md) Триггер разминки вызывается при добавлении экземпляра для масштабирования приложения функции выполнения. Вы можете использовать триггер для предварительной загрузки пользовательских зависимостей во время [предварительного нагрева,](./functions-premium-plan.md#pre-warmed-instances) так что ваши функции готовы начать обработку запросов немедленно. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>Пакеты - Функции 2.x и выше

Требуется пакет [Microsoft.Azure.WebJobs.Extensions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet, версия **3.0.5 или выше.** Исходный код для пакета находится в репозитории GitHub [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/). 

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Триггер

Триггер разминки позволяет определить функцию, которая будет работать на новом экземпляре при добавлении в запущенное приложение. Вы можете использовать функцию разогрева для открытия соединений, загрузки зависимостей или запуска любой другой пользовательской логики, прежде чем приложение начнет получать трафик. 

Триггер разминки предназначен для создания общих зависимостей, которые будут использоваться другими функциями в приложении. [Смотрите примеры общих зависимостей здесь](./manage-connections.md#client-code-examples).

Обратите внимание, что триггер разминки вызывается только во время операций по масштабу, а не во время перезагрузки или других немасштабных запусков. Необходимо убедиться, что логика может загрузить все необходимые зависимости без использования триггера для разогрева. Ленивая загрузка является хорошим шаблоном для достижения этой цели.

## <a name="trigger---example"></a>Пример триггера

# <a name="c"></a>[C #](#tab/csharp)

В следующем примере показана [функция C-программы,](functions-dotnet-class-library.md) которая будет работать на каждом новом экземпляре при добавлении в приложение. Атрибут значения возврата не требуется.


* Ваша функция должна ```warmup``` быть названа (нечувствительным к случаям) и может быть только одна функция разогрева в приложении.
* Для использования разминки в качестве функции библиотеки класса .NET, пожалуйста, убедитесь, что у вас есть ссылка на пакет **на Microsoft.Azure.WebJobs.Extensions >3.0.5**
    * ```<PackageReference Include="Microsoft.Azure.WebJobs.Extensions" Version="3.0.5" />```


Комментарии заполнителя показывают, где в приложении декларировать и инициализировать общие зависимости. 
[Подробнее об общих зависимостях можно узнать здесь.](./manage-connections.md#client-code-examples)

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
 
namespace WarmupSample
{

    //Declare shared dependencies here

    public static class Warmup
    {
        [FunctionName("Warmup")]
        public static void Run([WarmupTrigger()] WarmupContext context,
            ILogger log)
        {
            //Initialize shared dependencies here
            
            log.LogInformation("Function App instance is warm 🌞🌞🌞");
        }
    }
}
```
# <a name="c-script"></a>[Скрипт C#](#tab/csharp-script)


В следующем примере показан триггер разогрева в файле *function.json* и [функция скрипта C',](functions-reference-csharp.md) которая будет работать на каждом новом экземпляре при добавлении в приложение.

Ваша функция должна ```warmup``` быть названа (нечувствительная к случаю), и в приложении может быть только одна функция разогрева.

Ниже показан файл *function.json*.

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

В разделе [Конфигурация](#trigger---configuration) описываются эти свойства.

Ниже приведен код сценария C#, который выполняет привязку к `HttpRequest`:

```cs
public static void Run(ILogger log)
{
    log.LogInformation("Function App instance is warm 🌞🌞🌞");  
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

В следующем примере показан триггер разогрева в файле *function.json* и [функция JavaScript,](functions-reference-node.md) которая будет работать на каждом новом экземпляре при добавлении в приложение.

Ваша функция должна ```warmup``` быть названа (нечувствительным к случаям) и может быть только одна функция разогрева в приложении.

Ниже показан файл *function.json*.

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

В разделе [Конфигурация](#trigger---configuration) описываются эти свойства.

Ниже показан код JavaScript.

```javascript
module.exports = async function (context, warmupContext) {
    context.log('Function App instance is warm 🌞🌞🌞');
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

В следующем примере показан триггер разогрева в файле *function.json* и [функция Python,](functions-reference-python.md) которая будет работать на каждом новом экземпляре при добавлении в приложение.

Ваша функция должна ```warmup``` быть названа (нечувствительным к случаям) и может быть только одна функция разогрева в приложении.

Ниже показан файл *function.json*.

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

В разделе [Конфигурация](#trigger---configuration) описываются эти свойства.

Ниже приведен код Python.

```python
import logging
import azure.functions as func


def main(warmupContext: func.Context) -> None:
    logging.info('Function App instance is warm 🌞🌞🌞')
```

# <a name="java"></a>[Java](#tab/java)

В следующем примере показан триггер разминки, который выполняется при добавлении каждого нового экземпляра в приложение.

Ваша функция должна `warmup` быть названа (нечувствительным к случаям) и может быть только одна функция разогрева в приложении.

```java
@FunctionName("Warmup")
public void run( ExecutionContext context) {
       context.getLogger().info("Function App instance is warm 🌞🌞🌞");
}
```

---

## <a name="trigger---attributes"></a>Атрибуты триггера

В [библиотеках класса C q](functions-dotnet-class-library.md) `WarmupTrigger` атрибут доступен для настройки функции.

# <a name="c"></a>[C #](#tab/csharp)

В этом примере показано, как использовать атрибут [разминки.](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions/Extensions/Warmup/Trigger/WarmupTriggerAttribute.cs)

Обратите внимание, что ```Warmup``` ваша функция должна быть вызвана и может быть только одна функция разогрева в приложении.

```csharp
 [FunctionName("Warmup")]
        public static void Run(
            [WarmupTrigger()] WarmupContext context, ILogger log)
        {
            ...
        }
```

Для полного примера см. [пример триггера.](#trigger---example)

# <a name="c-script"></a>[Скрипт C#](#tab/csharp-script)

Атрибуты не поддерживаются скриптом C'.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Атрибуты не поддерживаются JavaScript.

# <a name="python"></a>[Python](#tab/python)

Атрибуты не поддерживаются Python.

# <a name="java"></a>[Java](#tab/java)

Триггер разминки не поддерживается в Java как атрибут.

---

## <a name="trigger---configuration"></a>Конфигурация триггера

В следующей таблице объясняется свойства связывающей конфигурации, `WarmupTrigger` установленные в файле *function.json* и атрибуте.

|свойство function.json | Свойство атрибута |Описание|
|---------|---------|----------------------|
| **тип** | Недоступно| Обязательное. Необходимо задать значение `warmupTrigger`. |
| **direction** | Недоступно| Обязательное. Необходимо задать значение `in`. |
| **name** | Недоступно| Требуется - переменное имя, используемое в функциональном коде.|

## <a name="trigger---usage"></a>Использование триггера

Дополнительная информация о срабатывающей функции разогрева при вызове не предоставляется.

## <a name="trigger---limits"></a>Триггер — ограничения

* Триггер разминки доступен только для приложений, работающих по [плану Premium.](./functions-premium-plan.md)
* Триггер разминки вызывается только во время операций масштабирования, а не во время перезагрузки или других немасштабных запусков. Необходимо убедиться, что логика может загрузить все необходимые зависимости без использования триггера для разогрева. Ленивая загрузка является хорошим шаблоном для достижения этой цели.
* Триггер разминки не может быть вызван, как только экземпляр уже запущен.
* В приложении может быть только одна функция триггера для разогрева.

## <a name="next-steps"></a>Дальнейшие действия

[Основные понятия триггеров и привязок в Функциях Azure](functions-triggers-bindings.md)
