---
title: Аналитика в базе знаний
titleSuffix: Azure Cognitive Services
description: QnA Maker сохраняет все журналы чатов и другие данные телеметрии, если во время создания службы QnA Maker вы включили App Insights. Чтобы получить журналы чатов из App Insights, воспользуйтесь приведенными здесь примерами запросов.
services: cognitive-services
author: tulasim88
manager: nitinme
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/04/2019
ms.author: tulasim
ms.openlocfilehash: 09369e760c654e7e56067e6da02bb772bffa7400
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61372623"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Получение аналитических данных о базе знаний

QnA Maker сохраняет все журналы чатов и другие данные телеметрии, если во время [создания службы QnA Maker](./set-up-qnamaker-service-azure.md) вы включили App Insights. Чтобы получить журналы чатов из App Insights, воспользуйтесь приведенными здесь примерами запросов.

1. Перейдите к нужному ресурсу App Insights.

    ![Выбор ресурса Application Insights](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Нажмите кнопку **Аналитика**. Откроется новое окно, в котором можно запросить данные телеметрии QnA Maker.

    ![Выбор функции аналитики](../media/qnamaker-how-to-analytics-kb/analytics.png)

3. Вставьте следующий запрос и выполните его.

    ```query
        requests
        | where url endswith "generateAnswer"
        | project timestamp, id, name, resultCode, duration
        | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
        | join kind= inner (
        traces | extend id = operation_ParentId
        ) on id
        | extend question = tostring(customDimensions['Question'])
        | extend answer = tostring(customDimensions['Answer'])
        | project KbId, timestamp, resultCode, duration, question, answer
    ```

    Щелкните **Запуск**, чтобы выполнить запрос.

    ![Выполнение запроса](../media/qnamaker-how-to-analytics-kb/run-query.png)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Выполнение запросов для получения других аналитических данных о базе знаний QnA Maker

### <a name="total-90-day-traffic"></a>Общий трафик за 90 дней

```query
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Общий трафик вопросов за определенный период времени

```query
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

```query
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

```query
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Управление ключами](./key-management.md)
