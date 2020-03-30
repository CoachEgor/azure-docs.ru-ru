---
title: Анализ событий Azure Service Fabric с журналами Azure Monitor
description: Узнайте о визуализации и анализе событий с помощью журналов Azure Monitor для мониторинга и диагностики кластеров Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: srrengar
ms.openlocfilehash: 40dd930aa21e3056d5ecc908359215d6874ed8ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464735"
---
# <a name="event-analysis-and-visualization-with-azure-monitor-logs"></a>Анализ событий и визуализация с помощью журналов Azure Monitor
 Azure Monitor собирает и анализирует данные телеметрии от приложений и служб, размещенных в облаке, и предоставляет средства анализа, с помощью которых вы сможете максимально увеличить их доступность и производительность. В этой статье описывается, как запускать запросы в журналах Azure Monitor, чтобы получить информацию и устранить неполадки, что происходит в кластере. Рассматриваются следующие распространенные вопросы:

* Как устранить неполадки событий работоспособности?
* Как узнать, что узел вышел из строя?
* Как узнать, запущены или остановлены ли службы приложения?

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview-of-the-log-analytics-workspace"></a>Обзор рабочего пространства анализа журналов

>[!NOTE] 
>Хранилище диагностики по умолчанию включено в кластере во время создания, но вам нужно настроить рабочую область Log Analytics для чтения из хранилища диагностики.

Журналы Azure Monitor собирают данные из управляемых ресурсов, включая таблицу хранения Azure или агента, и сохраняют их в центральном репозитории. Эти данные далее могут использоваться для анализа, оповещения, визуализации или последующего экспорта. Журналы Azure Monitor поддерживают события, данные о производительности или любые другие пользовательские данные. Ознакомьтесь с [шагами по настройке расширения диагностики для агрегирования событий](service-fabric-diagnostics-event-aggregation-wad.md) и [шагов по созданию рабочего пространства analytics журнала для чтения из событий в хранилище,](service-fabric-diagnostics-oms-setup.md) чтобы убедиться, что данные перетекают в журналы Azure Monitor.

После получения данных журналами Azure Monitor у Azure есть несколько *решений для мониторинга,* которые являются расфасованными решениями или рабочими панелями мониторинга для мониторинга входящих данных, настроенных по нескольким сценариям. К ним относятся *Аналитика Service Fabric* и *Контейнеры* — два наиболее важных решения для диагностики и мониторинга кластеров Service Fabric. В этой статье описывается использование решения "Аналитика Service Fabric", созданного с помощью рабочей области.

## <a name="access-the-service-fabric-analytics-solution"></a>Получение доступа к решению "Аналитика Service Fabric"

На [портале Azure](https://portal.azure.com)перейдите к группе ресурсов, в которой вы создали решение Service Fabric Analytics.

Выберите ресурс **ServiceFabric\<имя_рабочей_области_OMS\>**.

В `Summary` вы увидите плитки в форме графа для каждого включенного решения, включая одну для Service Fabric. Щелкните граф **Service Fabric**, чтобы продолжить решение службы Fabric Analytics.

![Решение Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

На приведенном ниже рисунке показана домашняя страница решения "Аналитика Service Fabric". На этой домашней странице представлена информация, касающаяся работы вашего кластера.

![Решение Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

 Если включить диагностику во время создания кластера, можно просмотреть такие события: 

* [события кластера Service Fabric;](service-fabric-diagnostics-event-generation-operational.md)
* [События модели программирования на основе Reliable Actors](service-fabric-reliable-actors-diagnostics.md).
* [События модели программирования на основе Reliable Services](service-fabric-reliable-services-diagnostics.md).

>[!NOTE]
>Помимо готовых событий Service Fabric, можно собирать дополнительные системные события. Для этого [обновите файл конфигурации расширения диагностики](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

## <a name="view-service-fabric-events-including-actions-on-nodes"></a>Просмотр событий Service Fabric, включая действия на узлах

На странице аналитики Service Fabric щелкните диаграмму для **События Service Fabric**.

![Операционный канал решения Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

Щелкните **Список**, чтобы просмотреть список событий. Здесь вы увидите все собранные системные события. Для справки, они из **WADServiceFabricSystemEventsTable** в учетной записи хранения Azure, и аналогичным образом надежные службы и события субъектов вы видите далее из этих соответствующих таблиц.
    
![Операционный канал запроса](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

Кроме того, чтобы найти необходимые данные, можно щелкнуть значок лупы в левой части экрана и воспользоваться языком запросов Kusto. Например, чтобы найти все действия, выполняемые в узлах кластера, можно использовать приведенный ниже запрос. Идобыты событий, используемые ниже, приведены в [ссылке на события оперативного канала.](service-fabric-diagnostics-event-generation-operational.md)

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Вы можете запрашивать много дополнительных полей (например, конкретные узлы (Компьютер) и системную службу (TaskName)).

## <a name="view-service-fabric-reliable-service-and-actor-events"></a>Просмотр событий служб Reliable Services и субъектов Reliable Actors в Service Fabric

На странице аналитики Service Fabric щелкните диаграмму для **Reliable Services**.

![Reliable Services в решении Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

Щелкните **Список**, чтобы просмотреть список событий. Здесь можно просмотреть события из служб Reliable Services. Когда служба RunAsync запускается и завершается, могут отображаться различные события. Обычно это происходит при развертывании и обновлении. 

![Запрос к Reliable Services](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Аналогичным образом можно просмотреть события субъектов Reliable Actors. Чтобы настроить более подробные события для субъектов Reliable Actors, необходимо изменить `scheduledTransferKeywordFilter` в конфигурации для расширения диагностики (см. ниже). Подробная информация о значениях для них в [надежных субъектов событий ссылки](service-fabric-reliable-actors-diagnostics.md#keywords).

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

Язык запросов Kusto предоставляет широкие возможности. Вы можете выполнить другой полезный запрос, чтобы узнать, какие узлы создают больше событий. Запрос на снимке экрана, приведенном ниже, показывает операционное событие Service Fabric агрегированное с конкретной службой и узлом.

![События запросов на каждом узле](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Дальнейшие действия

* Чтобы включить мониторинг инфраструктуры (т. е. счетчики производительности), перейдите к разделу о [добавлении агента Log Analytics](service-fabric-diagnostics-oms-agent.md). Агент собирает счетчики производительности и добавляет их в имеющуюся рабочую область.
* Для локтевых кластеров журналы Azure Monitor предлагают шлюз (HTTP Forward Proxy), который можно использовать для отправки данных в журналы Azure Monitor. Подробнее об этом читайте в [материале "Подключение компьютеров без доступа в Интернет к журналам Azure Monitor с помощью шлюза Log Analytics".](../azure-monitor/platform/gateway.md)
* Настройте [автоматические оповещения](../log-analytics/log-analytics-alerts.md), которые помогают выполнять обнаружение и диагностику.
* Ознакомьтесь с функциями [поиска по журналам и запросов к журналам](../log-analytics/log-analytics-log-searches.md), которые являются частью журналов Azure Monitor.
* Получить более подробный обзор журналов Azure Monitor и что он предлагает, прочитайте [Что такое журналы Azure Monitor?.](../operations-management-suite/operations-management-suite-overview.md)
