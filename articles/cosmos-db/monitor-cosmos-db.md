---
title: Мониторинг Azure Cosmos DB | Документация Майкрософт
description: Узнайте, как отслеживать производительность и доступность Azure Cosmos DB.
author: bwren
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: bwren
ms.custom: subject-monitoring
ms.openlocfilehash: c166811bbfd27691f9a01a944d304d06560b0232
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75445183"
---
# <a name="monitoring-azure-cosmos-db"></a>Мониторинг Azure Cosmos DB
При наличии критически важных приложений и бизнес-процессов, использующих ресурсы Azure, необходимо отслеживать эти ресурсы на предмет их доступности, производительности и работы. В этой статье описываются данные мониторинга, созданные в базах данных Azure Cosmos, а также использование функций Azure Monitor для анализа и оповещения об этих данных.

## <a name="what-is-azure-monitor"></a>Общие сведения об Azure Monitor
Azure Cosmos DB создает данные мониторинга с помощью [Azure Monitor](../azure-monitor/overview.md) , которая представляет собой полную службу мониторинга стека в Azure, которая предоставляет полный набор функций для мониторинга ресурсов Azure в дополнение к ресурсам в других облаках и локальных средах. 

Если вы еще не знакомы с наблюдением за службами Azure, начните с статьи [мониторинг ресурсов Azure с помощью Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md) , в котором описано следующее:

- Общие сведения об Azure Monitor
- Затраты, связанные с мониторингом
- Данные мониторинга, собранные в Azure
- Настройка коллекции данных
- Стандартные средства в Azure для анализа и оповещения о данных мониторинга

Следующие разделы посвящены описанию данных, собранных из Azure Cosmos DB, и предоставлены примеры настройки сбора данных и анализа этих данных с помощью средств Azure.

## <a name="azure-monitor-for-cosmos-db-preview"></a>Azure Monitor для Cosmos DB (Предварительная версия)
[Azure Monitor для Azure Cosmos DB](../azure-monitor/insights/cosmosdb-insights-overview.md) основаны на [функциях книг в Azure Monitor](../azure-monitor/app/usage-workbooks.md) и использует те же данные мониторинга, собранные для Cosmos DB, описанных в следующих разделах. Используйте это средство для просмотра общей производительности, сбоев, емкости и работоспособности всех Azure Cosmos DBных ресурсов в едином интерактивном интерфейсе и использования других функций Azure Monitor для подробного анализа и оповещений. 

![Azure Monitor для Cosmos DB](media/monitor-cosmos-db/azure-monitor-cosmos-db.png)

## <a name="monitoring-data-collected-from-azure-cosmos-db"></a>Данные мониторинга, собранные из Azure Cosmos DB

