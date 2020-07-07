---
title: Установка Azure IoT Edge для Linux в Windows | Документация Майкрософт
description: Azure IoT Edge инструкции по установке для контейнеров Linux в Windows 10, Windows Server и Windows IoT базовая
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: kgremban
ms.openlocfilehash: 8a4579e092bbc4fd58954f1ce1f1dad3a8ddbbba
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "80133163"
---
# <a name="use-iot-edge-on-windows-to-run-linux-containers"></a>Использование IoT Edge в Windows для запуска контейнеров Linux

Тестирование модулей IoT Edge для устройств Linux с помощью компьютера Windows.

В рабочем сценарии устройства Windows должны работать только с контейнерами Windows. Однако распространенным сценарием разработки является использование компьютера Windows для создания модулей IoT Edge для устройств Linux. Среда выполнения IoT Edge для Windows позволяет запускать контейнеры Linux в целях **тестирования и разработки** .

В этой статье перечислены действия по установке среды выполнения Azure IoT Edge с помощью контейнеров Linux в системе Windows x64 (AMD/Intel). Дополнительные сведения о программе установки среды выполнения IoT Edge, включая сведения обо всех параметрах установки, см. в разделе [Установка среды выполнения Azure IOT EDGE в Windows](how-to-install-iot-edge-windows.md).

