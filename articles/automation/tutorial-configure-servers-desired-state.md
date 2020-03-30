---
title: Настройка требуемого состояния серверов и управление смещением с помощью службы автоматизации Azure
description: Руководство по управлению настройками сервера с помощью службы "Настройка состояния службы автоматизации Azure"
services: automation
ms.subservice: dsc
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: 9e2f04f59a56be6c516eb90de45fdf7327673086
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75416592"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Настройка требуемого состояния серверов и управление смещением

Служба "Настройка состояния службы автоматизации Azure" позволяет задавать конфигурации для серверов и гарантировать, что спустя время они будут находиться в указанном состоянии.

> [!div class="checklist"]
> - Подключение виртуальной машины для управления с помощью DSC службы автоматизации Azure.
> - Передача конфигурации в службу автоматизации Azure.
> - Компиляция конфигурации в конфигурации узла
> - Назначение конфигурации узла управляемому узлу.
> - Проверка состояния соответствия управляемого узла

## <a name="prerequisites"></a>Предварительные требования

Для выполнения этого руководства потребуется следующее:

- Учетная запись службы автоматизации Azure. Указания по созданию учетной записи запуска от имени пользователя для службы автоматизации Azure см. в статье [Проверка подлинности модулей Runbook в Azure с помощью учетной записи запуска от имени](automation-sec-configure-azure-runas-account.md).
- Виртуальная машина Azure Resource Manager (неклассическая) под управлением Windows Server 2008 R2 или более поздней версии. Инструкции по созданию виртуальной машины см. в статье [Создание первой виртуальной машины Windows на портале Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Azure PowerShell 3.6.0 или более поздней версии. Чтобы узнать версию, выполните команду `Get-Module -ListAvailable AzureRM`. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).
- Знакомство с платформой Desired State Configuration (DSC) Дополнительные сведения о DSC см. в статье [Общие сведения о службе настройки требуемого состояния Windows PowerShell](/powershell/scripting/dsc/overview/overview).

## <a name="log-in-to-azure"></a>Вход в Azure

Войдите в подписку Azure с помощью команды `Connect-AzureRmAccount` и следуйте инструкциям на экране.

```powershell
Connect-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Создание и передача конфигурации в службе автоматизации Azure

В этом руководстве используется простая конфигурация DSC, которая гарантирует, что на виртуальной машине установлены службы IIS.

Дополнительные сведения о конфигурации DSC см. в статье [Конфигурации DSC](/powershell/scripting/dsc/configurations/configurations).

В текстовом редакторе введите следующую команду и сохраните ее локально как `TestConfig.ps1`.

```powershell
configuration TestConfig {
   Node WebServer {
      WindowsFeature IIS {
         Ensure               = 'Present'
         Name                 = 'Web-Server'
         IncludeAllSubFeature = $true
      }
   }
}
```

> [!NOTE]
> В более продвинутых сценариях, где требуется импортировать несколько модулей, `Import-DscResource` обеспечивающих Ресурсы DSC, убедитесь, что каждый модуль имеет уникальную линию в конфигурации.

Вызовите командлет `Import-AzureRmAutomationDscConfiguration`, чтобы отправить конфигурацию в учетную запись службы автоматизации:

```powershell
 Import-AzureRmAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Компиляция конфигурации в конфигурации узла

Прежде чем назначать конфигурацию DSC узлу, ее нужно скомпилировать в конфигурации узла.

Дополнительные сведения о компилировании конфигураций см. в статье [Конфигурации DSC](/powershell/scripting/dsc/configurations/configurations).

Вызовите командлет `Start-AzureRmAutomationDscCompilationJob`, чтобы скомпилировать конфигурацию `TestConfig` в конфигурацию узла:

```powershell
Start-AzureRmAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

В вашей учетной записи службы автоматизации будет создана конфигурация узла с именем `TestConfig.WebServer`.

## <a name="register-a-vm-to-be-managed-by-state-configuration"></a>Регистрация виртуальной машины, управляемой с помощью настройки состояния

С помощью службы "Настройка состояния службы автоматизации Azure" можно управлять виртуальными машинами Azure (классическими или Resource Manager), локальными виртуальными машинами, компьютерами Linux, виртуальными машинами AWS и локальными физическими компьютерами. В этом разделе рассматривается регистрация только виртуальных машин Azure Resource Manager. Дополнительные сведения о регистрации компьютеров других типов см. в статье [Подключение компьютеров для управления с помощью Azure Automation DSC](automation-dsc-onboarding.md).

Вызовите командлет `Register-AzureRmAutomationDscNode`, чтобы зарегистрировать виртуальную машину в службе "Настройка состояния службы автоматизации Azure".

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm'
```

Указанная виртуальная машина будет зарегистрирована в качестве управляемого узла на платформе настройки состояния.

### <a name="specify-configuration-mode-settings"></a>Указание параметров режима конфигурации

