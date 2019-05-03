---
title: Обработка ошибок в политиках управления API Azure | Документация Майкрософт
description: Информация о том, как следует реагировать на ошибки, возникающие во время обработки запросов в службе управления API Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: apimpm
ms.openlocfilehash: 2bde63bb668188936b3dd3cf5ecbf3b8c604eb95
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60564329"
---
# <a name="error-handling-in-api-management-policies"></a>Обработка ошибок в политиках управления API

Предоставляя объект `ProxyError`, служба управления API Azure позволяет издателям реагировать на ошибки, которые могут возникать во время обработки запросов. Доступ к объекту `ProxyError` осуществляется через свойство [context.LastError](api-management-policy-expressions.md#ContextVariables), и его могут использовать политики в разделе `on-error`. Эта статья содержит справочную информацию о возможностях обработки ошибок, которые предоставляет служба управления API Azure.  
  
## <a name="error-handling-in-api-management"></a>Обработка ошибок в службе управления API

 Политики в службе управления API Azure включают разделы `inbound`, `backend`, `outbound` и `on-error`, как показано в следующем примере.  
  
```xml  
<policies>  
  <inbound>  
    <!-- statements to be applied to the request go here -->  
  </inbound>  
  <backend>  
    <!-- statements to be applied before the request is   
         forwarded to the backend service go here -->  
    </backend>  
    <outbound>  
      <!-- statements to be applied to the response go here -->  
    </outbound>  
    <on-error>  
        <!-- statements to be applied if there is an error   
             condition go here -->  
  </on-error>  
</policies>  
```  
  
Во время обработки запроса встроенные действия выполняются с соблюдением всех политик в области запроса. Если возникает ошибка, обработка немедленно переходит к разделу `on-error` соответствующей политики.  
Раздел политики `on-error` можно использовать в любой области. Издатели API могут настроить требуемые действия, например запись информации об ошибках в концентраторы событий или создание нового ответа для передачи вызывающему объекту.  
  
> [!NOTE]
>  Раздел `on-error` в политиках отсутствует по умолчанию. Чтобы добавить в политику раздел `on-error`, перейдите к требуемой политике в редакторе политик и добавьте раздел. Дополнительные сведения о настройке политик см. в статье [Политики в Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/).  
>   
>  Если в политике нет раздела `on-error`, вызывающий объект при возникновении ошибки получит сообщение с HTTP-кодом 400 или 500.  
  
### <a name="policies-allowed-in-on-error"></a>Политики, которые можно использовать в разделе on-error

 В разделе `on-error` можно использовать следующие политики.  
  
-   [choose](api-management-advanced-policies.md#choose)  
-   [set-variable](api-management-advanced-policies.md#set-variable)  
-   [find-and-replace](api-management-transformation-policies.md#Findandreplacestringinbody)  
-   [return-response](api-management-advanced-policies.md#ReturnResponse)  
-   [set-header](api-management-transformation-policies.md#SetHTTPheader)  
-   [set-method](api-management-advanced-policies.md#SetRequestMethod)  
-   [set-status](api-management-advanced-policies.md#SetStatus)  
-   [send-request](api-management-advanced-policies.md#SendRequest)  
-   [send-one-way-request](api-management-advanced-policies.md#SendOneWayRequest)  
-   [log-to-eventhub](api-management-advanced-policies.md#log-to-eventhub)  
-   [json-to-xml](api-management-transformation-policies.md#ConvertJSONtoXML)  
-   [xml-to-json](api-management-transformation-policies.md#ConvertXMLtoJSON)  
  
## <a name="lasterror"></a>lastError

 Когда возникает ошибка и управление переходит к разделу политики `on-error`, информация об ошибке сохраняется в свойстве [context.LastError](api-management-policy-expressions.md#ContextVariables), к которому могут обратиться политики, определенные в разделе `on-error`. LastError имеет следующие свойства.  
  
| ИМЯ     | Тип   | Описание                                                                                               | Обязательно для заполнения |
|----------|--------|-----------------------------------------------------------------------------------------------------------|----------|
| `Source`   | string | Указывает имя элемента, в котором произошла ошибка. Это может быть имя политики или встроенного шага конвейера.     | Да      |
| `Reason`   | string | Код ошибки в машинном формате, который удобно использовать для обработки ошибок.                                       | Нет       |
| `Message`  | string | Описание ошибки в понятном для человека формате.                                                                         | Да      |
| `Scope`    | string | Имя области, в которой возникла ошибка. Здесь возможны следующие значения: global, product, api или operation. | Нет       |
| `Section`  | string | Имя раздела, в котором произошла ошибка. Возможные значения: inbound, backend, outbound или on-error.       | Нет       |
| `Path`     | string | Задает вложенные политики, например: choose[3]/when[2].                                                        | Нет       |
| `PolicyId` | string | Значение атрибута `id` для политики, в которой произошла ошибка (если указано клиентом).             | Нет       |

> [!TIP]
> Доступ к коду состояния можно получить с помощью context.Response.StatusCode.  

> [!NOTE]
> Все политики имеют дополнительный атрибут `id`, который может быть добавлен к корневому элементу политики. Если этот атрибут присутствует в политике, в которой возникает ошибка, его значение можно извлечь с помощью свойства `context.LastError.PolicyId`.

## <a name="predefined-errors-for-built-in-steps"></a>Стандартные ошибки для встроенных шагов  
 Далее перечислены стандартные ошибки, которые могут возникать во время оценки встроенных шагов обработки.  
  
| `Source`        | Условие                                 | `Reason`                  | `Message`                                                                                                                |
|---------------|-------------------------------------------|-------------------------|------------------------------------------------------------------------------------------------------------------------|
| конфигурация | URI не соответствует ни одному API или операции | OperationNotFound       | Unable to match incoming request to an operation. (Не удалось сопоставить входящий запрос с операцией.)                                                                      |
| авторизация | Не предоставлен ключ подписки             | SubscriptionKeyNotFound | Access denied due to missing subscription key. Make sure to include subscription key when making requests to this API. (Доступ запрещен из-за отсутствия ключа подписки. Обязательно включайте ключ подписки в запросы к этому API.) |
| авторизация | Недопустимое значение ключа подписки         | SubscriptionKeyInvalid  | Access denied due to invalid subscription key. Make sure to provide a valid key for an active subscription. (Доступ запрещен из-за недопустимого ключа подписки. Укажите допустимый ключ активной подписки.)            |
  
## <a name="predefined-errors-for-policies"></a>Стандартные ошибки для политик  
 Далее перечислены стандартные ошибки, которые могут возникнуть во время оценки политик.  
  
| `Source`       | Условие                                                       | `Reason`                    | `Message`                                                                                                                              |
|--------------|-----------------------------------------------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| rate-limit   | Превышено ограничение скорости                                             | RateLimitExceeded         | Rate limit is exceeded (Превышено ограничение скорости)                                                                                                               |
| квота        | Превышена квота                                                  | QuotaExceeded             | превышена квота на количество вызовов. Quota will be replenished in xx:xx:xx. (Квота будет пополнена в xx:xx:xx.) -или- Out of bandwidth quota. (Превышена квота пропускной способности.) Quota will be replenished in xx:xx:xx. (Квота будет пополнена в xx:xx:xx.) |
| jsonp        | Недопустимое значение параметра обратного вызова (содержит неправильные символы) | CallbackParameterInvalid  | Value of callback parameter {callback-parameter-name} is not a valid JavaScript identifier. (Значение параметра обратного вызова {имя параметра обратного вызова} не является допустимым идентификатором JavaScript.)                                          |
| ip-filter    | Не удалось проанализировать IP-адрес вызывающего объекта из запроса                          | FailedToParseCallerIP     | Failed to establish IP address for the caller. Access denied. (Не удалось установить IP-адрес вызывающего объекта. Доступ запрещен.)                                                                        |
| ip-filter    | IP-адрес вызывающего объекта не входит в список разрешенных                                | CallerIpNotAllowed        | Caller IP address {ip-address} is not allowed. Access denied. (Недопустимый IP-адрес вызывающего объекта: {IP-адрес}. Доступ запрещен.)                                                                        |
| ip-filter    | IP-адрес вызывающего объекта включен в список заблокированных                                    | CallerIpBlocked           | Caller IP address is blocked. Access denied. (IP-адрес вызывающего объекта заблокирован. Доступ запрещен.)                                                                                         |
| check-header | Отсутствует обязательный заголовок или его значение               | HeaderNotFound            | Header {header-name} was not found in the request. Access denied. (Заголовок {имя_заголовка} не найден в запросе. Доступ запрещен.)                                                                    |
| check-header | Отсутствует обязательный заголовок или его значение               | HeaderValueNotAllowed     | Header {header-name} value of {header-value} is not allowed. Access denied. (Недопустимое значение {значение_заголовка} для заголовка {имя_заголовка}. Доступ запрещен.)                                                          |
| validate-jwt | В запросе отсутствует маркер JWT                                 | TokenNotFound             | JWT not found in the request. Access denied. (В запросе не найден маркер JWT. Доступ запрещен.)                                                                                         |
| validate-jwt | Ошибка при проверке подписи                                     | TokenSignatureInvalid     | <сообщение из библиотеки jwt\>. Access denied. (Доступ запрещен.)                                                                                          |
| validate-jwt | Недопустимая аудитория                                                | TokenAudienceNotAllowed   | <сообщение из библиотеки jwt\>. Access denied. (Доступ запрещен.)                                                                                          |
| validate-jwt | Недопустимый издатель                                                  | TokenIssuerNotAllowed     | <сообщение из библиотеки jwt\>. Access denied. (Доступ запрещен.)                                                                                          |
| validate-jwt | Истек срок действия маркера                                                   | TokenExpired              | <сообщение из библиотеки jwt\>. Access denied. (Доступ запрещен.)                                                                                          |
| validate-jwt | Ключ подписи не удалось разрешить по идентификатору                            | TokenSignatureKeyNotFound | <сообщение из библиотеки jwt\>. Access denied. (Доступ запрещен.)                                                                                          |
| validate-jwt | В маркере отсутствуют необходимые утверждения                          | TokenClaimNotFound        | JWT token is missing the following claims: <c1\>, <c2\>, … Access denied. (В маркере JWT отсутствуют следующие утверждения: <c1>, <c2>, … Доступ запрещен.)                                                            |
| validate-jwt | Несоответствие значений утверждения                                           | TokenClaimValueNotAllowed | Claim {claim-name} value of {claim-value} is not allowed. Access denied. (Недопустимое значение {значение_утверждения} для утверждения {имя_утверждения}. Доступ запрещен.)                                                             |
| validate-jwt | Прочие сбои при проверке данных                                       | JwtInvalid                | <сообщение из библиотеки jwt\>                                                                                                          |

## <a name="example"></a>Пример

Если задать политику API

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
    <on-error>
        <set-header name="ErrorSource" exists-action="override">
            <value>@(context.LastError.Source)</value>
        </set-header>
        <set-header name="ErrorReason" exists-action="override">
            <value>@(context.LastError.Reason)</value>
        </set-header>
        <set-header name="ErrorMessage" exists-action="override">
            <value>@(context.LastError.Message)</value>
        </set-header>
        <set-header name="ErrorScope" exists-action="override">
            <value>@(context.LastError.Scope)</value>
        </set-header>
        <set-header name="ErrorSection" exists-action="override">
            <value>@(context.LastError.Section)</value>
        </set-header>
        <set-header name="ErrorPath" exists-action="override">
            <value>@(context.LastError.Path)</value>
        </set-header>
        <set-header name="ErrorPolicyId" exists-action="override">
            <value>@(context.LastError.PolicyId)</value>
        </set-header>
        <set-header name="ErrorStatusCode" exists-action="override">
            <value>@(context.Response.StatusCode.ToString())</value>
        </set-header>
        <base />
    </on-error>
</policies>
```

и отправить неавторизованный запрос, появится следующий ответ:

![Ответ при ошибке авторизации](media/api-management-error-handling-policies/error-response.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о работе с политиками см. в следующих статьях:

+ [Политики в управлении API](api-management-howto-policies.md)
+ [Преобразование API-интерфейсов](transform-api.md).
+ Полный перечень операторов политик и их параметров см. в [справочнике по политикам](api-management-policy-reference.md).
+ [Примеры политик](policy-samples.md).   