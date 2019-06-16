---
title: API HTTP в устойчивых функциях — Azure
description: Сведения о том, как внедрять API HTTP в расширении устойчивых функций для Функций Azure.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: azfuncdf
ms.openlocfilehash: 2f0b01601dfb28b2b6b8ee8ca53398ec3dccb803
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65787289"
---
# <a name="http-apis-in-durable-functions-azure-functions"></a>API HTTP в устойчивых функциях (Функции Azure)

Расширение устойчивых задач предоставляет набор API HTTP, которые могут использоваться для выполнения следующих задач:

* получение состояния экземпляра оркестрации;
* отправка события в ожидающий экземпляр оркестрации;
* завершение работающего экземпляра оркестрации.

Каждый из этих API HTTP является операцией веб-перехватчика, обрабатываемой напрямую расширением устойчивых задач. Они не относятся к какой-либо функции в приложении-функции.

> [!NOTE]
> Эти операции также могут быть вызваны напрямую с помощью API управления экземплярами в классе [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html). Дополнительные сведения см. в [статье об управлении экземплярами](durable-functions-instance-management.md).

## <a name="http-api-url-discovery"></a>Обнаружение URL-адреса API HTTP

Класс [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) предоставляет API [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_), который можно использовать для создания полезных данных ответа HTTP, содержащих ссылки на все поддерживаемые операции. Ниже приведен пример функции HTTP-триггера, в котором показано, как использовать этот API:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (только для решения "Функции" версии 2.x)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

Эти примеры функций создают следующие данные ответа JSON. Тип данных всех полей — `string`.

| Поле                   |Описание                           |
|-------------------------|--------------------------------------|
| **`id`**                |Идентификатор экземпляра оркестрации. |
| **`statusQueryGetUri`** |URL-адрес состояния экземпляра оркестрации. |
| **`sendEventPostUri`**  |URL-адрес вызова события экземпляра оркестрации. |
| **`terminatePostUri`**  |URL-адрес завершения экземпляра оркестрации. |
| **`rewindPostUri`**     |URL-адрес перемотки экземпляра оркестрации. |

Вот пример ответа на запрос:

```http
HTTP/1.1 202 Accepted
Content-Length: 923
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX

{
    "id":"34ce9a28a6834d8492ce6a295f1a80e2",
    "statusQueryGetUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "sendEventPostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "terminatePostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "rewindPostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/rewind?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
}
```

> [!NOTE]
> Формат URL-адресов веб-перехватчиков может отличаться в зависимости от того, какая версия узла Функций Azure выполняется. Приведенный выше пример использует формат адресов для узла Функций Azure 2.x.

## <a name="async-operation-tracking"></a>Отслеживание асинхронных операций

Упомянутый ранее HTTP-ответ предназначен для помощи в реализации долго выполняющихся асинхронных API-интерфейсов HTTP с устойчивыми функциями. Это иногда называется *шаблоном опрашивающего объекта-получателя*. Поток клиента или сервера работает следующим образом:

1. Клиент отправляет запрос HTTP для запуска длительного процесса, например функции оркестратора.
2. Целевой триггер HTTP возвращает ответ HTTP 202 с заголовком `Location` со значением `statusQueryGetUri`.
3. Клиент отправляет URL-адрес в заголовке `Location`. Он продолжает просматривать ответы HTTP 202 с заголовком `Location`.
4. При завершении (или сбое) экземпляра конечная точка в заголовке `Location` возвращает ответ HTTP 200.

Этот протокол позволяет согласовать долго выполняющиеся процессы с помощью внешних клиентов или служб, которые поддерживают опрос конечной точки HTTP и используют заголовок `Location`. Ключевые компоненты уже встроены в API HTTP устойчивых функций.

