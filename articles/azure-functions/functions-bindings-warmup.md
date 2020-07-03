---
title: Триггер прогрева функций Azure
description: Узнайте, как использовать триггер прогрева в функциях Azure.
documentationcenter: na
author: alexkarcher-msft
manager: gwallace
keywords: функции Azure, функции, обработка событий, прогрев, холодный запуск, Premium, динамические вычислительные ресурсы, бессерверная архитектура
ms.service: azure-functions
ms.topic: reference
ms.date: 11/08/2019
ms.author: alkarche
ms.openlocfilehash: 013001eebeec232cc60e31f1a850aeab4fd6c905
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82982247"
---
# <a name="azure-functions-warm-up-trigger"></a>Триггер прогрева функций Azure

В этой статье объясняется, как работать с триггером прогрева в функциях Azure. Триггер прогрева поддерживается только для приложений-функций, работающих в [плане Premium](functions-premium-plan.md). Триггер прогрева вызывается при добавлении экземпляра для масштабирования работающего приложения-функции. Вы можете использовать триггер прогрева для предварительной загрузки пользовательских зависимостей во время [предварительной подготовки](./functions-premium-plan.md#pre-warmed-instances) , чтобы ваши функции были готовы к немедленному запуску обработки запросов. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>Packages — функции 2. x и более поздних версий

Требуется пакет NuGet [Microsoft. Azure. веб-JOBSS. Extensions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) , версия **3.0.5 или более поздняя** . Исходный код для пакета находится в репозитории GitHub [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/). 

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Триггер

Триггер прогрева позволяет определить функцию, которая будет выполняться в новом экземпляре при добавлении в выполняемое приложение. Функцию прогрева можно использовать для открытия подключений, загрузки зависимостей или выполнения любой другой пользовательской логики, прежде чем приложение начнет получать трафик. 

Триггер прогрева предназначен для создания общих зависимостей, которые будут использоваться другими функциями в приложении. [Примеры общих зависимостей см. здесь](./manage-connections.md#client-code-examples).

Обратите внимание, что триггер прогрева вызывается только во время операций горизонтального масштабирования, а не во время перезапуска или других немасштабируемых запусков. Необходимо убедиться, что логика может загружать все необходимые зависимости без использования триггера прогрева. Для достижения этой цели хорошим шаблоном является отложенная загрузка.

## <a name="trigger---example"></a>Пример триггера

# <a name="c"></a>[C#](#tab/csharp)

В следующем примере показана [функция C#](functions-dotnet-class-library.md) , которая будет выполняться на каждом новом экземпляре при добавлении в приложение. Атрибут возвращаемого значения не является обязательным.


* Функция должна называться ```warmup``` (без учета регистра), а для каждого приложения может существовать только одна функция прогрева.
* Чтобы использовать прогрев в качестве функции библиотеки классов .NET, убедитесь, что у вас есть ссылка на пакет **Microsoft. Azure. веб-задания. extensions >= 3.0.5**
    * ```<PackageReference Include="Microsoft.Azure.WebJobs.Extensions" Version="3.0.5" />```


Комментарии заполнителя показывают, где в приложении следует объявлять и инициализировать общие зависимости. 
Дополнительные [сведения об общих зависимостях](./manage-connections.md#client-code-examples)см. здесь.

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


В следующем примере показан триггер прогрева в файле *Function. JSON* и [функция сценария C#](functions-reference-csharp.md) , которая будет выполняться на каждом новом экземпляре при добавлении в приложение.

Функция должна называться ```warmup``` (без учета регистра), а для каждого приложения может существовать только одна функция прогрева.

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

В следующем примере показан триггер прогрева в файле *Function. JSON* и [функция JavaScript](functions-reference-node.md) , которая будет выполняться на каждом новом экземпляре при добавлении в приложение.

Функция должна называться ```warmup``` (без учета регистра), а для каждого приложения может существовать только одна функция прогрева.

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
};
```

# <a name="python"></a>[Python](#tab/python)

В следующем примере показан триггер прогрева в файле *Function. JSON* и [функция Python](functions-reference-python.md) , которая будет выполняться на каждом новом экземпляре при добавлении в приложение.

Функция должна называться ```warmup``` (без учета регистра), а для каждого приложения может существовать только одна функция прогрева.

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

В следующем примере показан триггер прогрева, который выполняется при добавлении каждого нового экземпляра в приложение.

Функция должна называться `warmup` (без учета регистра), а для каждого приложения может существовать только одна функция прогрева.

```java
@FunctionName("Warmup")
public void run( ExecutionContext context) {
       context.getLogger().info("Function App instance is warm 🌞🌞🌞");
}
```

---

## <a name="trigger---attributes"></a>Атрибуты триггера

В [библиотеках классов C#](functions-dotnet-class-library.md) `WarmupTrigger` атрибут доступен для настройки функции.

# <a name="c"></a>[C#](#tab/csharp)

В этом примере демонстрируется использование атрибута [прогрева](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions/Extensions/Warmup/Trigger/WarmupTriggerAttribute.cs) .

Обратите внимание, что функция должна ```Warmup``` быть вызвана, и для каждого приложения может быть только одна функция прогрева.

```csharp
 [FunctionName("Warmup")]
        public static void Run(
            [WarmupTrigger()] WarmupContext context, ILogger log)
        {
            ...
        }
```

Полный пример см. в [примере триггера](#trigger---example).

# <a name="c-script"></a>[Скрипт C#](#tab/csharp-script)

Атрибуты не поддерживаются скриптом C#.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Атрибуты не поддерживаются в JavaScript.

# <a name="python"></a>[Python](#tab/python)

Атрибуты не поддерживаются в Python.

# <a name="java"></a>[Java](#tab/java)

Триггер прогрева не поддерживается в Java как атрибут.

---

## <a name="trigger---configuration"></a>Конфигурация триггера

В следующей таблице описаны свойства конфигурации привязки, заданные в файле *Function. JSON* и `WarmupTrigger` атрибуте.

|свойство function.json | Свойство атрибута |Описание|
|---------|---------|----------------------|
| **type** | н/д| Обязательное. Необходимо задать значение `warmupTrigger`. |
| **двух** | н/д| Обязательное. Необходимо задать значение `in`. |
| **name** | н/д| Обязательное. имя переменной, используемое в коде функции.|

## <a name="trigger---usage"></a>Использование триггера

При вызове функции, активируемой для прогрева, дополнительные сведения не предоставляются.

## <a name="trigger---limits"></a>Триггер — ограничения

* Триггер прогрева доступен только для приложений, работающих в [плане Premium](./functions-premium-plan.md).
* Триггер прогрева вызывается только во время операций горизонтального масштабирования, а не во время перезапуска или других немасштабируемых запусков. Необходимо убедиться, что логика может загружать все необходимые зависимости без использования триггера прогрева. Для достижения этой цели хорошим шаблоном является отложенная загрузка.
* Триггер прогрева нельзя вызывать после того, как уже запущен экземпляр.
* В каждом приложении функции может быть только одна функция триггера прогрева.

## <a name="next-steps"></a>Дальнейшие действия

[Основные понятия триггеров и привязок в Функциях Azure](functions-triggers-bindings.md)
