---
title: Мониторинг фабрик данных с помощью Azure Monitor | Документация Майкрософт
description: Сведения об использовании Azure Monitor для мониторинга конвейеров фабрики данных с помощью включения журналов диагностики с данными из фабрики данных Azure.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: shlo
ms.openlocfilehash: 6bad74d33f5d50bb7a35de69927bf97daad07798
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326873"
---
# <a name="alert-and-monitor-data-factories-using-azure-monitor"></a>Использование оповещений и мониторинг фабрик данных с помощью Azure Monitor
Облачные приложения являются сложными и содержат множество подвижных частей. Мониторинг дает возможность отслеживать данные, чтобы обеспечить работоспособность приложения, а также позволяет предотвратить потенциальные проблемы или устранить неполадки. Кроме того, данные мониторинга можно использовать для получения подробных сведений о приложении. Эти знания могут помочь повысить его производительность и улучшить возможности обслуживания, а также автоматизировать действия, которые в противном случае выполнялись бы вручную.

Azure Monitor предоставляет метрики инфраструктуры базового уровня, а также журналы для большинства служб Microsoft Azure. Дополнительные сведения см. в статье [Обзор Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor). Журналы диагностики Azure генерируются ресурсом. Они содержат подробные и своевременные данные о работе этого ресурса. Фабрика данных выводит журналы диагностики в Azure Monitor.

## <a name="persist-data-factory-data"></a>Хранение данных службы "Фабрика данных"
В фабрике данных данные запуска конвейеров сохраняются только в течение 45 дней. Если вы хотите хранить данные о запуске конвейера более 45 дней, используя Azure Monitor, вы можете не только перенаправлять журналы диагностики для анализа, но и хранить их в учетной записи хранения. Так у вас будут сведения о фабрике данных в течение требуемого периода времени.

## <a name="diagnostic-logs"></a>Журналы диагностики

* Сохранение журналов в **учетную запись хранения** для аудита или проверки вручную. В параметрах диагностики вы также можете задать время хранения (в днях).
* **Потоковая передача журналов в Центры событий** для приема сторонней службой или пользовательским аналитическим решением, например Power BI.
* Анализ журналов с помощью **Log Analytics**.

Вы можете использовать учетную запись хранения или пространство имен концентратора событий, не входящие в подписку, в которой создаются журналы. Пользователю, который настраивает этот параметр, должен быть предоставлен соответствующий уровень доступа RBAC к обеим подпискам.

## <a name="set-up-diagnostic-logs"></a>Настройка журналов диагностики

### <a name="diagnostic-settings"></a>параметрах диагностики
Журналы диагностики для всех ресурсов, кроме вычислительных, настраиваются с помощью параметров диагностики. Параметры диагностики для ресурса определяют следующие настройки.

* Куда отправляются журналы диагностики (учетная запись хранения, концентраторы событий или журналы Azure Monitor).
* Какие категории журналов будут отправляться.
* Как долго должны храниться журналы каждой категории в учетной записи хранения.
* Срок хранения 0 дней означает, что журналы хранятся неограниченно долго. В противном случае укажите количество дней в диапазоне от 1 до 2 147 483 647.
* Если политики хранения заданы, но хранение журналов в учетной записи хранения отключено (например, выбраны только параметры концентраторов событий или журналов Azure Monitor), политики хранения не будут действовать.
* Политики хранения применяются по дням, поэтому в конце дня (по времени в формате UTC) журналы, срок которых теперь превышает период хранения, будут удалены. Например, если настроена политика хранения в течение одного дня, то в начале текущего дня журналы за вчерашний день будет удалены.

### <a name="enable-diagnostic-logs-via-rest-apis"></a>Включение журналов диагностики с помощью REST API

Создайте или обновите параметры диагностики в REST API Azure Monitor.

