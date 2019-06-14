---
title: Потоковая передача журналов диагностики Azure в концентратор событий
description: Узнайте, как настроить потоковую передачу журналов диагностики Azure в концентратор событий.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: b5299af375646e7759d0770139df2cd6d7ce105c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60237732"
---
# <a name="stream-azure-diagnostic-logs-to-an-event-hub"></a>Потоковая передача журналов диагностики Azure в концентратор событий
**[Журналы диагностики Azure](diagnostic-logs-overview.md)** можно передавать в близком к реальному времени в любое приложение. Для этого достаточно использовать стандартный параметр "Экспорт в Центры событий" на портале или включить идентификатор правила авторизации концентратора событий в параметрах диагностики с помощью командлетов Azure PowerShell или интерфейса командной строки Azure.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Что можно делать с журналами диагностики и Центрами событий
Мы приведем лишь несколько примеров использования потоковой передачи журналов диагностики.

* **Потоковая передача журналов в сторонние системы ведения журналов и сбора телеметрии.** Вы можете передать потоком все журналы диагностики в отдельный концентратор событий, чтобы направить данные журналов в канал SIEM стороннего производителя или средство анализа журналов.
* **Проверка работоспособности службы путем потоковой передачи данных критического пути в Power BI.** С помощью Центров событий Stream Analytics и Power BI можно в близком к реальному времени преобразовать аналитическую информацию о службах Azure на основе диагностических данных. [В этой статье приводятся подробные инструкции по настройке Центров событий, обработке данных в Stream Analytics и выводу информации через Power BI](../../stream-analytics/stream-analytics-power-bi-dashboard.md). Вот несколько советов по настройке журналов диагностики:

  * Концентраторы событий для категории журналов диагностики создаются автоматически, когда вы выбираете соответствующий вариант на портале или включаете его с помощью PowerShell. Поэтому концентраторы событий следует выбирать в пространстве имен, имя которого начинается с **insights-** .
  * Приведенный ниже пример SQL-запроса для Stream Analytics позволяет перенести все данные из журнала в таблицу Power BI:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the Power BI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Создание пользовательской платформы для телеметрии и ведения журнала**. Если у вас уже есть пользовательская платформа для телеметрии и ведения журнала или вы только планируете ее создать, высокая масштабируемость публикации и подписки Центров событий позволит вам гибко настраивать прием журналов диагностики. [Ознакомьтесь с руководством Дэна Росановы (Dan Rosanova) по использованию Центров событий для глобальной платформы телеметрии](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Включение потоковой передачи журналов диагностики

Потоковую передачу журналов диагностики можно включить программно, через портал или с помощью [REST API Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). В любом случае создается параметр диагностики, в котором необходимо указать пространство имен Центров событий, а также категории журнала и метрики, которые требуется отправить в пространство имен. Концентратор событий создается в пространстве имен для каждой включенной категории журнала. Категория **журналов диагностики** — это тип журналов, которые может собирать ресурс.

> [!WARNING]
> Для включения и потоковой передачи журналов диагностики из вычислительных ресурсов (например, виртуальных машин или Service Fabric) [используется другая последовательность действий](../../azure-monitor/platform/diagnostics-extension-stream-event-hubs.md).

Пространство имен Центров событий не должно находиться в той же подписке, что и ресурс, генерирующий журналы, если пользователь, который настраивает этот параметр, имеет соответствующий доступ RBAC к обеим подпискам и обе подписки являются частью одного и того же клиента AAD.

> [!NOTE]
> Отправка многомерных метрик с помощью параметров диагностики сейчас не поддерживается. Метрики с измерениями экспортируются как преобразованные в плоскую структуру одномерные метрики, агрегированные по значениям измерений.
>
> *Пример*. Метрику "Входящие сообщения" в концентраторе событий можно изучить и вывести в виде диаграммы на уровне очереди. Но при экспорте с помощью параметров диагностики метрика будет представлена в виде всех входящих сообщений для всех очередей концентратора событий.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Потоковая передача журналов диагностики с помощью портала

1. На портале перейдите к Azure Monitor и щелкните **Параметры диагностики**.

    ![Раздел мониторинга Azure Monitor](media/diagnostic-logs-stream-event-hubs/diagnostic-settings-blade.png)

2. При необходимости отфильтруйте список по группе или типу ресурса, а затем щелкните ресурс, для которого необходимо задать параметр диагностики.

3. Если параметров для выбранного ресурса не существует, вам будет предложено создать параметр. Щелкните Turn on diagnostics (Включить диагностику).

   ![Добавление параметра диагностики — имеющиеся параметры отсутствуют](media/diagnostic-logs-stream-event-hubs/diagnostic-settings-none.png)

   Если в ресурсе имеются параметры, для него отобразится список настроенных параметров. Нажмите Add diagnostic setting (Добавить параметр диагностики).

   ![Добавление параметра диагностики — имеющиеся параметры](media/diagnostic-logs-stream-event-hubs/diagnostic-settings-multiple.png)

3. Введите имя параметра и установите флажок **Stream to an event hub** (Потоковая передача в концентратор событий), затем выберите пространство имен Центров событий.

   ![Добавление параметра диагностики — имеющиеся параметры](media/diagnostic-logs-stream-event-hubs/diagnostic-settings-configure.png)

   В выбранном пространстве имен будет создан концентратор событий (если вы еще не выполняли потоковую передачу журналов диагностики), или в него будет выполняться передача (если уже существуют ресурсы для потоковой передачи соответствующей категории журналов в это пространство имен). Разрешения, предоставляемые механизму потоковой передачи, определяются политикой. Сейчас для потоковой передачи журналов в концентратор событий нужны разрешения "Управление", "Отправка" и "Прослушивание". Политики общего доступа для пространства имен Центров событий можно создать или изменить на портале, на вкладке "Настройка" соответствующего пространства имен. Чтобы обновить один из этих параметров диагностики, клиент должен иметь разрешение ListKey для правила авторизации Центров событий. При необходимости можно также указать имя концентратора событий. Это позволит перенаправлять журналы в этот концентратор событий, а не в созданный для каждой категории журналов.

4. Выберите команду **Сохранить**.

Через несколько секунд новый параметр появится в списке параметров этого ресурса, и сразу же после создания данных о событии журналы диагностики будут отправлены в необходимый концентратор событий.

### <a name="via-powershell-cmdlets"></a>С помощью командлетов PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Для включения потоковой передачи с помощью [командлетов Azure PowerShell](../../azure-monitor/platform/powershell-quickstart-samples.md) применяется командлет `Set-AzDiagnosticSetting` с такими параметрами:

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource ID] -EventHubAuthorizationRuleId [your Event Hub namespace auth rule ID] -Enabled $true
```

Идентификатор правила авторизации концентратора событий представляет собой строку в формате `{Event Hub namespace resource ID}/authorizationrules/{key name}`. Пример: `/subscriptions/{subscription ID}/resourceGroups/{resource group}/providers/Microsoft.EventHub/namespaces/{Event Hub namespace}/authorizationrules/RootManageSharedAccessKey`. Сейчас с помощью PowerShell нельзя выбирать концентратор событий с определенным именем.

### <a name="via-azure-cli"></a>С помощью интерфейса командной строки Azure

Чтобы включить потоковую передачу с помощью [Azure CLI](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest), используйте команду [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create).

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --event-hub <event hub name> \
    --event-hub-rule <event hub rule ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Вы можете расширить набор категорий в журнале диагностики, добавив словари в массив JSON, переданный в качестве параметра `--logs`.

Для аргумента `--event-hub-rule` используется то же формат, что и для идентификатора правила авторизации концентратора событий, как описано для командлета PowerShell.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Как используются данные журнала из Центров событий?

Ниже приведен пример выходных данных из Центров событий.

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Имя элемента | Описание |
| --- | --- |
| records |Массив всех событий журнала, включенных в этот набор полезных данных. |
| time |Время возникновения события. |
| category |Категория журнала для этого события. |
| resourceId |Идентификатор ресурса, который создал это событие. |
| operationName |Имя операции. |
| level |Необязательный элемент. Уровень ведения журнала событий. |
| properties |Свойства события. |

Список всех поставщиков ресурсов, которые поддерживают потоковую передачу в Центры событий, см. [здесь](diagnostic-logs-overview.md).

## <a name="stream-data-from-compute-resources"></a>Потоковая передача данных от вычислительных ресурсов

Можно также передавать поток данных журналов диагностики от вычислительных ресурсов с помощью агента системы диагностики Azure. [В этой статье](../../azure-monitor/platform/diagnostics-extension-stream-event-hubs.md) описано, как это настроить.

## <a name="next-steps"></a>Дальнейшие действия

* [Руководство по потоковой передаче журналов Azure Active Directory с использованием Azure Monitor](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [Дополнительные сведения о журналах диагностики Azure](diagnostic-logs-overview.md)
* [Начало работы с Центрами событий](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

