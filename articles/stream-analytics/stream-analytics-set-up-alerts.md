---
title: Настройка мониторинга и оповещений для заданий Azure Stream Analytics
description: В этой статье объясняется, как с помощью портала Azure настроить мониторинг и оповещения для заданий Azure Stream Analytics.
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 836b7a489e3c73d745b128cbbc0c3566220ac409
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458732"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Настройка оповещений для заданий Azure Stream Analytics

Очень важно отслеживать задание Azure Stream Analytics, чтобы убедиться, что оно выполняется непрерывно и без каких-либо проблем. В этой статье описывается, как настроить оповещения для распространенных сценариев, которые необходимо отслеживать. 

Правила по метрикам можно определить по данным "Операции журналы" через портал, а также [программно.](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a)

## <a name="set-up-alerts-in-the-azure-portal"></a>Настройка оповещений на портале Azure
### <a name="get-alerted-when-a-job-stops-unexpectedly"></a>Получайте оповещение, когда задание неожиданно останавливается

Ниже приведен пример того, как настроить оповещения, когда задание переходит в состояние сбоя. Рекомендуется настроить это оповещение для всех заданий.

1. На портале Azure откройте задание Stream Analytics, для которого требуется создать оповещение.

2. На странице **Задание** перейдите к разделу **Мониторинг**.  

3. Выберите **метрики,** а затем **новое правило оповещения.**

   ![Настройка оповещений Stream Analytics на портале Azure](./media/stream-analytics-set-up-alerts/stream-analytics-set-up-alerts.png)  

4. Имя задания Stream Analytics должно автоматически появиться в разделе **Ресурс**. Щелкните **Добавить условие** и выберите **All Administrative operations** (Все административные операции) в разделе **Настроить логику сигналов**.

   ![Выбор названия сигнала для оповещения Stream Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-condition-signal.png)  

5. В разделе **Настроить логику сигналов** задайте для параметра **Уровень события** значение **Все**, а для параметра **Состояние** — значение **Сбой**. Оставьте **событие, инициированное** пустым и выберите **Done**.

   ![Настройка логики сигналов для оповещения Stream Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-configure-signal-logic.png) 

6. Создайте группу действий или выберите имеющуюся. В этом примере новая группа действий **TIDashboardGroupActions** была создана с действием **Сообщения электронной почты**, которое отправляет сообщение пользователям с ролью **Владелец** Azure Resource Manager.

   ![Настройка оповещений для заданий Azure Streaming Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-add-group-email-action.png)

7. В полях **Ресурс**, **Условие** и **Группы действий** должны быть записи. Обратите внимание, что для работы оповещений должны быть соблюдены определенные условия. Например, вы можете каждые 5 минут измерять среднее значение по метрике за последние 15 минут.

   ![Создание правила генерации оповещений Stream Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule-2.png)

   В разделе **Сведения об оповещении** введите значения в поля **Имя правила генерации оповещений** и **Описание**, укажите свою **группу ресурсов** и щелкните **Создать правило генерации оповещений**, чтобы создать правило для задания Stream Analytics.

   ![Создание правила генерации оповещений Stream Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule.png)
   
## <a name="scenarios-to-monitor"></a>Сценарии для мониторинга

Для мониторинга производительности задания Stream Analytics рекомендуется использовать следующие оповещения. Эти метрики должны оцениваться каждую минуту за последний 5-минутный период.

|Метрика|Условие|Агрегат времени|Порог|Корректирующие действия|
|-|-|-|-|-|
|Использование единиц потоковой передачи (%)|Больше|Максимальная|80|Существуют несколько факторов, повышающих процент использования единиц потоковой передачи. Вы можете масштабировать с применением параллелизации запросов или увеличить число единиц потоковой передачи. Дополнительные сведения см. в статье [Использование параллелизации запросов в Azure Stream Analytics](stream-analytics-parallelization.md).|
|Ошибки среды выполнения|Больше|Итог|0|Изучите журналы действий или диагностики и внесите соответствующие изменения во входные данные, запросы или выходные данные.|
|Предельная задержка|Больше|Максимальная|Если среднее значение метрики за последние 15 минут больше допустимого интервала поступления с задержкой (в секундах). Если вы не изменили допустимый интервал поступления с задержкой, значение по умолчанию устанавливается на 5 секунд.|Попробуйте увеличить число единиц потоковой передачи или выполнить параллелизацию запроса. Дополнительные сведения о единицах потоковой передачи см. в статье [Обзор и настройка единиц потоковой передачи](stream-analytics-streaming-unit-consumption.md#how-many-sus-are-required-for-a-job). Дополнительные сведения см. в статье [Использование параллелизации запросов в Azure Stream Analytics](stream-analytics-parallelization.md).|
|Ошибки десериализации входных данных|Больше|Итог|0|Изучите журналы действий или диагностики и внесите соответствующие изменения во входные данные. Дополнительные сведения о журналах диагностики см. в статье [Устранение неполадок Azure Stream Analytics с помощью журналов диагностики](stream-analytics-job-diagnostic-logs.md).|

## <a name="get-help"></a>Получить справку

Дополнительные сведения о настройке оповещений на портале Azure см. в статье [Получение уведомлений](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

Для получения дополнительной помощи попробуйте наш [форум Azure Stream Analytics.](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Дальнейшие действия
* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Начало использования аналитики потоков Azure](stream-analytics-get-started.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

