---
title: Действия веб-перехватчика для оповещений журнала в оповещениях Azure
description: В этой статье описывается, как создать правило генерации оповещений журнала с помощью Log Analytics рабочей области или Application Insights, как оповещение отправляет данные в виде веб-перехватчика HTTP, а также сведения о различных возможных настройках.
author: yanivlavi
ms.author: yalavi
services: monitoring
ms.topic: conceptual
ms.date: 06/25/2019
ms.subservice: alerts
ms.openlocfilehash: 3311819f021533a28a41daf2c2f08193218fae96
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "87075277"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Действия веб-перехватчика для правил оповещений журнала
При [создании оповещения журнала в Azure](alerts-log.md)можно [настроить его с помощью групп действий](action-groups.md) для выполнения одного или нескольких действий. В этой статье описаны различные действия веб-перехватчика, а также показано, как настроить пользовательский интерфейс, основанный на JSON.

> [!NOTE]
> Вы также можете использовать [общую схему предупреждений](https://aka.ms/commonAlertSchemaDocs) для интеграции веб-перехватчика. Общая схема предупреждений предоставляет преимущества единого расширяемого и унифицированного набора полезных данных оповещений во всех службах предупреждений в Azure Monitor. Обратите внимание, что общая схема предупреждений не хонаур пользовательский параметр JSON для оповещений журнала. Он откладывается на общие полезные данные схемы предупреждений, если он выбран независимо от настройки, которая могла быть выполнена на уровне правила оповещения. [Сведения об общих определениях схемы предупреждений.](https://aka.ms/commonAlertSchemaDefinitions)

## <a name="webhook-actions"></a>Действия webhook

С помощью действий веб-перехватчика можно вызывать внешний процесс через один запрос HTTP POST. Вызываемая служба должна поддерживать веб-перехватчики и определять, как использовать полученные полезные данные.

Для выполнения действий webhook требуются свойства, приведенные в таблице ниже.

| Свойство | Описание: |
|:--- |:--- |
| **URL-адрес Webhook** |URL-адрес действия webhook. |
| **Настраиваемые полезные данные JSON** |Пользовательские полезные данные для отправки с веб-перехватчиком, если этот параметр выбран во время создания предупреждения. Дополнительные сведения см. в разделе [Управление оповещениями журнала](alerts-log.md).|

> [!NOTE]
> Кнопка **Просмотр веб-перехватчика** рядом с параметром **включить пользовательские полезные данные JSON для веб-перехватчика** для оповещения журнала содержит образец полезных данных веб-перехватчика для предоставленной настройки. Он не содержит фактических данных, но является репрезентативной схемой JSON, используемой для оповещений журнала. 

Веб-перехватчики включают в себя URL-адрес и полезные данные, отформатированные в формате JSON, которые передаются во внешнюю службу. По умолчанию полезные данные включают в себя значения из приведенной ниже таблицы. Такие полезные данные можно заменить на собственные. В этом случае используйте переменные в таблице для каждого параметра, чтобы включить их значения в пользовательские полезные данные.


| Параметр | Переменная | Описание |
|:--- |:--- |:--- |
| *алертруленаме* |#alertrulename |Имя правила генерации оповещений. |
| *Уровень серьезности* |#severity |Уровень серьезности, установленный для срабатывающего оповещения журнала. |
| *AlertThresholdOperator* |#thresholdoperator |Оператор threshold для правила генерации оповещений, которое использует больше или меньше. |
| *AlertThresholdValue* |#thresholdvalue |Значение порога для правила генерации оповещений. |
| *линктосеарчресултс* |#linktosearchresults |Ссылка на портал аналитики, который возвращает записи из запроса, создавшего предупреждение. |
| *линктосеарчресултсапи* |#linktosearchresultsapi |Ссылка на API аналитики, который возвращает записи из запроса, создавшего предупреждение. |
| *линктофилтередсеарчресултсуи* |#linktofilteredsearchresultsui |Ссылка на портал аналитики, который возвращает записи из запроса, отфильтрованного по значениям измерений, создавшего предупреждение. |
| *линктофилтередсеарчресултсапи* |#linktofilteredsearchresultsapi |Ссылка на API аналитики, который возвращает записи из запроса, отфильтрованного по значениям измерений, создавшего предупреждение. |
| *ResultCount* |#searchresultcount |Число записей в результатах поиска. |
| *Время окончания интервала поиска* |#searchintervalendtimeutc |Время окончания запроса в формате UTC с форматом mm/дд/гггг чч: мм: сс AM/PM. |
| *Интервал поиска* |#searchinterval |Временное окно для правила генерации оповещений с форматом чч: мм: СС. |
| *Время начала интервала поиска* |#searchintervalstarttimeutc |Время начала запроса в формате UTC с форматом mm/дд/гггг чч: мм: сс AM/PM. 
| *SearchQuery* |#searchquery |Запрос поиска журналов, используемый правилом генерации оповещений. |
| *SearchResults* |"IncludeSearchResults": true|Записи, возвращаемые запросом в виде таблицы JSON, ограничены первыми записями 1 000. "IncludeSearchResults": значение true добавляется в определение пользовательского веб-перехватчика JSON в качестве свойства верхнего уровня. |
| *Измерения* |"Инклудедименсионс": true|Сочетания значений измерений, которые активируют предупреждение как раздел JSON. "Инклудедименсионс": значение true добавляется в определение пользовательского веб-перехватчика JSON в качестве свойства верхнего уровня. |
| *Тип оповещения*| #alerttype | Тип правила оповещения журнала, настроенного как [измерение метрик](alerts-unified-log.md#metric-measurement-alert-rules) или [Количество результатов](alerts-unified-log.md#number-of-results-alert-rules).|
| *WorkspaceID* |#workspaceid |Идентификатор рабочей области Log Analytics. |
| *Идентификатор приложения* |#applicationid |Идентификатор приложения Application Insights. |
| *Идентификатор подписки* |#subscriptionid |ИДЕНТИФИКАТОР используемой подписки Azure. 

> [!NOTE]
> Предоставленные ссылки передают параметры, такие как *SearchQuery*, *интервал поиска StartTime*и *время окончания интервала поиска* , в URL-адресе портал Azure или API.

Например, можно указать следующие пользовательские полезные данные, содержащие один параметр — *text*. Служба, которую вызывает этот веб-перехватчик, требует этот параметр.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
В этом примере полезная нагрузка разрешается примерно следующим образом при отправке в веб-перехватчик:

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Так как все переменные в пользовательском веб-перехватчике должны быть указаны в корпусе JSON, например "#searchinterval, у результирующего веб-перехватчика также есть переменные данные внутри корпуса, например" 00:05:00 ".

Чтобы включить результаты поиска в пользовательские полезные данные, убедитесь, что **IncludeSearchResults** задан как свойство верхнего уровня в полезных данных JSON. 

## <a name="sample-payloads"></a>Примеры полезных данных
В этом разделе показаны примеры полезных данных для веб-перехватчиков для оповещений журнала. Примерами полезных данных являются примеры, когда полезные данные являются стандартными и когда они являются пользовательскими.

### <a name="standard-webhook-for-log-alerts"></a>Стандартный веб-перехватчик для оповещений журнала 
Оба этих примера имеют фиктивную полезную нагрузку с двумя столбцами и двумя строками.

#### <a name="log-alert-for-log-analytics"></a>Оповещение журнала для Log Analytics
Ниже приведен пример полезных данных для стандартного действия веб-перехватчика *без настраиваемого параметра JSON* , используемого для оповещений на основе log Analytics:

```json
{
    "SubscriptionId": "12345a-1234b-123c-123d-12345678e",
    "AlertRuleName": "AcmeRule",
    "SearchQuery": "Perf | where ObjectName == \"Processor\" and CounterName == \"% Processor Time\" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "LinkToFilteredSearchResultsUI": "https://portal.azure.com/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "LinkToSearchResultsAPI": "https://api.loganalytics.io/v1/workspaces/workspaceID/query?query=Heartbeat&timespan=2020-05-07T18%3a11%3a51.0000000Z%2f2020-05-07T18%3a16%3a51.0000000Z",
    "LinkToFilteredSearchResultsAPI": "https://api.loganalytics.io/v1/workspaces/workspaceID/query?query=Heartbeat&timespan=2020-05-07T18%3a11%3a51.0000000Z%2f2020-05-07T18%3a16%3a51.0000000Z",
    "Description": "log alert rule",
    "Severity": "Warning",
    "AffectedConfigurationItems": [
        "INC-Gen2Alert"
    ],
    "Dimensions": [
        {
            "name": "Computer",
            "value": "INC-Gen2Alert"
        }
    ],
    "SearchResult": {
        "tables": [
            {
                "name": "PrimaryResult",
                "columns": [
                    {
                        "name": "$table",
                        "type": "string"
                    },
                    {
                        "name": "Computer",
                        "type": "string"
                    },
                    {
                        "name": "TimeGenerated",
                        "type": "datetime"
                    }
                ],
                "rows": [
                    [
                        "Fabrikam",
                        "33446677a",
                        "2018-02-02T15:03:12.18Z"
                    ],
                    [
                        "Contoso",
                        "33445566b",
                        "2018-02-02T15:16:53.932Z"
                    ]
                ]
            }
        ]
    },
    "WorkspaceId": "12345a-1234b-123c-123d-12345678e",
    "AlertType": "Metric measurement"
}
 ```

> [!NOTE]
> Значение поля "серьезность" может измениться, если вы [изменили настройки API](alerts-log-api-switch.md) для оповещений журнала на log Analytics.


#### <a name="log-alert-for-application-insights"></a>Оповещение журнала для Application Insights
Ниже приведен пример полезных данных для стандартного веб-перехватчика *без пользовательского параметра JSON* , когда он используется для оповещений журнала на основе Application Insights:
    
```json
{
    "schemaId": "Microsoft.Insights/LogAlert",
    "data": {
        "SubscriptionId": "12345a-1234b-123c-123d-12345678e",
        "AlertRuleName": "AcmeRule",
        "SearchQuery": "requests | where resultCode == \"500\" | summarize AggregatedValue = Count by bin(Timestamp, 5m), IP",
        "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
        "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
        "AlertThresholdOperator": "Greater Than",
        "AlertThresholdValue": 0,
        "ResultCount": 2,
        "SearchIntervalInSeconds": 3600,
        "LinkToSearchResults": "https://portal.azure.com/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
        "LinkToFilteredSearchResultsUI": "https://portal.azure.com/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
        "LinkToSearchResultsAPI": "https://api.applicationinsights.io/v1/apps/0MyAppId0/metrics/requests/count",
        "LinkToFilteredSearchResultsAPI": "https://api.applicationinsights.io/v1/apps/0MyAppId0/metrics/requests/count",
        "Description": null,
        "Severity": "3",
        "Dimensions": [
            {
                "name": "IP",
                "value": "1.1.1.1"
            }
        ],
        "SearchResult": {
            "tables": [
                {
                    "name": "PrimaryResult",
                    "columns": [
                        {
                            "name": "$table",
                            "type": "string"
                        },
                        {
                            "name": "Id",
                            "type": "string"
                        },
                        {
                            "name": "Timestamp",
                            "type": "datetime"
                        }
                    ],
                    "rows": [
                        [
                            "Fabrikam",
                            "33446677a",
                            "2018-02-02T15:03:12.18Z"
                        ],
                        [
                            "Contoso",
                            "33445566b",
                            "2018-02-02T15:16:53.932Z"
                        ]
                    ]
                }
            ]
        },
        "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1",
        "AlertType": "Metric measurement"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Оповещение журнала с пользовательскими полезными данными JSON
Например, чтобы создать пользовательские полезные данные, включающие только имя предупреждения и результаты поиска, можно использовать следующее: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Ниже приведен пример полезных данных для настраиваемого действия веб-перехватчика для любого оповещения журнала.
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResults":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Дальнейшие действия
- Сведения об [оповещениях журнала в оповещениях Azure](alerts-unified-log.md).
- Узнайте, как [управлять оповещениями журнала в Azure](alerts-log.md).
- Создание [групп действий в Azure](action-groups.md)и управление ими.
- Дополнительные сведения об [Application Insights](../log-query/log-query-overview.md).
- Дополнительные сведения о [запросах журналов](../log-query/log-query-overview.md). 
