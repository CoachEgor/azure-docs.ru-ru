---
title: Гибридные рабочие роли Runbook службы автоматизации Azure для Windows
description: В этой статье содержатся сведения об установке гибридной рабочей роли Runbook в службе автоматизации Azure, которая позволяет запускать модули runbook на компьютерах под управлением Windows в локальном центре обработки данных или облачной среде.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 11/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: cd599fcfe403d64483e6b4db869b93b26f5db754
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2019
ms.locfileid: "74480814"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Развертывание гибридной рабочей роли Runbook для Windows

Гибридную рабочую роль Runbook службы автоматизации Azure можно использовать для выполнения модулей runbook непосредственно на компьютере, размещающем роль, для работы с ресурсами в среде, что позволяет управлять этими локальными ресурсами. Для хранения модулей runbook и управления ими используется служба автоматизации Azure, затем они передаются на один или несколько целевых компьютеров. В этой статье описано, как установить гибридную рабочую роль Runbook на компьютере Windows.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>Установка гибридной рабочей роли Runbook Windows

Ниже описаны два метода установки и настройки гибридной рабочей роли Runbook для Windows. Рекомендуется использовать модуль Runbook службы автоматизации для полностью автоматической настройки компьютера Windows. Второй метод заключается в пошаговом выполнении инструкций по ручной установке и настройке роли.

> [!NOTE]
> Чтобы управлять конфигурацией серверов, поддерживающих гибридную рабочую роль Runbook с настройкой требуемого состояния (DSC), добавьте такие серверы в качестве узлов DSC.

Ниже приведены минимальные требования для гибридной рабочей роли Runbook Windows.

* Windows Server 2012 или более поздней версии.
* Windows PowerShell версии 5.1 или более поздней ([скачать WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)).
* .NET Framework 4.6.2 или более поздней версии.
* Два ядра.
* 4 ГБ ОЗУ.
* Порт 443 (исходящий).

