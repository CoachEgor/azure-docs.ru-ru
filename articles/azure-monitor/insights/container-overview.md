---
title: Обзор мониторинга контейнеров Azure | Документация Майкрософт
description: В этой статье представлен обзор различных методов, доступных в Azure для мониторинга контейнеров в Azure, используя которые можно быстро получить представление о работоспособности и доступности кластеров.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: d137576b4beb5cf36dce99ffb1869049f37b60b2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60494649"
---
# <a name="overview-of-monitoring-containers-in-azure"></a>Обзор мониторинга контейнеров в Azure
С помощью Azure можно эффективно отслеживать рабочие нагрузки, развернутые в контейнерах Azure под управлением Kubernetes или Docker, и управлять этими рабочими нагрузками. Важно понимать, как работают контейнеры с несколькими приложениями для микрослужб, чтобы предоставить надежную службу соответствующего масштаба и поддержать план мониторинга. В этой статье приводится краткий обзор возможностей управления и мониторинга в Azure. Она поможет вам понять, какие именно из этих возможностей соответствуют вашим требованиям.

С помощью компонента [Azure Monitor для контейнеров](container-insights-overview.md) можно просмотреть производительность и работоспособность инфраструктуры контейнеров Linux и быстро найти причину проблем. Данные телеметрии хранятся в рабочей области Log Analytics и интегрируются с порталом Azure, где вы можете просматривать, фильтровать и разделять статистические данные с помощью панели мониторинга, чтобы следить за нагрузкой, производительностью и работоспособностью.  

Для контейнеров, запущенных вне размещенной в Azure службы Kubernetes, можно использовать [решение для контейнеров Windows и Docker](../../azure-monitor/insights/containers.md) Log Analytics, с помощью которого можно просматривать узлы контейнеров Windows и Docker и управлять ими. В рабочей области Log Analytics можно просмотреть данные инвентаризации, производительности и событий из узлов и контейнеров в среде. Можно просмотреть подробные данные аудита, содержащие команды, выполненные с контейнерами, а также устранить неполадки в работе контейнеров, просмотрев централизованные журналы и выполнив в них поиск без необходимости удаленного доступа к узлам Docker или Windows.

Чтобы обеспечить целостный или комплексный мониторинг приложения, включая все его зависимости (как ресурсы Azure, так и локальные ресурсы), необходимо использовать Azure Monitor или Log Analytics.  Следует включить в мониторинг прикладной уровень, чтобы добавить дополнительный уровень учета работоспособности, как на уровне платформы, так и на уровне приложения, с помощью Application Insights. На уровне платформы доступны пакеты SDK Application Insights для [Kubernetes]( https://github.com/Microsoft/ApplicationInsights-Kubernetes), [Docker](https://hub.docker.com/r/microsoft/applicationinsights/) и [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights). Для приложений для микрослужб, предусмотрена поддержка [Java](../../azure-monitor/app/java-get-started.md), [Node.js](../../azure-monitor/learn/nodejs-quick-start.md), [.NET](../../azure-monitor/app/asp-net.md), [.NET Core](../../azure-monitor/app/asp-net-core.md), а также ряд других [языками и платформами](../../azure-monitor/app/platforms.md). 

В противном случае проблемы в работе приложения могут остаться необнаруженными и повлиять на доступность этого приложения, что помешает достижению целевых показателей уровня обслуживания.  
