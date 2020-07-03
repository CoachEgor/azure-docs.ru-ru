---
title: Пример для CLI. Копирование моментального снимка управляемых дисков в подписку
description: Пример сценария Azure CLI. Копирование (или перемещение) моментального снимка управляемого диска в ту же или другую подписку с помощью CLI
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
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 707fc2f805e19487f93affd2c58943090233967f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "81459973"
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Копирование моментального снимка управляемого диска в ту же или другую подписку с помощью интерфейса командной строки

Этот сценарий копирует моментальный снимок управляемого диска в ту же или другую подписку. Используйте этот скрипт в следующих сценариях:

1. чтобы перенести моментальный снимок из хранилища класса Premium (Premium_LRS) в стандартное хранилище (Standard_LRS или Standard_ZRS) для сокращения расходов;
1. чтобы перенести моментальный снимок из локально избыточного хранилища (Premium_LRS, Standard_LRS) в хранилище, избыточное между зонами (Standard_ZRS), для повышения его надежности;
1. чтобы переместить моментальный снимок в другую подписку в том же регионе на долгосрочное хранение.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Описание скрипта

Этот сценарий использует приведенные ниже команды для создания моментального снимка в целевой подписке с помощью идентификатора исходного моментального снимка. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot) | Получает все свойства моментального снимка, используя имя и свойства группы ресурсов моментального снимка. Свойство идентификатора используется для копирования моментального снимка в другую подписку.  |
| [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot) | Копирует моментальный снимок путем создания моментального снимка в другой подписке с помощью идентификатора и имени родительского снимка.  |

## <a name="next-steps"></a>Дальнейшие действия

[Создание виртуальной машины на основе моментального снимка](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры сценариев интерфейса командной строки для виртуальных машин и управляемых дисков см. в [документации по виртуальным машинам Azure под управлением Linux](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
