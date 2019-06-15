---
title: Использование PowerShell для создания и настройки рабочей области Log Analytics | Документация Майкрософт
description: Рабочие области log Analytics в Azure Monitor хранить данные с серверов в вашей локальной или облачной инфраструктуре. При генерировании системой диагностики Azure можно брать данные компьютера из хранилища Azure.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.devlang: powershell
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.openlocfilehash: 36cb2462a47f9d175ca25bbbde46a14009637db0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65907885"
---
# <a name="manage-log-analytics-workspace-in-azure-monitor-using-powershell"></a>Управление рабочей областью Log Analytics в Azure Monitor с помощью PowerShell

Можно использовать [командлеты Log Analytics PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/) для выполнения различных операций в рабочей области Log Analytics в Azure Monitor в командной строке или в скрипте.  Примеры задач, которые можно выполнять с помощью PowerShell.

* Создание рабочей области
* Добавление или удаление решения
* Импорт и экспорт сохраненных поисков
* Создание группы компьютеров
* Включение сбора журналов IIS с компьютеров, на которых установлен агент Windows
* Сбор счетчиков производительности с компьютеров под управлением Linux и Windows
* Сбор событий из системного журнала с компьютеров Linux
* Сбор событий из журналов событий Windows
* Сбор пользовательских журналов событий
* Добавление агента Log Analytics на виртуальную машину Azure
* Настройка Log Analytics для индексирования данных, собранных системой диагностики Azure