При регистрации виртуальной машины в качестве управляемого узла можно также указать свойства конфигурации. Например, вы можете указать, что состояние машины должно применяться только один раз (DSC не будет пытаться применить конфигурацию после начальной проверки), указав `ApplyOnly` в качестве значения свойства **ConfigurationMode**.

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationMode 'ApplyOnly'
```

Вы также можете указать, как часто DSC нужно проверять состояние конфигурации, с помощью свойства **ConfigurationModeFrequencyMins**.

```powershell
# Run a DSC check every 60 minutes
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationModeFrequencyMins 60
```

Дополнительные сведения о настройке свойств конфигурации для управляемого узла см. в статье о [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode).

Дополнительные сведения о настройках конфигурации DSC см. в статье [Настройка локального диспетчера конфигураций](/powershell/scripting/dsc/managing-nodes/metaConfig).

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Назначение конфигурации узла управляемому узлу.

Теперь можно назначить конфигурации скомпилированного узла для виртуальной машины, которую необходимо настроить.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -NodeId $node.Id
```

При этом конфигурация узла с именем `TestConfig.WebServer` присваивается зарегистрированному узлу DSC с именем `DscVm`.
По умолчанию узел DSC проверяется на соответствие конфигурации узла каждые 30 минут.
Сведения о том, как изменить интервал проверки соответствия, см. в статье [Настройка локального диспетчера конфигураций](/powershell/scripting/dsc/managing-nodes/metaConfig).

## <a name="working-with-partial-configurations"></a>Работа с частичными конфигурациями

Государственная конфигурация Azure Automation поддерживает использование [частичных конфигураций.](/powershell/scripting/dsc/pull-server/partialconfigs)
В этом сценарии DSC настроен для самостоятельного управления несколькими конфигурациями, и каждая конфигурация извлекается из Azure Automation.
Однако только одна конфигурация может быть назначена узлам для учетной записи автоматизации.
Это означает, что если вы используете две конфигурации для узла, вам потребуются две учетные записи автоматизации.

Подробную информацию о том, как зарегистрировать частичную конфигурацию из службы pull, можно узнать документацию для [частичных конфигураций.](https://docs.microsoft.com/powershell/scripting/dsc/pull-server/partialconfigs#partial-configurations-in-pull-mode)

Для получения дополнительной информации о том, как команды могут работать вместе, чтобы совместно управлять серверами с помощью конфигурации, как код увидеть [Понимание роли DSC в CI / CD трубопровода](/powershell/scripting/dsc/overview/authoringadvanced).

## <a name="check-the-compliance-status-of-a-managed-node"></a>Проверка состояния соответствия управляемого узла

Отчеты о состоянии соответствия управляемого узла можно получить, вызвав командлет `Get-AzureRmAutomationDscNodeReport`.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzureRmAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeId $node.Id

# Display the most recent report
$reports[0]
```

## <a name="removing-nodes-from-service"></a>Удаление узлов из службы

При добавлении узла в конфигурацию состояния автоматизации Azure настройки установки в локальном менеджере конфигурации устанавливаются для регистрации в конфигурациях службы и вытягивания и необходимых модулей для настройки машины.
Если вы решите удалить узел из службы, это можно сделать с помощью портала Azure или Az cmdlets.

> [!NOTE]
> Отменить узла из службы только устанавливают настройки локального диспетчера конфигурации, чтобы узла больше не подключали к службе.
> Это не влияет на конфигурацию, которая в настоящее время применяется к узелу.
> Чтобы удалить текущую конфигурацию, используйте [PowerShell](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument?view=powershell-5.1) или удалите локальный файл конфигурации (это единственный вариант для узлов Linux).

### <a name="azure-portal"></a>Портал Azure

Из Azure Automation нажмите на **конфигурацию состояния (DSC)** в таблице содержимого.
Далее нажмите **узлы,** чтобы просмотреть список узлов, зарегистрированных в службе.
Нажмите на название узла, который вы хотите удалить.
В открываемом представлении узла нажмите **Unregister.**

### <a name="powershell"></a>PowerShell

Чтобы отменить регистрацию узла из службы государственной конфигурации Azure Automation с помощью PowerShell, следуйте документации для cmdlet [Unregister-AzAutomationDscNode.](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-2.0.0)

## <a name="next-steps"></a>Дальнейшие действия

- Чтобы приступить к работе со службой "Настройка состояния службы автоматизации Azure", см. сведения в [этой статье](automation-dsc-getting-started.md).
- Дополнительные сведения о подключении узлов см. в статье [Подключение компьютеров для управления с помощью Azure Automation DSC](automation-dsc-onboarding.md).
- Сведения о компилировании конфигураций DSC, которые затем можно назначить целевым узлам, см. в статье [Компилирование конфигураций в Azure Automation DSC](automation-dsc-compile.md).
- Справочник по командлетам PowerShell для службы "Настройка состояния службы автоматизации Azure" см. в [этой статье](/powershell/module/azurerm.automation/#automation).
- Сведения о ценах см. на странице [с ценами на службу "Настройка состояния службы автоматизации Azure"](https://azure.microsoft.com/pricing/details/automation/).
- Пример использования службы "Настройка состояния службы автоматизации Azure" и Chocolatey в конвейере непрерывного развертывания см. в [этой статье](automation-dsc-cd-chocolatey.md).
