---
title: Пример для PowerShell. Создание виртуальной машины путем подключения управляемого диска в качестве диска ОС
description: Пример скрипта Azure PowerShell. Создание виртуальной машины путем подключения управляемого диска в качестве диска ОС
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 4514940918f33ee28fc4195b821018df54252e13
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "75368494"
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a>Создание виртуальной машины с помощью существующего управляемого диска ОС с использованием PowerShell

Этот скрипт создает виртуальную машину путем подключения существующего управляемого диска как диска ОС. Используйте этот скрипт в таких сценариях:
* создание виртуальной машины из существующего управляемого диска ОС, скопированного из управляемого диска в другой подписке;
* создание виртуальной машины из существующего управляемого диска, который был создан из специального файла VHD; 
* создание виртуальной машины из существующего управляемого диска ОС, который был создан из моментального снимка. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды для получения свойств управляемого диска, его подключения к новой виртуальной машине и создания виртуальной машины. Для каждого элемента в таблице приведены ссылки на документацию по команде.

| Get-Help | Примечания |
|---|---|
| [Get-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/Get-AzDisk) | Возвращает объект диска на основе имени и группы ресурсов диска. Свойство идентификатора возвращенного объекта диска используется для подключения диска к новой виртуальной машине. |
| [New-AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) | Создает конфигурацию виртуальной машины. Эта конфигурация включает в себя такие сведения, как имя виртуальной машины, операционную систему и учетные данные администратора. Данная конфигурации используется при создании виртуальной машины. |
| [Set-AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) | Подключает управляемый диск, используя свойство идентификатора диска как диск ОС для новой виртуальной машины. |
| [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) | Создает общедоступный IP-адрес. |
| [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) | Создает сетевой интерфейс. |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | Создайте виртуальную машину. |
|[Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Удаляет группу ресурсов и все ресурсы, содержащиеся в ней. |

Для образов из marketplace используйте [Set-AzVMPlan](https://docs.microsoft.com/powershell/module/az.compute/set-azvmplan), чтобы задать сведения о плане.

```powershell
Set-AzVMPlan -VM $VirtualMachine -Publisher $Publisher -Product $Product -Name $Bame
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о модуле Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

Дополнительные примеры сценариев PowerShell для виртуальных машин представлены в [документации по виртуальным машинам Azure под управлением Windows](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
