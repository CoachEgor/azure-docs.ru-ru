---
title: Пример скрипта Azure CLI. Пиринг между двумя виртуальными сетями
description: Пример скрипта Azure CLI. Пиринг между двумя виртуальными сетями.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: kumud
ms.openlocfilehash: 2dd5336d66872cc8c56fd372e89b67ce9c892f3a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "74083840"
---
# <a name="peer-two-virtual-networks-script-sample"></a>Пример скрипта для установки пиринга между двумя виртуальными сетями

В этом примере скрипта создаются две виртуальные сети, которые соединяются в одном регионе через сеть Azure. В результате выполнения скрипта будет создан пиринг между двумя виртуальными сетями.

Вы можете выполнить скрипт из Azure [Cloud Shell](https://shell.azure.com/bash) или из локальной установки Azure CLI. Если вы используете CLI локально, для этого скрипта требуется версия 2.0.28 или выше. Выполните командлет `az --version`, чтобы узнать установленную версию. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI](/cli/azure/install-azure-cli). Если интерфейс командной строки запущен локально, необходимо также выполнить командлет `az login`, чтобы создать подключение к Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы:

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной машины и всех связанных ресурсов этот скрипт использует следующие команды. Для каждой команды в следующей таблице приведены ссылки на соответствующую документацию:

| Get-Help | Примечания |
|---|---|
| [az group create](/cli/azure/group) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az network vnet create](/cli/azure/network/vnet) | Создает виртуальную сеть и подсеть Azure. |
| [az network vnet peering create](/cli/azure/network/vnet/peering) | Создает пиринг между двумя виртуальными сетями.  |
| [az group delete](/cli/azure/vm/extension) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры скриптов см. на [странице с примерами CLI для виртуальных сетей](../cli-samples.md).
