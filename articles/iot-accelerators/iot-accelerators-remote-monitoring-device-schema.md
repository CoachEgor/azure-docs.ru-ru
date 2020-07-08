---
title: Схема устройства в решении для удаленного мониторинга Azure | Документация Майкрософт
description: Эта статья содержит сведения о схеме JSON, которая определяет имитированное устройство в решении для удаленного мониторинга.
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 12/18/2018
ms.topic: conceptual
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: ac681bb13ccea49c7a2f566a6fcdb6adb8cec5bb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "81683747"
---
# <a name="understand-the-device-model-schema"></a>Общие сведения о схеме модели устройства

С помощью имитированных устройств в решении для удаленного мониторинга можно проверить его реакцию на событие. Решение для удаленного мониторинга включает в себя службу моделирования устройств для запуска имитированных устройств. При развертывании решения для удаленного мониторинга коллекция имитированных устройств подготавливается автоматически. Вы можете создать имитированные устройства или настроить имеющиеся.

В этой статье описана схема модели устройства, которая определяет возможности имитированного устройства и его реакцию на событие. Модель устройства хранится в JSON-файле.

> [!NOTE]
> Эта схема моделей устройств предназначена только для имитированных устройств, размещенных в службе моделирования устройств. Если вам требуется создать реальное устройство, см. статью [Подключение устройства к акселератору решения для удаленного мониторинга (Node.js)](iot-accelerators-connecting-devices.md).

В приведенных ниже статьях описаны связанные сведения.

* Статья [Implement the device model behavior](iot-accelerators-remote-monitoring-device-behavior.md) (Реакция модели устройства на событие) содержит сведения о файлах JavaScript, используемых для того, чтобы имитированное устройство реагировало на событие.
* Статья [Create a new simulated device](iot-accelerators-remote-monitoring-create-simulated-device.md) (Создание имитированного устройства) объединяет все сведения, а также описывает процедуру развертывания нового типа имитированного устройства в решение.

Вы узнаете, как выполнять следующие задачи:

>[!div class="checklist"]
> * использование JSON-файла для определения модели имитированного устройства;
> * указание свойств имитированного устройства;
> * указание данных телеметрии, отправляемых имитированным устройством;
> * указание методов передачи из облака на устройство, на которые отвечает устройство.

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

| Запись схемы | Описание: |
| -- | --- |
| `SchemaVersion` | Версия схемы всегда будет `1.0.0`. Она соответствует формату этого файла. |
| `Id` | Уникальный идентификатор для модели устройства. |
| `Version` | Определяет версию модели устройства. |
| `Name` | Понятное имя модели устройства. |
| `Description` | Описание модели устройства. |
| `Protocol` | Протокол подключения, который использует устройство. Возможные значения: `AMQP`, `MQTT` и `HTTP`. |

В следующих разделах описаны другие разделы схемы JSON.

## <a name="simulation"></a>Simulation

В разделе `Simulation` определите внутреннее состояние имитированного устройства. Все значения телеметрии, отправленные устройством, должны быть частью состояния устройства.

Определение состояния устройства состоит из двух элементов:

* `InitialState` определяет начальные значения для всех свойств объекта состояния устройства.
* `Script` указывает файл JavaScript, который выполняется по расписанию, чтобы обновить состояние устройства. Вы можете использовать файл сценария, чтобы создать случайные значения телеметрии, отправленные устройством.

Дополнительные сведения о файле JavaScript, который обновляет объект состояния устройства, см. в статье [Implement the device model behavior](../../articles/iot-accelerators/iot-accelerators-device-simulation-advanced-device.md) (Реакция модели устройства на событие).

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

## <a name="properties"></a>Элемент Property

Раздел `Properties` схемы определяет значения свойств, которые устройство передает в решение. Пример:

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
* текст
* Логическое
* Целое число
* Double
* Дата и время

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

Дополнительные сведения о файле JavaScript, который реализует реагирование устройства, см. в статье [Implement the device model behavior](../../articles/iot-accelerators/iot-accelerators-device-simulation-advanced-device.md) (Реакция модели устройства на событие).

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

## <a name="next-steps"></a>Дальнейшие шаги

В этой статье описано, как создать пользовательскую модель имитированного устройства. Из этой статьи вы узнали, как выполнять следующие задачи:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * использование JSON-файла для определения модели имитированного устройства;
> * указание свойств имитированного устройства;
> * указание данных телеметрии, отправляемых имитированным устройством;
> * указание методов передачи из облака на устройство, на которые отвечает устройство.

Теперь, когда вы узнали о схеме JSON, предлагаем ознакомиться со статьей [Реализация режима работы модели устройства](iot-accelerators-remote-monitoring-device-behavior.md).

Дополнительные сведения для разработчиков о решении для удаленного мониторинга см. в следующих источниках:

* [Справочник разработчика](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Руководство по устранению неполадок для разработчиков](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)
