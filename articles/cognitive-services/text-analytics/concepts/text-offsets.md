---
title: Текстовые смещения в API аналитики текста
titleSuffix: Azure Cognitive Services
description: Узнайте о смещениях, вызванных многоязычными и эмодзи кодирования.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 03/09/2020
ms.author: aahi
ms.reviewer: jdesousa
ms.openlocfilehash: 6e404c710a244f06676edf50c3f5c95a7d681e35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219239"
---
# <a name="text-offsets-in-the-text-analytics-api-output"></a>Текстовые смещения в выходе API API текстовой аналитики

Многоязычная поддержка и поддержка смайликов привела к созданию кодов Unicode, которые используют более одной [кодовой точки](https://wikipedia.org/wiki/Code_point) для представления одного отображаемого символа, называемого графемой. Например, смайлики, 👍 такие как 🌷 и могут использовать несколько символов для составления формы с дополнительными символами для визуальных атрибутов, таких как тон кожи. Аналогичным образом, слово `अनुच्छेद` хинди кодируется как пять букв и три комбинированные знаки.

Из-за разной длины возможных многоязычных и эмодзи кодирования, Text Analytics API может вернуть смещения в ответ.

## <a name="offsets-in-the-api-response"></a>Смещения в ответе API. 

Всякий раз, когда смещения возвращаются API ответ, такие как [именованные сущности распознавания](../how-tos/text-analytics-how-to-entity-linking.md) или [анализа настроений,](../how-tos/text-analytics-how-to-sentiment-analysis.md)помните следующее:

* Элементы в ответе могут быть специфичны для самой конкретной точки. 
* Полезной нагрузки HTTP POST/GET закодированы в [UTF-8,](https://www.w3schools.com/charsets/ref_html_utf8.asp)который может или не может быть кодированием символов по умолчанию на компиляторе или операционной системе на стороне клиента.
* Смещения относятся к графеме, основанному на стандарте [Unicode 8.0.0,](https://unicode.org/versions/Unicode8.0.0) а не на количествах символов.

## <a name="extracting-substrings-from-text-with-offsets"></a>Извлечение подстроек из текста с компенсациями

Смещения могут вызвать проблемы при использовании методов подстриты на основе символов, например [метода substring .NET.NET.](https://docs.microsoft.com/dotnet/api/system.string.substring?view=netframework-4.8) Одна из проблем заключается в том, что смещение может привести к тому, что метод подстритов закончится в середине кодирования многосимвольного графема вместо конца.

В .NET рассмотрите возможность использования класса [StringInfo,](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) который позволяет работать со строкой как с рядом текстовых элементов, а не отдельными объектами символов. Вы также можете посмотреть на библиотеки сплиттера графема в предпочтительной среде программного обеспечения. 

API Text Analytics возвращает эти текстовые элементы также для удобства.

## <a name="see-also"></a>См. также

* [Text Analytics overview](../overview.md) (Общие сведения об анализе текста)
* [Анализ тональности](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Распознавание сущности](../how-tos/text-analytics-how-to-entity-linking.md)
* [Определение языка](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Распознавание языка](../how-tos/text-analytics-how-to-language-detection.md)
