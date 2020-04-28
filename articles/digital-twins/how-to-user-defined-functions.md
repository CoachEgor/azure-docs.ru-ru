---
title: Создание определяемых пользователем функций — в Azure Digital двойников | Документация Майкрософт
description: Создание определяемых пользователем функций, сопоставителей и назначений ролей с помощью Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/17/2020
ms.custom: seodec18
ms.openlocfilehash: 232d85789c25e905873286eba6fda32c327a6e25
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "76276941"
---
# <a name="how-to-create-user-defined-functions-in-azure-digital-twins"></a>Создание определяемых пользователем функций в Azure Digital Twins

[Определяемые пользователем функции](./concepts-user-defined-functions.md) предоставляют пользователям возможность настраивать логику для выполнения из входящих сообщений телеметрии и метаданных пространственного графа. Это также позволит пользователям отправлять события в заранее определенные [конечные точки](./how-to-egress-endpoints.md).

В этом руководстве рассматривается пример, демонстрирующий, как обнаруживать и оповещать о любых показаниях, которые превышают определенную температуру в результате полученных температурных событий.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>Справочник по клиентской библиотеке

Функции, доступные в качестве вспомогательных методов в среде выполнения определяемых пользователем функций, перечислены в документе [Справочник по клиентским библиотекам определяемых пользователем функций](./reference-user-defined-functions-client-library.md).

## <a name="create-a-matcher"></a>Создание сопоставителя

Сопоставители как объекты графа хранят сведения о том, какие определяемые пользователем функции будут выполняться для некоторого сообщения телеметрии.

- Сравнения допустимых состояний сопоставителя:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Целевые показатели допустимых состояний сопоставителя:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

В приведенном ниже примере сопоставитель возвращает значение true для любого события телеметрии датчика с типом значения данных `"Temperature"`. Вы можете создать несколько сопоставлений сущностей на определяемую пользователем функцию, сделав аутентифицированный запрос HTTP POST.

```URL
YOUR_MANAGEMENT_API_URL/matchers
```

С помощью текста в формате JSON:

```JSON
{
  "id": "3626464-f39b-46c0-d9b0c-436aysj55",
  "name": "Temperature Matcher",
  "spaceId": "YOUR_SPACE_IDENTIFIER",
  "conditions": [
    {
      "id": "ag7gq35cfu3-e15a-4e9c-6437-sj6w68sy44s",
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ]
}
```

| Значение | Заменить на |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | Регион сервера, в котором размещен ваш экземпляр |

## <a name="create-a-user-defined-function"></a>создание определяемой пользователем функции;

Создание определяемой пользователем функции включает в себя составной HTTP-запрос к API управления Azure Digital Twins.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

После создания сопоставления загрузите фрагмент функции со следующим аутентифицированным составным HTTP-запросом POST в следующее расположение.

```URL
YOUR_MANAGEMENT_API_URL/userdefinedfunctions
```

Используйте следующий текст.

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "spaceId": "YOUR_SPACE_IDENTIFIER",
  "name": "User Defined Function",
  "description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Значение | Заменить на |
| --- | --- |
| USER_DEFINED_BOUNDARY | Имя границы составного содержимого |
| YOUR_SPACE_IDENTIFIER | Идентификатор пространства  |
| YOUR_MATCHER_IDENTIFIER | Идентификатор необходимого сопоставителя |

