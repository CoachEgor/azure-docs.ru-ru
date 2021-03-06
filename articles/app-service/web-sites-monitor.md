---
title: Мониторинг приложений
description: Узнайте, как выполнять мониторинг приложений в службе приложений Azure с помощью портала Azure. Сведения о квотах и метриках, о которых сообщается.
author: btardif
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.topic: article
ms.date: 04/23/2020
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: b201ebb5ad8ab9d98a76a29831fa12d6174e47cc
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93125212"
---
# <a name="monitor-apps-in-azure-app-service"></a>Мониторинг приложений в службе приложений Azure
[Служба приложений Azure](./overview.md) предоставляет встроенные функции мониторинга для веб-приложений, мобильных устройств и приложений API в [портал Azure](https://portal.azure.com).

В портал Azure можно просматривать *квоты* и *метрики* для приложения и плана службы приложений, а также настраивать *оповещения* и метрики на основе правил *автоматического масштабирования* .

## <a name="understand-quotas"></a>Общие сведения о квотах

На приложения, размещенные в службе приложений, распространяются определенные ограничения на использование ресурсов. Эти ограничения определены в рамках плана службы приложений, связанного с приложением.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Если приложение используется в режиме плана *Бесплатный* или *Общий* , ограничения на использование ресурсов выражены в виде квот.

Если приложение используется в режиме плана *Базовый* , *Стандартный* или *Премиум* , ограничения выражаются в виде *размера* выделенных ресурсов ("Малый", "Средний" или "Большой") и *числа экземпляров* (1, 2, 3 и т. д.) плана службы приложений.

Для приложений, работающих в режиме плана "Бесплатный" или "Общий", предоставляются следующие квоты.

| Quota | Описание |
| --- | --- |
| **ЦП (короткое)** | Объем ресурсов ЦП, который может потребить приложение в течение 5 минут. Эта квота повторно назначается каждые 5 минут. |
| **ЦП (день)** | Общий объем ресурсов ЦП, который может потребить приложение в течение одного дня. Эта квота повторно назначается каждые 24 часа в полночь (в формате UTC); |
| **Память** | Общий объем ресурсов ЦП, который может потребить приложение. |
| **Пропускная способность** | Общий объем исходящей пропускной способности, который может использовать приложение в течение одного дня. Эта квота повторно назначается каждые 24 часа в полночь (в формате UTC); |
| **Системой** | Общий объем доступного пространства для хранения. |

Единственной квотой, применимой к приложениям, размещенным в *Basic* , *Standard* и *Premium* , является файловая система.

Дополнительные сведения об определенных квотах, ограничениях и функциях, доступных для разных номеров SKU службы приложений, см. в статье [Подписка Azure, границы, квоты и ограничения службы](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits).

### <a name="quota-enforcement"></a>Принудительное применение квот

Если квоты на использование ЦП ( *короткое* и *день* ), а также квота на *пропускную способность* будут превышены, работа приложения будет остановлена до сброса квоты. В течение этого времени все входящие запросы завершаются с ошибкой HTTP 403.

![Сообщение об ошибке "ошибка 403"][http403]

Если превышена квота памяти приложения, приложение временно останавливается.

При превышении квоты на файловую систему все операции записи завершаются сбоем. Записи в журналы также завершаются сбоем.

Квоты на использование можно увеличить или удалить из приложения путем изменения плана службы приложений.

## <a name="understand-metrics"></a>Общие сведения о метриках

> [!NOTE]
> **Использование файловой системы** — это новая метрика, которая создается глобально. Если приложение не размещено в среда службы приложений, данные не ожидаются.
> 

> [!IMPORTANT]
> **Среднее время отклика** будет нерекомендуемым, чтобы избежать путаницы с агрегатами метрик. Используйте **время ответа** в качестве замены.

> [!NOTE]
> Метрики для приложения включают запросы на сайт SCM приложения (KUDU).  Сюда входят запросы на просмотр логстреамов сайта с помощью KUDU.  Запросы логстреам могут охватывать несколько минут, что повлияет на метрики времени запроса.  Пользователи должны учитывать эту связь при использовании этих метрик с логикой автомасштабирования.
> 

Метрики предоставляют сведения о приложении или поведении плана службы приложений.

Для приложения доступны приведенные далее метрики.

| Метрика | Описание |
| --- | --- |
| **Время ответа** | Время, затраченное приложением на обслуживание запросов в секундах. |
| **Среднее время отклика (не рекомендуется)** | Среднее время, затрачиваемое приложением на обслуживание запросов в секундах. |
| **средний размер рабочего набора памяти;** | Средний объем памяти в мегабайтах (МБ), используемый приложением |
| **Соединения** | Число связанных сокетов, существующих в песочнице (w3wp.exe и его дочерние процессы).  Связанный сокет создается путем вызова API bind() или connect() и остается до тех пор, пока не будет закрыт посредством методов CloseHandle() или closesocket(). |
| **Время ЦП** | Количество времени ЦП (в секундах), используемого приложением Дополнительные сведения об этой метрике см. в разделе [время ЦП и процент ЦП](#cpu-time-vs-cpu-percentage). |
| **Текущее число сборок** | Текущее количество сборок, загруженных по всем доменам приложений в этом приложении. |
| **Данные в** | Объем входящей пропускной способности, используемый приложением (в МиБ/с) |
| **Выходные данные** | Объем исходящей пропускной способности, используемый приложением (в МиБ/с) |
| **Использование файловой системы** | Процент квот файловой системы, используемой приложением. |
| **Сборок мусора поколения 0** | Количество удалений объектов поколения 0 сборкой мусора с момента запуска процесса приложения. Сборки мусора более высокого поколения включают все предыдущие поколения.|
| **Сборок мусора поколения 1** | Количество удалений объектов поколения 1 сборкой мусора с момента запуска процесса приложения. Сборки мусора более высокого поколения включают все предыдущие поколения.|
| **Сборок мусора поколения 2** | Количество удалений объектов поколения 2 сборкой мусора с момента запуска процесса приложения.|
| **Счетчик дескрипторов** | Общее количество дескрипторов, в данный момент открытых процессом приложения.|
| **Состояние проверки работоспособности** | Среднее состояние работоспособности для экземпляров приложения в плане службы приложений.|
| **HTTP 2xx** | Количество запросов, для которых возвращается ошибка HTTP с кодом состояния, большим или равным 200, но меньшим 300 |
| **HTTP 3xx:** | Количество запросов, для которых возвращается ошибка HTTP с кодом состояния, большим или равным 300, но меньшим 400 |
| **HTTP 401:** | Количество запросов, для которых возвращается ошибка HTTP с кодом состояния 401 |
| **HTTP 403** | Количество запросов, для которых возвращается ошибка HTTP с кодом состояния 403 |
| **HTTP 404** | Количество запросов, для которых возвращается ошибка HTTP с кодом состояния 404 |
| **HTTP 406:** | Количество запросов, для которых возвращается ошибка HTTP с кодом состояния 406 |
| **4xx HTTP** | Количество запросов, для которых возвращается ошибка HTTP с кодом состояния, большим или равным 400, но меньшим 500 |
| **Ошибки HTTP-сервера** | Количество запросов, для которых возвращается ошибка HTTP с кодом состояния, большим или равным 500, но меньшим 600 |
| **Операции ввода-вывода: прочие, байт в секунду** | Скорость, с которой процесс приложения выдает байты для операций ввода-вывода, которые не затрагивают данные, такие как операции управления.|
| **Операции ввода-вывода: прочих операций в секунду** | Скорость, с которой процесс приложения выполняет операции ввода-вывода, которые не являются операциями чтения или записи.|
| **Операции ввода-вывода: чтение, байт в секунду** | Скорость, с которой процесс приложения считывает байты из операций ввода-вывода.|
| **Операции ввода-вывода: операций чтения в секунду** | Скорость, с которой процесс приложения вызывает операции ввода-вывода для чтения.|
| **Операции ввода-вывода: запись, байт в секунду** | Скорость, с которой процесс приложения записывает байты в операции ввода-вывода.|
| **Операции ввода-вывода: операций записи в секунду** | Скорость, с которой процесс приложения вызывает операции записи.|
| **рабочий набор памяти;** | Текущий объем используемой приложением памяти (в МиБ/с) |
| **Байты исключительного пользования** | Байт исключительного пользования — это текущий размер (в байтах) памяти, выделенной процессом приложения, который не может использоваться совместно с другими процессами.|
| **Запросы** | Общее количество запросов, для которых возвращается ошибка HTTP (независимо от кода состояния) |
| **Запросов в очереди приложений** | Количество запросов в очереди запросов приложений.|
| **Число потоков** | Число активных потоков в процессе приложения.|
| **Всего доменов приложений** | Текущее количество доменов приложений, загруженных в это приложение.|
| **Всего выгруженных доменов приложений** | Общее количество доменов приложений, выгруженных с момента запуска приложения.|


Для плана службы приложений доступны следующие метрики.

> [!NOTE]
> Метрики плана службы приложений поддерживаются только для планов категорий *Базовый* , *Стандартный* и *Премиум* .
> 

| Метрика | Описание |
| --- | --- |
| **Процент загрузки ЦП** | средний процент использования ЦП всеми экземплярами плана; |
| **Процент памяти** | средний процент использование памяти всеми экземплярами плана; |
| **Данные в** | средний показатель использования входящей пропускной способности всеми экземплярами плана; |
| **Выходные данные** | средний показатель использования исходящей пропускной способности всеми экземплярами плана; |
| **Длина очереди диска** | среднее число запросов на чтение и запись, поставленных в очередь в хранилище. Высокая длина очереди диска — это указание приложения, которое может замедлить работу из-за чрезмерного ввода/вывода на диск. |
| **Длина очереди HTTP** | среднее число запросов HTTP, которые ставятся в очередь перед выполнением. Большая или увеличивающая длина очереди HTTP является признаком интенсивной нагрузки плана. |

### <a name="cpu-time-vs-cpu-percentage"></a>Время и процент использования ЦП
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Доступны две метрики, которые отражают использование ЦП.

**Время ЦП** : полезно для приложений, размещенных в планах Free или Shared, так как одна из своих квот определяется в минутах ЦП, используемых приложением.

**Процент использования ЦП** : используется для приложений, размещенных в планах "базовый", "Стандартный" и "Премиум", так как их можно масштабировать. Процент загрузки ЦП является хорошим показателем общего использования во всех экземплярах.

## <a name="metrics-granularity-and-retention-policy"></a>Степень детализации метрик и политика их хранения
Метрики для приложения и плана службы приложений записываются в журнал и обрабатываются службой и [сохраняются в соответствии с этими правилами](../azure-monitor/platform/data-platform-metrics.md#retention-of-metrics).

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Мониторинг квот и метрик на портале Azure
Состояние квот и метрик, влияющих на приложение, можно просмотреть на [портале Azure](https://portal.azure.com).

![Диаграмма квот на портале Azure][quotas]

Чтобы найти квоты, выберите **Параметры**  >  **квоты** . На диаграмме отображаются указанные далее сведения о квотах. 
1. Имя квоты.
1. Интервал повторного назначения.
1. Текущее ограничение.
1. Текущее значение.

![Диаграмма метрик в портал Azure ][metrics] доступ к метрикам можно получить непосредственно на странице **обзора** ресурсов. Здесь вы увидите диаграммы, представляющие некоторые метрики приложений.

Щелкнув любую из этих диаграмм, вы перейдете к представлению метрик, где можно создавать пользовательские диаграммы, запрашивать различные метрики и многое другое. 

Дополнительные сведения о метриках см. в статье о [данных мониторинга](../azure-monitor/platform/data-platform.md).

## <a name="alerts-and-autoscale"></a>Оповещения и автомасштабирование
Метрики для плана приложения или службы приложений можно привязать к оповещениям. Дополнительные сведения см. в статье [Получение уведомлений об оповещениях](../azure-monitor/platform/alerts-classic-portal.md).

Приложения службы приложений, размещенные в планах служб приложений уровня Basic или выше, поддерживают Автомасштабирование. Автомасштабирование позволяет настроить правила, которые отслеживают метрики плана службы приложений. Правила могут увеличивать или уменьшать число экземпляров, предоставляя дополнительные ресурсы при необходимости. Правила также помогают сократить расходы на чрезмерную подготовку приложений.

Дополнительные сведения об автомасштабировании см. в статьях [Начало работы с автомасштабированием в Azure](../azure-monitor/platform/autoscale-get-started.md) и [Рекомендации по автомасштабированию](../azure-monitor/platform/autoscale-best-practices.md).

[fzilla]:https://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:https://go.microsoft.com/fwlink/?LinkID=309169

<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
