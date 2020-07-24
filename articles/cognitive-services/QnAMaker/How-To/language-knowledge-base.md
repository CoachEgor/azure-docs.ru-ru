---
title: Основные понятия языка — QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker поддерживает содержимое базы знаний на различных языках. При этом каждая служба QnA Maker должна быть зарезервирована для одного языка. Первая база знаний, созданная для определенной службы QnA Maker, задает язык этой службы.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: b0d4250a6659996187923905955a9825a44cea42
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2020
ms.locfileid: "87132625"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Языковая поддержка содержимого базы знаний для QnA Maker

Язык для службы выбирается при создании первой базы знаний в ресурсе. Все дополнительные базы знаний в ресурсе должны быть на одном языке.

Язык определяет релевантность результатов QnA Maker предоставляемых в ответ на запросы пользователя.

## <a name="one-language-for-all-knowledge-bases-in-resource"></a>Один язык для всех баз знаний в ресурсе

QnA Maker позволяет выбрать язык для службы QnA при создании первой базы знаний. Все базы знаний в ресурсе QnA Maker должны быть на одном языке. Этот язык нельзя изменить.

Создание баз знаний на разных языках в одном ресурсе отрицательно влияет на релевантность результатов QnA Maker предоставляемых в ответ на запросы пользователя.

Ознакомьтесь со списком [поддерживаемых языков](../overview/language-support.md#languages-supported) и Узнайте, как языки влияют на [сопоставление и релевантность](#query-matching-and-relevance).

## <a name="select-language-when-creating-first-knowledge-base"></a>Выбрать язык при создании первой базы знаний

Выбор языка является частью этапов создания первой базы знаний в ресурсе.

![Снимок экрана QnA Maker портала выбора языка для первой базы знаний](../media/language-support/select-language-when-creating-knowledge-base.png)

## <a name="query-matching-and-relevance"></a>Сопоставление запросов и релевантность
QnA Maker зависит от [анализаторов языка Azure когнитивный Поиск](https://docs.microsoft.com/rest/api/searchservice/language-support) для предоставления результатов.

Хотя возможности Когнитивный поиск Azure относятся к поддерживаемым языкам, QnA Maker имеет дополнительного ранжирования, который располагается над результатами поиска Azure. В этой модели ранжирования мы используем некоторые специальные функции на основе семантики и слов на следующих языках.

|Языки с дополнительным званием|
|--|
|Китайский|
|Чешский|
|Нидерландский|
|Английский|
|Французский|
|Немецкий|
|Венгерский|
|Итальянский|
|Японский|
|Корейский|
|Польский|
|Португальский|
|Испанский|
|Шведский|

Это дополнительное ранжирование является внутренней работой ранжирования QnA Maker.

## <a name="verify-language"></a>Проверить язык

Язык ресурса QnA Maker можно проверить на странице "Параметры службы" в QnA Maker.

![Снимок экрана QnA Maker портала на странице "Параметры службы"](../media/language-support/language-knowledge-base.png)


## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Перенос базы знаний](../Tutorials/migrate-knowledge-base.md)