> [!NOTE]
> По умолчанию все действия на основе HTTP, предоставляемые [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), поддерживают стандартную модель асинхронных операций. Эта возможность позволяет внедрять долго выполняющиеся устойчивые функции в рамках рабочего процесса Logic Apps. Дополнительные сведения о поддержке Logic Apps для асинхронных шаблонов HTTP см. в разделе [Модель асинхронных операций](../../logic-apps/logic-apps-workflow-actions-triggers.md#asynchronous-patterns).

## <a name="http-api-reference"></a>Справочник по API HTTP

Все API HTTP, реализованные с помощью расширения, принимают следующие параметры. Тип данных всех параметров — `string`.

| Параметр        | Тип параметра  | Описание |
|------------------|-----------------|-------------|
| **`taskHub`**    | Строка запроса    | Имя [центра задач](durable-functions-task-hubs.md). Если не указано, предполагается имя центра задач текущего приложения-функции. |
| **`connection`** | Строка запроса    | **Имя** строки подключения для учетной записи хранения. Если не указано, предполагается строка подключения по умолчанию для приложения-функции. |
| **`systemKey`**  | Строка запроса    | Ключ авторизации, необходимый для вызова API. |

`systemKey` — это ключ авторизации, автоматически создаваемый узлом Функций Azure. В частности, он предоставляет доступ к API расширения устойчивых задач и им можно управлять так же, как и [другими ключами авторизации](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API). Самый простой способ обнаружения значения `systemKey` — с помощью API `CreateCheckStatusResponse`, упомянутого ранее.

В следующих нескольких разделах рассматриваются определенные API HTTP, поддерживаемые расширением, и приведены примеры их использования.

### <a name="get-instance-status"></a>Получение состояния экземпляра

Возвращает состояние определенного экземпляра оркестрации.

#### <a name="request"></a>Запрос

Для версии 1.x среды выполнения функций, запрос имеет формат (для ясности показаны несколько строк) следующим образом:

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}
    ?taskHub={taskHub
    &connection={connectionName}
    &code={systemKey}
    &showHistory=[true|false]
    &showHistoryOutput=[true|false]
    &showInput=[true|false]
```

В версии 2.x среды выполнения функций, формат URL-адреса имеет те же параметры, но с немного другой префикс:

```http
GET /runtime/webhooks/durabletask/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &showHistory=[true|false]
    &showHistoryOutput=[true|false]
    &showInput=[true|false]
```

Параметры запроса для этого API включают набор по умолчанию, упомянутый ранее, а также следующие уникальные параметры:

| Поле                   | Тип параметра  | Описание |
|-------------------------|-----------------|-------------|
| **`instanceId`**        | URL-адрес             | Идентификатор экземпляра оркестрации. |
| **`showInput`**         | Строка запроса    | Необязательный параметр. Если значение `false`, функция входные данные не будут включены в полезных данных ответа.|
| **`showHistory`**       | Строка запроса    | Необязательный параметр. Если задано значение `true`, журнал выполнения оркестрации будет включен в полезные данные ответа.|
| **`showHistoryOutput`** | Строка запроса    | Необязательный параметр. Если значение `true`, функция выводит будут включены в журнал выполнения оркестрации.|
| **`createdTimeFrom`**   | Строка запроса    | Необязательный параметр. При указании фильтрует список возвращаемое значение экземпляров, которые были созданы в или после заданной метки времени ISO8601.|
| **`createdTimeTo`**     | Строка запроса    | Необязательный параметр. При указании фильтрует список возвращаемое значение экземпляров, которые были созданы во время или до заданной метки времени ISO8601.|
| **`runtimeStatus`**     | Строка запроса    | Необязательный параметр. При указании он фильтрует список возвращаемого значения экземпляров на основе их состояния среды выполнения. Чтобы просмотреть список возможных значений состояния среды выполнения, см. раздел [Запросы экземпляров](durable-functions-instance-management.md). |

#### <a name="response"></a>Ответ

Может быть возвращено несколько кодов состояния.

* **HTTP 200 (OK)** (HTTP 200 (ОК)): указанный экземпляр находится в завершенном состоянии.
* **HTTP 202 (Accepted)** (HTTP 202 (принято)): указанный экземпляр выполняется.
* **HTTP 400 (Bad Request)** (HTTP 400 (недопустимый запрос)): на определенном экземпляре произошел сбой, или его работа была прервана.
* **HTTP 404 (Not Found)** (HTTP 404 (не найдено)): указанный экземпляр не существует или не был запущен.
* **HTTP 500 (Internal Server Error)** (HTTP 500 (внутренняя ошибка сервера)): сбой указанного экземпляра с необработанным исключением.

Полезные данные ответа для случаев **HTTP 200** и **HTTP 202** являются объектами JSON со следующими полями:

| Поле                 | Тип данных | Описание |
|-----------------------|-----------|-------------|
| **`runtimeStatus`**   | string    | Состояние среды выполнения экземпляра. Возможные значения: *Running*, *Pending*, *Failed*, *Canceled*, *Terminated*, *Completed*. |
| **`input`**           | JSON      | Данные JSON, используемые для инициализации экземпляра. Это поле имеет значение `null`, если для параметра строки запроса `showInput` задано значение `false`.|
| **`customStatus`**    | JSON      | Данные JSON, используемые для состояния пользовательской оркестрации. Если поле не заполнено, для него устанавливается значение `null`. |
| **`output`**          | JSON      | Выходные данные JSON экземпляра. Это поле имеет значение `null`, если экземпляр не находится в завершенном состоянии. |
| **`createdTime`**     | string    | Время, когда был создан экземпляр. Использует расширенную нотацию ISO 8601. |
| **`lastUpdatedTime`** | string    | Время, когда экземпляр был в последний раз сохранен. Использует расширенную нотацию ISO 8601. |
| **`historyEvents`**   | JSON      | Массив JSON, содержащий журнал выполнения оркестрации. Это поле имеет значение `null`, если для параметра строки запроса `showHistory` не задано значение `true`. |

Ниже приведен пример полезных данных ответа, включающий журнал выполнения оркестрации и выходные данные действия (в удобном для чтения формате).

```json
{
  "createdTime": "2018-02-28T05:18:49Z",
  "historyEvents": [
      {
          "EventType": "ExecutionStarted",
          "FunctionName": "E1_HelloSequence",
          "Timestamp": "2018-02-28T05:18:49.3452372Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Tokyo!",
          "ScheduledTime": "2018-02-28T05:18:51.3939873Z",
          "Timestamp": "2018-02-28T05:18:52.2895622Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Seattle!",
          "ScheduledTime": "2018-02-28T05:18:52.8755705Z",
          "Timestamp": "2018-02-28T05:18:53.1765771Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello London!",
          "ScheduledTime": "2018-02-28T05:18:53.5170791Z",
          "Timestamp": "2018-02-28T05:18:53.891081Z"
      },
      {
          "EventType": "ExecutionCompleted",
          "OrchestrationStatus": "Completed",
          "Result": [
              "Hello Tokyo!",
              "Hello Seattle!",
              "Hello London!"
          ],
          "Timestamp": "2018-02-28T05:18:54.3660895Z"
      }
  ],
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "lastUpdatedTime": "2018-02-28T05:18:54Z",
  "output": [
      "Hello Tokyo!",
      "Hello Seattle!",
      "Hello London!"
  ],
  "runtimeStatus": "Completed"
}
```

Ответ **HTTP 202** также включает заголовок ответа **Location**, который ссылается на тот же URL-адрес, что и поле `statusQueryGetUri`, упомянутое ранее.

### <a name="get-all-instances-status"></a>Получение состояния всех экземпляров

Можно также запросить состояние всех экземпляров, удалив `instanceId` из запроса «Получение состояния экземпляра». В этом случае основные параметры одинаковы как «состояние экземпляра Get». Также поддерживаются параметры строки запроса для фильтрации.

Следует помнить, что `connection` и `code` являются дополнительными. Если у вас есть анонимная проверка подлинности на функцию, кода не требуется.
Если вы не хотите использовать строку подключения другое хранилище, отличное от указанного в параметре приложения AzureWebJobsStorage, затем можно игнорировать параметр строки запроса подключения.

#### <a name="request"></a>Запрос

Для версии 1.x среды выполнения функций, запрос имеет формат (для ясности показаны несколько строк) следующим образом:

```http
GET /admin/extensions/DurableTaskExtension/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
    &showInput=[true|false]
    &top={integer}
```

В версии 2.x среды выполнения функций, формат URL-адреса имеет те же параметры, но с немного другой префикс:

```http
GET /runtime/webhooks/durableTask/instances?
    taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
    &showInput=[true|false]
    &top={integer}
```

Параметры запроса для этого API включают набор по умолчанию, упомянутый ранее, а также следующие уникальные параметры:

| Поле                   | Тип параметра  | Описание |
|-------------------------|-----------------|-------------|
| **`instanceId`**        | URL-адрес             | Идентификатор экземпляра оркестрации. |
| **`showInput`**         | Строка запроса    | Необязательный параметр. Если значение `false`, функция входные данные не будут включены в полезных данных ответа.|
| **`showHistory`**       | Строка запроса    | Необязательный параметр. Если задано значение `true`, журнал выполнения оркестрации будет включен в полезные данные ответа.|
| **`showHistoryOutput`** | Строка запроса    | Необязательный параметр. Если значение `true`, функция выводит будут включены в журнал выполнения оркестрации.|
| **`createdTimeFrom`**   | Строка запроса    | Необязательный параметр. При указании фильтрует список возвращаемое значение экземпляров, которые были созданы в или после заданной метки времени ISO8601.|
| **`createdTimeTo`**     | Строка запроса    | Необязательный параметр. При указании фильтрует список возвращаемое значение экземпляров, которые были созданы во время или до заданной метки времени ISO8601.|
| **`runtimeStatus`**     | Строка запроса    | Необязательный параметр. При указании он фильтрует список возвращаемого значения экземпляров на основе их состояния среды выполнения. Чтобы просмотреть список возможных значений состояния среды выполнения, см. раздел [Запросы экземпляров](durable-functions-instance-management.md). |
| **`top`**               | Строка запроса    | Необязательный параметр. Если указано, ограничивает число экземпляров, возвращаемых запросом. |

#### <a name="response"></a>Ответ

Вот пример полезных данных ответа, включая состояние оркестрации (в удобном для чтения формате):

```json
[
    {
        "instanceId": "7af46ff000564c65aafbfe99d07c32a5",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:39Z",
        "lastUpdatedTime": "2018-06-04T10:46:47Z"
    },
    {
        "instanceId": "80eb7dd5c22f4eeba9f42b062794321e",
        "runtimeStatus": "Running",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:28Z",
        "lastUpdatedTime": "2018-06-04T15:18:38Z"
    },
    {
        "instanceId": "9124518926db408ab8dfe84822aba2b1",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:54Z",
        "lastUpdatedTime": "2018-06-04T10:47:03Z"
    },
    {
        "instanceId": "d100b90b903c4009ba1a90868331b11b",
        "runtimeStatus": "Pending",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:39Z",
        "lastUpdatedTime": "2018-06-04T15:18:39Z"
    }
]
```

> [!NOTE]
> Эта операция может быть весьма затратной с точки зрения операций ввода-вывода службы хранилища Azure, если в таблице экземпляров много строк. Дополнительные сведения о таблице экземпляров см. в документации по [производительности и масштабируемости в устойчивых функциях (Функциях Azure)](durable-functions-perf-and-scale.md#instances-table).
>

Если больше нет результатов, маркер продолжения возвращается в заголовке ответа.  Имя заголовка — `x-ms-continuation-token`.

Если вы задали значение токена продолжения в заголовке следующего запроса, можно получить следующую страницу результатов. Это имя заголовка запроса является также `x-ms-continuation-token`.

### <a name="purge-single-instance-history"></a>Очистить журнал одного экземпляра

Удаляет журнал и связанные артефакты для определенного экземпляра оркестрации.

#### <a name="request"></a>Запрос

Для версии 1.x среды выполнения функций, запрос имеет формат (для ясности показаны несколько строк) следующим образом:

```http
DELETE /admin/extensions/DurableTaskExtension/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connection}
    &code={systemKey}
