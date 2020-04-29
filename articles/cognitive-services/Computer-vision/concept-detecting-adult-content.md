---
title: Взрослый, носящих непристойный характер, жуткие Content-Компьютерное зрение
titleSuffix: Azure Cognitive Services
description: Основные понятия, связанные с обнаружением содержимого для взрослых в изображениях с помощью Компьютерное зрение APi.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ee18916a59bb081d65494f46e7aba7c29c7177cc
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "71718510"
---
# <a name="detect-adult-content"></a>Обнаружение содержимого для взрослых

Компьютерное зрение может обнаруживать материалы для взрослых в изображениях, чтобы разработчики могли ограничить отображение этих изображений в программном обеспечении. Флаги содержимого применяются с рейтингом от единицы до нуля, чтобы разработчики могли интерпретировать результаты в зависимости от их собственных предпочтений.

> [!NOTE]
> Большая часть этой функции предлагается службой [Content Moderator Azure](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview) . Ознакомьтесь также с другими решениями для более тщательной модерации контента, например с модерацией текста или рабочими процессами пользовательской проверки.

## <a name="content-flag-definitions"></a>Определения флага содержимого

В классификации «Взрослый» есть несколько различных категорий:

- Образы для **взрослых** определяются как те, которые явно представляют сексуальное упоминание и часто обрабатывают Нагота и сексуальное взаимодействие.
- Под изображениями **непристойного характера** понимаются изображения, имеющие неприличный характер, но зачастую менее откровенные, чем изображения категории **Для взрослых**.
- Образы **жуткие** определяются так, как они описывают гор.

## <a name="use-the-api"></a>Использование API

Вы можете обнаружить содержимое для взрослых с помощью API [анализа изображений](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) . При добавлении `Adult` значения в параметр запроса **висуалфеатурес** API возвращает три логических свойства&mdash;`isAdultContent` `isRacyContent`, и `isGoryContent` &mdash;в ответе JSON. Метод также возвращает соответствующие свойства&mdash;`adultScore`, `racyScore`и `goreScore` &mdash;, которые представляют показатели достоверности между нулем и единицей для каждой соответствующей категории.

- [Краткое руководство. анализ образа (пакет SDK для .NET)](./quickstarts-sdk/csharp-analyze-sdk.md)
- [Краткое руководство. Анализ изображения (REST API)](./quickstarts/csharp-analyze.md)
