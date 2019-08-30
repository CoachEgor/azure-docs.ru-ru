---
title: Выполнение модулей runbook в гибридной рабочей роли Runbook в службе автоматизации Azure
description: Эта статья содержит сведения о выполнении модулей runbook на компьютерах в локальном центре обработки данных или поставщике облачных решений с помощью гибридной рабочей роли Runbook.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 01/29/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 97d900146b3485df5cdf226f07aaa3aee116fc92
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186767"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Запуск модулей runbook в гибридной рабочей роли Runbook

В структуре модулей Runbook, которые работают в службе автоматизации Azure, отсутствует разница между ними и теми модулями, которые работают в гибридной рабочей роли Runbook. Модули Runbook, которые используются с каждым из этих элементов, скорее всего, значительно отличаются друг от друга. Эта разница связана с тем, что модули Runbook, предназначенные для гибридной рабочей роли Runbook, обычно управляют ресурсами на самом локальном компьютере или ресурсами в локальной среде, где она развернута. Модули Runbook в службе автоматизации Azure обычно управляют ресурсами в облаке Azure.

Когда вы создаете модули runbook для запуска на гибридной рабочей роли Runbook, настройте и протестируйте их выполнение на компьютере, где размещена эта гибридная рабочая роль. Этот компьютер имеет все модули PowerShell и доступ к сети, необходимые для доступа к локальным ресурсам и управления ими. Протестировав модуль runbook на компьютере с гибридной рабочей ролью, вы можете передать его в среду службы автоматизации Azure, где он будет доступен для запуска в гибридной рабочей роли. Важно понимать задания, выполняемые в локальной системной учетной записи для Windows или специальной учетной записи пользователя `nxautomation` в Linux. В Linux это означает, что `nxautomation` учетная запись должна иметь доступ к расположению, где хранятся модули. При использовании командлета [Install-Module](/powershell/module/powershellget/install-module) укажите `naxautomation` **ALLUSERS** в `-Scope` параметре, чтобы убедиться, что у учетной записи есть доступ.

