---
title: 'Сетка событий Azure: безопасность и проверка подлинности'
description: В статье описываются сетка событий Azure и ее основные понятия.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: babanisa
ms.openlocfilehash: 87cfce6045ce84f83ca651472635227547c26ee9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2019
ms.locfileid: "66117022"
---
# <a name="event-grid-security-and-authentication"></a>Сетка событий: безопасность и проверка подлинности 

В сетке событий Azure предусмотрено три типа проверки подлинности:

* Доставка событий веб-перехватчика
* Подписки на события
* Публикация пользовательских разделов

## <a name="webhook-event-delivery"></a>Доставка событий веб-перехватчика

Веб-перехватчики — это один из многих способов получения событий из службы "Сетка событий Azure". При возникновении нового события служба "Сетка событий" отправляет в настроенную конечную точку HTTP-запрос, в тексте которого находится событие.

Как и многие другие службы, поддерживающие веб-перехватчики, служба "Сетка событий" требует, чтобы вы подтвердили право собственности на конечную точку веб-перехватчика до того, как служба начнет доставку событий в эту конечную точку. Это требование не позволяет пользователю-злоумышленнику переполнить вашу конечную точку событиями. Инфраструктура Azure автоматически обрабатывает эту проверку, когда вы используете любую из следующих трех служб Azure:

