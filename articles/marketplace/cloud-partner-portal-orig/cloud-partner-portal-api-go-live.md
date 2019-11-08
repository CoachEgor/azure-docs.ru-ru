---
title: Начать прямо | Azure Marketplace
description: API запуска инициирует динамический перечень процессов предложения.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 30500e9dfae9411563fc727290d0569998ba3550
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2019
ms.locfileid: "73819687"
---
<a name="go-live"></a>Запуск
=======

Этот API запускает процесс для принудительной отправки приложения рабочей среде. Выполнение этой операции занимает много времени. Этот вызов использует список уведомлений по электронной почте из [публикации](./cloud-partner-portal-api-publish-offer.md) операции API.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

<a name="uri-parameters"></a>Параметры универсального кода ресурса (URI)
--------------

|  **имя**      |   **Описание**                                                           | **Тип данных** |
|  --------      |   ---------------                                                           | ------------- |
| publisherid    | Идентификатор издателя для извлечения предложения, например `contoso`.       |  string       |
| OfferId        | Идентификатор извлекаемой записи предложения.                                   |  string       |
| api-version    | Последняя версия API                                                   |  Дата         |
|  |  |  |


<a name="header"></a>Заголовок
------

|  **имя**       |     **Значение**       |
|  ---------      |     ----------      |
| Content-Type    | `application/json`  |
| Авторизация   | `Bearer YOUR_TOKEN` |
|  |  |


<a name="body-example"></a>Пример текста
------------

### <a name="response"></a>Ответ

`Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Заголовок ответа

|  **имя**             |      **Значение**                                                            |
|  --------             |      ----------                                                           |
| Operation-Location    |  URL-адрес, который можно запросить для определения текущего состояния операции.            |
|  |  |


### <a name="response-status-codes"></a>Коды состояния ответа

| **Код** |  **Описание**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted`. Запрос был успешно принят. Ответ содержит расположение для отслеживания состояния операции. |
|  400     | `Bad/Malformed request` — дополнительные сведения об ошибке находятся в тексте ответа. |
|  404     |  `Not found` — указанная сущность не существует.                                       |
|  |  |