**Запрос**
```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Заголовки**
* Замените `{api-version}` на `2016-09-01`.
* Замените `{resource-id}` идентификатором ресурса, параметры диагностики которого вы хотите изменить. Дополнительные сведения см. в статье [Управление ресурсами Azure через портал](../azure-resource-manager/manage-resource-groups-portal.md).
* В качестве заголовка `Content-Type` установите `application/json`.
* В качестве заголовка авторизации установите веб-маркер JSON, полученный из Azure Active Directory. Дополнительные сведения см. в статье [Сценарии аутентификации в Azure Active Directory](../active-directory/develop/authentication-scenarios.md).

**Текст**
```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "metrics": [
        ],
        "logs": [
                {
                    "category": "PipelineRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "TriggerRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "ActivityRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                }
            ]
    },
    "location": ""
}
```

| Свойство | type | Описание |
| --- | --- | --- |
| storageAccountId |String | Идентификатор учетной записи хранения, в которую необходимо отправить журналы диагностики. |
| serviceBusRuleId |String | Идентификатор правила служебной шины для пространства имен служебной шины, в котором будут созданы концентраторы событий для потоковой передачи журналов диагностики. Идентификатор правила имеет формат: {идентификатор ресурса служебной шины}/authorizationrules/{имя ключа}.|
| workspaceId | Сложный тип | Массив интервалов времени метрики и политики их хранения. В настоящее время это свойство пусто. |
|metrics| Значения параметров выполнения конвейера, которые должны быть переданы в вызываемый конвейер.| Объект JSON, сопоставляющий имена параметров со значениями аргументов. |
| logs| Сложный тип| Имя категории журнала диагностики для типа ресурса. Чтобы получить список категорий журналов диагностики ресурса, сначала выполните операцию получения параметров диагностики. |
| category| String | Массив категорий журналов и политики их хранения. |
| timeGrain | String | Уровень детализации метрик, сохраненных в формате длительности ISO 8601. Должно использоваться значение PT1M (одна минута).|
| enabled| Логическое | Указывает, включена ли коллекция этой метрики или категории журнала для выбранного ресурса.|
| retentionPolicy| Сложный тип| Описывает политику хранения метрики или категории журналов. Используется только для хранения параметра учетной записи.|
| days| int| Число дней для хранения метрик или журналов. Нулевое значение означает, что журналы хранятся неограниченно долго. Используется только для хранения параметра учетной записи. |

**Ответ**

200 ОК


```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```

Получение параметров диагностики в REST API Azure Monitor

**Запрос**
```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Заголовки**
* Замените `{api-version}` на `2016-09-01`.
* Замените `{resource-id}` идентификатором ресурса, параметры диагностики которого вы хотите изменить. Дополнительные сведения см. в статье "Управление ресурсами Azure через портал".
* В качестве заголовка `Content-Type` установите `application/json`.
* В качестве заголовка авторизации установите веб-маркер JSON, полученный из Azure Active Directory. Дополнительные сведения см. в статье "Сценарии аутентификации в Azure Active Directory".

**Ответ**

200 ОК

```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```
[Подробнее](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)

## <a name="schema-of-logs--events"></a>Схема журналов и событий

### <a name="azure-monitor-schema"></a>Схема Azure Monitor

#### <a name="activity-run-logs-attributes"></a>Атрибуты журналов выполнения действий

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "activityRunId":"",
   "pipelineRunId":"",
   "resourceId":"",
   "category":"ActivityRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "activityName":"",
   "start":"",
   "end":"",
   "properties":
       {
          "Input": "{
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "BlobSink"
              }
           }",
          "Output": "{"dataRead":121,"dataWritten":121,"copyDuration":5,
               "throughput":0.0236328132,"errors":[]}",
          "Error": "{
              "errorCode": "null",
              "message": "null",
              "failureType": "null",
              "target": "CopyBlobtoBlob"
        }
    }
}
```

| Свойство | type | Описание | Пример |
| --- | --- | --- | --- |
| Уровень |Строка, | Уровень журналов диагностики. Уровень 4 всегда относится к журналам выполнения действий. | `4`  |
| correlationId |Строка, | Уникальный идентификатор для полного отслеживания конкретного запроса. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | Строка, | Время события в формате UTC`YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|activityRunId| Строка,| Идентификатор выполнения действия. | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|pipelineRunId| Строка,| Идентификатор выполнения конвейера. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| Строка, | Идентификатор связанного ресурса фабрики данных. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Строка, | Категория журналов диагностики. Для этого свойства задайте значение ActivityRuns. | `ActivityRuns` |
|уровень| Строка, | Уровень журналов диагностики. Для этого свойства задайте значение Informational. | `Informational` |
|operationName| Строка, |Имя действия и состояние. Если состояние — начать пульс, это `MyActivity -`. Если состояние — окончить пульс, это `MyActivity - Succeeded` с конечным состоянием. | `MyActivity - Succeeded` |
|pipelineName| Строка, | Имя конвейера. | `MyPipeline` |
|activityName| Строка, | Имя действия. | `MyActivity` |
|start| Строка, | Время начала выполнения действия в формате UTC. | `2017-06-26T20:55:29.5007959Z`|
|end| Строка, | Время окончания выполнения действия в формате UTC. Если действие не завершено (журнал диагностики для действия выполняется), устанавливается значение по умолчанию `1601-01-01T00:00:00Z`.  | `2017-06-26T20:55:29.5007959Z` |

