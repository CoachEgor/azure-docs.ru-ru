---
title: Управление доступом к ресурсам Azure с помощью RBAC и REST API Azure | Документация Майкрософт
description: Узнайте, как управлять доступом пользователей, групп и приложений к ресурсам Azure с помощью управления доступом на основе ролей (RBAC) и REST API. Сюда также входят сведения о том, как перечислять, предоставлять и удалять права доступа.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 0e0c83d411242be38992dd763dea72eda70ffbf4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531845"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-rest-api"></a>Управление доступом к ресурсам Azure с помощью RBAC и REST API

[Управление доступом на основе ролей (RBAC)](overview.md) — это способ управления доступом к ресурсам Azure. В этой статье описывается, как управлять доступом пользователей, групп и приложений, используя RBAC и REST API.

## <a name="list-access"></a>Вывод списка доступа

При использовании RBAC, чтобы узнать, кому предоставлен доступ, вам нужно получить список назначений ролей. Чтобы перечислить назначения ролей, используйте один из REST API [Назначения ролей — список](/rest/api/authorization/roleassignments/list). Чтобы уточнить результаты, укажите область и дополнительный фильтр.

1. Можете начать со следующего запроса:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. Внутри URI замените *{scope}* областью, для которой требуется вывести список назначений ролей.

    | Область | type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Подписка |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Группа ресурсов |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ресурс |

1. Замените *{filter}* условием, по которому требуется отфильтровать список назначений ролей.

    | Фильтр | ОПИСАНИЕ |
    | --- | --- |
    | `$filter=atScope()` | Вывод списка назначений ролей только для определенной области без учета внутренних областей. |
    | `$filter=principalId%20eq%20'{objectId}'` | Вывод списка назначений ролей для определенного пользователя, группы или субъекта-службы. |
    | `$filter=assignedTo('{objectId}')` | Вывод списка назначений ролей для определенного пользователя, включая унаследованные от групп. |

## <a name="grant-access"></a>Предоставление доступа

При использовании RBAC, чтобы предоставить доступ, нужно создать назначение ролей. Чтобы создать назначение роли, используйте REST API [Назначения ролей — создание](/rest/api/authorization/roleassignments/create) и укажите субъект безопасности, определение роли и область. Чтобы вызвать этот API, нужен доступ к операции `Microsoft.Authorization/roleAssignments/write`. Из встроенных ролей эту операцию могут выполнять только [владелец](built-in-roles.md#owner) и [администратор доступа пользователей](built-in-roles.md#user-access-administrator).

1. Используйте REST API [Определения ролей — список](/rest/api/authorization/roledefinitions/list) или изучите раздел [Встроенные роли](built-in-roles.md), чтобы получить идентификатор для определения роли, которое требуется назначить.

1. Используйте средство GUID для создания уникального идентификатора, который будет использоваться для назначения роли. Этот идентификатор имеет следующий формат: `00000000-0000-0000-0000-000000000000`

1. Можете начать со следующего запроса и основного текста:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```
    
1. Внутри URI замените *{scope}* областью для назначения роли.

    | Область | type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Подписка |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Группа ресурсов |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ресурс |

1. Замените *{roleAssignmentName}* идентификатором GUID для назначения роли.

1. В тексте запроса замените *{subscriptionId}* своим идентификатором подписки.

1. Замените *{roleDefinitionId}* идентификатором определения роли.

1. Замените *{principalId}* идентификатором объекта для пользователя, группы или субъекта-службы, которым будет назначена роль.

## <a name="remove-access"></a>Запрет доступа

В RBAC, чтобы удалить доступ, нужно удалить назначение роли. Чтобы удалить назначение роли, используйте REST API [Назначения ролей — удаление](/rest/api/authorization/roleassignments/delete). Чтобы вызвать этот API, нужен доступ к операции `Microsoft.Authorization/roleAssignments/delete`. Из встроенных ролей эту операцию могут выполнять только [владелец](built-in-roles.md#owner) и [администратор доступа пользователей](built-in-roles.md#user-access-administrator).

1. Получите идентификатор (GUID) для назначения роли. Его можно получить при первом создании назначения роли, а также при перечислении назначений ролей.

1. Можете начать со следующего запроса:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. Внутри URI замените *{scope}* областью для удаления назначения роли.

    | Область | type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Подписка |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Группа ресурсов |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ресурс |

1. Замените *{roleAssignmentName}* идентификатором GUID для назначения роли.

## <a name="next-steps"></a>Дальнейшие действия

- [Развертывание ресурсов с использованием шаблонов и REST API Resource Manager](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Справочник по REST API Azure](/rest/api/azure/)
- [Создание пользовательских ролей для ресурсов Azure с помощью REST API](custom-roles-rest.md)