---
title: Настройка оповещений в Application Insights с помощью PowerShell | Документация Майкрософт
description: Автоматизация настройки Application Insights для получения сообщений электронной почты об изменениях метрик.
ms.topic: conceptual
ms.date: 07/23/2016
ms.openlocfilehash: 74d477b6660c0f7ec2ee32b34169bb85886936e5
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/28/2020
ms.locfileid: "87322471"
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Настройка оповещений в Application Insights с помощью PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Вы можете автоматизировать настройку [оповещений](../platform/alerts-log.md) в [Application Insights](./app-insights-overview.md).

Кроме того, вы можете [установить объекты webhook, чтобы автоматизировать реагирование на оповещения](../platform/alerts-webhooks.md).

> [!NOTE]
> Если вы хотите создать ресурсы и оповещения одновременно, рассмотрите возможность [использования шаблона Azure Resource Manager](powershell.md).

## <a name="one-time-setup"></a>Однократная настройка
Если вы ранее не использовали PowerShell для подписки Azure:

Установите модуль Azure Powershell на компьютере, где требуется выполнять сценарии.

* Установите [установщик веб-платформы Майкрософт (версии 5 или более поздней)](https://www.microsoft.com/web/downloads/platform.aspx).
* Используйте его для установки Microsoft Azure PowerShell.

## <a name="connect-to-azure"></a>Подключение к Azure
Запустите Azure PowerShell и [подключитесь к вашей подписке](/powershell/azure/).

```azurepowershell
Add-AzAccount
```


## <a name="get-alerts"></a>Получение оповещений

```azurepowershell
Get-AzAlertRule -ResourceGroup "Fabrikam" `
  [-Name "My rule"] `
  [-DetailedOutput]
```

## <a name="add-alert"></a>Добавление оповещения

```azurepowershell
Add-AzMetricAlertRule -Name "{ALERT NAME}" `
  -Description "{TEXT}" `
  -ResourceGroup "{GROUP NAME}" `
  -TargetResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
  -MetricName "{METRIC NAME}" `
  -Operator GreaterThan `
  -Threshold {NUMBER}  `
  -WindowSize {HH:MM:SS} `
  [-SendEmailToServiceOwners] `
  [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM"] `
  -Location "East US" // must be East US at present `
  -RuleType Metric
```


## <a name="example-1"></a>Пример 1
Я хочу получать электронные сообщения, если в среднем за 5 минут ответ сервера на HTTP-запросы выполняется дольше 1 секунды. Мой ресурс Application Insights называется IceCreamWebApp, и он находится в группе ресурсов Fabrikam. Я владелец подписки Azure.

GUID — это идентификатор подписки (не ключ инструментирования приложения).

```azurepowershell
Add-AzMetricAlertRule -Name "slow responses" `
  -ResourceGroup "Fabrikam" `
  -TargetResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
  -MetricName "request.duration" `
  -Operator GreaterThan `
  -Threshold 1 `
  -WindowSize 00:05:00 `
  -SendEmailToServiceOwners `
  -Location "East US" `
  -RuleType Metric
```

## <a name="example-2"></a>Пример 2
У меня есть приложение, в котором используется [TrackMetric()](./api-custom-events-metrics.md#trackmetric) для предоставления метрики salesPerHour. Я хочу, чтобы моим коллегам отправлялось электронное сообщение, если в среднем за 24 часа salesPerHour станет меньше 100.

```azurepowershell
Add-AzMetricAlertRule -Name "poor sales" `
  -Description "slow sales alert" `
  -ResourceGroup "Fabrikam" `
  -TargetResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
  -MetricName "salesPerHour" `
  -Operator LessThan `
  -Threshold 100 `
  -WindowSize 24:00:00 `
  -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
  -Location "East US" `
  -RuleType Metric
```

Это же правило можно использовать для метрики, передаваемой с помощью [параметра измерения](./api-custom-events-metrics.md#properties) другого вызова отслеживания, например TrackEvent или trackPageView.

## <a name="metric-names"></a>Имена метрик
| Имя метрики | Имя экрана | Описание |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Исключения браузера |Число необработанных исключений в браузере. |
| `basicExceptionServer.count` |Исключения сервера |Число необработанных исключений приложения |
| `clientPerformance.clientProcess.value` |Время обработки клиента |Время с момента получения последнего байта документа до загрузки модели DOM. Обработка асинхронных запросов может продолжаться. |
| `clientPerformance.networkConnection.value` |Время подключения к сети при загрузке страницы |Время, необходимое браузеру для подключения к сети. Может быть 0, если страница в кэше. |
| `clientPerformance.receiveRequest.value` |Время получения ответа |Время с момента отправки браузером запроса до начала получения ответа. |
| `clientPerformance.sendRequest.value` |Время отправки запроса |Время, необходимое браузеру для отправки запроса. |
| `clientPerformance.total.value` |Время загрузки страницы в браузере |Время с момента отправки запроса пользователя до загрузки DOM, таблиц стилей, сценариев и изображений. |
| `performanceCounter.available_bytes.value` |Объем доступной памяти |Физическая память, доступная для использования процессами или системой. |
| `performanceCounter.io_data_bytes_per_sec.value` |Скорость обработки операций ввода-вывода |Общее число байтов в секунду в операциях чтения и записи в файлы, сеть и устройства. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |Частота порождения исключений |Количество исключений, порождаемых в секунду. |
| `performanceCounter.percentage_processor_time.value` |Обработка ЦП |Процент времени, затраченного всеми потоками процессов, используемых процессором для выполнения инструкций для процесса приложения. |
| `performanceCounter.percentage_processor_total.value` |Процессорное время |Процент времени, затраченного процессором на непростаивающие потоки. |
| `performanceCounter.process_private_bytes.value` |Количество байтов исключительного использования процессов |Память, выделенная исключительно для процессов наблюдаемого приложения. |
| `performanceCounter.request_execution_time.value` |Время выполнения запроса ASP.NET |Время выполнения самого последнего запроса. |
| `performanceCounter.requests_in_application_queue.value` |Число запросов ASP.NET в очереди выполнения |Длина очереди запросов приложений. |
| `performanceCounter.requests_per_sec.value` |Частота запросов ASP.NET |Частота всех запросов из ASP.NET к приложению в секунду. |
| `remoteDependencyFailed.durationMetric.count` |Ошибки зависимости |Количество неудачных вызовов внешних ресурсов серверным приложением. |
| `request.duration` |Время ответа от сервера |Время с момента получения HTTP-запроса до завершения отправки ответа. |
| `request.rate` |Частота запросов |Частота всех запросов к приложению в секунду. |
| `requestFailed.count` |Failed requests (Неудачные запросы) |Число HTTP-запросов, приведших к отображению кода ответа >= 400. |
| `view.count` |Просмотры страниц |Количество клиентских запросов пользователя для веб-страницы. Искусственный трафик отфильтровывается. |
| {имя пользовательской метрики} |{имя метрики} |Значение метрики, переданное [TrackMetric](./api-custom-events-metrics.md#trackmetric) или в [параметре измерения вызова отслеживания](./api-custom-events-metrics.md#properties). |

Метрики отправляются различными модулями телеметрии:

| Группа метрик | Модуль сборщика |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>представление |[Browser JavaScript](./javascript.md) |
| performanceCounter |[Производительность](./configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Зависимость](./configuration-with-applicationinsights-config.md) |
| request,<br/>requestFailed |[Server request](./configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>Веб-перехватчики
Вы можете [автоматизировать реагирование на оповещения](../platform/alerts-webhooks.md). При возникновении оповещения Azure будет вызывать выбранный вами веб-адрес.

## <a name="see-also"></a>См. также раздел
* [Сценарий настройки Application Insights](./create-new-resource.md#creating-a-resource-automatically)
* [Создание ресурсов Application Insights и веб-тестов на основе шаблонов](powershell.md)
* [Автоматизация связывания Диагностики Microsoft Azure с Application Insights](powershell-azure-diagnostics.md)
* [Автоматизация реагирования на предупреждения](../platform/alerts-webhooks.md)

