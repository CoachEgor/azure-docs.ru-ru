---
title: Изменение идентификатора клиента хранилища ключей после перемещения подписки — Azure Key Vault | Документация Майкрософт
description: Узнайте, как изменить идентификатор клиента хранилища ключей после перемещения подписки в другой клиент.
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 2159b5b515e22458edf3ba0eb5b6f23f3f37ce95
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990107"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Изменение идентификатора клиента хранилища ключей после перемещения подписки

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>Вопрос. Моя подписка перемещена из клиента А в клиент Б. Как изменить идентификатор клиента существующего хранилища ключей и настроить правильные списки ACL для субъектов в клиенте Б?

При создании хранилища ключей в подписке оно автоматически привязывается к идентификатору клиента Azure Active Directory по умолчанию для этой подписки. Все записи политики доступа также привязываются к этому идентификатору клиента. При перемещении подписки Azure из клиента A в клиент Б существующие хранилища ключей недоступны для субъектов (пользователей и приложений) в клиенте Б. Вот как устранить эту проблему.

* Измените идентификатор клиента, связанный со всеми существующими хранилищами ключей в этой подписке, для клиента Б.
* Удалите все существующие записи политики доступа.
* Добавьте новые записи политики доступа, связанные с клиентом Б.

Например, при наличии хранилища ключей myvault в подписке, которая была перенесена из клиента A в клиент Б, необходимо изменить идентификатор клиента для этого хранилища ключей и удалить старые политики доступа.

<pre>
Select-AzSubscription -SubscriptionId YourSubscriptionID                   # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your Keyvault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your Keyvault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your Keyvault resides in
$vault.Properties.AccessPolicies = @()                                     # Accesspolicies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                                                                              # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the kevault's properties.
</pre>

Так как это хранилище располагалось в клиенте A перед перемещением, исходное значение **$vault.Properties.TenantId** является клиентом А, а значение **(Get-AzContext).Tenant.TenantId** — это клиент Б.

Теперь, когда хранилище связано с правильным идентификатором клиента и старые записи политики доступа удалены, можно настроить новые записи политики доступа с помощью командлета [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy).

## <a name="next-steps"></a>Дополнительная информация

Если у вас возникли вопросы о хранилище ключей Azure, посетите [форумы хранилища ключей Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
