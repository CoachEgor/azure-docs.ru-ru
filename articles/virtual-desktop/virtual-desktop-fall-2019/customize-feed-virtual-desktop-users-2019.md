---
title: Настройка веб-канала для пользователей виртуальных рабочих столов Windows (классическая модель) в Azure
description: Настройка веб-канала для пользователей виртуальных рабочих столов Windows (классические) с помощью командлетов PowerShell.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3a8c1a5da4f4d46d98e01eeb0bf210567af19a95
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "88007239"
---
# <a name="customize-feed-for-windows-virtual-desktop-classic-users"></a>Настройка веб-канала для пользователей виртуальных рабочих столов Windows (классические)

>[!IMPORTANT]
>Это содержимое применимо к Виртуальному рабочему столу Windows (классическому), который не поддерживает объекты Azure Resource Manager для Виртуального рабочего стола Windows. Сведения об обеспечении управления объектами Azure Resource Manager для Виртуального рабочего стола Windows см. в [этой статье](../customize-feed-for-virtual-desktop-users.md).

Вы можете настроить веб-канал таким образом, чтобы ресурсы RemoteApp и удаленный рабочий стол отображались для пользователей с распознаваемым способом.

Сначала [скачайте и импортируйте модуль PowerShell для Виртуального рабочего стола Windows](/powershell/windows-virtual-desktop/overview/) для использования в сеансе PowerShell (если вы еще это не сделали). После этого выполните следующий командлет, чтобы войти в учетную запись:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="customize-the-display-name-for-a-remoteapp"></a>Настройка отображаемого имени для RemoteApp

Вы можете изменить отображаемое имя для опубликованного RemoteApp, задав понятное имя. По умолчанию понятное имя совпадает с именем удаленного приложения RemoteApp.

Чтобы получить список опубликованных RemoteApp для группы приложений, выполните следующий командлет PowerShell:

```powershell
Get-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```

> [!div class="mx-imgBorder"]
> ![Снимок экрана командлета PowerShell Get-Рдсремотеапп с выделенным именем и FriendlyName.](../media/get-rdsremoteapp.png)

Чтобы назначить понятное имя RemoteApp, выполните следующий командлет PowerShell:

```powershell
Set-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -Name <existingappname> -FriendlyName <newfriendlyname>
```

> [!div class="mx-imgBorder"]
> ![Снимок экрана командлета PowerShell Set-Рдсремотеапп с именем и новым FriendlyName.](../media/set-rdsremoteapp.png)

## <a name="customize-the-display-name-for-a-remote-desktop"></a>Настройка отображаемого имени для удаленный рабочий стол

Вы можете изменить отображаемое имя для опубликованного удаленного рабочего стола, указав Понятное имя. Если вы вручную создали пул узлов и группу классических приложений с помощью PowerShell, понятное имя по умолчанию — "сеанс рабочего стола". Если вы создали пул узлов и группу классических приложений с помощью шаблона Azure Resource Manager GitHub или предложения Azure Marketplace, понятное имя по умолчанию совпадает с именем пула узлов.

Чтобы получить ресурс удаленного рабочего стола, выполните следующий командлет PowerShell:

```powershell
Get-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```

> [!div class="mx-imgBorder"]
> ![Снимок экрана командлета PowerShell Get-Рдсремотеапп с выделенным именем и FriendlyName.](../media/get-rdsremotedesktop.png)

Чтобы назначить понятное имя ресурсу удаленного рабочего стола, выполните следующий командлет PowerShell:

```powershell
Set-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -FriendlyName <newfriendlyname>
```

> [!div class="mx-imgBorder"]
> ![Снимок экрана командлета PowerShell Set-Рдсремотеапп с именем и новым FriendlyName.](../media/set-rdsremotedesktop.png)

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы настроили веб-канал для пользователей, вы можете войти в клиент виртуальных рабочих столов Windows, чтобы протестировать его. Для этого перейдите к инструкциям по подключению к виртуальному рабочему столу Windows.

 * [Подключение из Windows 10 или Windows 7](connect-windows-7-10-2019.md)
 * [Подключение из веб-браузера](connect-web-2019.md)
