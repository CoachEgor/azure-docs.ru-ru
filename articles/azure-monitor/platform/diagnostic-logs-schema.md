---
title: Поддерживаемые службы и схемы в журналах Системы диагностики Azure
description: Узнайте о поддерживаемых схемах служб и событий для журналов диагностики Azure.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 10/11/2018
ms.author: robb
ms.subservice: logs
ms.openlocfilehash: 21eec5ee2fef185a927f6a416732303765e02b1c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65789326"
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Поддерживаемые службы, схемы и категории для журналов диагностики Azure

[Журналы диагностики Azure Monitor](../../azure-monitor/platform/diagnostic-logs-overview.md) — это созданные службами Azure журналы, описывающие работу этих ресурсов и служб. Все журналы диагностики, доступные в Azure Monitor, предоставляют гибкую общую схему верхнего уровня, позволяющую каждой службе задавать уникальные свойства для своих событий.

Сочетание типа ресурса (доступного в свойстве `resourceId`) и свойства `category` является уникальным идентификатором схемы. В этой статье описывается схема верхнего уровня для журналов диагностики и приводятся ссылки на схемы для каждой службы.

## <a name="top-level-diagnostic-logs-schema"></a>Схема журналов диагностики верхнего уровня

| ИМЯ | Обязательный/необязательный | ОПИСАНИЕ |
|---|---|---|
| Twitter в режиме реального | Обязательно для заполнения | Метка времени события (UTC). |
| ResourceId | Обязательно для заполнения | Идентификатор ресурса, создавшего событие. Для служб клиента он имеет формат "/tenants/ИД_клиента/providers/имя_поставщика". |
| tenantId | Требуется для журналов клиента | Идентификатор клиента Active Directory клиента, к которому привязано это событие. Это свойство используется только для журналов уровня клиентов и не отображается в журналах уровня ресурсов. |
| operationName | Обязательно для заполнения | Имя операции, которую представляет это событие. Если событие представляет операцию RBAC, то это имя операции RBAC (например, Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Обычно моделируются в виде операции Resource Manager, даже если они фактически не являются задокументированными операциями Resource Manager (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | Необязательно | Версия API, связанного с операцией, если операция operationName выполнялась с помощью API (например, `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Если для этой операции не существует соответствующего API, то версия представляет версию этой операции, чтобы в дальнейшем изменить свойства, связанные с операцией. |
| category | Обязательно для заполнения | Категория журнала для события. Категория — степень детализации, при которой можно включать или отключать журналы для определенного ресурса. Свойства, которые отображаются в свойствах BLOB-объекта события, одинаковы в пределах определенной категории журнала и типа ресурса. Типичными категориями журнала являются "Аудит", "Операционный", "Выполнение" и "Запрос". |
| resultType | Необязательно | Состояние события. Обычные значения: "Запущен", "Выполняется", "Успешно", "Сбой", "Активно", "Разрешено". |
| resultSignature | Необязательно | Дополнительное состояние события. Если эта операция соответствует вызову REST API, то это код состояния HTTP соответствующего вызова REST. |
| resultDescription | Необязательно | Статическое текстовое описание этой операции, например "Получить файл хранилища". |
| durationMs | Необязательно | Время выполнения операции в миллисекундах. |
| callerIpAddress | Необязательно | IP-адрес вызывающего объекта, если операция соответствует вызову API, который будет приходить с объекта с общедоступным IP-адресом. |
| correlationId | Необязательно | Идентификатор GUID, используемый для формирования набора связанных событий. Как правило, если два события имеют одно и то же значение параметра operationName, но имеют различные состояния (например, "Запущен" и "Успешно"), они совместно используют один и тот же идентификатор корреляции. Идентификатор может также представлять другие связи между событиями. |
| identity | Необязательно | BLOB-объект типа JSON, описывающий идентификацию пользователя или приложения, выполнившего операцию. Обычно он включает авторизацию и утверждения или токен JWT из Active Directory. |
| Уровень | Необязательно | Уровень серьезности события. Должен быть одним из следующих значений: "Информационное", "Предупреждение", "Ошибка" или "Критическое". |
| location | Необязательно | Регион ресурса, создавшего событие, например "Восточная часть США" или "Южная Франция" |
| properties | Необязательно | Любые расширенные свойства, связанные с этой конкретной категорией событий. Все пользовательские или уникальные свойства должны быть размещены внутри этой "части B" схемы. |

## <a name="service-specific-schemas-for-resource-diagnostic-logs"></a>Схемы журналов диагностики ресурсов для конкретных служб
Схема для журналов диагностики ресурсов зависит от типа ресурса и категории журнала. В этом списке приведены все службы, которые предоставляют журналы диагностики, а также ссылки на схемы для конкретной службы и категории (при наличии).

| Service | Схемы и документы |
| --- | --- |
| Azure Active Directory | [Общие сведения о](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [схема журнала аудита](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) и [схемы входы в систему](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| Управление API | [Журналы диагностики управления API](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Шлюзы приложений |[Ведение журнала диагностики для шлюза приложений](../../application-gateway/application-gateway-diagnostics.md) |
| Служба автоматизации Azure |[Log Analytics для службы автоматизации Azure](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Пакетная служба Azure |[Ведение журналов диагностики пакетной службы Azure](../../batch/batch-diagnostics.md) |
| База данных Azure для MySQL | [Журналы сервера в базе данных Azure для MySQL](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| База данных Azure для PostgreSQL | [Журналы сервера в базе данных Azure для PostgreSQL](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Cognitive Services | Схема недоступна. |
| Сеть доставки содержимого | [Журналы Системы диагностики Azure для CDN](../../cdn/cdn-azure-diagnostic-logs.md) |
| Cosmos DB | [Журнал ведения диагностики Azure Cosmos DB](../../cosmos-db/logging.md) |
| Фабрика данных | [Мониторинг фабрик данных с помощью Azure Monitor](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Доступ к журналам диагностики для Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Доступ к журналам диагностики Azure Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Центры событий; |[Журналы диагностики Центров событий Azure](../../event-hubs/event-hubs-diagnostic-logs.md) |
| ExpressRoute | Схема недоступна. |
| Брандмауэр Azure | Схема недоступна. |
| Центр Интернета вещей | [Использование Azure Monitor](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Ведение журнала хранилища ключей Azure](../../key-vault/key-vault-logging.md) |
| Load Balancer |[Log Analytics для Azure Load Balancer](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Настраиваемая схема отслеживания сообщений B2B для приложений логики](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| группы сетевой безопасности; |[Аналитика журналов для групп безопасности сети](../../virtual-network/virtual-network-nsg-manage-log.md) |
| Защита от атак DDoS | [Управление службой "Защита от атак DDoS Azure" уровня "Стандартный"](../../virtual-network/manage-ddos-protection.md) |
| Выделенная служба PowerBI | [Ведение журнала диагностики для Power BI Embedded в Azure](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Службы восстановления | [Модель данных для Azure Backup](../../backup/backup-azure-reports-data-model.md)|
| поиска |[Включение и использование аналитики поискового трафика](../../search/search-traffic-analytics.md) |
| Служебная шина Azure |[Журналы диагностики служебной шины Azure](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| База данных SQL | [Метрики и журналы диагностики базы данных SQL Azure](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Журналы диагностики задания](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Диспетчер трафика | [Схема журнала диспетчера трафика](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Виртуальные сети | Схема недоступна. |
| Шлюзы виртуальной сети | Схема недоступна. |

## <a name="supported-log-categories-per-resource-type"></a>Поддерживаемые категории журнала для каждого типа ресурса
|Тип ресурса|Категория|Отображаемое имя категории|
|---|---|---|
|Microsoft.AnalysisServices/servers|Двигатель|Двигатель|
|Microsoft.AnalysisServices/servers|Service|Service|
|Microsoft.ApiManagement/service|GatewayLogs|Журналы, относящихся к шлюзу ApiManagement.|
|Microsoft.Automation/automationAccounts|JobLogs|Журналы заданий|
|Microsoft.Automation/automationAccounts|JobStreams|Потоки заданий|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Состояние узла DSC.|
|Microsoft.Batch/batchAccounts|ServiceLog|Журналы служб|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Возвращает метрики конечной точки, например пропускную способность, исходящий трафик и т. д.|
|Microsoft.ClassicNetwork/networksecuritygroups|Событие потока правил группы безопасности сети|Событие потока правил группы безопасности сети|
|Microsoft.CognitiveServices/accounts|Аудит|Журналы аудита|
|Microsoft.CognitiveServices/accounts|RequestResponse|Журналы запросов и ответов|
|Microsoft.ContainerService/managedClusters|kube-apiserver|Сервер API Kubernetes|
|Microsoft.ContainerService/managedClusters|kube-controller-manager|Диспетчер контроллеров Kubernetes|
|Microsoft.ContainerService/managedClusters|cluster-autoscaler|Средство автомасштабирования кластера Kubernetes|
|Microsoft.ContainerService/managedClusters|kube-scheduler|Планировщик Kubernetes|
|Microsoft.ContainerService/managedClusters|guard|Веб-перехватчик проверки подлинности|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataFactory/factories;|ActivityRuns|Журнал выполнения действий конвейера|
|Microsoft.DataFactory/factories;|PipelineRuns|Журнал запусков конвейера|
|Microsoft.DataFactory/factories;|TriggerRuns|Журнал запусков триггера|
|Microsoft.DataLakeAnalytics/accounts|Аудит|Журналы аудита|
|Microsoft.DataLakeAnalytics/accounts|Запросы|Журналы запросов|
|Microsoft.DataLakeStore/accounts|Аудит|Журналы аудита|
|Microsoft.DataLakeStore/accounts|Requests|Журналы запросов|
|Microsoft.DBforMySQL/servers|MySqlSlowLogs|Журналы сервера MySQL|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|Журналы сервера PostgreSQL|
|Microsoft.Devices/IotHubs|Подключения|Подключения|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Телеметрия устройства|
|Microsoft.Devices/IotHubs|C2DCommands|Команды, отправляемые из облака на устройство|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Операции с удостоверениями устройства|
|Microsoft.Devices/IotHubs|FileUploadOperations|Операции отправки файлов|
|Microsoft.Devices/IotHubs|Маршруты|Маршруты|
|Microsoft.Devices/IotHubs|Операции переноса с двойника устройства в облако|Операции переноса с двойника устройства в облако|
|Microsoft.Devices/IotHubs|C2DTwinOperations|Операции переноса из облака на двойник устройства|
|Microsoft.Devices/IotHubs|TwinQueries|Запросы к двойникам|
|Microsoft.Devices/IotHubs|JobsOperations|Операции заданий|
|Microsoft.Devices/IotHubs|DirectMethods|Прямые методы|
|Microsoft.Devices/IotHubs|E2EDiagnostics|Диагностика E2E (предварительная версия)|
|Microsoft.Devices/IotHubs|Конфигурации|Конфигурации|
|Microsoft.Devices/provisioningServices|DeviceOperations|Операции с устройствами|
|Microsoft.Devices/provisioningServices|ServiceOperations|Операции со службой|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.EventHub/namespaces|ArchiveLogs|Журналы архивации|
|Microsoft.EventHub/namespaces|OperationalLogs|Журналы операций|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Журналы автомасштабирования.|
|Microsoft.Insights/AutoscaleSettings|AutoscaleEvaluations|Оценки автомасштабирования|
|Microsoft.Insights/AutoscaleSettings|AutoscaleScaleActions|Действия автоматического увеличения масштаба|
|Microsoft.IoTSpaces/Graph|Трассировка|Трассировка|
|Microsoft.IoTSpaces/Graph|Рабочий режим|Рабочий режим|
|Microsoft.IoTSpaces/Graph|Аудит|Аудит|
|Microsoft.IoTSpaces/Graph|UserDefinedFunction|UserDefinedFunction|
|Microsoft.IoTSpaces/Graph|Входящие|Входящие|
|Microsoft.IoTSpaces/Graph|Исходящие|Исходящие|
|Microsoft.KeyVault/vaults|AuditEvent|Журналы аудита|
|Microsoft.Logic/workflows|WorkflowRuntime|События диагностики среды выполнения рабочего процесса|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Integration Account track events|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Событие группы безопасности сети|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Счетчик правил группы безопасности сети|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|События оповещения балансировщика нагрузки|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Состояние работоспособности балансировщика нагрузки|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|Уведомления о защите от атак DDoS|
|Microsoft.Network/publicIPAddresses|DDoSMitigationFlowLogs|Журналы последовательностей принятия решений по защите от атак DDoS|
|Microsoft.Network/publicIPAddresses|DDoSMitigationReports|Отчеты о защите от атак DDoS|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|Оповещения о защите виртуальной машины|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Журнал доступа к шлюзу приложений|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Журнал производительности шлюза приложений|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Журнал брандмауэра шлюза приложений|
|Microsoft.Network/securegateways|AzureFirewallApplicationRule|Правило приложения службы "Брандмауэр Azure"|
|Microsoft.Network/securegateways|AzureFirewallNetworkRule|Правило сети службы "Брандмауэр Azure"|
|Microsoft.Network/azurefirewalls|AzureFirewallApplicationRule|Правило приложения службы "Брандмауэр Azure"|
|Microsoft.Network/azurefirewalls|AzureFirewallNetworkRule|Правило сети службы "Брандмауэр Azure"|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Журналы диагностики шлюзов|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Журналы диагностики туннелей|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Журналы диагностики маршрутов|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|Журналы диагностики IKE|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|Журналы диагностики P2S|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Событие вывода результатов проверки работоспособности диспетчера трафика|
|Microsoft.Network/expressRouteCircuits|PeeringRouteLog|Журналы таблиц пиринга маршрутов|
|Microsoft.Network/frontdoors|FrontdoorAccessLog|Журнал доступа Frontdoor|
|Microsoft.Network/frontdoors|FrontdoorWebApplicationFirewallLog|Журнал брандмауэра веб-приложения Frontdoor|
|Microsoft.PowerBIDedicated/capacities|Двигатель|Двигатель|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Данные отчетов службы архивации Azure|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Задания Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|События Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Реплицированные элементы Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Статистика репликации Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Точки восстановления Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Периодичность отправки данных репликации Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Обновление данных защищенного диска Azure Site Recovery|
|Microsoft.Search/searchServices|OperationLogs|Журналы операций|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Журналы операций|
|Microsoft.Sql/servers/databases|SQLInsights|Аналитика SQL|
|Microsoft.Sql/servers/databases|AutomaticTuning|Автоматическая настройка|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Статистика среды выполнения хранилища запросов|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Статистика времени ожидания хранилища запросов|
|Microsoft.Sql/servers/databases|Errors|Errors|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Статистика времени ожидания базы данных|
|Microsoft.Sql/servers/databases|Время ожидания|Время ожидания|
|Microsoft.Sql/servers/databases|Блоки|Блоки|
|Microsoft.Sql/servers/databases|Взаимоблокировки|Взаимоблокировки|
|Microsoft.Sql/servers/databases|Аудит|Журналы аудита|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|Событие аудита безопасности SQL|
|Microsoft.Sql/servers/databases|DmsWorkers|Рабочие роли DMS|
|Microsoft.Sql/servers/databases|ExecRequests|Выполняющиеся запросы|
|Microsoft.Sql/servers/databases|RequestSteps|Действия, из которых состоит запрос|
|Microsoft.Sql/servers/databases|SqlRequests|Запросы SQL|
|Microsoft.Sql/servers/databases|Ожидания|Ожидания|
|Microsoft.Sql/managedInstances|ResourceUsageStats|Статистика использования ресурсов|
|Microsoft.Sql/managedInstances|SQLSecurityAuditEvents|Событие аудита безопасности SQL|
|Microsoft.Sql/managedInstances/databases|SQLInsights|Аналитика SQL|
|Microsoft.Sql/managedInstances/databases|QueryStoreRuntimeStatistics|Статистика среды выполнения хранилища запросов|
|Microsoft.Sql/managedInstances/databases|QueryStoreWaitStatistics|Статистика времени ожидания хранилища запросов|
|Microsoft.Sql/managedInstances/databases|Errors|Errors|
|Microsoft.StreamAnalytics/streamingjobs|Выполнение|Выполнение|
|Microsoft.StreamAnalytics/streamingjobs|Разработка|Разработка|
|microsoft.web/sites|FunctionExecutionLogs|Журналы выполнения функций|
|microsoft.web/sites/slots|FunctionExecutionLogs|Журналы выполнения функций|

## <a name="next-steps"></a>Следующие шаги

* [Сбор и использование данных журнала из ресурсов Azure](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Потоковая передача журналов диагностики Azure в **Центры событий**](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)
* [Создание или обновление диагностического параметра](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [Сбор журналов и метрик для служб Azure для использования в Log Analytics](../../azure-monitor/platform/collect-azure-metrics-logs.md)