#### <a name="pipeline-run-logs-attributes"></a>Атрибуты журналов выполнения конвейеров

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "runId":"",
   "resourceId":"",
   "category":"PipelineRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "start":"",
   "end":"",
   "status":"",
   "properties":
    {
      "Parameters": {
        "<parameter1Name>": "<parameter1Value>"
      },
      "SystemParameters": {
        "ExecutionStart": "",
        "TriggerId": "",
        "SubscriptionId": ""
      }
    }
}
```

| Свойство | type | Описание | Пример |
| --- | --- | --- | --- |
| Уровень |Строка, | Уровень журналов диагностики. Уровень 4 относится к журналам выполнения действий. | `4`  |
| correlationId |Строка, | Уникальный идентификатор для полного отслеживания конкретного запроса. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | Строка, | Время события в формате UTC`YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|runId| Строка,| Идентификатор выполнения конвейера. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| Строка, | Идентификатор связанного ресурса фабрики данных. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Строка, | Категория журналов диагностики. Для этого свойства задайте значение PipelineRuns. | `PipelineRuns` |
|уровень| Строка, | Уровень журналов диагностики. Для этого свойства задайте значение Informational. | `Informational` |
|operationName| Строка, |Имя конвейера и его состояние. Pipeline - Succeeded с конечным состоянием при завершении выполнения конвейера.| `MyPipeline - Succeeded` |
|pipelineName| Строка, | Имя конвейера. | `MyPipeline` |
|start| Строка, | Время начала выполнения действия в формате UTC. | `2017-06-26T20:55:29.5007959Z`|
|end| Строка, | Время окончания выполнения действия в формате UTC. Если действие не завершено (журнал диагностики для действия выполняется), устанавливается значение по умолчанию `1601-01-01T00:00:00Z`.  | `2017-06-26T20:55:29.5007959Z` |
|status| Строка, | Конечное состояние выполнения конвейера (Succeeded или Failed). | `Succeeded`|

#### <a name="trigger-run-logs-attributes"></a>Атрибуты журналов запуска триггеров.

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "triggerId":"",
   "resourceId":"",
   "category":"TriggerRuns",
   "level":"Informational",
   "operationName":"",
   "triggerName":"",
   "triggerType":"",
   "triggerEvent":"",
   "start":"",
   "status":"",
   "properties":
   {
      "Parameters": {
        "TriggerTime": "",
       "ScheduleTime": ""
      },
      "SystemParameters": {}
    }
}

```

| Свойство | type | Описание | Пример |
| --- | --- | --- | --- |
| Уровень |Строка, | Уровень журналов диагностики. Задайте уровень 4 для журналов выполнения действий. | `4`  |
| correlationId |Строка, | Уникальный идентификатор для полного отслеживания конкретного запроса. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | Строка, | Время события в формате UTC`YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|triggerId| Строка,| Идентификатор запуска триггера. | `08587023010602533858661257311` |
|resourceId| Строка, | Идентификатор связанного ресурса фабрики данных. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Строка, | Категория журналов диагностики. Для этого свойства задайте значение PipelineRuns. | `PipelineRuns` |
|уровень| Строка, | Уровень журналов диагностики. Для этого свойства задайте значение Informational. | `Informational` |
|operationName| Строка, |Имя триггера с конечным состоянием в случае успешного запуска. MyTrigger - Succeeded при успешном запуске пульса.| `MyTrigger - Succeeded` |
|triggerName| Строка, | Имя триггера. | `MyTrigger` |
|triggerType| Строка, | Тип триггера (ручной триггер или запланированный триггер). | `ScheduleTrigger` |
|triggerEvent| Строка, | Событие триггера. | `ScheduleTime - 2017-07-06T01:50:25Z` |
|start| Строка, | Время запуска триггера в формате UTC. | `2017-06-26T20:55:29.5007959Z`|
|status| Строка, | Конечное состояние запуска триггера (Succeeded или Failed). | `Succeeded`|

