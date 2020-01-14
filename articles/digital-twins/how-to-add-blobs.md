---
title: Добавление больших двоичных объектов в объекты в Azure Digital двойников | Документация Майкрософт
description: Узнайте, как добавлять большие двоичные объекты для пользователей, устройств и пространств в Azure Digital двойников.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/10/2020
ms.custom: seodec18
ms.openlocfilehash: c85db05e6feeea43023c2391998f837348caed4e
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2020
ms.locfileid: "75929657"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Добавление больших двоичных объектов к объектам в Azure Digital Twins

Большие двоичные объекты представляют собой неструктурированные представления распространенных типов файлов, например изображения и журналы. Большие двоичные объекты отслеживают, какие данные они представляют с помощью типа MIME (например: image/jpeg) и метаданных (имя, описание, тип и т. д.).

Azure Digital Twins поддерживает присоединение больших двоичных объектов к устройствам, пространствам и пользователям. Большие двоичные объекты могут представлять фотографию профиля пользователя, фото устройства, видео, карты, индекс встроенного ПО, данные JSON или журналы.

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-overview"></a>Обзор отправки больших двоичных объектов

Вы можете использовать составные запросы для отправки больших двоичных объектов в определенные конечные точки и их соответствующие компоненты.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>Метаданные больших двоичных объектов

В дополнение к **Content-Type** и **Content-Disposition** составные запросы больших двоичных объектов Azure Digital Twins должны также указывать правильный текст JSON. Текст JSON для отправки зависит от типа выполняемой операции HTTP-запроса.

Ниже приведены четыре основные схемы JSON:

[![схем JSON](media/how-to-add-blobs/blob-models-swagger-img.png)](media/how-to-add-blobs/blob-models-swagger-img.png#lightbox)

Метаданные большого двоичного объекта JSON соответствуют следующей модели:

```JSON
{
    "parentId": "00000000-0000-0000-0000-000000000000",
    "name": "My First Blob",
    "type": "Map",
    "subtype": "GenericMap",
    "description": "A well chosen description",
    "sharing": "None"
  }
```

| attribute | Тип | Description |
| --- | --- | --- |
| **parentId** | String | Родительская сущность, с которой связывается большой двоичный объект (пространства, устройства или пользователи) |
| **name** |String | Понятное имя большого двоичного объекта |
| **type** | String | Нельзя использовать типы большого двоичного объекта *type* и *typeId*.  |
| **typeId** | Целое число | Нельзя использовать идентификатор типов большого двоичного объекта *type* и *typeId*. |
| **subtype** | String | Нельзя использовать подтипы большого двоичного объекта *subtype* и *subtypeId*. |
| **subtypeId** | Целое число | Нельзя использовать идентификатор подтипа большого двоичного объекта *subtype* и *subtypeId*. |
| **Описание** | String | Настраиваемое описание большого двоичного объекта |
| **Общий доступ** | String | Можно ли предоставить большой двоичный объект для общего доступа — перечисление [`None`, `Tree`, `Global`] |

Метаданные больших двоичных объектов всегда передаются в качестве первого фрагмента с **типом содержимого** `application/json` или в виде файла `.json`. Данные файла предоставляются во втором блоке и могут быть любого поддерживаемого типа MIME.

Эти схемы модели подробно описаны в документации Swagger.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Сведения об использовании справочной документации см. в статье [Использование Swagger с Digital Twins](./how-to-use-swagger.md).

### <a name="blobs-response-data"></a>Данные ответа большого двоичного объекта

Возвращаемые по отдельности большие двоичные объекты соответствуют следующей схеме JSON:

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "parentId": "00000000-0000-0000-0000-000000000000",
  "type": "string",
  "subtype": "string",
  "typeId": 0,
  "subtypeId": 0,
  "sharing": "None",
  "description": "string",
  "contentInfos": [
    {
      "type": "string",
      "sizeBytes": 0,
      "mD5": "string",
      "version": "string",
      "lastModifiedUtc": "2019-01-12T00:58:08.689Z",
      "metadata": {
        "additionalProp1": "string",
        "additionalProp2": "string",
        "additionalProp3": "string"
      }
    }
  ],
  "fullName": "string",
  "spacePaths": [
    "string"
  ]
}
```

| attribute | Тип | Description |
| --- | --- | --- |
| **идентификатор** | String | Уникальный идентификатор BLOB-объекта. |
| **name** |String | Понятное имя большого двоичного объекта |
| **parentId** | String | Родительская сущность, с которой связывается большой двоичный объект (пространства, устройства или пользователи) |
| **type** | String | Нельзя использовать типы большого двоичного объекта *type* и *typeId*.  |
| **typeId** | Целое число | Нельзя использовать идентификатор типов большого двоичного объекта *type* и *typeId*. |
| **subtype** | String | Нельзя использовать подтипы большого двоичного объекта *subtype* и *subtypeId*. |
| **subtypeId** | Целое число | Нельзя использовать идентификатор подтипа большого двоичного объекта *subtype* и *subtypeId*. |
| **Общий доступ** | String | Можно ли предоставить большой двоичный объект для общего доступа — перечисление [`None`, `Tree`, `Global`] |
| **Описание** | String | Настраиваемое описание большого двоичного объекта |
| **contentInfos** | Array | Указывает неструктурированную информацию о метаданных, включая версию |
| **fullName** | String | Полное имя большого двоичного объекта |
| **spacePaths** | String | Путь к пространству |

Метаданные больших двоичных объектов всегда передаются в качестве первого фрагмента с **типом содержимого** `application/json` или в виде файла `.json`. Данные файла предоставляются во втором блоке и могут быть любого поддерживаемого типа MIME.

### <a name="blob-multipart-request-examples"></a>Примеры составного запроса больших двоичных объектов

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Для отправки текстового файла в виде большого двоичного объекта и связывания его с пространством создайте аутентифицированный запрос HTTP POST:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs
```

