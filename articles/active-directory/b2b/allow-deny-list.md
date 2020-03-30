---
title: Разрешить или заблокировать приглашения в определенные организации - Azure AD
description: Эта статья содержит сведения о том, как администратор может настроить список разрешенных и запрещенных для предоставления или отзыва приглашений пользователям B2B из определенных доменов с помощью портала Azure или PowerShell.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 07/15/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: sasubram
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b5100c4406cfd4a8395dfa177dc3cd5e911decb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "74273428"
---
# <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Предоставление или отзыв приглашений пользователям B2B из отдельных организаций

С помощью списка разрешенных или запрещенных можно предоставить или отозвать приглашения пользователям B2B из отдельных организаций. Например, если вы хотите заблокировать домены личного адреса электронной почты, можно настроить список запрещенных, содержащий домены, такие как Gmail.com и Outlook.com. Или, если ваша организация сотрудничает с другими предприятиями, например Contoso.com, Fabrikam.com и Litware.com, и вы хотите предоставлять приглашения только им, добавьте Contoso.com, Fabrikam.com и Litware.com в список разрешенных.
  
## <a name="important-considerations"></a>Важные сведения

- Вы можете создать только список разрешенных или запрещенных. Настроить оба типа списков нельзя. По умолчанию все домены, которые не входят в список разрешенных, помещаются в список запрещенных, и наоборот. 
- Для организации можно создать только одну политику. Ее можно обновить, чтобы включить дополнительные домены. Кроме того, вы можете удалить политику, чтобы создать другую. 
- Количество доменов, которые можно добавить в список разрешений или отрицать, ограничено только размером политики. Максимальный размер всей политики составляет 25 кБ (25 000 символов), который включает в себя список разрешений или отрицания и любые другие параметры, настроенные для других функций.
- Этот список и список разрешенных и запрещенных OneDrive для бизнеса и SharePoint Online независимы друг от друга. Если вы хотите ограничить общий доступ к отдельным файлам в SharePoint Online, необходимо настроить список разрешенных или запрещенных для OneDrive для бизнеса и SharePoint Online. Дополнительные сведения см. в статье [Ограничение общего доступа для доменов в SharePoint Online и OneDrive для бизнеса](https://support.office.com/article/restricted-domains-sharing-in-sharepoint-online-and-onedrive-for-business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9).
- Список не распространяется на внешних пользователей, которые уже выкупили приглашение. Список будет действовать после его настройки. Если приглашения пользователей находятся в состоянии ожидания и вы настроили политику, блокирующую их домены, пользователи не смогут активировать приглашение.

## <a name="set-the-allow-or-deny-list-policy-in-the-portal"></a>Настройка политики списка разрешенных и запрещенных на портале

Параметр **Allow invitations to be sent to any domain (most inclusive)** (Разрешить отправлять приглашения любому домену (всеохватный)) активирован по умолчанию. В этом случае вы можете пригласить пользователей B2B из любой организации.

### <a name="add-a-deny-list"></a>Добавление списка запрещенных

Это самый распространенный сценарий, когда ваша организация хочет сотрудничать со всеми организациями, но не желает приглашать пользователей определенных доменов в качестве пользователей B2B.

Чтобы добавить список запрещенных, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите > настройки > **пользователей****активных каталогов** **Azure.**
3. Под **внешними пользователями**выберите **Параметры внешнего взаимодействия.**
4. В разделе **Collaboration restrictions** (Ограничения сотрудничества) выберите **Deny invitations to the specified domains** (Запретить отправлять приглашения указанным доменам).
5. В разделе **Целевые домены** введите имя одного из доменов, который следует заблокировать. Если нужно заблокировать несколько доменов, вводите каждый из них в новой строке. Пример:

   ![Отображение параметра запрета приглашений и добавленных доменов](./media/allow-deny-list/DenyListSettings.png)
 
6. Закончив, нажмите кнопку **Сохранить**.

Если вы настроите политику и попытаетесь пригласить пользователя из заблокированного домена, отобразится сообщение о том, что домен пользователя заблокирован политикой приглашений.
 
### <a name="add-an-allow-list"></a>Добавление списка разрешенных

Это более строгая конфигурация, где можно настроить определенные домены в списке разрешенных и запретить отправлять приглашения любой организации или доменам, которые не указаны. 

Прежде чем использовать список разрешенных, убедитесь, что вы полностью проанализировали потребности своего бизнеса. Если политика будет слишком строгой, ваши пользователи могут начать отправлять документы по электронной почте или найти другие способы сотрудничества (не санкционированные ИТ-службой).


Чтобы добавить список разрешений, сделайте вот что:

1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите > настройки > **пользователей****активных каталогов** **Azure.**
3. Под **внешними пользователями**выберите **Параметры внешнего взаимодействия.**
4. В разделе **Ограничения взаимодействия** выберите **Разрешить отправку приглашений только в указанные домены (максимальное ограничение)**.
5. В разделе **Целевые домены** введите имя одного из доменов, который следует разрешить. Если нужно заблокировать несколько доменов, вводите каждый из них в новой строке. Пример:

   ![Отображение параметра разрешения приглашений и добавленных доменов](./media/allow-deny-list/AllowListSettings.png)
 
6. Закончив, нажмите кнопку **Сохранить**.

Если вы настроите политику и попытаетесь пригласить пользователя, домен которого не добавлен в список разрешений, отобразится сообщение о том, что домен пользователя заблокирован политикой приглашений.

### <a name="switch-from-allow-to-deny-list-and-vice-versa"></a>Переход со списка разрешенных на список запрещенных и наоборот 

При переходе на другую политику текущая конфигурация политики будет отменена. Прежде чем выполнить переход, создайте резервные копии сведений о конфигурации. 

## <a name="set-the-allow-or-deny-list-policy-using-powershell"></a>Настройка списка разрешенных или запрещенных с помощью PowerShell

### <a name="prerequisite"></a>Предварительные требования

> [!Note]
> Модуль AzureADPreview не является полностью поддерживаемым модулем, как это происходит в предварительном просмотре. 

Чтобы настроить список разрешенных или запрещенных с помощью PowerShell, необходимо установить предварительную версию модуля Azure Active Directory для Windows PowerShell. В частности, установите модуль AzureADPreview 2.0.0.98 или более поздней версии.

Чтобы проверить версию модуля (и проверить, установлен ли он), сделайте следующее:
 
1. Откройте Windows PowerShell с повышенными правами пользователя (запустите от имени администратора). 
2. Выполните следующую команду, чтобы проверить, установлены ли на компьютере версии модуля Azure Active Directory для Windows PowerShell:

   ```powershell  
   Get-Module -ListAvailable AzureAD*
   ```

Если модуль не установлен или у вас нет необходимой версии, выполните одно из следующих действий:

- Если результаты не отображаются, выполните следующую команду, чтобы установить последнюю версию модуля AzureADPreview:
  
   ```powershell  
   Install-Module AzureADPreview
   ```
- Если в результатах отображается только модуль AzureAD, выполните следующие команды, чтобы установить модуль AzureADPreview: 

   ```powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ```
- Если в результатах отображается только модуль AzureADPreview, но используется версия ниже 2.0.0.98, выполните следующие команды, чтобы обновить ее: 

   ```powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
   ```

- Если в результатах отображаются модули AzureAD и AzureADPreview, но версия модуля AzureADPreview ниже 2.0.0.98, выполните следующие команды, чтобы обновить ее: 

   ```powershell 
   Uninstall-Module AzureAD 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
    ```

### <a name="use-the-azureadpolicy-cmdlets-to-configure-the-policy"></a>Настройка политики с помощью командлетов AzureADPolicy

Чтобы создать список разрешенных или запрещенных, выполните командлет [New AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview). В следующем примере показано, как настроить список запрещенных, который блокирует домен live.com.

```powershell 
$policyValue = @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}")

New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
```

Далее приведен идентичный пример, но со встроенным определением политики.

```powershell  
New-AzureADPolicy -Definition @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}") -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
```

Чтобы настроить политику разрешенных или запрещенных, выполните командлет [Set-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview). Пример:

```powershell   
Set-AzureADPolicy -Definition $policyValue -Id $currentpolicy.Id 
```

Чтобы получить политику, выполните командлет [Get-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview). Пример:

```powershell
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy'} | select -First 1 
```

Чтобы удалить политику, выполните командлет [Remove-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview). Пример:

```powershell
Remove-AzureADPolicy -Id $currentpolicy.Id 
```

## <a name="next-steps"></a>Дальнейшие действия

- Общие сведения об Azure AD B2B см. в статье [Что такое служба совместной работы Azure AD B2B](what-is-b2b.md).
- Для получения информации об условном доступе и сотрудничестве B2B [см.](conditional-access.md)



