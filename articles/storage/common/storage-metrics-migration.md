---
title: Перенос метрик службы хранилища Azure | Документация Майкрософт
description: Узнайте, как переносить старые метрики в новые, которые управляются службой Azure Monitor.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 03/30/2018
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 58ac15c1aba715c9a5b67e723401b531e76608b2
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153596"
---
# <a name="azure-storage-metrics-migration"></a>Перенос метрик службы хранилища Azure

В соответствии со стратегией унификации мониторинга в Azure служба хранилища Azure интегрирует метрики на платформе Azure Monitor. В будущем предоставление поддержки метрик прежних версий будет завершено с предварительным уведомлением на основе политики Azure. Если вы используете метрики прежних версий, службы хранилища, необходимо выполнить перенос до даты окончания действия службы, чтобы сохранить данные метрик.

В этой статье показано, как перейти от метрик прежних версий к новым.

## <a name="understand-old-metrics-that-are-managed-by-azure-storage"></a>Основные сведения о метриках прежних версий, управляемых службой хранилища Azure

Служба хранилища Azure собирает метрики прежних версий, выполняет статистические вычисления и сохраняет их в таблицах $Metric в одной учетной записи хранения. Портал Azure можно использовать для настройки диаграммы мониторинга. Кроме того, можно использовать пакеты SDK службы хранилища Azure для чтения данных из таблиц $Metric, основанных на схеме. Дополнительные сведения см. в [этой статье](./storage-analytics.md).

Метрики прежних версий предоставляют метрики производительности только для хранилища BLOB-объектов Azure. Метрики прежних версий предоставляют метрики транзакций для хранилища BLOB-объектов, хранилища таблиц, службы "Файлы Azure" и хранилища очередей.

Метрики прежних версий разработаны на основе неструктурированной схемы. Поэтому при отсутствии шаблонов трафика для активации метрики ее данные отсутствовали. Например, **ServerTimeoutError** имеет значение 0 в таблицах $Metric даже в том случае, если вы не получаете какие-либо ошибки времени ожидания сервера из реального трафика для учетной записи хранения.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Основные сведения о новых метриках, управляемых Azure Monitor

