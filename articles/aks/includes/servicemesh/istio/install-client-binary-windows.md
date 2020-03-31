---
author: paulbouwer
ms.topic: include
ms.date: 11/15/2019
ms.author: pabouwer
ms.openlocfilehash: 562382cc1cfb6adb7e65d76e717df4c4e2962ba7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594005"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Скачать и установить двоичный клиент Istio istioctl

В оболочке на основе PowerShell `Invoke-WebRequest` на Windows используйте для загрузки релиза Istio, а затем извлекайте `Expand-Archive` следующие

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.4.0"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

Клиент `istioctl` двоичный работает на вашей клиентской машине и позволяет взаимодействовать с сеткой обслуживания Istio. Используйте следующие команды для установки двоичного файла клиента Istio `istioctl` в оболочку на основе PowerShell в Windows. Эти команды копируют `istioctl` двоичную клиентскую папку Istio, а затем делают ее доступной как немедленно `PATH`(в текущей оболочке), так и постоянно (через перезапуск оболочки) через ваш . Для выполнения этих команд не нужны повышенные (админские) привилегии, и вам не нужно перезапускать оболочку.

```powershell
# Copy istioctl.exe to C:\Istio
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:\Istio"
Copy-Item -Path .\bin\istioctl.exe -Destination "C:\Istio\"

# Add C:\Istio to PATH. 
# Make the new PATH permanently available for the current User, and also immediately available in the current shell.
$PATH = [environment]::GetEnvironmentVariable("PATH", "User") + "; C:\Istio\"
[environment]::SetEnvironmentVariable("PATH", $PATH, "User") 
[environment]::SetEnvironmentVariable("PATH", $PATH)
```