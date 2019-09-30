---
title: Переключение с устаревших API оповещений Log Analytics на новые API оповещений Azure
description: Общие сведения об API-интерфейсе предупреждений Log Analytics на основе savedSearch для переключения правил генерации оповещений в новый API Счедуледкуерирулес с подробными сведениями об устранении распространенных проблем клиента.
author: yanivlavi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: yalavi
ms.subservice: alerts
ms.openlocfilehash: 7b3a09c9227110d6dba205987903a2c97dccf1b8
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677792"
---
# <a name="switch-api-preference-for-log-alerts"></a>Переключение предпочтений API для оповещений журнала

> [!NOTE]
> Содержимое, указанное в подходящих для пользователей общедоступном облаке Azure, **не** предназначено для Azure для государственных организаций или Azure для Китая.  

До недавнего времени вы управляли правилами генерации оповещений на портале Microsoft Operations Management Suite. Новые возможности оповещений были интегрированы с различными службами в Microsoft Azure, включая Log Analytics, и мы попросили [расширить ваши правила оповещения с портала OMS в Azure](alerts-extend.md). Но чтобы свести нарушение работы клиентов к минимуму, процессу не удалось изменить программный интерфейс для его потребления в [API оповещений Log Analytics](api-alerts.md) на основе SavedSearch.

