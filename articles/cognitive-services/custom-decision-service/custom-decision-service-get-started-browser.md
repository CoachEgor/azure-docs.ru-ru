---
title: Вызов API из браузера — пользовательская служба принятия решений
titlesuffix: Azure Cognitive Services
description: Узнайте как оптимизировать веб-страницы с помощью вызовов API непосредственно из браузера в Пользовательскую службу принятия решений.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: slivkins
ROBOTS: NOINDEX
ms.openlocfilehash: 28ad4681242765bf2da9b1f13dc828e23cce1794
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707269"
---
# <a name="call-api-from-a-browser"></a>Вызов API из браузера

В статье описано, как выполнять вызовы API-интерфейсов Пользовательской службы принятия решений Azure непосредственно из браузера.

Не забудьте сначала [зарегистрировать приложение](custom-decision-service-get-started-register.md).

Давайте приступим. Модель приложения включает основную страницу, на которой есть ссылки на несколько страниц со статьями. На этой странице используется Пользовательская служба принятия решений для упорядочивания страниц статей. Вставьте следующий код в заголовок HTML основной страницы:

```html
// Define the "callback function" to render UI
<script> function callback(data) { … } </script>

// call Ranking API, after callback() is defined
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>" async></script>
```

Аргумент `data` ранжирует отображаемые URL-адреса. Дополнительные сведения см. в справке по [API](custom-decision-service-api-reference.md).

Для обработки выполняемого пользователем выбора верхней статьи (щелчка) вызовите следующий код на основной странице:

```javascript
// call Reward API to report a click
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Здесь `data` — аргумент функции `callback()`. Пример реализации можно найти в этом [руководстве](custom-decision-service-tutorial-news.md#use-the-apis).

Наконец, необходимо указать API набора действий, который возвращает список статей (действий), обрабатываемых Пользовательской службой принятия решений. Реализуйте этот API в виде RSS-канала, как показано ниже:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <pubDate>Thu, 27 Apr 2017 16:30:52 GMT</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Здесь каждый элемент `<item>` верхнего уровня описывает одну статью. Элемент `<link>` является обязательным и используется Пользовательской службой принятия решений как идентификатор действия. При наличии более 15 статей укажите `<date>` (в стандартном формате RSS). Используются 15 последних статей. Элемент `<title>` необязателен и используется для создания связанных с текстом признаков статьи.

## <a name="next-steps"></a>Следующие шаги

* Изучите [руководство](custom-decision-service-tutorial-news.md) с более подробным примером.
* Обратитесь к справочнику по [API](custom-decision-service-api-reference.md), чтобы получить дополнительные сведения о предоставляемых возможностях.