Дополнительные требования к сети для гибридной рабочей роли Runbook см. в разделе [Настройка сети](automation-hybrid-runbook-worker.md#network-planning).

Дополнительные сведения о развертывании серверов для управления с помощью DSC см. в статье [Подключение компьютеров для управления с помощью Azure Automation DSC](automation-dsc-onboarding.md).
Если вы включите [решение по управлению обновлениями](../operations-management-suite/oms-solution-update-management.md), любой подключенный к рабочей области Azure Log Analytics компьютер Windows будет автоматически настроен в качестве гибридной рабочей роли Runbook для поддержки модулей runbook, которые входят в это решение. Но он не регистрируется в группах гибридных рабочих ролей, которые уже определены в вашей учетной записи службы автоматизации. 

Его можно добавить в группу гибридных рабочих ролей Runbook в учетной записи службы автоматизации, чтобы обеспечить поддержку модулей Runbook службы автоматизации при условии, что вы используете одну и ту же учетную запись для решения и для членства в группе гибридных рабочих ролей Runbook. Эта функция добавлена в версии 7.2.12024.0 гибридной рабочей роли Runbook.

После успешного развертывания рабочей роли Runbook ознакомьтесь с [запуском модулей runbook в гибридной рабочей роли Runbook](automation-hrw-run-runbooks.md), чтобы узнать, как настроить модули runbook для автоматизации процессов в локальном центре обработки данных или другой облачной среде.

### <a name="automated-deployment"></a>Автоматизированное развертывание

Выполните следующие действия для автоматизации установки и настройки гибридной рабочей роли Windows:

1. Скачайте сценарий New-OnPremiseHybridWorker.ps1 из [коллекции PowerShell](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker) непосредственно на компьютер, на котором выполняется гибридная рабочая роль Runbook, или на другой компьютер в вашей среде. Скопируйте этот сценарий в рабочую роль.

   Для выполнения сценария New-OnPremiseHybridWorker.ps1 необходимо задать следующие параметры.

   * *AutomationAccountName* (обязательный) — имя учетной записи службы автоматизации.
   * *AAResourceGroupName* (обязательный) — имя группы ресурсов, связанной с вашей учетной записью службы автоматизации.
   * *OMSResourceGroupName* (необязательный) — имя группы ресурсов для рабочей области Log Analytics. Если эта группа ресурсов не указана, используется *AAResourceGroupName*.
   * *HybridGroupName* (обязательный) — имя группы гибридных рабочих ролей Runbook, которую вы указываете в качестве целевой для модулей runbook, поддерживающих этот сценарий.
   * *SubscriptionID* (обязательный) — идентификатор подписки Azure, которая используется для учетной записи службы автоматизации.
   * *WorkspaceName* (необязательный) — имя рабочей области Log Analytics. Если у вас нет рабочей области Log Analytics, сценарий создаст и настроит ее.

   > [!NOTE]
   > При включении решений только определенные регионы поддерживают связывание рабочей области Log Analytics и учетной записи службы автоматизации.
   >
   > For a list of the supported mapping pairs, see [Region mapping for Automation Account and Log Analytics workspace](how-to/region-mappings.md).

2. На своем компьютере откройте **Windows PowerShell** на **начальном** экране в режиме администратора.
3. Из оболочки командной строки PowerShell перейдите в папку, которая содержит загруженный сценарий. Измените значения параметров *-AutomationAccountName*, *-AAResourceGroupName*, *-OMSResourceGroupName*, *-HybridGroupName*, *-SubscriptionId* и *-WorkspaceName*. Затем выполните сценарий.

     > [!NOTE]
     > После выполнения сценария вы увидите запрос на аутентификацию в Azure. Для входа *необходимо* использовать учетную запись, которая является участником роли администраторов подписки и соадминистратором подписки.

   ```powershell-interactive
   .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
   -OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
   -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
   ```

4. Также вы получите предложение подтвердить установку пакета NuGet и указать учетные данные Azure для аутентификации.

5. После завершения выполнения сценария на странице **Группы гибридных рабочих ролей** отображается новая группа и количество элементов. Если указать имеющуюся группу, количество элементов в ней увеличивается. Вы можете выбрать группу из списка на странице **Группы гибридных рабочих ролей** и щелкнуть плитку **Гибридные рабочие роли**. На странице **Гибридные рабочие роли** отображается список элементов группы.

### <a name="manual-deployment"></a>Развертывание вручную

Выполните первые два шага для среды автоматизации, а затем повторите остальные шаги для каждого компьютера с рабочей ролью.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

#### <a name="1-create-a-log-analytics-workspace"></a>1. Create a Log Analytics workspace

Если у вас еще нет рабочей области Log Analytics, [создайте](../azure-monitor/platform/manage-access.md) ее. Если у вас уже есть рабочая область, вы можете использовать ее.

#### <a name="2-add-the-automation-solution-to-the-log-analytics-workspace"></a>2. Add the Automation solution to the Log Analytics workspace

The Automation Azure Monitor logs solution adds functionality for Azure Automation, including support for Hybrid Runbook Worker. Когда вы добавляете решение в рабочую область, она автоматически загружает компоненты рабочей роли на компьютер агента (вы установите его на следующем этапе).

To add the **Automation** Azure Monitor logs solution to your workspace, run the following PowerShell.

```powershell-interactive
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <LogAnalyticsWorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true
```

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Install the Microsoft Monitoring Agent

The Microsoft Monitoring Agent connects computers to Azure Monitor logs. Когда агент устанавливается на локальный компьютер и подключается к рабочей области, он автоматически скачивает компоненты, необходимые для гибридной рабочей роли Runbook.

To install the agent on the on-premises computer, follow the instructions at [Connect Windows computers to Azure Monitor logs](../log-analytics/log-analytics-windows-agent.md). Выполните эту процедуру на нескольких компьютерах, чтобы добавить в среду несколько компонентов Worker.

When the agent has successfully connected to Azure Monitor logs, it's listed on the **Connected Sources** tab of the log analytics **Settings** page. Если агент скачал решение службы автоматизации правильно, в каталоге C:\Program Files\Microsoft Monitoring Agent\Agent появится папка **AzureAutomationFiles**. Чтобы проверить версию гибридной рабочей роли Runbook, откройте папку C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ и найдите вложенную папку \\*version*.

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Install the runbook environment and connect to Azure Automation

When you add an agent to Azure Monitor logs, the Automation solution pushes down the **HybridRegistration** PowerShell module, which contains the **Add-HybridRunbookWorker** cmdlet. Этот командлет используется для установки среды модулей Runbook на компьютере и ее регистрации в службе автоматизации Azure.

Откройте сеанс PowerShell с правами администратора и импортируйте модуль, выполнив указанные ниже команды.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

После этого запустите командлет **Add-HybridRunbookWorker**, используя следующий синтаксис:

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Данные, необходимые для этого командлета, можно найти на странице **Управление ключами** на портале Azure. Откройте эту страницу, выбрав параметр **Ключи** на странице **Параметры** для учетной записи службы автоматизации.

![Страница "Управление ключами"](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **Имя группы** — это имя группы гибридных рабочих ролей Runbook. Если эта группа уже существует в учетной записи службы автоматизации, то к ней добавляется текущий компьютер. Если эта группа не существует, она добавляется.
* **Конечная точка** — это запись **URL-адрес** на странице **Управление ключами**.
* **Токен** — это запись **Первичный ключ доступа** на странице **Управление ключами**.

Для получения подробных сведений об установке используйте параметр **-Verbose** с командлетом **Add-HybridRunbookWorker**.

#### <a name="5-install-powershell-modules"></a>5. Install PowerShell modules

Модули runbook могут использовать любые из действий и командлетов, которые определены в модулях, установленных в вашей среде службы автоматизации Azure. Эти модули не развертываются на локальных компьютерах автоматически, поэтому их необходимо устанавливать вручную. Исключением является модуль Azure, который устанавливается по умолчанию и предоставляет доступ к командлетам для всех служб и действий службы автоматизации Azure.

Так как главной целью функции гибридной рабочей роли Runbook является управление локальными ресурсами, вам могут потребоваться модули, которые поддерживают эти ресурсы. Сведения об установке модулей Windows PowerShell см. в [этой статье](/powershell/scripting/developer/windows-powershell). 

Устанавливаемые модули должны находиться в расположении, указанном в переменной среды **PSModulePath**, чтобы гибридная рабочая роль автоматически импортировала их. Дополнительные сведения см. в статье об [изменении пути установки PSModulePath](/powershell/scripting/developer/windows-powershell).

## <a name="next-steps"></a>Дальнейшие действия

* Чтобы узнать, как настроить модули runbook для автоматизации процессов в локальном центре обработки данных или другой облачной среде, см. статью [Запуск модулей runbook в гибридной рабочей роли Runbook](automation-hrw-run-runbooks.md).
* Инструкции по удалению гибридных рабочих ролей Runbook см. в разделе [Удаление гибридных рабочих ролей Runbook в службе автоматизации Azure](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker) статьи "Автоматизация ресурсов в центре обработки данных или облаке с помощью гибридной рабочей роли Runbook".
* Дополнительные сведения об устранении неполадок с гибридными рабочими ролями Runbook см. в [этом разделе](troubleshoot/hybrid-runbook-worker.md#windows).
* Дополнительные действия по устранению проблем с решением "Управление обновлениями" см. в разделе [Устранение неполадок](troubleshoot/update-management.md) статьи "Решение для управления обновлениями в Azure".
