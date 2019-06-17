---
title: Исправление несоответствующих ресурсов
description: В этом руководстве описывается исправление ресурсов, которые не соответствуют политикам в службе "Политика Azure".
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: ba015a1d5183fcf27cfcc05ef1d0cd838201e91e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077123"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Исправление несоответствующих ресурсов с помощью службы "Политика Azure"

Ресурсы, которые не соответствуют политике **deployIfNotExists**, могут быть переведены в состояние соответствия с помощью **исправления**. Исправление достигается, указывая политику Azure для запуска **deployIfNotExists** влияние на существующие ресурсы назначенной политики. В этой статье показаны действия, необходимые для понимания и выполнения обновлений с помощью политики Azure.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="how-remediation-security-works"></a>Как работает безопасность исправлений

Если политика Azure работает шаблон **deployIfNotExists** Определение политики, он использует [управляемого удостоверения](../../../active-directory/managed-identities-azure-resources/overview.md).
Политика Azure создается управляемое удостоверение для каждого назначения, но должен иметь сведения о роли для предоставления управляемого удостоверения. Если в управляемом удостоверении отсутствуют роли, эта ошибка отображается во время назначения политики или в инициативе. При использовании портала, Azure автоматически предоставляется управляемого удостоверения перечисленных ролей после запуска назначения.

