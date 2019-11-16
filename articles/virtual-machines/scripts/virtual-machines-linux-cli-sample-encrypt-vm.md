---
title: Пример сценария Azure CLI для шифрования виртуальной машины Linux.
description: Пример сценария Azure CLI для шифрования виртуальной машины Linux.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 26d453adb4ded59de162fda0ae62322399fa4d9a
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2019
ms.locfileid: "74040278"
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>Шифрование виртуальной машины Linux в Azure

Этот сценарий создает безопасное Azure Key Vault, ключи шифрования, субъект-службу Azure Active Directory и виртуальную машину Linux. Затем эта виртуальная машина шифруется с помощью ключа шифрования из Key Vault и учетных данных субъекта-службы.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Описание скрипта

Этот сценарий использует приведенные ниже команды для создания группы ресурсов, Azure Key Vault, субъекта-службы, виртуальной машины и всех связанных ресурсов. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault) | Создает Azure Key Vault для хранения защищенных данных, таких как ключи шифрования. |
| [az keyvault key create](https://docs.microsoft.com/cli/azure/keyvault/key) | Создает ключ шифрования в Key Vault. |
| [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp) | Создает субъект-службу Azure Active Directory для безопасной аутентификации и контроля доступа к ключам шифрования. |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault) | Задает разрешения для Key Vault, предоставляя субъекту-службе доступ к ключам шифрования. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Создает виртуальную машину и подключает ее к сетевой карте, виртуальной сети, подсети и группе безопасности сети. Эта команда также указывает образ виртуальной машины, который будет использоваться, и учетные данные администратора.  |
| [az vm encryption enable](https://docs.microsoft.com/cli/azure/vm/encryption) | Включает шифрование на виртуальной машине с помощью учетных данных субъекта-службы и ключа шифрования. |
| [az vm encryption show](https://docs.microsoft.com/cli/azure/vm/encryption) | Отображает состояние шифрования виртуальной машины. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры скриптов интерфейса командной строки для виртуальных машин см. в [документации по виртуальным машинам Azure под управлением Linux](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
