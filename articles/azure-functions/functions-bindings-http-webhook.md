---
title: Триггеры и привязки HTTP в службе "Функции Azure"
description: Узнайте, как использовать триггеры и привязки HTTP в службе "Функции Azure".
author: craigshoemaker
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: e97a6e1adff02001e36a43d9fb4a917b7e133257
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2020
ms.locfileid: "75922438"
---
# <a name="azure-functions-http-triggers-and-bindings"></a>Триггеры и привязки HTTP в службе "Функции Azure"

В этой статье объясняется, как использовать триггеры и выходные привязки в службе "Функции Azure".

Триггер HTTP можно настроить на ответ [веб-перехватчикам](https://en.wikipedia.org/wiki/Webhook).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

Код в этой статье по умолчанию использует синтаксис .NET Core, используемый в функциях версии 2. x и более поздних. Сведения о синтаксисе версии 1.x см. на странице с [соответствующими шаблонами функций](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates).

## <a name="packages---functions-1x"></a>Пакеты – Функции 1.x

Привязки служебной шины доступны в пакете NuGet [Microsoft.Azure.WebJobs.Extensions.Http](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http), версия 1.х. Исходный код для пакета находится в репозитории GitHub [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.Http).

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x-and-higher"></a>Packages — функции 2. x и более поздних версий

Привязки служебной шины доступны в пакете NuGet [Microsoft.Azure.WebJobs.Extensions.Http](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http), версия 3.х. Исходный код для пакета находится в репозитории GitHub [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/).

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Триггер

Триггер HTTP позволяет вызвать функцию с помощью HTTP-запроса. Триггер HTTP можно использовать для создания независимых от сервера API-интерфейсов и для ответа веб-перехватчикам.

По умолчанию триггер HTTP возвращает значение HTTP 200 OK с пустым телом в функциях 1. x или HTTP 204 без содержимого с пустым телом в функциях 2. x и более поздних версий. Чтобы изменить ответ, настройте [выходную привязку HTTP](#output).

## <a name="trigger---example"></a>Пример триггера

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), выполняющая поиск параметра `name` в строке запроса или в тексте HTTP-запроса. Обратите внимание, что возвращаемое значение используется для привязки выходных данных, но атрибут возвращаемого значения не является обязательным.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] 
    HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

# <a name="c-scripttabcsharp-script"></a>[C#Индекса](#tab/csharp-script)

В следующем примере показаны привязка триггера в файле *function.json* и [функция сценария C#](functions-reference-csharp.md), которая использует эту привязку. В функции выполняется поиск параметра `name` в строке запроса или в тексте HTTP-запроса.

Ниже показан файл *function.json*.

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
```

В разделе [Конфигурация](#trigger---configuration) описываются эти свойства.

Ниже приведен код сценария C#, который выполняет привязку к `HttpRequest`:

```cs
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

Вы можете выполнить привязку к пользовательскому объекту вместо `HttpRequest`. Этот объект создается из текста запроса и анализируется как JSON. Аналогичным образом тип можно передать в привязку вывода ответа HTTP. Результатом будет текст ответа с кодом состояния 200.

```csharp
using System.Net;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static string Run(Person person, ILogger log)
{   
    return person.Name != null
        ? (ActionResult)new OkObjectResult($"Hello, {person.Name}")
        : new BadRequestObjectResult("Please pass an instance of Person.");
}

public class Person {
     public string Name {get; set;}
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

В следующем примере показана привязка триггера в файле *function.json* и [функция JavaScript](functions-reference-node.md), которая использует привязку. В функции выполняется поиск параметра `name` в строке запроса или в тексте HTTP-запроса.

Ниже показан файл *function.json*.

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

В разделе [Конфигурация](#trigger---configuration) описываются эти свойства.

Ниже показан код JavaScript.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

В следующем примере показаны привязка триггера в файле *function.json* и [функция Python](functions-reference-python.md), использующая эту привязку. В функции выполняется поиск параметра `name` в строке запроса или в тексте HTTP-запроса.

Ниже показан файл *function.json*.

```json
{
    "scriptFile": "__init__.py",
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

В разделе [Конфигурация](#trigger---configuration) описываются эти свойства.

Ниже приведен код Python.

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```

# <a name="javatabjava"></a>[Java](#tab/java)

* [Чтение параметра из строки запроса](#read-parameter-from-the-query-string)
* [Чтение текста из запроса POST](#read-body-from-a-post-request)
* [Чтение параметра из маршрута](#read-parameter-from-a-route)
* [Чтение текста POJO из запроса POST](#read-pojo-body-from-a-post-request)

В следующем примере показана привязка триггера HTTP в файле *function.json* и соответствующие [функции JavaScript](functions-reference-java.md), которые используют привязку. 

Ниже показан файл *function.json*.

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "anonymous",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

#### <a name="read-parameter-from-the-query-string"></a>Считывание параметра из строки запроса

Этот пример считывает параметр с именем ```id``` из строки запроса и использует его, чтобы создать документ JSON, возвращаемый клиенту, с типом содержимого ```application/json```. 

```java
@FunctionName("TriggerStringGet")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", 
            methods = {HttpMethod.GET}, 
            authLevel = AuthorizationLevel.ANONYMOUS)
        HttpRequestMessage<Optional<String>> request,
        final ExecutionContext context) {
    
    // Item list
    context.getLogger().info("GET parameters are: " + request.getQueryParameters());

    // Get named parameter
    String id = request.getQueryParameters().getOrDefault("id", "");

    // Convert and display
    if (id.isEmpty()) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                        .body("Document not found.")
                        .build();
    } 
    else {
        // return JSON from to the client
        // Generate document
        final String name = "fake_name";
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                        "\"description\": \"" + name + "\"}";
        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(jsonDocument)
                        .build();
    }
}
```

#### <a name="read-body-from-a-post-request"></a>Чтение текста из запроса POST

Этот пример считывает текст из запроса POST как ```String``` и использует его, чтобы создать документ JSON, возвращаемый клиенту, с типом содержимого ```application/json```.

```java
    @FunctionName("TriggerStringPost")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Request body is: " + request.getBody().orElse(""));

        // Check request body
        if (!request.getBody().isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final String body = request.getBody().get();
            final String jsonDocument = "{\"id\":\"123456\", " + 
                                         "\"description\": \"" + body + "\"}";
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(jsonDocument)
                          .build();
        }
    }
