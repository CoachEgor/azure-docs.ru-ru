---
title: Наблюдение за расположением данных в Azure Monitor | Документация Майкрософт
description: Описание различных расположений, в которых данные мониторинга хранятся в Azure, включая платформу данных Azure Monitor.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/21/2019
ms.openlocfilehash: bb7e92a676115d784bd19714178b3bd442798e26
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515552"
---
# <a name="monitoring-data-locations-in-azure-monitor"></a>Наблюдение за расположением данных в Azure Monitor

Azure Monitor основан на [платформе данных](data-platform.md) [журналов](data-platform-logs.md) и [метриках](data-platform-metrics.md) , как описано в статье [Azure Monitor Data Platform](data-platform.md). Данные мониторинга из ресурсов Azure могут быть записаны в другие расположения, прежде чем они будут скопированы в Azure Monitor или поддерживать дополнительные сценарии. 

## <a name="monitoring-data-locations"></a>Мониторинг расположения данных

В следующей таблице указаны различные расположения, в которых отправляются данные мониторинга в Azure, и различные методы доступа к ним.

| Расположение | Описание | Варианты доступа |
|:---|:---|:---|:--|
| Метрики Azure Monitor | База данных временных рядов, оптимизированная для анализа данных с отметками времени. | [Обозреватель метрик](metrics-getting-started.md)<br>[API метрик Azure Monitor](/rest/api/monitor/metrics) |
| Журналы Azure Monitor    | Log Analytics рабочей области, основанной на обозреватель данных Azure, которая предоставляет мощный модуль анализа и расширенный язык запросов. | [Служба Log Analytics](../log-query/log-query-overview.md)<br>[Log Analytics API](https://dev.loganalytics.io/)<br>[API Application Insights](https://dev.applicationinsights.io/reference/get-query) |
| Журнал действий | Данные из журнала действий наиболее полезны при отправке в журналы Azure Monitor для анализа с другими данными, но они также собираются самостоятельно, чтобы их можно было просматривать непосредственно в портал Azure. | [Портал Azure](./activity-log.md#view-the-activity-log)<br>[API событий Azure Monitor](/rest/api/monitor/eventcategories) |
| Хранилище Azure | Некоторые источники данных будут записывать данные непосредственно в службу хранилища Azure и требовать настройки для перемещения данных в журналы. Вы также можете отправить данные в службу хранилища Azure для архивирования и интеграции с внешними системами.  | [Аналитика службы хранилища](/rest/api/storageservices/storage-analytics)<br>[Обозреватель серверов](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage)<br>[Обозреватель службы хранилища](../../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows) |
| Центры событий | Отправка данных в концентраторы событий Azure для потоковой передачи в другие расположения. | [Запись в хранилище](../../event-hubs/event-hubs-capture-overview.md)  |
| Azure Monitor для виртуальных машин | Azure Monitor для виртуальных машин сохраняет данные о работоспособности рабочей нагрузки в пользовательском расположении, которое используется для мониторинга в портал Azure. | [Портал Azure](../insights/vminsights-overview.md)<br>[Монитор рабочей нагрузки REST API](/rest/api/monitor/microsoft.workloadmonitor/components)<br>[REST API работоспособности ресурсов Azure](/rest/api/resourcehealth/)  |
| видны узлы | Предупреждения, созданные Azure Monitor. | [Портал Azure](alerts-managing-alert-instances.md)<br>[REST API управления оповещениями](/rest/api/monitor/alertsmanagement/alerts) |



## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь с различными источниками [данных мониторинга, собранными Azure Monitor](data-sources.md).
- Сведения о [типах данных мониторинга, собираемых Azure Monitor](data-platform.md) , а также о том, как просматривать и анализировать эти данные.
