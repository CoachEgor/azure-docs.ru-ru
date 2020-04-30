---
title: Пример для PowerShell. Экспорт или копирование VHD управляемого диска в учетную запись хранения в другом регионе
description: Пример сценария Azure PowerShell, который позволяет экспортировать или копировать VHD управляемого диска в учетную запись хранения в другом регионе.
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/17/2018
ms.author: ramankum
ms.openlocfilehash: 8388863bb88e3e97d16ad7c80aad6387cf9e03b9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "81459378"
---
# <a name="exportcopy-the-vhd-of-a-managed-disk-to-a-storage-account-in-different-region-with-powershell"></a>Экспорт или копирование VHD управляемого диска в учетную запись хранения в другом регионе с помощью PowerShell

Этот сценарий экспортирует VHD управляемого диска в учетную запись хранения в другом регионе. Сначала он создает URI SAS управляемого диска, а затем использует его для копирования VHD в учетную запись хранения в другом регионе. Этот сценарий также позволяет копировать управляемый диск в другой регион для продвижения.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.ps1 "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Описание скрипта

Этот сценарий выполняет приведенные ниже команды для создания универсального кода ресурса (URI) SAS управляемого диска и копирует базовый VHD в учетную запись хранения, используя созданный URI SAS. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [Grant-AzDiskAccess](https://docs.microsoft.com/powershell/module/az.compute/grant-azdiskaccess) | Создает URI SAS управляемого диска, который используется во время копирования базового VHD в учетную запись хранения. |
| [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/New-AzureStorageContext) | Создает контекст учетной записи хранения с помощью имени учетной записи и ключа. Этот контекст можно использовать для выполнения операций чтения и записи с учетной записью хранения. |
| [Start-AzureStorageBlobCopy](https://docs.microsoft.com/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | Копирует базовый VHD моментального снимка в учетную запись хранения. |

## <a name="next-steps"></a>Дальнейшие действия

[Создание управляемого диска на основе VHD](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[Создание виртуальной машины на основе управляемого диска](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Дополнительные сведения о модуле Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

Дополнительные примеры сценариев PowerShell для виртуальных машин представлены в [документации по виртуальным машинам Azure под управлением Windows](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).