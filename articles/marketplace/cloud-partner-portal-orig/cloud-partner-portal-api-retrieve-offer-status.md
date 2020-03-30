---
title: Получить статус предложения Лазурный рынок
description: API получает текущее состояние предложения.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 2f5211716145d6c05bbfb0132c4a6ba2f9cceabe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280513"
---
<a name="retrieve-offer-status"></a>Получение состояния предложения 
=====================

Получение текущего состояния предложения.

  `GET  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/status?api-version=2017-10-31`

<a name="uri-parameters"></a>Параметры универсального кода ресурса (URI)
--------------

|  **Название**       |   **Описание**                            |  **Тип данных** |
|  -------------  |  ------------------------------------------  |  ------------  |
|  publisherid    | Идентификатор издателя, например `Contoso`  |     Строка     |
|  offerId        | Глобальный уникальный идентификатор уникально идентифицирующий предложение      |     Строка     |
|  api-version    | API последней версии                        |     Дата       |
|  |  |


<a name="header"></a>Заголовок
------

|  name           |  Значение               |
|  -------------  | -------------------  |
|  Content-Type   |  `application/json`  |
|  Авторизация  | `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Пример текста запроса
------------

### <a name="response"></a>Ответ

``` json
  {
      "status": "succeeded",
      "messages": [],
      "steps": [
      {
          "estimatedTimeFrame": "< 15 min",
          "id": "displaydummycertify",
          "stepName": "Validate Pre-Requisites",
          "description": "Offer settings provided are validated.",
          "status": "complete",
          "messages": [
              {
                  "messageHtml": "Step completed.",
                  "level": "information",
                  "timestamp": "2018-03-16T17:50:45.7215661Z"
              }
          ],       
          "progressPercentage": 100
      },
      {
          "estimatedTimeFrame": "~2-3 days",
          "id": "displaycertify",
          "stepName": "Certification",
          "description": "Your offer is analyzed by our certification systems for issues.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 day",
          "id": "displayprovision",
          "stepName": "Provisioning",
          "description": "Your virtual machine is being replicated in our production systems.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "displaypackage",
          "stepName": "Packaging and Lead Generation Registration",
          "description": "Your virtual machine is being packaged for customers. Additionally, lead systems are being configured and set up.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "publisher-signoff",
          "stepName": "Publisher signoff",
          "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "~2-5 days",
          "id": "live",
          "stepName": "Live",
          "description": "Offer is publicly visible and is available for purchase.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      }
      ],
      "previewLinks": [],
      liveLinks": [],
      "notificationEmails": "jdoe@contoso.com"
  } 
```


### <a name="response-body-properties"></a>Свойства текста ответа

|  **Название**             |    **Описание**                                                                             |
| --------------------  |   -------------------------------------------------------------------------------------------- |
|  status               | Состояние предложения. Список возможных значений, см. ниже в разделе [Состояние предложения](#offer-status). |
|   отправляемых из облака на устройство             | Массив сообщений связанный с предложением                                                    |
|  steps                | Массив шагов, которые предлагает предложение во время публикации                      |
|  estimatedTimeFrame   | Оценка времени, необходимого для завершения этого шага в удобном формате                       |
|  идентификатор                   | Идентификатор шага                                                                         |
|  stepName             | Имя шага                                                                               |
|  description          | Описание шага                                                                        |
|  status               | Состояние шага. Список возможных значений, см. ниже в разделе [Состояние шага](#step-status).    |
|   отправляемых из облака на устройство             | Массив сообщений, относящихся к шагу                                                          |
|  processPercentage    | Процент выполнения шага                                                              |
|  previewLinks         | *В настоящее время не реализован*                                                                    |
|  liveLinks            | *В настоящее время не реализован*                                                                    |
|  notificationEmails   | Список адресов электронной почты, разделенных запятыми, для получения уведомлений о ходе операции        |
|  |  |


### <a name="response-status-codes"></a>Коды состояния ответа

| **Код** |   **Описание**                                                                                 |
| -------  |   ----------------------------------------------------------------------------------------------- |
|  200     |  `OK`. Запрос успешно обработан и текущее состояние предложения возвращено. |
|  400     | `Bad/Malformed request` — текст ответа ошибки может содержать дополнительные сведения.                 |
|  404     | `Not found` — указанная сущность не существует.                                                |
|  |  |


### <a name="offer-status"></a>Состояние предложения

|  **Название**                    |    **Описание**                                       |
|  --------------------------  |  ------------------------------------------------------  |
|  NeverPublished              | Предложение не было опубликовано.                          |
|  NotStarted                  | Предложение новое и не запущенное.                            |
|  WaitingForPublisherReview   | Предложение ожидает утверждения издателя.                 |
|  Запущен                     | Отправка предложения обрабатывается.                     |
|  Выполнено                   | Отправка предложения прекратила обработку.               |
|  Отменено                    | Отправка предложения была отменена.                           |
|  Ошибка                      | Не удалось отправить предложение.                                 |
|  |  |


### <a name="step-status"></a>Состояние шага

|  **Название**                    |    **Описание**                           |
|  -------------------------   |  ------------------------------------------  |
|  NotStarted                  | Выполнение шага не началось.                        |
|  InProgress                  | Шаг выполняется.                             |
|  WaitingForPublisherReview   | Шаг ожидает утверждения издателя.      |
|  WaitingForApproval          | Шаг ожидает утверждения процесса.        |
|  Заблокировано                     | Шаг заблокирован.                             |
|  Отклонено                    | Шаг отклонен.                            |
|  Завершение                    | Шаг завершен.                            |
|  Отменено                    | Шаг отменен.                           |
|  |  |
