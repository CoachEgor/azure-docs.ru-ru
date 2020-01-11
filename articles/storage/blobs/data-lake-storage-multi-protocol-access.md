---
title: Доступ с несколькими протоколами на Azure Data Lake Storage | Документация Майкрософт
description: Используйте API больших двоичных объектов и приложения, использующие API больших двоичных объектов с Azure Data Lake Storage 2-го поколения.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/01/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: f34c5d5069a158579864320d0fbf965de8936d9c
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2020
ms.locfileid: "75896109"
---
# <a name="multi-protocol-access-on-azure-data-lake-storage"></a>Доступ с несколькими протоколами на Azure Data Lake Storage

Интерфейсы API больших двоичных объектов теперь работают с учетными записями, имеющими иерархическое пространство имен. Это разблокирует экосистему средств, приложений и служб, а также несколько функций хранилища BLOB-объектов для учетных записей с иерархическим пространством имен.

До последнего времени может потребоваться поддерживать отдельные решения хранилища для хранилища объектов и хранилища аналитики. Это связано с тем, что Azure Data Lake Storage 2-го поколения поддерживала ограниченную поддержку экосистемы. Кроме того, у него есть ограниченный доступ к функциям службы BLOB-объектов, таким как журнал диагностики. Решение с фрагментированным хранилищем сложно поддерживать, так как для выполнения различных сценариев необходимо переместить данные между учетными записями. Это больше не нужно.

Используя многопротокольный доступ к Data Lake Storage, вы можете работать с данными с помощью экосистемы средств, приложений и служб. Сюда также входят средства и приложения сторонних производителей. Можно указать учетные записи с иерархическим пространством имен, не изменяя их. Эти приложения работают, даже если они вызывают API больших двоичных объектов, *так как API* -интерфейсы больших двоичных объектов теперь могут работать с данными в учетных записях, имеющих иерархическое пространство имен.

Функции хранилища BLOB-объектов, такие как [ведение журнала диагностики](../common/storage-analytics-logging.md), [уровни доступа](storage-blob-storage-tiers.md)и [политики управления жизненным циклом хранилища BLOB-объектов](storage-lifecycle-management-concepts.md) , теперь работают с учетными записями, имеющими иерархическое пространство имен. Таким образом, можно включить иерархические пространства имен в учетных записях хранения BLOB-объектов без потери доступа к этим важным функциям. 

> [!NOTE]
> Многопротокольный доступ на Data Lake Storage является общедоступным и доступен во всех регионах. Некоторые службы Azure или функции хранилища BLOB-объектов, включенные при доступе по нескольким протоколам, остаются в предварительной версии. Дополнительные сведения см. в таблицах в каждом разделе этой статьи. 

## <a name="how-multi-protocol-access-on-data-lake-storage-works"></a>Как работает многопротокольный доступ к хранилищу данных Data Lake

API-интерфейсы больших двоичных объектов и API Data Lake Storage 2-го поколения могут обрабатывать одни и те же данные в учетных записях хранения, имеющих иерархическое пространство имен. Data Lake Storage 2-го поколения направляет интерфейсы API больших двоичных объектов через иерархическое пространство имен, чтобы получить преимущества операций с каталогами первого класса и совместимых со стандартом POSIX списков управления доступом (ACL). 

![Общие сведения о многопротокольном доступе Data Lake Storage](./media/data-lake-storage-interop/interop-concept.png) 

Существующие средства и приложения, использующие API больших двоичных объектов, получают эти преимущества автоматически. Разработчикам не придется изменять их. Data Lake Storage 2-го поколения постоянно применяет списки ACL уровня каталога и файлов независимо от протокола, используемого инструментами и приложениями для доступа к данным. 

## <a name="blob-storage-feature-support"></a>Поддержка функций хранилища BLOB-объектов

Многопротокольный доступ на Data Lake Storage позволяет использовать дополнительные функции хранилища BLOB-объектов с Data Lake Storage. В этой таблице перечислены функции, включенные с помощью многопротокольного доступа к Data Lake Storage. 

