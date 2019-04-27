---
title: Общие сведения об Azure Cognitive Services
description: Azure Cognitive Services — это API-интерфейсы, пакеты SDK и службы, которые можно использовать для создания интеллектуальных приложений с помощью Microsoft Azure.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: article
ms.date: 03/19/2019
ms.author: nitinme
ms.openlocfilehash: 2464424b537f6187542fb612626cff115711d6e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60608977"
---
# <a name="what-are-azure-cognitive-services"></a>Общие сведения об Azure Cognitive Services

Azure Cognitive Services — это API-интерфейсы, пакеты SDK и службы, которые помогают создавать интеллектуальные приложения разработчикам без опыта работы со средствами искусственного интеллекта и обработки и анализа данных. Azure Cognitive Services позволяют разработчикам легко добавлять когнитивные функции, например определение эмоций и обнаружение видео; лиц, речи и распознавание концепции; и понимание речи и языка – в свои приложения. Задача Azure Cognitive Services — помочь разработчикам создавать приложения, которые умеют видеть, слышать, разговаривать и даже в некоторой степени размышлять. Службы Azure Cognitive Services можно разделить на пять категорий, которые связаны со зрением, речью, языком, поиском и знанием.

## <a name="vision-apis"></a>API, связанные со зрением

|Имя службы|Описание службы|
|:-----------|:------------------|
|[Компьютерное зрение](https://docs.microsoft.com/azure/cognitive-services/computer-vision/ "Computer Vision")|Служба "Компьютерное зрение" предоставляет доступ к передовым алгоритмам обработки изображений и возврата данных.|
|[Пользовательская служба визуального распознавания](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/home "Custom Vision Service") (предварительная версия)|Пользовательская служба визуального распознавания позволяет создавать пользовательские классификаторы изображений.|
|[Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview "Content Moderator")|Content Moderator позволяет отслеживать потенциально оскорбительное, нежелательное и представляющее риск содержимое.|
|[API распознавания лиц](https://docs.microsoft.com/azure/cognitive-services/face/ "Face API")|API распознавания лиц обеспечивает доступ к расширенным алгоритмам, позволяя определять и распознавать лица на основе атрибутов.|
| [Индексатор видео](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview "Video Indexer")|Индексатор видео позволяет извлекать аналитические сведения из видео.|

## <a name="speech-apis"></a>API, связанные с речью

|Имя службы|Описание службы|
|:-----------|:------------------|
|[Служба "Речь"](https://docs.microsoft.com/azure/cognitive-services/speech-service/ "Speech Service") (предварительная версия)|Служба "Речь" позволяет добавлять в приложения функции с поддержкой речи.|
|[API распознавания речи Bing](https://docs.microsoft.com/azure/cognitive-services/speech/home "Bing Speech API")|API распознавания речи Bing позволяет без усилий добавлять в приложения функции с поддержкой речи.|
|[Перевод речи](https://docs.microsoft.com/azure/cognitive-services/translator-speech/ "Translator Speech")|Перевод речи — это служба машинного перевода.|
|[API распознавания говорящего](https://docs.microsoft.com/azure/cognitive-services/speaker-recognition/home "Speaker Recognition API") (предварительная версия)|API распознавания говорящего обеспечивает доступ к алгоритмам идентификации говорящего.|

## <a name="language-apis"></a>API, связанные с языком

|Имя службы|Описание службы|
|:-----------|:------------------|
|[Проверка орфографии Bing](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/ "Bing Spell Check")|Служба "Проверка орфографии Bing" предназначена для контекстной проверки грамматики и орфографии.|
|[Интеллектуальная служба распознавания речи (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/ "Language Understanding")|Интеллектуальная служба распознавания речи (LUIS) позволяет приложению понимать желания пользователей, выраженные словами.|
|[Анализ текста](https://docs.microsoft.com/azure/cognitive-services/text-analytics/ "Text Analytics")|Служба "Анализ текста" — это служба обработки естественного языка, выполняющая анализ тональности необработанного текста, извлечение ключевых фраз и определение языка.|
|[Перевод текстов](https://docs.microsoft.com/azure/cognitive-services/translator/ "Translator Text")|Служба "Перевод текстов" предназначена для машинного перевода текстов практически в реальном времени.|

## <a name="search-apis"></a>Интерфейсы API для поиска

|Имя службы|Описание службы|
|:-----------|:------------------|
|[Поиск новостей Bing](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/ "Bing News Search")|Служба "Поиск новостей Bing" выполняет поиск новостных статей релевантных запросу пользователя.|
|[Поиск видео Bing](https://docs.microsoft.com/azure/cognitive-services/Bing-Video-Search/ "Bing Video Search")|Служба "Поиск видео Bing" выполняет поиск видео, релевантных запросу пользователя.|
|[Поиск в Интернете Bing](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/ "Bing Web Search")|Служба "Поиск в Интернете Bing" выполняет поиск материалов в Интернете, релевантных запросу пользователя.|
|[Автозаполнение Bing](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest "Bing Autosuggest")|Служба "Автозаполнение Bing" предлагает отправлять в Bing часть слова поискового запроса и получать список предлагаемых запросов.|
|[Пользовательский поиск Bing](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search "Bing Custom Search")|Служба "Пользовательский поиск Bing" позволяет выполнять пользовательскую настройку функции поиска требуемых материалов.|
|[Поиск сущностей Bing](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/ "Bing Entity Search")|Служба "Поиск сущностей Bing" возвращает сведения о сущностях, которые Bing определяет как релевантные запросу пользователя.|
|[Поиск изображений Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search "Bing Image Search")|Служба "Поиск изображений Bing" отображает изображения, релевантные запросу пользователя.|
|[Визуальный поиск Bing](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search "Bing Visual Search")|Служба "Визуальный поиск Bing" возвращает аналитические сведения об изображении, например визуально схожие изображения, ресурсы, на которых можно купить продукты, найденные на изображении, а также связанные результаты поиска.|
|[API Bing для поиска местных организациях](https://docs.microsoft.com/azure/cognitive-services/bing-local-business-search/ "API Bing для поиска местных организациях")| API поиска Bing локального бизнеса позволяет приложениям найти контактные данные и расположение сведения о местных учреждениях, исходя из запросов поиска.|

## <a name="knowledge-apis"></a>API, связанные со знанием

|Имя службы|Описание службы|
|:-----------|:------------------|
|[QnA Maker](https://docs.microsoft.com/azure/cognitive-services/qnamaker/index "QnA Maker")|Служба QnA Maker позволяет создавать службу для работы с вопросами и ответами на основе частично структурированного содержимого.|

## <a name="use-free-trials"></a>Воспользуйтесь бесплатными пробными версиями

[При регистрации для получения бесплатных пробных версий](https://azure.microsoft.com/try/cognitive-services/ "Справка по регистрации") требуется указать адрес электронной почты и выполнить несколько простых действий. У вас должна быть учетная запись Майкрософт. Вы получите уникальную пару ключей для каждого запрошенного API. Второй ключ — просто запасной. Эти ключи необходимо хранить в секрете. Бесплатные пробные версии имеют ограничение по частоте проведения транзакций в секунду или минуту, а также по объему использования в месяц. Транзакция — это просто вызов API. Вы можете обновиться до платных уровней, чтобы снять ограничения.

## <a name="subscription-management"></a>Управление подпиской

После входа в учетную запись Майкрософт в разделе [Мои подписки](https://www.microsoft.com/cognitive-services/en-us/subscriptions "Мои подписки") можно просмотреть используемые продукты, оставшуюся квоту и возможность добавления других продуктов в подписку.

## <a name="upgrade-to-unlock-limits"></a>Обновление для снятия ограничений

Все API предлагаются в бесплатной пробной версии.  Когда для каждого API появятся платные предложения, вам будет предложено приобрести их на портале Azure.  Ссылки для *покупки* доступны на странице подписок, если вы уже используете эти API. Вы также можете отказаться от бесплатных пробных версий и приобрести платные, перейдя по соответствующим ссылкам на странице [цен](https://www.microsoft.com/cognitive-services/en-us/pricing "pricing").  Для этого потребуется настроить учетную запись подписчика Azure, указав данные кредитной карты и номер телефона. Если у вас есть особые пожелания или вы хотите пообщаться с торговым представителем, щелкните "Свяжитесь с отделом продаж" в верхней части страницы "Цены Azure".

## <a name="regional-availability"></a>Доступ по регионам

API-интерфейсы в Cognitive Services размещаются в растущей сети центров обработки данных корпорации Майкрософт. Вы можете найти локализацию для каждого API в [списке регионов Azure](https://azure.microsoft.com/regions).

Ищете регион, который пока не поддерживается? Сообщите нам, отправив запрос функции на наш [форум UserVoice](https://cognitive.uservoice.com/).

## <a name="supported-cultural-languages"></a>Поддерживаемые региональные языки

 Cognitive Services поддерживает широкий спектр региональных языков на уровне службы. Дополнительные сведения о доступности языков для каждого API можно найти в [списке поддерживаемых языков](language-support.md).

## <a name="container-support"></a>Поддержка контейнеров

 Cognitive Services предоставляет контейнеры для развертывания в облаке Azure или локально. Дополнительные сведения о [контейнерах Cognitive Services](cognitive-services-container-support.md).

## <a name="certifications-and-compliance"></a>Сертификация и соответствие

Cognitive Services получила сертификаты, например сертификацию CSA STAR, FedRAMP — средний уровень и HIPAA BAA. 

Вы можете [загрузить](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) сертификаты для собственных аудиты и проверки системы безопасности. 

Чтобы понять, конфиденциальности и управления данными, перейдите в раздел [Центр управления безопасностью](https://servicetrust.microsoft.com/). 

## <a name="support"></a>Поддержка

* Для поддержки и технических вопросов разместите пост на [Stack Overflow](https://stackoverflow.com/questions/tagged/microsoft-cognitive)
* Для отзывов и запросов функций перейдите на форум [UserVoice](https://cognitive.uservoice.com/)

## <a name="next-steps"></a>Дальнейшие действия

* [Создание учетной записи Cognitive Services](cognitive-services-apis-create-account.md)