Azure Cosmos DB собирает данные мониторинга тех же типов, что и другие ресурсы Azure, которые описаны в разделе [мониторинг данных из ресурсов Azure](../azure-monitor/insights/monitor-azure-resource.md#monitoring-data). Подробный справочник по журналам и метрикам, созданным Azure Cosmos DB, см. в [справочнике по данным мониторинга Azure Cosmos DB](monitor-cosmos-db-reference.md) .

Страница **Обзор** в портал Azure для каждой базы данных Azure Cosmos содержит краткий обзор использования базы данных, включая его запрос и почасовую оплату. Это полезная информация, но только небольшой объем доступных данных мониторинга. Некоторые из этих данных собираются автоматически и доступны для анализа сразу же после создания базы данных, при этом вы можете включить дополнительный сбор данных с некоторой конфигурацией.

![Страница "Обзор"](media/monitor-cosmos-db/overview-page.png)

## <a name="analyzing-metric-data"></a>Анализ данных метрик

Azure Cosmos DB предоставляет пользовательский интерфейс для работы с метриками. Дополнительные сведения об использовании этого интерфейса и анализе различных сценариев Azure Cosmos DB см. в статье [мониторинг и отладка Azure Cosmos DB метрик из Azure Monitor](cosmos-db-azure-monitor-metrics.md) .

Вы можете анализировать метрики для Azure Cosmos DB с метриками из других служб Azure с помощью обозревателя метрик, открыв **метрики** из меню **Azure Monitor** . Дополнительные сведения об использовании этого средства см. в статье [Приступая к работе с Azure обозреватель метрик](../azure-monitor/platform/metrics-getting-started.md) . Все метрики для Azure Cosmos DB находятся в пространстве имен **Cosmos DB стандартных метриках**. При добавлении фильтра к диаграмме можно использовать следующие измерения со следующими метриками.

- CollectionName
- имя_базы_данных
- OperationType
- Регион
- StatusCode


## <a name="analyzing-log-data"></a>Анализ данных журнала
Данные в журналах Azure Monitor хранятся в таблицах, в которых каждая таблица имеет собственный набор уникальных свойств. Azure Cosmos DB сохраняет данные в следующих таблицах.

| Таблицы | Description |
|:---|:---|
| AzureDiagnostics | Общая таблица, используемая несколькими службами для хранения журналов ресурсов. Журналы ресурсов из Azure Cosmos DB можно определить с помощью `MICROSOFT.DOCUMENTDB`.   |
| AzureActivity    | Общая таблица, в которой хранятся все записи из журнала действий. 


> [!IMPORTANT]
> При выборе **журналов** в меню Azure Cosmos DB log Analytics открывается с областью запроса, заданной в текущей базе данных Azure Cosmos. Это означает, что запросы журнала будут содержать только данные из этого ресурса. Если требуется выполнить запрос, включающий данные из других баз данных или из других служб Azure, выберите **журналы** в меню **Azure Monitor** . Дополнительные сведения см. [в разделе Область запроса журнала и временной диапазон в Azure Monitor log Analytics](../azure-monitor/log-query/scope.md) .

### <a name="azure-cosmos-db-log-analytics-queries-in-azure-monitor"></a>Azure Cosmos DB запросов Log Analytics в Azure Monitor

Ниже приведены некоторые запросы, которые можно ввести в строку поиска по **журналам** , чтобы упростить мониторинг контейнеров Azure Cosmos. Эти запросы поддерживают [новый язык](../log-analytics/log-analytics-log-search-upgrade.md).

Ниже приведены запросы, которые можно использовать для мониторинга баз данных Azure Cosmos.

* Для запроса всех журналов диагностики из Azure Cosmos DB за указанный период времени:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"

    ```

* Для запроса 10 последних событий журнала:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | limit 10
    ```

* Для запроса всех операций, сгруппированных по типу операции:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName
    ```

* Для запроса всех операций, сгруппированных по ресурсам:

    ```Kusto
    AzureActivity 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource

    ```

* Для запроса всех действий пользователя, сгруппированных по ресурсу:

    ```Kusto
    AzureActivity 
    | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource
    ```
* Чтобы получить все запросы, превышающие 100 RUs, Объединенные с данными из **DataPlaneRequests** и **куерирунтиместатистикс**.

    ```Kusto
    AzureDiagnostics
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" and todouble(requestCharge_s) > 100.0
    | project activityId_g, requestCharge_s
    | join kind= inner (
           AzureDiagnostics
           | where ResourceProvider =="MICROSOFT.DOCUMENTDB" and Category == "QueryRuntimeStatistics"
           | project activityId_g, querytext_s
    ) on $left.activityId_g == $right.activityId_g
    | order by requestCharge_s desc
    | limit 100
    ```

* Для запроса операций, которые выполнялись дольше 3 миллисекунд:

    ```Kusto
    AzureDiagnostics 
    | where toint(duration_s) > 3 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* Для запроса агента, выполняющего операции:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName, userAgent_s
    ```

* Для запроса времени выполнения длительных операций:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | project TimeGenerated , duration_s 
    | summarize count() by bin(TimeGenerated, 5s)
    | render timechart
    ```
    
* Получение статистики ключа секции для вычисления смещения по основным трем секциям для учетной записи базы данных:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
    | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
    ```

## <a name="monitor-azure-cosmos-db-programmatically"></a>Программный мониторинг Azure Cosmos DB
Доступные на портале метрики уровня учетной записи, такие как данные об использовании хранилища учетной записи и общее число запросов, недоступны через API-интерфейсы SQL. Тем не менее, можно получить данные об использовании на уровне коллекции с помощью API SQL. Чтобы получить данных на уровне коллекции, выполните следующие действия:

* Чтобы использовать REST API, [выполните запрос GET к коллекции](https://msdn.microsoft.com/library/mt489073.aspx). Квота и данные об использовании коллекции возвращаются в заголовке ответа x-ms-resource-quota и x-ms-resource-usage headers.
* Чтобы использовать пакет SDK для .NET, воспользуйтесь методом [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx), возвращающим объект [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx), который содержит ряд свойств использования, например **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage** и прочие.

Чтобы получить дополнительные метрики, используйте [пакет SDK для Azure Monitor](https://www.nuget.org/packages/Microsoft.Azure.Insights). Доступные определения метрик можно получить с помощью следующего вызова.

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

В запросах для извлечения отдельных метрик используется следующий формат.

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z



## <a name="next-steps"></a>Дальнейшие действия

- Справочник по журналам и метрикам, созданным Azure Cosmos DB, см. в разделе [Azure Cosmos DB Справочник по данным мониторинга](monitor-cosmos-db-reference.md) .
- Дополнительные сведения о мониторинге ресурсов Azure см. в статье [мониторинг ресурсов Azure с помощью Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md) .
