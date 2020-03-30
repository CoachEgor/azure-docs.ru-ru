---
title: Создайте & настроить аналитику журналов с помощью PowerShell
description: Рабочие области журналов Analytics в Azure Monitor хранят данные с серверов в вашей постоянной или облачной инфраструктуре. При генерировании системой диагностики Azure можно брать данные компьютера из хранилища Azure.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/19/2019
ms.openlocfilehash: 2584cedceab1386cbab9c72bb4b510eebe2122bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "80054696"
---
# <a name="manage-log-analytics-workspace-in-azure-monitor-using-powershell"></a>Управление рабочим пространством аналитики журналов в Azure Monitor с помощью PowerShell

[Смдлеты Log Analytics PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/) можно использовать для выполнения различных функций в рабочем пространстве log Analytics в Azure Monitor из командной строки или как часть скрипта.  Примеры задач, которые можно выполнять с помощью PowerShell.

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

## <a name="prerequisites"></a>Предварительные требования
Эти примеры работают с версией 1.0.0 или позже модуля Az.OperationalInsights.


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
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique across all Azure subscriptions - Get-Random helps with this for the example code
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
        "Query":  "Perf | where ObjectName == "LogicalDisk" and CounterName == "Current Disk Queue Length" and InstanceName == "C:"",
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
Get-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

# List enabled solutions
(Get-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

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
New-AzOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzOperationalInsightsLinuxPerformanceCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

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

> [!NOTE]
> Формат параметра **CustomLogRawJson,** определяющий конфигурацию для пользовательского журнала, может быть сложным. Используйте [Get-AzOperationalInsightsDataSource](https://docs.microsoft.com/powershell/module/az.operationalinsights/get-azoperationalinsightsdatasource?view=azps-3.2.0) для получения конфигурации для существующего пользовательского журнала. Свойство **Свойство** — это конфигурация, необходимая для параметра **CustomLogRawJson.**

В указанном примере regexDelimiter был определен как \\n для новой строки. Разделителем журнала также может служить метка времени.  Далее приведены поддерживаемые форматы.

| Формат | В стандартных RegEx формат JSON RegEx использует два знака \\ для каждого "\", если тестирование в приложениях RegEx уменьшить с \\ на "\". | | |
| --- | --- | --- | --- |
| `YYYY-MM-DD HH:MM:SS` | `((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |
| `M/D/YYYY HH:MM:SS AM/PM` | `(([0-1]\\d)|[0-9])/(([0-3]\\d)|(\\d))/((\\d{2})|(\\d{4}))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]\\s(AM|PM|am|pm)` | | |
| `dd/MMM/yyyy HH:MM:SS` | `(([0-2][1-9]|[3][0-1])\\/(Jan|Feb|Mar|May|Apr|Jul|Jun|Aug|Oct|Sep|Nov|Dec|jan|feb|mar|may|apr|jul|jun|aug|oct|sep|nov|dec)\\/((19|20)[0-9][0-9]))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9])` |
| `MMM dd yyyy HH:MM:SS` | `(((?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Sept|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)).*?((?:(?:[0-2]?\\d{1})|(?:[3][01]{1})))(?![\\d]).*?((?:(?:[1]{1}\\d{1}\\d{1}\\d{1})|(?:[2]{1}\\d{3})))(?![\\d]).*?((?:(?:[0-1][0-9])|(?:[2][0-3])|(?:[0-9])):(?:[0-5][0-9])(?::[0-5][0-9])?(?:\\s?(?:am|AM|pm|PM))?))` | | |
| `yyMMdd HH:mm:ss` | `([0-9]{2}([0][1-9]|[1][0-2])([0-2][0-9]|[3][0-1])\\s\\s?([0-1]?[0-9]|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `ddMMyy HH:mm:ss` | `(([0-2][0-9]|[3][0-1])([0][1-9]|[1][0-2])[0-9]{2}\\s\\s?([0-1]?[0-9]|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\s\\s?([0]?[1-9]|[1-2][0-9]|[3][0-1])\\s([0-1]?[0-9]|[2][0-3]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM  d HH:mm:ss` <br> два пробела после МММ | `(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\s\\s([0]?[1-9]|[1-2][0-9]|[3][0-1])\\s([0][0-9]|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\s([0]?[1-9]|[1-2][0-9]|[3][0-1])\\s([0][0-9]|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `dd/MMM/yyyy:HH:mm:ss +zzzz` <br> где "+" обозначает "+" или "-" <br> смещение времени часового пояса | `(([0-2][1-9]|[3][0-1])\\/(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\/((19|20)[0-9][0-9]):([0][0-9]|[1][0-2]):([0-5][0-9]):([0-5][0-9])\\s[\\+|\\-][0-9]{4})` | | |
| `yyyy-MM-ddTHH:mm:ss` <br> T является литералом буквы T | `((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))T((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |

## <a name="configuring-log-analytics-to-send-azure-diagnostics"></a>Настройка аналитики журналов для отправки диагностики Azure
Для отслеживания ресурсов Azure без использования агента необходимо включить для этих ресурсов систему диагностики Azure и настроить ее для записи данных в рабочую область Log Analytics. Этот подход отправляет данные непосредственно в рабочее пространство и не требует записи данных на учетную запись хранилища. Ниже перечислены поддерживаемые ресурсы:

| Тип ресурса | Журналы | Метрики |
| --- | --- | --- |
| Шлюзы приложений    | Да | Да |
| Учетные записи службы автоматизации     | Да | |
| Учетные записи пакетной службы          | Да | Да |
| Data Lake Analytics     | Да | |
| Data Lake Store         | Да | |
| пул эластичных баз данных SQL;        |     | Да |
| пространство имен концентратора событий;     |     | Да |
| Центры Интернета вещей;                |     | Да |
| Key Vault               | Да | |
| Балансировщики нагрузки          | Да | |
| Logic Apps              | Да | Да |
| группы сетевой безопасности; | Да | |
| Кэш Redis для Azure             |     | Да |
| Службы поиска         | Да | Да |
| Пространство имен служебной шины   |     | Да |
| SQL (версия 12)               |     | Да |
| Веб-сайты               |     | Да |
| Фермы веб-серверов        |     | Да |

Дополнительные сведения о доступных метриках см. в разделе [Метрики, поддерживаемые Azure Monitor](../../azure-monitor/platform/metrics-supported.md).

Для получения подробной информации о доступных журналах обратитесь к [поддерживаемым службам и схемам для журналов ресурсов.](../../azure-monitor/platform/diagnostic-logs-schema.md)

```powershell
$workspaceId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

Кроме того, вы можете использовать предыдущий командлет для сбора журналов из ресурсов, которые находятся в разных подписках. Этот командлет может работать в нескольких подписках, так как вы предоставляете идентификаторы ресурса, для которого создаются журналы, и рабочей области, в которую они отправляются.


## <a name="configuring-log-analytics-workspace-to-collect-azure-diagnostics-from-storage"></a>Настройка рабочего пространства log Analytics для сбора диагностики Azure из хранилища
Для сбора данных журнала из работающего экземпляра классической облачной службы или кластера Service Fabric необходимо сначала записать данные в службу хранилища Azure. Затем для сбора журналов из учетной записи хранилища настраивается рабочее пространство «Аналитика журнала». Ниже перечислены поддерживаемые ресурсы:

* классические облачные службы (рабочие и веб-роли);
* кластеры Service Fabric;

В приведенном ниже примере показано, как выполнить следующие задачи.

1. Перечислите существующие учетные записи и местоположения, которые рабочее пространство будет индексировать данные из
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

