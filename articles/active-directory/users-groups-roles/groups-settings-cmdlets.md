---
title: Настройка параметров группы с помощью PowerShell — Azure AD | Документация Майкрософт
description: Управление параметрами групп с помощью командлетов Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef0bfcb8c82d3f3caf90500e8852ca9e02c725aa
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74382972"
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Настройка параметров групп с помощью командлетов Azure Active Directory

Эта статья содержит инструкции по использованию командлетов PowerShell в Azure Active Directory (Azure AD) для создания и изменения групп. Это содержимое относится только к группам Office 365, также известным как единые группы.

> [!IMPORTANT]
> Для доступа к некоторым параметрам требуется лицензия Azure Active Directory Premium P1. Дополнительные сведения см. [здесь](#template-settings).

Чтобы узнать, как запретить пользователям без прав администратора создавать группы безопасности, установите  `Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False`, как описано в статье [Set-MSOLCompanySettings](https://docs.microsoft.com/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

Параметры групп Office 365 настраиваются с помощью объектов Settings и SettingsTemplate. Изначально в каталоге не отображаются все объекты параметров, так как он настроен с параметрами по умолчанию. Чтобы изменить параметры по умолчанию, необходимо с помощью шаблона параметров создать объект параметров. Шаблоны параметров определены корпорацией Майкрософт. Поддерживается несколько разных шаблонов параметров. Чтобы настроить параметры группы Office 365 для каталога, используйте шаблон Group.Unified. Для настройки параметров отдельной группы Office 365 используйте шаблон Group.Unified.Guest. Этот шаблон используется для управления гостевым доступом к группе Office 365. 

Командлеты являются частью модуля PowerShell версии 2 для Azure Active Directory. Дополнительные сведения о скачивании и установке модуля на компьютер см. в статье [PowerShell версии 2 для Azure Active Directory](https://docs.microsoft.com/powershell/azuread/). Вы можете установить выпуск версии 2 модуля [из коллекции PowerShell](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="install-powershell-cmdlets"></a>Установка командлетов PowerShell

Обязательно удалите все старые версии Azure Active Directory PowerShell для модуля Graph для Windows PowerShell и установите [общедоступную предварительную версию Azure Active Directory PowerShell для Graph 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137), прежде чем выполнять команды PowerShell.

1. Запустите приложение Windows PowerShell от имени администратора.
2. Удалите все предыдущие версии AzureADPreview.
  
   ``` PowerShell
   Uninstall-Module AzureADPreview
   Uninstall-Module azuread
   ```

3. Установите последнюю версию AzureADPreview.
  
   ``` PowerShell
   Install-Module AzureADPreview
   ```
   
## <a name="create-settings-at-the-directory-level"></a>Создание параметров на уровне каталога
Далее приведены действия, необходимые для создания параметров на уровне каталога, применимые ко всем группам Office 365 в каталоге. Командлет Get-AzureADDirectorySettingTemplate доступен только в [модуле предварительной версии Azure AD PowerShell для Graph](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

1. В командлетах DirectorySettings необходимо указать идентификатор объекта SettingsTemplate, который вы хотите использовать. Если он вам неизвестен, выполните приведенный ниже командлет, чтобы отобразить все шаблоны параметров.
  
   ```powershell
   Get-AzureADDirectorySettingTemplate

   ```
   Этот командлет отображает все шаблоны, которые доступны.
  
   ```powershell
   Id                                   DisplayName         Description
   --                                   -----------         -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Office 365 group
   16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
   ```
2. Чтобы добавить URL-адрес правил использования, сначала необходимо получить объект SettingsTemplate, который определяет значение URL-адреса правил использования; это — шаблон Group.Unified:
  
   ```powershell
   $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
   ```
3. Затем создайте новый объект параметров на основе этого шаблона:
  
   ```powershell
   $Setting = $template.CreateDirectorySetting()
   ```  
4. Затем обновите значение правил использования:
  
   ```powershell
   $Setting["UsageGuidelinesUrl"] = "https://guideline.example.com"
   ```  
5. Затем примените параметр:
  
   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
6. Значения можно считывать с помощью:

   ```powershell
   $Setting.Values
   ```  
## <a name="update-settings-at-the-directory-level"></a>Обновление параметров на уровне каталога
Чтобы обновить значение параметра UsageGuideLinesUrl в шаблоне параметров, просто измените URL-адрес на шаге 4 выше, а затем выполните шаг 5, чтобы задать новое значение.

Чтобы удалить значение UsageGuideLinesUrl, измените URL-адрес на пустую строку, выполнив шаг 4 выше.

   ```powershell
   $Setting["UsageGuidelinesUrl"] = ""
   ```  
Затем выполните шаг 5, чтобы задать новое значение.

## <a name="template-settings"></a>Параметры шаблона
Ниже приведены параметры, определенные в объекте SettingsTemplate шаблона Group.Unified. Если не указано иное, для доступа к этим параметрам требуется лицензия Azure Active Directory Premium P1. 

| **Параметр** | **Описание** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Тип: логический<li>Default: True |Флаг, указывающий, разрешено ли пользователям без прав администратора создание группы Office 365 в каталоге. Для этого параметра не требуется лицензия Azure Active Directory Premium P1.|
|  <ul><li>GroupCreationAllowedGroupId<li>Тип: строка<li>Default: “” |Идентификатор GUID группы безопасности, участникам которой разрешено создавать группы Office 365, даже когда EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Тип: строка<li>Default: “” |Ссылка на правила использования группы. |
|  <ul><li>ClassificationDescriptions<li>Тип: строка<li>Default: “” | Разделенный запятыми список описаний классификаций. Значение ClassificationDescriptions допустимо только в следующем формате:<br>$setting ["Классификатиондескриптионс"] = "Классификация: описание, классификация: описание"<br>где классификация соответствует строкам в Классификатионлист.<br>Этот параметр не применяется, если Енаблемиплабелс = = true.|
|  <ul><li>DefaultClassification<li>Тип: строка<li>Default: “” | Классификация, которая должна использоваться по умолчанию для группы, если не была указана иная классификация.<br>Этот параметр не применяется, если Енаблемиплабелс = = true.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Тип: строка<li>Default: “” | Строка с максимальной длиной 64 символа, которая определяет соглашение об именовании, настроенное для групп Office 365. Дополнительные сведения см. в статье [Принудительное применение политики именования для групп Office 365](groups-naming-policy.md). |
| <ul><li>CustomBlockedWordsList<li>Тип: строка<li>Default: “” | Строка фраз с разделителями-запятыми, которые не разрешено использовать в именах или псевдонимах групп. Дополнительные сведения см. в статье [Принудительное применение политики именования для групп Office 365](groups-naming-policy.md). |
| <ul><li>EnableMSStandardBlockedWords<li>Тип: логический<li>Значение по умолчанию: False. | Не используйте.
|  <ul><li>AllowGuestsToBeGroupOwner<li>Тип: логический<li>значение по умолчанию: False | Логическое значение, указывающее, может ли гостевой пользователь быть владельцем группы. |
|  <ul><li>AllowGuestsToAccessGroups<li>Тип: логический<li>Default: True | Логическое значение, указывающее, может ли гостевой пользователь иметь доступ к содержимому групп Office 365.  Для этого параметра не требуется лицензия Azure Active Directory Premium P1.|
|  <ul><li>GuestUsageGuidelinesUrl<li>Тип: строка<li>Default: “” | URL-адрес ссылки на правила использования гостя. |
|  <ul><li>AllowToAddGuests<li>Тип: логический<li>Default: True | Логическое значение, указывающее, разрешено ли добавлять гостей в этот каталог. <br>Этот параметр может быть переопределен и доступен только для чтения, если *енаблемиплабелс* имеет значение *true* , а гостевая политика связана с меткой конфиденциальности, назначенной группе. |
|  <ul><li>ClassificationList<li>Тип: строка<li>Default: “” |Разделенный запятыми список допустимых значений классификации, которые можно применять к группам Office 365. <br>Этот параметр не применяется, если Енаблемиплабелс = = true.|
|  <ul><li>енаблемиплабелс<li>Тип: логический<li>Значение по умолчанию: False. |Флаг, указывающий, можно ли применить метки чувствительности, опубликованные в Microsoft 365 центре соответствия требованиям, к группам Office 365. Дополнительные сведения см. в статье [Назначение меток чувствительности для групп Office 365](groups-assign-sensitivity-labels.md). |

## <a name="example-configure-guest-policy-for-groups-at-the-directory-level"></a>Пример. Настройка гостевой политики для групп на уровне каталога
1. Получите все шаблоны параметров:
   ```powershell
   Get-AzureADDirectorySettingTemplate
   ```
2. Чтобы настроить гостевую политику для групп на уровне каталога, необходим шаблон Group. Unified.
   ```powershell
   $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
   ```
3. Затем создайте новый объект параметров на основе этого шаблона:
  
   ```powershell
   $Setting = $template.CreateDirectorySetting()
   ```  
4. Затем обновите параметр Алловаддгуестс
   ```powershell
   $Setting["AllowAddGuests"] = $False
   ```  
5. Затем примените параметр:
  
   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
6. Значения можно считывать с помощью:

   ```powershell
   $Setting.Values
   ```   

## <a name="read-settings-at-the-directory-level"></a>Чтение параметров на уровне каталога

Если вам известно имя параметра, которое необходимо получить, можно использовать приведенный ниже командлет. В этом примере показано получение значения для параметра с именем UsageGuidelinesUrl. 

   ```powershell
   (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
   ```
Далее приведены действия, необходимые для чтения параметров на уровне каталога, применимые ко всем группам Office в каталоге.

1. Чтение всех существующих параметров каталога:
   ```powershell
   Get-AzureADDirectorySetting -All $True
   ```
   Этот командлет возвращает список всех параметров каталога:
   ```powershell
   Id                                   DisplayName   TemplateId                           Values
   --                                   -----------   ----------                           ------
   c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
   ```

2. Чтение всех параметров определенной группы:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
   ```

3. Считывает все значения параметров каталога определенного объекта параметров каталога, используя параметры GUID ID:
   ```powershell
   (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
   ```
   Этот командлет возвращает имена и значения в объекте Settings для этой конкретной группы:
   ```powershell
   Name                          Value
   ----                          -----
   ClassificationDescriptions
   DefaultClassification
   PrefixSuffixNamingRequirement
   CustomBlockedWordsList        
   AllowGuestsToBeGroupOwner     False 
   AllowGuestsToAccessGroups     True
   GuestUsageGuidelinesUrl
   GroupCreationAllowedGroupId
   AllowAddGuests              True
   UsageGuidelinesUrl            https://guideline.example.com
   ClassificationList
   EnableGroupCreation           True
   ```

## <a name="remove-settings-at-the-directory-level"></a>Удаление параметров на уровне каталога
Далее приведено действие, необходимое для удаления параметров на уровне каталога, применимое ко всем группам Office в каталоге.
   ```powershell
   Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
   ```

## <a name="create-settings-for-a-specific-group"></a>Создание параметров для определенной группы

1. Найдите шаблон параметров Groups.Unified.Guest.
   ```powershell
   Get-AzureADDirectorySettingTemplate
  
   Id                                   DisplayName            Description
   --                                   -----------            -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Office 365 group
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
   ```
2. Получите объект SettingTemplate для шаблона Groups.Unified.Guest.
   ```powershell
   $Template1 = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
   ```
3. Создайте объект Settings из шаблона.
   ```powershell
   $SettingCopy = $Template1.CreateDirectorySetting()
   ```

4. Установите требуемое значение для параметра.
   ```powershell
   $SettingCopy["AllowAddGuests"]=$False
   ```
5. Получите идентификатор группы, к которой нужно применить этот параметр:
   ```powershell
   $groupID= (Get-AzureADGroup -SearchString "YourGroupName").ObjectId
   ```
6. Создайте параметр для требуемой группы в каталоге.
   ```powershell
   New-AzureADObjectSetting -TargetType Groups -TargetObjectId $groupID -DirectorySetting $SettingCopy
   ```
7. Чтобы проверить параметры, выполните следующую команду:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups | fl Values
   ```

## <a name="update-settings-for-a-specific-group"></a>Обновление параметров определенной группы
1. Получите идентификатор группы, параметр которой необходимо обновить.
   ```powershell
   $groupID= (Get-AzureADGroup -SearchString "YourGroupName").ObjectId
   ```
2. Получите параметр группы:
   ```powershell
   $Setting = Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups
   ```
3. Обновите настройку группы по мере необходимости, например
   ```powershell
   $Setting["AllowAddGuests"] = $True
   ```
4. Затем получите идентификатор параметра для этой конкретной группы:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups
   ```
   Вы получите ответ, аналогичный следующему:
   ```powershell
   Id                                   DisplayName            TemplateId                             Values
   --                                   -----------            -----------                            ----------
   2dbee4ca-c3b6-4f0d-9610-d15569639e1a Group.Unified.Guest    08d542b9-071f-4e16-94b0-74abb372e3d9   {class SettingValue {...
   ```
5. Затем можно задать новое значение для этого параметра:
   ```powershell
   Set-AzureADObjectSetting -TargetType Groups -TargetObjectId $groupID -Id 2dbee4ca-c3b6-4f0d-9610-d15569639e1a -DirectorySetting $Setting
   ```
6. Можно прочитать значение параметра, чтобы убедиться, что оно правильно Обновлено:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups | fl Values
   ```

## <a name="cmdlet-syntax-reference"></a>Справочник по синтаксису командлетов
Дополнительную документацию по PowerShell Azure Active Directory см. в разделе [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0) (Командлеты Azure Active Directory).

## <a name="additional-reading"></a>Дополнительные материалы

* [Управление доступом к ресурсам с помощью групп Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Интеграция локальных удостоверений с Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
