---
title: Provision legacy devices using symmetric keys - Azure IoT Hub Device Provisioning Service
description: Подготовка устаревших устройств с использованием симметричных ключей в экземпляре службы подготовки устройств.
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 3e3b54592608f5c39d618f5ceda40747ad4fd0fe
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74209919"
---
# <a name="how-to-provision-legacy-devices-using-symmetric-keys"></a>Как подготовить устаревшие устройства с использованием симметричных ключей

Распространенной проблемой многих устаревших устройств является то, что у них часто есть идентификатор, который представляет собой одну единицу информации. Этими идентификационными данными обычно выступает MAC-адрес или серийный номер. В устаревших устройствах может отсутствовать сертификат, доверенный платформенный модуль (TPM) или любая другая функция безопасности, которую можно использовать для безопасной идентификации устройства. Служба подготовки устройств к добавлению в Центр Интернета вещей включает аттестацию симметричных ключей. Аттестацию симметричных ключей можно использовать для идентификации устройства на основе таких данных, как MAC-адрес или серийный номер.

Если вы можете легко установить [аппаратный модуль безопасности (HSM)](concepts-security.md#hardware-security-module) и сертификат, то это может быть лучшим подходом к идентификации и подготовке устройств. Этот подход позволяет обойти обновления кода, развернутого на все устройства, поэтому у вас отпадет необходимость в секретном ключе, внедренном в образ устройства.

В этой статье предполагается, что ни HSM, ни сертификат не являются приемлемым вариантом. Тем не менее предполагается, что существует некий метод обновления кода устройств для подготовки этих устройств с помощью службы подготовки. 

В этой статье также предполагается, что обновление устройства выполняется в защищенной среде, чтобы предотвратить несанкционированный доступ к главному ключу группы или производному ключу устройства.

В этой статье описывается использование рабочей станции под управлением Windows. Тем не менее эти процедуры можно выполнить и на Linux. Пример для Linux см. в статье [Подготовка к мультитенантности](how-to-provision-multitenant.md).

> [!NOTE]
> The sample used in this article is written in C. There is also a [C# device provisioning symmetric key sample](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/provisioning/Samples/device/SymmetricKeySample) available. To use this sample, download or clone the [azure-iot-samples-csharp](https://github.com/Azure-Samples/azure-iot-samples-csharp) repository and follow the in-line instructions in the sample code. You can follow the instructions in this article to create a symmetric key enrollment group using the portal and to find the ID Scope and enrollment group primary and secondary keys needed to run the sample. You can also create individual enrollments using the sample.

## <a name="overview"></a>Краткое описание

Уникальный идентификатор регистрации будет определен для каждого устройства на основе сведений, идентифицирующих это устройство. Например, MAC-адреса или серийного номера.

Служба подготовки устройств создаст группу регистраций, использующую [аттестацию симметричного ключа](concepts-symmetric-key-attestation.md). Группа регистраций будет содержать главный ключ группы. Этот главный ключ будет использоваться для хэширования каждого уникального идентификатора регистрации, чтобы получить уникальный ключ для каждого устройства. Устройство будет использовать этот производный ключ устройства вместе с уникальным идентификатором регистрации для аттестации с помощью службы подготовки устройств и будет назначено Центру Интернета вещей.

Код устройства, приведенный в этой статье, следует тому же шаблону, который используется в [кратком руководстве по подготовке имитированного устройства с использованием симметричных ключей](quick-create-simulated-device-symm-key.md). Код будет имитировать устройство с использованием примера из [пакета SDK Azure IoT для C](https://github.com/Azure/azure-iot-sdk-c). Имитированное устройство пройдет аттестацию с группой регистраций вместо отдельной регистрации, как показано в кратком руководстве.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Технические условия

* Выполните процедуру, описанную в кратком руководстве по [настройке службы подготовки устройств Центра Интернета вещей на портале Azure](./quick-setup-auto-provision.md).
* [Visual Studio 2015](https://visualstudio.microsoft.com/vs/) или более поздней версии с включенной рабочей нагрузкой [Разработка классических приложений на C++](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/).
* Установите последнюю версию [Git](https://git-scm.com/download/).


## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Подготовка среды разработки для пакета SDK Azure IoT для C

В этом разделе вы подготовите среду разработки, которая используется для сборки [пакета SDK Azure IoT для C](https://github.com/Azure/azure-iot-sdk-c). 

В пакет SDK входит пример кода для имитированного устройства. Для этого имитированного устройства будет выполнена попытка подготовки во время последовательности загрузки.

1. Скачайте [систему сборки CMake](https://cmake.org/download/).

    **Перед** установкой `CMake` очень важно установить на компьютер необходимые компоненты Visual Studio (Visual Studio с рабочей нагрузкой "Разработка классических приложений на C++"). После установки компонентов и проверки загрузки установите систему сборки CMake.

2. Откройте командную строку или оболочку Git Bash. Выполните следующую команду для клонирования репозитория GitHub пакета SDK Azure IoT для C:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Выполнение этой операции может занять несколько минут.


3. Создайте подкаталог `cmake` в корневом каталоге репозитория Git и перейдите в эту папку. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Выполните приведенную ниже команду, чтобы создать версию пакета SDK для используемой клиентской платформы разработки. Эта команда также создает решение Visual Studio для имитированного устройства в каталоге `cmake`. 

    ```cmd
    cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    ```
    
    Если `cmake` не удастся найти компилятор C++, могут возникнуть ошибки сборки во время выполнения предыдущей команды. В этом случае попробуйте, выполнить эту команду в [командной строке Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    После успешного создания последние несколько строк выходных данных будут выглядеть следующим образом:

    ```cmd/sh
    $ cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```


## <a name="create-a-symmetric-key-enrollment-group"></a>Создание группы регистраций симметричного ключа

1. Войдите на [портал Azure](https://portal.azure.com) и перейдите к своему экземпляру службы подготовки устройств.

2. Выберите вкладку **Управление регистрациями**, а затем нажмите кнопку **Добавить группу регистрации** в верхней части страницы. 

3. В разделе **Добавление группы регистрации** введите следующие сведения ниже, а затем нажмите кнопку **Сохранить**.

   - **Имя группы**: введите **mylegacydevices**.

   - **Тип аттестации**: выберите **Симметричный ключ**.

   - **Автоматически создавать ключи**: установите флажок.

   - **Выберите способ назначения устройств для Центров**: выберите **статическую конфигурацию**, чтобы вы могли назначить устройство определенному центру.

   - **Select the IoT hubs this group can be assigned to** (Выберите Центры Интернета вещей, которым может быть назначена эта группа): выберите один из центров.

     ![Добавление группы регистраций для аттестации симметричного ключа](./media/how-to-legacy-device-symm-key/symm-key-enrollment-group.png)

4. После сохранения регистрации **первичный** и **вторичный ключи** будут созданы и добавлены в запись регистрации. Ваша группа регистраций симметричного ключа отображается как **mylegacydevices** в столбце *Имя группы* на вкладке *Группы регистрации*. 

    Откройте регистрацию и скопируйте значение сформированного **первичного ключа**. Этот ключ является главным ключом группы.


## <a name="choose-a-unique-registration-id-for-the-device"></a>Выбор уникального идентификатора регистрации для устройства

Уникальный идентификатор регистрации должен быть определен для идентификации каждого устройства. Можно использовать MAC-адрес, серийный номер или любые уникальные сведения устройства. 

В этом примере мы используем сочетание MAC-адреса и серийного номера, формирующие следующую строку для идентификатора регистрации.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Создайте уникальный идентификатор регистрации для устройства. Допустимые знаки — буквы в нижнем регистре, цифры и дефис ("-").


## <a name="derive-a-device-key"></a>Получение ключа устройства 

Чтобы создать ключ устройства, используйте главный ключ группы для вычисления [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) на основе уникального идентификатора регистрации для устройства и преобразования результата в формат Base64.

Не включайте главный ключ группы в код устройства.


#### <a name="linux-workstations"></a>Рабочие станции Linux

Если вы используете рабочую станцию Linux, можно использовать OpenSSL для формирования производного ключа устройства, как показано в следующем примере.

Замените значение **KEY** значением **первичного ключа**, записанным ранее.

Замените значение **REG_ID** идентификатором регистрации.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


#### <a name="windows-based-workstations"></a>Рабочие станции на основе Windows

Если вы используете рабочую станцию для Windows, можно использовать PowerShell для формирования производного ключа устройства, как показано в следующем примере.

Замените значение **KEY** значением **первичного ключа**, записанным ранее.

Замените значение **REG_ID** идентификатором регистрации.

```powershell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```powershell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


Устройство будет использовать производный ключ устройства с уникальным идентификатором регистрации для выполнения аттестации симметричного ключа в группе регистраций во время подготовки.



## <a name="create-a-device-image-to-provision"></a>Создание образа устройства для подготовки к работе

В этом разделе вы обновите пример для подготовки **prov\_dev\_client\_sample**, который размещен в пакете SDK Azure IoT для C, настроенном ранее. 

Пример кода имитирует последовательность загрузки устройства, которое отправляет запрос на подготовку в экземпляр службы подготовки устройств. При выполнении последовательности загрузки устройство будет распознано и назначено настроенному Центру Интернета вещей в группе регистраций.

1. На портале Azure выберите вкладку **Обзор** службы подготовки устройств и запишите значение **_области идентификатора_** .

    ![Извлеките сведения о конечной точке службы подготовки устройств из колонки на портале](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. В Visual Studio откройте файл решения **azure_iot_sdks.sln**, который был создан ранее в результате запуска CMake. Файл решения должен находиться в следующем расположении:

    ```
    \azure-iot-sdk-c\cmake\azure_iot_sdks.sln
    ```

3. В окне *Обозреватель решений* Visual Studio перейдите в папку **Provision\_Samples**. Разверните пример проекта с именем **prov\_dev\_client\_sample**. Разверните **исходные файлы** и откройте **prov\_dev\_client\_sample.c**.

4. Найдите константу `id_scope` и замените ее значение ранее скопированным значением **области идентификатора**. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Найдите определение функции `main()` в том же файле. Проверьте, чтобы переменной `hsm_type` было задано значение `SECURE_DEVICE_TYPE_SYMMETRIC_KEY`, как показано ниже.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. Найдите закомментированный вызов `prov_dev_set_symmetric_key_info()` в **prov\_dev\_client\_sample.c**.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    Раскомментируйте вызов функции и замените значения заполнителей (включая угловые скобки) уникальным идентификатором регистрации для своего устройства и производным ключом устройства, который вы создали.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6", "Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=");
    ```
   
    Сохраните файл.

7. Щелкните проект **prov\_dev\_client\_sample** правой кнопкой мыши и выберите пункт **Назначить запускаемым проектом**. 

8. В меню Visual Studio выберите **Отладка** > **Запуск без отладки**, чтобы запустить решение. При появлении запроса перестроить проект щелкните **Да**, чтобы перестроить его перед запуском.

    Следующий результат является примером успешной загрузки имитированного устройства и его подключения к экземпляру службы подготовки для назначения Центру Интернета вещей:

    ```cmd
    Provisioning API Version: 1.2.8

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: 
    test-docs-hub.azure-devices.net, deviceId: sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

    Press enter key to exit:
    ```

9. In the portal, navigate to the IoT hub your simulated device was assigned to and click the **IoT Devices** tab. On successful provisioning of the simulated to the hub, its device ID appears on the **IoT Devices** blade, with *STATUS* as **enabled**. Возможно, вам потребуется нажать кнопку **Обновить** в верхней области. 

    ![Устройство зарегистрировано в Центре Интернета вещей](./media/how-to-legacy-device-symm-key/hub-registration.png) 



## <a name="security-concerns"></a>Соображения безопасности

Имейте в виду, что в таком случае производный ключ устройства остается как часть образа, что не рекомендуется из соображений безопасности. Это одна из причин, почему нужно идти на компромиссы, выбирая между безопасностью и простотой использования. 





## <a name="next-steps"></a>Дальнейшие действия

* To learn more Reprovisioning, see [IoT Hub Device reprovisioning concepts](concepts-device-reprovision.md) 
* [Краткое руководство по подготовке имитированного устройства с использованием симметричных ключей](quick-create-simulated-device-symm-key.md)
* To learn more Deprovisioning, see [How to deprovision devices that were previously auto-provisioned](how-to-unprovision-devices.md) 











