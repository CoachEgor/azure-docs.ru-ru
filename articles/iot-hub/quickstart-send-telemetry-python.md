---
title: Краткое руководство по отправке телеметрии в Центр Интернета вещей (Python) | Документация Майкрософт
description: В этом кратком руководстве мы рассмотрим процесс отправки имитированных данных телеметрии в Центр Интернета вещей на примере приложения Python и с помощью служебной программы попробуем считать данные телеметрии из Центра Интернета вещей.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/28/2019
ms.openlocfilehash: a8abd71609d3e063c92541485007a3bde44be954
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051234"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-python"></a>Краткое руководство. Отправка данных телеметрии из устройства в Центр Интернета вещей и их чтение с помощью внутреннего приложения (Python)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

Центр Интернета вещей — это служба Azure, которая позволяет получать большие объемы телеметрии с ваших устройств Центра Интернета вещей в облаке на хранение или обработку. В рамках этого краткого руководства вы отправите данные телеметрии из приложения имитированного устройства через Центр Интернета вещей во внутреннее приложение для обработки.

В этом кратком руководстве используется предварительно написанное приложение Python для отправки телеметрии и служебная программа CLI для чтения телеметрии из центра. Прежде чем запускать эти приложения, создайте Центр Интернета вещей и зарегистрируйте устройство в центре.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Примеры приложений, запускаемые в рамках этого краткого руководства, написаны на языке Python. Сейчас пакеты SDK Microsoft Azure IoT для Python поддерживают только конкретные версии Python для каждой платформы. Дополнительные сведения см. в [файле сведений пакета SDK для Python](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues).

В этом кратком руководстве предполагается, что вы используете компьютер разработки под управлением Windows. Для систем Windows поддерживается только [Python 3.6.x](https://www.python.org/downloads/release/python-368/). При выборе установщика Python нужно руководствоваться архитектурой системы, в которой вы работаете. Если архитектура системы ЦП 32-разрядная, скачайте установщик x86. Для 64-разрядной архитектуры необходимо скачать установщик x86-64. Также убедитесь, что [Распространяемый компонент Microsoft Visual C++ для Visual Studio 2019](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) установлен для вашей архитектуры (x86 или x64).

Python, предназначенный для других платформ, можно скачать на сайте [Python.org](https://www.python.org/downloads/).

Текущую версию Python на компьютере, на котором ведется разработка, можно проверить, выполнив одну из следующих команд:

```python
python --version
```

```python
python3 --version
```

Выполните следующую команду, чтобы добавить расширение Интернета вещей Microsoft Azure для Azure CLI в экземпляр Cloud Shell. Расширение Интернета вещей добавляет в Azure CLI специальные команды Центра Интернета вещей, IoT Edge и службы подготовки устройств Интернета вещей (DPS).

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Скачайте пример проекта Python по адресу https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip и извлеките ZIP-архив.

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Регистрация устройства

Устройство должно быть зарегистрировано в Центре Интернета вещей, прежде чем оно сможет подключиться. В этом кратком руководстве для регистрации имитируемого устройства используется Azure Cloud Shell.

1. Выполните приведенные ниже команды в Azure Cloud Shell, чтобы создать удостоверение устройства.

    **YourIoTHubName**. Замените этот заполнитель именем вашего Центра Интернета вещей.

    **MyPythonDevice**. Это имя, присвоенное зарегистрированному устройству. Используйте MyPythonDevice, как показано ниже. Если вы выбрали другое имя для устройства, используйте его при работе с этим руководством и обновите имя устройства в примерах приложений перед их запуском.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyPythonDevice
    ```

1. Выполните следующую команду в Azure Cloud Shell, чтобы получить _строку подключения_ зарегистрированного устройства:

    **YourIoTHubName**. Замените этот заполнитель именем вашего центра Интернета вещей.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyPythonDevice --output table
    ```

    Запишите строку подключения устройства, которая выглядит так:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Это значение понадобится позже в рамках этого краткого руководства.

## <a name="send-simulated-telemetry"></a>Отправка имитированной телеметрии

Приложение имитированного устройства подключается к конечной точке конкретного устройства Центра Интернета вещей и отправляет имитированную телеметрию температуры и влажности.

1. В окне терминала на локальном компьютере перейдите в корневую папку примера проекта Python. Затем перейдите в папку **iot-hub\Quickstarts\simulated-device**.

1. Откройте файл **SimulatedDevice.py** в любом текстовом редакторе.

    Замените значение переменной `CONNECTION_STRING` записанной ранее строкой подключения к устройству. Сохраните изменения в файле **SimulatedDevice.py**.

1. Установите необходимые библиотеки для приложения имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Запустите приложение имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    На следующем снимке экрана показан пример выходных данных, когда приложение имитированного устройства отправляет данные телеметрии в Центр Интернета вещей:

    ![Запуск виртуального устройства](media/quickstart-send-telemetry-python/SimulatedDevice.png)
    
### <a name="to-avoid-the-import-iothubclient-error"></a>Предотвращение ошибки импорта iothub_client
Текущая версия пакета SDK Интернета вещей Azure для Python является оболочкой [нашего пакета C SDK](https://github.com/azure/azure-iot-sdk-c). Он создается с помощью библиотеки [Boost](https://www.boost.org/). Из-за этого он предоставляется с некоторыми значительными ограничениями. Дополнительные сведения см. [здесь](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues)

1. Убедитесь, что у вас установлена правильная версия [Python](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues). Имейте в виду, что только определенные версии подходят для этого примера. 
2. Убедитесь, что у вас установлена необходимая версия [Распространяемого компонента Microsoft Visual C++ для Visual Studio 2019](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) для среды выполнения C++. (Мы рекомендуем последнюю версию).
3. Убедитесь, что вы установили клиент Центра Интернета вещей: `pip install azure-iothub-device-client`.

## <a name="read-the-telemetry-from-your-hub"></a>Чтение данных телеметрии из концентратора

Расширение CLI для Центра Интернета вещей позволяет подключить конечную точку сервера **События** к Центру Интернета вещей. Расширение получает сообщения с устройства в облако, отправленные с имитированного устройства. Внутреннее приложение Центра Интернета вещей обычно запускается в облаке, чтобы получать сообщения с устройства в облако и обрабатывать их.

Выполните следующие команды в Azure Cloud Shell, заменив `YourIoTHubName` на имя вашего центра Интернета вещей:

```azurecli-interactive
az iot hub monitor-events --hub-name YourIoTHubName --device-id MyPythonDevice 
```

На следующем снимке экрана показан пример выходных данных, когда расширение получает данные телеметрии, отправленные в центр имитированным устройством:

![Запуск внутреннего приложения](media/quickstart-send-telemetry-python/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве вы настроили Центр Интернета вещей, зарегистрировали устройство, отправили имитированные данные телеметрии в центр с помощью приложения Python, а также считали данные телеметрии из центра, используя простое внутреннее приложение.

Чтобы узнать, как управлять имитированным устройством из внутреннего приложения, перейдите к следующему краткому руководству.

> [!div class="nextstepaction"]
> [Краткое руководство. Управление подключенным к Центру Интернета вещей устройством](quickstart-control-device-python.md)
