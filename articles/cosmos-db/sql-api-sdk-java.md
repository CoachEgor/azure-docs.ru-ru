---
title: Azure Cosmos DB — API, пакет SDK и ресурсы для SQL Java
description: Сведения о пакете SDK и API-интерфейсе SQL Java, включая даты выхода и прекращения использования, а также изменения, внесенные в каждую версию пакета SDK SQL Java для Azure Cosmos DB.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/12/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 0b48b8d954b132caab96e5978c90687899ea04c2
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "96549231"
---
# <a name="azure-cosmos-db-java-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB: Заметки о выпуске и материалы
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]
> [!div class="op_single_selector"]
> * [Пакет SDK версии 3 для .NET](sql-api-sdk-dotnet-standard.md)
> * [Пакет SDK для .NET версии 2](sql-api-sdk-dotnet.md)
> * [Пакет SDK для .NET Core версии 2](sql-api-sdk-dotnet-core.md)
> * [Пакет SDK для .NET Change Feed версии 2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Пакет SDK для Java версии 4](sql-api-sdk-java-v4.md)
> * [Пакет SDK для Async Java версии 2](sql-api-sdk-async-java.md)
> * [Пакет SDK для Sync Java версии 2](sql-api-sdk-java.md)
> * [Spring Data версии 2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data версии 3](sql-api-sdk-java-spring-v3.md)
> * [Соединитель Spark](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [Поставщик ресурсов REST](/rest/api/cosmos-db-resource-provider/)
> * [SQL](./sql-query-getting-started.md)
> * [Исполнитель массовых операций — .NET версии 2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Исполнитель массовых операций — Java](sql-api-sdk-bulk-executor-java.md)

Это исходный пакет SDK для Sync Java версии 2 для API SQL Azure Cosmos DB, который поддерживает синхронные операции.

> [!IMPORTANT]  
> Это *не* последняя версия пакета SDK для Java для Azure Cosmos DB! Для проекта рекомендуется использовать [пакет SDK для Java версии 4 для Azure Cosmos DB](sql-api-sdk-java-v4.md). Следуйте инструкциям из руководства по [переходу на использование пакета SDK для Java версии 4 для Azure Cosmos DB](migrate-java-v4-sdk.md) и статье о [Reactor и RxJava](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/main/reactor-rxjava-guide.md). 
>

| |  |
|---|---|
|**Скачивание пакета SDK**|[Maven](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)|
|**Документация по API**|[Справочная документация по API Java](/java/api/com.microsoft.azure.documentdb)|
|**Участие в разработке пакета SDK**|[GitHub](https://github.com/Azure/azure-documentdb-java/)|
|**Начало работы**|[Приступая к работе с пакетом SDK для Java](./create-sql-api-java.md)|
|**Учебник по веб-приложениям**|[Руководство по ASP.NET MVC. Разработка веб-приложений в Azure Cosmos DB](sql-api-java-application.md)|
|**Минимальная поддерживаемая среда выполнения**|[Пакет средств разработки Java (JDK) 7+](/java/azure/jdk/?view=azure-java-stable&preserve-view=true)|

## <a name="release-notes"></a>Заметки о выпуске

### <a name="251"></a><a name="2.5.1"></a>2.5.1
* Устраняет проблему с кэшем основного раздела в запросе коллекции documentcollection.

### <a name="250"></a><a name="2.5.0"></a>2.5.0
* Добавлена поддержка повторной пользовательской конфигурации 449.

### <a name="247"></a><a name="2.4.7"></a>2.4.7
* Исправлена ошибка времени ожидания пула соединений.
* Исправлено обновление маркера проверки подлинности при внутренних повторных попытках.

### <a name="246"></a><a name="2.4.6"></a>2.4.6
* Обновлен правильный тег политики реплики на стороне клиента в databaseAccount и выполнено считывание конфигурации databaseAccount из кэша.

### <a name="245"></a><a name="2.4.5"></a>2.4.5
* Исключение повторных попыток при ошибке неверного диапазона ключей секций, если пользователь предоставляет pkRangeId.

### <a name="244"></a><a name="2.4.4"></a>2.4.4
* Оптимизировано обновление кэша диапазона ключей секций.
* Исправлена ситуация, когда пакет SDK не принимает указание разбиения секций с сервера, что приводит к неправильному обновлению кэша маршрутизации на стороне клиента.

### <a name="242"></a><a name="2.4.2"></a>2.4.2
* Оптимизировано обновление кэша коллекции.

### <a name="241"></a><a name="2.4.1"></a>2.4.1
* Добавлена поддержка извлечения внутреннего сообщения об исключении из строки диагностики запроса.

### <a name="240"></a><a name="2.4.0"></a>2.4.0
* В PartitionKeyDefinition добавлена версия API.

### <a name="230"></a><a name="2.3.0"></a>2.3.0
* Добавлена отдельная поддержка тайм-аутов для прямого режима.

### <a name="223"></a><a name="2.2.3"></a>2.2.3
* Извлечение пустого сообщения об ошибке из службы и создание исключения клиента документа.

### <a name="222"></a><a name="2.2.2"></a>2.2.2
* Улучшение подключения к сокету, значение SoKeepAlive по умолчанию установлено в true.

### <a name="220"></a><a name="2.2.0"></a>2.2.0
* Добавлена поддержка строк диагностики запросов.

### <a name="213"></a><a name="2.1.3"></a>2.1.3
* Исправлена ошибка в PartitionKey для Hash V2.

### <a name="212"></a><a name="2.1.2"></a>2.1.2
* Добавлена поддержка составных индексов.
* Исправлена ошибка в глобальном диспетчере конечных точек для принудительного обновления.
* Исправлена ошибка для операций upsert с предварительными условиями в режиме прямого подключения.

### <a name="211"></a><a name="2.1.1"></a>2.1.1
* Исправлена ошибка в кэше адресов шлюза.

### <a name="210"></a><a name="2.1.0"></a>2.1.0
* Добавлена поддержка операций записи в нескольких регионах для режима прямого подключения.
* Добавлена поддержка обработки исключений IOException, генерируемых как исключения ServiceUnavailable из прокси-сервера.
* Исправлена ошибка в политике повтора обнаружения конечных точек.
* Исправлена ошибка, приводившая к возникновению исключения пустого указателя в BaseDatabaseAccountConfigurationProvider.
* Исправлена ошибка, в результате которой QueryIterator возвращал нулевые значения.
* Исправлена ошибка, не разрешающая использование большого ключа секции.

### <a name="200"></a><a name="2.0.0"></a>2.0.0
* Добавлена поддержка операций записи в нескольких регионах для режима шлюза.

### <a name="1164"></a><a name="1.16.4"></a>1.16.4
* Исправлена ошибка в чтении диапазонов ключей секций для запроса.

### <a name="1163"></a><a name="1.16.3"></a>1.16.3
* Исправлена ошибка в настройке размера заголовка маркера продолжения в режиме DirectHttps.

### <a name="1162"></a><a name="1.16.2"></a>1.16.2
* Добавлена поддержка отработки отказа потоковой передачи.
* Добавлена поддержка для пользовательских метаданных.
* Улучшена логика обработки сеанса.
* Исправлена ошибка в кэше диапазона ключей секций.
* Исправлена ошибка с NPE в режиме прямого подключения.

### <a name="1161"></a><a name="1.16.1"></a>1.16.1
* Добавлена поддержка уникальных индексов.
* Добавлена поддержка ограничения размера маркера продолжения в параметрах веб-канала.
* Исправлена ошибка сериализации JSON (метка времени).
* Исправлена ошибка сериализации JSON (перечисление).
* Зависимость от com.fasterxml.jackson.core:jackson-databind обновлена до версии 2.9.5.

### <a name="1160"></a><a name="1.16.0"></a>1.16.0
* Улучшен пул подключений для режима прямого подключения.
* Улучшена предварительная выборка для запросов между секциями, отличных от orderBy.
* Улучшено создание UUID.
* Улучшена логика согласованности сеанса.
* Добавлена поддержка MultiPolygon.
* Добавлена поддержка статистики диапазона ключей секции для коллекции.
* Исправлена ошибка, возникающая при поддержке нескольких регионов.

### <a name="1150"></a><a name="1.15.0"></a>1.15.0
* Повышенная производительность сериализации JSON.
* Для этой версии пакета SDK требуется последняя версия [эмулятора Azure Cosmos DB](https://aka.ms/cosmosdb-emulator).

### <a name="1140"></a><a name="1.14.0"></a>1.14.0
* Внутренние изменения в библиотеках партнеров корпорации Майкрософт.

### <a name="1130"></a><a name="1.13.0"></a>1.13.0
* Устранена проблема при считывании диапазонов односекционных ключей.
* Устранена проблема с синтаксическим анализом в ResourceID, которая затрагивала базы данных с сокращенными именами.
* Исправлена проблема, вызванная кодированием ключа секции.

### <a name="1120"></a><a name="1.12.0"></a>1.12.0
* Важные исправления для обработки запросов во время разбиения на секции.
* Исправлена проблема с уровнями согласованности Strong и BoundedStaleness.

### <a name="1110"></a><a name="1.11.0"></a>1.11.0
* Добавлена поддержка нового уровня согласованности с именем ConsistentPrefix.
* Исправлена ошибка чтения коллекции в режиме сеанса.

### <a name="1100"></a><a name="1.10.0"></a>1.10.0
* Включена поддержка секционированных коллекций с производительностью 2500 ЕЗ/с, а также масштабирование с шагом в 100 ЕЗ/с.
* Исправлена ошибка в машинной сборке, которая могла вызывать в некоторых запросах исключение NullRef.

### <a name="196"></a><a name="1.9.6"></a>1.9.6
* Исправлена ошибка в конфигурации обработчика запросов, которая могла вызывать исключения для запросов в режиме шлюза.
* Исправлены некоторые ошибки в контейнере сеансов, которые могли вызывать исключение Owner resource not found (Ресурс владельца не найден) для запросов сразу после создания коллекции.

### <a name="195"></a><a name="1.9.5"></a>1.9.5
* Добавлена поддержка статистических запросов (COUNT, MIN, MAX, SUM и AVG). Дополнительные сведения см. в разделе [Статистические функции](sql-query-aggregate-functions.md).
* Добавлена поддержка веб-канала изменений.
* Добавлена поддержка сведений о квотах коллекций посредством RequestOptions.setPopulateQuotaInfo.
* Добавлена поддержка ведения журнала сценариев хранимых процедур посредством RequestOptions.setScriptLoggingEnabled.
* Исправлена ошибка, из-за которой запросы в режиме DirectHttps переставали отвечать при обнаружении ошибок регулирования.
* Исправлена ошибка в режиме согласованности сеанса.
* Исправлена ошибка, которая могла приводить к порождению исключения NullReferenceException в HttpContext при высокой частоте запросов.
* Повышена производительность режима DirectHttps.

### <a name="194"></a><a name="1.9.4"></a>1.9.4
* Добавлена поддержка прокси-сервера на основе экземпляра простого клиента с помощью API ConnectionPolicy.setProxy().
* Добавлен API DocumentClient.close() для правильного завершения работы экземпляра DocumentClient.
* Повышена производительность запросов в режиме прямого подключения за счет получения плана запроса от машинной сборки, а не шлюза.
* Задан параметр FAIL_ON_UNKNOWN_PROPERTIES = false, чтобы пользователям не нужно было определять JsonIgnoreProperties в POJO.
* Выполнен рефакторинг ведения журнала для использования SLF4J.
* Исправлено несколько ошибок в модуле чтения данных согласованности.

### <a name="193"></a><a name="1.9.3"></a>1.9.3
* Исправлена ошибка в управлении подключениями для предотвращения утечек в режиме прямого подключения.
* Исправлена ошибка в запросе TOP, которая могла порождать исключение NullReference.
* Повышена производительности за счет сокращения числа сетевых вызовов к внутренним кэшам.
* В DocumentClientException добавлены код состояния, ActivityID и универсальный код ресурса (URI) для более удобного устранения неполадок.

### <a name="192"></a><a name="1.9.2"></a>1.9.2
* Исправлена проблема в управлении подключениями для повышения стабильности.

### <a name="191"></a><a name="1.9.1"></a>1.9.1
* Добавлена поддержка уровня согласованности BoundedStaleness.
* Добавлена поддержка прямого подключения для операций CRUD с секционированными коллекциями.
* Исправлена ошибка, возникающая при выполнении запросов к базе данных с помощью SQL.
* Исправлена ошибка в кэше сеанса, приводившая к неправильному заданию маркера сеанса.

### <a name="190"></a><a name="1.9.0"></a>1.9.0
* Добавлена поддержка параллельных запросов между секциями.
* Добавлена поддержка запросов TOP и ORDER BY для секционированных коллекций.
* Добавлена поддержка строгой согласованности.
* Добавлена поддержка запросов по имени при использовании прямого подключения.
* Внесено исправление, обеспечивающее согласованность ActivityId для всех попыток выполнения запроса.
* Исправлена ошибка, связанная с кэшем сеанса и воссозданием коллекции с тем же именем.
* Добавлены типы данных Polygon и LineString и задана политика индексирования коллекций для запросов пространственных геозон.
* Устранены проблемы с JavaDoc для Java 1.8.

### <a name="181"></a><a name="1.8.1"></a>1.8.1
* Исправлена ошибка в PartitionKeyDefinitionMap, из-за которой осуществлялось кэширование коллекций одной секции и не выполнялись дополнительные запросы на получение ключа секции.
* Исправлена ошибка, из-за которой при предоставлении неправильного ключа секции не предпринималась повторная попытка.

### <a name="180"></a><a name="1.8.0"></a>1.8.0
* Добавлена поддержка учетных записей базы данных в нескольких регионах.
* Добавлена поддержка автоматического повтора на отрегулированных запросов с параметрами для настройки максимального количества повторов и максимального время между повторами.  Ознакомьтесь с RetryOptions и ConnectionPolicy.getRetryOptions().
* Не рекомендуется использовать IPartitionResolver на основе пользовательского кода секционирования. Используйте секционированные коллекции, чтобы увеличить возможности хранилища и пропускную способность.

### <a name="171"></a><a name="1.7.1"></a>1.7.1
* Добавлена поддержка политики повтора для ограничения скорости.  

### <a name="170"></a><a name="1.7.0"></a>1.7.0
* Добавлена поддержка срока жизни для документов.

### <a name="160"></a><a name="1.6.0"></a>1.6.0
* Реализованы [секционированные коллекции](partitioning-overview.md) и [определяемые пользователем уровни производительности](performance-levels.md).

### <a name="151"></a><a name="1.5.1"></a>1.5.1
* Исправлена ошибка в HashPartitionResolver, чтобы значения создавались с прямым порядком байтов для согласования с другими пакетами SDK.

### <a name="150"></a><a name="1.5.0"></a>1.5.0
* Добавьте сопоставители разделов "Хэш" и "Диапазон" для сегментирования приложений на разделы.

### <a name="140"></a><a name="1.4.0"></a>1.4.0
* Реализована операция Upsert. Для поддержки функции Upsert добавлены новые методы upsertXXX.
* Реализована маршрутизация на основе идентификатора. Отсутствуют изменения общего API-интерфейса. Все изменения касаются внутреннего интерфейса.

### <a name="130"></a><a name="1.3.0"></a>1.3.0
* Пропущен номер выпуска для согласованности номера версии с другими пакетами SDK.

### <a name="120"></a><a name="1.2.0"></a>1.2.0
* Поддержка геопространственного индекса
* Проверка свойств идентификатора для всех ресурсов. Идентификаторы ресурсов не могут содержать знаки ?, /, #, \, или заканчиваться пробелом.
* Добавлен новый заголовок "ход выполнения преобразования индекса" в ResourceResponse.

### <a name="110"></a><a name="1.1.0"></a>1.1.0
* Реализована политика индексации версии 2.

### <a name="100"></a><a name="1.0.0"></a>1.0.0
* Пакет SDK общей доступности.

## <a name="release-and-retirement-dates"></a>Даты выпуска и выбытия

Корпорация Майкрософт отправит уведомление минимум за **12 месяцев** до вывода пакета SDK из эксплуатации, чтобы обеспечить более плавный переход на новую или поддерживаемую версию. Новые функции, возможности и оптимизации добавляются только в текущую версию пакета SDK, поэтому рекомендуется как можно раньше обновлять пакет SDK до последней версии.

> [!WARNING]
> После 30 мая 2020 г. Azure Cosmos DB больше не будет устранять ошибки, добавить новые функции и предоставить поддержку версий 1. x пакета SDK для Java Azure Cosmos DB для API SQL. Даже если вы не выполните обновление, запросы, отправляемые из версий 1.х пакета SDK, будут обрабатываться службой Azure Cosmos DB.
>
> После 29 февраля 2016 Azure Cosmos DB больше не будет вносить исправления в ошибки, добавлять новые функции и предоставлять поддержку версий 0. x пакета SDK для Java Azure Cosmos DB для API SQL. Если вы предпочитаете не выполнять обновление, запросы, отправленные с версии 0. x пакета SDK, будут по прежнему обслуживаться службой Azure Cosmos DB.


| Версия | Дата выпуска | Дата вывода |
| --- | --- | --- |
| [2.5.1](#2.5.1) |03 июня, 2020 |--- |
| [2.5.0](#2.5.0) |12 мая 2020 г. |--- |
| [2.4.7](#2.4.7) |20 февраля 2020 г. |--- |
| [2.4.6](#2.4.6) |24 января 2020 г. |--- |
| [2.4.5](#2.4.5) |10 ноября 2019 г. |--- |
| [2.4.4](#2.4.4) |24 октября 2019 г. |--- |
| [2.4.2](#2.4.2) |26 сентября 2019 г. |--- |
| [2.4.1](#2.4.1) |18 июля 2019 г. |--- |
| [2.4.0](#2.4.0) |04 мая 2019 г. |--- |
| [2.3.0](#2.3.0) |24 апреля 2019 г. |--- |
| [2.2.3](#2.2.3) |16 апреля 2019 г. |--- |
| [2.2.2](#2.2.2) |05 апреля 2019 г. |--- |
| [2.2.0](#2.2.0) |27 марта 2019 г. |--- |
| [2.1.3](#2.1.3) |13 марта 2019 г. |--- |
| [2.1.2](#2.1.2) |09 марта 2019 г. |--- |
| [2.1.1](#2.1.1) |13 декабря 2018 г. |--- |
| [2.1.0](#2.1.0) |20 ноября 2018 г. |--- |
| [2.0.0](#2.0.0) |21 сентября 2018 г. |--- |
| [1.16.4](#1.16.4) |10 сентября 2018 г. |30 мая 2020 г. |
| [1.16.3](#1.16.3) |9 сентября 2018 г. |30 мая 2020 г. |
| [1.16.2](#1.16.2) |29 июня 2018 г. |30 мая 2020 г. |
| [1.16.1](#1.16.1) |16 мая 2018 г. |30 мая 2020 г. |
| [1.16.0](#1.16.0) |15 марта 2018 г. |30 мая 2020 г. |
| [1.15.0](#1.15.0) |14 ноября 2017 г. |30 мая 2020 г. |
| [1.14.0](#1.14.0) |28 октября 2017 г. |30 мая 2020 г. |
| [1.13.0](#1.13.0) |25 августа 2017 г. |30 мая 2020 г. |
| [1.12.0](#1.12.0) |11 июля 2017 г. |30 мая 2020 г. |
| [1.11.0](#1.11.0) |10 мая 2017 г. |30 мая 2020 г. |
| [1.10.0](#1.10.0) |11 марта 2017 г. |30 мая 2020 г. |
| [1.9.6](#1.9.6) |21 февраля 2017 г. |30 мая 2020 г. |
| [1.9.5](#1.9.5) |31 января 2017 г. |30 мая 2020 г. |
| [1.9.4](#1.9.4) |24 ноября 2016 г. |30 мая 2020 г. |
| [1.9.3](#1.9.3) |30 октября 2016 г. |30 мая 2020 г. |
| [1.9.2](#1.9.2) |28 октября 2016 г. |30 мая 2020 г. |
| [1.9.1](#1.9.1) |26 октября 2016 г. |30 мая 2020 г. |
| [1.9.0](#1.9.0) |3 октября 2016 г. |30 мая 2020 г. |
| [1.8.1](#1.8.1) |30 июня 2016 г. |30 мая 2020 г. |
| [1.8.0](#1.8.0) |14 июня 2016 г. |30 мая 2020 г. |
| [1.7.1](#1.7.1) |30 апреля 2016 г. |30 мая 2020 г. |
| [1.7.0](#1.7.0) |27 апреля 2016 г. |30 мая 2020 г. |
| [1.6.0](#1.6.0) |29 марта 2016 г. |30 мая 2020 г. |
| [1.5.1](#1.5.1) |31 декабря 2015 г. |30 мая 2020 г. |
| [1.5.0](#1.5.0) |4 декабря 2015 г. |30 мая 2020 г. |
| [1.4.0](#1.4.0) |5 октября 2015 г. |30 мая 2020 г. |
| [1.3.0](#1.3.0) |5 октября 2015 г. |30 мая 2020 г. |
| [1.2.0](#1.2.0) |5 августа 2015 г. |30 мая 2020 г. |
| [1.1.0](#1.1.0) |9 июля 2015 г. |30 мая 2020 г. |
| 1.0.1 |12 мая 2015 г. |30 мая 2020 г. |
| [1.0.0](#1.0.0) |7 апреля 2015 г. |30 мая 2020 г. |
| 0.9.5-prelease |9 марта 2015 г. |29 февраля 2016 г. |
| 0.9.4-prelease |17 февраля 2015 г. |29 февраля 2016 г. |
| 0.9.3-prelease |13 января 2015 г. |29 февраля 2016 г. |
| 0.9.2-prelease |19 декабря 2014 г. |29 февраля 2016 г. |
| 0.9.1-prelease |19 декабря 2014 г. |29 февраля 2016 г. |
| 0.9.0-prelease |10 декабря 2014 г. |29 февраля 2016 г. |

## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>См. также раздел
Дополнительные сведения о Cosmos DB см. на странице службы [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).