Элементы, отображаемые в этой таблице, будут меняться со временем, так как поддержка функций хранилища BLOB-объектов будет по-видимому расширена. 

> [!NOTE]
> Хотя доступ с несколькими протоколами в Data Lake Storage является общедоступным, поддержка некоторых из этих функций остается в предварительной версии. 

|Функция хранилища BLOB-объектов | Уровень поддержки |
|---|---|
|[Стильный уровень доступа](storage-blob-storage-tiers.md)|Общедоступная версия|
|API-интерфейсы RESTFUL для BLOB-объектов|Общедоступная версия|
|Пакеты SDK для BLOB-объектов |Общедоступная версия|
|[PowerShell (BLOB-объект)](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-powershell) |Общедоступная версия|
|[Интерфейс командной строки (BLOB)](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-cli) |Общедоступная версия|
|[Уведомления через службу "Сетка событий Azure"](data-lake-storage-events.md)|Общедоступная версия|
|Пакеты SDK для BLOB-объектов с семантикой файловой системы ([.net](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-directory-file-acl-dotnet) &vert; [Python](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-directory-file-acl-python) &vert; [Java](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-directory-file-acl-java))|Предварительная версия|
|[PowerShell с семантикой файловой системы](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-directory-file-acl-powershell)|Предварительная версия|
|[Интерфейс командной строки с семантикой файловой системы](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-directory-file-acl-cli)|Предварительная версия|
|[Журналы диагностики](../common/storage-analytics-logging.md)| Предварительная версия|
|[Политики управления жизненным циклом](storage-lifecycle-management-concepts.md)| Предварительная версия|
|[Архивный уровень доступа](storage-blob-storage-tiers.md)| Предварительная версия|
|[blobfuse](storage-how-to-mount-container-linux.md)|Еще не поддерживается|
|[Неизменяемое хранилище](storage-blob-immutable-storage.md)|Еще не поддерживается|
|[Моментальные снимки](storage-blob-snapshots.md)|Еще не поддерживается|
|[Обратимое удаление](storage-blob-soft-delete.md)|Еще не поддерживается|
|[Статические веб-сайты](storage-blob-static-website.md)|Еще не поддерживается|

Дополнительные сведения об общих проблемах и ограничениях, связанных с Azure Data Lake Storage 2-го поколения, см. в разделе [Известные проблемы](data-lake-storage-known-issues.md).

## <a name="azure-ecosystem-support"></a>Поддержка экосистемы Azure

Многопротокольный доступ на Data Lake Storage также позволяет подключать другие службы Azure к Data Lake Storage. В этой таблице перечислены службы, включенные с помощью многопротокольного доступа к Data Lake Storage. 

Как и список поддерживаемых функций хранилища BLOB-объектов, элементы, отображаемые в этой таблице, будут меняться со временем, так как поддержка служб Azure продолжится. 

> [!NOTE]
> Хотя доступ с несколькими протоколами в Data Lake Storage является общедоступным, поддержка некоторых из этих служб остается в предварительной версии. 

|Служба Azure | Уровень поддержки |
|---|---|
|[Azure Data Box](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|Общедоступная версия|
|[Запись концентраторов событий Azure](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|Общедоступная версия|
|[Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal)|Общедоступная версия|
|[Центр Интернета вещей](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|Общедоступная версия|
|[Приложения логики](https://azure.microsoft.com/services/logic-apps/)|Общедоступная версия|
|[Когнитивный поиск Azure](https://docs.microsoft.com/azure/search/search-howto-index-azure-data-lake-storage)|Предварительная версия|

Полный список поддержки для Data Lake Storage 2-го поколения в службе экосистемы Azure см. в статье [интеграция Azure Data Lake Storage со службами Azure](data-lake-storage-integrate-with-azure-services.md).

Дополнительные сведения об общих проблемах и ограничениях, связанных с Azure Data Lake Storage 2-го поколения, см. в разделе [Известные проблемы](data-lake-storage-known-issues.md).

## <a name="next-steps"></a>Дальнейшие действия

См. [Известные проблемы](data-lake-storage-known-issues.md)