1. Убедитесь, что заголовки включают `Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
1. Убедитесь, что текст состоит из нескольких частей:

   - Первая часть содержит необходимые метаданные определяемой пользователем функции.
   - Вторая часть содержит логику вычислений на JavaScript.

1. В разделе **USER_DEFINED_BOUNDARY** замените значения **spaceId** (`YOUR_SPACE_IDENTIFIER`) и **matchers** (`YOUR_MATCHER_IDENTIFIER`).
1. Убедитесь, что определяемая пользователем функция JavaScript указана как `Content-Type: text/javascript`.

### <a name="example-functions"></a>Примеры функций

Установите чтение данных телеметрии датчика непосредственно для датчика с типом данных **Temperature**, который является `sensor.DataType`.

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

Параметр **телеметрии** предоставляет атрибуты **сенсорид** и **Message** , соответствующие сообщению, отправленному датчиком. Параметр **ExecutionContext** предоставляет следующие атрибуты.

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

В следующем примере мы сохраняет сообщение в журнал, если значение температуры из телеметрии датчика превысит предварительно заданный порог. Если для экземпляра Azure Digital Twins включены параметры диагностики, передаются также журналы из определяемых пользователем функций.

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

Следующий код активирует уведомление, когда уровень температуры превышает предопределенную константу.

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Чтобы получить более сложный пример кода определяемой пользователем функции, ознакомьтесь с [кратким руководством по заполнению](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Создание назначения роли

Создайте назначение роли для выполнения определяемой пользователем функции. Если для определяемой пользователем функции не назначена никакая роль, у нее не будет нужных разрешений для взаимодействия с API управления или доступа к выполнению действий над графическими объектами. Действия, которые может выполнять определяемая пользователем функция, задаются и определяются с помощью управления доступом на основе ролей в API управления Azure Digital Twins. Например, функции, определенные пользователем, можно ограничить по областям, указав определенные роли или пути управления доступом. Дополнительные сведения см. в документации по [управлению доступом на основе ролей](./security-role-based-access-control.md) .

1. [Запросите API системы](./security-create-manage-role-assignments.md#retrieve-all-roles) для всех ролей, чтобы получить идентификатор роли, которую необходимо назначить определяемой пользователем функции. Сделайте это, создавая аутентифицированный запрос HTTP GET.

    ```URL
    YOUR_MANAGEMENT_API_URL/system/roles
    ```
   Сохраните идентификатор требуемой роли. Он будет передан как атрибут текста JSON **roleId** (`YOUR_DESIRED_ROLE_IDENTIFIER`) ниже.

1. **ObjectID** (`YOUR_USER_DEFINED_FUNCTION_ID`) будет идентификатором определяемой пользователем функции, которая была создана ранее.
1. Найдите значение **path** (`YOUR_ACCESS_CONTROL_PATH`), запросив пробелы с помощью `fullpath`.
1. Скопируйте возвращенное значение `spacePaths`. Оно понадобится вам далее. Создайте аутентифицированный запрос HTTP GET.

    ```URL
    YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Значение | Заменить на |
    | --- | --- |
    | YOUR_SPACE_NAME | Имя необходимого пространства |

1. Вставьте возвращенное значение `spacePaths` в **path**, чтобы создать назначение роли определяемой пользователем функции, сделав аутентифицированный HTTP-запрос POST в следующее расположение.

    ```URL
    YOUR_MANAGEMENT_API_URL/roleassignments
    ```
    С помощью текста в формате JSON:

    ```JSON
    {
      "roleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "objectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "objectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | Значение | Заменить на |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | Идентификатор необходимой роли |
    | YOUR_USER_DEFINED_FUNCTION_ID | Идентификатор необходимой определяемой пользователем функции |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | Идентификатор, указывающий тип определяемой пользователем функции (`UserDefinedFunctionId`) |
    | YOUR_ACCESS_CONTROL_PATH | Путь для управления доступом |

>[!TIP]
> Дополнительные сведения об операциях API управления, связанных с определяемой пользователем функцией и конечными точками, см. в статье [Создание назначений ролей и управление ими в Azure Digital Twins](./security-create-manage-role-assignments.md).

## <a name="send-telemetry-to-be-processed"></a>Отправка данных телеметрии для обработки

Датчик, определенный на пространственном интеллектуальном графике, отправляет телеметрию. В свою очередь, телеметрия запускает выполнение пользовательской функции, которая была загружена. Обработчик данных получает данные телеметрии. Затем создается план выполнения для вызова определяемой пользователем функции.

1. Извлеките сопоставители для датчика, с которого было выполнено чтение.
1. В зависимости от того, что именно сопоставители успешно оценили, извлеките связанные определяемые пользователем функции.
1. Выполните каждую определяемую пользователем функцию.

## <a name="next-steps"></a>Следующие шаги

- Узнайте, как [создать конечные точки Azure Digital Twins](./how-to-egress-endpoints.md), чтобы отправлять в них события.

- Дополнительные сведения о маршрутизации событий и сообщений в Azure Digital Twins см. в [этой статье](./concepts-events-routing.md).

- См. статью [Справочник по клиентским библиотекам определяемых пользователем функций](./reference-user-defined-functions-client-library.md).