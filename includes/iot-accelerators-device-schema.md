---
title: включение файла
description: включение файла
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 07/26/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 414bb0183e68cb46e52c379ea3f7aceda5d4170e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61446139"
---
## <a name="the-parts-of-the-device-model-schema"></a>Части схемы модели устройства

Каждая модель устройства, например chiller или truck, определяет тип устройства, которое служба моделирования может имитировать. Все модели устройства хранятся в JSON-файле со следующей схемой верхнего уровня:

```json
{
  "SchemaVersion": "1.0.0",
  "Id": "elevator-01",
  "Version": "0.0.1",
  "Name": "Elevator",
  "Description": "Elevator with floor, vibration and temperature sensors.",
  "Protocol": "AMQP",
  "Simulation": {
    // Specify the simulation behavior
  },
  "Properties": {
    // Define properties
  },
  "Telemetry": [
    // Specify telemetry
  ],
  "CloudToDeviceMethods": {
    // Specify methods
  }
}
```

Вы можете просмотреть файлы схемы для стандартных имитированных устройств в [папке devicemodels](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels) на GitHub.

В следующей таблице описаны записи схемы верхнего уровня:

| Запись схемы | ОПИСАНИЕ |
| -- | --- |
| `SchemaVersion` | Версия схемы всегда будет `1.0.0`. Она соответствует формату этого файла. |
| `Id` | Уникальный идентификатор для модели устройства. |
| `Version` | Определяет версию модели устройства. |
| `Name` | Понятное имя модели устройства. |
| `Description` | Описание модели устройства. |
| `Protocol` | Протокол подключения, который использует устройство. Возможные значения: `AMQP`, `MQTT` и `HTTP`. |

В следующих разделах описаны другие разделы схемы JSON.

## <a name="simulation"></a>Моделирование

В разделе `Simulation` определите внутреннее состояние имитированного устройства. Все значения телеметрии, отправленные устройством, должны быть частью состояния устройства.

Определение состояния устройства состоит из двух элементов:

* `InitialState` определяет начальные значения для всех свойств объекта состояния устройства.
* `Script` указывает файл JavaScript, который выполняется по расписанию, чтобы обновить состояние устройства. Вы можете использовать файл сценария, чтобы создать случайные значения телеметрии, отправленные устройством.

Дополнительные сведения о файле JavaScript, который обновляет объект состояния устройства, см. в статье [Implement the device model behavior](../articles/iot-accelerators/iot-accelerators-device-simulation-device-behavior.md) (Реакция модели устройства на событие).

В следующем примере приводится определение объекта состояния устройства для имитированного устройства Chiller:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Interval": "00:00:10",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

Служба моделирования запускает файл **chiller-01-state.js** каждые 5 секунд, чтобы обновить состояние устройства. Вы можете просмотреть файлы JavaScript для стандартных имитированных устройств в [папке сценариев](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) на GitHub. Обычно эти файлы JavaScript имеют суффикс **-state**. Это позволяет отличить их от файлов, которые реализуют метод реакции на события.

## <a name="properties"></a>properties

Раздел `Properties` схемы определяет значения свойств, которые устройство передает в решение. Например: 

```json
"Properties": {
  "Type": "Elevator",
  "Location": "Building 2",
  "Latitude": 47.640792,
  "Longitude": -122.126258
}
```

При запуске решения оно отправляет запросы во все имитированные устройства на создание списка значений `Type` для использования в пользовательском интерфейсе. Решение использует свойства `Latitude` и `Longitude` для добавления расположения устройства к карте на панели мониторинга.

## <a name="telemetry"></a>Телеметрия

Массив `Telemetry` выводит список всех типов телеметрии, которые имитированное устройство отправляет в решение.

В следующем примере сообщение телеметрии JSON отправляется каждые 10 секунд с данными `floor`, `vibration` и `temperature`, собранными c датчиков лифта.

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate": "{\"floor\":${floor},\"vibration\":${vibration},\"vibration_unit\":\"${vibration_unit}\",\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
    "MessageSchema": {
      "Name": "elevator-sensors;v1",
      "Format": "JSON",
      "Fields": {
        "floor": "integer",
        "vibration": "double",
        "vibration_unit": "text",
        "temperature": "double",
        "temperature_unit": "text"
      }
    }
  }
]
```

`MessageTemplate` определяет структуру сообщения JSON, отправленного имитированным устройством. Заполнители в `MessageTemplate` используют синтаксис `${NAME}`, где `NAME` — это ключ [объекта состояния устройства](#simulation). Строки следует заключить в кавычки, а числа — нет.

`MessageSchema` определяет схему сообщения, отправленного имитированным устройством. Схема сообщения также публикуется в Центр Интернета вещей. Это позволяет серверным приложениям повторно использовать сведения для интерпретации входящей телеметрии.

Сейчас вы можете использовать только схемы сообщений JSON. Поля, перечисленные в схеме, могут быть следующих типов:

* Object — можно сериализовать с помощью JSON.
* Binary — можно сериализовать с помощью Base64.
* Text
* Boolean
* Целое число 
* Double
* DateTime

Чтобы отправить сообщения телеметрии с разными интервалами, добавьте несколько типов телеметрии к массиву `Telemetry`. В следующем примере данные температуры и влажности отправляются каждые 10 секунд, а сведения о состоянии освещения — каждую минуту.

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate":
      "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"humidity\":\"${humidity}\"}",
    "MessageSchema": {
      "Name": "RoomComfort;v1",
      "Format": "JSON",
      "Fields": {
        "temperature": "double",
        "temperature_unit": "text",
        "humidity": "integer"
      }
    }
  },
  {
    "Interval": "00:01:00",
    "MessageTemplate": "{\"lights\":${lights_on}}",
    "MessageSchema": {
      "Name": "RoomLights;v1",
      "Format": "JSON",
      "Fields": {
        "lights": "boolean"
      }
    }
  }
],
```

## <a name="cloudtodevicemethods"></a>CloudToDeviceMethods

Имитированное устройство может отвечать на вызовы методов передачи информации из облака на устройство, поступающих из Центра Интернета вещей. Раздел `CloudToDeviceMethods` в файле схемы модели устройства позволяет выполнить следующие действия:

* Определить методы, на которые имитированное устройство может отвечать.
* Указать файл JavaScript, который содержит логику для выполнения.

Имитированное устройство отправляет список поддерживаемых методов в Центр Интернета вещей.

Дополнительные сведения о файле JavaScript, который реализует реагирование устройства, см. в статье [Implement the device model behavior](../articles/iot-accelerators/iot-accelerators-device-simulation-device-behavior.md) (Реакция модели устройства на событие).

В следующем примере указаны три поддерживаемых метода и файлы JavaScript, которые их реализуют.

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

Вы можете просмотреть файлы JavaScript для стандартных имитированных устройств в [папке сценариев](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) на GitHub. Обычно эти файлы JavaScript имеют суффикс **-method**. Это позволяет отличить их от файлов, которые реализуют поведение состояния.