Но теперь, предупреждая пользователей, вы объявляете для Log Analytics истинную программную альтернативу Azure (см. в статье [Правила запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)), которая также отражается в разделе [Цены и выставление счетов для оповещений журнала](alerts-unified-log.md#pricing-and-billing-of-log-alerts). Дополнительные сведения об управлении оповещениями журналов с помощью API см. в статье [Управление оповещениями журнала с помощью шаблона ресурсов Azure](alerts-log.md#managing-log-alerts-using-azure-resource-template) и [Управление оповещениями журнала с помощью PowerShell](alerts-log.md#managing-log-alerts-using-powershell).

## <a name="benefits-of-switching-to-new-azure-api"></a>Преимущества коммутации на новый интерфейс API Azure

Существует несколько преимуществ создания и управления оповещениями с помощью [правил запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) над [устаревшими API оповещениями Log Analytics](api-alerts.md). Некоторые из них указаны ниже.

- Возможность [выполнение поиска по журналам по рабочим областях](../log-query/cross-workspace-query.md) в правилах оповещения и расширения внешних ресурсов, например рабочих областей Log Analytics или даже приложения Application Insights.
- При использовании нескольких полей для группировки в запросе с помощью [scheduleQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) пользователь может указать поле для агрегирования на портале Azure.
- Журнал оповещений, созданный с помощью [scheduleQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), может иметь определенный период до 48 часов и получать данные в течение более длительного периода, чем раньше.
- Создание правила генерации оповещений как единый ресурс за один раз без необходимости создавать три уровня ресурсов, как с [устаревшими API оповещения Log Analytics](api-alerts.md).
- Единый программный интерфейс для всех вариантов оповещений журнала на основе запросов в Azure — новые [правила запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), которые можно использовать для управления правилами для Log Analytics, а также для Application Insights.
- Управление оповещениями журналов с помощью [командлетов PowerShell](alerts-log.md#managing-log-alerts-using-powershell)
- Все новые функции оповещений журнала и будущие разработки будут доступны только через новые [правила запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)

## <a name="process-of-switching-from-legacy-log-alerts-api"></a>Процесс коммутации с устаревших оповещений журнала API

Пользователи могут использовать любое [устаревшие оповещение API Log Analytics](api-alerts.md) или новое [правило запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Правила оповещений, созданные любым API, будут управляться *тем же API*, а также порталом Azure. По умолчанию Azure Monitor будет продолжать использовать [устаревшие API предупреждений log Analytics](api-alerts.md) для создания нового правила генерации оповещений из портал Azure для существующих рабочих областей log Analytics. Как [объявлено о новой рабочей области журнала, созданной после 1 июня 2019](https://azure.microsoft.com/updates/switch-api-preference-log-alerts/) г., по умолчанию будет автоматически использовать новый [API счедуледкуерирулес](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) , включая в портал Azure.

Последствия переключения предпочтений API правил запросов по расписанию скомпилированы ниже:

- Все взаимодействия, выполняемые для управления оповещениями журнала с помощью программного интерфейса, теперь должны выполняться с использованием [правил запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Дополнительные сведения см. в статьях [Пример использования с помощью шаблона ресурсов Azure](alerts-log.md#managing-log-alerts-using-azure-resource-template) и [Пример использования](alerts-log.md#managing-log-alerts-using-powershell) с помощью PowerShell.
- Любое новое правило генерации оповещений журнала, созданное на портале Azure, будет создано с использованием [правил запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) и позволит пользователям также использовать [дополнительную функциональность нового API](#benefits-of-switching-to-new-azure-api) через портал Azure.
- Серьезность для правил генерации оповещений журнала будет смещена с: *Критическое, предупреждение & информационное* *значение для серьезности 0, 1 & 2*. Вместе с параметром для создания или обновления правил генерации оповещений с уровнем серьезности 3 и 4.

Процесс перемещения правил генерации оповещений из [устаревших оповещений API Log Analytics](api-alerts.md) не затрагивает определение, запрос или конфигурацию оповещения каким-либо образом. Правила оповещений и мониторинг не затрагиваются, и предупреждения не будут остановлены или остановлены во время или после переключения. Единственное изменение — изменение предпочтений API и доступ к правилам через новый API.

> [!NOTE]
> После того как пользователь перейдет к новому [интерфейсу API счедуледкуерирулес](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), вы не сможете вернуться назад или вернуться к использованию старого API- [интерфейса предупреждений устаревших log Analytics](api-alerts.md).

Любой клиент, который хочет добровольно переключиться на новые [правила запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) и заблокировать использование из [устаревшего оповещения API Log Analytics](api-alerts.md), может сделать это, выполнив вызов PUT в указанном ниже API для переключения всех правил оповещения, связанных с определенной рабочей областью Log Analytics.

```
PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

С текстом запроса, который содержит указанный ниже JSON.

```json
{
    "scheduledQueryRulesEnabled" : true
}
```

Доступ к API также можно получить из командной строки PowerShell с помощью [ARMClient](https://github.com/projectkudu/ARMClient), инструмента программы командной строки с открытым кодом, который упрощает вызов API Azure Resource Manager. Как показано ниже, в примере вызова PUT используется инструмент ARMclient для переключения всех правил генерации оповещений, связанных с конкретной рабочей областью Log Analytics.

```powershell
$switchJSON = '{"scheduledQueryRulesEnabled": "true"}'
armclient PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $switchJSON
```

Если переключение всех правил генерации оповещений в рабочей области Log Analytics на использование новых [правил запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) выполнено успешно, предоставляется следующий ответ.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```

Пользователи также могут проверить текущее состояние вашей рабочей области Log Analytics и посмотреть, была ли она переключена на использование только [правил запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Чтобы проверить, могут ли пользователи выполнить вызов GET на API, указанный ниже.

```
GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Чтобы выполнить указанное выше с помощью командной строки PowerShell, используя инструмент [ARMClient](https://github.com/projectkudu/ARMClient), см. пример ниже.

```powershell
armclient GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Если указанная рабочая область Log Analytics переключена на использование только [правил запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), тогда ответ JSON будет указан в списке, указанном ниже.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```
В противном случае, если указанное рабочее пространство Log Analytics еще не переключено на использование только [правил запросов по расписанию](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules); тогда ответ JSON будет таким, как указано ниже.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : false
}
```

## <a name="next-steps"></a>Следующие шаги

- Дополнительные сведения см. в статье [Оповещения журнала в Azure Monitor](alerts-unified-log.md).
- Узнайте о том, как создать [журнал оповещений в оповещениях Azure](alerts-log.md).
- Дополнительные сведения об [оповещениях Azure](../../azure-monitor/platform/alerts-overview.md).
