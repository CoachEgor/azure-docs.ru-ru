---
title: Защита зоны DNS и записей — Azure DNS
description: В этой схеме обучения приступите к защите зон и наборов записей DNS в Microsoft Azure DNS.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 12/4/2018
ms.author: rohink
ms.openlocfilehash: 549090f04f4969b00dc1c8ee8d5cc70a50523ca8
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/04/2020
ms.locfileid: "76983832"
---
# <a name="how-to-protect-dns-zones-and-records"></a>Как защитить зоны и записи DNS

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Зоны и записи DNS являются критически важными ресурсами. Удаление зоны DNS или даже одной записи DNS может привести к полному отключению службы.  Поэтому важно, чтобы критически важные зоны и записи DNS были защищены от несанкционированных или случайных изменений.

В этой статье объясняется, как с помощью службы DNS Azure защитить зоны и записи DNS от таких изменений.  Мы применяем две эффективные функции безопасности, предоставляемые Azure Resource Manager: [управление доступом на основе ролей](../role-based-access-control/overview.md) и [блокировки ресурсов](../azure-resource-manager/management/lock-resources.md).

## <a name="role-based-access-control"></a>Контроль доступа на основе ролей

Управление доступом на основе ролей (RBAC) Azure обеспечивает точное управление доступом для пользователей, групп и ресурсов Azure. С помощью RBAC вы можете с высокой точностью предоставлять пользователям доступ, необходимый для выполнения поставленных перед ними задач. Дополнительные сведения о том, как RBAC помогает управлять доступом, см. в статье [Начало работы с управлением доступом на портале Azure](../role-based-access-control/overview.md).

### <a name="the-dns-zone-contributor-role"></a>Роль "Участник зоны DNS"

Встроенная роль "Участник зоны DNS" предоставляется в Azure для управления ресурсами DNS.  Назначение пользователю или группе разрешений участника зоны DNS дает возможность этой группе управлять ресурсами DNS, но не позволяет управлять ресурсами других типов.

Предположим, например, что группа ресурсов *myzones* содержит пять зон для компании Contoso Corporation. Если предоставить администратору DNS разрешения "Участник зоны DNS" для этой группы ресурсов, то он получит полный контроль над этими зонами DNS. Это также позволяет избежать предоставления ненужных разрешений. Например, администратор DNS не сможет создавать или останавливать виртуальные машины.

Самый простой способ назначения разрешений RBAC — [через портал Azure](../role-based-access-control/role-assignments-portal.md).  Откройте колонку **Управление доступом (IAM)** для группы ресурсов, затем щелкните **Добавить**, выберите роль **Участник зоны DNS** и выберите пользователей или группы, которым будут предоставлены эти разрешения.

![Назначение разрешений RBAC на уровне группы ресурсов через портал Azure](./media/dns-protect-zones-recordsets/rbac1.png)