Текст должен быть следующим:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "ParentId": "54213cf5-285f-e611-80c3-000d3a320e1e",
  "Name": "My First Blob",
  "Type": "Map",
  "SubType": "GenericMap",
  "Description": "A well chosen description",
  "Sharing": "None"
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="myblob.txt"
Content-Type: text/plain

This is my blob content. In this case, some text, but I could also be uploading a picture, an JSON file, a firmware zip, etc.

--USER_DEFINED_BOUNDARY--
```

| Значение | Заменить на |
| --- | --- |
| USER_DEFINED_BOUNDARY | Имя границы составного содержимого |

Приведенный ниже код представляет собой реализацию .NET для передачи одного и того же большого двоичного объекта с помощью класса [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metadata in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

//MY_BLOB.txt is the String representation of your text file
var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

Наконец, пользователи [cURL](https://curl.haxx.se/) могут выполнять запросы в составной форме таким же образом:

```bash
curl -X POST "YOUR_MANAGEMENT_API_URL/spaces/blobs" \
 -H "Authorization: Bearer YOUR_TOKEN" \
 -H "Accept: application/json" \
 -H "Content-Type: multipart/form-data" \
 -F "meta={\"ParentId\":\"YOUR_SPACE_ID\",\"Name\":\"My CURL Blob\",\"Type\":\"Map\",\"SubType\":\"GenericMap\",\"Description\":\"A well chosen description\",\"Sharing\":\"None\"};type=application/json" \
 -F "text=PATH_TO_FILE;type=text/plain"