```

В версии 2.x среды выполнения функций, формат URL-адреса имеет те же параметры, но с немного другой префикс:

```http
DELETE /runtime/webhooks/durabletask/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connection}
    &code={systemKey}
```

Параметры запроса для этого API включают набор по умолчанию, упомянутый ранее, а также следующие уникальные параметры:

| Поле             | Тип параметра  | Описание |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL-адрес             | Идентификатор экземпляра оркестрации. |

#### <a name="response"></a>Ответ

Могут быть возвращены следующие значения кода состояния HTTP.

* **HTTP 200 (OK)** (HTTP 200 (ОК)): Журнал экземпляра был удален успешно.
* **HTTP 404 (Not Found)** (HTTP 404 (не найдено)): Указанный экземпляр не существует.

Полезные данные ответа для **HTTP 200** случай — объект JSON с следующее поле:

| Поле                  | Тип данных | Описание |
|------------------------|-----------|-------------|
| **`instancesDeleted`** | целое число   | Количество удаленных экземпляров. В случае одного экземпляра это значение всегда должно быть `1`. |

Ниже приведен пример полезных данных ответа (в формате, удобном для чтения):

```json
{
    "instancesDeleted": 1
}
```

### <a name="purge-multiple-instance-history"></a>Очистить журнал нескольких экземпляра

Можно также удалить историю и связанные артефакты для нескольких экземпляров в центр задач путем удаления `{instanceId}` из запроса «Очистить журнал один экземпляр». Чтобы выборочно очистить журнал экземпляра, используйте те же фильтры, описанные в запросе «Получить состояние всех экземпляров».

#### <a name="request"></a>Запрос

Для версии 1.x среды выполнения функций, запрос имеет формат (для ясности показаны несколько строк) следующим образом:

```http
DELETE /admin/extensions/DurableTaskExtension/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
```

В версии 2.x среды выполнения функций, формат URL-адреса имеет те же параметры, но с немного другой префикс:

```http
DELETE /runtime/webhooks/durabletask/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
```

Параметры запроса для этого API включают набор по умолчанию, упомянутый ранее, а также следующие уникальные параметры:

| Поле                 | Тип параметра  | Описание |
|-----------------------|-----------------|-------------|
| **`createdTimeFrom`** | Строка запроса    | Фильтрует список удаленных экземпляров, которые были созданы в или после заданной метки времени ISO8601.|
| **`createdTimeTo`**   | Строка запроса    | Необязательный параметр. При указании фильтрует список удаленных экземпляров, которые были созданы во время или до заданной метки времени ISO8601.|
| **`runtimeStatus`**   | Строка запроса    | Необязательный параметр. При указании фильтрует список удаленных экземпляров на основе их состояние среды выполнения. Чтобы просмотреть список возможных значений состояния среды выполнения, см. раздел [Запросы экземпляров](durable-functions-instance-management.md). |

> [!NOTE]
> Эта операция может быть очень затратными с точки зрения ввода-вывода хранилища Azure, если существует много строк в экземпляры и/или журнал таблицы. Дополнительные сведения об этих таблицах можно найти в [производительность и масштабирование в устойчивых функциях (функции Azure)](durable-functions-perf-and-scale.md#instances-table) документации.

#### <a name="response"></a>Ответ

Могут быть возвращены следующие значения кода состояния HTTP.

* **HTTP 200 (OK)** (HTTP 200 (ОК)): Журнал экземпляра был удален успешно.
* **HTTP 404 (Not Found)** (HTTP 404 (не найдено)): Экземпляры не найдены, соответствуют критерию фильтра.

Полезные данные ответа для **HTTP 200** случай — объект JSON с следующее поле:

| Поле                   | Тип данных | Описание |
|-------------------------|-----------|-------------|
| **`instancesDeleted`**  | целое число   | Количество удаленных экземпляров. |

Ниже приведен пример полезных данных ответа (в формате, удобном для чтения):

```json
{
    "instancesDeleted": 250
}
```

### <a name="raise-event"></a>Вызов события

Отправляет сообщение уведомления о событии в выполняющийся экземпляр оркестрации.

#### <a name="request"></a>Запрос

Для версии 1.x среды выполнения функций, запрос имеет формат (для ясности показаны несколько строк) следующим образом:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

В версии 2.x среды выполнения функций, формат URL-адреса имеет те же параметры, но с немного другой префикс:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/raiseEvent/{eventName}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

Параметры запроса для этого API включают набор по умолчанию, упомянутый ранее, а также следующие уникальные параметры:

| Поле             | Тип параметра  | Описание |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL-адрес             | Идентификатор экземпляра оркестрации. |
| **`eventName`**   | URL-адрес             | Имя события, которое ожидает целевой экземпляр оркестрации. |
| **`{content}`**   | Содержимое запроса | Полезные данные события в формате JSON. |

#### <a name="response"></a>Ответ

Может быть возвращено несколько кодов состояния.

* **HTTP 202 (Accepted)** (HTTP 202 (принято)): сгенерированное событие принято в обработку.
* **HTTP 400 (Bad Request)** (HTTP 400 (недопустимый запрос)): содержимое запроса не является допустимым типом `application/json` или не соответствует формату JSON.
* **HTTP 404 (Not Found)** (HTTP 404 (не найдено)): указанный экземпляр не найден.
* **HTTP 410 (Gone)** (HTTP 410 (потеряно)): указанный экземпляр выполнен или завершился с ошибкой и не может обрабатывать возникающие события.

Ниже приведен пример запроса, отправляющий строку JSON `"incr"` в экземпляр, который ожидает событие с именем **operation**:

```http
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

