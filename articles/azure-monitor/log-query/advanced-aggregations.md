---
title: Расширенное агрегирование в запросах журнала Azure Monitor | Документация Майкрософт
description: В этой статье описываются некоторые более расширенные параметры агрегирования, доступные для запросов журнала Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: dba058dce09e958a2ae769d927a5569fb3e42113
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87324562"
---
# <a name="advanced-aggregations-in-azure-monitor-log-queries"></a>Расширенное агрегирование в запросах журнала Azure Monitor

> [!NOTE]
> Прежде чем приступать к работе с этим руководством, ознакомьтесь со статьей [Aggregations in Azure Monitor log queries](./aggregations.md) (Агрегирование в запросах журнала Azure Monitor).

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

В этой статье описываются некоторые более сложные параметры агрегирования, доступные для запросов Azure Monitor.

## <a name="generating-lists-and-sets"></a>Создание списков и наборов
Вы можете использовать `makelist`, чтобы свести данные по порядку значений в отдельном столбце. Например, вы можете изучить наиболее распространенные события на компьютерах по порядку. Вы можете сводить данные по порядку идентификаторов событий на каждом компьютере. 

```Kusto
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makelist(EventID) by Computer
```

|Компьютер|list_EventID|
|---|---|
| computer1 | [704,701,1501,1500,1085,704,704,701] |
| computer2 | [326,105,302,301,300,102] |
| ... | ... |

`makelist` формирует список в порядке, в котором были переданы данные. Чтобы отсортировать события от старых к новым, используйте `asc` в операторе порядка, а не `desc`. 

Также эффективно создавать список только уникальных значений. Такой список называется _набором_, и его можно создать с помощью `makeset`:

```Kusto
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makeset(EventID) by Computer
```

|Компьютер|list_EventID|
|---|---|
| computer1 | [704,701,1501,1500,1085] |
| computer2 | [326,105,302,301,300,102] |
| ... | ... |

Как и `makelist`, `makeset` также работает с упорядоченными данными и генерирует массивы на основе порядка переданных строк.

## <a name="expanding-lists"></a>Развертывание списков
Обратной операцией `makelist` или `makeset` является инструкция `mvexpand`, которая расширяет список значений для разделения строк. Ее можно развернуть в любом количестве динамических столбцов — в формате JSON и массива. Например, вы можете просканировать таблицу *пульса* для определения решений, отправляющих данные с компьютеров, которые отправили пакет пульса за последний час:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, Solutions
```

| Компьютер | Решения | 
|--------------|----------------------|
| computer1 | "security", "updates", "changeTracking" |
| computer2 | "security", "updates" |
| computer3 | "antiMalware", "changeTracking" |
| ... | ... |

Используйте `mvexpand`, чтобы показать каждое значение в отдельной строке вместо списка, разделенного запятыми:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
```

| Компьютер | Решения | 
|--------------|----------------------|
| computer1 | "security" |
| computer1 | "updates" |
| computer1 | "changeTracking" |
| computer2 | "security" |
| computer2 | "updates" |
| computer3 | "antiMalware" |
| computer3 | "changeTracking" |
| ... | ... |


Затем вы можете использовать `makelist` снова, чтобы сгруппировать элементы. Теперь вы можете просмотреть список компьютеров для каждого решения:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
| summarize makelist(Computer) by tostring(Solutions) 
```

|Решения | list_Computer |
|--------------|----------------------|
| "security" | ["computer1", "computer2"] |
| "updates" | ["computer1", "computer2"] |
| "changeTracking" | ["computer1", "computer3"] |
| "antiMalware" | ["computer3"] |
| ... | ... |

## <a name="handling-missing-bins"></a>Обработка отсутствующих ячеек
Полезное приложение `mvexpand` — это необходимость заполнения значений по умолчанию в для отсутствующих ячеек. Например, предположим, что вы ищете время бесперебойной работы определенной машины, изучая его пульс. Также вы хотите просмотреть источник пульса, который находится в столбце _категории_. Как правило, мы используем простой оператор суммирования следующим образом:

```Kusto
Heartbeat
| where TimeGenerated > ago(12h)
| summarize count() by Category, bin(TimeGenerated, 1h)
```

| Категория | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Direct Agent | 2017-06-06T17:00:00Z | 15 |
| Direct Agent | 2017-06-06T18:00:00Z | 60 |
| Direct Agent | 2017-06-06T20:00:00Z | 55 |
| Direct Agent | 2017-06-06T21:00:00Z | 57 |
| Direct Agent | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |

Однако, в этих результатах контейнер, связанный с "2017-06-06T19:00:00Z", отсутствует, так как отсутствуют данные пульса для этого времени. Используйте функцию `make-series`, чтобы присвоить значение по умолчанию пустым контейнерам. Это приведет к созданию строки для каждой категории с двумя дополнительными столбцами массива, одним для значений и одним для сопоставления контейнеров по времени:

```Kusto
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
```

| Категория | count_ | TimeGenerated |
|---|---|---|
| Direct Agent | [15,60,0,55,60,57,60,...] | ["2017-06-06T17:00:00.0000000Z","2017-06-06T18:00:00.0000000Z","2017-06-06T19:00:00.0000000Z","2017-06-06T20:00:00.0000000Z","2017-06-06T21:00:00.0000000Z",...] |
| ... | ... | ... |

Третий элемент массива *count_*, как и ожидалось, равен 0, и есть соответствующая метка времени "2017-06-06T19:00:00.0000000Z" в массиве _TimeGenerated_. Однако этот формат массива трудно прочитать. Используйте `mvexpand`, чтобы развернуть массивы и создать выходные данные того же формата, что и сгенерированные `summarize`:

```Kusto
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
| mvexpand TimeGenerated, count_
| project Category, TimeGenerated, count_
```

| Категория | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Direct Agent | 2017-06-06T17:00:00Z | 15 |
| Direct Agent | 2017-06-06T18:00:00Z | 60 |
| Direct Agent | 2017-06-06T19:00:00Z | 0 |
| Direct Agent | 2017-06-06T20:00:00Z | 55 |
| Direct Agent | 2017-06-06T21:00:00Z | 57 |
| Direct Agent | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |



## <a name="narrowing-results-to-a-set-of-elements-let-makeset-toscalar-in"></a>Сужение результатов до набора элементов: `let`, `makeset`, `toscalar`, `in`
Распространенным сценарием является выбор имен некоторых конкретных сущностей на основе набора критериев, а затем фильтрация другого набора данных до этого набора сущностей. Например, вы можете найти компьютеры, у которых отсутствуют обновления, и определить IP-адреса, для которых вызываются эти компьютеры:


```Kusto
let ComputersNeedingUpdate = toscalar(
    Update
    | summarize makeset(Computer)
    | project set_Computer
);
WindowsFirewall
| where Computer in (ComputersNeedingUpdate)
```

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с другими статьями по использованию [языка запросов Kusto](/azure/kusto/query/) с данными журналов Azure Monitor.

- [Работа со строками](string-operations.md)
- [Работа со значениями даты и времени](datetime-operations.md)
- [Статистические функции в запросах Log Analytics](aggregations.md)
- [Расширенные статистические функции в запросах Azure Log Analytics]()
- [Работа с JSON и структурами данных в запросах Log Analytics](json-data-structures.md)
- [Joins](joins.md)
- [Создание графиков](charts.md)

