---
title: Управление учетными записями запуска от имени службы автоматизации Azure
description: В этой статье объясняется, как управлять учетными записями запуска от имени с помощью PowerShell или портала.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/21/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3afe27bf71d112b53c31ab696f71d4e1a0cf6b79
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002504"
---
# <a name="manage-azure-automation-run-as-accounts"></a>Управление учетными записями запуска от имени службы автоматизации Azure

Учетные записи запуска от имени в службе автоматизации Azure позволяют выполнять проверку подлинности для управления ресурсами в Azure с помощью командлетов Azure.

При создании учетной записи запуска от имени в Azure Active Directory создается пользователь субъекта-службы и ему назначается роль участника на уровне подписки. Для модулей runbook, использующих гибридные рабочие роли Runbook на виртуальных машинах Azure, можно использовать [управляемые удостоверения для ресурсов Azure](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) вместо учетной записи запуска от имени для аутентификации при получении доступа к ресурсам Azure.

Есть два типа учетных записей запуска от имени:

* **Azure Run As Account** -эта учетная запись используется для управления [модели развертывания Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md) ресурсы.
  * Создается приложение Azure AD с самозаверяющим сертификатом, учетная запись субъекта-службы для этого приложения в Azure AD, а также назначается роль участника для учетной записи в текущей подписке. Вместо этой роли можно использовать роль владельца или любую другую роль. Дополнительные сведения см. в статье [Управление доступом на основе ролей в службе автоматизации Azure](automation-role-based-access-control.md).
  * Ресурс сертификатов службы автоматизации с именем *AzureRunAsCertificate* в указанной учетной записи службы автоматизации. Этот ресурс содержит закрытый ключ сертификата, используемый в приложении Azure AD.
  * Ресурс подключений службы автоматизации с именем *AzureRunAsConnection* в указанной учетной записи службы автоматизации. Этот ресурс содержит идентификаторы приложения, клиента и подписки, а также отпечаток сертификата.

