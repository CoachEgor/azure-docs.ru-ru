---
title: Настройка требуемого состояния серверов и управление смещением с помощью службы автоматизации Azure
description: Руководство по управлению настройками сервера с помощью службы "Настройка состояния службы автоматизации Azure"
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
manager: carmonm
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: 3bcdb667ee649b9bbf32ad33e74e876cdd2b5cbf
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144191"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Настройка требуемого состояния серверов и управление смещением

Служба "Настройка состояния службы автоматизации Azure" позволяет задавать конфигурации для серверов и гарантировать, что спустя время они будут находиться в указанном состоянии.

> [!div class="checklist"]
> - Подключение виртуальной машины для управления с помощью DSC службы автоматизации Azure.
> - Передача конфигурации в службу автоматизации Azure.
> - Компиляция конфигурации в конфигурации узла.
> - Назначение конфигурации узла управляемому узлу.
> - Проверка состояния соответствия управляемого узла

## <a name="prerequisites"></a>Технические условия

Для работы с этим учебником требуется:

- Учетная запись службы автоматизации Azure. Указания по созданию учетной записи запуска от имени пользователя для службы автоматизации Azure см. в статье [Проверка подлинности модулей Runbook в Azure с помощью учетной записи запуска от имени](automation-sec-configure-azure-runas-account.md).
- Виртуальная машина Azure Resource Manager (неклассическая) под управлением Windows Server 2008 R2 или более поздней версии. Инструкции по созданию виртуальной машины см. в статье [Создание первой виртуальной машины Windows на портале Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Azure PowerShell 3.6.0 или более поздней версии. Чтобы узнать версию, выполните команду `Get-Module -ListAvailable AzureRM`. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).
- Знакомство с платформой Desired State Configuration (DSC) Дополнительные сведения о DSC см. в статье [Общие сведения о службе настройки требуемого состояния Windows PowerShell](https://docs.microsoft.com/powershell/dsc/overview).

## <a name="log-in-to-azure"></a>Вход в Azure

Войдите в подписку Azure с помощью команды `Connect-AzureRmAccount` и следуйте инструкциям на экране.

```powershell
Connect-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Создание и передача конфигурации в службе автоматизации Azure

В этом руководстве используется простая конфигурация DSC, которая гарантирует, что на виртуальной машине установлены службы IIS.

Дополнительные сведения о конфигурации DSC см. в статье [Конфигурации DSC](/powershell/dsc/configurations).

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

Вызовите командлет `Import-AzureRmAutomationDscConfiguration`, чтобы отправить конфигурацию в учетную запись службы автоматизации:

```powershell
 Import-AzureRmAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Компиляция конфигурации в конфигурации узла

Прежде чем назначать конфигурацию DSC узлу, ее нужно скомпилировать в конфигурации узла.

Дополнительные сведения о компилировании конфигураций см. в статье [Конфигурации DSC](/powershell/dsc/configurations).

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

Дополнительные сведения о настройках конфигурации DSC см. в статье [Настройка локального диспетчера конфигураций](/powershell/dsc/metaconfig).

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Назначение конфигурации узла управляемому узлу

Теперь можно назначить конфигурации скомпилированного узла для виртуальной машины, которую необходимо настроить.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -NodeId $node.Id
```

При этом конфигурация узла с именем `TestConfig.WebServer` присваивается зарегистрированному узлу DSC с именем `DscVm`.
По умолчанию узел DSC проверяется на соответствие конфигурации узла каждые 30 минут.
Сведения о том, как изменить интервал проверки соответствия, см. в статье [Настройка локального диспетчера конфигураций](/PowerShell/DSC/metaConfig).

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

При добавлении узла в конфигурации состояния службы автоматизации Azure для регистрации в конфигурации службы и по запросу и необходимые модули, для настройки компьютера задаются параметры локального диспетчера конфигураций.
Если вы решили удалить узел из службы, это можно сделать с помощью портала Azure или командлетов Az.

> [!NOTE]
> Отмена регистрации узла в службе только задает параметры локального диспетчера конфигураций, поэтому узел больше не подключается к службе.
> Это не влияет на конфигурацию, применяемое к узлу.
> Чтобы удалить текущую конфигурацию, используйте [PowerShell](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument?view=powershell-5.1) или удалить локальный файл конфигурации (это параметр только для узлов Linux).

### <a name="azure-portal"></a>Портал Azure

Из службы автоматизации Azure, щелкните **конфигурации состояния (DSC)** в оглавлении.
Далее щелкните **узлы** для просмотра списка узлов, зарегистрированных в службе.
Щелкните имя узла, который вы хотите удалить.
В отобразившемся представлении узла, щелкните **Unregister**.

### <a name="powershell"></a>PowerShell

Чтобы отменить регистрацию узла из службы настройки состояния службы автоматизации Azure с помощью PowerShell, следуя инструкциям в документации по командлету [Unregister-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-2.0.0).

## <a name="next-steps"></a>Дальнейшие действия

- Чтобы приступить к работе со службой "Настройка состояния службы автоматизации Azure", см. сведения в [этой статье](automation-dsc-getting-started.md).
- Дополнительные сведения о подключении узлов см. в статье [Подключение компьютеров для управления с помощью Azure Automation DSC](automation-dsc-onboarding.md).
- Сведения о компилировании конфигураций DSC, которые затем можно назначить целевым узлам, см. в статье [Компилирование конфигураций в Azure Automation DSC](automation-dsc-compile.md).
- Справочник по командлетам PowerShell для службы "Настройка состояния службы автоматизации Azure" см. в [этой статье](/powershell/module/azurerm.automation/#automation).
- Сведения о ценах см. на странице [с ценами на службу "Настройка состояния службы автоматизации Azure"](https://azure.microsoft.com/pricing/details/automation/).
- Пример использования службы "Настройка состояния службы автоматизации Azure" и Chocolatey в конвейере непрерывного развертывания см. в [этой статье](automation-dsc-cd-chocolatey.md).
