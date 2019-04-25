---
title: Использование назначаемого системой управляемого удостоверения виртуальной машины Windows для доступа к API Graph в Azure AD
description: Из этого руководства вы узнаете, как получить доступ к API Graph в Azure AD с помощью назначаемого системой управляемого удостоверения виртуальной машины Windows.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60938f26c27b9f94046b1be8e3d0cb6b247017c9
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997790"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-ad-graph-api"></a>Руководство по Использование назначаемого системой управляемого удостоверения виртуальной машины Windows для доступа к API Graph в Azure AD

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice.md)]

В этом руководстве описано, как получить доступ к API Graph AAD с помощью назначенного системой управляемого удостоверения на виртуальной машине Windows для получения сведений о членстве в группах. Управляемыми удостоверениями для ресурсов Azure автоматически управляет Azure. Они позволяют проходить проверку подлинности в службах, поддерживающих аутентификацию Azure Active Directory, без указания учетных данных в коде.  В этом руководстве описано, как выполнить запрос на членство удостоверения виртуальной машины в группах Azure AD. Сведения о группе часто используются при принятии решений об авторизации, например. На уровне платформы AAD управляемое удостоверение виртуальной машины представляется **субъектом-службой**. Перед выполнением запроса на членство в группе добавьте субъект-службу, представляющий удостоверение виртуальной машины, в группу Azure AD. Это можно сделать с помощью Azure PowerShell, Azure AD PowerShell или Azure CLI.

> [!div class="checklist"]
> * Подключение к Azure AD
> * Добавление удостоверения виртуальной машины в группу Azure AD 
> * Предоставление удостоверению виртуальной машины доступа к Azure AD Graph 
> * Получение маркера доступа с использованием удостоверения виртуальной машины и применение маркера в вызове к API Graph AAD

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- Чтобы предоставить удостоверению виртуальной машины доступ к Azure AD Graph, необходимо назначить вашей учетной записи роль **глобального администратора** в Azure AD.
- Установите последнюю версию модуля [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2), если он не установлен. 

## <a name="connect-to-azure-ad"></a>Подключение к Azure AD

Необходимо подключиться к Azure AD, чтобы назначить виртуальную машину в группу, а также предоставить виртуальной машине разрешение на получение членства в группах. Можно использовать командлет Connect-AzureAD напрямую или с параметром TenantId, если у вас несколько клиентов.

```powershell
Connect-AzureAD
```
Или
```powershell
Connect-AzureAD -TenantId "Object Id of the tenant"
```

## <a name="add-your-vm-identity-to-a-group-in-azure-ad"></a>Добавление удостоверения виртуальной машины в группу Azure AD

При включении назначаемого системой управляемого удостоверения на виртуальной машине Windows оно создает субъект-службу в Azure AD.  Теперь вам нужно добавить виртуальную машину в группу.  В следующем примере создается группа в Azure AD, в которую добавляется субъект-служба вашей виртуальной машины:

```powershell
New-AzureADGroup -DisplayName "myGroup" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$AzureADGroup = Get-AzureADGroup -Filter "displayName eq 'myGroup'"
$ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq 'myVM'"
Add-AzureADGroupMember -ObjectId $AzureADGroup.ObjectID -RefObjectId $ManagedIdentitiesServicePrincipal.ObjectId
```
## <a name="grant-your-vm-access-to-the-azure-ad-graph-api"></a>Предоставление виртуальной машине доступа к API Graph в Azure AD

С помощью управляемых удостоверений для ресурсов Azure код может получить маркеры доступа, чтобы пройти проверку подлинности и получить доступ к ресурсам, поддерживающим аутентификацию Azure AD. API Graph в Microsoft Azure AD поддерживает проверку подлинности AAD. На этом шаге вы предоставите субъекту-службе удостоверения виртуальной машины доступ к Azure AD Graph, чтобы он мог запрашивать членство в группах. Субъектам-службам доступ к Microsoft или Azure AD Graph предоставляется с помощью **разрешений приложения**. Тип разрешения приложения, которое требуется для предоставления доступа, зависит от сущности, к которой необходимо получить доступ в Microsoft или Azure AD Graph.

В этом руководстве вы предоставите удостоверению виртуальной машины возможность запрашивать членство в группах с помощью разрешения приложения ```Directory.Read.All```. Чтобы предоставить это разрешение, необходима учетная запись пользователя, которой назначена роль глобального администратора в Azure AD. Как правило, чтобы предоставить разрешение приложения, можно открыть страницу регистрации приложения на портале Azure и добавить на ней разрешение. Однако управляемые удостоверения для ресурсов Azure не регистрируют в AAD объекты приложений, а только субъекты-службы. Чтобы зарегистрировать разрешение приложения, воспользуемся средством командной строки AAD PowerShell. 

