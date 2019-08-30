---
title: Поддержка платформы пакетов SDK для устройств Azure IoT | Документация Майкрософт
description: Основные понятия — список платформ, поддерживаемых пакетами SDK для устройств Azure IoT.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: robinsh
ms.openlocfilehash: 9a64c6e476910ab4fe983fa949680f05fdded3ae
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2019
ms.locfileid: "70161862"
---
# <a name="azure-iot-sdks-platform-support"></a>Поддержка платформ пакетов SDK для устройств Azure IoT

[Пакеты SDK для устройств Azure IoT](iot-hub-devguide-sdks.md) представляют собой набор библиотек для взаимодействия с Центром Интернета вещей и Службой подготовки устройств с расширенной поддержкой языков и платформ. Пакеты SDK работают на наиболее распространенных платформах. Разработчики могут перенести пакет C SDK на определенную платформу, следуя [руководству по переносу](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md). 

Корпорация Майкрософт поддерживает различные операционные системы, платформы, программы и может быть расширена с помощью пакета C SDK для Azure IoT. Некоторые из них официально поддерживаются командой, сгруппированной по категориям, которые предоставляют пользователям ожидаемый уровень поддержки. *Полностью поддерживаемая платформа* означает, что корпорация Майкрософт:

- Непрерывно создает и выполняет тщательное тестирование данных основной версии и версий с поддержкой LTS.  Чтобы охватить различные версии при тестировании, мы обычно тестируем последнюю версию LTS и наиболее популярную версию.  Поддержка других версий той же платформы может предоставляться через совместимость версий платформы.
- Предоставляет руководство по установке или пакеты, если они применяются.
- Полностью поддерживает платформы на сайте GitHub.

Кроме того, список партнеров выполнил перенос нашего пакета C SDK на другие платформы. Они обслуживают слой платформенных абстракций (PAL). [Каталог устройств Microsoft Azure Certified for IoT](https://catalog.azureiotsolutions.com/) также включает список платформ ОС, на базе которых были протестированы разные пакеты SDK. Пакеты SDK также регулярно создают на этих платформах (с ограниченным тестированием и поддержкой):

* MBED2;
* Arduino
* Windows CE 2013 (выйдет из эксплуатации с октября 2018 г.);
* .NET Standard 1.3 с .NET Core 2.1 и .NET Framework 4.7;
* Xamarin iOS, Android, UWP;

## <a name="supported-platforms"></a>Поддерживаемые платформы

Существует поддержка нескольких платформ.

### <a name="c-sdk"></a>Пакет C SDK

| OS                  | Arch | Компилятор             | Библиотека TLS       |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | gcc-5.4.0            | openssl — 1.0.2g |
| Ubuntu 18.04 LTS    | X64  | gcc-7.3              | WolfSSL — 1.13    |
| Ubuntu 18.04 LTS    | X64  | Clang 6.0.X          | OpenSSL — 1.1.0g  |
| OSX 10.13.4         | x64  | XCode 9.4.1          | Собственная библиотека OSX        |
| Windows Server 2016 | x64  | Visual Studio 14.0.X | SChannel          |
| Windows Server 2016 | x86  | Visual Studio 14.0.X | SChannel          |
| Debian 9 Stretch    | x64  | gcc-7.3              | OpenSSL — 1.1.0f  |

### <a name="python-sdk"></a>Пакет SDK для Python

| OS                  | Arch | Компилятор   | Библиотека TLS |
|---------------------|------|------------|-------------|
| Windows Server 2016 | x86  | Python 2,7 | OpenSSL     |
| Windows Server 2016 | x64  | Python 2,7 | OpenSSL     |
| Windows Server 2016 | x86  | Python 3.5 | OpenSSL     |
| Windows Server 2016 | x64  | Python 3.5 | OpenSSL     |
| Ubuntu 18.04 LTS    | x86  | Python 2,7 | OpenSSL     |
| Ubuntu 18.04 LTS    | x86  | Python 3.4 | OpenSSL     |
| MacOS High Sierra   | x64  | Python 2,7 | OpenSSL     |

### <a name="net-sdk"></a>Пакет SDK для .NET

| OS                  | Arch | FRAMEWORK            | Стандарт          |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | .NET Core 2.1        | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET Core 2.1        | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET Framework 4.7   | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET Framework 4.5.1 | Н/Д               |

### <a name="nodejs-sdk"></a>Пакет SDK для Node.js

| OS                                           | Arch | Версия узла    |
|----------------------------------------------|------|-----------------|
| Ubuntu 16.04 LTS (с использованием образа docker node 6) | X64  | LTS и текущий |
| Windows Server 2016                          | X64  | LTS и текущий |

### <a name="java-sdk"></a>Пакет SDK для Java

| OS                  | Arch | Версия Java |
|---------------------|------|--------------|
| Ubuntu 16.04 LTS    | X64  | Java 8       |
| Windows Server 2016 | X64  | Java 8       |
| API 28 для Android | X64  | Java 8       |
| Элементы Android | X64  | Java 8      |

## <a name="partner-supported-platforms"></a>Поддерживаемые платформы партнеров

Клиенты могут расширять поддержку платформы путем переноса пакета C SDK Azure IoT, в частности, создавая уровень платформенной абстракции (PAL) пакета SDK. Корпорация Майкрософт сотрудничает с партнерами для предоставления расширенной поддержки. Список партнеров, которые выполняют перенос пакета SDK C на другие платформы и поддерживают PAL.

| Партнер             | Устройства                            | Ссылка                     | Поддержка |
|---------------------|------------------------------------|--------------------------|---------|
| Espressif           | ESP32 <br/> ESP8266                              | [ESP-azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  
| Qualcomm            | Модем Qualcomm MDM9206 LTE IoT     | [Qualcomm LTE для пакета SDK IoT](https://developer.qualcomm.com/software/lte-iot-sdk) | [Форум](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST Microelectronics | Серии STM32L4 <br/> Серии STM32F4 <br/>  Серии STM32F7 <br/>  Пакет обнаружения STM32L4 для узла Интернета вещей    | [X-CUBE-CLOUD](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-cloud.html) <br/> [X-CUBE-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-azure.html) <br/> [P-NUCLEO AZURE](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html) <br/> [FP-CLD AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            | [Поддержка](https://www.st.com/content/st_com/en/support/support-home.html)
| Texas Instruments   | Панель запуска CC3220SF </br> Панель запуска CC3220S </br> Панель запуска CC3235SF </br> Панель запуска CC3235S </br> Панель запуска MSP432E4 | [Run a simple C sample on a SimpleLink device](https://github.com/TexasInstruments/azure-iot-pal-simplelink) (Запуск простого примера C на устройстве SimpleLink) | [Форум TI E2E](https://e2e.ti.com) <br/> [Форум TI E2E для CC3220](https://e2e.ti.com/support/wireless_connectivity/simplelink_wifi_cc31xx_cc32xx/) <br/> [Форум TI E2E для MSP432E4](https://e2e.ti.com/support/microcontrollers/msp430/) |

## <a name="next-steps"></a>Следующие шаги

* [Пакеты SDK для устройств и служб](iot-hub-devguide-sdks.md)
* [Руководство по переносу](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)