Данные новых метрик служба хранилища Azure передает в серверную часть Azure Monitor. Azure Monitor обеспечивает унифицированный мониторинг, включая данные с портала и прием данных. Дополнительные сведения можно найти в этой [статье](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Новые метрики обеспечивают данные о емкости и транзакциях для больших двоичных объектов, таблиц, файлов, очередей и хранилища уровня "Премиум".

Многомерность является одной из функций, предоставляемых Azure Monitor. Служба хранилища Azure учитывает это при определении новой схемы метрики. Дополнительные сведения о поддерживаемых измерениях метрик можно найти в статье о [метриках службы хранилища Azure в Azure Monitor](./storage-metrics-in-azure-monitor.md). Многомерность обеспечивает экономичность в использовании пропускной способности при приеме и емкости при хранении данных метрик. Следовательно, если трафик не активировал связанные метрики, то их данные не создаются. Например, если трафик не активировал какие-либо ошибки времени ожидания сервера, то Azure Monitor не возвращает никаких данных при запросе значения **Transactions** метрики с измерением **ResponseType**, равным **ServerTimeoutError**.

## <a name="metrics-mapping-between-old-metrics-and-new-metrics"></a>Сопоставление метрик прежних и новых версий

При программном чтении данных метрик необходимо перейти на новую схему метрики в своих программах. Чтобы лучше понять изменения, можно ознакомиться с сопоставлением в следующей таблице:

**Метрики емкости**

| Метрика прежних версий | Новая метрика |
| ------------------- | ----------------- |
| **Производительность**            | **BlobCapacity** с измерением **BlobType**, равным **BlockBlob** или **PageBlob**. |
| **ObjectCount**        | **BlobCount** с измерением **BlobType**, равным **BlockBlob** или **PageBlob**. |
| **ContainerCount**      | **ContainerCount** |

Следующие метрики представляют собой новые предложения, не поддерживаемые метриками прежних версий.
* **TableCapacity**
* **TableCount**
* **TableEntityCount**
* **QueueCapacity**
* **QueueCount**
* **QueueMessageCount**
* **FileCapacity**
* **FileCount**
* **FileShareCount**
* **UsedCapacity**

**Метрики транзакций**

| Метрика прежних версий | Новая метрика |
| ------------------- | ----------------- |
| **AnonymousAuthorizationError** | Transactions с измерением **ResponseType**, равным **AuthorizationError**, и измерением **Authentication**, равным **Anonymous** |
| **AnonymousClientOtherError** | Transactions с измерением **ResponseType**, равным **ClientOtherError**, и измерением **Authentication**, равным **Anonymous** |
| **AnonymousClientTimeoutError** | Transactions с измерением **ResponseType**, равным **ClientTimeoutError**, и измерением **Authentication**, равным **Anonymous** |
| **AnonymousNetworkError** | Transactions с измерением **ResponseType**, равным **NetworkError**, и измерением **Authentication**, равным **Anonymous** |
| **AnonymousServerOtherError** | Transactions с измерением **ResponseType**, равным **ServerOtherError**, и измерением **Authentication**, равным **Anonymous** |
| **AnonymousServerTimeoutError** | Transactions с измерением **ResponseType**, равным **ServerTimeoutError**, и измерением **Authentication**, равным **Anonymous** |
| **AnonymousSuccess** | Transactions с измерением **ResponseType**, равным **Success**, и измерением **Authentication**, равным **Anonymous** |
| **AnonymousThrottlingError** | Transactions с измерением **ResponseType**, равным **ClientThrottlingError** или **ServerBusyError**, и измерением **Authentication**, равным **Anonymous** |
| **AuthorizationError** | Transactions с измерением **ResponseType**, равным **AuthorizationError**. |
| **Доступность** | **Доступность** |
| **AverageE2ELatency** | **SuccessE2ELatency** |
| **AverageServerLatency** | **SuccessServerLatency** |
| **ClientOtherError** | Transactions с измерением **ResponseType**, равным **ClientOtherError**. |
| **ClientTimeoutError** | Transactions с измерением **ResponseType**, равным **ClientTimeoutError**. |
| **NetworkError** | Transactions с измерением **ResponseType**, равным **NetworkError**. |
| **PercentAuthorizationError** | Transactions с измерением **ResponseType**, равным **AuthorizationError**. |
| **PercentClientOtherError** | Transactions с измерением **ResponseType**, равным **ClientOtherError**. |
| **PercentNetworkError** | Transactions с измерением **ResponseType**, равным **NetworkError**. |
| **PercentServerOtherError** | Transactions с измерением **ResponseType**, равным **ServerOtherError**. |
| **PercentSuccess** | Transactions с измерением **ResponseType**, равным **Success**. |
| **PercentThrottlingError** | Transactions с измерением **ResponseType**, равным **ClientThrottlingError** или **ServerBusyError**. |
| **PercentTimeoutError** | Transactions с измерением **ResponseType**, равным **ServerTimeoutError**, или с измерением **ResponseType**, равным **ClientTimeoutError**. |
| **SASAuthorizationError** | Transactions с измерением **ResponseType**, равным **AuthorizationError**, и измерением **Authentication**, равным **SAS** |
| **SASClientOtherError** | Transactions с измерением **ResponseType**, равным **ClientOtherError**, и измерением **Authentication**, равным **SAS** |
| **SASClientTimeoutError** | Transactions с измерением **ResponseType**, равным **ClientTimeoutError**, и измерением **Authentication**, равным **SAS** |
| **SASNetworkError** | Transactions с измерением **ResponseType**, равным **NetworkError**, и измерением **Authentication**, равным **SAS** |
| **SASServerOtherError** | Transactions с измерением **ResponseType**, равным **ServerOtherError**, и измерением **Authentication**, равным **SAS** |
| **SASServerTimeoutError** | Transactions с измерением **ResponseType**, равным **ServerTimeoutError**, и измерением **Authentication**, равным **SAS** |
| **SASSuccess** | Transactions с измерением **ResponseType**, равным **Success**, и измерением **Authentication**, равным **SAS** |
| **SASThrottlingError** | Transactions с измерением **ResponseType**, равным **ClientThrottlingError** или **ServerBusyError**, и измерением **Authentication**, равным **SAS** |
| **ServerOtherError** | Transactions с измерением **ResponseType**, равным **ServerOtherError**. |
| **ServerTimeoutError** | Transactions с измерением **ResponseType**, равным **ServerTimeoutError**. |
| **Success** | Transactions с измерением **ResponseType**, равным **Success**. |
| **ThrottlingError** | **Transactions** с измерением **ResponseType**, равным **ClientThrottlingError** или **ServerBusyError**.|
| **TotalBillableRequests** | **Транзакции** |
| **TotalEgress** | **Исходящий трафик** |
| **TotalIngress** | **Входящий трафик** |
| **TotalRequests** | **Транзакции** |

## <a name="faq"></a>Часто задаваемые вопросы

### <a name="how-should-i-migrate-existing-alert-rules"></a>Как перенести существующие правила генерации оповещений?

Если вы создали классические правила генерации оповещений, основанные на метриках службы хранилища прежних версий, необходимо создать новые правила генерации оповещений, основанные на новой схеме метрики.

### <a name="is-new-metric-data-stored-in-the-same-storage-account-by-default"></a>Будет ли данные новых метрик по умолчанию сохраняться в ту же учетную запись хранения?

№ Чтобы заархивировать данные метрик в учетную запись хранения, используйте [API настройки диагностики Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate).

## <a name="next-steps"></a>Дальнейшие действия

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Метрики службы хранилища в Azure Monitor](./storage-metrics-in-azure-monitor.md)