Разрешения также можно [предоставить с помощью Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

Аналогичную команду также [можно выполнить через интерфейс командной строки Azure](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>RBAC на уровне зоны

Правила RBAC Azure могут применяться к подписке, группе ресурсов или к отдельному ресурсу. В случае службы DNS Azure таким ресурсом может быть отдельная зона DNS или даже отдельный набор записей.

Предположим, например, что группа ресурсов *myzones* содержит зону *contoso.com* и подзону *customers.contoso.com*, в которой для каждой учетной записи клиента созданы записи CNAME.  Учетной записи, используемой для управления этими записями CNAME, необходимо назначить разрешения на создание записей только в зоне *customers.contoso.com*. Она не должна иметь доступ к другим зонам.

Разрешения RBAC на уровне зоны можно предоставлять через портал Azure.  Откройте колонку **Управление доступом (IAM)** для зоны, затем щелкните **Добавить**, выберите роль **Участник зоны DNS** и выберите пользователей или группы, которым будут предоставлены эти разрешения.

![Назначение разрешений RBAC на уровне зоны DNS через портал Azure](./media/dns-protect-zones-recordsets/rbac2.png)

Разрешения также можно [предоставить с помощью Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

Аналогичную команду также [можно выполнить через интерфейс командной строки Azure](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>RBAC на уровне набора записей

Мы можем пойти еще дальше. Давайте рассмотрим пример с администратором почтового сервера в Contoso Corporation, которому требуется доступ к записям MX и TXT на вершине зоны contoso.com.  Ему не нужен доступ к другим записям MX и TXT или к записям любого другого типа.  Служба DNS Azure позволяет назначить разрешения на уровне набора записей, именно к тем записям, к которым администратору почтового сервера требуется доступ.  Администратор почтового сервера получает именно тот уровень контроля, который ему необходим, и не может вносить другие изменения.

Разрешения RBAC на уровне набора записей можно настроить через портал Azure с помощью кнопки **Пользователи** в колонке набора записей:

![Назначение разрешений RBAC на уровне набора записей через портал Azure](./media/dns-protect-zones-recordsets/rbac3.png)

Разрешения RBAC на уровне набора записей также можно [предоставить с помощью Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant permissions to a specific record set
New-AzRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

Аналогичную команду также [можно выполнить через интерфейс командной строки Azure](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Пользовательские роли

Встроенная роль "Участник зоны DNS" разрешает полный контроль над ресурсом DNS. Можно также создавать собственные пользовательские роли Azure, чтобы обеспечить более детализированный контроль.

Давайте вернемся к примеру, где в зоне *customers.contoso.com* для каждой учетной записи клиента Contoso Corporation создана запись CNAME.  Учетной записи, используемой для управления этими записями CNAME, необходимо предоставить разрешение на управление только записями CNAME.  В этом случае она не сможет изменять записи других типов (такие как записи MX) или выполнять операции на уровне зоны, такие как удаление зоны.

В следующем примере показано определение пользовательской роли для управления только записями CNAME:

```json
{
    "Name": "DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/dnsZones/CNAME/*",
        "Microsoft.Network/dnsZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/ c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

Свойство Actions определяет следующие разрешения для DNS:

* `Microsoft.Network/dnsZones/CNAME/*` предоставляет полный контроль над записями CNAME.
* `Microsoft.Network/dnsZones/read` предоставляет разрешение на чтение зон DNS, но не позволяет их изменять. Таким образом вы можете видеть зону, в которой создается запись CNAME.

Остальные элементы свойства Actions копируются из [встроенной роли участника зоны DNS](../role-based-access-control/built-in-roles.md#dns-zone-contributor).

> [!NOTE]
> Применение пользовательской роли RBAC для предотвращения удаления наборов записей, разрешая при этом их обновление, не является эффективным. Наборы записей нельзя удалить, но ничто не препятствует их изменению.  К разрешенным изменениям относятся добавление и удаление записей из набора записей, включая удаление всех записей (при этом остается "пустой" набор записей). Это действует так же, как удаление набора записей с точки зрения разрешения DNS.

В настоящее время определения пользовательских ролей не определяются через портал Azure. Пользовательскую роль на основе этого определения роли можно создать с помощью Azure PowerShell:

```azurepowershell
# Create new role definition based on input file
New-AzRoleDefinition -InputFile <file path>
```

Ее также можно создать с помощью интерфейса командной строки Azure:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

Затем роль можно назначить таким же образом, как и встроенные роли. Этот процесс описан ранее в этой статье.

Дополнительные сведения о создании и назначении пользовательских ролей, а также об управлении ими см. в статье [Пользовательские роли в Azure RBAC](../role-based-access-control/custom-roles.md).

## <a name="resource-locks"></a>Блокировки ресурсов

Помимо функции RBAC Azure Resource Manager поддерживает и другой тип защиты — возможность "блокировать" ресурсы. Если правила RBAC позволяют контролировать действия определенных пользователей и групп, то блокировки ресурсов применяются к конкретному ресурсу и действуют для всех пользователей и ролей. Дополнительные сведения см. в статье [Блокировка ресурсов с помощью диспетчера ресурсов Azure](../azure-resource-manager/management/lock-resources.md).

Существует два типа блокировки ресурсов: **CanNotDelete** и **ReadOnly**. Они могут применяться к зоне DNS или к отдельному набору записей.  Следующие разделы описывают несколько распространенных сценариев и способы их поддержки с помощью блокировок ресурсов.

### <a name="protecting-against-all-changes"></a>Защита от любых изменений

Чтобы предотвратить внесение каких-либо изменений, примените к зоне блокировку ReadOnly.  Эта блокировка не позволит создавать наборы записей, а также изменять или удалять существующие наборы.

Блокировки ресурсов на уровне зоны можно создавать через портал Azure.  На странице зоны DNS выберите **Блокировки**, затем щелкните **+ Добавить**:

![Блокировка ресурсов на уровне зоны через портал Azure](./media/dns-protect-zones-recordsets/locks1.png)

Блокировки ресурсов на уровне зоны также можно создать с помощью Azure PowerShell:

```azurepowershell
# Lock a DNS zone
New-AzResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

В настоящее время настройка блокировки ресурсов Azure с помощью интерфейса командной строки Azure не поддерживается.

### <a name="protecting-individual-records"></a>Защита отдельных записей

Чтобы предотвратить внесение изменений в существующий набор записей DNS, примените к нему блокировку ReadOnly.

> [!NOTE]
> Применение к набору записей блокировки CanNotDelete (Запрет удаления) не является эффективным. Набор записей нельзя удалить, но ничто не препятствует его изменению.  К разрешенным изменениям относятся добавление и удаление записей из набора записей, включая удаление всех записей (при этом остается "пустой" набор записей). Это действует так же, как удаление набора записей с точки зрения разрешения DNS.

В настоящее время блокировки ресурсов на уровне наборов записей можно настраивать только с помощью Azure PowerShell.  Эта возможность пока не предоставляется через портал Azure или интерфейс командной строки Azure.

```azurepowershell
# Lock a DNS record set
New-AzResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Защита зоны от удаления

При удалении зоны в службе DNS Azure все наборы записей в этой зоне также удаляются.  Отменить эту операцию невозможно.  Случайное удаление критически важной зоны может оказать серьезное влияние на коммерческую деятельность.  Поэтому очень важно защитить зоны от случайного удаления.

Применение блокировки CanNotDelete (Запрет удаления) предотвращает удаление зоны.  Однако, поскольку блокировки наследуются дочерними ресурсами, это действие также предотвращает удаление всех наборов записей, находящихся в этой зоне, что может быть нежелательно.  Более того, как уже говорилось ранее в примечании, это неэффективно, так как записи по-прежнему могут удаляться из существующих наборов записей.

Альтернативным решением может быть применение блокировки CanNotDelete (Запрет удаления) к набору записей в зоне, например к набору записей типа SOA.  Так как зону нельзя удалить, не удалив также наборы записей, это обеспечивает защиту зоны от удаления, при этом наборы записей в зоне по-прежнему можно свободно изменять. При попытке удалить зону Azure Resource Manager определяет, что это действие также удалит набор записей типа SOA, и блокирует вызов, так как набор SOA заблокирован.  Наборы записей не будут удалены.

Следующая команда PowerShell создает блокировку CanNotDelete (Запрет удаления) для записи типа SOA в указанной зоне:

```azurepowershell
# Protect against zone delete with CanNotDelete lock on the record set
New-AzResourceLock -LockLevel CanNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType "Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Другой способ предотвратить случайное удаление зоны — настроить пользовательскую роль, благодаря чему в учетных записях оператора и службы, используемых для управления зонами, не будет разрешений на удаление зоны. Если вам понадобится удалить зону, можно применить двухэтапное удаление. Сначала нужно предоставить разрешения на удаление зоны (в области зоны, чтобы предотвратить удаление не той зоны), после чего ее можно будет удалить.

Преимущество этого подхода заключается в том, что он работает для всех зон, к которым у этих учетных записей есть доступ, и при этом не нужно создавать блокировки. Но у него также есть недостаток, заключающийся в том, что пользователи учетных записей с разрешениями на удаление зоны, например владелец подписки, по-прежнему могут случайно удалить критическую зону.

Оба подхода — блокировки ресурсов и пользовательские роли — можно использовать одновременно в качестве эшелонированного подхода к защите зоны DNS.

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о работе с RBAC см. в статье [Начало работы с управлением доступом на портале Azure](../role-based-access-control/overview.md).
* Дополнительные сведения о работе с блокировками ресурсов см. в статье [Блокировка ресурсов с помощью диспетчера ресурсов Azure](../azure-resource-manager/management/lock-resources.md).