```

#### <a name="read-parameter-from-a-route"></a>Чтение параметра из маршрута

Этот пример считывает обязательный параметр с именем ```id``` и необязательный параметр ```name``` из пути маршрута и использует их для создания документа JSON, возвращаемого клиенту, с типом содержимого ```application/json```. T

```java
@FunctionName("TriggerStringRoute")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", 
            methods = {HttpMethod.GET}, 
            authLevel = AuthorizationLevel.ANONYMOUS,
            route = "trigger/{id}/{name=EMPTY}") // name is optional and defaults to EMPTY
        HttpRequestMessage<Optional<String>> request,
        @BindingName("id") String id,
        @BindingName("name") String name,
        final ExecutionContext context) {
    
    // Item list
    context.getLogger().info("Route parameters are: " + id);

    // Convert and display
    if (id == null) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                        .body("Document not found.")
                        .build();
    } 
    else {
        // return JSON from to the client
        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                        "\"description\": \"" + name + "\"}";
        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(jsonDocument)
                        .build();
    }
}
```

#### <a name="read-pojo-body-from-a-post-request"></a>Чтение тела POJO из запроса POST

Ниже приведен код для класса ```ToDoItem```, указанного в этом примере:

```java

public class ToDoItem {

  private String id;
  private String description;  

  public ToDoItem(String id, String description) {
    this.id = id;
    this.description = description;
  }

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }
  
  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}

