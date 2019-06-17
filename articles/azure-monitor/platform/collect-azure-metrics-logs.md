---
title: Сбор журналов службы Azure и метрик в рабочей области Log Analytics | Документация Майкрософт
description: Настройка диагностики ресурсов Azure для записи журналов и метрик в рабочей области Log Analytics в Azure Monitor.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: magoedte
ms.openlocfilehash: d086b6f844deb06d98edec8d8ec0f5670d84f066
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66129717"
---
# <a name="collect-azure-service-logs-and-metrics-into-log-analytics-workspace-in-azure-monitor"></a>Сбор журналов службы Azure и метрик в рабочей области Log Analytics в Azure Monitor

Сбор журналов и метрик для служб Azure можно выполнить четырьмя разными способами:

1. Направление диагностики Azure в рабочую область Log Analytics в Azure Monitor (*диагностики* в следующей таблице)
2. Система диагностики Azure в службу хранилища Azure в рабочую область Log Analytics в Azure Monitor (*хранения* в следующей таблице)
3. Использование соединителей для служб Azure (*соединители* в следующей таблице).
4. Сценарии для сбора и публикации данных в рабочей области Log Analytics в Azure Monitor (пробелы в следующей таблице, а также для служб, которые не перечислены)


| Service                 | Тип ресурса                           | Журналы        | metrics     | Решение |
| --- | --- | --- | --- | --- |
| Шлюзы приложений    | Microsoft.Network/applicationGateways   | Диагностика | Диагностика | [Анализ шлюзов приложений Azure](../insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-azure-monitor) |
| Application Insights    |                                         | Соединитель   | Соединитель   | [Соединитель Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (предварительная версия) |
| Учетные записи службы автоматизации     | Microsoft.Automation/AutomationAccounts | Диагностика |             | [Дополнительные сведения](../../automation/automation-manage-send-joblogs-log-analytics.md)|
| Учетные записи пакетной службы          | Microsoft.Batch/batchAccounts           | Диагностика | Диагностика | |
| Классические облачные службы  |                                         | Хранилище     |             | [Дополнительные сведения](azure-storage-iis-table.md) |
| Cognitive Services      | Microsoft.CognitiveServices/accounts    |             | Диагностика | |
| Data Lake Analytics     | Microsoft.DataLakeAnalytics/accounts    | Диагностика |             | |
| Data Lake Store         | Microsoft.DataLakeStore/accounts        | Диагностика |             | |
| пространство имен концентратора событий;     | Microsoft.EventHub/namespaces           | Диагностика | Диагностика | |
| Центры Интернета вещей;                | Microsoft.Devices/IotHubs               |             | Диагностика | |
| Key Vault               | Microsoft.KeyVault/vaults               | Диагностика |             | [Анализ Key Vault](../insights/azure-key-vault.md) |
| Балансировщики нагрузки          | Microsoft.Network/loadBalancers         | Диагностика |             |  |
| Logic Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Диагностика | Диагностика | |
| группы сетевой безопасности; | Microsoft.Network/networksecuritygroups | Диагностика |             | [Анализ групп безопасности сети Azure](../insights/azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-azure-monitor) |
| Хранилища восстановления         | Microsoft.RecoveryServices/vaults       |             |             | [Служба анализа служб восстановления Azure (предварительная версия)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
| Службы поиска         | Microsoft.Search/searchServices         | Диагностика | Диагностика | |
| Пространство имен служебной шины   | Microsoft.ServiceBus/namespaces         | Диагностика | Диагностика | [Служба анализа служебной шины (предварительная версия)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
| Service Fabric          |                                         | Хранилище     |             | [Анализ Service Fabric (предварительная версия)](../../service-fabric/service-fabric-diagnostics-oms-setup.md) |
| SQL (версия 12)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Диагностика | [Службы анализа SQL Azure (предварительная версия)](../insights/azure-sql.md) |
| Хранилище                 |                                         |             | Скрипт      | [Служба анализа службы хранилища Azure (предварительная версия)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution) |
| Виртуальные машины        | Microsoft.Compute/virtualMachines       | Добавочный номер   | Добавочный номер <br> Диагностика  | |
| Масштабируемые наборы виртуальных машин | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Диагностика | |
| Фермы веб-серверов        | Microsoft.Web/serverfarms               |             | Диагностика | |
| Веб-сайты               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Диагностика | [Служба анализа веб-приложений Azure (предварительная версия)](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-web-apps-analytics) |


> [!NOTE]
> Для мониторинга виртуальных машин Azure (Linux и Windows) рекомендуется установить [расширение виртуальной машины Analytics журнала](../../azure-monitor/learn/quick-collect-azurevm.md). Агент предоставляет сведения, собранные в виртуальных машинах. Вы можете также использовать расширение для масштабируемых наборов виртуальных машин.
>
>

## <a name="azure-diagnostics-direct-to-log-analytics"></a>Направление диагностики Azure в Log Analytics
Многие ресурсы Azure могут записывать журналы диагностики и метрик непосредственно в рабочую область Log Analytics в Azure Monitor, и это предпочтительный способ сбора данных для анализа. При использовании диагностики Azure данные сразу записываются в рабочую область, и нет необходимости, чтобы сначала записать данные в хранилище.

Ресурсы Azure с поддержкой [Azure monitor](../../azure-monitor/overview.md) можно отправлять свои журналы и метрики напрямую в рабочую область Log Analytics.

> [!NOTE]
> Отправка многомерных метрик в рабочей области Log Analytics с помощью параметров диагностики сейчас не поддерживается. Метрики с измерениями экспортируются как преобразованные в плоскую структуру одномерные метрики, агрегированные по значениям измерений.
>
> *Пример*. Метрику "Входящие сообщения" в концентраторе событий можно изучить и вывести в виде диаграммы на уровне очереди. Но при экспорте с помощью параметров диагностики метрика представлена в виде всех входящих сообщений для всех очередей концентратора событий.
>
>

* Дополнительные сведения о доступных метриках см. в разделе [Метрики, поддерживаемые Azure Monitor](../../azure-monitor/platform/metrics-supported.md).
* Дополнительные сведения о доступных журналах см. в разделе [Поддерживаемые службы и схемы для журналов диагностики](../../azure-monitor/platform/diagnostic-logs-schema.md).

### <a name="enable-diagnostics-with-powershell"></a>Включение диагностики с помощью PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

В следующем примере PowerShell показано, как использовать [AzDiagnosticSetting набора](/powershell/module/Az.Monitor/Set-AzDiagnosticSetting) включение диагностики в группе безопасности сети. Тот же подход работает для всех поддерживаемых ресурсов. Задайте для параметра `$resourceId` идентификатор ресурса, для которого нужно включить диагностику.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>Включение диагностики с помощью шаблонов Resource Manager

Чтобы включить диагностику при создании ресурса и отправлять ее в рабочую область Log Analytics, можно использовать шаблон, аналогичный приведенному ниже. Этот пример предназначен для учетной записи службы автоматизации, но он также подходит для всех поддерживаемых типов ресурсов.

```json
        {
            "type": "Microsoft.Automation/automationAccounts/providers/diagnosticSettings",
            "name": "[concat(parameters('omsAutomationAccountName'), '/', 'Microsoft.Insights/service')]",
            "apiVersion": "2015-07-01",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('omsAutomationAccountName'))]",
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspaceName'))]"
            ],
            "properties": {
                "workspaceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('omsWorkspaceName'))]",
                "logs": [
                    {
                        "category": "JobLogs",
                        "enabled": true
                    },
                    {
                        "category": "JobStreams",
                        "enabled": true
                    }
                ]
            }
        }
```

[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="azure-diagnostics-to-storage-then-to-log-analytics"></a>Отправка диагностики Azure в хранилище и в Log Analytics

Для сбора журналов из некоторых ресурсов, существует возможность отправлять журналы в службу хранилища Azure, а затем настройте рабочую область Log Analytics для чтения журналов из хранилища.

Этот подход можно использовать в Azure Monitor для сбора диагностических данных из хранилища Azure для журналов и следующих ресурсов:

| Ресурс | Журналы |
| --- | --- |
| Service Fabric |Событие трассировки событий Windows <br> Операционное событие <br> Событие субъектов Reliable Actors <br> Событие надежных служб |
| Виртуальные машины |Системный журнал Linux <br> Событие Windows <br> Журнал IIS <br> Событие трассировки событий Windows |
| веб-роли; <br> Рабочие роли |Системный журнал Linux <br> Событие Windows <br> Журнал IIS <br> Событие трассировки событий Windows |

> [!NOTE]
> Обычные данные Azure тарифы на хранение и передачу взимается при отправке диагностики в учетную запись хранения и рабочей области Log Analytics считывает данные из вашей учетной записи хранения.
>
>

См. в разделе [использование хранилища BLOB-объектов для IIS и хранилища таблиц для событий](azure-storage-iis-table.md) Дополнительные сведения о том, как Azure Monitor может собирать эти журналы.

## <a name="connectors-for-azure-services"></a>Соединители для служб Azure

Отсутствует соединитель для Application Insights, что позволяет данные, собранные Application Insights, отправляемых в рабочую область Log Analytics.

Дополнительные сведения о [соединителе Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-to-collect-and-post-data-to-log-analytics-workspace"></a>Сценарии для сбора и отправки данных в рабочей области Log Analytics

Для служб Azure, которые не предоставляют простой способ отправки журналов и метрик в рабочую область Log Analytics можно использовать сценарий автоматизации Azure для сбора журналов и метрик. Сценарий может отправить данные в рабочей области с помощью [API сборщика данных](../../azure-monitor/platform/data-collector-api.md)

Коллекции шаблонов Azure есть [примеры использования службы автоматизации Azure](https://azure.microsoft.com/resources/templates/?term=OMS) для сбора данных из служб и отправить его в Azure Monitor.

## <a name="next-steps"></a>Дальнейшие действия

* [Используйте хранилище BLOB-объектов для IIS и хранилище таблиц для событий](azure-storage-iis-table.md), чтобы считывать журналы служб Azure, которые записывают диагностические данные в табличное хранилище, или журналы IIS, записанные в хранилище BLOB-объектов.
* [Включите решения](../../azure-monitor/insights/solutions.md) , чтобы обеспечить глубокое понимание данных.
* [Воспользуйтесь запросами поиска](../../azure-monitor/log-query/log-query-overview.md) для анализа данных.
