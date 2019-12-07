---
title: Управление сущностями служб мультимедиа с помощью REST | Документация Майкрософт
description: В этой статье показано, как управлять сущностями служб мультимедиа с помощью REST API.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 95262a32-0f2a-4286-b9e2-1a1ca6399b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 1ecbca99a65d99cc3b8d842a7b61d858398a80ff
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74885710"
---
# <a name="managing-media-services-entities-with-rest"></a>Управление сущностями служб мультимедиа с помощью REST  

> [!div class="op_single_selector"]
> * [REST](media-services-rest-manage-entities.md)
> * [.NET](media-services-dotnet-manage-entities.md)
> 
> 

Службы мультимедиа Microsoft Azure — это служба на основе REST, построенная на базе OData 3. Вы можете добавлять, запрашивать, обновлять и удалять сущности, как и в любой другой службе OData. Исключения будут выделены, когда это понадобится. Дополнительные сведения об OData см. в [документации по протоколу Open Data Protocol](https://www.odata.org/documentation/).

В этой статье показано, как управлять сущностями служб мультимедиа с помощью REST.

>[!NOTE]
> Начиная с 1 апреля 2017 г. все записи задания в вашей учетной записи и связанные с ней записи задач старше 90 дней будут автоматически удалены, даже если общее число записей не превышает значение максимальной квоты. Например, 1 апреля 2017 г. будет автоматически удалена любая запись задания в вашей учетной записи, созданная ранее 31 декабря 2016 г. Если необходимо архивировать данные задания или задачи, то можно использовать код, описанный в этой статье.

## <a name="considerations"></a>Рекомендации  

При доступе к сущностям в службах мультимедиа необходимо задать определенные поля и значения заголовков в HTTP-запросах. Дополнительную информацию см. в статье [Обзор интерфейса REST API служб мультимедиа](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Подключение к службам мультимедиа

См. дополнительные сведения о [получении доступа к API служб мультимедиа Azure с помощью аутентификации Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="adding-entities"></a>Добавление сущностей
Каждая сущность в службах мультимедиа добавляется в набор сущностей, например активы (Assets), посредством запроса HTTP POST.

В следующем примере показано, как создать AccessPolicy.

    POST https://media.windows.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

## <a name="querying-entities"></a>Запрашивание сущностей
Запрашивание и перечисление сущностей выполняется просто. Для этого используется только запрос HTTP GET и необязательные операции OData.
В следующем примере извлекается список всех сущностей MediaProcessor.

    GET https://media.windows.net/API/MediaProcessors HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Кроме того, вы можете получить конкретную сущность или все наборы сущностей, связанные с определенной сущностью, как в приведенных ниже примерах.

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c')/TaskTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Следующий пример возвращает только свойство State всех сущностей Jobs.

    GET https://media.windows.net/API/Jobs?$select=State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

В следующем примере возвращаются все объекты JobTemplate с именем "SampleTemplate".

    GET https://media.windows.net/API/JobTemplates?$filter=startswith(Name,%20'SampleTemplate') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

> [!NOTE]
> Операция $expand не поддерживается в службах мультимедиа, так же как и неподдерживаемые методы LINQ, описанные в разделе "Рекомендации по LINQ (службы WCF Data Services)".
> 
> 

## <a name="enumerating-through-large-collections-of-entities"></a>Перечисление больших коллекций сущностей
При запросе сущностей существует ограничение в 1000 сущностей, возвращаемых за один раз, так как в открытой версии 2 REST количество результатов запросов ограничено 1000. Используйте **skip** и **top** для перебора больших коллекций объектов. 

В следующем примере показано, как с помощью ссылок **skip** и **top** пропустить первые 2000 заданий и получить следующую 1000 заданий.  

    GET https://media.windows.net/api/Jobs()?$skip=2000&$top=1000 HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net

## <a name="updating-entities"></a>Обновление сущностей
В зависимости от типа сущности и состояния, в котором она находится, можно обновить свойства этой сущности с помощью HTTP-запросов PATCH, PUT или MERGE. Дополнительные сведения об этих операциях см. [здесь](https://msdn.microsoft.com/library/dd541276.aspx).

В следующем примере кода показано, как обновить свойство Name сущности Asset.

    MERGE https://media.windows.net/API/Assets('nb:cid:UUID:80782407-3f87-4e60-a43e-5e4454232f60') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net
    Content-Length: 21
    Expect: 100-continue

    {"Name" : "NewName" }

## <a name="deleting-entities"></a>Удаление сущностей
Сущности в службах мультимедиа можно удалить с помощью запроса HTTP DELETE. В зависимости от сущности порядок, в котором удаляются сущности, может быть важным. Например, чтобы удалить сущность наподобие Asset, нужно перед этим отозвать (или удалить) все указатели (Locator), которые ссылаются на эту сущность.

В приведенном ниже примере показан способ удаления сущности Locator (указатель), которая использовалась для отправки файла в хранилище больших двоичных объектов.

    DELETE https://media.windows.net/API/Locators('nb:lid:UUID:76dcc8e8-4230-463d-97b0-ce25c41b5c8d') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net
    Content-Length: 0

## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отправить отзыв
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

