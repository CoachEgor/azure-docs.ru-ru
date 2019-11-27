---
title: Привязки службы SignalR для службы "Функции Azure"
description: Узнайте, как использовать привязки службы SignalR с помощью службы "Функции Azure".
author: craigshoemaker
ms.topic: reference
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: 4c7d5d4d8777fee445585b43b58ceb261176b7f4
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74231017"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Привязки службы SignalR для службы "Функции Azure"

В этой статье объясняется, как пройти проверку подлинности и отправлять сообщения в режиме реального времени для клиентов, подключенных к [службе Azure SignalR](https://azure.microsoft.com/services/signalr-service/) с использованием привязок службы SignalR в службе "Функции Azure". Служба "Функции Azure" поддерживает входные и выходные привязки для службы SignalR.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Пакеты — Функции 2.x

Привязки службы SignalR предоставляются в пакете NuGet [Microsoft. Azure. веб-задания. Extensions. сигналрсервице](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService) , версия 1. *. Исходный код для пакета находится в репозитории GitHub [azure-functions-signalrservice-extension](https://github.com/Azure/azure-functions-signalrservice-extension).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]


### <a name="java-annotations"></a>Заметки Java

Чтобы использовать заметки службы SignalR в функциях Java, необходимо добавить зависимость к артефакту *Azure-functions-Java-Library-SignalR* (версии 1,0 или более поздней) в файл POM. XML.

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

> [!NOTE]
> Чтобы использовать привязку Службы SignalR в Java, версия основных инструментов службы "Функции Azure" должна быть 2.4.419 или более поздней (версия узла 2.0.12332).

## <a name="using-signalr-service-with-azure-functions"></a>Использование службы SignalR с функциями Azure

Дополнительные сведения о настройке и использовании службы SignalR и функций Azure см. в статье [Разработка и Настройка функций Azure с помощью службы Azure SignalR](../azure-signalr/signalr-concept-serverless-development-config.md).

## <a name="signalr-connection-info-input-binding"></a>Входная привязка сведений о подключении SignalR

Прежде чем клиент сможет подключиться к службе Azure SignalR, необходимо получить URL-адрес конечной точки службы и действительный маркер доступа. Входная привязка *SignalRConnectionInfo* создает URL-адрес конечной точки службы SignalR и допустимый маркер, которые используются для подключения к службе. Так как маркер ограничен по времени и может использоваться для идентификации конкретного пользователя при подключении, не следует его кэшировать или передавать в совместное пользование между клиентами. С помощью этой привязки клиенты могут использовать триггер HTTP для получения сведений о подключении.

Языковой пример см. в разделах:

* [2.x C#](#2x-c-input-examples)
* [2.x JavaScript](#2x-javascript-input-examples)
* [2. x Java](#2x-java-input-examples)

Дополнительные сведения о том, как эта привязка используется для создания функции "Negotiate", которая может использоваться клиентским пакетом SDK для SignalR, см. в [статье о разработке и настройке функций Azure](../azure-signalr/signalr-concept-serverless-development-config.md) в документации по основным понятиям службы SignalR.

### <a name="2x-c-input-examples"></a>Примеры входных C# данных 2. x

В следующем примере показана [функция C#](functions-dotnet-class-library.md), получающая сведения о подключении SignalR с помощью входной привязки и возвращающая их по протоколу HTTP.

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Прошедшие проверку подлинности маркеры

Если функцию активирует прошедший проверку подлинности клиент, вы можете добавить утверждение идентификатора пользователя для созданного маркера. Вы можете легко добавить проверку подлинности в приложение-функцию с помощью [проверки подлинности службы приложений](../app-service/overview-authentication-authorization.md).

Проверка подлинности службы приложений задает заголовки HTTP `x-ms-client-principal-id` и `x-ms-client-principal-name`, содержащие имя и идентификатор субъекта клиента прошедшего проверку подлинности пользователя соответственно. В качестве значения свойства `UserId` привязки можно задать один из заголовков с помощью [выражения привязки](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` или `{headers.x-ms-client-principal-name}`. 

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

### <a name="2x-javascript-input-examples"></a>Примеры входных данных JavaScript в 2. x

В следующем примере показана входная привязка для сведений о подключении SignalR в файле *function.json* и [функция JavaScript](functions-reference-node.md), использующая привязку для возврата сведений о подключении.

Данные привязки в файле *function.json*:

Пример файла function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Ниже показан код JavaScript.

```javascript
module.exports = async function (context, req, connectionInfo) {
    context.res.body = connectionInfo;
};
```

#### <a name="authenticated-tokens"></a>Прошедшие проверку подлинности маркеры

Если функцию активирует прошедший проверку подлинности клиент, вы можете добавить утверждение идентификатора пользователя для созданного маркера. Вы можете легко добавить проверку подлинности в приложение-функцию с помощью [проверки подлинности службы приложений](../app-service/overview-authentication-authorization.md).

Проверка подлинности службы приложений задает заголовки HTTP `x-ms-client-principal-id` и `x-ms-client-principal-name`, содержащие имя и идентификатор субъекта клиента прошедшего проверку подлинности пользователя соответственно. В качестве значения свойства `userId` привязки можно задать один из заголовков с помощью [выражения привязки](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` или `{headers.x-ms-client-principal-name}`. 

Пример файла function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Ниже показан код JavaScript.

```javascript
module.exports = async function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    context.res.body = connectionInfo;
};
```

### <a name="2x-java-input-examples"></a>2. x входные Примеры Java

В следующем примере показана [функция Java](functions-reference-java.md) , которая получает сведения о соединении SignalR с помощью входной привязки и возвращает ее по протоколу HTTP.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Прошедшие проверку подлинности маркеры

Если функцию активирует прошедший проверку подлинности клиент, вы можете добавить утверждение идентификатора пользователя для созданного маркера. Вы можете легко добавить проверку подлинности в приложение-функцию с помощью [проверки подлинности службы приложений](../app-service/overview-authentication-authorization.md).

Проверка подлинности службы приложений задает заголовки HTTP `x-ms-client-principal-id` и `x-ms-client-principal-name`, содержащие имя и идентификатор субъекта клиента прошедшего проверку подлинности пользователя соответственно. В качестве значения свойства `UserId` привязки можно задать один из заголовков с помощью [выражения привязки](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` или `{headers.x-ms-client-principal-name}`.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat",
            userId = "{headers.x-ms-client-principal-id}") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

## <a name="signalr-output-binding"></a>Выходная привязка SignalR

Выходная привязка *SignalR* используется для отправки одного или нескольких сообщений с помощью службы Azure SignalR. Вы можете выполнить широковещательную передачу сообщения для всех подключенных клиентов или только для подключенных клиентов, которые прошли проверку подлинности для конкретного пользователя.

Его также можно использовать для управления группами, к которым принадлежит пользователь.

Языковой пример см. в разделах:

* [2.x C#](#2x-c-send-message-output-examples)
* [2.x JavaScript](#2x-javascript-send-message-output-examples)
* [2. x Java](#2x-java-send-message-output-examples)

### <a name="2x-c-send-message-output-examples"></a>Примеры вывода сообщений C# 2. x для отправки

#### <a name="broadcast-to-all-clients"></a>Широковещательная передача для всех клиентов

В следующем примере показана [функция C#](functions-dotnet-class-library.md), отправляющая сообщение для всех подключенных клиентов с помощью выходной привязки. `Target` — это имя метода, вызываемого для каждого клиента. Свойство `Arguments` представляет собой пустой массив или массив с несколькими объектами, передаваемый в метод клиента.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

#### <a name="send-to-a-user"></a>Отправка пользователю

Сообщения можно отправлять только для подключений, которые прошли проверку подлинности для пользователя, задав свойство `UserId` сообщения SignalR.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will only be sent to this user ID
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

#### <a name="send-to-a-group"></a>Отправить в группу

Сообщение можно отправить только для соединений, добавленных в группу путем задания свойства `GroupName` сообщения SignalR.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will be sent to the group with this name
            GroupName = "myGroup",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

### <a name="2x-c-group-management-output-examples"></a>Примеры выходных данных C# управления группой 2. x

Служба SignalR позволяет добавлять пользователей в группы. Затем сообщения могут быть отправлены в группу. Для управления членством пользователя в группе можно использовать класс `SignalRGroupAction` с выходной привязкой `SignalR`.

#### <a name="add-user-to-a-group"></a>Добавление пользователя в группу

В следующем примере пользователь добавляется в группу.

```csharp
[FunctionName("addToGroup")]
public static Task AddToGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Удаление пользователя из группы

В следующем примере пользователь удаляется из группы.

```csharp
[FunctionName("removeFromGroup")]
public static Task RemoveFromGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

> [!NOTE]
> Чтобы обеспечить правильную привязку `ClaimsPrincipal`, необходимо настроить параметры проверки подлинности в функциях Azure.

### <a name="2x-javascript-send-message-output-examples"></a>Примеры выходных данных сообщения об отправке для JavaScript 2. x

#### <a name="broadcast-to-all-clients"></a>Широковещательная передача для всех клиентов

В следующем примере показана выходная привязка SignalR в файле *function.json* и [функция JavaScript](functions-reference-node.md), использующая эту привязку для отправки сообщения с помощью службы Azure SignalR. Для выходной привязки задайте массив из одного или нескольких сообщений SignalR. Сообщение SignalR состоит из свойства `target`, которое указывает имя метода, вызываемого для каждого клиента, и `arguments`, которое представляет собой массив объектов, передаваемых в метод клиента в качестве аргументов.

Данные привязки в файле *function.json*:

Пример файла function.json:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Ниже показан код JavaScript.

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

#### <a name="send-to-a-user"></a>Отправка пользователю

Сообщения можно отправлять только для подключений, которые прошли проверку подлинности для пользователя, задав свойство `userId` сообщения SignalR.

Файл *function.json* остается без изменений. Ниже показан код JavaScript.

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this user ID
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

#### <a name="send-to-a-group"></a>Отправить в группу

Сообщение можно отправить только для соединений, добавленных в группу путем задания свойства `groupName` сообщения SignalR.

Файл *function.json* остается без изменений. Ниже показан код JavaScript.

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this group
        "groupName": "myGroup",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

### <a name="2x-javascript-group-management-output-examples"></a>2. x примеры выходных данных для управления группами JavaScript

Служба SignalR позволяет добавлять пользователей в группы. Затем сообщения могут быть отправлены в группу. Вы можете использовать выходную привязку `SignalR` для управления членством пользователя в группе.

#### <a name="add-user-to-a-group"></a>Добавление пользователя в группу

В следующем примере пользователь добавляется в группу.

*function.json*

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "signalR",
      "name": "signalRGroupActions",
      "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
      "hubName": "chat",
      "direction": "out"
    }
  ]
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "add"
  }];
};
```

#### <a name="remove-user-from-a-group"></a>Удаление пользователя из группы

В следующем примере пользователь удаляется из группы.

*function.json*

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "signalR",
      "name": "signalRGroupActions",
      "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
      "hubName": "chat",
      "direction": "out"
    }
  ]
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "remove"
  }];
};
```

### <a name="2x-java-send-message-output-examples"></a>2. x. примеры выходных данных отправки сообщений Java

#### <a name="broadcast-to-all-clients"></a>Широковещательная передача для всех клиентов

В следующем примере показана [функция Java](functions-reference-java.md) , которая отправляет сообщение, используя выходную привязку для всех подключенных клиентов. `target` — это имя метода, вызываемого для каждого клиента. Свойство `arguments` представляет собой пустой массив или массив с несколькими объектами, передаваемый в метод клиента.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

#### <a name="send-to-a-user"></a>Отправка пользователю

Сообщения можно отправлять только для подключений, которые прошли проверку подлинности для пользователя, задав свойство `userId` сообщения SignalR.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.userId = "userId1";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

#### <a name="send-to-a-group"></a>Отправить в группу

Сообщение можно отправить только для соединений, добавленных в группу путем задания свойства `groupName` сообщения SignalR.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.groupName = "myGroup";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

### <a name="2x-java-group-management-output-examples"></a>2. x. примеры выходных данных для управления группами Java

Служба SignalR позволяет добавлять пользователей в группы. Затем сообщения могут быть отправлены в группу. Для управления членством пользователя в группе можно использовать класс `SignalRGroupAction` с выходной привязкой `SignalROutput`.

#### <a name="add-user-to-a-group"></a>Добавление пользователя в группу

В следующем примере пользователь добавляется в группу.

```java
@FunctionName("addToGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction addToGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "add";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

#### <a name="remove-user-from-a-group"></a>Удаление пользователя из группы

В следующем примере пользователь удаляется из группы.

```java
@FunctionName("removeFromGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction removeFromGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "remove";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

## <a name="configuration"></a>Параметр Configuration

### <a name="signalrconnectioninfo"></a>SignalRConnectionInfo

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json* и атрибуте `SignalRConnectionInfo`.

|свойство function.json | Свойство атрибута |ОПИСАНИЕ|
|---------|---------|----------------------|
|**type**|| Нужно задать значение `signalRConnectionInfo`.|
|**direction**|| Нужно задать значение `in`.|
|**name**|| Имя переменной, используемой в коде функции для объекта сведений о подключении. |
|**hubName**|**HubName**| В качестве значения следует указать имя центра SignalR, для которого создаются сведения о подключении.|
|**userId**|**UserId**| Необязательно: значение утверждения идентификатора пользователя, которое следует задать в маркере доступа. |
|**connectionStringSetting**|**ConnectionStringSetting**| Имя параметра приложения, который содержит строку подключения службы SignalR (по умолчанию — AzureSignalRConnectionString) |

### <a name="signalr"></a>SignalR

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json* и атрибуте `SignalR`.

|свойство function.json | Свойство атрибута |ОПИСАНИЕ|
|---------|---------|----------------------|
|**type**|| Нужно задать значение `signalR`.|
|**direction**|| Нужно задать значение `out`.|
|**name**|| Имя переменной, используемой в коде функции для объекта сведений о подключении. |
|**hubName**|**HubName**| В качестве значения следует указать имя центра SignalR, для которого создаются сведения о подключении.|
|**connectionStringSetting**|**ConnectionStringSetting**| Имя параметра приложения, который содержит строку подключения службы SignalR (по умолчанию — AzureSignalRConnectionString) |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Основные понятия триггеров и привязок в Функциях Azure](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure Functions development and configuration with Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md) (Разработка и настройка функций Azure с помощью Службы Azure SignalR)
