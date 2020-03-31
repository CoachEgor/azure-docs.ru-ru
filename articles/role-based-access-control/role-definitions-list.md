---
title: Перечислите определения ролей в Azure RBAC с помощью портала Azure, Azure PowerShell, Azure CLI или REST API Документы Майкрософт
description: Узнайте, как перечислить встроенные и пользовательские роли в Azure RBAC с помощью портала Azure, Azure PowerShell, Azure CLI или REST API.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: aa888eedc81ceb3188f801e273c70722207bf512
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062988"
---
# <a name="list-role-definitions-in-azure-rbac"></a>Список определений ролей в Azure RBAC

Определение роли — это набор разрешений, которые могут быть выполнены, такие как чтение, запись и удаление. Обычно это называется ролью. [Контроль доступа на основе ролей Azure (RBAC)](overview.md) имеет более 120 [встроенных ролей](built-in-roles.md) или вы можете создавать свои собственные пользовательские роли. В этой статье описывается, как перечислить встроенные и пользовательские роли, которые можно использовать для предоставления доступа к ресурсам Azure.

Чтобы просмотреть список ролей администратора для [Administrator role permissions in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md)каталога Активных функций Azure, см.

## <a name="azure-portal"></a>Портал Azure

### <a name="list-all-roles"></a>Вывод списка всех ролей

Выполните следующие действия, чтобы перечислить все роли на портале Azure.

1. На портале Azure щелкните **все службы,** а затем выберите любую область. Например, можно выбрать **Группы управления**, **Подписки**, **Группы ресурсов** или конкретный ресурс.

1. Нажмите на конкретный ресурс.

1. Нажмите **на элемент управления доступом (IAM)**.

1. Чтобы просмотреть список всех встроенных и пользовательских ролей, щелкните вкладку **Роли**.

   Вы можете увидеть количество пользователей и групп, назначенных каждой роли в текущем объеме.

   ![Список ролей](./media/role-definitions-list/roles-list.png)

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="list-all-roles"></a>Вывод списка всех ролей

Чтобы перечислить все роли в Azure PowerShell, используйте [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition).

```azurepowershell
Get-AzRoleDefinition | FT Name, Description
```

```Example
AcrImageSigner                                    acr image signer
AcrQuarantineReader                               acr quarantine data reader
AcrQuarantineWriter                               acr quarantine data writer
API Management Service Contributor                Can manage service and the APIs
API Management Service Operator Role              Can manage service but not the APIs
API Management Service Reader Role                Read-only access to service and APIs
Application Insights Component Contributor        Can manage Application Insights components
Application Insights Snapshot Debugger            Gives user permission to use Application Insights Snapshot Debugge...
Automation Job Operator                           Create and Manage Jobs using Automation Runbooks.
Automation Operator                               Automation Operators are able to start, stop, suspend, and resume ...
...
```

### <a name="list-a-role-definition"></a>Перечислите определение роли

Чтобы перечислить детали конкретной роли, используйте [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition).

```azurepowershell
Get-AzRoleDefinition <role_name>
```

```Example
PS C:\> Get-AzRoleDefinition "Contributor"

Name             : Contributor
Id               : b24988ac-6180-42a0-ab88-20f7382dd24c
IsCustom         : False
Description      : Lets you manage everything except access to resources.
Actions          : {*}
NotActions       : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
                   Microsoft.Authorization/elevateAccess/Action}
DataActions      : {}
NotDataActions   : {}
AssignableScopes : {/}
```

### <a name="list-a-role-definition-in-json-format"></a>Перечислите определение роли в формате JSON

Чтобы перечислить роль в формате JSON, используйте [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition).

```azurepowershell
Get-AzRoleDefinition <role_name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzRoleDefinition "Contributor" | ConvertTo-Json

{
  "Name": "Contributor",
  "Id": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "IsCustom": false,
  "Description": "Lets you manage everything except access to resources.",
  "Actions": [
    "*"
  ],
  "NotActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action",
    "Microsoft.Blueprint/blueprintAssignments/write",
    "Microsoft.Blueprint/blueprintAssignments/delete"
  ],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/"
  ]
}
```

### <a name="list-permissions-of-a-role-definition"></a>Список разрешений определения роли

Чтобы перечислить разрешения на определенную роль, используйте [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition).

```azurepowershell
Get-AzRoleDefinition <role_name> | FL Actions, NotActions
```

```Example
PS C:\> Get-AzRoleDefinition "Contributor" | FL Actions, NotActions

Actions    : {*}
NotActions : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
             Microsoft.Authorization/elevateAccess/Action,
             Microsoft.Blueprint/blueprintAssignments/write...}
```

