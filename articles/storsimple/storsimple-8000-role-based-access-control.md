---
title: Использование управления доступом на основе ролей для StorSimple | Документация Майкрософт
description: В этой статье описано, как использовать управление доступом на основе ролей (RBAC) в Azure в контексте StorSimple.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2017
ms.author: alkohli
ms.openlocfilehash: a79753a897a62e194a759c23a9c0acc45c5f36c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "66159124"
---
# <a name="role-based-access-control-for-storsimple"></a>Управление доступом на основе ролей для StorSimple

В этой статье содержится краткое описание использования управления доступом на основе ролей (RBAC) в Azure для устройства StorSimple. RBAC предлагает точное управление доступом для Azure. Вместо предоставления всем неограниченного доступа используйте RBAC, чтобы назначить пользователям StorSimple только те права доступа, которые необходимы им для выполнения заданий. Общие сведения об управлении доступом в Azure см. в статье [Начало работы с управлением доступом на основе ролей на портале Azure](../role-based-access-control/overview.md).

Эта статья относится к устройствам StorSimple серии 8000 с обновлением 3.0 или более поздней версии на портале Azure.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-storsimple"></a>Роли RBAC для StorSimple

RBAC можно назначить на основе ролей. Роли предоставляют определенные уровни разрешений, исходя из доступных ресурсов в среде. Существует два типа ролей, которые пользователи StorSimple могут выбрать: встроенная и пользовательская.

* **Встроенные роли**. Доступны такие встроенные роли, как владелец, участник, читатель или администратор доступа пользователей. Для получения дополнительной информации смотрите [встроенные роли для управления доступом на основе ролей Azure.](../role-based-access-control/built-in-roles.md)

* **Пользовательские роли**. Если встроенные роли не подходят, для StorSimple можно создать пользовательские роли RBAC. Чтобы создать пользовательскую роль RBAC, запустите встроенную роль, отредактируйте ее и импортируйте обратно в среду. Скачивание и передача роли осуществляется с помощью Azure PowerShell или интерфейса командной строки Azure. Дополнительные сведения см. в статье [Создание пользовательских ролей для управления доступом на основе ролей в Azure](../role-based-access-control/custom-roles.md).

Чтобы просмотреть роли, доступные для пользователя устройства StorSimple на портале Azure, войдите в службу диспетчера устройств StorSimple, а затем выберите **Управление доступом (IAM) > Роли**.


## <a name="create-a-custom-role-for-storsimple-infrastructure-administrator"></a>Создание пользовательской роли для администратора инфраструктуры StorSimple

В следующем примере мы начнем со встроенной роли **Читатель**, которая позволяет пользователям просматривать все области действия ресурсов, но не изменять или создавать их. Затем мы расширяем эту роль, чтобы создать новую пользовательскую роль StorSimple инфраструктуры админ. Эта роль присваивается пользователям, которые могут управлять инфраструктурой для устройств StorSimple.

1. Запустите Windows PowerShell от имени администратора.

2. Войдите в Azure.

    `Connect-AzAccount`

3. Экспортируйте роль "Читатель" в качестве шаблона JSON на компьютер.

    ```powershell
    Get-AzRoleDefinition -Name "Reader"

    Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
    ```

4. Откройте JSON-файл в Visual Studio. Обратите внимание, что типичная роль RBAC состоит из трех основных разделов: **Actions**, **NotActions** и **AssignableScopes**.

    В разделе **Actions** перечислены все разрешенные операции этой роли. Каждое действие назначено из поставщика ресурсов. Для администратора инфраструктуры StorSimple используйте поставщик ресурсов `Microsoft.StorSimple`.

    Чтобы просмотреть все доступные и зарегистрированные в подписке поставщики ресурсов, используйте PowerShell.

    `Get-AzResourceProvider`

    Вы также можете проверить все доступные командлеты PowerShell для управления поставщиками ресурсов.

    В разделе **NotActions** перечислены все запрещенные действия для определенной роли RBAC. В этом примере нет запрещенных действий.
    
    В разделе **AssignableScopes** указаны идентификаторы подписок. Убедитесь, что роль RBAC содержит явные идентификаторы подписок, в которых она используется. Если указан неправильный идентификатор подписки, вы не сможете импортировать роль в подписке.

    Измените файл, учитывая предыдущие рекомендации.

    ```json
    {
        "Name":  "StorSimple Infrastructure Admin",
        "Id":  "<guid>",
        "IsCustom":  true,
        "Description":  "Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install, download etc.",
        "Actions":  [
                        "Microsoft.StorSimple/managers/alerts/read",
                        "Microsoft.StorSimple/managers/devices/volumeContainers/read",
                        "Microsoft.StorSimple/managers/devices/jobs/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/write",
                        "Microsoft.StorSimple/managers/clearAlerts/action",
                        "Microsoft.StorSimple/managers/devices/networkSettings/read",
                        "Microsoft.StorSimple/managers/devices/publishSupportPackage/action",
                        "Microsoft.StorSimple/managers/devices/scanForUpdates/action",
                        "Microsoft.StorSimple/managers/devices/metrics/read"

                    ],
        "NotActions":  [

                    ],
        "AssignableScopes":  [
                                "/subscriptions/<subscription_ID>/"
                            ]
    }
    ```

