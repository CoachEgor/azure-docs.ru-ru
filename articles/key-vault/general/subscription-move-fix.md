---
title: Изменение идентификатора клиента хранилища ключей после перемещения подписки — Azure Key Vault | Документация Майкрософт
description: Узнайте, как изменить идентификатор клиента хранилища ключей после перемещения подписки в другой клиент.
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: fcb9d0d6c7e077ee805662df1d2ad87872b42f97
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84629332"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Изменение идентификатора клиента хранилища ключей после перемещения подписки

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]


При создании хранилища ключей в подписке оно автоматически привязывается к идентификатору клиента Azure Active Directory по умолчанию для этой подписки. Все записи политики доступа также привязываются к этому идентификатору клиента. 

При перемещении подписки Azure из клиента A в клиент Б существующие хранилища ключей недоступны для субъектов (пользователей и приложений) в клиенте Б. Вот как устранить эту проблему.

* Измените идентификатор клиента, связанный со всеми существующими хранилищами ключей в этой подписке, для клиента Б.
* Удалите все существующие записи политики доступа.
* Добавьте новые записи политики доступа, связанные с клиентом Б.

Например, при наличии хранилища ключей myvault в подписке, которая была перенесена из клиента A в клиент Б, используя Azure PowerShell можно изменить идентификатор клиента и удалить старые политики доступа.

```azurepowershell
Select-AzSubscription -SubscriptionId <your-subscriptionId>                # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your key vault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your key vault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your key vault resides in
$vault.Properties.AccessPolicies = @()                                     # Access policies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                             # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the key vault's properties.
````

Или же можно использовать интерфейс командной строки Azure.

```azurecli
az account set -s <your-subscriptionId>                                    # Select your Azure Subscription
$tenantId=$(az account show --query tenantId)                               # Get your tenantId
az keyvault update -n myvault --remove Properties.accessPolicies           # Remove the access policies
az keyvault update -n myvault --set Properties.tenantId=$tenantId          # Update the key vault tenantId
```

Теперь, когда хранилище связано с правильным идентификатором клиента, и старые записи политики доступа удалены, задайте новые записи политики доступа с помощью командлета [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy) или команды Azure CLI [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy).

Если вы используете управляемое удостоверение для ресурсов Azure, необходимо также обновить его до нового клиента Azure AD. Дополнительные сведения об управляемых удостоверениях см. в разделе [Выполнение проверки подлинности Key Vault с помощью управляемого удостоверения](managed-identity.md).


Если вы используете MSI, необходимо также обновить идентификатор MSI, так как старый идентификатор больше не будет находиться в правильном клиенте AAD.

## <a name="next-steps"></a>Дальнейшие действия

Если у вас возникли вопросы об Azure Key Vault, посетите [страницу корпорации Майкрософт с вопросами и ответами об Azure Key Vault](https://docs.microsoft.com/answers/topics/azure-key-vault.html).
