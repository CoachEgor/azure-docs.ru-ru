---
title: Общие сведения о пакетах SDK для Azure IoT | Документация Майкрософт
description: Руководство разработчика. Сведения и ссылки на различные пакеты SDK для устройств и служб Azure IoT, которые можно использовать для создания приложений для устройств и внутренних приложений.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: be5fae45ee513dddf002995ce9c37c6b6565f50c
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "81258394"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Понимание и использование пакетов SDK для Центра Интернета вещей Azure

Есть две категории пакетов средств разработки (SDK) для работы с Центром Интернета вещей:

* **SDK-устройства IoT Hub позволяют** создавать приложения, которые работают на устройствах IoT с помощью клиента устройства или модуля. Эти приложения отправляют данные телеметрии в центр Интернета вещей и при необходимости получают из него сообщения, задания, методы или обновления двойников.  Вы также можете использовать клиент модуля для создания [модулей](../iot-edge/iot-edge-modules.md) для [среды выполнения Azure IoT Edge](../iot-edge/about-iot-edge.md).

* **SDK-службы концентратора IoT** позволяют создавать бэкэнд-приложения для управления концентратором IoT и дополнительно отправлять сообщения, планировать задания, вызывать прямые методы или отправлять желаемые обновления свойств на ваши устройства или модули.

Кроме того, мы также предоставляем набор пакетов SDK для работы с [Службой подготовки устройств](../iot-dps/about-iot-dps.md).
* **Пакеты SDK для устройств подготовки к работе** позволяют создавать приложения, которые запускаются на устройствах Интернета вещей, для связи со службой подготовки устройств.

* **Пакеты SDK для службы подготовки к работе** дают возможность создавать серверные приложения для управления вашей регистрации в Службе подготовки устройств.

Узнайте о [преимуществах разработки с использованием пакетов SDK для Azure IoT](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]


### <a name="os-platform-and-hardware-compatibility"></a>Платформа ОС и совместимость оборудования

Поддерживаемые платформы для пакетов SDK можно найти в [Поддержка платформ пакетов SDK для устройств Azure IoT](iot-hub-device-sdk-platform-support.md).

