---
title: Use different attestation mechanisms with the Azure IoT Hub Device Provisioning Service Client SDK
description: Практическое руководство Azure. Использование разных механизмов аттестации с клиентским пакетом SDK для службы подготовки устройств в Azure
author: robinsh
ms.author: robinsh
ms.date: 03/30/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 0cde591d2ec8c6f2f51c83b3f263c188c8cf2605
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74228272"
---
# <a name="how-to-use-different-attestation-mechanisms-with-device-provisioning-service-client-sdk-for-c"></a>Использование разных механизмов аттестации с клиентским пакетом SDK для службы подготовки устройств в Azure для С

В этой статье объясняется, как использовать разные [механизмы аттестации](concepts-security.md#attestation-mechanism) с помощью клиентского пакета SDK для C для службы подготовки устройств. Вы можете использовать физическое устройство или симулятор. The provisioning service supports authentication for two types of attestation mechanisms: X.509 and Trusted Platform Module (TPM).

## <a name="prerequisites"></a>Технические условия

Подготовьте среду разработки в соответствии с инструкциями в разделе "Подготовка среды разработки" руководства по [созданию и подготовке имитированного устройства](./quick-create-simulated-device.md).

### <a name="choose-an-attestation-mechanism"></a>Выбор механизма аттестации

Производителю устройства прежде всего необходимо выбрать механизм аттестации с учетом одного из поддерживаемых типов. Сейчас [клиентский пакет SDK для C для службы подготовки устройств](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client) поддерживает следующие механизмы аттестации: 

- [Доверенный платформенный модуль (TPM)](https://en.wikipedia.org/wiki/Trusted_Platform_Module) является установленным стандартом для большинства платформ устройств на базе Windows, а также некоторых устройств на базе Linux и Ubuntu. Производитель устройства может выбрать этот механизм аттестации, если устройства работают под управлением любой из этих операционных систем, а также если необходим установленный стандарт. С микросхемами доверенного платформенного модуля (TPM) можно только регистрировать каждое устройство отдельно в службе подготовки устройств. В целях разработки можно использовать симулятор доверенного платформенного модуля на компьютере разработки Windows или Linux.

- [X.509](https://cryptography.io/en/latest/x509/): сертификаты X.509, могут храниться в относительно новых микросхемах, которые называются [аппаратными модулями безопасности (HSM)](concepts-security.md#hardware-security-module). Корпорация Майкрософт также работает над микросхемами RIoT и DICE, в которых реализуются сертификаты X.509. С помощью микросхем X.509 можно выполнять массовую регистрацию устройств на портале. Они также поддерживают некоторые операционные системы, отличные от Windows, такие как embedOS. В целях разработки клиентский пакет SDK службы подготовки устройств поддерживает симулятор устройств X.509. 

См. дополнительные сведения о [защите, обеспечиваемой службой "Подготовка устройств к добавлению в Центр Интернета вещей"](concepts-security.md) и [автоматической подготовке](/azure/iot-dps/concepts-auto-provisioning).

## <a name="enable-authentication-for-supported-attestation-mechanisms"></a>Включение аутентификации для поддерживаемых механизмов аттестации

The SDK authentication mode (X.509 or TPM) must be enabled for the physical device or simulator before they can be enrolled in the Azure portal. Сначала перейдите в корневую папку для azure-iot-sdk-c. Затем выполните указанную команду в соответствии с выбранным режимом аутентификации.

### <a name="use-x509-with-simulator"></a>Use X.509 with simulator

The provisioning service ships with a Device Identity Composition Engine (DICE) emulator that generates an **X.509** certificate for authenticating the device. To enable **X.509** authentication, run the following command: 

```
cmake -Ddps_auth_type=x509 ..
```

Сведения, касающиеся оборудования с DICE, можно найти [здесь](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/).

### <a name="use-x509-with-hardware"></a>Use X.509 with hardware

The provisioning service can be used with **X.509** on other hardware. Для обмена данными между оборудованием и пакетом SDK требуется интерфейс. Сведения об интерфейсе можно получить у производителя HSM.

### <a name="use-tpm"></a>Использование TPM

Служба подготовки может подключаться к микросхемам TPM оборудования Windows и Linux с использованием маркера SAS. Чтобы включить аутентификацию TPM, выполните следующую команду:

```
cmake -Ddps_auth_type=tpm ..
```

### <a name="use-tpm-with-simulator"></a>Использование TPM с симулятором

Если у вас нет устройства с микросхемами TPM, можно использовать симулятор для разработки в ОС Windows. Чтобы включить аутентификацию TPM и запустить симулятор TPM, выполните следующую команду:

```
cmake -Ddps_auth_type=tpm_simulator ..
```

## <a name="build-the-sdk"></a>Сборка пакета SDK
Перед регистрацией устройства выполните сборку пакета SDK.

### <a name="linux"></a>Linux
- Вот как выполнить сборку SDK в Linux:
    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    cmake ..
    cmake --build .  # append '-- -j <n>' to run <n> jobs in parallel
    ```
- Чтобы создать двоичные файлы отладки, добавьте соответствующий параметр CMake в команду создания проекта, например:
    ```
    cmake -DCMAKE_BUILD_TYPE=Debug ..
    ```

- Для создания пакета SDK доступны разные [варианты конфигурации CMake](https://cmake.org/cmake/help/v3.6/manual/cmake.1.html). Например, можно отключить один из доступных стеков протоколов, добавив аргумент в команду создания проекта CMake:
    ```
    cmake -Duse_amqp=OFF ..
    ```
- Кроме того, можно создать и запустить модульный тест:
    ```
    cmake -Drun_unittests=ON ..
    cmake --build .
    ctest -C "debug" -V
    ```

### <a name="windows"></a>Windows
- Чтобы выполнить сборку пакета SDK в Windows, следуйте инструкциям по созданию файлов проекта ниже:
  - Откройте командную строку разработчика для VS2015.
  - Выполните следующие команды CMake из корня репозитория:
    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    cmake -G "Visual Studio 14 2015" ..
    ```
    Эта команда отвечает за создание библиотек x86. Для создания x64 измените аргумент генератора cmake: 
    ```
    cmake .. -G "Visual Studio 14 2015 Win64"
    ```

- После успешного создания проекта в папке `cmake` появится файл решения Visual Studio (.sln). Чтобы создать пакет SDK,
    - откройте **cmake\azure_iot_sdks.sln** в Visual Studio и выполните сборку **ИЛИ**
    - выполните следующую команду в командной строке, которая использовалась для создания файлов проекта:
      ```
      cmake --build . -- /m /p:Configuration=Release
      ```
- Чтобы выполнить сборку двоичных файлов отладки, используйте соответствующий аргумент MSBuild: 
    ```
    cmake --build . -- /m /p:Configuration=Debug`
    ```
- Для создания пакета SDK доступны разные варианты конфигурации CMake. Например, можно отключить один из доступных стеков протоколов, добавив аргумент в команду создания проекта CMake:
    ```
    cmake -G "Visual Studio 14 2015" -Duse_amqp=OFF ..
    ```
- Кроме того, можно создать и запустить модульные тесты:
    ```
    cmake -G "Visual Studio 14 2015" -Drun_unittests=ON ..
    cmake --build . -- /m /p:Configuration=Debug
    ctest -C "debug" -V
    ```
  
### <a name="libraries-to-include"></a>Библиотеки, которые необходимо включить
- В пакет SDK требуется включить следующие библиотеки:
    - служба подготовки — dps_http_transport dps_client, dps_security_client;
    - безопасность Центра Интернета вещей — iothub_security_client.

## <a name="create-a-device-enrollment-entry-in-device-provisioning-services"></a>Создание записи регистрации устройства в службе подготовки устройств

### <a name="tpm"></a>TPM
Если вы используете TPM, следуйте инструкциям из статьи [Создание и подготовка имитированного устройства доверенного платформенного модуля (ТРМ) с помощью пакета SDK для устройства C для службы подготовки устройств Центра Интернета вещей](./quick-create-simulated-device.md), чтобы создать запись регистрации устройств в службе подготовки устройств и смоделировать первую загрузку.

### <a name="x509"></a>X.509

1. Чтобы зарегистрировать устройство в службе подготовки, запишите ключ подтверждения и идентификатор регистрации для каждого устройства, которое отображается в средстве подготовки клиентского пакета SDK. Выполните следующую команду, чтобы распечатать сертификат корневого ЦС (для групп регистрации) и конечный сертификат подписавшего (для отдельной регистрации):
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Войдите на портал Azure, нажмите кнопку **Все ресурсы** в меню слева и откройте службу подготовки устройств.
   - **X.509 Individual Enrollment**: On the provisioning service summary blade, select **Manage enrollments**. На вкладке **Отдельные регистрации** и нажмите кнопку **Добавить** сверху. Select **X.509** as the identity attestation *Mechanism*, upload the leaf certificate as required by the blade. После завершения нажмите кнопку **Сохранить**. 
   - **X.509 Group Enrollment**: On the provisioning service  summary blade, select **Manage enrollments**. Выберите вкладку **Групповые регистрации** и нажмите кнопку **Добавить** вверху. Select **X.509** as the identity attestation *Mechanism*, enter a group name and certification name, upload the CA/Intermediate certificate as required by the blade. После завершения нажмите кнопку **Сохранить**. 

## <a name="enable-authentication-for-devices-using-a-custom-attestation-mechanism-optional"></a>Включение аутентификации для устройств с помощью пользовательских механизмов аттестации (необязательно)

> [!NOTE]
> Этот раздел применим только для устройств, которым требуется поддержка пользовательских платформ или механизмов аттестации, которая не предоставляется клиентским пакетом SDK для службы подготовки устройств для С. Кроме того, для пакета SDK в качестве замены термина "механизм аттестации" часто используется термин "HSM".

Сначала необходимо разработать репозиторий и библиотеку для пользовательского механизма аттестации:

1. Разработайте библиотеку для доступа к механизму аттестации. Для этого проекта необходимо создать статическую библиотеку пакета SDK для подготовки устройств.

2. Библиотека должна реализовывать функции, определенные в следующем файле заголовка: 

    - Для пользовательского доверенного платформенного модуля: реализуйте функции, определенные в разделе [HSM TPM API](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-tpm-api).  
    - Для пользовательского сертификата X.509: реализуйте функции, определенные в разделе [HSM X.509 API](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-x509-api). 

Созданную библиотеку нужно интегрировать (связать) с клиентским пакетом SDK для службы подготовки устройств. :

1. Укажите пользовательский репозиторий GitHub и библиотеку в команде `cmake`:
    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=<path_and_name_of_library> <PATH_TO_AZURE_IOT_SDK>
    ```
   
2. Откройте файл решения Visual Studio, созданный с помощью CMake (`\azure-iot-sdk-c\cmake\azure_iot_sdks.sln`), и выполните его сборку. 

    - В ходе сборки будет скомпилирована библиотека пакета SDK.
    - Пакет SDK попытается связать пользовательскую библиотеку, определенную в команде `cmake`.

3. Чтобы проверить правильность реализации пользовательского механизма аттестации, запустите пример приложения prov_dev_client_ll_sample в папке Provision_Samples (`\azure-iot-sdk-c\cmake\provisioning_client\samples\prov_dev_client_ll_sample`).

## <a name="connecting-to-iot-hub-after-provisioning"></a>Подключение к Центру Интернета вещей после подготовки

Once the device has been provisioned with the provisioning service, this API uses the specified authentication mode (**X.509** or TPM) to connect with IoT Hub: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```