Дополнительные сведения о PowerShell в Linux см. в статье [Известные проблемы для PowerShell на платформах, отличных от Windows](https://docs.microsoft.com/powershell/scripting/whats-new/known-issues-ps6?view=powershell-6#known-issues-for-powershell-on-non-windows-platforms).

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Запуск runbook в гибридной рабочей роли Runbook

[Запуск модуля Runbook в службе автоматизации Azure](automation-starting-a-runbook.md) описываются различные методы запуска модуля Runbook. Гибридный компонент Runbook Worker добавляет параметр **RunOn** , при помощи которого можно указать имя группы гибридных компонентов Runbook Worker. Если группа указана, модуль Runbook извлекается и запускается одной из рабочих ролей в этой группе. Если этот параметр не указан, то он будет запущен в службе автоматизации Azure в обычном режиме.

При запуске модуля Runbook на портале Azure отображается запрос **Выполнить в**, где вы можете выбрать в качестве значения варианты **Azure** или **Гибридная рабочая роль**. При выборе значения **Гибридный компонент Worker**можно выбрать группу из раскрывающегося списка.

Используйте параметр **RunOn**. Вы можете использовать следующую команду, чтобы с помощью Windows PowerShell запустить модуль runbook с названием Test-Runbook в группе гибридных рабочих ролей Runbook с именем MyHybridGroup.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> Параметр **RunOn** был добавлен в командлет **Start-AzureAutomationRunbook** в Microsoft Azure PowerShell версии 0.9.1. Если у вас установлена более ранняя версия, следует [загрузить последнюю версию](https://azure.microsoft.com/downloads/) . Эту версию достаточно установить только на той рабочей станции, на которой вы будете запускать модуль runbook с помощью PowerShell. Вам не нужно устанавливать его на компьютер с компонентом Worker, если вы не собираетесь запускать модули Runbooks с этого компьютера.

## <a name="runbook-permissions"></a>Разрешения для модулей Runbook

Так как модули Runbook, запущенные в гибридной рабочей роли Runbook, будут обращаться к ресурсам за пределами Azure, они не смогут использовать стандартный метод аутентификации в Azure. Модуль Runbook может предоставить собственную аутентификацию для локальных ресурсов или настроить ее с помощью [управляемых удостоверений для ресурсов Azure](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager
). Или вы можете указать учетную запись запуска от имени, чтобы предоставить контекст пользователя для всех модулей Runbook.

### <a name="runbook-authentication"></a>Проверка подлинности модуля Runbook

По умолчанию модули Runbook работают в контексте учетной записи локальной системы для Windows и специальной учетной записи пользователя `nxautomation` для Linux на локальном компьютере, поэтому они должны предоставить свою собственную проверку подлинности при доступе к ресурсам.

В модуле Runbook можно использовать ресурсы [учетных данных](automation-credentials.md) и [сертификата](automation-certificates.md) с командлетами, которые позволяют указывать такие данные для аутентификации доступа к разным ресурсам. В следующем примере показана часть модуля Runbook, предназначенная для перезапуска компьютера. Он извлекает учетные данные из набора учетных данных, а также имя компьютера из набора переменных, после чего использует эти значения в сочетании с командлетом Restart-Computer.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

Вы можете также использовать [InlineScript](automation-powershell-workflow.md#inlinescript), что позволяет запускать блоки кода на другом компьютере с учетными данными, указанными в [общем параметре PSCredential](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="runas-account"></a>Учетная запись запуска от имени

По умолчанию гибридная рабочая роль Runbook использует локальную систему для Windows и специальную учетную запись пользователя `nxautomation` для Linux для выполнения модулей Runbook. Кроме варианта, когда модули Runbook выполняют собственную проверку подлинности на локальных ресурсах, вы можете указать для группы гибридных рабочих ролей учетную запись **Запуск от имени** . Укажите [ресурс учетных данных](automation-credentials.md) , который имеет доступ к локальным ресурсам, включая хранилища сертификатов, и все модули Runbook выполняются под этими учетными данными при запуске в гибридной рабочей роли Runbook в группе.

Имя пользователя для учетных данных должно быть представлено в одном из следующих форматов:

* домен\имя пользователя;
* username@domain
* имя пользователя (для локальных учетных записей на локальном компьютере).

Указать учетную запись запуска от имени для гибридной рабочей роли можно так.

1. Создайте [ресурс учетных данных](automation-credentials.md) с доступом к локальным ресурсам.
2. На портале Azure откройте учетную запись службы автоматизации.
3. Щелкните плитку **Группы гибридных рабочих ролей** и выберите группу.
4. Щелкните **Все параметры**, а затем — **Настройки группы гибридных рабочих ролей**.
5. Измените для параметра **Запуск от имени** значение **По умолчанию** на **Пользовательский**.
6. Выберите учетные данные и нажмите кнопку **Сохранить**.

### <a name="managed-identities-for-azure-resources"></a>Управляемые удостоверения для ресурсов Azure

Гибридные рабочие роли Runbook, запущенные на виртуальных машинах Azure, используют управляемые удостоверения для аутентификации ресурсов Azure. Есть множество преимуществ использования управляемых удостоверений вместо учетных записей запуска от имени:

* не нужно экспортировать сертификат запуска от имени, а затем импортировать его в гибридную рабочую роль Runbook;
* не нужно обновлять сертификат учетной записи запуска от имени;
* не нужно обрабатывать объект подключения запуска от имени в коде Runbook.

Чтобы использовать управляемое удостоверение для ресурсов Azure в гибридной рабочей роли Runbook, выполните следующие действия.

1. Создание виртуальной машины Azure
2. [Настройте управляемые удостоверения для ресурсов Azure на вашей виртуальной машине](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm).
3. [Предоставьте виртуальной машине доступ к группе ресурсов в Resource Manager](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager).
4. [Получите маркер доступа, используя назначенное системой управляемое удостоверение виртуальной машины](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#get-an-access-token-using-the-vms-system-assigned-managed-identity-and-use-it-to-call-azure-resource-manager).
5. [Установите гибридную рабочую роль Runbook Windows](automation-windows-hrw-install.md#installing-the-windows-hybrid-runbook-worker) на виртуальную машину.

По завершении предыдущих этапов можно использовать `Connect-AzureRmAccount -Identity` в модуле Runbook для аутентификации ресурсов Azure. Эта конфигурация снижает потребность использовать учетную запись запуска от имени и управлять ее сертификатом.

```powershell
# Connect to Azure using the Managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
Connect-AzureRmAccount -Identity

# Get all VM names from the subscription
Get-AzureRmVm | Select Name
```

### <a name="runas-script"></a>Учетная запись запуска от имени службы автоматизации

В ходе автоматизированного процесса сборки для развертывания ресурсов в Azure вам может потребоваться доступ к локальным системам для выполнения задачи или набора действий, входящих в последовательность развертывания. Чтобы использовать аутентификацию в Azure с использованием учетной записи запуска от имени, необходимо установить сертификат учетной записи запуска от имени.

Модуль runbook PowerShell с именем **Export-RunAsCertificateToHybridWorker** экспортирует сертификат запуска от имени из учетной записи службы автоматизации Azure, а затем скачивает его и импортирует в хранилище сертификатов локального компьютера, относящегося к гибридной рабочей роли, подключенной к этой учетной записи. После этого модуль проверяет, может ли рабочая роль успешно выполнить аутентификацию в Azure с использованием этой учетной записи запуска от имени.

```azurepowershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
Run this runbook in the hybrid worker where you want the certificate installed.
This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
AUTHOR: Azure Automation Team
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test that authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm Azure Resource Manager calls are working
Get-AzureRmAutomationAccount | Select-Object AutomationAccountName
```

> [!IMPORTANT]
> **Add-AzureRmAccount** теперь является псевдонимом для **Connect-AzureRMAccount**. Если при поиске в библиотеке элементов вы не видите элемент **Connect-AzureRMAccount**, можно использовать **Add-AzureRmAccount** или обновить модули в своей учетной записи службы автоматизации.

Сохраните модуль runbook *Export-RunAsCertificateToHybridWorker* на компьютер с расширением `.ps1`. Импортируйте модуль runbook в учетную запись автоматизации и замените значение переменной `$Password` своим паролем. Опубликуйте и запустите модуль Runbook. Нацельтесь на группу гибридных рабочих ролей, в которой для запуска и аутентификации модулей runbook будет применяться учетная запись запуска от имени. Поток заданий сообщает о попытке импортировать сертификат в хранилище локального компьютера и предоставляет несколько строк. Это поведение зависит от количества учетных записей службы автоматизации, определенных в вашей подписке, и успешного прохождения проверки подлинности.

## <a name="job-behavior"></a>Поведение заданий

Задания обрабатываются в гибридных рабочих ролях Runbook немного иначе, чем при запуске в песочницах Azure. Одно из основных отличий заключается в том, что в гибридных рабочих ролях Runbook нет ограничений на продолжительность заданий. Модули runbook, выполняющиеся в песочницах Azure, ограничены тремя часами по причине [справедливого распределения](automation-runbook-execution.md#fair-share). Убедитесь, что длительный модуль runbook устойчивый к возможному перезапуску. Например, если компьютер, на котором размещена гибридная рабочая роль, будет перезагружен. При перезагрузке компьютера, выполняющего гибридную рабочую роль, все выполняемые задания runbook запускаются заново, с самого начала или с последней контрольной точки, если это runbook рабочих процессов PowerShell. Если задание runbook перезапускается более трех раз, его выполнение приостанавливается.

## <a name="run-only-signed-runbooks"></a>Запуск только подписанных модулей Runbook

Гибридные рабочие роли Runbook можно настроить для запуска только подписанных модулей Runbook с определенной конфигурацией. В следующем разделе описывается, как настроить гибридные рабочие роли Runbook для запуска подписанных заданий гибридных рабочих ролей Runbook [Windows](#windows-hybrid-runbook-worker) и [Linux](#linux-hybrid-runbook-worker).

> [!NOTE]
> Настроенные гибридные рабочие роли Runbook запускаются, только когда модули Runbook подписаны, а модули Runbook, которые **не** были подписаны, не будут выполнены в рабочей роли.

### <a name="windows-hybrid-runbook-worker"></a>Гибридная рабочая роль Runbook для Windows

#### <a name="create-signing-certificate"></a>Создание сертификата для подписи

В следующем примере создается самозаверяющийся сертификат, который можно использовать для подписывания модулей runbook. Образец создает сертификат и экспортирует его. Позже сертификат импортируется в гибридную рабочую роль Runbook. Отпечаток также возвращается; он будет использоваться позже для указания сертификата.

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
                                        -Subject "CN=contoso.com" `
                                        -KeyAlgorithm RSA `
                                        -KeyLength 2048 `
                                        -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                                        -KeyExportPolicy Exportable `
                                        -KeyUsage DigitalSignature `
                                        -Type CodeSigningCert


# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

#### <a name="configure-the-hybrid-runbook-workers"></a>Настройка гибридной рабочей роли Runbook

Скопируйте сертификат, созданный для гибридной рабочей роли Runbook, в группу. Выполните следующий сценарий, чтобы импортировать сертификат и настроить гибридную рабочую роль, для использования проверки подписи в модулях Runbook.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

#### <a name="sign-your-runbooks-using-the-certificate"></a>Вход в модули Runbook с помощью сертификата

С помощью гибридных рабочих ролей Runbook, которые настроены для использования только подписанных модулей runbook, необходимо подписать модули runbook, которые будут использоваться в гибридной рабочей роли Runbook. Используйте следующий образец PowerShell для подписи модулей Runbook.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

После подписывания runbook его нужно импортировать в учетную запись службы автоматизации Azure и опубликовать с блоком подписей. Чтобы узнать, как импортировать модули runbook, ознакомьтесь с разделом [Creating or importing a runbook in Azure Automation](manage-runbooks.md#import-a-runbook) (Создание или импорт модуля Runbook в службу автоматизации Azure).

### <a name="linux-hybrid-runbook-worker"></a>Гибридная рабочая роль Runbook для Linux

Чтобы подписать модули Runbook в гибридной рабочей роли Runbook для Linux, на компьютере этой роли необходимо наличие исполняемого файла [GPG](https://gnupg.org/index.html).

#### <a name="create-a-gpg-keyring-and-keypair"></a>Создание набора и пары ключей GPG

Чтобы создать набор и пару ключей, необходимо использовать учетную запись гибридной рабочей роли Runbook `nxautomation`.

Используйте `sudo` для входа в качестве учетной записи `nxautomation`.

```bash
sudo su – nxautomation
```

Используя учетную запись `nxautomation`, создайте пару ключей GPG.

```bash
sudo gpg --generate-key
```

С помощью GPG вы сможете выполнить шаги по созданию пары ключей. Вам необходимо указать имя, адрес электронной почты, время окончания срока действия и парольную фразу, а затем дождаться объема энтропии, достаточного для создания ключа на компьютере.

Так как каталог GPG был сформирован с использованием программы Sudo, вам необходимо изменить его владельца на `nxautomation`.

Чтобы изменить владельца, выполните следующую команду.

```bash
sudo chown -R nxautomation ~/.gnupg
```

#### <a name="make-the-keyring-available-the-hybrid-runbook-worker"></a>Предоставление доступа к набору ключей для гибридной рабочей роли Runbook

Создав набор ключей, вы должны сделать его доступным для гибридной рабочей роли Runbook. Измените файл параметров `/var/opt/microsoft/omsagent/state/automationworker/diy/worker.conf` таким образом, чтобы в разделе `[worker-optional]` был следующий пример.

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

#### <a name="verify-signature-validation-is-on"></a>Включение проверки подписи

Если на компьютере отключена проверка подписи, вам необходимо включить ее. Чтобы включить проверку подписи, выполните следующую команду. Замените `<LogAnalyticsworkspaceId>` идентификатором рабочей области.

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

#### <a name="sign-a-runbook"></a>Подпись модуля runbook

Настроив проверку подписи для подписи модуля runbook, можно использовать следующую команду:

```bash
gpg –-clear-sign <runbook name>
```

Подписанный модуль runbook будет иметь имя `<runbook name>.asc`.

Теперь подписанный модуль runbook можно отправить в службу автоматизации Azure и выполнять как обычный модуль runbook.

## <a name="next-steps"></a>Следующие шаги

* Дополнительные сведения о разных методах запуска модуля см. в статье [Запуск модуля Runbook в службе автоматизации Azure](automation-starting-a-runbook.md).
* Описание разных методов работы с модулями Runbook PowerShell в службе автоматизации Azure с использованием текстового редактора см. в статье [Изменение модулей Runbook в службе автоматизации Azure](automation-edit-textual-runbook.md).
* Если модули Runbook выполняются с ошибками, см. раздел [Устранение неполадок с выполнением модулей Runbook](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).
* Дополнительные сведения о PowerShell, включая Справочник по языку и обучающие модули, см. в документации по [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/overview).
