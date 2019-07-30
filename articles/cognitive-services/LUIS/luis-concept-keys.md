---
title: Ключи подписки — LUIS
titleSuffix: Azure Cognitive Services
description: 'LUIS использует два ключа: бесплатный ключ разработки для создания модели и ключ конечной точки с тарифным планом для выполнения запросов к конечной точке прогнозирования с использованием фраз пользователя.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 69b4a92470b33ab6615eead16be044a88436d64e
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619764"
---
# <a name="authoring-and-query-prediction-endpoint-keys-in-luis"></a>Ключи разработки и запрашивания конечной точки прогнозирования в LUIS
В LUIS используются два ключа: [разработки](#programmatic-key) и [конечных точек](#endpoint-key). Ключ разработки формируется автоматически при создании учетной записи LUIS. Когда вы будете готовы опубликовать свое приложение LUIS, потребуется [создать ключ конечной точки](luis-how-to-azure-subscription.md), [назначить его](luis-how-to-azure-subscription.md) приложению LUIS и [использовать его в запросе к конечной точке](#use-endpoint-key-in-query). 

|Ключ|Цель|
|--|--|
|[Ключ разработки](#programmatic-key)|Разработка, публикация, управление участниками совместной работы, управление версиями|
|[Ключ конечной точки](#endpoint-key)| Выполнение запроса|

Важно разрабатывать приложение LUIS в [том же регионе](luis-reference-regions.md#publishing-regions), в котором вы хотите опубликовать его и выполнять запросы.

<a name="programmatic-key" ></a>

## <a name="authoring-key"></a>Ключ разработки

Ключ разработки, который также называется начальным ключом, создается автоматически при создании учетной записи LUIS и является бесплатным. У вас есть по одному ключу разработки для всех приложений LUIS в каждом [регионе](luis-reference-regions.md) разработки. Ключ разработки предоставляется для разработки приложения LUIS или для проверки отправки запросов к конечным точкам. 

Чтобы найти ключ разработки, войдите на [веб-сайт LUIS](luis-reference-regions.md#luis-website) и щелкните имя учетной записи на панели навигации справа вверху, чтобы открыть **Параметры учетной записи**.

![Ключ разработки](./media/luis-concept-keys/programatic-key.png)

Если вы хотите отправлять запросы к **рабочей конечной точке**, создайте [подписку LUIS](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) в Azure. 

> [!CAUTION]
> Для удобства во многих примерах используется ключ разработки, так как в [квоту](luis-boundaries.md#key-limits) этого ключа входят несколько вызовов к конечным точкам.  

## <a name="endpoint-key"></a>Ключ конечной точки
При необходимости **запросов к рабочей конечной точки**, создайте ресурс Azure, а затем назначьте его для приложения LUIS. 

[!INCLUDE [Azure resource creation for Language Understanding and Cognitive Service resources](../../../includes/cognitive-services-luis-azure-resource-instructions.md)]

Создав ресурс Azure, [назначьте ключ](luis-how-to-azure-subscription.md) приложению. 

* Ключ конечной точки предоставляет квоту обращений к конечной точке, которая зависит от плана использования, указанного при создании ключа. Сведения о ценах см. в разделе [Цены на Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h).

* Ключ конечной точки можно использовать для всех приложений LUIS или для конкретных приложений LUIS. 

* Не используйте ключ конечной точки для разработки приложений LUIS. 

## <a name="use-endpoint-key-in-query"></a>Использование ключа конечной точки в запросе
Конечная точка LUIS принимает два стиля запроса. Ключ конечной точки используется в каждом из них, но в различных местах:

|Команда|Пример URL-адреса и расположение ключа|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`<br><br>значение строки запроса для `subscription-key`<br><br>Чтобы использовать квоту конечной точки LUIS, измените значение запроса конечной точки для `subscription-key` с ключа разработки (начального ключа) на новый ключ конечной точки. Если создать и назначать ключ, но не изменять значение запроса конечной точки для `subscription-key`, вы не будете использовать квоту ключа конечной точки.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`<br><br> значение заголовка для `Ocp-Apim-Subscription-Key`<br><br>Чтобы использовать квоту конечной точки LUIS, измените значение запроса конечной точки для `Ocp-Apim-Subscription-Key` с ключа разработки (начального ключа) на новый ключ конечной точки. Если создать и назначать ключ, но не изменять значение запроса конечной точки для `Ocp-Apim-Subscription-Key`, вы не будете использовать квоту ключа конечной точки.|

Идентификатор приложения, используемый в предыдущих URL-адресах, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, является общедоступным приложением Интернета вещей, используемым для [интерактивной демонстрации](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Использование API Ocp-Apim-Subscription-Key
API-интерфейсы LUIS используют заголовок `Ocp-Apim-Subscription-Key`. Имя заголовка не изменяется в зависимости от ключа и набора API, которые вы используете. Для API разработки установите ключ разработки в качестве заголовка. Если вы используете конечную точку, установите ключ конечной точки в качестве заголовка. 

Вы не можете использовать ключ конечной точки для API разработки. При попытке выполнить это действие, появится сообщение об ошибке 401 ("Доступ запрещен, указан недопустимый ключ конечной точки"). 

## <a name="key-limits"></a>Ограничения ключа
См. разделы [Ограничения ключа](luis-boundaries.md#key-limits) и [Регионы Azure](luis-reference-regions.md). Ключ разработки бесплатен и используется для разработки. Для ключа конечной точки LUIS есть уровень "Бесплатный", но этот ключ должен создаваться вами, и его необходимо связать с приложением LUIS на странице **Публикация**. Он не может использоваться для разработки, а только для запросов к конечным точкам.

Регионы публикации отличаются от регионов разработки. Убедитесь, что вы разрабатываете приложение в регионе разработки, соответствующем желаемому региону публикации.

## <a name="key-limit-errors"></a>Ошибки ограничений ключей
При превышении заданного значения квоты в секунду вы получите ошибку HTTP 429. При превышении заданного значения квоты в месяц вы получите ошибку HTTP 403. Чтобы исправить эти ошибки, получите ключ [конечной точки](#endpoint-key) LUIS и [назначьте](luis-how-to-azure-subscription.md) его приложению на странице **Публикация** веб-сайта [LUIS](luis-reference-regions.md#luis-website).

## <a name="assignment-of-the-endpoint-key"></a>Назначение ключа конечной точки

Вы можете [назначить](luis-how-to-azure-subscription.md) ключ конечной точки на [портале LUIS](https://www.luis.ai) или с помощью соответствующих API. 


## <a name="next-steps"></a>Следующие шаги

* Ознакомьтесь с [понятиями](luis-how-to-azure-subscription.md), связанными с ключами разработки и конечных точек.
