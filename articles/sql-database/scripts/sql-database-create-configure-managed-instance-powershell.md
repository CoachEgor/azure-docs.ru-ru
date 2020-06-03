---
title: Пример для PowerShell. Создание управляемого экземпляра в Базе данных SQL Azure
description: Пример сценария Azure PowerShell для создания управляемого экземпляра в Базе данных SQL Azure
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/25/2019
ms.openlocfilehash: e8df39b5bd6c2948fa0f4392b7417a9ffb10a03f
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2020
ms.locfileid: "83772562"
---
# <a name="use-powershell-to-create-an-azure-sql-database-managed-instance"></a>Создание управляемого экземпляра Базы данных SQL Azure с помощью PowerShell

Этот пример сценария PowerShell создает управляемый экземпляр Базы данных SQL Azure в выделенной подсети в новой виртуальной сети. Он также настраивает таблицу маршрутов и группу безопасности сети для виртуальной сети. После успешного выполнения сценария доступ к управляемому экземпляру можно получить из виртуальной сети или из локальной среды. См. стать. о [настройке виртуальной машины Azure для подключения к Управляемому экземпляру Базы данных SQL Azure](../sql-database-managed-instance-configure-vm.md), а также стать. о [настройке подключения "точка — сеть" к Управляемому экземпляру Базы данных SQL Azure из локальной среды](../sql-database-managed-instance-configure-p2s.md).

> [!IMPORTANT]
> Сведения об ограничениях см. в разделах [Поддерживаемые регионы](../sql-database-managed-instance-resource-limits.md#supported-regions) и [Поддерживаемые типы подписок](../sql-database-managed-instance-resource-limits.md#supported-subscription-types).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Чтобы установить и использовать PowerShell локально, вам понадобится AZ PowerShell 1.4.0 или последующей версии для работы с этим руководством. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-az-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzAccount`, чтобы создать подключение к Azure.

## <a name="sample-script"></a>Пример скрипта

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/managed-instance/create-and-configure-managed-instance.ps1 "Create managed instance")]

## <a name="clean-up-deployment"></a>Очистка развертывания

Используйте следующую команду, чтобы удалить группу ресурсов и все связанные с ней ресурсы.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует часть из следующих команд. Чтобы получить дополнительные сведения об используемых и других командах, включенных в таблицу ниже, используйте ссылки на документацию по конкретным командам.

| Get-Help | Примечания |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Создает группу ресурсов, в которой хранятся все ресурсы.
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Создает виртуальную сеть. |
| [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Add-AzVirtualNetworkSubnetConfig) | Добавляет конфигурацию подсети в виртуальную сеть. |
| [Get-AzVirtualNetwork](/powershell/module/az.network/Get-AzVirtualNetwork) | Получает виртуальную сеть в группе ресурсов. |
| [Set-AzVirtualNetwork](/powershell/module/az.network/Set-AzVirtualNetwork) | Задает состояние цели для виртуальной сети. |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Get-AzVirtualNetworkSubnetConfig) | Получает подсеть в виртуальной сети. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Set-AzVirtualNetworkSubnetConfig) | Настраивает состояние цели для конфигурации подсети в виртуальной сети. |
| [New-AzRouteTable](/powershell/module/az.network/New-AzRouteTable) | Создает таблицу маршрутов. |
| [Get-AzRouteTable](/powershell/module/az.network/Get-AzRouteTable) | Получает таблицы маршрутов. |
| [Set-AzRouteTable](/powershell/module/az.network/Set-AzRouteTable) | Задает состояние цели для таблицы маршрутов. |
| [New-AzSqlInstance](/powershell/module/az.sql/New-AzSqlInstance) | Создает управляемый экземпляр Базы данных SQL Azure. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Удаляет группу ресурсов со всеми вложенными ресурсами. |
|||

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

Дополнительные примеры сценариев PowerShell для базы данных SQL Azure можно найти [здесь](../sql-database-powershell-samples.md).