Ответы этого API не содержат какого-либо содержимого.

### <a name="terminate-instance"></a>Завершение экземпляра

Завершите выполнение экземпляра оркестрации.

#### <a name="request"></a>Запрос

Для версии 1.x среды выполнения функций, запрос имеет формат (для ясности показаны несколько строк) следующим образом:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

В версии 2.x среды выполнения функций, формат URL-адреса имеет те же параметры, но с немного другой префикс:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/terminate
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Параметры запроса для этого API включают набор по умолчанию, упомянутый ранее, а также следующий уникальный параметр.

| Поле             | Тип параметра  | Описание |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL-адрес             | Идентификатор экземпляра оркестрации. |
| **`reason`**      | Строка запроса    | Необязательный элемент. Причина завершения работы экземпляра оркестрации. |

#### <a name="response"></a>Ответ

Может быть возвращено несколько кодов состояния.

* **HTTP 202 (Accepted)** (HTTP 202 (принято)): запрос на завершение принят в обработку.
* **HTTP 404 (Not Found)** (HTTP 404 (не найдено)): указанный экземпляр не найден.
* **HTTP 410 (Gone)** (HTTP 410 (потеряно)): определенный экземпляр выполнен успешно или завершился сбоем.

Ниже приведен пример запроса, который завершает выполнение экземпляра и указывает причину **ошибки**:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Ответы этого API не содержат какого-либо содержимого.