Дополнительные сведения о совместимости пакета SDK с определенными устройствами см. в [каталоге устройств, сертифицированных по программе Microsoft Azure Certified for IoT](https://catalog.azureiotsolutions.com/) или в описании для конкретного репозитория.

## <a name="azure-iot-hub-device-sdks"></a>Пакеты SDK для устройств Центра Интернета вещей Azure

Пакеты SDK для устройств Центра Интернета вещей Microsoft Azure содержат код, упрощающий построение приложений, которые подключаются к службам Центра Интернета вещей и управляются с помощью этих служб.

Пакет SDK для устройств Azure IoT для .NET: 

* Скачать с [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/).  Пространство имен — это Microsoft.Azure.Devices.Clients, который содержит клиенты устройств Центра Интернета вещей (DeviceClient, ModuleClient).
* [Исходный код](https://github.com/Azure/azure-iot-sdk-csharp)
* [Справочник по API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [Справочник по модулям](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet).

Пакет SDK для устройств Центра Интернета вещей Azure для C (ANSI C-C99):

* Установка из [apt-get, MBED, Arduino IDE или iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)
* [Исходный код](https://github.com/Azure/azure-iot-sdk-c)
* [Компиляция пакета SDK для устройств C](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#compiling-the-c-device-sdk).
* [Справочник по API](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
* [Справочник по модулям](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h).
* [Перенос пакета SDK C для других платформ](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)
* [Документация для разработчиков](https://github.com/Azure/azure-iot-sdk-c/tree/master/doc) содержит сведения о кроссплатформенной компиляции, начале работы на различных платформах и т. д.
* [Информация о потреблении ресурсов Azure IoT Hub C SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/c_sdk_resource_information.md)

Пакет SDK для устройств Azure IoT для Java: 

* добавление в проект [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk);
* [Исходный код](https://github.com/Azure/azure-iot-sdk-java)
* [Справочник по API](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device)
* [Справочник по модулям](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable).

Пакет SDK для устройств Azure IoT для Node.js: 

* установка из [npm](https://www.npmjs.com/package/azure-iot-device);
* [Исходный код](https://github.com/Azure/azure-iot-sdk-node)
* [Справочник по API](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest)
* [Справочник по модулям](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

Пакет SDK для устройств Azure IoT для Python: 

* установка из [pip](https://pypi.org/project/azure-iot-device/);
* [Исходный код](https://github.com/Azure/azure-iot-sdk-python)
* [Справочник по API](https://docs.microsoft.com/python/api/azure-iot-device)

Пакет SDK для устройств Центра Интернета вещей для iOS: 

* установка из [CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient);
* [Примеры](https://github.com/Azure-Samples/azure-iot-samples-ios)
* Справочник по API: см. [Справочная документация по API для C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/).

## <a name="azure-iot-hub-service-sdks"></a>Пакеты SDK для службы Центра Интернета вещей Azure

Пакеты SDK для службы Интернета вещей Azure содержат код, который облегчает создание приложений, взаимодействующих непосредственно с Центром Интернета вещей, для управления устройствами и безопасностью.

Пакет SDK для службы Центра Интернета вещей для .NET:

* Скачать с [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices/).  Пространство имен — это Microsoft.Azure.Devices, который содержит клиенты службы центра Интернета вещей (RegistryManager, ServiceClients).
* [Исходный код](https://github.com/Azure/azure-iot-sdk-csharp)
* [Справочник по API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices)

Пакет SDK для службы Центра Интернета вещей для Java: 

* добавление в проект [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk);
* [Исходный код](https://github.com/Azure/azure-iot-sdk-java)
* [Справочник по API](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service)

Пакет SDK для службы Центра Интернета вещей для Node.js: 

* скачивание из [npm](https://www.npmjs.com/package/azure-iothub);
* [Исходный код](https://github.com/Azure/azure-iot-sdk-node)
* [Справочник по API](https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest)

Пакет SDK для службы Центра Интернета вещей для Пакет SDK для Python: 

* скачивание из [pip](https://pypi.python.org/pypi/azure-iot-hub/);
* [Исходный код](https://github.com/Azure/azure-iot-sdk-python/tree/master)

Пакет SDK для службы Центра Интернета вещей для C: 

* Скачать с [apt-get, MBED, Arduino IDE, или NuGet](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Исходный код](https://github.com/Azure/azure-iot-sdk-c)

Пакет SDK для служб Центра Интернета вещей для iOS: 

* установка из [CocoaPod](https://cocoapods.org/pods/AzureIoTHubServiceClient);
* [Примеры](https://github.com/Azure-Samples/azure-iot-samples-ios)

> [!NOTE]
> Сведения об установке двоичных файлов и зависимостей на компьютере для разработки с помощью диспетчера пакетов, зависящего от языка или платформы, см. в файле сведений в репозиториях GitHub.

## <a name="microsoft-azure-provisioning-sdks"></a>Пакеты SDK для подготовки Microsoft Azure

**Пакеты SDK для подготовки Microsoft Azure** позволяют вам предоставлять устройства в Центр Интернета вещей с помощью [Службы подготовки устройств](../iot-dps/about-iot-dps.md).

Устройство подготовки Azure и пакеты SDK службы для C#:

* Скачивание из [пакета SDK для устройств](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Client/) и [пакета SDK службы](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) из NuGet.
* [Исходный код](https://github.com/Azure/azure-iot-sdk-csharp/)
* [Справочник по API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.provisioning.client?view=azure-dotnet)

Устройство подготовки Azure и пакеты SDK службы для C:

* Установка из [apt-get, MBED, Arduino IDE или iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)
* [Исходный код](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [Справочник по API](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

Устройство подготовки Azure и пакеты SDK службы для Java:

* добавление в проект [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk);
* [Исходный код](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning)
* [Справочник по API](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.provisioning.device?view=azure-java-stable)

Устройство подготовки Azure и пакеты SDK службы для Node.js:

* [Исходный код](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning)
* [Справочник по API](https://docs.microsoft.com/javascript/api/overview/azure/iothubdeviceprovisioning?view=azure-node-latest)
* Скачивание [пакета SDK для устройств](https://badge.fury.io/js/azure-iot-provisioning-device) и [пакета SDK службы](https://badge.fury.io/js/azure-iot-provisioning-service) из npm

Устройство подготовки Azure и пакеты SDK службы для Python:

* [Исходный код](https://github.com/Azure/azure-iot-sdk-python)
* Скачивание [пакета SDK для устройств](https://pypi.org/project/azure-iot-device/) и [пакета SDK службы](https://pypi.org/project/azure-iothub-provisioningserviceclient/) из pip

## <a name="next-steps"></a>Дальнейшие действия

Пакеты SDK для Azure IoT также предоставляют набор инструментов для разработки:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics) — средство кроссплатформенной командной строки для диагностирования проблем, связанных с подключением к Центру Интернета вещей;
* [azure-iot-explorer:](https://github.com/Azure/azure-iot-explorer)кросс-платформенное настольное приложение для подключения к вашему концентратору IoT и добавления/управления/связи с устройствами IoT.

Соответствующие документы, относящиеся к разработке с помощью пакетов SDK для Azure IoT.
* Дополнительные сведения о том, как управлять подключениями и надежным обменом сообщениями с помощью пакетов SDK центра Интернета вещей, см. в [этой статье](iot-hub-reliability-features-in-sdks.md).
* Дополнительные сведения см. в статье [Разработка для мобильных устройств с помощью пакетов SDK для Центра Интернета вещей Azure](iot-hub-how-to-develop-for-mobile-devices.md).
* [Поддержка платформ пакетов SDK для устройств Azure IoT](iot-hub-device-sdk-platform-support.md)


Другие справочные статьи в руководстве разработчика для Центра Интернета вещей:

* [Конечные точки Центра Интернета вещей](iot-hub-devguide-endpoints.md)
* [Язык запросов Центра Интернета вещей для двойников устройств, заданий и маршрутизации сообщений](iot-hub-devguide-query-language.md)
* [Квоты и регулирование](iot-hub-devguide-quotas-throttling.md)
* [Поддержка MQTT в Центре Интернета вещей](iot-hub-mqtt-support.md)
* [Справочник по REST API Центра Интернета вещей](/rest/api/iothub/)