```

Этот пример считывает текст запроса POST. Текст запроса автоматически десериализуется в объект ```ToDoItem``` и возвращается к клиенту с типом содержимого ```application/json```. Параметр ```ToDoItem``` сериализуется средой выполнения службы "Функции", так как он назначен свойству ```body``` класса ```HttpMessageResponse.Builder```.

```java
@FunctionName("TriggerPojoPost")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", 
            methods = {HttpMethod.POST}, 
            authLevel = AuthorizationLevel.ANONYMOUS)
        HttpRequestMessage<Optional<ToDoItem>> request,
        final ExecutionContext context) {
    
    // Item list
    context.getLogger().info("Request body is: " + request.getBody().orElse(null));

    // Check request body
    if (!request.getBody().isPresent()) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                        .body("Document not found.")
                        .build();
    } 
    else {
        // return JSON from to the client
        // Generate document
        final ToDoItem body = request.getBody().get();
        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(body)
                        .build();
    }
}
```

---

## <a name="trigger---attributes"></a>Атрибуты триггера

В [ C# библиотеках классов](functions-dotnet-class-library.md) и Java атрибут `HttpTrigger` доступен для настройки функции.

Можно задать уровень авторизации и допустимые методы HTTP в параметрах конструктора атрибутов, типе веб-перехватчика и шаблоне маршрута. Дополнительные сведения об этих параметрах см. в разделе [Конфигурация триггера](#trigger---configuration).

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

В этом примере показано, как использовать атрибут [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) .

```csharp
[FunctionName("HttpTriggerCSharp")]
public static Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous)] HttpRequest req)
{
    ...
}
```

Полный пример см. в [примере триггера](#trigger---example).

# <a name="c-scripttabcsharp-script"></a>[C#Индекса](#tab/csharp-script)

Атрибуты не поддерживаются C# сценарием.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Атрибуты не поддерживаются в JavaScript.

# <a name="pythontabpython"></a>[Python](#tab/python)

Атрибуты не поддерживаются в Python.

# <a name="javatabjava"></a>[Java](#tab/java)

В этом примере показано, как использовать атрибут [HttpTrigger](https://github.com/Azure/azure-functions-java-library/blob/dev/src/main/java/com/microsoft/azure/functions/annotation/HttpTrigger.java) .

```java
@FunctionName("HttpTriggerJava")
public HttpResponseMessage<String> HttpTrigger(
        @HttpTrigger(name = "req",
                     methods = {"get"},
                     authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<String> request,
        final ExecutionContext context) {

    ...
}
```

Полный пример см. в [примере триггера](#trigger---example).

---

## <a name="trigger---configuration"></a>Конфигурация триггера

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json* и атрибуте `HttpTrigger`.

|свойство function.json | Свойство атрибута |Description|
|---------|---------|----------------------|
| **type** | Н/Д| Обязательное. Необходимо задать значение `httpTrigger`. |
| **direction** | Н/Д| Обязательное. Необходимо задать значение `in`. |
| **name** | Н/Д| Обязательное. Имя переменной, из которой в коде функции можно получить запрос или текст запроса. |
| <a name="http-auth"></a>**authLevel** |  **AuthLevel** |Определяет, какие ключи (если они требуются) должны присутствовать в запросе, чтобы вызвать функцию. Уровень авторизации может принимать одно из следующих значений: <ul><li><code>anonymous</code>&mdash; — ключи API не требуются.</li><li><code>function</code>&mdash; — требуется ключ API для конкретной функции. Это значение используется по умолчанию, если не указано иное.</li><li><code>admin</code>&mdash; — требуется главный ключ.</li></ul> Дополнительные сведения см. в разделе [Ключи авторизации](#authorization-keys). |
| **methods** |**Методы** | Массив методов HTTP, на которые отвечает функция. Если свойство не указано, функция отвечает на все методы HTTP. См. раздел [Настройка конечной точки HTTP](#customize-the-http-endpoint). |
| **route** | **Route** | Шаблон маршрута, определяющий URL-адреса запросов, на которые отвечает функция. Если значение не указано, по умолчанию используется `<functionname>`. Дополнительные сведения см. [в разделе Настройка конечной точки HTTP](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** | _Поддерживается только для среды выполнения версии 1.x._<br/><br/>Указывает, что триггер HTTP должен выступать в качестве получателя [веб-перехватчика](https://en.wikipedia.org/wiki/Webhook) для указанного поставщика. Если вы установите это свойство, не устанавливайте свойство `methods`. Тип веб-перехватчика может принимать одно из следующих значений:<ul><li><code>genericJson</code>&mdash; — конечная точка веб-перехватчика общего назначения без логики для конкретного поставщика. Этот параметр определяет, что принимаются только запросы HTTP POST с содержимым типа `application/json`.</li><li><code>github</code>&mdash; — функция отвечает на вызовы [веб-перехватчиков GitHub](https://developer.github.com/webhooks/). Не используйте свойство _authLevel_ вместе с веб-перехватчиками GitHub. Дополнительные сведения см. в этой статье, в разделе веб-перехватчики GitHub.</li><li><code>slack</code>&mdash; — функция отвечает на вызовы [веб-перехватчиков Slack](https://api.slack.com/outgoing-webhooks). Не используйте свойство _authLevel_ вместе с веб-перехватчиками Slack. Дополнительные сведения см. в разделе о веб-перехватчиках Slack далее в этой статье.</li></ul>|

## <a name="trigger---usage"></a>Использование триггера

Тип входных данных триггера объявлен как `HttpRequest` или пользовательский тип. Если вы выберете `HttpRequest`, то получите полный доступ к объекту запроса. При объявлении пользовательского типа среда выполнения попытается проанализировать текст запроса JSON для задания свойств объекта.

### <a name="customize-the-http-endpoint"></a>Настройка конечной точки HTTP

По умолчанию при создании функции для триггера HTTP маршрут для ее адресации имеет следующий вид:

    http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>

Этот маршрут можно настроить с помощью дополнительного свойства `route` привязки для вывода триггера HTTP. Например, приведенный ниже файл *function.json* определяет свойство `route` для триггера HTTP.

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

При такой конфигурации функция доступна по приведенному ниже маршруту вместо исходного.

```
http://<APP_NAME>.azurewebsites.net/api/products/electronics/357
```

Это позволяет использовать в коде функции два параметра, передаваемых в адресе, — _category_ и _id_.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Для параметров можно использовать любое [ограничение маршрута веб-API](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints). Приведенный ниже код функции C# используют оба параметра.

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;

public static IActionResult Run(HttpRequest req, string category, int? id, ILogger log)
{
    var message = String.Format($"Category: {category}, ID: {id}");
    return (ActionResult)new OkObjectResult(message);
}
```

# <a name="c-scripttabcsharp-script"></a>[C#Индекса](#tab/csharp-script)

Для параметров можно использовать любое [ограничение маршрута веб-API](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints). Приведенный ниже код функции C# используют оба параметра.

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;

public static IActionResult Run(HttpRequest req, string category, int? id, ILogger log)
{
    var message = String.Format($"Category: {category}, ID: {id}");
    return (ActionResult)new OkObjectResult(message);
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

В узле Среда выполнения функций предоставляет текст запроса из объекта `context`. Дополнительные сведения см. в разделе [Пример JavaScript в триггере](#trigger---example).

В следующем примере показано, как считывать параметры маршрута из `context.bindingData`.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;
    var message = `Category: ${category}, ID: ${id}`;

    context.res = {
        body: message;
    }

    context.done();
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Контекст выполнения функции предоставляется с помощью параметра, объявленного как `func.HttpRequest`. Этот экземпляр позволяет функции обращаться к параметрам маршрута данных, значениям строк запросов и методам, которые позволяют возвращать HTTP-ответы.

После определения параметры маршрута становятся доступными для функции путем вызова метода `route_params`.

```python
import logging

import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:

    category = req.route_params.get('category')
    id = req.route_params.get('id')
    message = f"Category: {category}, ID: {id}"

    return func.HttpResponse(message)
```

# <a name="javatabjava"></a>[Java](#tab/java)

Контекст выполнения функции — это свойства, объявленные в атрибуте `HttpTrigger`. Атрибут позволяет определять параметры маршрута, уровни авторизации, HTTP-команды и экземпляр входящего запроса.

Параметры маршрута определяются с помощью атрибута `HttpTrigger`.

```java
package com.function;

import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class HttpTriggerJava {
    public HttpResponseMessage<String> HttpTrigger(
            @HttpTrigger(name = "req",
                         methods = {"get"},
                         authLevel = AuthorizationLevel.FUNCTION,
                         route = "products/{category:alpha}/{id:int}") HttpRequestMessage<String> request,
            @BindingName("category") String category,
            @BindingName("id") int id,
            final ExecutionContext context) {

        String message = String.format("Category  %s, ID: %d", category, id);
        return request.createResponseBuilder(HttpStatus.OK).body(message).build();
    }
}
```

---

По умолчанию все маршруты функций начинаются с префикса *api*. Вы можете настроить или удалить этот префикс с помощью свойства `http.routePrefix` в файле [host.json](functions-host-json.md). В следующем примере префикс маршрута *api* удаляется. Он заменяется пустой строкой в файле *host.json*.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### <a name="using-route-parameters"></a>Использование параметров маршрута

Параметры маршрута, которые определяют шаблон `route` функции, доступны для каждой привязки. Например, если имеется маршрут, определенный как `"route": "products/{id}"` то привязка хранилища таблиц может использовать значение параметра `{id}` в конфигурации привязки.

В следующей конфигурации показано, как параметр `{id}` передается в `rowKey`привязки.

```json
{
    "type": "table",
    "direction": "in",
    "name": "product",
    "partitionKey": "products",
    "tableName": "products",
    "rowKey": "{id}"
}
```


### <a name="working-with-client-identities"></a>Работа с удостоверениями клиентов

Если в приложении-функции используется [аутентификация и авторизация Службы приложений](../app-service/overview-authentication-authorization.md), сведения об аутентифицированных клиентах можно посмотреть прямо в коде. Эта информация доступна в виде [заголовков запросов, которые вставляет платформа](../app-service/app-service-authentication-how-to.md#access-user-claims). 

Эти сведения также можно считывать из данных привязки. Эта возможность доступна только для среды выполнения функций в 2. x и более поздних версиях. Кроме того, сейчас она доступна только для языков .NET.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Сведения о клиентах, прошедших проверку подлинности, доступны в виде [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). ClaimsPrincipal доступен как часть контекста запроса, как показано в следующем примере:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;

public static IActionResult Run(HttpRequest req, ILogger log)
{
    ClaimsPrincipal identities = req.HttpContext.User;
    // ...
    return new OkObjectResult();
}
```

Кроме того, ClaimsPrincipal можно включить как дополнительный параметр в сигнатуру функции:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;
using Newtonsoft.Json.Linq;

public static void Run(JObject input, ClaimsPrincipal principal, ILogger log)
{
    // ...
    return;
}
```

# <a name="c-scripttabcsharp-script"></a>[C#Индекса](#tab/csharp-script)

Сведения о клиентах, прошедших проверку подлинности, доступны в виде [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). ClaimsPrincipal доступен как часть контекста запроса, как показано в следующем примере:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;

public static IActionResult Run(HttpRequest req, ILogger log)
{
    ClaimsPrincipal identities = req.HttpContext.User;
    // ...
    return new OkObjectResult();
}
```

Кроме того, ClaimsPrincipal можно включить как дополнительный параметр в сигнатуру функции:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;
using Newtonsoft.Json.Linq;

public static void Run(JObject input, ClaimsPrincipal principal, ILogger log)
{
    // ...
    return;
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Прошедший проверку подлинности пользователь доступен через [заголовки HTTP](../app-service/app-service-authentication-how-to.md#access-user-claims).

# <a name="pythontabpython"></a>[Python](#tab/python)

Прошедший проверку подлинности пользователь доступен через [заголовки HTTP](../app-service/app-service-authentication-how-to.md#access-user-claims).

# <a name="javatabjava"></a>[Java](#tab/java)

Прошедший проверку подлинности пользователь доступен через [заголовки HTTP](../app-service/app-service-authentication-how-to.md#access-user-claims).

---

### <a name="authorization-keys"></a>Ключи авторизации

Служба "Функции" позволяет использовать ключи, чтобы затруднить несанкционированный доступ к конечным точкам функции HTTP во время развертывания.  Стандартному триггеру HTTP может потребоваться, чтобы такой ключ API был в запросе. 

> [!IMPORTANT]
> Хотя ключи могут помочь в маскировке конечных точек HTTP во время развертывания, они не предназначены для защиты триггера HTTP в рабочей среде. Дополнительные сведения см. в разделе [Защита конечной точки HTTP в рабочей среде](#secure-an-http-endpoint-in-production).

> [!NOTE]
> В среде выполнения службы "Функции" версии 1.х веб-перехватчик может использовать ключи для авторизации запросов различными способами, в зависимости от поддерживаемых поставщиком технологий. Это рассматривается в разделе [Веб-перехватчики и ключи](#webhooks-and-keys). Среда выполнения функций в версии 2. x и выше не включает встроенную поддержку поставщиков веб-перехватчиков.

Существует два типа ключей.

* **Ключи узла**, которые являются общими для всех функций в приложении-функции. Если такой ключ используется в качестве ключа API, он предоставляет доступ к любой функции в приложении-функции.
* **Ключи функции**, которые применяются только для конкретных функций, в которых они определены. Если такой ключ используется в качестве ключа API, он предоставляет доступ только к определенной функции.

Каждый ключ имеет имя для удобства использования, а один из ключей (с именем default) используется как ключ по умолчанию на уровне узла и функции. Ключи функций имеют приоритет над ключами узла. Если определены два ключа с одним именем, всегда используется ключ функции.

Каждое приложение-функция также имеет специальный **главный ключ**. Это ключ узла с именем `_master`, который предоставляет административный доступ к API среды выполнения. Его невозможно отозвать. При задании уровня авторизации `admin` запросы должны использовать главный ключ. Использование любого другого ключа приведет к ошибке авторизации.

> [!CAUTION]  
> Главный ключ предоставляет высокий уровень разрешений в приложении-функции, поэтому никогда не передавайте этот ключ третьим лицам и не включайте его в состав клиентских приложений. Соблюдайте осторожность при выборе уровня авторизации для администратора.

### <a name="obtaining-keys"></a>Получение ключей

Ключи хранятся в Azure в составе приложения-функции в зашифрованном виде. Чтобы просмотреть ключи, создать новые или сменить значения ключей, откройте на [портале Azure](https://portal.azure.com) нужную функцию, активируемую по HTTP, и выберите **Управление**.

![Управляйте ключами функций на портале.](./media/functions-bindings-http-webhook/manage-function-keys.png)

Ключи функций можно получить программно с помощью [API управления ключами](https://github.com/Azure/azure-functions-host/wiki/Key-management-API).

### <a name="api-key-authorization"></a>Проверка подлинности с помощью ключа API

Большинству шаблонов триггеров HTTP требуется ключ API в запросе. Поэтому HTTP-запрос обычно выглядит как следующий URL-адрес:

    https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?code=<API_KEY>

Ключ можно передать в переменной строки запроса с именем `code`, как показано выше. Его также можно включить в заголовок HTTP `x-functions-key`. Значением ключа может быть любой ключ функции, определенный для запрашиваемой функции, или любой ключ узла.

Вы можете разрешить анонимные запросы, не требующие наличие ключей. Вы также можете указать, чтобы использовался главный ключ. Уровень авторизации по умолчанию можно изменить с помощью свойства `authLevel` в объекте JSON для привязки. Дополнительные сведения см. в разделе [Конфигурация триггера](#trigger---configuration).

> [!NOTE]
> При локальном запуске функций авторизация отключается независимо от указанного параметра уровня авторизации. После публикации в Azure в триггере применяется параметр `authLevel`. Ключи по-прежнему требуются при [локальном запуске в контейнере](functions-create-function-linux-custom-image.md#run-the-image-locally).


### <a name="secure-an-http-endpoint-in-production"></a>Защита конечной точки HTTP в рабочей среде

Чтобы полностью защитить функции конечных точек в рабочей среде, необходимо реализовать один из следующих вариантов безопасности на уровне приложения-функции:

* Включить аутентификацию и авторизацию в Службе приложений для приложения-функции. Платформа службы приложений позволяет использовать Azure Active Directory (AAD) и несколько сторонних поставщиков удостоверений для проверки подлинности клиентов. Эта возможность позволяет реализовать пользовательские правила авторизации для функций и работать с информацией пользователя из кода функции. Дополнительные сведения см. в разделе [Работа с удостоверениями клиентов](#working-with-client-identities) и статье [Проверка подлинности и авторизация в службе приложений Azure](../app-service/overview-authentication-authorization.md).

* Используйте службу управления API Azure (APIM) для аутентификации запросов. APIM предоставляет широкий набор параметров безопасности API для входящих запросов. Дополнительные сведения см. [в статье о политиках аутентификации службы "Управление API"](../api-management/api-management-authentication-policies.md). С помощью службы "Управление API" можно настроить приложение-функцию на прием запросов только с IP-адреса вашего экземпляра этой службы. Дополнительные сведения см. в разделе [Ограничения IP-адресов](ip-addresses.md#ip-address-restrictions).

* Разверните приложение-функцию в среде службы приложений Azure (ASE). ASE предоставляет выделенную среду размещения, в которой можно выполнять функции. ASE позволяет настроить один интерфейсный шлюз, который можно использовать для аутентификации всех входящих запросов. Дополнительные сведения см. в статье [Настройка брандмауэра веб-приложения (WAF) для среды службы приложений](../app-service/environment/app-service-app-service-environment-web-application-firewall.md).

При использовании одного из этих функций безопасности уровня приложения следует задать для уровня авторизации функции, активируемого HTTP, значение `anonymous`.

### <a name="webhooks"></a>Веб-перехватчики

> [!NOTE]
> Режим веб-перехватчика доступен только для версии 1.x среды выполнения функций. Это изменение было внесено для повышения производительности триггеров HTTP в версиях 2. x и более поздних.

В версии 1.x шаблоны веб-перехватчика обеспечивают дополнительную проверку полезных данных веб-перехватчика. В версиях 2. x и выше базовый триггер HTTP по-прежнему работает и является рекомендуемым подходом для веб-перехватчиков. 

#### <a name="github-webhooks"></a>Веб-перехватчики GitHub

Чтобы настроить ответ на вызовы веб-перехватчика GitHub, прежде всего создайте функцию с триггером HTTP, для которого свойство **webHookType** будет иметь значение `github`. Затем скопируйте его URL-адрес и ключ API в репозиторий GitHub, используя страницу **Добавить веб-перехватчик**. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

#### <a name="slack-webhooks"></a>Веб-перехватчики Slack

Webhook Slack создает маркер автоматически и не позволяет вам задать его, поэтому необходимо настроить ключ для конкретной функции с использованием маркера, предоставленного Slack. Ознакомьтесь с разделом [Ключи авторизации](#authorization-keys).

### <a name="webhooks-and-keys"></a>Веб-перехватчики и ключи

Авторизация веб-перехватчика обрабатывается компонентом получателя веб-перехватчика, который входит в состав триггера HTTP. Механизм обработки различается в зависимости от типа веб-перехватчика. Каждый из этих механизмов зависит от ключей. По умолчанию используется ключ функции с именем default. Чтобы использовать другой ключ, необходимо настроить поставщик веб-перехватчика так, чтобы он отправлял имя ключа в составе запроса одним из следующих способов.

* **В строке запроса**: поставщик передает имя ключа в параметре `clientid` строки запроса, например `https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?clientid=<KEY_NAME>`.
* **В заголовке запроса**: поставщик передает имя ключа в заголовке `x-functions-clientid`.

## <a name="trigger---limits"></a>Триггер — ограничения

Длина HTTP-запроса ограничена 100 МБ (104 857 600 байт), а длина URL-адреса — 4 КБ (4096 байт). Эти ограничения задает элемент `httpRuntime`[файла Web.config](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config) среды выполнения.

Если функция, использующая триггер HTTP, не завершается в течение 230 секунд, [Azure Load Balancer](../app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds) истечет время ожидания и ВОЗВРАТИТ ошибку HTTP 502. Функция продолжит работу, но вернуть ответ HTTP будет невозможно. Для долго выполняющихся функций рекомендуем следовать асинхронным шаблонам и возвращать расположение, в котором можно проверить состояние запроса. Чтобы узнать, как долго может выполняться функция, см. раздел [Масштабирование и размещение — план](functions-scale.md#timeout).

## <a name="output"></a>Выходные данные

Привязка для вывода HTTP используется для ответа отправителю запроса HTTP. Эта привязка требует наличия триггера HTTP и позволяет настроить ответ, возвращаемый на запрос этого триггера. Если выходная Привязка HTTP не предоставлена, триггер HTTP возвращает HTTP 200 OK с пустым телом в функциях 1. x или HTTP 204 без содержимого с пустым телом в функциях 2. x и более поздних версий.

## <a name="output---configuration"></a>Выходная конфигурация

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json*. Для библиотек класса C# свойства атрибута, соответствующие этим свойствам *function.json*, отсутствуют.

|Свойство  |Description  |
|---------|---------|
| **type** |Нужно задать значение `http`. |
| **direction** | Нужно задать значение `out`. |
| **name** | Имя переменной, используемое в коде функции для ответа, или `$return` для использования возвращаемого значения. |

## <a name="output---usage"></a>Использование выходной привязки

Чтобы отправить ответ HTTP, используйте шаблоны ответов языкового стандарта. В C# или скрипте C# задайте тип возвращаемого значения функции `IActionResult` или `Task<IActionResult>`. В C# атрибут возвращаемого значения не является обязательным.

Примеры ответов см. в разделе с [примером триггера](#trigger---example).

## <a name="hostjson-settings"></a>Параметры файла host.json

В этом разделе описаны глобальные параметры конфигурации, доступные для этой привязки в версиях 2. x и более поздних. Пример файла host. JSON ниже содержит только параметры версии 2. x + для этой привязки. Дополнительные сведения о глобальных параметрах конфигурации в версиях 2. x и более поздних версий см. в [справочнике по Host. JSON для функций Azure](functions-host-json.md).

> [!NOTE]
> Чтобы получить дополнительные сведения о файле host.json в Функции 1.x, см. статью [host.json reference for Azure Functions 1.x](functions-host-json-v1.md#http)(Справочник по файлу host.json для службы "Функции Azure" версии 1.x.).

```json
{
    "extensions": {
        "http": {
            "routePrefix": "api",
            "maxOutstandingRequests": 200,
            "maxConcurrentRequests": 100,
            "dynamicThrottlesEnabled": true,
            "hsts": {
                "isEnabled": true,
                "maxAge": "10"
            },
            "customHeaders": {
                "X-Content-Type-Options": "nosniff"
            }
        }
    }
}
```

|Свойство  |По умолчанию | Description |
|---------|---------|---------| 
| кустомхеадерс|none|Позволяет задавать пользовательские заголовки в HTTP-ответе. В предыдущем примере в ответ добавляется заголовок `X-Content-Type-Options`, чтобы избежать перехвата типа содержимого. |
|dynamicThrottlesEnabled|true<sup>\*</sup>|Если этот параметр включен, он заставляет конвейер обработки запросов периодически проверять счетчики производительности системы (подключений, потоков, процессов, памяти, ЦП и т. д.). При превышении встроенного порогового высокого значения (80 %) любого из этих счетчиков запросы будут отклоняться с ответом "429 — cлишком много запросов" до тех пор, пока счетчики не вернутся к нормальному уровню.<br/><sup>\*</sup> По умолчанию в плане потребления `true`. Значение по умолчанию в выделенном плане — `false`.|
|HSTS|не включено|Если для параметра `isEnabled` задано значение `true`, то принудительное [поведение протокола HTTP (HSTS) в .NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.0&tabs=visual-studio#hsts) применяется, как определено в [классе`HstsOptions`](/dotnet/api/microsoft.aspnetcore.httpspolicy.hstsoptions?view=aspnetcore-3.0). Приведенный выше пример также задает для свойства [`maxAge`](/dotnet/api/microsoft.aspnetcore.httpspolicy.hstsoptions.maxage?view=aspnetcore-3.0#Microsoft_AspNetCore_HttpsPolicy_HstsOptions_MaxAge) значение 10 дней. Поддерживаемые свойства `hsts`: <table><tr><th>Свойство</th><th>Description</th></tr><tr><td>ексклудедхостс</td><td>Массив строк имен узлов, для которого заголовок HSTS не добавляется.</td></tr><tr><td>includeSubDomains</td><td>Логическое значение, указывающее, включен ли параметр Инклудесубдомаин заголовка с уровнем безопасности "Долгосрочный транспорт — Безопасность".</td></tr><tr><td>maxAge</td><td>Строка, определяющая параметр max-age заголовка с ограничением транспорта по безопасности.</td></tr><tr><td>Preload (Предварительная загрузка)</td><td>Логическое значение, указывающее, включен ли параметр предварительной загрузки заголовка с уровнем безопасности "Долгосрочный транспорт — Безопасность".</td></tr></table>|
|maxConcurrentRequests|100<sup>\*</sup>|Максимальное число функций HTTP, выполняемых параллельно. Это позволяет регулировать параллелизм, что может помочь в управлении использованием ресурсов. Например, у вас может быть функция HTTP, которая использует большой объем системных ресурсов (память, ЦП и сокеты), что вызывает проблемы при слишком высоком уровне параллелизма. Или же функция может выполнять исходящие запросы к сторонней службе, и частоту таких вызовов необходимо ограничить. В таких случаях может помочь применение регулирования. <br/><sup>*</sup> Значение по умолчанию для плана потребления — 100. Значение по умолчанию для выделенного плана не ограничено (`-1`).|
|maxOutstandingRequests|200<sup>\*</sup>|Максимальное число невыполненных запросов, которое хранится в любой отдельно взятый момент времени. Это ограничение включает запросы, которые находятся в очереди, но еще не начали выполняться, а также все запросы в процессе выполнения. Все входящие запросы, превышающие это ограничение, отклоняются с ответом 429 "Too Busy" (Перегрузка). Это позволяет вызывающим объектам использовать стратегии повторов на основе времени, а также помогает вам контролировать максимальные задержки запросов. Эта настройка влияет только на очереди, которые создаются по пути выполнения средства обработки скриптов. Она не влияет на другие очереди, такие как очередь запросов ASP.NET. <br/><sup>\*</sup> Значение по умолчанию для плана потребления — 200. Значение по умолчанию для выделенного плана не ограничено (`-1`).|
|routePrefix|api|Префикс маршрута, который применяется ко всем маршрутам. Используйте пустую строку, чтобы удалить префикс по умолчанию. |


## <a name="next-steps"></a>Дальнейшие действия

[Основные понятия триггеров и привязок в Функциях Azure](functions-triggers-bindings.md)
