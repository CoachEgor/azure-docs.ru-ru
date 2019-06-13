---
title: Что такое API анализа текста? - Возможности -
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать API Анализа текста в Azure Cognitive Services для анализа тональности, извлечения ключевых фраз, а также определения языка и распознаванием сущностей.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 04/03/2019
ms.author: aahi
ms.openlocfilehash: 7d52585b51af09c430130141c3680b5630f7b95e
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417287"
---
# <a name="what-is-text-analytics-api"></a>Что такое API анализа текста?

API Анализа текста — это облачная служба, которая предоставляет расширенную обработку необработанного текста на естественных языках и включает в себя четыре основные функции: анализ тональности, извлечение ключевых фраз, определение языка и распознавание сущностей.

API — это компонент [Microsoft Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/), коллекции алгоритмов машинного обучения и ИИ в облаке для использования в проектах по разработке.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Understanding-Text-using-Cognitive-Services/player]

Анализ текста может иметь различные значения, но Cognitive Services API анализа текста предоставляет четыре типа анализа, описанные ниже.

## <a name="sentiment-analysis"></a>Анализ тональности
Используйте [анализ тональности высказываний](how-tos/text-analytics-how-to-sentiment-analysis.md), чтобы узнать, что клиенты думают о вашем бренде или торговой марке, анализируя необработанный текст для получения сведений о тональности выражений (положительная или отрицательная). Этот API возвращает оценку тональности (0 или 1) для каждого документа, где 1 означает положительную тональность.<br /> Модели анализа предварительно обучены с использованием обширного набора текстов и технологий естественного языка корпорации Майкрософт. Для [выбранных языков](text-analytics-supported-languages.md) API может анализировать и оценивать любой необработанный текст, напрямую возвращая результаты вызывающему приложению. Можно использовать [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c9) API или [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) SDK.

## <a name="key-phrase-extraction"></a>Извлечение ключевых фраз
Автоматическое [извлечение ключевых фраз](how-tos/text-analytics-how-to-keyword-extraction.md) в тексте поможет быстро определить основные мысли. Например, для входного текста "Еда была вкусной и персонал был замечательным" API вернет основные тезисы в записи: "еда" и "замечательный персонал". Можно использовать [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c6) API или [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) SDK.

## <a name="language-detection"></a>Распознавание языка
Вы можете [определить, на каком языке написан входной текст](how-tos/text-analytics-how-to-language-detection.md), и сообщить единый код языка для каждого документа, представленного по запросу, на разных языках, диалектах и некоторых местных наречиях. Код языка сопряжен с показателем, указывающим степень оценки. Можно использовать [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c7) API или [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) SDK.

## <a name="named-entity-recognition"></a>Распознавание именованных сущностей
[Определение сущностей и их распределение по категориям](how-tos/text-analytics-how-to-entity-linking.md), таким как текст, люди, места, организации, дата и время, количество, проценты, валюта и многое другое. Также можно определить известные сущности и связать их с дополнительной информацией в Интернете. Можно использовать [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/5ac4251d5b4ccd1554da7634) API.

## <a name="use-containers"></a>Использование контейнеров

[Контейнеры API анализа текста](how-tos/text-analytics-how-to-install-containers.md) можно использовать для извлечения ключевых фраз, распознавания языка и анализа тональности на локальном компьютере, установив стандартные контейнеры Docker в непосредственной близости к своим данным.

## <a name="typical-workflow"></a>Стандартный рабочий процесс

Рабочий процесс прост: данные отправляются для анализа, а выходные данные обрабатываются в коде. Анализаторы используются как есть без дополнительной конфигурации или настройки.

1. [Зарегистрируйтесь](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) для получения [ключа доступа](how-tos/text-analytics-how-to-access-key.md). Ключ должен передаваться по каждому запросу.

