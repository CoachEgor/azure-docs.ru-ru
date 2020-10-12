---
title: Агрегирование в запросах к журналам Azure Monitor | Документация Майкрософт
description: Здесь описаны функции агрегирования в запросах к журналам Azure Monitor, предоставляющие эффективные способы анализа данных.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: d164c53e7e2be55f3cede389901a256ba388808d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "77670310"
---
# <a name="aggregations-in-azure-monitor-log-queries"></a>Агрегирование в запросах к журналам Azure Monitor

> [!NOTE]
> Прежде чем приступить к этому уроку, необходимо ознакомиться со статьями [Начало работы с порталом аналитики](get-started-portal.md) и [Начало работы с запросами](get-started-queries.md).

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

В этой статье описаны функции агрегирования в запросах к журналам Azure Monitor, предоставляющие эффективные способы анализа данных. Все эти функции работают с помощью оператора `summarize`, который выдает таблицу с агрегированными результатами из входной таблицы.

## <a name="counts"></a>Подсчеты

### <a name="count"></a>count
Выполните подсчет количества строк в результирующем наборе после применения фильтров. Следующий пример возвращает общее количество строк в таблице _Perf_ за последние 30 минут. Результат возвращается в столбец под именем *count_*, если присвоено конкретное имя:


```Kusto
Perf
| where TimeGenerated > ago(30m) 
| summarize count()
```

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| summarize num_of_records=count() 
```

Чтобы просмотреть тенденцию по времени, можно использовать визуализацию временной диаграммы:

```Kusto
Perf 
| where TimeGenerated > ago(30m) 
| summarize count() by bin(TimeGenerated, 5m)
| render timechart
```

В результате этого примера показана линия тенденции количества записей perf с интервалами в 5 минут:

![Тенденция подсчета](media/aggregations/count-trend.png)


### <a name="dcount-dcountif"></a>dcount, dcountif
Используйте `dcount` и `dcountif`, чтобы подсчитать уникальные значения в определенном столбце. Следующий запрос вычисляет, сколько отдельных компьютеров отправили пакеты пульса за последний час:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcount(Computer)
```

Чтобы подсчитать только компьютеры Linux, отправлявшие пакеты пульса, используйте `dcountif`:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcountif(Computer, OSType=="Linux")
```

### <a name="evaluating-subgroups"></a>Оценка подгрупп
Чтобы выполнить подсчет или другие агрегирования с подгруппами в данных, используйте ключевое слово `by`. Например, чтобы подсчитать количество уникальных компьютеров Linux, которые отправляли пульсы в каждой стране или регионе, сделайте следующее:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry
```

|RemoteIPCountry  | distinct_computers  |
------------------|---------------------|
|США    | 19                  |
|Канада           | 3                   |
|Ирландия          | 0                   |
|Соединенное Королевство   | 0                   |
|Нидерланды      | 2                   |


Чтобы проанализировать меньшие подгруппы данных, добавьте имена дополнительных столбцов в раздел `by`. Например, может потребоваться подсчитать различные компьютеры из каждой страны или региона на OSType:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry, OSType
```

## <a name="percentiles-and-variance"></a>Процентили и дисперсия
При вычислении числовых значений распространенной практикой является их усреднение с помощью `summarize avg(expression)`. На средние значения влияют крайние значения, характерные только в нескольких случаях. Чтобы устранить эту проблему, можно использовать менее чувствительные функции, например `median` или `variance`.

### <a name="percentile"></a>Процентиль
Чтобы найти медиану, используйте функцию `percentile` со значением, чтобы указать процентиль:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 50) by Computer
```

Вы также можете указать различные процентили, чтобы получить агрегированный результат для каждого из них:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 25, 50, 75, 90) by Computer
```

Таким образом можно определить, что некоторые ЦП имеют похожие медианы, но различные условия: в некоторых показатели стабильны возле медианы, а в других определяются более низкие и высокие значения ЦП. Это означает, что ЦП испытал пиковые нагрузки.

### <a name="variance"></a>Variance
Чтобы напрямую рассчитать дисперсию значения, используйте стандартное отклонение и методы дисперсии:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), variance(CounterValue) by Computer
```

Хороший способ проанализировать стабильность загрузки ЦП — объединить значение stdev с вычислением медианы:

```Kusto
Perf
| where TimeGenerated > ago(130m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), percentiles(CounterValue, 50) by Computer
```

Ознакомьтесь с другими статьями по использованию [языка запросов Kusto](/azure/kusto/query/) с данными журналов Azure Monitor.

- [Работа со строками](string-operations.md)
- [Работа со значениями даты и времени](datetime-operations.md)
- [Расширенные статистические функции в запросах Azure Log Analytics](advanced-aggregations.md)
- [Работа с JSON и структурами данных в запросах Log Analytics](json-data-structures.md)
- [Составление расширенных запросов](advanced-query-writing.md)
- [Joins](joins.md)
- [Создание графиков](charts.md)
