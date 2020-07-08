---
title: Подготовка к изменению формата для Azure Monitor журналов ресурсов
description: Описание влияния и способов обновления инструментов для обработки новых журналов ресурсов Azure, которые были изменены для использования добавочных больших двоичных объектов в 1 ноября 2018 г.
author: johnkemnetz
services: monitoring
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 582e7ea4e954a1aa5ee27314d038601f7e26c61e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "84803931"
---
# <a name="format-of-azure-monitor-resource-logs-archived-to-a-storage-account"></a>Формат журналов ресурсов Azure Monitor, архивированных в учетную запись хранения

> [!NOTE]
> Если вы отправляете [журналы ресурсов Azure или метрики в учетную запись хранения с помощью параметров диагностики ресурсов](./../../azure-monitor/platform/archive-diagnostic-logs.md) или [журналов действий в учетную запись хранения с помощью профилей журналов](./../../azure-monitor/platform/archive-activity-log.md), формат данных в учетной записи хранения ИЗМЕНИЛся на строки JSON-файла ноября. 1, 2018. Ниже описывается влияние этого изменения, а также приводятся инструкции по обновлению инструментария для включения поддержки нового формата. 
>
> 

## <a name="what-is-changing"></a>Изменения

Azure Monitor предлагает возможность отправки диагностических данных ресурсов и данных журнала действий в учетную запись хранения Azure, пространство имен концентраторов событий или в рабочую область Log Analytics в Azure Monitor. В целях устранения проблем с производительностью системы **1 ноября 2018 г. в полночь по времени в формате UTC** изменится формат данных журналов, отправляемых в хранилище BLOB-объектов. Если у вас есть инструменты для считывания данных из хранилища BLOB-объектов, их необходимо обновить для распознавания нового формата данных.

* В четверг 1 ноября 2018 г. в полночь по времени UTC формат BLOB-объектов изменится на [JSON Lines](http://jsonlines.org/). Это означает, что каждая запись будет разделяться новой строкой. Массив внешних записей и запятые между записями JSON использоваться не будут.
* Формат BLOB-объектов меняется сразу для всех параметров диагностики во всех подписках. Этот новый формат будет использоваться для первого файла PT1H.json, выпущенного 1 ноября. Имена больших двоичных объектов и контейнеров останутся без изменений.
* Задание параметра диагностики в период с текущей даты до 1 ноября будет поддерживаться для передачи данных в текущем формате до 1 ноября.
* Это изменение произойдет один раз во всех регионах общедоступного облака. Это изменение не будет происходить в Microsoft Azure, которые будут использоваться облаками 21Vianet, Azure для Германии и Azure для государственных организаций.
* Это изменение затрагивает следующие типы данных:
  * [Журналы ресурсов Azure](archive-diagnostic-logs.md) ([см. список ресурсов здесь](diagnostic-logs-schema.md))
  * [метрики ресурсов Azure, экспортируемые с помощью параметров диагностики](diagnostic-settings.md);
  * [данные журнала действий Azure, экспортируемые с помощью профилей журналов](archive-activity-log.md).
* Это изменение не затрагивает следующие данные:
  * журналы сетевых потоков;
  * Журналы службы Azure недоступны через Azure Monitor еще (например, журналы ресурсов службы приложений Azure, журналы аналитики хранилища).
  * Маршрутизация журналов ресурсов Azure и журналов действий в другие назначения (концентраторы событий, Log Analytics)

### <a name="how-to-see-if-you-are-impacted"></a>Как узнать о возможном влиянии

Это изменение затронет вас только в приведенных ниже случаях.
1. Вы отправляете данные журнала в учетную запись хранения Azure с помощью параметра диагностики ресурсов.
2. У вас есть инструменты, зависящие от структуры JSON этих журналов в хранилище.
 
Чтобы определить наличие параметров диагностики ресурсов, которые отправляют данные в учетную запись хранения Azure, на портале перейдите в раздел **Monitor**, щелкните **Параметры диагностики** и определите ресурсы, в колонке **Состояние диагностики** которых задано значение **Включено**.

![Колонка "Параметры диагностики" Azure Monitor](./media/diagnostic-logs-append-blobs/portal-diag-settings.png)

Если в колонке "Состояние диагностики" задано значение "Включено", для данного ресурса существует активный параметр диагностики. Щелкните ресурс, чтобы просмотреть, используются ли какие-либо параметры диагностики для отправки данных в учетную запись хранения.

![Учетная запись хранения включена](./media/diagnostic-logs-append-blobs/portal-storage-enabled.png)

Если у вас есть ресурсы, которые отправляют данные в учетную запись хранения с помощью этих параметров диагностики ресурса, это изменение затронет формат данных в этой учетной записи. Если у вас нет специальных инструментов, которые работают с этими учетными записями хранения, изменение формата вас не коснется.

### <a name="details-of-the-format-change"></a>Сведения об изменении формата

Текущий формат файла PT1H.json в хранилище BLOB-объектов Azure использует массив записей JSON. Ниже приведен пример файла журнала KeyVault.

```json
{
    "records": [
        {
            "time": "2016-01-05T01:32:01.2691226Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "78",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        },
        {
            "time": "2016-01-05T01:33:56.5264523Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "83",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        }
    ]
}
```

В новом формате используются [строки JSON](http://jsonlines.org/), где каждое событие представляет собой строку, а символ новой строки означает новое событие. После изменения приведенный выше пример в файле PT1H.json будет выглядеть следующим образом:

```json
{"time": "2016-01-05T01:32:01.2691226Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "78","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
{"time": "2016-01-05T01:33:56.5264523Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "83","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
```

Этот новый формат позволяет Azure Monitor отправлять файлы журналов с помощью [добавочных BLOB-объектов](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs), которые более эффективны для постоянного добавления новых данных событий.

## <a name="how-to-update"></a>Способ обновления

Обновления следует выполнять только при наличии специальных инструментов, которые принимают эти файлы журналов для дальнейшей обработки. Если вы используете внешний инструмент анализа журналов или SIEM, рекомендуется [заменить его на концентраторы событий для приема данных](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/). Интеграция Центров событий оптимально подходит для обработки журналов из многих служб и создания закладок для расположений в определенном журнале.

Пользовательские инструменты необходимо обновить для обработки текущего формата и описанного выше формата JSON Lines. Тем самым гарантируется, что они сохранят работоспособность при появлении данных в новом формате.

## <a name="next-steps"></a>Дальнейшие шаги

* Сведения о [архивировании журналов ресурсов ресурсов в учетную запись хранения](./../../azure-monitor/platform/archive-diagnostic-logs.md)
* Узнайте больше об [архивации данных журнала действий в учетной записи хранения](./../../azure-monitor/platform/archive-activity-log.md).