* Azure Logic Apps с [соединителем Сетки событий](https://docs.microsoft.com/connectors/azureeventgrid/);
* служба автоматизации Azure через [веб-перехватчик](../event-grid/ensure-tags-exists-on-new-virtual-machines.md);
* Функции Azure с [триггером службы "Сетка событий"](../azure-functions/functions-bindings-event-grid.md).

При использовании любого другого типа конечной точки, такого как функция Azure на основе триггера HTTP, код конечной точки должен участвовать в подтверждении проверки со службой "Сетка событий". Сетка событий поддерживает два типа проверки подписки.

1. **Подтверждение на основе кода проверки (программное подтверждение).** Рекомендуется использовать этот метод, если вы управляете исходным кодом для конечной точки. Во время создания подписки на события Сетка событий отправляет в вашу конечную точку событие проверки подписки. Схема этого события похожа на любое другое событие Сетки событий. Часть данных этого события включает свойство `validationCode`. Ваше приложение проверяет, предназначен ли запрос проверки для ожидаемой подписки на события, и возвращает код проверки в Сетку событий. Этот механизм подтверждения поддерживается во всех версиях Сетки событий.

2. **Подтверждение на основе URL-адреса проверки (подтверждение вручную).** В некоторых случаях вы не можете получить доступ к исходному коду конечной точки, чтобы реализовать подтверждение на основе кода проверки. Например, если вы используете стороннюю службу (например, [Zapier](https://zapier.com) или [IFTTT](https://ifttt.com/)), вы не можете ответить программным способом с использованием кода проверки.

   Начиная с версии 2018-05-01-preview Сетка событий поддерживает подтверждение проверки вручную. Если вы создаете подписку на события с помощью пакета SDK или инструмента, который использует версию API 2018-05-01-preview или более позднюю, Сетка событий отправит свойство `validationUrl` в часть данных события проверки подписки. Чтобы выполнить подтверждение, найдите URL-адрес в данных события и отправьте по нему запрос GET. Вы можете использовать клиент REST или свой веб-браузер.

   Предоставленный URL-адрес действителен в течение 5 минут. В течение этого времени состояние подготовки подписки на событие находится в `AwaitingManualAction`. Если вы не завершите проверку вручную в течение пяти минут, состояние подготовки имеет значение `Failed`. Прежде чем выполнить проверку вручную, потребуется создать подписку на событие еще раз.

    Этот механизм проверки подлинности также требуется конечная точка веб-перехватчик возвращает код состояния HTTP 200, таким образом, чтобы он знает, что запрос POST события проверки был принят, прежде чем его можно перевести в режим ручной проверки. Другими словами Если конечная точка возвращает 200, но ответ не возвращается обратно, проверки программным образом, режим переходе в режим ручной проверки. Если GET на URL-адрес проверки в течение пяти минут, считается, что подтверждение проверки успешной.

> [!NOTE]
> С использованием самозаверяющих сертификатов для проверки не поддерживается. Вместо этого используйте подписанного сертификата из центра сертификации (ЦС).

### <a name="validation-details"></a>Сведения о проверке

* Во время создания или обновления подписки на события Сетка событий публикует в целевую конечную точку событие проверки подписки. 
* В качестве заголовка событие содержит значение "aeg-event-type: SubscriptionValidation".
* Текст события имеет ту же схему, что и другие события сетки событий.
* Свойству события eventType соответствует значение `Microsoft.EventGrid.SubscriptionValidationEvent`.
* К свойству данных события относится свойство `validationCode` со строкой, сгенерированной случайным образом. Например "validationCode: acb13…".
* Данные о событии включают свойство `validationUrl` с URL-адресом для проверки подписки вручную.
* Массив содержит только событие проверки. Другие события отправляются в отдельном запросе после возврата кода проверки.
* Пакеты SDK EventGrid DataPlane содержат классы, соответствующие данным события проверки подписки и ответу на проверку подписки.

Пример SubscriptionValidationEvent показан в следующем примере:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "0000000000-0000-0000-0000-00000000000000",
    "validationUrl": "https://rp-eastus2.eventgrid.azure.net:553/eventsubscriptions/estest/validate?id=0000000000-0000-0000-0000-0000000000000&t=2018-04-26T20:30:54.4538837Z&apiVersion=2018-05-01-preview&token=1A1A1A1A"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Чтобы подтвердить владение конечной точкой, в свойстве validationResponse возвращается код проверки, как показано в следующем примере:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Должен возвратиться код состояния ответа HTTP 200 OK. HTTP 202 Accepted не распознается как действительный ответ проверки подписки Сетки событий.

Можно также вручную проверить подписку, отправив запрос GET по URL-адресу проверки. Подписка на событие остается в состоянии ожидания, пока проверка не будет завершена.

Пример обработки подтверждения проверки подписки на C# см. на [сайте GitHub](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs).

### <a name="checklist"></a>Контрольный список

Во время создания события подписки, если видите сообщение об ошибке, такие как «попытка проверить указанная конечная точка https:\//your-endpoint-here не удалось. Дополнительные сведения см. на https:\//aka.ms/esvalidation», значит, что в подтверждении проверки возникает сбой. Чтобы устранить эту ошибку, проверьте следующие аспекты:

* Вы можете управлять кодом приложения в целевой конечной точке? Например, когда вы пишете функцию Azure на основе триггера HTTP, есть ли у вас доступ к коду приложения для внесения в него изменений?
* Если у вас есть доступ к коду приложения, реализуйте механизм подтверждения на основе ValidationCode, как показано в приведенном выше примере.

* Если у вас нет доступа к коду приложения (например, если вы используете стороннюю службу, поддерживающую веб-перехватчики), можно использовать механизм подтверждения вручную. Убедитесь в том, что для получения validationUrl в событии проверки используется API версии 2018-05-01-preview или более поздняя версия (установите расширение Azure CLI для Сетки событий Azure). Чтобы выполнить подтверждение проверки вручную, получите значение свойства `validationUrl` и перейдите по этому URL-адресу в своем веб-браузере. Если проверка прошла успешно, то в веб-браузере появится сообщение об этом. Состояние параметра подписки на событие provisioningState изменится на "Успешно". 

### <a name="event-delivery-security"></a>Защита доставки событий

Защитить конечную точку веб-перехватчика можно путем добавления параметров запроса для URL-адреса веб-перехватчика при создании подписки на события. Задайте один из этих параметров запроса в качестве секрета, например [маркер доступа](https://en.wikipedia.org/wiki/Access_token). С его помощью веб-перехватчик сможет распознавать событие, поступающее из Сетки событий с допустимыми разрешениями. Сетка событий будет включать эти параметры в каждую доставку событий для веб-перехватчика.

При изменении подписки на событие, если в [интерфейсе командной строки](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) Azure не используется параметр [--include-full-endpoint-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show), параметры запроса не отображаются и не возвращаются.

Наконец, необходимо отметить, что служба "Сетка событий Azure" поддерживает только конечные точки веб-перехватчиков HTTPS.

## <a name="event-subscription"></a>Подписка на события

Чтобы подписаться на событие, необходимо предоставить доказательство, что вы обладаете доступом к источнику и обработчику события. В предыдущем разделе было описано, как доказать, что вы владеете веб-перехватчиком. При использовании обработчика событий, который не является веб-перехватчиком (например, концентратор событий или хранилище очередей), у пользователя возникает необходимость в доступе на запись к данному ресурсу. Данная проверка разрешений предотвращает попытки неавторизованных пользователей отправить события в ресурс.

Чтобы использовать ресурс, который является источником события, необходимо иметь разрешение **Microsoft.EventGrid/EventSubscriptions/Write**. Это разрешение необходимо, так как вы записываете новую подписку в области действия ресурса. Требуемый ресурс зависит от того, на какой раздел оформляется подписка: системный или пользовательский. В этом разделе описываются оба типа подписки.

### <a name="system-topics-azure-service-publishers"></a>Системные разделы (издатели служб Azure)

Для системных разделов необходимо разрешение на запись новой подписки на события в области действия ресурса, публикующего событие. Ресурс имеет следующий формат: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Например, чтобы подписаться на событие в учетной записи хранения с именем **myacct**, требуется разрешение Microsoft.EventGrid/EventSubscriptions/Write для следующего ресурса: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Пользовательские разделы

Для пользовательских разделов необходимо разрешение на запись новой подписки на события в области действия для сетки событий. Ресурс имеет следующий формат: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Например, чтобы подписаться на пользовательский раздел с именем **mytopic**, требуется разрешение Microsoft.EventGrid/EventSubscriptions/Write для следующего ресурса: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="custom-topic-publishing"></a>Публикация пользовательских разделов

Для пользовательских разделов используется либо подписанный URL-адрес (SAS), либо аутентификация с помощью ключа. Рекомендуется использовать SAS, но проверка подлинности с использованием ключа обеспечивает простое программирование, а также совместима со множеством существующих издателей веб-перехватчиков. 

Значение проверки подлинности следует включить в заголовок HTTP. Для SAS в качестве значения заголовка используйте **aeg-sas-token**. Для подлинности с использованием ключа в качестве значения заголовка следует использовать **aeg-sas-key**.

### <a name="key-authentication"></a>Проверка подлинности с использованием ключа

Проверка подлинности с использованием ключа — самая простая форма проверки подлинности. Используйте следующий формат: `aeg-sas-key: <your key>`

Например, для передачи ключа можно использовать следующую команду:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>Маркеры SAS

Маркеры SAS для сетки события включают ресурс, срок его действия и подпись. Маркер SAS имеет следующий формат: `r={resource}&e={expiration}&s={signature}`.

Ресурс — это путь для раздела сетки событий, в который вы отправляете события. Вот пример допустимого пути к ресурсу: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Вы создаете подпись на основе ключа.

Например, допустимое значение **aeg-sas-token** выглядит следующим образом:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

В следующем примере создается маркер SAS для использования с сеткой событий:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Контроль доступа к управлению

Сетка событий Azure позволяет контролировать уровень доступа, предоставленного разным пользователям для выполнения различных операций управления, таких как создание списка подписок на события, создание новых подписок и генерирование ключей. В службе "Сетка событий" используется управление доступом на основе ролей Azure (RBAC).

### <a name="operation-types"></a>Типы операций

Сетка событий поддерживает следующие действия:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Последние три операции возвращают потенциально секретную информацию, которая отфильтровывается из обычных операций чтения. Доступ к этим операциям рекомендуется ограничить. 

### <a name="built-in-roles"></a>Встроенные роли

"Сетка событий" предоставляет две встроенные роли для управления подписками на события. Эти роли необходимы для реализации [доменов событий](event-domains.md), потому что они предоставляют пользователям разрешения, необходимые для подписки на темы в домене событий. Эти роли предназначены для подписки на события и не предоставляют доступа к действиям, например для создания тем.

Вы можете [назначить эти роли для пользователя или группы](../role-based-access-control/quickstart-assign-role-user-portal.md).

**EventGrid EventSubscription Contributor (предварительная версия)**: управляет операциями подписки Сетки событий.

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

**EventGrid EventSubscription Reader (предварительная версия)**: позволяет читать подписки Сетки событий.

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

### <a name="custom-roles"></a>Пользовательские роли

Если вам нужно указать разрешения, которые отличаются от встроенных ролей, можно создать пользовательские роли.

Далее приведены примеры определений ролей Сетки событий, позволяющих пользователям выполнять различные действия. Эти пользовательские роли отличаются от встроенных, потому что они предоставляют более широкий доступ, чем просто подписка на события.

**EventGridReadOnlyRole.json**: разрешены только операции чтения.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: разрешен ограниченный набор действий по публикации и запрещены действия удаления.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: разрешено выполнять все действия Сетки событий.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

Пользовательские роли можно создавать с помощью [PowerShell](../role-based-access-control/custom-roles-powershell.md), [Azure CLI](../role-based-access-control/custom-roles-cli.md) и [REST](../role-based-access-control/custom-roles-rest.md).

## <a name="next-steps"></a>Дальнейшие действия

* Общие сведения о сетке событий см. в статье [Сведения о сетке событий](overview.md)
