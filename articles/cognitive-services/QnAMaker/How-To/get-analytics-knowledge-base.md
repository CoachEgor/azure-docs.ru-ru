---
title: Аналитика в базе знаний — QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker сохраняет все журналы чатов и другие данные телеметрии, если во время создания службы QnA Maker вы включили App Insights. Чтобы получить журналы чатов из App Insights, воспользуйтесь приведенными здесь примерами запросов.
services: cognitive-services
author: diberry
manager: nitinme
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: diberry
ms.openlocfilehash: 5c55084a57e46931049841f5011941b2115e9e69
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961536"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Получение аналитических данных о базе знаний

QnA Maker сохраняет все журналы чатов и другие данные телеметрии, если во время [создания службы QnA Maker](./set-up-qnamaker-service-azure.md) вы включили App Insights. Чтобы получить журналы чатов из App Insights, воспользуйтесь приведенными здесь примерами запросов.

1. Перейдите к нужному ресурсу App Insights.

    ![Выбор ресурса Application Insights](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Выберите **Log (Analytics)** . Откроется новое окно, в котором можно запросить данные телеметрии QnA Maker.

3. Вставьте следующий запрос и выполните его.

    ```kusto
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration, performanceBucket
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId
    ) on id
    | extend question = tostring(customDimensions['Question'])
    | extend answer = tostring(customDimensions['Answer'])
    | extend score = tostring(customDimensions['Score'])
    | project timestamp, resultCode, duration, id, question, answer, score, performanceBucket,KbId 
    ```

    Щелкните **Запуск**, чтобы выполнить запрос.

    [![Выполнение запроса для определения вопросов, ответов и оценок пользователей](../media/qnamaker-how-to-analytics-kb/run-query.png)](../media/qnamaker-how-to-analytics-kb/run-query.png#lightbox)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Выполнение запросов для получения других аналитических данных о базе знаний QnA Maker

### <a name="total-90-day-traffic"></a>Общий трафик за 90 дней

```kusto
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Общий трафик вопросов за определенный период времени

```kusto
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Трафик пользователя

```kusto
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Распределение задержки вопросов

```kusto
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Выберите капактий](../tutorials/choosing-capacity-qnamaker-deployment.md)
