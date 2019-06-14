---
title: Анализ событий Azure Service Fabric с помощью Application Insights | Документы Майкрософт
description: Ознакомьтесь со сведениями о визуализации и анализе событий с использованием Application Insights для мониторинга и диагностики кластеров Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: f4c620bbb0e17abfacb504866230786a971ff409
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60393197"
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Анализ и визуализация событий с помощью Application Insights

Application Insights — это расширяемая платформа для мониторинга и диагностики приложений, предоставляемая в рамках Azure Monitor. Она включает в себя эффективное средство аналитики и запросов, настраиваемую панель мониторинга, визуализации и дополнительные параметры, включая автоматическое оповещение. Интеграция Application Insights с Service Fabric включает средства для Visual Studio и портала Azure, а также некоторые метрики Service Fabric, обеспечивая комплексную фиксацию в журнале всех необходимых данных. Хотя многие журналы создаются автоматически, а данные собираются при использовании Application Insights, мы советуем создавать в своих приложениях дополнительные специализированные журналы для получения более объемной диагностической информации.

Эта статья поможет найти ответ на следующие распространенные вопросы:

* как узнать, что происходит в приложениях и службах, и как собрать данные телеметрии;
* как устранить неполадки в приложениях, особенно если это несколько взаимосвязанных служб;
* как получить метрики выполнения служб, такие как время загрузки страниц и данные об HTTP-запросах.

В этой статье описываются процессы анализа и устранения неполадок в Application Insights. Если вы хотите узнать, как установить и настроить Application Insights с использованием Service Fabric, ознакомьтесь с этим [руководством](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="monitoring-in-application-insights"></a>Мониторинг в Application Insights

Application Insights имеет широкие возможности для использования Service Fabric. На странице "Обзор" в Application Insights содержатся основные сведения о вашей службе, например время отклика и число обработанных запросов. Щелкнув кнопку "Поиск" вверху, вы увидите список последних запросов в приложении. Кроме того, вы увидите здесь неудачные запросы и сможете продиагностировать возникшие ошибки.

![Обзор Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

На предыдущем рисунке вы видите справа панель со списком элементов двух основных типов: запросы и события. Запросы поступают в виде вызовов API-интерфейса приложения, в нашем примере это HTTP-запросы. Событиями называются любые пользовательские события, которые можно добавить в любое место кода в качестве данных телеметрии. Дополнительные сведения об инструментировании приложений вы найдете в статье [API Application Insights для пользовательских событий и метрик](../azure-monitor/app/api-custom-events-metrics.md). Щелкнув запрос, вы увидите представленные на следующем рисунке сведения о нем, включая данные о Service Fabric, которые собираются в пакете NuGet Service Fabric для Application Insights. Эти данные полезны для устранения неполадок и анализа состояния приложения. Все они доступны для поиска в Application Insights.

![Сведения о запросах Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights имеет специальное представление для запросов по поступающим данным. Щелкните "Обозреватель метрик" вверху страницы "Обзор", чтобы перейти на портал Application Insights. Здесь вы можете с помощью языка запросов Kusto получить данные об описанных выше пользовательских событиях, запросах, исключениях, счетчиках производительности и других метриках. В следующем примере показаны все запросы за последний час.

![Сведения о запросах Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Для дальнейшего изучения возможностей портала Application Insights перейдите на [портал документации Application Insights](../azure-monitor/app/app-insights-dashboards.md).

### <a name="configuring-application-insights-with-eventflow"></a>Настройка Application Insights с использованием EventFlow

Если для статистической обработки событий используется EventFlow, импортируйте пакет NuGet `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`. В разделе *outputs* файла *eventFlowConfig.json* должен присутствовать следующий код:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Обязательно внесите необходимые изменения в фильтры, а также включите все остальные входные данные (с соответствующими пакетами NuGet).

## <a name="application-insights-sdk"></a>Пакет SDK для Application Insights

В качестве решений для статистической обработки мы рекомендуем использовать EventFlow и WAD, так как они лучше поддерживают модульный подход к диагностике и мониторингу. Например, чтобы изменить выходные данные EventFlow, не придется менять инструментирование, достаточно лишь внести небольшое изменение в файл конфигурации. Но если вы решите инвестировать в использование Application Insights и не будете переходить на другую платформу, рассмотрите возможность применения нового пакета SDK для Application Insights с целью объединения событий и отправки их в Application Insights. Благодаря этому вам больше не придется настраивать EventFlow для отправки данных в Application Insights. Вместо этого вы установите пакет NuGet Service Fabric для Application Insights. Подробные сведения о пакете можно найти [здесь](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

В статье [Application Insights support for Microservices and Containers](https://azure.microsoft.com/blog/app-insights-microservices/) (Поддержка микрослужб и контейнеров в Application Insights) описываются некоторые новые возможности, над которыми ведется работа (сейчас они доступны в виде бета-версии) и которые расширяют набор встроенных параметров мониторинга в Application Insights. Они включают в себя отслеживание зависимостей (используется при построении карты AppMap для всех служб и приложений в кластере и взаимосвязей между ними), а также улучшенное сопоставление трассировок, поступающих из служб (помогает выявлять проблемы в рабочем процессе приложения или службы).

Если вы ведете разработку на .NET, используете модели программирования Service Fabric и хотите применять Application Insights в качестве платформы для визуализации и анализа данных событий и журналов, мы рекомендуем использовать пакет SDK для Application Insights в рабочем процессе мониторинга и диагностики. Ознакомьтесь с [этой](../azure-monitor/app/asp-net-more.md) и [этой](../azure-monitor/app/asp-net-trace-logs.md) статьями, чтобы начать использование Application Insights для сбора и отображения журналов.

## <a name="navigating-the-application-insights-resource-in-azure-portal"></a>Навигация в ресурсе Application Insights на портале Azure

После настройки Application Insights для сбора событий и журналов сведения должны начать отображаться в ресурсе Application Insights через несколько минут. Перейдите в ресурс Application Insights. При этом откроется информационная панель ресурса Application Insights. На панели задач Application Insights щелкните **Поиск**, чтобы просмотреть последние полученные трассировки и при необходимости отфильтровать их.

*Обозреватель метрик* — это полезное средство для создания пользовательских панелей мониторинга на основе метрик, которые могут предоставляться приложениями, службами и кластером. Сведения о настройке диаграмм на основе собираемых данных см. в статье [Исследование метрик в Application Insights](../azure-monitor/app/metrics-explorer.md).

Если щелкнуть **Аналитика**, откроется портал аналитики Application Insights, на котором доступно больше возможностей по запросу событий и трассировок. Дополнительные сведения см. в статье [Аналитика в Application Insights](../azure-monitor/app/analytics.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Настройте оповещения в Application Insights](../azure-monitor/app/alerts.md), чтобы узнавать об изменениях в производительности или характере использования.
* [Интеллектуальное обнаружение в Application Insights](../azure-monitor/app/proactive-diagnostics.md) осуществляет упреждающий анализ данных телеметрии, отправляемых в Application Insights, и предупреждает о потенциальных проблемах с производительностью.
