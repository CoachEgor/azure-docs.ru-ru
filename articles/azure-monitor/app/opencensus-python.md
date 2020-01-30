---
title: Мониторинг приложений Python с помощью Azure Monitor (Предварительная версия) | Документация Майкрософт
description: Содержит инструкции по установлению связи Опенценсус Python с Azure Monitor
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 87c0b62cec0b61bfc52ec31233ca7c1f947fdd98
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846132"
---
# <a name="set-up-azure-monitor-for-your-python-application-preview"></a>Настройка Azure Monitor для приложения Python (Предварительная версия)

Azure Monitor поддерживает распределенную трассировку, сбор метрик и ведение журнала приложений Python с помощью интеграции с [опенценсус](https://opencensus.io). В этой статье описывается процесс настройки Опенценсус для Python и отправки данных мониторинга в Azure Monitor.

## <a name="prerequisites"></a>Технические условия

- Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.
- Установка Python. В этой статье используется [Python 3.7.0](https://www.python.org/downloads/), хотя более ранние версии, скорее всего, будут работать с незначительными изменениями.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/).

## <a name="create-an-application-insights-resource-in-azure-monitor"></a>Создание Application Insights ресурса в Azure Monitor

Сначала необходимо создать ресурс Application Insights в Azure Monitor, который создаст ключ инструментирования (iKey). Затем iKey используется для настройки пакета SDK Опенценсус для отправки данных телеметрии в Azure Monitor.

1. Последовательно выберите **Создать ресурс** > **Средства разработчика** > **Application Insights**.

   ![Добавление ресурса Application Insights](./media/opencensus-python/0001-create-resource.png)

1. Появится окно конфигурации. Используйте следующую таблицу для заполнения полей ввода.

   | Параметр        | Значение           | Description  |
   | ------------- |:-------------|:-----|
   | **имя**;      | Глобальное уникальное значение | Имя, идентифицирующее отслеживаемое приложение |
   | **Группа ресурсов**     | myResourceGroup      | Имя новой группы ресурсов для размещения данных Application Insights |
   | **Расположение** | Восточная часть США | Расположение рядом с вами или рядом с размещением приложения |

1. Нажмите кнопку **создания**.

## <a name="instrument-with-opencensus-python-sdk-for-azure-monitor"></a>Инструмент с пакетом SDK для Опенценсус Python для Azure Monitor

Установите Опенценсус Azure Monitor:

```console
python -m pip install opencensus-ext-azure
```

Полный список пакетов и интеграции см. в разделе [пакеты опенценсус](https://docs.microsoft.com/azure/azure-monitor/app/nuget#common-packages-for-python-using-opencensus).

> [!NOTE]
> В команде `python -m pip install opencensus-ext-azure` предполагается, что для установки Python задана `PATH`ая переменная среды. Если вы не настроили эту переменную, необходимо указать полный путь к каталогу, в котором находится исполняемый файл Python. Результатом является команда, подобная следующей: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

Пакет SDK использует три Azure Monitor экспорта для отправки различных типов телеметрии в Azure Monitor: трассировка, метрики и журналы. Дополнительные сведения об этих типах телеметрии см. [в разделе Общие сведения о платформе данных](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform). Используйте следующие инструкции для отправки этих типов телеметрии с помощью трех средств экспорта.

## <a name="telemetry-type-mappings"></a>Сопоставления типов телеметрии

Ниже приведены программы экспорта, которые Опенценсус сопоставляются с типами данных телеметрии, которые вы увидите в Azure Monitor.

![Снимок экрана: сопоставление типов телеметрии из Опенценсус с Azure Monitor](./media/opencensus-python/0012-telemetry-types.png)

### <a name="trace"></a>Трассировка

> [!NOTE]
> `Trace` в Опенценсус относится к [распределенной трассировке](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing). `AzureExporter` отправляет данные телеметрии `requests` и `dependency` Azure Monitor.

1. Сначала давайте создадим некоторые данные трассировки локально. В режиме простоя Python или в любом редакторе введите следующий код.

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2. При выполнении кода вам несколько раз будет предложено ввести значение. При каждой записи значение будет напечатано в оболочке, а модуль Опенценсус Python создаст соответствующий фрагмент `SpanData`. Проект Опенценсус определяет [трассировку в виде дерева диапазонов](https://opencensus.io/core-concepts/tracing/).
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

3. Хотя ввод значений полезен в демонстрационных целях, в конечном счете мы хотим выдать `SpanData` для Azure Monitor. Измените код с предыдущего шага на основе следующего примера кода:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

4. Теперь при запуске скрипта Python вам по-прежнему будет предложено ввести значения, но только значение будет напечатано в оболочке. Созданный `SpanData` будет отправлен на Azure Monitor. Выпущенные данные span можно найти в разделе `dependencies`.

5. Сведения о выборке в Опенценсус см. [в подборке в опенценсус](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications).

6. Дополнительные сведения о корреляции телеметрии в данных трассировки см. в описании [корреляции телеметрии](https://docs.microsoft.com/azure/azure-monitor/app/correlation#telemetry-correlation-in-opencensus-python)опенценсус.

### <a name="metrics"></a>Метрики

1. Сначала создадим данные локальной метрики. Мы создадим простую метрику для контроля числа нажатий клавиш ВВОД.

    ```python
    from datetime import datetime
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```
2. Выполнение кода приведет к многократному появлении запроса на нажатие клавиши ВВОД. Метрика создается для контроля числа нажатий клавиши ВВОД. При каждой записи значение будет увеличено, а сведения о метриках будут отображаться в консоли. Сведения включают текущее значение и текущую отметку времени при обновлении метрики.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Хотя ввод значений полезен в демонстрационных целях, в конечном счете мы хотим выдавать данные метрики Azure Monitor. Измените код с предыдущего шага на основе следующего примера кода:

    ```python
    from datetime import datetime
    from opencensus.ext.azure import metrics_exporter
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    # TODO: replace the all-zero GUID with your instrumentation key.
    exporter = metrics_exporter.new_metrics_exporter(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')

    view_manager.register_exporter(exporter)

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```

4. Программа экспорта будет отсылать данные метрик в Azure Monitor с фиксированным интервалом. Значение по умолчанию — каждые 15 секунд. Мы отслеживаем одну метрику, поэтому данные метрик с любыми значениями и отметками времени, которые она содержит, будут отправляться каждый интервал. Данные можно найти в разделе `customMetrics`.

### <a name="logs"></a>Журналы

1. Сначала создадим некоторые данные локального журнала.

    ```python
    import logging

    logger = logging.getLogger(__name__)

    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2.  Код будет постоянно запрашивать значение для входа. Запись журнала создается для каждого введенного значения.

    ```
    Enter a value: 24
    24
    Enter a value: 55
    55
    Enter a value: 123
    123
    Enter a value: 90
    90
    ```

3. Хотя ввод значений полезен в демонстрационных целях, в конечном счете, мы хотим выдавать данные журнала в Azure Monitor. Измените код с предыдущего шага на основе следующего примера кода:

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

4. Программа экспорта отправляет данные журнала в Azure Monitor. Данные можно найти в разделе `traces`. 

> [!NOTE]
> `traces` в этом контексте не совпадает с `Tracing`. `traces` относится к типу телеметрии, который будет отображаться в Azure Monitor при использовании `AzureLogHandler`. `Tracing` ссылается на концепцию в Опенценсус и относится к [распределенной трассировке](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing).

5. Чтобы отформатировать сообщения журнала, можно использовать `formatters` в встроенном [API ведения журнала](https://docs.python.org/3/library/logging.html#formatter-objects)Python.

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    format_str = '%(asctime)s - %(levelname)-8s - %(message)s'
    date_format = '%Y-%m-%d %H:%M:%S'
    formatter = logging.Formatter(format_str, date_format)
    # TODO: replace the all-zero GUID with your instrumentation key.
    handler = AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

6. В журналы можно также добавить пользовательские измерения. Они будут отображаться в виде пар "ключ-значение" в `customDimensions` в Azure Monitor.
> [!NOTE]
> Чтобы эта функция работала, необходимо передать словарь в качестве аргумента в журналы, любая другая структура данных будет пропущена. Чтобы сохранить форматирование строки, сохраните их в словаре и передайте их в качестве аргументов.

    ```python
    import logging
    
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    logger.warning('action', {'key-1': 'value-1', 'key-2': 'value2'})
    ```

7. Дополнительные сведения о том, как расширить журналы с помощью данных контекста трассировки, см. в разделе [Интеграция журналов](https://docs.microsoft.com/azure/azure-monitor/app/correlation#log-correlation)опенценсус Python.

## <a name="view-your-data-with-queries"></a>Просмотр данных с помощью запросов

Данные телеметрии, отправленные из приложения, можно просмотреть на вкладке **журналы (аналитика)** .

![Снимок экрана панели "Обзор" с "журналами (аналитика)", выделенными в красном поле](./media/opencensus-python/0010-logs-query.png)

В списке **активно**:

- Для телеметрии, отправленной с помощью программы экспорта трассировки Azure Monitor, входящие запросы отображаются в разделе `requests`. Исходящие или внутрипроцессный запросы отображаются в разделе `dependencies`.
- Для данных телеметрии, отправляемых с помощью программы экспорта метрик Azure Monitor, в разделе `customMetrics`отображаются метрики отправки.
- Для телеметрии, отправленной с помощью программы экспорта журналов Azure Monitor, в разделе `traces`отображаются журналы. Исключения отображаются в разделе `exceptions`.

Более подробные сведения об использовании запросов и журналов см. [в разделе журналы в Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs).

## <a name="learn-more-about-opencensus-for-python"></a>Дополнительные сведения о Опенценсус для Python

* [Опенценсус Python на GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Настройка](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Интеграция Flask](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Интеграция Django](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [Интеграция MySQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Дальнейшие действия

* [Сопоставление приложений](./../../azure-monitor/app/app-map.md)
* [Поиск и диагностика проблем производительности с помощью Azure Application Insights](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Оповещения

* [Тесты доступности.](../../azure-monitor/app/monitor-web-app-availability.md) Создавайте тесты, позволяющие проверить, доступен ли ваш сайт в Интернете.
* [Интеллектуальная диагностика.](../../azure-monitor/app/proactive-diagnostics.md) Эти тесты выполняются автоматически, поэтому вам не нужно их настраивать. Благодаря ей вы узнаете о необычном количестве неудачных запросов.
* [Оповещения метрики](../../azure-monitor/app/alerts.md). Настройте оповещения, чтобы предупредить вас, если метрика превышает пороговое значение. Их можно настроить для пользовательских метрик, добавляемых в код приложения.