6. Импортируйте пользовательскую роль RBAC обратно в среду.

    `New-AzRoleDefinition -InputFile "C:\ssrbaccustom.json"`


Эта роль должна отобразиться в списке ролей в колонке **Управление доступом**.

![Просмотр ролей RBAC](./media/storsimple-8000-role-based-access-control/rbac-role-types.png)

Дополнительную информацию см. в разделе [Пользовательские роли](../role-based-access-control/custom-roles.md).

### <a name="sample-output-for-custom-role-creation-via-the-powershell"></a>Пример выходных данных для создания пользовательской роли через PowerShell

```powershell
Connect-AzAccount
```

```Output
Environment           : AzureCloud
Account               : john.doe@contoso.com
TenantId              : <tenant_ID>
SubscriptionId        : <subscription_ID>
SubscriptionName      : Internal Consumption
CurrentStorageAccount :
```

```powershell
Get-AzRoleDefinition -Name "Reader"
```

```Output
Name             : Reader
Id               : <guid>
IsCustom         : False
Description      : Lets you view everything, but not make any changes.
Actions          : {*/read}
NotActions       : {}
AssignableScopes : {/}
```

```powershell
Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
New-AzRoleDefinition -InputFile "C:\ssrbaccustom.json"
```

```Output
Name             : StorSimple Infrastructure Admin
Id               : <tenant_ID>
IsCustom         : True
Description      : Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install,
                   download etc.
Actions          : {Microsoft.StorSimple/managers/alerts/read,
                   Microsoft.StorSimple/managers/devices/volumeContainers/read,
                   Microsoft.StorSimple/managers/devices/jobs/read,
                   Microsoft.StorSimple/managers/devices/alertSettings/read...}
NotActions       : {}
AssignableScopes : {/subscriptions/<subscription_ID>/}
```

## <a name="add-users-to-the-custom-role"></a>Добавление пользователей в пользовательскую роль

Вы можете предоставить доступ в рамках ресурса, группы ресурсов или подписки, которые входят в область назначения роли. При предоставлении доступа учитывайте, что права доступа, предоставленные в родительском узле, наследуются дочерним узлом. Дополнительные сведения см. в статье [Начало работы с управлением доступом на основе ролей на портале Azure](../role-based-access-control/overview.md).

1. Перейдите на вкладку **Управление доступом (IAM)**. В колонке "Управление доступом" щелкните **+ Добавить**.

    ![Добавление прав доступа к роли RBAC](./media/storsimple-8000-role-based-access-control/rbac-add-role.png)

2. Выберите роль, которую необходимо назначить. В данном случае это роль **администратора инфраструктуры StorSimple**.

3. В каталоге выберите пользователя, группу или приложение, которым нужно предоставить доступ. Поиск в каталоге можно выполнить по отображаемым именам, адресам электронной почты и идентификаторам объектов.

4. Нажмите кнопку **Сохранить**, чтобы создать назначение.

    ![Добавление разрешений в роль RBAC](./media/storsimple-8000-role-based-access-control/rbac-create-role-infra-admin.png)

Уведомление **Добавление пользователя** показывает ход выполнения. После успешного добавления пользователя список пользователей в колонке управления доступом обновляется.

## <a name="view-permissions-for-the-custom-role"></a>Просмотр разрешений для пользовательской роли

После создания этой роли разрешения, связанные с ней, можно просмотреть на портале Azure.

1. Чтобы просмотреть разрешения, связанные с этой ролью, перейдите на **> роли управления доступом > StorSimple Infrastructure Admin.** Отображается список пользователей в этой роли.

2. Выберите администратора инфраструктуры StorSimple и нажмите кнопку **Permissions** (Разрешения).

    ![Просмотр разрешений для роли администратора инфраструктуры StorSimple](./media/storsimple-8000-role-based-access-control/rbac-roles-view-permissions.png)

3. Отображаются разрешения, связанные с этой ролью.

    ![Просмотр пользователей в роли администратора инфраструктуры StorSimple](./media/storsimple-8000-role-based-access-control/rbac-infra-admin-permissions1.png)


## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в статье [Общие сведения об управлении доступом на основе ролей](../role-based-access-control/role-assignments-external-users.md).
