---
title: Пример для CLI. Копирование управляемых дисков в учетную запись хранения
description: Пример для Azure CLI. Экспорт или копирование управляемых дисков в учетную запись хранения
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/09/2019
ms.author: ramankum
ms.custom: mvc,seodec18
ms.openlocfilehash: d841519aeedb4b85fb897b45fa09be6c62b1fa2a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459990"
---
# <a name="exportcopy-a-managed-disk-to-a-storage-account-using-the-azure-cli"></a>Экспорт или копирование управляемых дисков в учетную запись хранения

Этот скрипт позволяет экспортировать базовый VHD управляемого диска в учетную запись хранения в том же или другом регионе. Сначала для управляемого диска создается URI SAS. Затем с его помощью VHD копируется в учетную запись хранения. Этот сценарий также позволяет копировать управляемый диск в другой регион для продвижения. Если вы хотите опубликовать VHD-файл управляемого диска в Azure Marketplace, можно использовать этот скрипт для копирования VHD-файла в учетную запись хранения и создания универсального кода ресурса SAS для скопированного VHD для его публикации в Marketplace.   


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.sh "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует приведенные ниже команды для создания универсального кода ресурса (URI) SAS управляемого диска. Затем базовый VHD копируется в учетную запись хранения с помощью созданного URI SAS. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [az disk grant-access](https://docs.microsoft.com/cli/azure/disk?view=azure-cli-latest#az-disk-grant-access) | Создает SAS только для чтения, который используется для копирования базового VHD-файла в учетную запись хранения или загрузки в локальную среду  |
| [az storage blob copy start](https://docs.microsoft.com/cli/azure/storage/blob/copy) | Асинхронно копирует большой двоичный объект из одной учетной записи хранения в другую. |

## <a name="next-steps"></a>Дальнейшие действия

[Создание управляемого диска на основе VHD](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Создание виртуальной машины на основе управляемого диска](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры сценариев интерфейса командной строки для виртуальных машин и управляемых дисков см. в [документации по виртуальным машинам Azure под управлением Linux](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