### <a name="log-analytics-schema"></a>Схема Log Analytics

Log Analytics наследует схему от Azure Monitor со следующими исключениями:

* Первая буква в имени каждого столбца будет прописной, а для экземпляра *correlationId* в Azure Monitor будет *correlationId* в log Analytics.
* *Уровень* столбца будет удален.
* *Свойства* динамического столбца будут сохранены как следующий динамический тип больших двоичных объектов JSON:

    | Azure Monitor столбец | Log Analytics столбец | Тип |
    | --- | --- | --- |
    | $. Properties. усерпропертиес | усерпропертиес | Динамический |
    | $. Properties. Примечания | Заметки | Динамический |
    | $. Properties. Входной | Ввод | Динамический |
    | $. Properties. Проверки | Вывод | Динамический |
    | $. Properties. Ошибка. errorCode | Код ошибки | ssNoversion |
    | $. Properties. Ошибка. сообщение | ErrorMessage | строка |
    | $. Properties. План | Error | Динамический |
    | $. Properties. Предшественников | Предшественников | Динамический |
    | $. Properties. Вход | Параметры | Динамический |
    | $. Properties. SystemParameters | SystemParameters | Динамический |
    | $. Properties. Тэги | Tags | Динамический |
    
## <a name="metrics"></a>metrics

Azure Monitor позволяет использовать данные телеметрии, чтобы получать сведения о производительности и работоспособности рабочих нагрузок в Azure. Наиболее важным типом данных телеметрии Azure являются метрики (также называемые счетчиками производительности), создаваемые большинством ресурсов Azure. Azure Monitor предоставляет несколько способов настройки и использования этих метрик для мониторинга и устранения неполадок.

ADFV2 выдает следующие метрики:

| **Метрика**           | **Отображаемое имя метрики**         | **Единица измерения** | **Тип агрегирования** | **Описание**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRuns | Метрики успешных выполнений конвейеров. | Count    | Итого                | Общее количество успешных выполнений конвейеров в минутном окне. |
| PipelineFailedRuns   | Метрики неудачных выполнений конвейеров.    | Count    | Итого                | Общее количество неудачных выполнений конвейеров в минутном окне.    |
| ActivitySucceededRuns | Метрики успешных выполнений действий. | Count    | Итого                | Общее количество успешных выполнений действий в минутном окне.  |
| ActivityFailedRuns   | Метрики неудачных выполнений действий.    | Count    | Итого                | Общее количество неудачных выполнений действий в минутном окне.     |
| TriggerSucceededRuns | Метрики успешных запусков триггеров.  | Count    | Итого                | Общее количество успешных запусков триггеров в минутном окне.   |
| TriggerFailedRuns    | Метрики неудачных запусков триггеров.     | Count    | Итого                | Общее количество успешных запусков триггеров в минутном окне.      |

