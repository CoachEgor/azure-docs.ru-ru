---
title: Привязки хранилища таблиц Azure для службы "Функции Azure"
description: Узнайте, как использовать привязки службы хранилища таблиц Azure в службе "Функции Azure".
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.openlocfilehash: 766bf1ba8e1070a3224bb9c50c527f6c709eb9a4
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75769444"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Привязки хранилища таблиц Azure для службы "Функции Azure"

В этой статье рассматривается работа с привязками хранилища таблиц Azure в Функциях Azure. Функции Azure поддерживают входные и выходные привязки для хранилища таблиц Azure.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Пакеты – Функции 1.x

Привязки службы "Хранилище таблиц" доступны в пакете NuGet [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) версии 2.х. Исходный код для пакета находится в репозитории GitHub [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table).

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x-and-higher"></a>Packages — функции 2. x и более поздних версий

Привязки хранилища таблиц доступны в пакете NuGet [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) версии 3.х. Исходный код для пакета находится в репозитории GitHub [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Входные данные

Используйте входную привязку хранилища таблиц Azure для чтения таблицы в учетной записи хранения Azure.

## <a name="input---example"></a>Пример входных данных

Языковой пример см. в разделах:

* [C# — чтение одной сущности](#input---c-example---one-entity);
* [C# — привязка к IQueryable](#input---c-example---iqueryable)
* [C# — привязка к CloudTable](#input---c-example---cloudtable)
* [Скрипт C# — чтение одной сущности](#input---c-script-example---one-entity)
* [Скрипт C# — привязка к IQueryable](#input---c-script-example---iqueryable)
* [Скрипт C# — привязка к CloudTable](#input---c-script-example---cloudtable)
* [F#](#input---f-example)
* [JavaScript](#input---javascript-example)
* [Java](#input---java-example)

### <a name="input---c-example---one-entity"></a>Пример входных данных C# — одна сущность

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая считывает одну строку таблицы. 

Значение ключа строки {queueTrigger} указывает, что ключ строки получен из строки сообщения очереди.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        ILogger log)
    {
        log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
    }
}
```

### <a name="input---c-example---iqueryable"></a>Пример входных данных C# — IQueryable

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая считывает несколько строк таблицы. Обратите внимание, что класс `MyPoco` является производным от `TableEntity`.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        ILogger log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
        }
    }
}
```

### <a name="input---c-example---cloudtable"></a>Пример входных данных C# — CloudTable

`IQueryable` не поддерживается в [среде выполнения Функций версии 2](functions-versions.md). Альтернативой является использование параметра метода `CloudTable` для чтения таблицы с помощью пакета SDK службы хранилища Azure. Ниже приведен пример функции, которая запрашивает таблицу журнала функций Azure:

```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;

namespace FunctionAppCloudTable2
{
    public class LogEntity : TableEntity
    {
        public string OriginalName { get; set; }
    }
    public static class CloudTableDemo
    {
        [FunctionName("CloudTableDemo")]
        public static async Task Run(
            [TimerTrigger("0 */1 * * * *")] TimerInfo myTimer, 
            [Table("AzureWebJobsHostLogscommon")] CloudTable cloudTable,
            ILogger log)
        {
            log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

            TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
                TableQuery.CombineFilters(
                    TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
                        "FD2"),
                    TableOperators.And,
                    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
                        "t")));

            // Execute the query and loop through the results
            foreach (LogEntity entity in 
                await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
            {
                log.LogInformation(
                    $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
            }
        }
    }
}
```

Дополнительную информацию об использовании CloudTable см. в статье [Начало работы с хранилищем таблиц Azure и API таблиц Azure Cosmos DB с помощью .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).

Если при попытке привязать к `CloudTable` вы получаете сообщение об ошибке, убедитесь, что у вас есть ссылка на [правильную версию пакета SDK для службы хранилища](#azure-storage-sdk-version-in-functions-1x).

### <a name="input---c-script-example---one-entity"></a>Пример входных данных скрипта C# — одна сущность

В следующем примере показана входная привязка таблицы в файле *function.json* и коде [скрипта C#](functions-reference-csharp.md), который использует привязку. Функция использует триггер очереди для чтения одной строки таблицы. 

Файл *function.json* определяет `partitionKey` и `rowKey`. Значение `rowKey` "{queueTrigger}" указывает, что ключ строки получен из строки сообщения очереди.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

В разделе [Конфигурация](#input---configuration) описываются эти свойства.

Ниже приведен код скрипта C#.

```csharp
public static void Run(string myQueueItem, Person personEntity, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    log.LogInformation($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---iqueryable"></a>Пример входных данных скрипта C# — IQueryable

В следующем примере показана входная привязка таблицы в файле *function.json* и коде [скрипта C#](functions-reference-csharp.md), который использует привязку. Функция считывает сущности ключа раздела, который указан в очереди сообщений.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

В разделе [Конфигурация](#input---configuration) описываются эти свойства.

В коде скрипта C# добавляется ссылка на пакет SDK для службы хранилища Azure, чтобы тип сущности мог быть производным от `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Extensions.Logging;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.LogInformation($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---cloudtable"></a>Пример входных данных скрипта C# — CloudTable

`IQueryable` не поддерживается в среде выполнения функций для [версий 2. x и выше.)](functions-versions.md) Альтернативой является использование параметра метода `CloudTable` для чтения таблицы с помощью пакета SDK службы хранилища Azure. Ниже приведен пример функции, которая запрашивает таблицу журнала функций Azure:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 */1 * * * *"
    },
    {
      "name": "cloudTable",
      "type": "table",
      "connection": "AzureWebJobsStorage",
      "tableName": "AzureWebJobsHostLogscommon",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, CloudTable cloudTable, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

    TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
            "FD2"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
            "a")));

    // Execute the query and loop through the results
    foreach (LogEntity entity in 
    await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
    {
        log.LogInformation(
            $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
    }
}

public class LogEntity : TableEntity
{
    public string OriginalName { get; set; }
}
```

Дополнительную информацию об использовании CloudTable см. в статье [Начало работы с хранилищем таблиц Azure и API таблиц Azure Cosmos DB с помощью .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).

Если при попытке привязать к `CloudTable` вы получаете сообщение об ошибке, убедитесь, что у вас есть ссылка на [правильную версию пакета SDK для службы хранилища](#azure-storage-sdk-version-in-functions-1x).

### <a name="input---f-example"></a>Пример входных данных F#

В следующем примере показана входная привязка таблицы в файле *function.json* и коде [скрипта F#](functions-reference-fsharp.md), который использует привязку. Функция использует триггер очереди для чтения одной строки таблицы. 

Файл *function.json* определяет `partitionKey` и `rowKey`. Значение `rowKey` "{queueTrigger}" указывает, что ключ строки получен из строки сообщения очереди.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

В разделе [Конфигурация](#input---configuration) описываются эти свойства.

Ниже показан код F#.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.LogInformation(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.LogInformation(sprintf "Name in Person entity: %s" personEntity.Name)
```

### <a name="input---javascript-example"></a>Пример входных данных JavaScript

В следующем примере показана входная привязка таблицы в файле *function.json* и [код JavaScript](functions-reference-node.md), который использует привязку. Функция использует триггер очереди для чтения одной строки таблицы. 

Файл *function.json* определяет `partitionKey` и `rowKey`. Значение `rowKey` "{queueTrigger}" указывает, что ключ строки получен из строки сообщения очереди.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

В разделе [Конфигурация](#input---configuration) описываются эти свойства.

Ниже показан код JavaScript.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

### <a name="input---java-example"></a>Входные данные в примере Java

В следующем примере показана функция, активируемая HTTP, которая возвращает общее количество элементов в указанном разделе в Хранилище таблиц.

```java
@FunctionName("getallcount")
public int run(
   @HttpTrigger(name = "req",
                 methods = {HttpMethod.GET},
                 authLevel = AuthorizationLevel.ANONYMOUS) Object dummyShouldNotBeUsed,
   @TableInput(name = "items",
                tableName = "mytablename",  partitionKey = "myparkey",
                connection = "myconnvarname") MyItem[] items
) {
    return items.length;
}
```


## <a name="input---attributes"></a>Входные атрибуты
 
В [библиотеках класса C#](functions-dotnet-class-library.md) используйте следующие атрибуты для настройки входной привязки таблицы:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  Конструктор атрибута принимает имя таблицы, ключ раздела и строки. Его можно использовать с параметром вывода или возвращаемым значением функции, как показано в следующем примере.

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Чтобы указать учетную запись хранения, которую нужно использовать, можно задать свойство `Connection`, как показано в следующем примере.

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Полный пример см. в разделе примеров входных данных C#.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Предоставляет еще один способ указать используемую учетную запись хранения. Конструктор принимает имя параметра приложения, содержащего строку подключения к службе хранилища. Атрибут может применяться на уровне класса, метода или параметра. Ниже показан пример уровня класса и метода.

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Используемая учетная запись хранения определяется в следующем порядке:

* Свойство `Table` атрибута `Connection`.
* Атрибут `StorageAccount`, примененный к тому же параметру, что и `Table`.
* Атрибут `StorageAccount`, примененный к функции.
* Атрибут `StorageAccount`, примененный к классу.
* Учетная запись хранения по умолчанию для приложения-функции (параметр приложения AzureWebJobsStorage).

## <a name="input---java-annotations"></a>Входные данные в заметках Java

В [библиотеке среды выполнения функций Java](/java/api/overview/azure/functions/runtime) используйте заметку `@TableInput` для параметров, значения которых будут поступать из Хранилища таблиц.  Эту аннотацию можно использовать с собственными типами Java, POJO или значениями, допускающими значение null, с помощью необязательного\<T >. 

## <a name="input---configuration"></a>Входная конфигурация

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json* и атрибуте `Table`.

|свойство function.json | Свойство атрибута |Description|
|---------|---------|----------------------|
|**type** | Н/Д | Нужно задать значение `table`. Это свойство задается автоматически при создании привязки на портале Azure.|
|**direction** | Н/Д | Нужно задать значение `in`. Это свойство задается автоматически при создании привязки на портале Azure. |
|**name** | Н/Д | Имя переменной, представляющей таблицу или сущность в коде функции. | 
|**tableName** | **TableName** | Имя таблицы.| 
|**partitionKey** | **PartitionKey** |Необязательный параметр. Ключ раздела сущности таблицы, которую нужно считать. Дополнительные сведения о том, как использовать это свойство, см. в этом [разделе](#input---usage).| 
|**rowKey** |**RowKey** | Необязательный параметр. Ключ строки сущности таблицы, которую нужно считать. Дополнительные сведения о том, как использовать это свойство, см. в этом [разделе](#input---usage).| 
|**take** |**Take** | Необязательный параметр. Максимальное количество считываемых сущностей в JavaScript. Дополнительные сведения о том, как использовать это свойство, см. в этом [разделе](#input---usage).| 
|**filter** |**Filter** | Необязательный параметр. Выражение фильтра OData для входных данных таблицы в JavaScript. Дополнительные сведения о том, как использовать это свойство, см. в этом [разделе](#input---usage).| 
|**connection** |**Соединение** | Имя параметра приложения, содержащего строку подключения к службе хранилища, используемой для этой привязки. Если имя параметра приложения начинается с AzureWebJobs, можно указать только остальную часть имени. Например, если для `connection` задано значение "MyStorage", среда выполнения функций ищет параметр приложения с именем "MyStorage". Если оставить строку `connection` пустой, среда выполнения службы "Функции" будет использовать строку подключения к службе хранилища по умолчанию для параметра приложения с именем `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Использование входной привязки

Входная привязка хранилища таблиц применима в следующих сценариях:

* **Чтение одной строки в коде или скрипте C#**

  Задайте значения для `partitionKey` и `rowKey`. Получите доступ к данным таблицы с помощью параметра метода `T <paramName>`. В скрипте C# `paramName` — это значение, заданное в свойстве `name` файла *function.json*. `T` обычно представляет собой тип, реализующий `ITableEntity`, или является производным от `TableEntity`. Свойства `filter` и `take` не используются в этом сценарии. 

* **Чтение одной или нескольких строк в коде или скрипте C#**

  Получите доступ к данным таблицы с помощью параметра метода `IQueryable<T> <paramName>`. В скрипте C# `paramName` — это значение, заданное в свойстве `name` файла *function.json*. `T` должен представлять собой тип, реализующий `ITableEntity`, или быть производным от `TableEntity`. Для выполнения фильтрации можно использовать методы `IQueryable`. Свойства `partitionKey`, `rowKey`, `filter` и `take` не используются в этом сценарии.  

  > [!NOTE]
  > `IQueryable` не поддерживается в [среде выполнения Функций версии 2](functions-versions.md). Альтернативой является [использование параметра метода CloudTable paramName](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) для чтения таблицы с помощью пакета SDK службы хранилища Azure. Если при попытке привязать к `CloudTable` вы получаете сообщение об ошибке, убедитесь, что у вас есть ссылка на [правильную версию пакета SDK для службы хранилища](#azure-storage-sdk-version-in-functions-1x).

* **Чтение одной или нескольких строк в JavaScript**

  Укажите свойства `filter` и `take`. Не определяйте свойства `partitionKey` и `rowKey`. Получите доступ к сущности (или сущностям) входной таблицы, используя `context.bindings.<BINDING_NAME>`. Десериализированный объект имеет свойства `RowKey` и `PartitionKey`.

## <a name="output"></a>Выходные данные

Используйте выходную привязку хранилища таблиц Azure для записи сущностей в таблицу в учетной записи хранения Azure.

> [!NOTE]
> Выходная привязка не поддерживает обновление существующих сущностей. Используйте соответствующий [`TableOperation`](/dotnet/api/microsoft.azure.cosmos.table.tableoperation?view=azure-dotnet) из [пакета SDK службы хранилища Azure](/azure/cosmos-db/tutorial-develop-table-dotnet#insert-or-merge-an-entity) , чтобы обновить имеющуюся сущность по мере необходимости.   

## <a name="output---example"></a>Пример выходных данных

Языковой пример см. в разделах:

* [C#](#output---c-example)
* [Скрипт C# (CSX)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Пример выходных данных C#

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая использует триггер HTTP для записи одной строки таблицы. 

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, ILogger log)
    {
        log.LogInformation($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

### <a name="output---c-script-example"></a>Пример выходных данных скрипта C#

В следующем примере показана выходная привязка таблицы в файле *function.json* и коде [скрипта C#](functions-reference-csharp.md), который использует привязку. Эта функция записывает несколько сущностей в таблице.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

В разделе [Конфигурация](#output---configuration) описываются эти свойства.

Ниже приведен код скрипта C#.

```csharp
public static void Run(string input, ICollector<Person> tableBinding, ILogger log)
{
    for (int i = 1; i < 10; i++)
        {
            log.LogInformation($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

### <a name="output---f-example"></a>Пример выходных данных F#

В следующем примере показана выходная привязка таблицы в файле *function.json* и коде [скрипта F#](functions-reference-fsharp.md), который использует привязку. Эта функция записывает несколько сущностей в таблице.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

В разделе [Конфигурация](#output---configuration) описываются эти свойства.

Ниже показан код F#.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: ILogger) =
    for i = 1 to 10 do
        log.LogInformation(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

### <a name="output---javascript-example"></a>Пример выходных данных JavaScript

В следующем примере показана выходная привязка таблицы в файле *function.json* и функции [JavaScript](functions-reference-node.md), которая использует привязку. Эта функция записывает несколько сущностей в таблице.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

В разделе [Конфигурация](#output---configuration) описываются эти свойства.

Ниже показан код JavaScript.

```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

## <a name="output---attributes"></a>Выходные атрибуты

В [библиотеках классов C#](functions-dotnet-class-library.md) используйте [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs).

Конструктор атрибута использует имя таблицы. Его можно использовать с параметром `out` или возвращаемым значением функции, как показано в следующем примере.

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Чтобы указать учетную запись хранения, которую нужно использовать, можно задать свойство `Connection`, как показано в следующем примере.

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Полный пример см. в разделе [Пример выходных данных C#](#output---c-example).

Чтобы указать учетную запись хранения на уровне класса, метода или параметра, можно использовать атрибут `StorageAccount`. Дополнительные сведения см. в разделе [Входные атрибуты](#input---attributes).

## <a name="output---configuration"></a>Выходная конфигурация

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json* и атрибуте `Table`.

|свойство function.json | Свойство атрибута |Description|
|---------|---------|----------------------|
|**type** | Н/Д | Нужно задать значение `table`. Это свойство задается автоматически при создании привязки на портале Azure.|
|**direction** | Н/Д | Нужно задать значение `out`. Это свойство задается автоматически при создании привязки на портале Azure. |
|**name** | Н/Д | Имя переменной, используемое в функции кода, которая представляет таблицу или сущность. Задайте значение `$return`, ссылающееся на возвращаемое значение функции.| 
|**tableName** |**TableName** | Имя таблицы.| 
|**partitionKey** |**PartitionKey** | Ключ раздела сущности таблицы, которую нужно записать. Дополнительные сведения о том, как использовать это свойство, см. в этом [разделе](#output---usage).| 
|**rowKey** |**RowKey** | Ключ строки сущности таблицы, которую нужно записать. Дополнительные сведения о том, как использовать это свойство, см. в этом [разделе](#output---usage).| 
|**connection** |**Соединение** | Имя параметра приложения, содержащего строку подключения к службе хранилища, используемой для этой привязки. Если имя параметра приложения начинается с AzureWebJobs, можно указать только остальную часть имени. Например, если для `connection` задано значение "MyStorage", среда выполнения функций ищет параметр приложения с именем "MyStorage". Если оставить строку `connection` пустой, среда выполнения службы "Функции" будет использовать строку подключения к службе хранилища по умолчанию для параметра приложения с именем `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Использование выходной привязки

Выходная привязка хранилища таблиц применима в следующих сценариях:

* **Запись одной строки на любом языке**

  В коде и скрипте C# получите доступ к сущности выходной таблицы с помощью параметра метода (например, `out T paramName`) или возвращаемого значения функции. В скрипте C# `paramName` — это значение, заданное в свойстве `name` файла *function.json*. `T` может быть любым сериализируемым типом, если ключ раздела и строки предоставляются в файле *function.json* или атрибуте `Table`. В противном случае `T` должен быть типом со свойствами `PartitionKey` и `RowKey`. В этом сценарии `T` обычно реализует `ITableEntity` или является производным от `TableEntity` (но необязательно).

* **Запись одной или нескольких строк в коде или скрипте C#**

  В коде и скрипте C# получите доступ к сущности выходной таблицы с помощью параметра метода (например, `ICollector<T> paramName` или `IAsyncCollector<T> paramName`). В скрипте C# `paramName` — это значение, заданное в свойстве `name` файла *function.json*. В `T` указывается схема сущностей, которые нужно добавить. Как правило, `T` является производным от `TableEntity` или реализует `ITableEntity`, но это необязательно. Значения ключа раздела и строки в *function.json* или конструктора атрибута `Table` не используются в этом сценарии.

  Альтернативой является использование параметра метода `CloudTable` для записи в таблицу с помощью пакета SDK службы хранилища Azure. Если при попытке привязать к `CloudTable` вы получаете сообщение об ошибке, убедитесь, что у вас есть ссылка на [правильную версию пакета SDK для службы хранилища](#azure-storage-sdk-version-in-functions-1x). Пример кода, который привязывается к `CloudTable`, см. в примерах входной привязки для [C#](#input---c-example---cloudtable) или [скрипта C#](#input---c-script-example---cloudtable) ранее в этой статье.

* **Запись одной или нескольких строк в JavaScript**

  В функциях JavaScript доступ к выходным данным таблицы можно получить, используя `context.bindings.<BINDING_NAME>`.

## <a name="exceptions-and-return-codes"></a>Исключения и коды возврата

| Привязка | Справочные материалы |
|---|---|
| Таблицы | [Коды ошибок таблицы](https://docs.microsoft.com/rest/api/storageservices/fileservices/table-service-error-codes) |
| Большой двоичный объект, таблица, очередь | [Коды ошибок хранилища](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Большой двоичный объект, таблица, очередь | [Устранение неполадок](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Основные понятия триггеров и привязок в Функциях Azure](functions-triggers-bindings.md)
