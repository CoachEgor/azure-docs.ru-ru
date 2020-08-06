---
title: Назначение роли Azure для доступа к данным с помощью PowerShell
titleSuffix: Azure Storage
description: Узнайте, как использовать PowerShell для назначения разрешений участнику безопасности Azure Active Directory с помощью управления доступом на основе ролей (RBAC). Служба хранилища Azure поддерживает встроенные и пользовательские роли Azure для проверки подлинности с помощью Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/16/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 17a34ac3017ee08626dfc17325c29b43b34b5505
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2020
ms.locfileid: "87827968"
---
# <a name="use-powershell-to-assign-an-azure-role-for-access-to-blob-and-queue-data"></a>Назначение роли Azure для доступа к данным BLOB-объектов и очередей с помощью PowerShell

Azure Active Directory (Azure AD) разрешает права доступа к защищенным ресурсам через [Управление доступом на основе ролей Azure (Azure RBAC)](../../role-based-access-control/overview.md). Служба хранилища Azure определяет набор встроенных ролей Azure, охватывающих общие наборы разрешений, используемых для доступа к контейнерам или очередям.

Когда роль Azure назначается субъекту безопасности Azure AD, Azure предоставляет доступ к этим ресурсам для этого субъекта безопасности. Доступ может ограничиваться уровнем подписки, группой ресурсов, учетной записью хранения или отдельным контейнером или очередью. Субъект безопасности Azure AD может быть пользователем, группой, субъектом-службой приложения или [управляемым удостоверением для ресурсов Azure](../../active-directory/managed-identities-azure-resources/overview.md).

В этой статье описывается, как использовать Azure PowerShell для перечисления встроенных ролей Azure и назначения их пользователям. Дополнительные сведения об использовании Azure PowerShell см. в разделе [обзор Azure PowerShell](https://docs.microsoft.com/powershell/azure/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="azure-roles-for-blobs-and-queues"></a>Роли Azure для больших двоичных объектов и очередей

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Определение области действия ресурса

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-azure-roles"></a>Вывод списка доступных ролей Azure

Чтобы получить список доступных встроенных ролей Azure с Azure PowerShell, используйте команду [Get-азроледефинитион](/powershell/module/az.resources/get-azroledefinition) :

```powershell
Get-AzRoleDefinition | FT Name, Description
```

Вы увидите список встроенных ролей данных службы хранилища Azure вместе с другими встроенными ролями Azure:

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-azure-role-to-a-security-principal"></a>Назначение роли Azure субъекту безопасности

Чтобы назначить роль Azure субъекту безопасности, используйте команду [New-азролеассигнмент](/powershell/module/az.resources/new-azroleassignment) . Формат команды может различаться в зависимости от области назначения. Чтобы выполнить команду, необходимо, чтобы владелец или роль участника были назначены в соответствующей области. В следующих примерах показано, как назначить роль пользователю в различных областях, но можно использовать ту же команду, чтобы назначить роль любому субъекту безопасности.

### <a name="container-scope"></a>Область контейнера

Чтобы назначить роль для контейнера, укажите строку, содержащую область действия контейнера для `--scope` параметра. Область для контейнера имеет вид:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

В следующем примере роль **участника данных BLOB-объекта хранилища** назначается пользователю с областью действия контейнера с именем *Sample-Container*. Обязательно замените образцы значений и значения заполнителей в квадратных скобках собственными значениями: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### <a name="queue-scope"></a>Область очереди

Чтобы назначить роль, ограниченную очередью, укажите строку, содержащую область действия очереди для `--scope` параметра. Область для очереди имеет вид:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

В следующем примере роль **участника данных очереди хранилища** назначается пользователю в очереди с именем *Sample-Queue*. Обязательно замените образцы значений и значения заполнителей в квадратных скобках собственными значениями: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### <a name="storage-account-scope"></a>Область учетной записи хранения

Чтобы назначить роль, ограниченную учетной записью хранения, укажите область ресурса учетной записи хранения для `--scope` параметра. Область для учетной записи хранения имеет вид:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

В следующем примере показано, как ограничить роль **модуля чтения данных большого двоичного объекта хранилища** для пользователя на уровне учетной записи хранения. Обязательно замените образцы значений собственными значениями: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>Область группы ресурсов

Чтобы назначить роль, ограниченную группой ресурсов, укажите имя или идентификатор группы ресурсов для `--resource-group` параметра. В следующем примере роль **модуля чтения данных очереди хранилища** назначается пользователю на уровне группы ресурсов. Не забудьте заменить образцы значений и значения заполнителей в квадратных скобках собственными значениями: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Reader" `
    -ResourceGroupName "sample-resource-group"
```

### <a name="subscription-scope"></a>Область действия подписки

Чтобы назначить роль, ограниченную подпиской, укажите область действия подписки для `--scope` параметра. Область для подписки имеет вид:

```
/subscriptions/<subscription>
```

В следующем примере показано, как назначить роль **модуля чтения данных большого двоичного объекта хранилища** пользователю на уровне учетной записи хранения. Обязательно замените образцы значений собственными значениями: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>Дальнейшие действия

- [Управление доступом с помощью RBAC и Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Предоставление доступа к BLOB-объектам Azure и создание очереди данных с использованием RBAC с помощью Azure CLI](storage-auth-aad-rbac-cli.md)
- [Предоставление доступа к BLOB-объектам Azure и создание очереди данных с использованием RBAC на портале Azure](storage-auth-aad-rbac-portal.md)