```

| Значение | Заменить на |
| --- | --- |
| YOUR_TOKEN | Допустимый маркер OAuth 2.0 |
| YOUR_SPACE_ID | Идентификатор пространства для связи с большим двоичным объектом |
| PATH_TO_FILE | Путь к текстовому файлу |

[Пример ![ного прилистывания](media/how-to-add-blobs/http-blob-post-through-curl-img.png)](media/how-to-add-blobs/http-blob-post-through-curl-img.png#lightbox)

Успешная отправка возвращает идентификатор нового большого двоичного объекта.

## <a name="api-endpoints"></a>Конечные точки API

В следующих разделах описываются основные конечные точки API, связанные с большими двоичными объектами, и их функциональность.

### <a name="devices"></a>Устройства

Большие двоичные объекты можно подключать к устройствам. На следующем изображении представлена справочная документация по Swagger для ваших API управления. На нем указаны конечные точки API, связанные с устройством, для использования больших двоичных объектов и любые требуемые параметры пути, которые можно передать в них.

[![ные BLOB-объекты устройств](media/how-to-add-blobs/blobs-device-api-swagger-img.png)](media/how-to-add-blobs/blobs-device-api-swagger-img.png#lightbox)

Например, для обновления или создания большого двоичного объекта, а затем подключения объекта к устройству выполняется аутентифицированный запрос HTTP PATCH к:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Параметр | Заменить на |
| --- | --- |
| *YOUR_BLOB_ID* | Идентификатор требуемого большого двоичного объекта |

Успешно выполненные запросы возвращают объект JSON, как [было описано ранее](#blobs-response-data).

### <a name="spaces"></a>Пробелы

Большие двоичные объекты можно также подключать к пространствам. На представленном ниже рисунке перечислены все конечные точки API пространства, отвечающие за обработку больших двоичных объектов. Кроме того, указаны разные параметры пути, которые можно передать в эти конечные точки.

[![объемных BLOB-объектов](media/how-to-add-blobs/blobs-space-api-swagger-img.png)](media/how-to-add-blobs/blobs-space-api-swagger-img.png#lightbox)

Например, для возврата больших двоичных объектов, присоединенных к пространству, выполняется аутентифицированный запрос HTTP GET к:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Параметр | Заменить на |
| --- | --- |
| *YOUR_BLOB_ID* | Идентификатор требуемого большого двоичного объекта |

Успешно выполненные запросы возвращают объект JSON, как [было описано ранее](#blobs-response-data).

Запрос PATCH к той же конечной точке обновляет описание метаданных и создает версии большого двоичного объекта. HTTP-запрос выполняется с использованием метода PATCH вместе с любыми необходимыми метаданными и составными данными.

### <a name="users"></a>Пользователи

Большие двоичные объекты можно подключить к моделям пользователя (например, чтобы связать изображение профиля). На представленном ниже рисунке показаны соответствующие конечные точки API для пользователей и любые необходимые параметры пути, такие как `id`:

[![пользовательские BLOB-объекты](media/how-to-add-blobs/blobs-users-api-swagger-img.png)](media/how-to-add-blobs/blobs-users-api-swagger-img.png#lightbox)

Например, для получения большого двоичного объекта, подключенного к пользователю, выполняется аутентифицированный запрос HTTP GET (с необходимыми данными формы) к:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Параметр | Заменить на |
| --- | --- |
| *YOUR_BLOB_ID* | Идентификатор требуемого большого двоичного объекта |

Успешно выполненные запросы возвращают объект JSON, как [было описано ранее](#blobs-response-data).

## <a name="common-errors"></a>Распространенные ошибки

* Распространенной ошибкой является предоставление неправильной информации заголовка:

  ```JSON
  {
      "error": {
          "code": "400.600.000.000",
          "message": "Invalid media type in first section."
      }
  }
  ```

  Чтобы устранить эту ошибку, убедитесь, что во всем запросе имеется соответствующий заголовок **Content-Type**:

     * `multipart/mixed`
     * `multipart/form-data`

  Кроме того, убедитесь, что каждый *составной блок* имеет соответствующий **тип содержимого**.

* Вторая распространенная ошибка возникает при назначении нескольких больших двоичных объектов одному и тому же ресурсу в [графе пространственной аналитики](concepts-objectmodel-spatialgraph.md):

  ```JSON
  {
      "error": {
          "code": "400.600.000.000",
          "message": "SpaceBlobMetadata already exists."
      }
  }
  ```

  > [!NOTE]
  > Значение атрибута **Message** зависит от ресурса. 

  Только один большой двоичный объект (для каждого типа) может быть присоединен к каждому ресурсу в пространственном графе. 

  Чтобы устранить эту ошибку, обновите существующий большой двоичный объект с помощью соответствующей операции API HTTP PATCH. Это приведет к замене существующих данных большого двоичного объекта нужными данными.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о справочной документации Swagger для Azure Digital Twins см. в статье [Использование Swagger с Digital Twins](how-to-use-swagger.md).

- Чтобы отправить большие двоичные объекты через Postman, ознакомьтесь со статьей [о настройке Postman](./how-to-configure-postman.md).