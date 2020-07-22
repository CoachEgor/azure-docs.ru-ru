---
title: Безопасная доставка веб-перехватчика с помощью Azure AD в службе "Сетка событий Azure"
description: Описание доставки событий в конечные точки HTTPS, защищенные Azure Active Directory с помощью службы "Сетка событий Azure"
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: b0503d7da9e191e9d6764076392ead8faa5109a1
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119129"
---
# <a name="publish-events-to-azure-active-directory-protected-endpoints"></a>Публикация событий в защищенных конечных точках Azure Active Directory

В этой статье описывается, как использовать преимущества Azure Active Directory для защиты подключения между подпиской на события и конечной точкой веб-перехватчика. Общие сведения о приложениях и субъектах-службах Azure AD см. в статье [Общие сведения о платформе удостоверений Майкрософт версии 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-overview).

В этой статье используется портал Azure для демонстрации, однако эту функцию можно также включить с помощью интерфейса командной строки, PowerShell или пакетов SDK.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-an-azure-ad-application"></a>Создание приложения Azure AD

Начните с создания приложения Azure AD для защищенной конечной точки. См. раздел https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview.
    - Настройте защищенный API, который будет вызываться приложением управляющей программы.
    
## <a name="enable-event-grid-to-use-your-azure-ad-application"></a>Включение использования приложения Azure AD в службе "Сетка событий"

Используйте приведенный ниже сценарий PowerShell, чтобы создать роль и субъект-службу в приложении Azure AD. Вам потребуется идентификатор клиента и идентификатор объекта из приложения Azure AD:

   > [!NOTE]
   > Для выполнения этого сценария необходимо быть членом [роли администратора приложения Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#available-roles).
    
1. Измените $myTenantId сценария PowerShell, чтобы использовать идентификатор клиента Azure AD.
1. Измените $myAzureADApplicationObjectId сценария PowerShell, чтобы использовать идентификатор объекта приложения Azure AD.
1. Запустите измененный сценарий.

```PowerShell
# This is your Tenant Id. 
$myTenantId = "<the Tenant Id of your Azure AD Application>"

Connect-AzureAD -TenantId $myTenantId
    
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the "Azure Event Grid" Azure Active Directory AppId
$eventGridAppId = "4962773b-9cdb-44cf-a8bf-237846a00ab7"
    
# This is the name of the new role we will add to your Azure AD Application
$eventGridRoleName = "AzureEventGridSecureWebhook"
    
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
    
# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles
$eventGridSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $eventGridAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match $eventGridRoleName)
{
    Write-Host "The Azure Event Grid role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $eventGridRoleName -Description "Azure Event Grid Role"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Create the service principal if it doesn't exist
if ($eventGridSP -match "Microsoft.EventGrid")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the "Azure Event Grid" Azure AD Application and add it to the role
    $eventGridSP = New-AzureADServicePrincipal -AppId $eventGridAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventGridSP.ObjectId -PrincipalId $eventGridSP.ObjectId
    
Write-Host "My Azure AD Tenant Id: $myTenantId"
Write-Host "My Azure AD Application Id: $($myApp.AppId)"
Write-Host "My Azure AD Application ObjectId: $($myApp.ObjectId)"
Write-Host "My Azure AD Application's Roles: "
Write-Host $myApp.AppRoles
```
    
## <a name="configure-the-event-subscription"></a>Настройка подписки на события

В последовательности создания подписки на события выберите тип конечной точки "веб-перехватчик". Получив URI конечной точки, щелкните вкладку Дополнительные компоненты в верхней части колонки создание подписок на события.

![Выберите веб-перехватчик типа конечной точки](./media/secure-webhook-delivery/select-webhook.png)

На вкладке Дополнительные компоненты установите флажок "использовать проверку подлинности AAD" и настройте идентификатор клиента и идентификатор приложения:

* Скопируйте идентификатор клиента Azure AD из выходных данных скрипта и введите его в поле Идентификатор клиента AAD.
* Скопируйте идентификатор приложения Azure AD из выходных данных скрипта и введите его в поле Application ID (идентификатор приложения AAD).

    ![Безопасное действие веб-перехватчика](./media/secure-webhook-delivery/aad-configuration.png)

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь со сведениями о [мониторинге доставки сообщений в службе "Сетка событий"](monitor-event-delivery.md).
* Дополнительные сведения о ключе аутентификации см. в статье [Сетка событий: безопасность и проверка подлинности](security-authentication.md).
* Дополнительные сведения о создании подписки на Сетку событий Azure см. в статье [Схема подписки для службы "Сетка событий"](subscription-creation-schema.md).
