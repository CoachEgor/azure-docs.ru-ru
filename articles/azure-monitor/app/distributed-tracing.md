---
title: Распределенная трассировка в Azure Application Insights | Документация Майкрософт
description: Предоставление сведений о поддержке корпорацией Майкрософт распределенной трассировки через локальный сервер пересылки и партнерство в проекте OpenCensus
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: nikmd23
ms.author: nimolnar
ms.date: 09/17/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 0b51617966ad85037ad29f3e5005a17f66602e01
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72677125"
---
# <a name="what-is-distributed-tracing"></a>Что такое распределенная трассировка?

Появление современных облачных архитектур и архитектур [микрослужб](https://azure.com/microservices) привело к созданию простых, независимо развертываемых служб, которые могут помочь снизить расходы при повышении доступности и пропускной способности. Но несмотря на то что эти движения облегчили понимание отдельных служб в целом, они усложнили общую систему и ее отладку.

В монолитных архитектурах мы привыкли к отладке с помощью стеков вызовов. Стеки вызовов — это блестящие инструменты для отображения потока выполнения (метод A, вызываемый методом Б, который вызывается методом В), а также данных и параметров каждого из этих вызовов. Это очень удобно для монолитных структур или служб, работающих в одном процессе, но как проводить отладку, когда вызов проходит через границу процесса, а не является просто ссылкой на локальный стек? 

Вот где происходит распределенная трассировка.  

Распределенная трассировка является эквивалентом стеков вызовов для современных облачных архитектур и архитектур микрослужб с добавлением упрощенного профилировщика производительности. В Azure Monitor предлагаются два варианта использования распределенной трассировки данных. Первое — это наша [диагностика транзакций](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics), которая похожа на стек вызовов с добавлением измерения времени. Представление диагностики транзакций обеспечивает видимость в одной единственной транзакции или запросе и помогает найти основную причину проблем с безопасностью и узкие места производительности для каждого запроса.

Azure Monitor также предлагает представление [схемы приложений](https://docs.microsoft.com/azure/application-insights/app-insights-app-map), которая объединяет много транзакций для отображения топологического вида взаимодействия систем и среднего коэффициента производительности и ошибок. 

## <a name="how-to-enable-distributed-tracing"></a>Включение распределенной трассировки

Включить распределенную трассировку между службами в приложении так же просто, как добавить правильный пакет SDK или библиотеку для каждой службы, в зависимости от языка, на котором была реализована служба.

## <a name="enabling-via-application-insights-sdks"></a>Включение с помощью пакетов SDK Application Insights

Пакеты SDK Application Insights для .NET, .NET Core, Java, Node.js и JavaScript изначально поддерживают распределенную трассировку. Инструкции по установке и настройке каждого пакета SDK Application Insights приведены ниже.

* [.NET](https://docs.microsoft.com/azure/application-insights/quick-monitor-portal)
* [.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-dotnetcore-quick-start)
* [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-get-started)
* [Node.js](https://docs.microsoft.com/azure/application-insights/app-insights-nodejs-quick-start)
* [JavaScript](https://docs.microsoft.com/azure/application-insights/app-insights-javascript)

При установке и настройке надлежащего пакета SDK Application Insights для популярных фреймворков, библиотек и технологий информация о трассировке автоматически собирается с помощью сборщиков зависимостей SDK. Полный список поддерживаемых технологий доступен в [документации по автоматическому сбору зависимостей](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies).

 Кроме того, любую технологию можно отслеживать вручную с помощью вызова [TrackDependency](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics) в методе [TeleletryClient](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics).

## <a name="enable-via-opencensus"></a>Включение с помощью OpenCensus

Помимо пакетов SDK, Application Insights поддерживает распределенную трассировку с помощью [OpenCensus](https://opencensus.io/). OpenCensus является единым для служб распределением библиотек с открытым исходным кодом, независимым от поставщика, для обеспечения сбора метрик и распределенной трассировки. Он также позволяет сообществу открытого исходного кода включить распределенную трассировку с использованием популярных технологий, таких как Redis, Memcached или MongoDB. [Корпорация Майкрософт в OpenCensus сотрудничает с несколькими другими партнерами по мониторингу и облакам](https://open.microsoft.com/2018/06/13/microsoft-joins-the-opencensus-project/).

Для добавления возможностей распределенной трассировки в приложение с OpenCensus сначала [установите и настройте локальный сервер пересылки Application Insights](./../../azure-monitor/app/opencensus-local-forwarder.md). Теперь можно настроить OpenCensus для маршрутизации данных распределенной трассировки с помощью локального сервера пересылки. Поддерживаются как [Python](./../../azure-monitor/app/opencensus-python.md), так и [Go](./../../azure-monitor/app/opencensus-go.md).

На веб-сайте OpenCensus поддерживается справочная документация по API для [Python](https://opencensus.io/api/python/trace/usage.html) и [Go](https://godoc.org/go.opencensus.io), а также несколько различных руководств по использованию OpenCensus. 

## <a name="next-steps"></a>Дальнейшие действия

* [Руководство по использованию OpenCensus для Python](https://opencensus.io/api/python/trace/usage.html)
* [Сопоставление приложений](./../../azure-monitor/app/app-map.md)
* [Поиск и диагностика проблем производительности с помощью Azure Application Insights](./../../azure-monitor/learn/tutorial-performance.md)
