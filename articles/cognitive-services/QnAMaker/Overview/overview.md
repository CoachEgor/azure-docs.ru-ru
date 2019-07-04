---
title: Общие сведения о QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker — это облачная служба API, которая применяет пользовательскую аналитику машинного обучения к вопросам пользователя на естественном языке, чтобы предоставить наилучший ответ.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: overview
ms.date: 04/05/2019
ms.author: diberry
ms.openlocfilehash: bfb6c5b7cc5a4bd1717fdd96f6d232cc269e702d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439592"
---
# <a name="what-is-qna-maker"></a>Общие сведения о QnA Maker

QnA Maker — это облачная API-служба, которая позволяет работать с данными в форме вопросов и ответов. 

С помощью QnA Maker из частично структурированного содержимого можно создать базу знаний. Примерами такого содержимого могут быть URL-адреса страниц с часто задаваемыми вопросами, руководства по продуктам, справочные документы и настраиваемые вопросы и ответы. Служба QnA Maker отвечает на сформулированные на естественном языке вопросы, сопоставляя их с наиболее точным ответом в базе знаний вопросов и ответов.

Простой в использовании [веб-портал](https://qnamaker.ai) позволяет создавать, обучать и публиковать службу и управлять ею, не выполняя действий по разработке. После публикации службы в конечной точке клиентское приложение, например чат-бот, может управлять диалогом с пользователем, чтобы получить вопросы и ответить на них. 

![Обзор](../media/qnamaker-overview-learnabout/overview.png)

## <a name="key-qna-maker-processes"></a>Основные процессы QnA Maker

В QnA Maker предусмотрено два основных способа обработки данных:

* **Извлечение**. Структурированные данные вопросов и ответов извлекаются из структурированных и частично структурированных [источников данных](../Concepts/data-sources-supported.md), например из документов с часто задаваемыми вопросами и руководств по продуктам. Извлечение файлов может выполняться как этап процесса [создания](https://aka.ms/qnamaker-docs-createkb) базы знаний или позже как часть процесса редактирования.

* **Сопоставление**. [Обученную и протестированную](https://aka.ms/qnamaker-docs-trainkb) базу знаний нужно [опубликовать](https://aka.ms/qnamaker-docs-publishkb). Таким образом формируется конечная точка доступа к базе знаний QnA Maker, которую затем можно использовать в боте или клиентском приложении. Эта конечная точка принимает вопрос пользователя и дает ответ на основе лучшего совпадения в базе знаний, указывая показатель достоверности совпадения.

```JSON
{
    "answers": [
        {
            "questions": [
                "How do I share a knowledge base with other?"
            ],
            "answer": "Sharing works at the level of a QnA Maker service, i.e. all knowledge bases in the services will be shared. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/how-to/collaborate-knowledge-base)how to collaborate on a knowledge base.",
            "score": 70.95,
            "id": 4,
            "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
            "metadata": []
        }
    ]
}

```

## <a name="qna-maker-architecture"></a>Архитектура QnA Maker

Архитектура QnA Maker состоит из двух компонентов:

1. **Службы управления QnA Maker**. Возможности управления для базы знаний QnA Maker, включая первоначальное создание, обновление, обучение и публикацию. Эти действия можно выполнить с помощью [портала](https://qnamaker.ai) или [API управления](https://go.microsoft.com/fwlink/?linkid=2092179). 

2. **Данные и среда выполнения QnA Maker**. Этот компонент развертывается в подписке Azure в указанном регионе. Содержимое вашей базы знаний хранится в службе [Поиск Azure](https://azure.microsoft.com/services/search/), а конечная точка развертывается как [служба приложений](https://azure.microsoft.com/services/app-service/). Вы можете также развернуть ресурс [Application Insights](https://azure.microsoft.com/services/application-insights/) для получения аналитических сведений.

![Архитектура](../media/qnamaker-overview-learnabout/architecture.png)


## <a name="service-highlights"></a>Краткие сведения о службе

- [Создание бота](../Quickstarts/create-publish-knowledge-base.md#create-a-bot) из базы знаний **без необходимости написания кода**.
- **Отсутствует регулирование сети для прогнозов**. Платите за размещение службы, а не за количество транзакций. Дополнительные сведения см. на [странице с ценами](https://aka.ms/qnamaker-docs-pricing).
- **Масштабирование по мере необходимости**. Выберите соответствующие номера SKU отдельных компонентов в соответствии с вашим сценарием. Узнайте, как [выбрать ресурсы](https://aka.ms/qnamaker-docs-capacity) для службы QnA Maker.


## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Создание службы QnA Maker](../how-to/set-up-qnamaker-service-azure.md)