2. [Сформулируйте запрос](how-tos/text-analytics-how-to-call-api.md#json-schema), содержащий ваши данные как необработанный неструктурированный текст в формате JSON.

3. Отправьте запрос конечной точке, установленной во время регистрации, добавив требуемый ресурс: анализ тональности, извлечение ключевых фраз, определение языка или идентификация сущности.

4. Выполните потоковую передачу ответа или сохраните его локально. В зависимости от запроса результаты — это либо оценка тональности, либо коллекция извлеченных ключевых фраз, либо код языка.

Выходные данные возвращаются как единый документ JSON с результатами для каждого опубликованного текстового документа на основе идентификатора. Впоследствии можно проанализировать, визуализировать или разделить результаты и превратить их в практические аналитические сведения.

Данные не хранятся в учетной записи. В операциях, выполняемых API анализа текста, не отслеживается состояние. Это означает, что предоставляемый текст обрабатывается и результаты возвращаются сразу.

## <a name="text-analytics-for-multiple-programming-experience-levels"></a>Анализ текста для разных уровней программирования

Вы можете начать использовать API Анализа текста в своих процессах, даже если у вас мало опыта в программировании. В этих руководствах описано, как можно разными способами использовать API для анализа текста с учетом ваших навыков. 

* Минимальный опыт программирования:
    * [Использование API Анализа текста и MS Flow для определения тональности комментариев в группе Yammer](https://docs.microsoft.com/Yammer/integrate-yammer-with-other-apps/sentiment-analysis-flow-azure?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Интеграция Power BI с API Анализа текста для анализа отзывов пользователей](tutorials/tutorial-power-bi-key-phrases.md)
* Рекомендуемый опыт программирования:
    * [Руководство по оценке тональности сообщений при потоковой передаче данных с использованием Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Создание приложения Flask для перевода текста, анализа тональности и синтеза речи](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)


<a name="supported-languages"></a>

## <a name="supported-languages"></a>Поддерживаемые языки

Этот раздел перемещен в отдельную статью, чтобы оптимизировать поиск информации. Соответствующее содержимое см. в статье [Supported languages in the Text Analytics API](text-analytics-supported-languages.md) (Поддерживаемые языки в API анализа текста).

<a name="data-limits"></a>

## <a name="data-limits"></a>Ограничения данных

Все конечные точки API анализа текста принимают необработанные текстовые данные. Текущий предел составляет 5120 символов для одного документа. Если необходимо проанализировать документ большего размера, его можно разбить на части. Если вам требуется более высокий лимит, [свяжитесь с нами](https://azure.microsoft.com/overview/sales-number/), чтобы мы могли обсудить ваши требования.

| Ограничение | Значение |
|------------------------|---------------|
| Максимальный размер одного документа | 5120 символов, измеренных методом [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements). |
| Максимальный размер всего запроса | 1 МБ |
| Предельное число документов в запросе | 1000 документов |

Ограничение частоты составляет 100 запросов в секунду и 1000 запросов в минуту. Вы можете отправлять большое количество документов в ходе одного вызова (до 1000 документов).

## <a name="unicode-encoding"></a>Кодировка Юникод

API анализа текста использует кодировку Юникод для текстового представления и подсчета количества символов. Запросы могут быть представлены как в UTF-8, так и в UTF-16 без каких-либо измеримых различий в количестве символов. Кодовые точки Юникод используются в качестве эвристики для определения длины символов и считаются эквивалентными для ограничения данных анализа текста. Если вы используете [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements), чтобы получить число символов, вы используете тот же метод, который используется для измерения объема данных.

## <a name="next-steps"></a>Дополнительная информация

+ [Зарегистрируйтесь](how-tos/text-analytics-how-to-signup.md) для получения ключа доступа и просмотрите шаги для [вызова API](how-tos/text-analytics-how-to-call-api.md).

+ Просмотрите [краткое руководство](quickstarts/csharp.md). Это пошаговое руководство по вызовам REST API на C#. Узнайте, как отправить текст, выбрать анализ и просмотреть результаты с минимальным объемом кода. При желании вы можете начать с [краткого руководства по Python](quickstarts/python.md).

+ Углубите свои знания с помощью [руководства по анализу тональности](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services), используя Azure Databricks.

+ Ознакомьтесь со списком публикаций в блоге и другими видеороликами о том, как использовать API анализа текста с другими инструментами и технологиями на нашей [странице внешнего контента и контента сообщества](text-analytics-resource-external-community.md).
