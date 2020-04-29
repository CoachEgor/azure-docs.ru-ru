---
title: Выражение app() в запросах журнала Azure Monitor | Документация Майкрософт
description: Выражение приложения используется в Azure Monitor запросе журнала для получения данных из определенного приложения Application Insights в той же группе ресурсов, в другой группе ресурсов или в другой подписке.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/25/2019
ms.openlocfilehash: 5502df1cd119c0f63c65945d73431a17282ebc0c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "77670266"
---
# <a name="app-expression-in-azure-monitor-query"></a>Выражение app() в запросах Azure Monitor

Выражение `app` используется в запросах Azure Monitor для получения данных из определенного приложения Application Insights, находящегося в той же или другой группе ресурсов либо в другой подписке. Его используют для добавления данных приложения в запрос журнала Azure Monitor и запрашивания данных из нескольких приложений с помощью запроса Application Insights.



## <a name="syntax"></a>Синтаксис

`app(`*Идентификатор*`)`


## <a name="arguments"></a>Аргументы

- *Идентификатор* идентифицирует приложение с помощью одного форматов, представленных в таблице ниже.

| Идентификатор | Описание | Пример
|:---|:---|:---|
| Имя ресурса | Понятное для человека имя приложения (или имя компонента) | app("fabrikamapp") |
| Полное имя | Полное имя приложения в формате subscriptionName/resourceGroup/componentName | app('AI-Prototype/Fabrikam/fabrikamapp') |
| ID | GUID приложения | app("988ba129-363e-4415-8fe7-8cbab5447518") |
| Идентификатор ресурса Azure | Идентификатор ресурса Azure |app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp") |


## <a name="notes"></a>Примечания

* У вас должен быть доступ на чтение приложения.
* Для идентификации приложения по имени его имя должно быть уникальным во всех доступных подписках. Если у вас есть несколько приложений с таким именем, запрос не будет выполнен из-за неоднозначности. В этом случае необходимо воспользоваться другим идентификатором.
* Связанное выражение [workspace](workspace-expression.md) используется для запрашивания данных из рабочих областей Log Analytics.
* Выражение app() сейчас не поддерживается в поисковом запросе при использовании портала Azure для создания [правила генерации оповещений о пользовательском поиске по журналам](../platform/alerts-log.md), если только приложение Application Insights не используется как ресурс для правила генерации оповещений.

## <a name="examples"></a>Примеры

```Kusto
app("fabrikamapp").requests | count
```
```Kusto
app("AI-Prototype/Fabrikam/fabrikamapp").requests | count
```
```Kusto
app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count
```
```Kusto
app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о рабочей области Log Analytics см. в статье [Выражение workspace() в запросах Log Analytics](workspace-expression.md).
- Подробнее о хранении данных Azure Monitor см. в статье [Анализ данных Log Analytics в Azure Monitor](../../azure-monitor/log-query/log-query-overview.md).
- Ознакомьтесь с полной документацией по [языку запросов Kusto](/azure/kusto/query/).
