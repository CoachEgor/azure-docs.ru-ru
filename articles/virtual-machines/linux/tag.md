---
title: Добавление тега к виртуальной машине Linux в Azure | Документация Майкрософт
description: Узнайте, как добавлять теги к виртуальной машине Linux, созданной в Azure с использованием модели развертывания с помощью Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: mmccrory
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: 290105b4e5e3ac3337b0be1b7d437601223bdf68
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708742"
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Пометка виртуальной машины Linux в Azure
В этой статье описываются разные способы назначения тегов виртуальной машине Linux в Azure с использованием модели развертывания Resource Manager. Теги — это определяемые пользователем пары "ключ-значение", которые можно помещать непосредственно в ресурс или группу ресурсов. В настоящий момент Azure поддерживает до 15 тегов на ресурс или группу ресурсов. Теги можно добавлять к ресурсу во время его создания или к уже существующему ресурсу. Обратите внимание, что теги поддерживаются только для тех ресурсов, которые созданы с помощью модели развертывания Resource Manager.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Отметка тегами с помощью интерфейса командной строки Azure

Для начала вам нужно установить последнюю версию [Azure CLI](/cli/azure/install-azure-cli) и войти в учетную запись Azure с помощью команды [az login](/cli/azure/reference-index#az-login).

Вы можете просмотреть все свойства определенной виртуальной машины, включая теги, с помощью следующей команды:

```azurecli
az vm show --resource-group MyResourceGroup --name MyTestVM
```

Чтобы добавить новый тег виртуальной машины через Azure CLI, можно применить команду `azure vm update` с параметром тега **--set**:

```azurecli
az vm update \
    --resource-group MyResourceGroup \
    --name MyTestVM \
    --set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2
```

Чтобы удалить теги, используйте в команде `azure vm update` параметр **--remove**.

```azurecli
az vm update --resource-group MyResourceGroup --name MyTestVM --remove tags.myNewTagName1
```

Теперь, когда мы применили теги к ресурсам с помощью Azure CLI и портала, рассмотрим сведения об использовании, чтобы увидеть теги на портале выставления счетов.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Следующие шаги
* Дополнительные сведения о добавлении тегов для ресурсов Azure, см. в разделе [Обзор диспетчера ресурсов Azure][Azure Resource Manager Overview] and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources].
* Чтобы увидеть, как теги помогают управлять использованием ресурсов Azure, см. в разделе [расшифровка счета Azure][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