Эта статья содержит два примера кода, иллюстрирующих некоторые доступные в PowerShell функции.  Сведения о других функциях см. в [справочнике по командлетам PowerShell Log Analytics](https://docs.microsoft.com/powershell/module/az.operationalinsights/).

> [!NOTE]
> Компонент Log Analytics раньше назывался Operational Insights, поэтому именно такое имя используется в командлетах.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Технические условия
Эти примеры работают с версии 1.0.0 или более поздней версии модуля Az.OperationalInsights.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Создание и настройка рабочей области Log Analytics
Этот пример сценария иллюстрирует следующие задачи.

1. Создание рабочей области
2. Вывод списка доступных решений
3. Добавление решений в рабочую область
4. Импорт сохраненных поисков
5. Экспорт сохраненных поисков
6. Создание группы компьютеров
7. Включение сбора журналов IIS с компьютеров, на которых установлен агент Windows
8. Сбор счетчиков производительности логического диска с компьютеров под управлением Linux ("Процент использования индексных дескрипторов"; "Свободно мегабайт"; "Процент используемого места"; "Количество обращений к диску (в секунду)"; "Количество обращений чтения или записи (в секунду))"
9. Сбор событий из системного журнала с компьютеров Linux
10. Сбор событий (ошибок и предупреждений) из журнала событий приложений с компьютеров Windows
11. Сбор данных счетчика производительности "Доступный объем памяти" (в МБ) с компьютеров Windows
12. Сбор пользовательского журнала

```powershell

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Perf | where ObjectName == "LogicalDisk" and CounterName == "Current Disk Queue Length" and InstanceName == "C:",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1",
    "description": "Example custom log datasource",
    "inputs": [
        {
            "location": {
            "fileSystemLocations": {
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ],
                "linuxFileTypeLogPaths": [ "/var/logs" ]
                }
            },
        "recordDelimiter": {
            "regexDelimiter": {
                "pattern": "\\n",
                "matchIndex": 0,
                "matchIndexSpecified": true,
                "numberedGroup": null
                }
            }
        }
    ],
    "extractions": [
        {
            "extractionName": "TimeGenerated",
            "extractionType": "DateTime",
            "extractionProperties": {
                "dateTimeExtraction": {
                    "regex": null,
                    "joinStringRegex": null
                    }
                }
            }
        ]
    }
"@

# Create the resource group if needed
try {
    Get-AzResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

# List enabled solutions
(Get-AzOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json

# Create Computer Group based on a query
New-AzOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernel syslog collection"
Enable-AzOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs

New-AzOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```
В указанном примере regexDelimiter был определен как \\n для новой строки. Разделителем журнала также может служить метка времени.  Далее приведены поддерживаемые форматы.

| Формат | В стандартных RegEx формат JSON RegEx использует два знака \\ для каждого "\", если тестирование в приложениях RegEx уменьшить с \\ на "\". | | |
| --- | --- | --- | --- |
| `YYYY-MM-DD HH:MM:SS` | `((\\\\d{2})\|(\\\\d{4}))-([0-1]\\\\d)-(([0-3]\\\\d)\|(\\\\d))\\\\s((\\\\d)\|([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |
| `M/D/YYYY HH:MM:SS AM/PM` | `(([0-1]\\\\d)\|[0-9])/(([0-3]\\\\d)\|(\\\\d))/((\\\\d{2})\|(\\\\d{4}))\\\\s((\\\\d)\|([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]\\\\s(AM\|PM\|am\|pm)` | | |
| `dd/MMM/yyyy HH:MM:SS` | `((([0-3]\\\\d)\` | `(\\\\d))/(Jan\|Feb\|Mar\|May\|Apr\|Jul\|Jun\|Aug\|Oct\|Sep\|Nov\|Dec\|jan\|feb\|mar\|may\|apr\|jul\|jun\|aug\|oct\|sep\|nov\|dec)/((\\\\d{2})\|(\\\\d{4}))\\\\s((\\\\d)\` | `([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9])` |
| `MMM dd yyyy HH:MM:SS` | `(((?:Jan(?:uary)?\|Feb(?:ruary)?\|Mar(?:ch)?\|Apr(?:il)?\|May\|Jun(?:e)?\|Jul(?:y)?\|Aug(?:ust)?\|Sep(?:tember)?\|Sept\|Oct(?:ober)?\|Nov(?:ember)?\|Dec(?:ember)?)).*?((?:(?:[0-2]?\\\\d{1})\|(?:[3][01]{1})))(?![\\\\d]).*?((?:(?:[1]{1}\\\\d{1}\\\\d{1}\\\\d{1})\|(?:[2]{1}\\\\d{3})))(?![\\\\d]).*?((?:(?:[0-1][0-9])\|(?:[2][0-3])\|(?:[0-9])):(?:[0-5][0-9])(?::[0-5][0-9])?(?:\\\\s?(?:am\|AM\|pm\|PM))?))` | | |
| `yyMMdd HH:mm:ss` | `([0-9]{2}([0][1-9]\|[1][0-2])([0-2][0-9]\|[3][0-1])\\\\s\\\\s?([0-1]?[0-9]\|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `ddMMyy HH:mm:ss` | `(([0-2][0-9]\|[3][0-1])([0][1-9]\|[1][0-2])[0-9]{2}\\\\s\\\\s?([0-1]?[0-9]\|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\s\\\\s?([0]?[1-9]\|[1-2][0-9]\|[3][0-1])\\\\s([0-1]?[0-9]\|[2][0-3]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM  d HH:mm:ss` <br> два пробела после МММ | `(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\s\\\\s([0]?[1-9]\|[1-2][0-9]\|[3][0-1])\\\\s([0][0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\s([0]?[1-9]\|[1-2][0-9]\|[3][0-1])\\\\s([0][0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `dd/MMM/yyyy:HH:mm:ss +zzzz` <br> где "+" обозначает "+" или "-" <br> смещение времени часового пояса | `(([0-2][1-9]\|[3][0-1])\\\\/(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\/((19\|20)[0-9][0-9]):([0][0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])\\\\s[\\\\+\|\\\\-][0-9]{4})` | | |
| `yyyy-MM-ddTHH:mm:ss` <br> T является литералом буквы T | `((\\\\d{2})\|(\\\\d{4}))-([0-1]\\\\d)-(([0-3]\\\\d)\|(\\\\d))T((\\\\d)\|([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |

## <a name="configuring-log-analytics-to-send-azure-diagnostics"></a>Настройка Log Analytics для отправки данных системы диагностики Azure
Для отслеживания ресурсов Azure без использования агента необходимо включить для этих ресурсов систему диагностики Azure и настроить ее для записи данных в рабочую область Log Analytics. Такой подход отправляет данные непосредственно в рабочую область и не требует данных для записи в учетную запись хранения. Ниже перечислены поддерживаемые ресурсы.

| Тип ресурса | Журналы | Метрики |
| --- | --- | --- |
| Шлюзы приложений    | Yes | Yes |
| Учетные записи службы автоматизации     | Yes | |
| Учетные записи пакетной службы          | Yes | Yes |
| Data Lake Analytics     | Yes | |
| Data Lake Store         | Yes | |
| пул эластичных баз данных SQL;        |     | Yes |
| пространство имен концентратора событий;     |     | Yes |
| Центры Интернета вещей;                |     | Yes |
| Key Vault               | Yes | |
| Балансировщики нагрузки          | Yes | |
| Logic Apps              | Yes | Yes |
| группы сетевой безопасности; | Yes | |
| Кэш Azure для Redis             |     | Yes |
| Службы поиска         | Yes | Yes |
| Пространство имен служебной шины   |     | Yes |
| SQL (версия 12)               |     | Yes |
| Веб-сайты               |     | Yes |
| Фермы веб-серверов        |     | Yes |

Дополнительные сведения о доступных метриках см. в разделе [Метрики, поддерживаемые Azure Monitor](../../azure-monitor/platform/metrics-supported.md).

Дополнительные сведения о доступных журналах см. в разделе [Поддерживаемые службы и схемы для журналов диагностики](../../azure-monitor/platform/diagnostic-logs-schema.md).

```powershell
$workspaceId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

Кроме того, вы можете использовать предыдущий командлет для сбора журналов из ресурсов, которые находятся в разных подписках. Этот командлет может работать в нескольких подписках, так как вы предоставляете идентификаторы ресурса, для которого создаются журналы, и рабочей области, в которую они отправляются.


## <a name="configuring-log-analytics-workspace-to-collect-azure-diagnostics-from-storage"></a>Настройка рабочей области Log Analytics для сбора из хранилища системы диагностики Azure
Для сбора данных журнала из работающего экземпляра классической облачной службы или кластера Service Fabric необходимо сначала записать данные в службу хранилища Azure. Рабочую область Log Analytics настраивается для сбора журналов из учетной записи хранения. Ниже перечислены поддерживаемые ресурсы.

* классические облачные службы (рабочие и веб-роли);
* кластеры Service Fabric;

В приведенном ниже примере показано, как выполнить следующие задачи.

1. Список существующих учетных записей хранения и расположений, рабочей области будет индексировать данные из
2. Создание конфигурации для чтения из учетной записи хранения.
3. Обновление созданной конфигурации для индексирования данных из дополнительных расположений
4. Удаление созданной конфигурации.

```powershell
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable"
$workspace = (Get-AzOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want the workspace to index
$storageId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight"

```

Кроме того, вы можете использовать предыдущий сценарий для сбора журналов из учетных записей хранения, которые находятся в разных подписках. Этот сценарий может работать в нескольких подписках, так как вы предоставляете идентификатор ресурса учетной записи хранения и соответствующий ключ доступа. При изменении ключа доступа необходимо обновить данные хранилища с учетом нового ключа.


## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения об использовании PowerShell для настройки Log Analytics см. в [описании командлетов PowerShell Log Analytics](https://docs.microsoft.com/powershell/module/az.operationalinsights/).

