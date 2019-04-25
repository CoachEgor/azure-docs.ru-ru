---
title: Управление устройствами Интернета вещей Azure с помощью расширения Интернета вещей для Azure CLI | Документы Майкрософт
description: С помощью расширения Интернета вещей для Azure CLI можно управлять устройствами центра Интернета вещей Azure, используя прямые методы и возможности управления требуемыми свойствами двойника.
author: chrissie926
manager: ''
keywords: управление устройствами Интернета вещей Azure, управление устройствами в Центре Интернета вещей Azure, управление устройствами, Интернет вещей, управление устройствами в Центре Интернета вещей
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: 6b1029c5532e106c269b47e6e184b9c93faf8d09
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60399629"
---
# <a name="use-the-iot-extension-for-azure-cli-for-azure-iot-hub-device-management"></a>Управление устройствами центра Интернета вещей Azure с помощью расширения Интернета вещей для Azure CLI

![Комплексная схема](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Расширение центра Интернета вещей для Azure CLI](https://github.com/Azure/azure-iot-cli-extension) является новым расширением центра Интернета вещей с открытым кодом, увеличивающим возможности [Azure CLI](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest). Azure CLI содержит команды для взаимодействия с Azure Resource Manager и конечными точками управления. Например, Azure CLI можно использовать для создания центра Интернета вещей или виртуальной машины Azure. Расширение CLI включает службу Azure для ускорения работы Azure CLI, за счет чего вы получаете доступ к дополнительным определенным функциям службы. Расширение Интернета вещей обеспечивает для разработчиков решений Интернета вещей доступ ко всем возможностям Центра Интернета вещей, IoT Edge и службе "Подготовка устройств к добавлению в Центр Интернета вещей" из командной строки.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Возможность управления          | Задача  |
|----------------------------|-----------|
| Прямые методы             | Выполнение на устройстве таких действий, как запуск и остановка отправки сообщений или перезагрузка устройства.                                        |
| Требуемые свойства двойников    | Перевод устройства в определенные состояния, например включение зеленого светодиодного индикатора или установка 30-минутного интервала отправки данных телеметрии.         |
| Сообщаемые свойства двойника   | Получение зарегистрированного состояния устройства, например данных о том, что сейчас на устройстве мигает индикатор.                                    |
| Теги двойников                  | Хранение метаданных для конкретного устройства в облаке. Например, расположение развертывания торгового автомата.                         |
| Запросы двойника устройства        | Запрос всех двойников устройства для получения тех из них, которые соответствуют произвольным условиям, например для определения устройств, которые доступны для использования. |

Более подробное объяснение различий и рекомендации по использованию этих параметров см. в статьях [Руководство по обмену данными между устройством и облаком](iot-hub-devguide-d2c-guidance.md) и [Руководство по обмену данными между облаком и устройством](iot-hub-devguide-c2d-guidance.md).

Двойники устройств — это документы JSON, хранящие сведения о состоянии устройства (метаданные, конфигурации и условия). Центр Интернета вещей сохраняет двойник устройства для каждого устройства, подключаемого к нему. Дополнительные сведения о двойниках устройства см. в статье [Начало работы с двойниками устройств](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Что вы узнаете

Вы узнаете, как использовать расширение Интернета вещей для Azure CLI с разными параметрами управления на компьютере разработки.

## <a name="what-you-do"></a>Что нужно сделать

Запустите Azure CLI и расширение Интернета вещей для него с разными параметрами управления.

## <a name="what-you-need"></a>Необходимые элементы

* Завершить [онлайн-симулятор Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md) руководства или одним из руководств устройства; например, [Raspberry Pi с помощью node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Они охватывают следующие требования:

  - Активная подписка Azure.
  - Центр Интернета вещей Azure в подписке;
  - клиентское приложение, которое отправляет сообщения в Центр Интернета вещей Azure.

* Во время работы с этим руководством на устройстве должно работать клиентское приложение.

* [Python 2.7x или Python 3.x](https://www.python.org/downloads/)

* Azure CLI. Если вам необходимо выполнить установку, см. статью [Установка Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Вам понадобится как минимум Azure CLI версии 2.0.24 или более поздней. Для проверки используйте `az –version`. 

* Установите расширение Интернета вещей. Проще всего запустить `az extension add --name azure-cli-iot-ext`. В [файле сведений расширения Интернета вещей](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) описывается несколько способов установки расширения.

## <a name="log-in-to-your-azure-account"></a>Вход в учетную запись Azure

Войдите в свою учетную запись Azure с помощью следующей команды:

```bash
az login
```

## <a name="direct-methods"></a>Прямые методы

```bash
az iot hub invoke-device-method --device-id <your device id> \
  --hub-name <your hub name> \
  --method-name <the method name> \
  --method-payload <the method payload>
```

## <a name="device-twin-desired-properties"></a>Требуемые свойства двойника устройства

Установите для требуемого свойства интервал 3000, выполнив следующую команду:

```bash
az iot hub device-twin update -n <your hub name> \
  -d <your device id> --set properties.desired.interval = 3000
```

Устройство может прочитать это свойство.

## <a name="device-twin-reported-properties"></a>Сообщаемые свойства двойника устройства

Получите сообщаемые свойства устройства, выполнив следующую команду:

```bash
az iot hub device-twin show -n <your hub name> -d <your device id>
```

Одно из сообщаемых свойств ($metadata.$lastUpdated) указывает время последнего обновления набора передаваемых свойств приложением для устройства.

## <a name="device-twin-tags"></a>Теги двойника устройства

Отобразите теги и свойства устройства, выполнив следующую команду:

```bash
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Добавьте на устройство поле role = temperature&humidity, выполнив следующую команду:

```bash
az iot hub device-twin update \
  --hub-name <your hub name> \
  --device-id <your device id> \
  --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="device-twin-queries"></a>Запросы двойника устройства

Отправьте запрос на предоставление данных устройств с тегом роли 'temperature&humidity', выполнив следующую команду:

```bash
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Отправьте запрос на предоставление данных всех устройств, кроме тех, для которых задан тег роли 'temperature&humidity', выполнив следующую команду:

```bash
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Дальнейшие действия

Вы узнали, как отслеживать сообщения, отправляемые из устройства Интернета вещей в облако Центра Интернета вещей, и отправлять сообщения из этого облака на устройство.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
