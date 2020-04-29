---
title: Настройка хранилища ключей
description: Настройка Key Vault для использования с виртуальной машиной.
author: mimckitt
manager: vashan
ms.service: virtual-machines
ms.subservice: security
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 01/24/2017
ms.author: mimckitt
ms.openlocfilehash: e4bff4d1826d9586495207095eccf8f6c66164a0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81870005"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Настройка хранилища ключей для виртуальных машин в Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

В стеке Azure Resource Manager секреты или сертификаты моделируются как ресурсы, которые предоставляются поставщиком ресурсов хранилища ключей. Чтобы больше узнать о хранилище ключей, ознакомьтесь с разделом [Что такое хранилище ключей Azure?](../../key-vault/general/overview.md)

> [!NOTE]
> 1. Чтобы хранилище ключей можно было использовать для виртуальных машин Azure Resource Manager, свойству **EnabledForDeployment** хранилища ключей должно быть присвоено значение true. Это можно сделать на различных клиентах.
> 2. Хранилище ключей должно быть создано в тех же подписке и расположении, что и виртуальная машина.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>Использование PowerShell для настройки хранилища ключей
Сведения о создании хранилища ключей с помощью PowerShell см. в статье [Настройка и получение секрета из Azure Key Vault с помощью PowerShell](../../key-vault/secrets/quick-create-powershell.md).

Для новых хранилищ ключей можно использовать этот командлет PowerShell:

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Для существующих хранилищ ключей можно использовать этот командлет PowerShell:

    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>Использование интерфейса командной строки для настройки хранилища ключей
Сведения об использовании интерфейса командной строки (CLI) для создания хранилища ключей см. в статье [Управление хранилищем ключей с помощью CLI](../../key-vault/general/manage-with-cli2.md#create-a-key-vault).

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

Сведения о других параметрах, которые можно настроить при создании хранилища ключей с помощью шаблонов, см. в разделе [Создание хранилища ключей](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
