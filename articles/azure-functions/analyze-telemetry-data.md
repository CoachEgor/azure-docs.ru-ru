---
title: Анализ телеметрии функций Azure в Application Insights
description: Узнайте, как просматривать и запрашивать данные телеметрии функций Azure, собираемых и хранимых в Application Insights Azure.
ms.topic: how-to
ms.date: 10/14/2020
ms.custom: contperf-fy21q2
ms.openlocfilehash: 1d8d9cc9a7a4111e98b1d9141957769d6f157d45
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97027736"
---
# <a name="analyze-azure-functions-telemetry-in-application-insights"></a>Анализ телеметрии функций Azure в Application Insights 

Функции Azure интегрируются с Application Insights, что позволяет лучше отслеживать приложения функций. Application Insights собирает данные телеметрии, созданные приложением функции, включая сведения, записываемые приложением в журналы. Application Insights интеграция обычно включается при создании приложения-функции. Если в приложении функции не задан ключ инструментирования, необходимо сначала [включить интеграцию с Application Insights](configure-monitoring.md#enable-application-insights-integration). 

По умолчанию данные, собранные из приложения-функции, хранятся в Application Insights. В [портал Azure](https://portal.azure.com)Application Insights предоставляет широкий набор визуализаций данных телеметрии. Вы можете детализировать журналы ошибок и события запросов и метрики. В этой статье приведены основные примеры просмотра собранных данных и выполнения запросов к ним. Дополнительные сведения о просмотре данных приложения-функции в Application Insights см. в разделе [что такое Application Insights?](../azure-monitor/app/app-insights-overview.md). 

Дополнительные сведения о хранении данных и потенциальных затратах на хранение см. [в разделе Сбор данных, хранение и хранение в Application Insights](../azure-monitor/app/data-retention-privacy.md).   

## <a name="viewing-telemetry-in-monitor-tab"></a>Просмотр телеметрии на вкладке "монитор"

При [включенной интеграции с Application Insights](configure-monitoring.md#enable-application-insights-integration) данные телеметрии можно просматривать на вкладке **Монитор**.

1. На странице приложения-функции выберите функцию, которая выполнялась по крайней мере один раз после настройки Application Insights. Затем в области слева выберите **Монитор**. Периодически выбирайте **Обновить**, пока не появится список вызовов функций.

   ![Список вызовов](media/functions-monitoring/monitor-tab-ai-invocations.png)

    > [!NOTE]
    > Для отображения списка может потребоваться до пяти минут. Это время необходимо клиенту, чтобы подготовить пакеты данных телеметрии для передачи на сервер. Такая задержка отсутствует в случае [Live Metrics Stream](../azure-monitor/app/live-stream.md). Эта служба подключается к узлу решения "Функции" при загрузке страницы, поэтому журналы передаются на нее напрямую.

1. Чтобы просмотреть журналы для вызова определенной функции, выберите ссылку на столбец **Дата (UTC)** для этого вызова. Выходные данные ведения журнала для этого вызова отображаются на новой странице.

   ![Сведения о вызове](media/functions-monitoring/invocation-details-ai.png)

1. Выберите **Запустить в Application Insights**, чтобы просмотреть источник запроса, который извлекает данные журнала Azure Monitor в журнале Azure. Если вы впервые используете функцию Azure Log Analytics в подписке, вам будет предложено ее включить.

1. После включения Log Analytics отображается следующий запрос. Можно увидеть, что результаты запроса ограничены последними 30 днями ( `where timestamp > ago(30d)` ), а результаты содержат не более 20 строк ( `take 20` ). В отличие от них, список сведений о вызовах для функции содержит информацию за последние 30 дней без ограничений.

   ![Список вызовов в аналитике Application Insights](media/functions-monitoring/ai-analytics-invocation-list.png)

Дополнительные сведения см. в разделе [Запросы к данным телеметрии](#query-telemetry-data) далее в этой статье.

## <a name="view-telemetry-in-application-insights"></a>Просмотр данных телеметрии в Application Insights

Чтобы открыть Application Insights из приложения-функции в [портал Azure](https://portal.azure.com):

1. Перейдите к приложению функции на портале.

1. Выберите **Application Insights** в разделе **Параметры** на левой странице. 

1. Если вы впервые используете Application Insights с подпиской, вам будет предложено включить ее. Для этого выберите **включить Application Insights** и нажмите кнопку **Применить** на следующей странице.

![Открытие Application Insights на странице обзора приложения-функции](media/functions-monitoring/ai-link.png)

Дополнительные сведения об использовании Application Insights см. в [документации по Application Insights](/azure/application-insights/). В этом разделе представлено несколько примеров просмотра данных в Application Insights. Если вы уже знакомы с Application Insights, можете сразу переходить к [разделам, посвященным конфигурации и настройке данных телеметрии](configure-monitoring.md#configure-log-levels).

![Вкладка обзора Application Insights](media/functions-monitoring/metrics-explorer.png)

При оценке поведения, производительности и ошибок в функциях могут быть полезны следующие области Application Insights.

| Изучение | Описание |
| ---- | ----------- |
| **[Сбои](../azure-monitor/app/asp-net-exceptions.md)** |  Создание диаграмм и оповещений на основе сбоев функции и исключений сервера. **Имя операции** обозначает имя функции. Сбои в зависимостях здесь не отображаются, если вы не настроили для них пользовательскую телеметрию. |
| **[Производительность](../azure-monitor/app/performance-counters.md)** | Анализ проблем с производительностью путем просмотра использования ресурсов и пропускной способности для **экземпляров облачных ролей**. Эти данные производительности могут быть полезны при отладке сценариев, в которых функции тормозя базовыми ресурсами. |
| **[Метрики](../azure-monitor/platform/metrics-charts.md)** | Создание диаграмм и оповещений на основе метрик. Используются такие метрики: число вызовов функций, время выполнения и частота успешных попыток. |
| **[Интерактивные метрики](../azure-monitor/app/live-stream.md)** | Просмотр данных метрик по мере их создания практически в реальном времени. |

## <a name="query-telemetry-data"></a>Запросы к данным телеметрии

[Аналитика в Application Insights](../azure-monitor/log-query/log-query-overview.md) предоставляет доступ ко всем данным телеметрии в формате таблиц в базе данных. Функция аналитики поддерживает язык запросов для извлечения, обработки и визуализации данных. 

Выберите **Журналы** для просмотра или запроса событий в журналах.

![Пример данных аналитики](media/functions-monitoring/analytics-traces.png)

В этом примере показано распределение запросов по рабочим ролям за последние 30 минут.

<pre>
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
</pre>

Доступные таблицы представлены на вкладке **Схема** в области слева. Данные, создаваемые при вызовах функций, вы найдете в следующих таблицах:

| Таблица | Описание |
| ----- | ----------- |
| **traces** | Журналы, созданные средой выполнения и трассировки из кода функции. |
| **requests** | По одному запросу для каждого вызова функции. |
| **exceptions** | Любые исключения в среде выполнения. |
| **customMetrics** | Число успешных и неудачных вызовов, доля успешных попыток, продолжительность. |
| **customEvents** | События, отслеживаемые средой выполнения, например HTTP-запросы, которые активируют функции; |
| **performanceCounters** | Сведения о производительности серверов, на которых выполняются функции. |

Остальные таблицы предназначены для проверок доступности, а также телеметрии клиента и браузера. Вы можете реализовать пользовательскую телеметрию, чтобы добавлять в них данные.

В каждой таблице есть поле `customDimensions`, где хранится часть данных о конкретной функции.  Например, следующий запрос получает все трассировки с уровнем журнала `Error`.

<pre>
traces 
| where customDimensions.LogLevel == "Error"
</pre>

Среда выполнения предоставляет поля `customDimensions.LogLevel` и `customDimensions.Category`. Вы можете указать дополнительные поля в журналах, которые указываются в коде функции. Пример в C# см. в статье [структурированное ведение журнала](functions-dotnet-class-library.md#structured-logging) в разделе Руководство разработчика библиотеки классов .NET.

## <a name="consumption-plan-specific-metrics"></a>Метрики, зависящие от плана потребления

При выполнении в [плане потребления](functions-scale.md#consumption-plan) *стоимость* выполнения одной функции измеряется в *ГБ/с*. Затраты на выполнение рассчитываются путем объединения использования памяти с временем выполнения. Дополнительные сведения см. в разделе [Оценка затрат на план потребления](functions-consumption-costs.md).

Следующие запросы телеметрии относятся к метрикам, влияющим на затраты на выполнение функций в плане потребления.

[!INCLUDE [functions-consumption-metrics-queries](../../includes/functions-consumption-metrics-queries.md)]

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о мониторинге функций Azure:

+ [Мониторинг Функций Azure](functions-monitoring.md)
+ [Настройка мониторинга для функций Azure](configure-monitoring.md)