* **Azure классической учетной записи запуска от** -эта учетная запись используется для управления [классической модели развертывания](../azure-resource-manager/resource-manager-deployment-model.md) ресурсы.
  * Создает сертификат управления в подписке
  * Ресурс сертификатов службы автоматизации с именем *AzureClassicRunAsCertificate* в указанной учетной записи службы автоматизации. Этот ресурс содержит закрытый ключ сертификата, используемый в сертификате управления.
  * Ресурс подключений службы автоматизации с именем *AzureClassicRunAsConnection* в указанной учетной записи службы автоматизации. Этот ресурс содержит имя подписки, идентификатор подписки и имя ресурса сертификатов.
  * Должно быть соадминистратора для подписки для создания или обновления
  
  > [!NOTE]
  > Подписки поставщика облачных решений Azure (Azure CSP) поддерживают только модель Azure Resource Manager; в этой программе отсутствуют все службы, созданные с помощью других моделей. При использовании подписки CSP учетная запись запуска от имени Azure не создается. Учетная запись запуска от имени Azure по-прежнему создается. Дополнительные сведения о подписках CSP см. в разделе [Доступные службы в подписках CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services#comments).

  > [!NOTE]
  > Субъект-службу для запуска от имени учетной записи нет разрешения на чтение Azure Active Directory по умолчанию. Если вы хотите добавить разрешения на чтение или управлять Azure Active directory, нужно будет предоставлять ему такое разрешение участника в группе службы **разрешения API**. Дополнительные сведения см. в разделе [добавить разрешения для доступа к веб-API](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="permissions"></a>Разрешения для настройки учетных записей запуска от имени

Чтобы создать или обновить учетную запись запуска от имени, необходимо иметь привилегии и разрешения. Все задачи можно выполнить глобальным администратором в Azure Active Directory и владельца в подписке. В следующей таблице показаны списки задач, эквивалентный командлет и необходимые разрешения для ситуаций, когда присутствует разделение обязанностей:

|Задача|Командлет  |Минимальные разрешения  |Где необходимо установить разрешения|
|---|---------|---------|---|
|Создание приложения Azure AD|[New-AzureRmADApplication](/powershell/module/azurerm.resources/new-azurermadapplication)     | Роль разработчика приложения<sup>1</sup>        |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>"Начальная страница > Azure Active Directory > Регистрация приложений" |
|Добавление учетных данных приложения|[New-AzureRmADAppCredential](/powershell/module/AzureRM.Resources/New-AzureRmADAppCredential)     | Администратор приложения или глобальный администратор<sup>1</sup>         |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>"Начальная страница > Azure Active Directory > Регистрация приложений"|
|Создание или получение субъекта-службы AAD|[New-AzureRMADServicePrincipal](/powershell/module/AzureRM.Resources/New-AzureRmADServicePrincipal)</br>[Get-AzureRmADServicePrincipal](/powershell/module/AzureRM.Resources/Get-AzureRmADServicePrincipal)     | Администратор приложения или глобальный администратор<sup>1</sup>        |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>"Начальная страница > Azure Active Directory > Регистрация приложений"|
|Назначение или получение роли RBAC для указанного субъекта|[New-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment)</br>[Get-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/Get-AzureRmRoleAssignment)      | Необходимо иметь следующие разрешения:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br>Или быть:</br></br>Администратор доступа пользователей или владелец        | [Подписка](../role-based-access-control/role-assignments-portal.md)</br>"Начальная страница > Подписки > \<имя подписки\> — Управление доступом (IAM)"|
|Создание или удаление сертификата службы автоматизации|[New-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/New-AzureRmAutomationCertificate)</br>[Remove-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationCertificate)     | Участник группы ресурсов         |Группа ресурсов учетной записи службы автоматизации|
|Создание или удаление подключения службы автоматизации|[New-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/New-AzureRmAutomationConnection)</br>[Remove-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationConnection)|Участник группы ресурсов |Группа ресурсов учетной записи службы автоматизации|

<sup>1</sup> Пользователи без прав администратора в клиенте Azure Active Directory могут [регистрировать приложения домена приложения](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions), если в нем для параметра **Пользователи могут регистрировать приложения** на странице **параметров пользователя** установлено значение **Да**. Если задано значение параметра регистрации приложений **нет**, пользователь, выполняющий это действие должно быть **глобального администратора** в Azure AD.

Если вы не являетесь членом экземпляра подписки Active Directory, прежде чем вы будете добавлены **глобального администратора** роли подписки, вы будете добавлены как Гость. В этом случае вы получите предупреждение `You do not have permissions to create…` на странице **добавления учетной записи службы автоматизации**. Пользователи, добавленные к **глобального администратора** роль сначала может быть удален из экземпляра подписки Active Directory и повторно добавить, чтобы предоставить им полный в Active Directory. Чтобы проверить это, на портале Azure в области **Azure Active Directory** выберите **Пользователи и группы** и **Все пользователи**. Выбрав нужного пользователя, щелкните **Профиль**. Значение атрибута **Тип пользователя** в профиле пользователя не должно соответствовать значению **Гость**.

## <a name="permissions-classic"></a>Разрешения для настройки классического запуска от имени учетных записей

Чтобы настроить или обновить классической запуска от имени учетных записей, необходимо иметь **соадминистратора** роли на уровне подписки. Дополнительные сведения о классическом разрешения, см. в разделе [Azure классические Администраторы подписки](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

## <a name="create-a-run-as-account-in-the-portal"></a>Создание учетной записи запуска от имени на портале

В этом разделе указаны действия, с помощью которых на портале Azure можно обновить учетную запись службы автоматизации Azure. Учетные записи запуска от имени и классические учетные записи запуска от имени создаются отдельно. Если вам не нужно управлять классическими ресурсами, достаточно создать учетную запись запуска от имени Azure.  

1. Войдите на портал Azure с помощью учетной записи, которая является участником роли "Администраторы подписки" и соадминистратором подписки.
2. На портале Azure щелкните **Все службы**. В списке ресурсов выберите **Автоматизация**. Как только вы начнете вводить символы, список отфильтруется соответствующим образом. Выберите элемент **Учетные записи службы автоматизации**.
3. На странице **учетных записей службы автоматизации** выберите в списке свою учетную запись.
4. В области слева выберите **Учетные записи запуска от имени** в разделе **Настройки учетной записи**.  
5. В зависимости от того, какую учетную запись необходимо создать, выберите **Учетная запись запуска от имени Azure** или **Классическая учетная запись запуска от имени Azure**. После выбора отобразится панель **Добавить учетную запись запуска от имени Azure** или **Добавить классическую учетную запись запуска от имени Azure**. Просмотрев общие сведения, нажмите кнопку **Создать**, чтобы продолжить создание учетной записи запуска от имени.  
6. Ход создания учетной записи запуска от имени в Azure можно отслеживать в разделе **Уведомления** в меню. Отображается также баннер с уведомлением о создании учетной записи. Процесс создания может занять несколько минут.  

## <a name="create-run-as-account-using-powershell"></a>Создание учетной записи запуска от имени с помощью PowerShell

## <a name="prerequisites"></a>Технические условия

В следующем списке приведены требования для создания учетной записи запуска от имени в PowerShell:

* Windows 10 или Windows Server 2016 с модулями Azure Resource Manager версии 3.4.1 и выше. Скрипт PowerShell не поддерживает более ранние версии Windows.
* Azure PowerShell 1.0 или более поздней версии. Сведения о выпуске PowerShell 1.0 см. в статье [Приступая к работе с командлетами Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Учетная запись службы автоматизации Azure, указанная как значение для параметров *-AutomationAccountName* и *-ApplicationDisplayName*.
* Разрешения, аналогичные списку в разделе [разрешений для настройки учетной записи запуска от имени](#permissions).

Чтобы получить значения для параметров *SubscriptionID*, *ResourceGroup* и *AutomationAccountName*, которые являются обязательными для скрипта, выполните следующие шаги:

1. На портале Azure щелкните **Все службы**. В списке ресурсов выберите **Автоматизация**. Как только вы начнете вводить символы, список отфильтруется соответствующим образом. Выберите элемент **Учетные записи службы автоматизации**.
1. На странице учетных записей службы автоматизации выберите нужную учетную запись, а затем в разделе **Настройки учетной записи** выберите пункт **Свойства**.  
1. Запишите значения **идентификатора подписки**, **имени** и **группы ресурсов** на странице **свойств**.

   ![Страница "Свойства" учетной записи службы автоматизации](media/manage-runas-account/automation-account-properties.png)

Этот сценарий PowerShell включает в себя поддержку следующих конфигураций:

* создание учетной записи запуска от имени с использованием самозаверяющего сертификата;
* создание учетной записи запуска от имени и классической учетной записи запуска от имени с использованием самозаверяющего сертификата;
* Создайте учетную запись запуска от имени и классическую учетную запись запуска от имени, используя сертификат, выданный центром сертификации предприятия.
* создание учетной записи запуска от имени и классической учетной записи запуска от имени с использованием самозаверяющего сертификата в облаке Azure для государственных организаций.

>[!NOTE]
> Если вы выбрали создание классической учетной записи запуска от имени, после выполнения сценария отправьте открытый сертификат (файл в формате CER) в хранилище управления подписки, в которой создана учетная запись службы автоматизации.

1. Сохраните приведенный ниже сценарий на компьютере. В этом примере используйте имя файла *New-RunAsAccount.ps1*.

   Для создания ресурсов сценарий использует несколько командлетов Azure Resource Manager. В следующей таблице показаны командлеты и необходимые разрешения.

    ```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory = $false)]
        [string] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureCloud", "AzureUSGovernment")]
        [string]$EnvironmentName = "AzureCloud",

        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )

    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }

    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId) 
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzureRmADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }

    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
    }

    function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
    }

    Import-Module AzureRM.Profile
    Import-Module AzureRM.Resources

    $AzureRMProfileVersion = (Get-Module AzureRM.Profile).Version
    if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 4) -or ($AzureRMProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }

    # To use the new Az modules to create your Run As accounts please uncomment the following lines and ensure you comment out the previous 8 lines that import the AzureRM modules to avoid any issues. To learn about about using Az modules in your Automation Account see https://docs.microsoft.com/azure/automation/az-modules

    # Import-Module Az.Automation
    # Enable-AzureRmAlias


    Connect-AzureRmAccount -Environment $EnvironmentName 
    $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"

    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }

    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName

    # Create the Automation certificate asset
    CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

    # Populate the ConnectionFieldValues
    $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1
    $Thumbprint = $PfxCert.Thumbprint
    $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

    # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

    if ($CreateClassicRunAsAccount) {
        # Create a Run As account by using a service principal
        $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
        $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
        $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
        $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
        "Log in to the Microsoft Azure portal (https://portal.azure.com) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"

        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red       $UploadMessage
    }
    ```

    > [!IMPORTANT]
    > **Add-AzureRmAccount** теперь является псевдонимом для **Connect-AzureRMAccount**. Если при поиске элементов библиотеки вы не видите командлет **Connect-AzureRMAccount**, можно использовать командлет **Add-AzureRmAccount** или [обновить модули](automation-update-azure-modules.md) в своей учетной записи службы автоматизации.

1. На компьютере запустите с повышенными правами **Windows PowerShell** с **начального** экрана.
1. Из оболочки командной строки с повышенными привилегиями перейдите в папку, которая содержит сценарий, созданный на этапе 1.  
1. Выполните этот сценарий, установив значения параметров в зависимости от требуемой конфигурации.

    **Создание учетной записи запуска от имени с использованием самозаверяющего сертификата**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
    ```

    **Создание учетной записи запуска от имени и классической учетной записи запуска от имени с использованием самозаверяющего сертификата**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
    ```

    **Создание учетной записи запуска от имени и классической учетной записи запуска от имени Azure с использованием корпоративного сертификата**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
    ```

    **Создание учетной записи запуска от имени и классической учетной записи запуска от имени с использованием самозаверяющего сертификата в облаке Azure для государственных организаций**
  
    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment
    ```

    > [!NOTE]
    > После выполнения сценария появится запрос на проверку подлинности в Azure. Войдите в систему, используя учетную запись, которая является участником роли "Администраторы подписки" и соадминистратором подписки.

После выполнения сценария обратите внимание на следующее.

* Если вы создали классическую учетную запись запуска от имени с использованием самозаверяющего открытого сертификата (CER-файл), сценарий создает ее и сохраняет в папке временных файлов на компьютере в профиле пользователя, который выполнял сеанс PowerShell: *%Профиль_пользователя%\AppData\Local\Temp*.

* Если вы создали классическую учетную запись запуска от имени с использованием открытого корпоративного сертификата (CER-файл), используйте этот сертификат. Следуйте инструкциям для [загрузки сертификата API управления на портал Azure](../azure-api-management-certs.md).

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Удаление учетной записи запуска от имени или классической учетной записи запуска от имени

В этом разделе описано, как удалить и повторно создать учетную запись запуска от имени или классическую учетную запись запуска от имени. При выполнении этого действия учетная запись службы автоматизации сохраняется. После удаления учетной записи запуска от имени или классической учетной записи запуска от имени ее можно создать заново на портале Azure.

1. На портале Azure откройте учетную запись службы автоматизации.

2. На странице **Учетная запись автоматизации** выберите **Run As Accounts** (Учетные записи запуска от имени).

3. На странице свойств **Run As Accounts** (Учетные записи запуска от имени) выберите учетную запись запуска от имени или классическую учетную запись запуска от имени, которую нужно удалить. Затем в области **Свойства** выбранной учетной записи щелкните **Удалить**.

   ![Удаление учетной записи запуска от имени](media/manage-runas-account/automation-account-delete-runas.png)

1. Ход удаления учетной записи можно отслеживать в разделе **Уведомления** в меню.

1. После удаления учетной записи ее можно повторно создать на странице свойств **Run As Accounts** (Учетные записи запуска от имени), выбрав параметр создания **Azure Run As Account** (Учетная запись запуска от имени Azure).

   ![Повторное создание учетной записи запуска от имени службы автоматизации](media/manage-runas-account/automation-account-create-runas.png)

## <a name="cert-renewal"></a>Обновление самозаверяющего сертификата

В определенный момент перед истечением срока действия учетной записи запуска от имени вам потребуется обновить сертификат. Если вы считаете, что учетная запись запуска от имени была скомпрометирована, ее можно удалить и создать заново. В этом разделе описано, как выполнить эти операции.

Срок действия самозаверяющего сертификата, созданного для учетной записи запуска от имени, составляет один год с момента создания. Его можно обновить в любое время до истечения его срока действия. При обновлении текущее значение сертификата сохраняется, чтобы гарантировать, что все модули runbook, которые поставлены в очередь или активно выполняются и для проверки подлинности которых используется учетная запись запуска от имени, не будут затронуты. Сертификат будет существовать до окончания срока действия.

> [!NOTE]
> Если вы настроили учетную запись запуска от имени службы автоматизации так, чтобы использовать сертификат, выданный центром сертификации предприятия, и используете этот параметр, то этот сертификат предприятия будет заменен самозаверяющим сертификатом.

Чтобы обновить сертификат, сделайте следующее:

1. На портале Azure откройте учетную запись службы автоматизации.

1. В разделе **параметров учетной записи** выберите **учетные записи запуска от имени**.

    ![Область свойств учетной записи службы автоматизации](media/manage-runas-account/automation-account-properties-pane.png)

1. На странице свойств **Run As Accounts** (Учетные записи запуска от имени) выберите учетную запись запуска от имени или классическую учетную запись запуска от имени, для которой нужно обновить сертификат.

1. В области **Свойства** выбранной учетной записи щелкните **Обновление сертификата**.

    ![Обновление сертификата для учетной записи запуска от имени](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Ход обновления сертификата можно отслеживать в разделе **Уведомления** в меню.

## <a name="limiting-run-as-account-permissions"></a>Ограничение разрешений учетной записи запуска от имени

Чтобы управлять автоматическими действиями с ресурсами в службе автоматизации Azure, по умолчанию в подписке учетной записи запуска от имени предоставляются права участника. Если нужно ограничить функции субъекта-службы RunAs, можно удалить учетную запись из роли участника для подписки и добавить ее в качестве участника в нужную группу ресурсов.

На портале Azure щелкните **Подписки** и выберите подписку учетной записи службы автоматизации. Выберите **Управление доступом (IAM)**, а затем вкладку **Назначение ролей**. Выполните поиск субъекта-службы для учетной записи службы автоматизации (он выглядит так: \<AutomationAccountName\>_уникальный идентификатор). Чтобы удалить учетную запись из подписки, выберите ее и нажмите кнопку **Удалить**.

![Участники подписки](media/manage-runas-account/automation-account-remove-subscription.png)

Чтобы добавить субъект-службу в группу ресурсов, на портале Azure выберите группу ресурсов и щелкните **Управление доступом (IAM)**. Выберите **Добавить назначение ролей**, чтобы открыть страницу **Добавление назначения ролей**. В поле **Роль** выберите **Участник**. В текстовое поле **Выбор** введите имя субъекта-службы для учетной записи запуска от имени и выберите субъект-службу в списке. Щелкните **Сохранить** , чтобы сохранить изменения. Выполните эти шаги для групп ресурсов, к которым нужно предоставить доступ субъекту-службе запуска от имени службы автоматизации Azure.

## <a name="misconfiguration"></a>Неправильные настройки

Во время начальной настройки некоторые из элементов конфигурации, необходимых для правильной работы учетной записи запуска от имени или классической учетной записи запуска от имени, могут быть удалены или неправильно созданы. К этим элементам относятся:

* ресурс сертификата,
* ресурс подключения,
* учетная запись запуска от имени (удалена из роли участника),
* субъект-служба или приложение-служба в Azure AD,

В случае такой неправильной настройки (или в других примерах) учетная запись службы автоматизации обнаружит эти изменения и отобразит в области свойств **учетных записей запуска от имени** состояние *Не выполнено*.

![Сообщение о том, что настройка учетной записи запуска от имени не завершена](media/manage-runas-account/automation-account-runas-incomplete-config.png)

При выборе учетной записи запуска от имени в области **Свойства** учетной записи отобразится следующее сообщение об ошибке:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Эти проблемы с учетной записью запуска от имени можно быстро устранить, удалив и повторно создав ее.

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о субъектах-службах см. в статье [Объекты приложения и субъекта-службы в Azure Active Directory (Azure AD)](../active-directory/develop/app-objects-and-service-principals.md).
* Дополнительные сведения о сертификатах и службах Azure см. в статье [Общие сведения о сертификатах для облачных служб Azure](../cloud-services/cloud-services-certs-create.md).