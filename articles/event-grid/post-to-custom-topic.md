---
title: Публикация события в пользовательском разделе для службы "Сетка событий Azure"
description: В этой статье объясняется, как опубликовать событие в пользовательском разделе. В ней также представлен формат записей и данные событий.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 0afad249f71a36bf7552da499e985b68d48ee7a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721563"
---
# <a name="post-to-custom-topic-for-azure-event-grid"></a>Публикация в пользовательском разделе для службы "Сетка событий Azure"

В этой статье объясняется, как опубликовать событие в пользовательском разделе. В ней также представлен формат записей и данные событий. [Соглашение об уровне обслуживания (SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/) применяется только к записям, которые соответствуют ожидаемому формату.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="endpoint"></a>Конечная точка

При отправке запроса HTTP POST в пользовательский раздел используйте URI в таком формате: `https://<topic-endpoint>?api-version=2018-01-01`.

Например, допустимый URI — `https://exampletopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01`.

Чтобы получить конечную точку для пользовательского раздела с помощью Azure CLI, используйте следующую команду:

```azurecli-interactive
az eventgrid topic show --name <topic-name> -g <topic-resource-group> --query "endpoint"
```

Чтобы получить конечную точку для пользовательского раздела с помощью Azure PowerShell, используйте следующую команду:

```powershell
(Get-AzEventGridTopic -ResourceGroupName <topic-resource-group> -Name <topic-name>).Endpoint
```

## <a name="header"></a>Заголовок

Добавьте в запрос значение заголовка `aeg-sas-key`, содержащее ключ для аутентификации.

Например, допустимое значение заголовка — `aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==`.

Чтобы получить ключ для пользовательского раздела с помощью Azure CLI, используйте следующую команду:

```azurecli
az eventgrid topic key list --name <topic-name> -g <topic-resource-group> --query "key1"
```

Чтобы получить ключ для пользовательского раздела с помощью PowerShell, используйте следующую команду:

```powershell
(Get-AzEventGridTopicKey -ResourceGroupName <topic-resource-group> -Name <topic-name>).Key1
```

## <a name="event-data"></a>Данные событий

Для пользовательского раздела данные верхнего уровня должны содержать те же поля, что и стандартные определенные события ресурса. Одно из этих свойств — свойство data, содержащее свойства, уникальные для пользовательского раздела. Свойства для этого объекта data определяет издатель событий. Используйте следующую схему:

```json
[
  {
    "id": string,    
    "eventType": string,
    "subject": string,
    "eventTime": string-in-date-time-format,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string
  }
]
```

Описание этих свойств см. в статье [Схема событий службы "Сетка событий Azure"](event-schema.md). При отправке событий в службу "Сетка событий" общий размер массива не может превышать 1 МБ. Каждое событие в массиве ограничено 64 КБ (общая доступность) или 1 МБ (предварительный просмотр).

> [!NOTE]
> Событие размером до 64 КБ покрывается Соглашением об уровне общего доступа (GA) (SLA). Поддержка события размером до 1 МБ в настоящее время находится в предварительном просмотре. События более 64 КБ взимается с шагом 64-КБ. 

Ниже приведен пример допустимой схемы данных события:

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0"
}]
```

## <a name="response"></a>Ответ

После отправки данных в конечную точку раздела вы получите ответ. Ответ — это стандартный код ответа HTTP. Ниже приведены некоторые распространенные ответы:

|Результат  |Ответ  |
|---------|---------|
|Успешно  | 200 ОК  |
|Неправильный формат данных события | 400 — недопустимый запрос |
|Недопустимый ключ доступа | 401 — недостаточно прав |
|Неправильная конечная точка | 404 — не найдено |
|Массив или событие превышает допустимый размер | 413 (слишком большой объем полезных данных) |

Текст сообщений об ошибках имеет следующий формат:

```json
{
    "error": {
        "code": "<HTTP status code>",
        "message": "<description>",
        "details": [{
            "code": "<HTTP status code>",
            "message": "<description>"
    }]
  }
}
```

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь со сведениями о [мониторинге доставки сообщений в службе "Сетка событий"](monitor-event-delivery.md).
* Дополнительные сведения о ключе аутентификации см. в статье [Сетка событий: безопасность и проверка подлинности](security-authentication.md).
* Дополнительные сведения о создании подписки на Сетку событий Azure см. в статье [Схема подписки для службы "Сетка событий"](subscription-creation-schema.md).