```azurepowershell
(Get-AzRoleDefinition <role_name>).Actions
```

```Example
PS C:\> (Get-AzRoleDefinition "Virtual Machine Contributor").Actions

Microsoft.Authorization/*/read
Microsoft.Compute/availabilitySets/*
Microsoft.Compute/locations/*
Microsoft.Compute/virtualMachines/*
Microsoft.Compute/virtualMachineScaleSets/*
Microsoft.DevTestLab/schedules/*
Microsoft.Insights/alertRules/*
Microsoft.Network/applicationGateways/backendAddressPools/join/action
Microsoft.Network/loadBalancers/backendAddressPools/join/action
...
```

## <a name="azure-cli"></a>Azure CLI

### <a name="list-all-roles"></a>Вывод списка всех ролей

Чтобы перечислить все роли в Azure CLI, используйте [список определений ролей az.](/cli/azure/role/definition#az-role-definition-list)

```azurecli
az role definition list
```

В следующем примере показан список имен доступных определений роли вместе с описанием.

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.roleName, "description":.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

В следующем примере перечислены все встроенные роли.

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "roleType": "BuiltInRole"
}

...
```

### <a name="list-a-role-definition"></a>Перечислите определение роли

Чтобы перечислить детали роли, используйте [список определения роли az.](/cli/azure/role/definition#az-role-definition-list)

```azurecli
az role definition list --name <role_name>
```

В следующем примере показано определение роли *Участник*.

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

### <a name="list-permissions-of-a-role-definition"></a>Список разрешений определения роли

В следующем примере перечислены только *действия,* а *недействия* роли *вкладчика.*

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.permissions[0].actions, "notActions":.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

В следующем примере перечислены только действия роли *вкладчика виртуальной машины.*

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="rest-api"></a>REST API

### <a name="list-role-definitions"></a>Вывод списка определений роли

Чтобы перечислить определения ролей, используйте [определение ролей - Список](/rest/api/authorization/roledefinitions/list) REST API. Чтобы уточнить результаты, укажите область и дополнительный фильтр.

1. Можете начать со следующего запроса:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?$filter={$filter}&api-version=2015-07-01
    ```

1. В URI замените *«область»* областью, в которой вы хотите перечислить определения ролей.

    > [!div class="mx-tableFixed"]
    > | Область | Тип |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | группа управления; |
    > | `subscriptions/{subscriptionId1}` | Подписка |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Группа ресурсов |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Ресурс |

    В предыдущем примере microsoft.web — это поставщик ресурсов, который относится к экземпляру Службы приложений. Аналогичным образом можно использовать любые другие поставщики ресурсов и указать область охвата. Для получения дополнительной информации смотрите [поставщики и типы ресурсов Ресурсов Azure и](../azure-resource-manager/management/resource-providers-and-types.md) поддерживается [ресурсами, которые поддерживаются ресурсами ресурсного поставщика ресурсов Azure.](resource-provider-operations.md)  
     
1. Замените *«фильтр»* условием, которое необходимо применить для фильтрации списка определения ролей.

    > [!div class="mx-tableFixed"]
    > | Filter | Описание |
    > | --- | --- |
    > | `$filter=atScopeAndBelow()` | Перечисляет определения ролей для указанной области и любых подскопов. |
    > | `$filter=type+eq+'{type}'` | Перечисляет определения ролей указанного типа. Тип роли может `CustomRole` `BuiltInRole`быть или . |

### <a name="list-a-role-definition"></a>Перечислите определение роли

Чтобы перечислить детали конкретной роли, используйте [определения ролей - Получить](/rest/api/authorization/roledefinitions/get) или определения ролей - Get By [Id](/rest/api/authorization/roledefinitions/getbyid) REST API.

1. Можете начать со следующего запроса:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    Для определения роли на уровне каталога можно использовать этот запрос:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. В URI замените *«область»* областью, в которой вы хотите перечислить определение роли.

    > [!div class="mx-tableFixed"]
    > | Область | Тип |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | группа управления; |
    > | `subscriptions/{subscriptionId1}` | Подписка |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Группа ресурсов |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Ресурс |
     
1. Замените *{roleDefinitionId}* идентификатором определения роли.

## <a name="next-steps"></a>Дальнейшие действия

- [Встроенные роли управления доступом на основе ролей в Azure](built-in-roles.md)
- [Пользовательские роли для ресурсов Azure](custom-roles.md)
- [Список ролевых заданий с помощью Azure RBAC и портала Azure](role-assignments-list-portal.md)
- [Добавление или удаление ролевых заданий с помощью Azure RBAC и портала Azure](role-assignments-portal.md)