Сведения о том, что входит в последнюю версию IoT Edge, см. в разделе [Azure IOT Edge выпуски](https://github.com/Azure/azure-iotedge/releases).

## <a name="prerequisites"></a>Предварительные требования

В этом разделе можно посмотреть, поддерживает ли устройство Windows IoT Edge, а также подготовить его для подсистемы контейнеров перед установкой.

### <a name="supported-windows-versions"></a>Поддерживаемые версии Windows

Azure IoT Edge с контейнерами Linux могут работать в любой версии Windows, удовлетворяющей [требованиям для настольных систем DOCKER](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) .

Если вы хотите установить IoT Edge на виртуальной машине, включите вложенную виртуализацию и выделите не менее 2 ГБ памяти. Включение вложенной виртуализации отличается в зависимости от используемой низкоуровневой оболочки. Для виртуальных машин Hyper-V версии 2 вложенная виртуализация включена по умолчанию. Для VMWare существует переключатель, позволяющий включить эту функцию на виртуальной машине.

### <a name="prepare-the-container-engine"></a>Подготовка подсистемы контейнеров

Служба Azure IoT Edge использует подсистему контейнеров, [совместимую с OCI](https://www.opencontainers.org/). Крупнейшим различием в конфигурации между запуском контейнеров Windows и Linux на компьютере Windows является то, что IoT Edgeная установка включает в себя среду выполнения контейнеров Windows, но перед установкой IoT Edge необходимо предоставить собственную среду выполнения для контейнеров Linux.

Чтобы настроить компьютер Windows для разработки и тестирования контейнеров для устройств Linux, можно использовать [DOCKER Desktop](https://www.docker.com/docker-windows) в качестве подсистемы контейнеров. Необходимо установить DOCKER и настроить его для [использования контейнеров Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) перед установкой IOT Edge.  

Если устройство IoT Edge является компьютером под управлением Windows, убедитесь, что оно соответствует [системным требованиям](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) для использования Hyper-V.

## <a name="install-iot-edge-on-a-new-device"></a>Установка IoT Edge на новое устройство

>[!NOTE]
>Использование программных пакетов Azure IoT Edge регулируется условиями лицензии, содержащейся в самих пакетах (в каталоге LICENSE). Прежде чем использовать пакет, ознакомьтесь с условиями лицензии. Установка и использование пакета означают, что вы принимаете эти условия. Если вы не согласны с условиями лицензии, не используйте пакет.

Сценарий PowerShell загружает и устанавливает управляющую программу безопасности Azure IoT Edge. Управляющая программа безопасности запускает первый из двух модулей среды выполнения, агент IoT Edge, который обеспечивает удаленные развертывания других модулей.

При установке среды выполнения IoT Edge в первый раз на устройстве нужно подготовить устройство с помощью удостоверения из центра Интернета вещей. Одно IoT Edge устройство можно подготовить вручную, используя строку подключений устройств, предоставленную Центром Интернета вещей. Вы также можете использовать службу подготовки устройств, что удобно, если нужно автоматически подготовить несколько устройств.

Дополнительные сведения о различных параметрах установки и параметрах см. в статье [Установка среды выполнения Azure IOT EDGE в Windows](how-to-install-iot-edge-windows.md). После установки и настройки DOCKER Desktop для контейнеров Linux основное различие в установке заключается в объявлении Linux с параметром **-контаинерос** . Пример:

1. Если вы еще этого не сделали, зарегистрируйте новое устройство IoT Edge и получите строку подключения устройства. Скопируйте строку подключения для использования далее в этом разделе. Этот шаг можно выполнить с помощью следующих средств:

   * [Портал Azure](how-to-register-device.md#register-in-the-azure-portal)
   * [Azure CLI](how-to-register-device.md#register-with-the-azure-cli)
   * [Visual Studio Code](how-to-register-device.md#register-with-visual-studio-code)

2. Откройте сеанс PowerShell от имени администратора.

   >[!NOTE]
   >Для установки IoT Edge используйте сеанс AMD64 PowerShell, а не PowerShell (x86). Чтобы узнать, какой тип сеанса используется, выполните следующую команду:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. Команда **deploy-IoTEdge** проверяет, поддерживается ли на компьютере Windows поддерживаемая версия, включает функцию контейнеров, а затем загружает среду выполнения значок Кита (которая не используется для контейнеров Linux) и IOT Edge среды выполнения. По умолчанию команда имеет контейнеры Windows, поэтому объявите Linux как нужную операционную систему контейнера.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Linux
   ```

4. На этом этапе устройства IoT Core могут перезапускаться автоматически. Другие устройства Windows 10 или Windows Server могут предложить перезагрузку. Если да, перезагрузите устройство прямо сейчас. Когда устройство будет готово, снова запустите PowerShell от имени администратора.

5. Команда **Initialize-IoTEdge** настраивает среду выполнения IoT Edge на вашем компьютере. По умолчанию команда подготавливается вручную с помощью строки подключения устройства. Объявите Linux как нужную операционную систему контейнера еще раз.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ContainerOs Linux
   ```

6. При появлении запроса введите строку подключения устройства, полученную на шаге 1. Строка подключения устройства связывает физическое устройство с ИДЕНТИФИКАТОРом устройства в центре Интернета вещей.

   Строка подключения устройства имеет следующий формат и не должна содержать кавычки:`HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

## <a name="verify-successful-installation"></a>Проверка установки

Проверьте состояние службы IoT Edge:

```powershell
Get-Service iotedge
```

Изучите журналы службы за последние 5 минут:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Выполните автоматическую проверку наиболее распространенных ошибок конфигурации и сети.

```powershell
iotedge check
```

Просмотрите список запущенных модулей. После новой установки единственным модулем, который вы должны увидеть, является **edgeAgent**. После первого [развертывания модулей IOT Edge](how-to-deploy-modules-portal.md) на устройстве будет запущен другой системный модуль **edgeHub**.

```powershell
iotedge list
```

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда подготовлено устройство IoT Edge и установлена среда выполнения, вы можете [развернуть модули IoT Edge](how-to-deploy-modules-portal.md).

Если вам не удается установить IoT Edge, перейдите на страницу со сведениями об [устранении неполадок](troubleshoot.md).

Чтобы обновить существующую установку до последней версии IoT Edge, см. раздел [Обновление управляющей программы безопасности и среды выполнения IoT Edge](how-to-update-iot-edge.md).
