---
title: Настройка Key Vault для виртуальных машин Windows в Azure Resource Manager
description: Узнайте, как настроить хранилище ключей для использования с виртуальной машиной Azure Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: a64163da1dee2bceb567436dc18ba0fa5274cfcb
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79243150"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Настройка хранилища ключей для виртуальных машин в Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

В стеке Azure Resource Manager секреты или сертификаты моделируются как ресурсы, которые предоставляются поставщиком ресурсов хранилища ключей. Чтобы больше узнать о хранилище ключей, ознакомьтесь с разделом [Что такое хранилище ключей Azure?](../../key-vault/key-vault-overview.md)

> [!NOTE]
> 1. Чтобы хранилище ключей можно было использовать для виртуальных машин Azure Resource Manager, свойству **EnabledForDeployment** хранилища ключей должно быть присвоено значение true. Это можно сделать на различных клиентах.
> 2. Хранилище ключей должно быть создано в тех же подписке и расположении, что и виртуальная машина.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>Использование PowerShell для настройки хранилища ключей
Сведения о создании хранилища ключей с помощью PowerShell см. в статье [Настройка и получение секрета из Azure Key Vault с помощью PowerShell](../../key-vault/quick-create-powershell.md).

Для новых хранилищ ключей можно использовать этот командлет PowerShell:

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Для существующих хранилищ ключей можно использовать этот командлет PowerShell:

    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>Использование интерфейса командной строки для настройки хранилища ключей
Сведения об использовании интерфейса командной строки (CLI) для создания хранилища ключей см. в статье [Управление хранилищем ключей с помощью CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Чтобы использовать интерфейс командной строки, необходимо сначала создать хранилище ключей, а затем назначить политику развертывания. Это можно сделать с помощью следующей команды:

    az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "EastAsia"
    
Затем, чтобы включить Key Vault для использования с развертыванием шаблона, выполните следующую команду:

    az keyvault update --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --enabled-for-deployment "true"

## <a name="use-templates-to-set-up-key-vault"></a>Использование шаблонов для настройки хранилища ключей
При использовании шаблона свойству `enabledForDeployment` ресурса хранилища ключей нужно присвоить значение `true`.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Сведения о других параметрах, которые можно настроить при создании хранилища ключей с помощью шаблонов, см. в статье [Создание хранилища ключей](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
