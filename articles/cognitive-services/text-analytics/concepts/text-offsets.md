---
title: Смещение текста в API анализа текста
titleSuffix: Azure Cognitive Services
description: Сведения о смещениях, вызванных кодировками многоязычного и эмодзи.
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79219239"
---
# <a name="text-offsets-in-the-text-analytics-api-output"></a>Смещения текста в выходных данных API анализа текста

Поддержка многоязычности и эмодзи привела к кодированиям в Юникоде, которые используют более одной [кодовой точки](https://wikipedia.org/wiki/Code_point) для представления одного отображаемого символа, называемого графеме. Например, эмодзи, такие как 🌷 и 👍, могут использовать несколько символов для создания фигуры с дополнительными символами для визуальных атрибутов, таких как тон кожи. Аналогичным образом, слово хинди `अनुच्छेद` кодируется как пять букв и три Объединенных знака.

Из-за различной длины возможных кодировок многоязычности и эмодзи API анализа текста может возвращать смещение в ответе.

## <a name="offsets-in-the-api-response"></a>Смещение в ответе API. 

Всякий раз, когда смещение возвращает ответ API, например [Распознавание сущностей](../how-tos/text-analytics-how-to-entity-linking.md) или [Анализ тональности](../how-tos/text-analytics-how-to-sentiment-analysis.md), помните следующее:

* Элементы в ответе могут быть специфическими для вызванной конечной точки. 
* Полезные данные HTTP POST/GET кодируются в [кодировке UTF-8](https://www.w3schools.com/charsets/ref_html_utf8.asp), которая может быть или не совпадать с кодировкой символов по умолчанию в клиентском компиляторе или операционной системе.
* Смещения относятся к счетчикам графеме, основанным на стандарте [Unicode 8.0.0](https://unicode.org/versions/Unicode8.0.0) , а не по числу символов.

## <a name="extracting-substrings-from-text-with-offsets"></a>Извлечение подстрок из текста с использованием смещений

Смещение может вызвать проблемы при использовании символьных методов подстроки, например метода .NET [substring ()](https://docs.microsoft.com/dotnet/api/system.string.substring?view=netframework-4.8) . Одна из проблем заключается в том, что смещение может привести к тому, что метод подстроки завершится в середине многосимвольной кодировки графеме, а не в конце.

В .NET рассмотрите возможность использования класса [StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) , который позволяет работать со строкой в виде набора текстовых элементов, а не отдельных символьных объектов. Вы также можете найти библиотеки графеме Splitter в предпочитаемой программной среде. 

API анализа текста также возвращает эти текстовые элементы для удобства.

## <a name="see-also"></a>См. также раздел

* [Text Analytics overview](../overview.md) (Общие сведения об анализе текста)
* [Пример. Как определить тональность с помощью Анализа текста](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Распознавание сущностей](../how-tos/text-analytics-how-to-entity-linking.md)
* [Пример. Как определить язык с помощью Анализа текста](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Распознавание языка](../how-tos/text-analytics-how-to-language-detection.md)