## <a name="rewind-instance-preview"></a>Экземпляр перемотки (предварительная версия)

Восстанавливает сбойный экземпляр оркестрации в рабочем состоянии путем воспроизведения последних неудачных операций.

### <a name="request"></a>Запрос

Для версии 1.x среды выполнения функций, запрос имеет формат (для ясности показаны несколько строк) следующим образом:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/rewind
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

В версии 2.x среды выполнения функций, формат URL-адреса имеет те же параметры, но с немного другой префикс:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/rewind
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Параметры запроса для этого API включают набор по умолчанию, упомянутый ранее, а также следующий уникальный параметр.

| Поле             | Тип параметра  | Описание |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL-адрес             | Идентификатор экземпляра оркестрации. |
| **`reason`**      | Строка запроса    | Необязательный элемент. Причина перемотки экземпляра оркестрации. |

### <a name="response"></a>Ответ

Может быть возвращено несколько кодов состояния.

* **HTTP 202 (Accepted)** (HTTP 202 (принято)): запрос на перемотку принят в обработку.
* **HTTP 404 (Not Found)** (HTTP 404 (не найдено)): указанный экземпляр не найден.
* **HTTP 410 (Gone)** (HTTP 410 (потеряно)): определенный экземпляр выполнен успешно или был прерван.

Ниже приведен пример запроса, который перематывает сбойный экземпляр и указывает причину **исправления**:

```http
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/rewind?reason=fixed&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Ответы этого API не содержат какого-либо содержимого.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Обработка ошибок в устойчивых функциях (Функции Azure)](durable-functions-error-handling.md)
