---
title: Пример сценария Azure PowerShell для создания моментального снимка из VHD для быстрого создания нескольких идентичных управляемых дисков.
description: Пример сценария Azure PowerShell для создания моментального снимка из VHD для быстрого создания нескольких идентичных управляемых дисков.
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 1104cd762fb054c13d4bd1ebb5f49afef670bcc3
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2019
ms.locfileid: "74039046"
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Создание моментального снимка из VHD для быстрого создания нескольких идентичных управляемых дисков с помощью PowerShell

Этот сценарий создает моментальный снимок из VHD-файла в учетной записи хранения в той же или другой подписке. Этот сценарий можно использовать, чтобы импортировать специализированный (не универсальный и не подготовленный командой Sysprep) VHD в моментальный снимок, затем с помощью этого моментального снимка создать несколько идентичных управляемых дисков за короткий промежуток времени. Кроме того, с его помощью можно импортировать VHD данных в моментальный снимок, а затем использовать этот моментальный снимок для быстрого создания нескольких управляемых дисков. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


 

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="script-explanation"></a>Описание скрипта

Этот сценарий использует приведенные ниже команды для создания управляемого диска на основе VHD. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzDiskConfig) | Создает конфигурацию диска, которая используется для создания диска. Она содержит тип хранилища, расположение, идентификатор ресурса учетной записи хранения, в которой хранится родительский VHD, и универсальный код ресурса (URI) родительского VHD. |
| [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | Создает диск с помощью конфигурации диска, имени диска и имени группы ресурсов, которые передаются в качестве параметров. |

## <a name="next-steps"></a>Дополнительная информация

[Создание управляемого диска из моментального снимка](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Создание виртуальной машины путем подключения управляемого диска как диска ОС](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Дополнительные сведения о модуле Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

Дополнительные примеры сценариев PowerShell для виртуальных машин представлены в [документации по виртуальным машинам Azure под управлением Windows](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
