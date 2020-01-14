---
title: Что такое API Поиска видео Bing
titleSuffix: Azure Cognitive Services
description: Узнайте, как искать видео в Интернете с помощью API Bing для поиска видео.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: 8377f0f5d586212c94bb763598b6e7a9e391073c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75382724"
---
# <a name="what-is-the-bing-video-search-api"></a>Что такое API Поиска видео Bing

API Bing для поиска видео позволяет легко добавлять возможности поиска видео в службы и приложения. Отправляя пользовательские поисковые запросы с помощью API, можете получить и отобразить соответствующие и высококачественные видео, которые аналогичны [Видео Bing](https://www.bing.com/video). Используйте этот API для результатов поиска, которые содержат только видео. [API Bing для поиска видео](../bing-web-search/search-the-web.md) может возвращать другие типы веб-содержимого, включая веб-страницы, видео, новости и изображения.

## <a name="bing-video-search-api-features"></a>Функции API Bing для поиска видео

| Компонент                                                                                                                                                                                 | Description                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Предложение условий поиска в режиме реального времени](concepts/sending-requests.md#suggest-search-terms-with-the-bing-autosuggest-api) | Улучшите работу приложения, используя [API автозаполнения Bing](../bing-autosuggest/get-suggested-search-terms.md), отображающий предлагаемые условия поиска по мере их ввода. |
| [Фильтрация и ограничение результатов поиска видео](concepts/get-videos.md#filtering-videos)                      | Отфильтруйте возвращаемые видео путем изменения параметров запроса.                                                                                                       |
| [Обрезка, изменения размера и отображение эскизов](../bing-web-search/resize-and-crop-thumbnails.md)                                                | Изменяйте и отображайте предварительные версии видео, возвращаемых API Bing для поиска видео.                                                                                      |
| [Получение видео, набирающих популярность](trending-videos.md) | Поиск видео, набирающих популярность, со всего мира.                                                                                                          |
| [Получение полезных сведений о видео](video-insights.md) | Настройте поиск популярных видео со всего мира.                                                                                                          |

## <a name="workflow"></a>Рабочий процесс

API Bing для поиска видео является веб-службой RESTful, которую легко вызвать с любого языка программирования, поддерживающего выполнение HTTP-запросов и анализ JSON. Вы можете использовать службу с помощью [REST API](csharp.md) или [пакета SDK](video-search-sdk-quickstart.md).

1. Создайте [учетную запись API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с доступом к API-интерфейсам поиска Bing. Если у вас нет подписки Azure, создайте бесплатную [учетную запись](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Отправьте запрос к API с допустимым поисковым запросом.
3. Обработайте ответ API путем анализа возвращенного сообщения JSON.


## <a name="next-steps"></a>Дальнейшие действия

В этой интерактивной демоверсии [API Bing для поиска видео](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) показано, как можно быстро настроить поисковый запрос и найти видео в Интернете.

Когда будете готовы вызвать API, создайте [учетную запись API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Если у вас нет подписки Azure, создайте бесплатную [учетную запись](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Используйте [это краткое руководство](csharp.md), чтобы быстро начать работу с первым запросом API.

## <a name="see-also"></a>См. также раздел

* Страница [Video Search API v7 reference](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference) (Справочник по API Bing для поиска видео версии 7) содержит список конечных точек, заголовков и параметров запроса, используемых для запроса результатов поиска.

* В статье [Требования к использованию и отображению API поиска Bing](./useanddisplayrequirements.md) рассматриваются приемлемые варианты использования содержимого и информации, получаемой с помощью API Bing для поиска.

* Сведения о других доступных API "Поиск Bing" см. на [главной странице](../bing-web-search/search-the-web.md).