![Управляемое удостоверение. Отсутствующая роль](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> Если ресурс, изменяемый предложением **deployIfNotExists**, находится вне области назначения политики или шаблон обращается к свойствам ресурсов вне области назначения политики, управляемым удостоверениям назначения нужно [предоставить доступ вручную](#manually-configure-the-managed-identity), иначе развертывание исправлений завершится сбоем.

## <a name="configure-policy-definition"></a>Настройка определения политики

Первым шагом является определение ролей, которые требуются **deployIfNotExists** в определении политики для успешного развертывания содержимого включенного шаблона. В свойстве **details** добавьте свойство **roleDefinitionIds**. Это свойство является массивом строк, соответствующих ролям в среде. Полный пример см. в [примере deployIfNotExists](../concepts/effects.md#deployifnotexists-example).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

Свойство **roleDefinitionIds** использует полный идентификатор ресурса и не принимает короткое **имя** роли. Чтобы получить идентификатор для роли "Участник" в среде, используйте следующий код:

```azurecli-interactive
az role definition list --name 'Contributor'
```

## <a name="manually-configure-the-managed-identity"></a>Настройка управляемого удостоверения вручную

При создании назначения с помощью портала, "Политика Azure" и создает управляемого удостоверения и предоставляет ему роли, определенные в **roleDefinitionIds**. В следующих ситуациях шаги по созданию управляемого удостоверения и присвоению ему разрешений должны выполняться вручную:

- при использовании пакета SDK (например, Azure PowerShell);
- при изменении ресурсов, находящихся за пределами области назначения, с помощью шаблона;
- при чтении ресурсов, находящихся за пределами области назначения, с помощью шаблона.

> [!NOTE]
> В настоящее время только пакеты SDK Azure PowerShell и .NET поддерживают эту возможность.

### <a name="create-managed-identity-with-powershell"></a>Создание управляемого удостоверения с помощью PowerShell

Для создания управляемого удостоверения во время назначения политики необходимо определить свойство **Location** и использовать свойство **AssignIdentity**. Следующий пример получает определение встроенной политики **Развернуть прозрачное шифрование базы данных SQL**, задает целевую группу ресурсов, а затем создает назначение.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the built-in "Deploy SQL DB transparent data encryption" policy definition
$policyDef = Get-AzPolicyDefinition -Id '/providers/Microsoft.Authorization/policyDefinitions/86a912f6-9a06-4e26-b447-11b16ba8659f'

# Get the reference to the resource group
$resourceGroup = Get-AzResourceGroup -Name 'MyResourceGroup'

# Create the assignment using the -Location and -AssignIdentity properties
$assignment = New-AzPolicyAssignment -Name 'sqlDbTDE' -DisplayName 'Deploy SQL DB transparent data encryption' -Scope $resourceGroup.ResourceId -PolicyDefinition $policyDef -Location 'westus' -AssignIdentity
```

Переменная `$assignment` теперь содержит идентификатор субъекта для управляемого удостоверения, а также стандартные значения, возвращаемые при создании назначения политики. Доступ может осуществляться через `$assignment.Identity.PrincipalId`.

### <a name="grant-defined-roles-with-powershell"></a>Предоставление определенных ролей с помощью PowerShell

Новое управляемое удостоверение должно выполнить репликацию через Azure Active Directory, прежде чем ему можно будет предоставить необходимые роли. После завершения репликации приведенный ниже пример выполняет итерацию определения политики в `$policyDef` для **roleDefinitionIds** и использует командлет [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment), чтобы предоставить роли новому управляемому удостоверению.

```azurepowershell-interactive
# Use the $policyDef to get to the roleDefinitionIds array
$roleDefinitionIds = $policyDef.Properties.policyRule.then.details.roleDefinitionIds

if ($roleDefinitionIds.Count -gt 0)
{
    $roleDefinitionIds | ForEach-Object {
        $roleDefId = $_.Split("/") | Select-Object -Last 1
        New-AzRoleAssignment -Scope $resourceGroup.ResourceId -ObjectId $assignment.Identity.PrincipalId -RoleDefinitionId $roleDefId
    }
}
```

### <a name="grant-defined-roles-through-portal"></a>Предоставление определенных ролей через портал

На портале управляемому удостоверению назначения можно присвоить определенные роли двумя способами: используя раздел **управление доступом (IAM)** либо изменив назначение политики или инициативы и выбрав **Сохранить**.

Чтобы добавить роль для управляемого удостоверения назначения, выполните следующие действия:

1. Запустите службу "Политика Azure" на портале Azure, щелкнув **Все службы**, а затем выполнив поиск и выбрав **Политика**.

1. Выберите **Назначения** на странице службы "Политики Azure" слева.

1. Найдите назначение, которое имеет управляемое удостоверение, и щелкните его имя.

1. Найдите свойство **Идентификатор назначения** на странице изменения. Идентификатор назначения будет выглядеть примерно так:

   ```output
   /subscriptions/{subscriptionId}/resourceGroups/PolicyTarget/providers/Microsoft.Authorization/policyAssignments/2802056bfc094dfb95d4d7a5
   ```

   Имя управляемого удостоверения — это последняя часть идентификатора ресурса назначения (в этом примере `2802056bfc094dfb95d4d7a5`). Скопируйте эту часть идентификатора ресурса назначения.

1. Перейдите к ресурсу или родительскому контейнеру ресурсов (группа ресурсов, подписка, группа управления), для которого нужно вручную добавить определение роли.

1. Щелкните ссылку **Управление доступом (IAM)** на странице ресурсов и выберите **+ Добавить назначение ролей** в верхней части страницы управления доступом.

1. Выберите нужную роль, соответствующую **roleDefinitionIds** определения политики.
   В поле **Назначение доступа к** оставьте значение по умолчанию "Пользователь, группа или приложение Azure AD". В поле **Выберите** вставьте или введите часть идентификатора ресурса назначения, обнаруженного ранее. После завершения поиска щелкните объект с тем же именем, чтобы выбрать идентификатор, и нажмите кнопку **Сохранить**.

## <a name="create-a-remediation-task"></a>Создание задачи исправления

### <a name="create-a-remediation-task-through-portal"></a>Создать задачу исправления через портал

При оценке назначение политики с эффектом **deployIfNotExists** определяет, имеются ли несоответствующие ресурсы. При обнаружении несоответствующих ресурсов на странице **Исправление** предоставляются дополнительные сведения. Вместе со списком политик, имеющих несоответствующие ресурсы, отображается параметр активации **задачи исправления**. Это параметр, который создает развертывание из шаблона **deployIfNotExists**.

Чтобы создать **задачу исправления**, выполните следующие действия:

1. Запустите службу "Политика Azure" на портале Azure, щелкнув **Все службы**, а затем выполнив поиск и выбрав **Политика**.

   ![Поиск политики в разделе "Все службы"](../media/remediate-resources/search-policy.png)

1. Выберите **Исправление** на странице службы "Политика Azure" слева.

   ![Установите исправление на странице "политики"](../media/remediate-resources/select-remediation.png)

1. Все назначения политики **deployIfNotExists** с несоответствующими ресурсами содержатся на вкладке **Политики, подлежащие исправлению** и в таблице данных. Выберите политику с несоответствующими ресурсами. Откроется страница **Новая задача исправления**.

   > [!NOTE]
   > Альтернативным способом открытия страницы **задачи исправления** — найти и щелкнуть политику на странице **соответствия**, а затем нажать кнопку **Создать задачу исправления**.

1. На странице **Новая задача исправления** отфильтруйте ресурсы, которые нужно исправить, используя многоточие возле поля **Область**, чтобы выбрать дочерние ресурсы, которым была присвоена политика (в том числе отдельные объекты ресурса). Кроме того, используйте раскрывающийся список **Расположения**, чтобы дополнительно отфильтровать ресурсы. Будут исправлены только те ресурсы, которые перечислены в таблице.

   ![Исправление — выбрать, какие ресурсы для устранения](../media/remediate-resources/select-resources.png)

1. Запустите задачу по исправлению после того, как ресурсы были отфильтрованы, щелкнув **Исправить**. На странице соответствия политики откроется вкладка **Задачи исправления**, где отображается состояние хода выполнения задач.

   ![Исправлять - хода выполнения задач по исправлению](../media/remediate-resources/task-progress.png)

1. Щелкните **задачу исправления** на странице политики соответствия, чтобы получить сведения о ходе выполнения. Фильтрация, используемая для задачи, отображается вместе со списком ресурсов, которые будут исправлены.

1. На странице **задачи исправления** щелкните ресурс правой кнопкой мыши, чтобы просмотреть развертывание задачи обновления или ресурс. В конце строки щелкните **Связанные события**, чтобы просмотреть сведения, такие как сообщение об ошибке.

   ![Исправление. Контекстное меню задачи ресурса](../media/remediate-resources/resource-task-context-menu.png)

Ресурсы, развернутые посредством **задачи исправления**, добавляются на вкладку **Развернутые ресурсы** на странице соответствия политике.

### <a name="create-a-remediation-task-through-azure-cli"></a>Создать задачу исправления через интерфейс командной строки Azure

Чтобы создать **задачи исправления** с помощью Azure CLI, используйте `az policy remediation` команды. Замените `{subscriptionId}` своим идентификатором подписки и `{myAssignmentId}` с вашей **deployIfNotExists** идентификатор назначения политики.

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Create a remediation for a specific assignment
az policy remediation create --name myRemediation --policy-assignment '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Другие исправления команды и примеры, см. в разделе [исправления политики az](/cli/azure/policy/remediation) команды.

### <a name="create-a-remediation-task-through-azure-powershell"></a>Создать задачу исправления через оболочку Azure PowerShell

Чтобы создать **задачи исправления** с помощью Azure PowerShell, используйте `Start-AzPolicyRemediation` команды. Замените `{subscriptionId}` своим идентификатором подписки и `{myAssignmentId}` с вашей **deployIfNotExists** идентификатор назначения политики.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Create a remediation for a specific assignment
Start-AzPolicyRemediation -Name 'myRemedation' -PolicyAssignmentId '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Другие командлеты исправления и примеры см. в разделе [Az.PolicyInsights](/powershell/module/az.policyinsights/#policy_insights) модуля.

## <a name="next-steps"></a>Дальнейшие действия

- Просмотрите примеры доступны на [примеры политик Azure](../samples/index.md).
- Изучите статью о [структуре определения Политики Azure](../concepts/definition-structure.md).
- Изучите [сведения о действии политик](../concepts/effects.md).
- Понять, как [программное создание политик](programmatically-create.md).
- Узнайте, как [получить данные о соответствии](getting-compliance-data.md).
- Дополнительные сведения о группе управления см. в статье [Упорядочивание ресурсов с помощью групп управления Azure](../../management-groups/overview.md).
