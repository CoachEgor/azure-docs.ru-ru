---
title: Новые возможности API анализа текста
titleSuffix: Text Analytics - Azure Cognitive Services
description: В этой статье содержатся сведения о новых выпусках и функциях Анализ текста Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: aahi
ms.openlocfilehash: 6fa7d6a93a56cc531df238a8580207ef7a89d5d0
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75732626"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Новые возможности в API "Анализ текста"

API анализа текста обновляется на постоянной основе. В этой статье содержатся сведения о новых выпусках и функциях, чтобы оставаться в курсе последних нововведений.

## <a name="named-entity-recognition-v3-public-preview---october-2019"></a>Распознавание именованных сущностей v3 — общедоступная Предварительная версия — Октябрь 2019

Следующая версия средства распознавания имен сущностей (NER) теперь доступна для общедоступной предварительной версии и обеспечивает Расширенное обнаружение и классификацию сущностей, найденных в тексте. Служба предоставляет:

* Распознавание следующих новых типов сущностей:
    * Номер телефона
    * IP-адрес

* [Новая конечная точка](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionPii) для распознавания типов сущностей личных сведений (только на английском языке)
* Разделяйте конечные точки для [распознавания сущностей](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral) и [связывания сущностей](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesLinking).

Связывание сущностей поддерживает английский и испанский языки. Языковая поддержка NER зависит от типа сущности. 

> [!div class="nextstepaction"]
> [Дополнительные сведения о распознавании имен сущностей v3](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)

## <a name="sentiment-analysis-v3-public-preview---october-2019"></a>Анализ тональности v3 (общедоступная Предварительная версия) — Октябрь 2019

[Следующая версия анализ тональности](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment) теперь доступна для общедоступной предварительной версии и обеспечивает значительные улучшения точности и детализации классификации и оценки текста API. Кроме того, он предоставляет:

* Автоматическая маркировка для различных тональности в тексте.
* Тональности анализ и вывод на уровне документа и предложения. 

Он поддерживает английский (`en`), японский (`ja`), Китайская упрощенная (`zh-Hans`), Китайская традиционная (`zh-Hant`), французский (`fr`), итальянский (`it`), Испанский (`es`), Нидерландский (`nl`), португальский (`pt`) и немецкий (`de`), доступны в следующих регионах: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`и `Southeast Asia`. 

> [!div class="nextstepaction"]
> [Дополнительные сведения о анализ тональности v3](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features)

## <a name="next-steps"></a>Дальнейшие действия

* [Что такое API "Анализ текста"?](overview.md)  
* [Примеры пользовательских сценариев](text-analytics-user-scenarios.md)
* [Пример. Как определить тональность с помощью Анализа текста](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Пример. Как определить язык с помощью Анализа текста](how-tos/text-analytics-how-to-language-detection.md)
* [Распознавание сущностей](how-tos/text-analytics-how-to-entity-linking.md)
* [Пример. Как извлечь ключевые фразы с помощью Анализа текста](how-tos/text-analytics-how-to-keyword-extraction.md)
