---
title: Пример скрипта Azure CLI. Развертывание стека LAMP в масштабируемом наборе виртуальных машин с балансировкой нагрузки
description: Использование расширения пользовательских сценариев для развертывания стека LAMP в масштабируемом наборе виртуальных машин с балансировкой нагрузки в Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
ms.author: cynthn
tags: azure-service-management
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.custom: mvc
ms.date: 04/05/2017
ms.openlocfilehash: 471690c5c0f5ebb4cea111b85cab1fe0524a36cd
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/20/2020
ms.locfileid: "86509729"
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Развертывание стека LAMP в масштабируемом наборе виртуальных машин с балансировкой нагрузки

В этом примере создается масштабируемый набор виртуальных машин и применяется расширение, которое запускает пользовательский сценарий для развертывания стека LAMP на каждой виртуальной машины в масштабируемом наборе.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Подключение

Используйте этот код, чтобы узнать, как подключаться к своим виртуальным машинам и масштабируемому набору.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, масштабируемый набор, виртуальные машины и все связанные с ней ресурсы.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной машины, группы доступности, балансировщика нагрузки и всех связанных ресурсов этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [az group create](/cli/azure/group) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az vmss create](/cli/azure/vmss) | Создает масштабируемый набор виртуальных машин. |
| [az network lb rule create](/cli/azure/network/lb/rule) | Добавляет конечную точку с балансировкой нагрузки. |
| [az vmss extension set](/cli/azure/vmss/extension) | Создает расширение, которое запускает пользовательский сценарий при развертывании виртуальной машины. |
| [az vmss update-instances](/cli/azure/vmss) | Запускает пользовательский сценарий на экземплярах виртуальной машины, которые были развернуты до применения расширения к масштабируемому набору. |
| [az vmss scale](/cli/azure/vmss) | Увеличивает масштабируемый набор, добавляя экземпляры виртуальной машины. После развертывания на этих экземплярах выполняется пользовательский сценарий. |
| [az network public-ip list](/cli/azure/network/public-ip) | Выводит IP-адреса виртуальных машин, созданных в примере. |
| [az network lb show](/cli/azure/network/lb) | Выводит интерфейсный и внутренний порты, используемые подсистемой балансировки нагрузки. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).

Дополнительные примеры скриптов интерфейса командной строки для виртуальных машин см. в [документации по виртуальным машинам Azure под управлением Linux](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