Чтобы получить доступ к метрикам, выполните инструкции в разделе [Azure Monitor Data Platform](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

## <a name="monitor-data-factory-metrics-with-azure-monitor"></a>Мониторинг метрики Фабрики данных с помощью Azure Monitor

Вы можете использовать интеграцию службы "Фабрика данных Azure" с Azure Monitor для передачи данных в Azure Monitor. Это удобно в следующих случаях:

1.  Требуется создавать сложные запросы по обширному набору метрик, публикуемых службой "Фабрика данных" в Azure Monitor. Вы также можете создавать настраиваемые оповещения на основе этих запросов с помощью Azure Monitor.

2.  Требуется отслеживать фабрики данных. Можно направлять данные из нескольких фабрик данных в одну рабочую область Azure Monitor.

Уделите 7 минут своего времени, чтобы просмотреть следующее видео с кратким обзором и демонстрацией этой функции.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

### <a name="configure-diagnostic-settings-and-workspace"></a>Настройка параметров диагностики и рабочей области

Включите параметры диагностики для фабрики данных.

1. На портале перейдите к Azure Monitor и щелкните **параметры диагностики** в меню **Параметры** .

2. Выберите фабрику данных, для которой необходимо настроить параметр диагностики.
    
3. Если в выбранной фабрике данных нет параметров, появится запрос на создание параметра. Щелкните Turn on diagnostics (Включить диагностику).

   ![Добавление параметра диагностики — имеющиеся параметры отсутствуют](media/data-factory-monitor-oms/monitor-oms-image1.png)

   Если в фабрике данных есть существующие параметры, вы увидите список параметров, уже настроенных для этой фабрики данных. Нажмите Add diagnostic setting (Добавить параметр диагностики).

   ![Добавление параметра диагностики — имеющиеся параметры](media/data-factory-monitor-oms/add-diagnostic-setting.png)

4. Введите имя параметра и установите флажок для **Отправить в Log Analytics**, затем выберите рабочую область Log Analytics.

    ![monitor-oms-image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

5. Нажмите кнопку **Сохранить**.

Через несколько секунд новый параметр появится в списке параметров для этой фабрики данных, а журналы диагностики будут передаваться в эту рабочую область сразу после создания данных о событиях. При порождении события и его появлении в Log Analytics может возникать до 15 минут.

> [!NOTE]
> Из-за явного ограничения любой таблицы журнала Azure, не имеющей более 500 столбцов, **настоятельно рекомендуется использовать режим**, зависящий от ресурса. Дополнительные сведения см. в разделе [log Analytics известные ограничения](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-stream-log-store#known-limitation-column-limit-in-azurediagnostics).

### <a name="install-azure-data-factory-analytics-from-azure-marketplace"></a>Установка пакета "Аналитика фабрики данных Azure" из Azure Marketplace

![monitor-oms-image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![monitor-oms-image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Щелкните **Создать** и выберите рабочую область и ее параметры.

![monitor-oms-image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

### <a name="monitor-data-factory-metrics"></a>Мониторинг метрики Фабрики данных

При установке **Аналитики фабрики данных Azure** создается стандартный набор представлений, позволяющий использовать следующие метрики:

- выполнение ADF: 1) Pipeline Runs by Data Factory;

- выполнение ADF: 2) Activity Runs by Data Factory;

- выполнение ADF: 3) Trigger Runs by Data Factory;

- ошибки ADF: 1) Top 10 Pipeline Errors by Data Factory;

- ошибки ADF: 2) Top 10 Activity Errors by Data Factory;

- ошибки ADF: 3) Top 10 Trigger Errors by Data Factory;

- статистика ADF: 1) Activity Runs by Type;

- статистика ADF: 2) Trigger Runs by Type;

- статистика ADF: 3) Max Pipeline Runs Duration.

![monitor-oms-image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![monitor-oms-image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

Можно визуализировать приведенные выше метрики, просматривать запросы, использующие эти метрики, изменять запросы, создавать оповещения и пр.

![monitor-oms-image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="alerts"></a>Предупреждения

Войдите в портал Azure и щелкните Monitor Alerts ( **мониторинг** > **предупреждений** ), чтобы создать оповещения.

![Оповещения в меню портала](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>Создание оповещений

1.  Нажмите кнопку **+Новое правило генерации оповещений**, чтобы создать оповещение.

    ![Новое правило генерации оповещений](media/monitor-using-azure-monitor/alerts_image4.png)

2.  Определите **условия оповещения**.

    > [!NOTE]
    > Выберите значение **Все** для параметра **фильтрации по типу ресурсов**.

    ![Условие оповещения, экран 1 из 3](media/monitor-using-azure-monitor/alerts_image5.png)

    ![Условие оповещения, экран 2 из 3](media/monitor-using-azure-monitor/alerts_image6.png)

    ![Условие оповещения, экран 3 из 3](media/monitor-using-azure-monitor/alerts_image7.png)

3.  Определите **сведения об оповещении**.

    ![Сведения об оповещении](media/monitor-using-azure-monitor/alerts_image8.png)

4.  Определите **группу действий**.

    ![Группа действий, экран 1 из 4](media/monitor-using-azure-monitor/alerts_image9.png)

    ![Группа действий, экран 2 из 4](media/monitor-using-azure-monitor/alerts_image10.png)

    ![Группа действий, экран 3 из 4](media/monitor-using-azure-monitor/alerts_image11.png)

    ![Группа действий, экран 4 из 4](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о мониторинге конвейеров и управлении ими см. в статье [Мониторинг конвейеров и управление ими с помощью программных средств](monitor-programmatically.md).