Azure AD Graph:
- Идентификатор приложения субъекта-службы (используется при предоставлении разрешения приложения): 00000002-0000-0000-c000-000000000000
- Идентификатор ресурса (используется при запрашивании маркера доступа из управляемых удостоверений для ресурсов Azure): https://graph.windows.net
- Справочник по областям разрешений: [Области разрешений | Основные понятия API Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

### <a name="grant-application-permissions-using-azure-ad-powershell"></a>Предоставление разрешений для приложений с помощью Azure AD PowerShell

Для использования этого способа вам потребуется Azure AD PowerShell. Если это средство не установлено, [скачайте его последнюю версию](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2), прежде чем продолжить.

1. Откройте окно PowerShell и подключитесь к Azure AD:

   ```powershell
   Connect-AzureAD
   ```
   Чтобы подключиться к определенной Azure Active Directory, используйте параметр _TenantId_, как показано ниже.

   ```powershell
   Connect-AzureAD -TenantId "Object Id of the tenant"
   ```

   
2. Выполните следующие команды PowerShell, чтобы назначить разрешение приложения ``Directory.Read.All`` субъекту-службе, представляющему удостоверение виртуальной машины.

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq 'myVM'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}
   New-AzureAdServiceAppRoleAssignment -ObjectId $ManagedIdentitiesServicePrincipal.ObjectId -PrincipalId $ManagedIdentitiesServicePrincipal.ObjectId -ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole.Id
   ``` 

   Выходные данные последней команды, которые возвращают идентификатора назначения, должны выглядеть так:
        
   `ObjectId`:`gzR5KyLAiUOTiqFhNeWZWBtK7ZKqNJxAiWYXYVHlgMs`

   `ResourceDisplayName`:`Windows Azure Active Directory`

   `PrincipalDisplayName`:`myVM` 

   Если вызов `New-AzureAdServiceAppRoleAssignment` завершается ошибкой `bad request, one or more properties are invalid`, возможно, разрешение приложения уже назначено субъекту-службе удостоверения виртуальной машины. Для проверки наличия разрешения приложения между удостоверением виртуальной машины и Azure AD Graph можно воспользоваться следующими командами PowerShell:

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq '<VM-NAME>'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}
   Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId | Where-Object {$_.Id -eq $AppRole.Id -and $_.PrincipalId -eq $ManagedIdentitiesServicePrincipal.ObjectId}
   ```

   С помощью следующих команд PowerShell можно получить список всех разрешений для приложений, которые были предоставлены Azure AD Graph:

   ```powershell
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId
   ``` 

   С помощью следующих команд PowerShell можно удалить все разрешения для приложений, которые были предоставлены удостоверению виртуальной машины для Azure AD Graph:

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq '<VM-NAME>'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}   
   $ServiceAppRoleAssignment = Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId | Where-Object {$_.Id -eq $AppRole.Id -and $_.PrincipalId -eq $ManagedIdentitiesServicePrincipal.ObjectId}
   Remove-AzureADServiceAppRoleAssignment -AppRoleAssignmentId $ServiceAppRoleAssignment.ObjectId -ObjectId $ManagedIdentitiesServicePrincipal.ObjectId
   ```
 
## <a name="get-an-access-token-using-the-vms-identity-to-call-azure-ad-graph"></a>Получение маркера доступа с использованием удостоверения виртуальной машины в вызове к Azure AD Graph 

Чтобы использовать назначаемое системой управляемое удостоверение виртуальной машины для проверки подлинности в Azure AD Graph, нужно отправлять запросы с виртуальной машины.

1. На портале перейдите к разделу **Виртуальные машины**, выберите свою виртуальную машину Windows и в колонке **Обзор** щелкните **Подключить**.  
2. Введите **имя пользователя** и **пароль**, указанные при создании виртуальной машины Windows.
3. Создав подключение к удаленному рабочему столу с виртуальной машиной, откройте PowerShell в удаленном сеансе.  
4. С помощью командлета PowerShell Invoke-WebRequest выполните запрос к локальным управляемым удостоверениям для конечной точки ресурсов Azure, чтобы получить маркер доступа для Azure AD Graph.

   ```powershell
   $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://graph.windows.net' -Method GET -Headers @{Metadata="true"}
   ```

   Преобразуйте ответ из объекта JSON в объект PowerShell.

   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Извлеките маркер доступа из ответа.

   ```powershell
   $AccessToken = $content.access_token
   ```

5. Используя идентификатор объекта субъекта-службы удостоверения виртуальной машины (это значение можно получить из переменной, объявленной на предыдущих шагах: ``$ManagedIdentitiesServicePrincipal.ObjectId``), можно выполнить запрос к API Graph для Azure AD для получения членства в группах. Замените `<OBJECT ID>` идентификатором объекта из предыдущего шага, а <`ACCESS-TOKEN>` — ранее полученным маркером доступа.

   ```powershell
   Invoke-WebRequest 'https://graph.windows.net/<Tenant ID>/servicePrincipals/<VM Object ID>/getMemberGroups?api-version=1.6' -Method POST -Body '{"securityEnabledOnly":"false"}' -Headers @{Authorization="Bearer $AccessToken"} -ContentType "application/json"
   ```
   
   Ответ содержит `Object ID` группы, в которую вы добавили субъект-службу виртуальной машины на предыдущих шагах.

   Ответ:

   ```powershell   
   Content : {"odata.metadata":"https://graph.windows.net/<Tenant ID>/$metadata#Collection(Edm.String)","value":["<ObjectID of VM's group membership>"]}
   ```

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как получить доступ к Azure AD Graph с помощью назначенного системой управляемого удостоверения на виртуальной машине Windows.  Дополнительные сведения об Azure AD Graph см. в следующей статье:

>[!div class="nextstepaction"]
>[Azure AD Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api)
