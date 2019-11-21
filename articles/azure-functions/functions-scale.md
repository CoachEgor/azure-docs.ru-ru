---
title: Масштабирование и размещение Функций Azure
description: Learn how to choose between Azure Functions Consumption plan and Premium plan.
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.topic: conceptual
ms.date: 03/27/2019
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b9644e89591d7d8b7642b5f381434357191d1711
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74226599"
---
# <a name="azure-functions-scale-and-hosting"></a>Масштабирование и размещение Функций Azure

When you create a function app in Azure, you must choose a hosting plan for your app. There are three hosting plans available for Azure Functions: [Consumption plan](#consumption-plan), [Premium plan](#premium-plan), and [App Service plan](#app-service-plan).

The hosting plan you choose dictates the following behaviors:

* How your function app is scaled.
* The resources available to each function app instance.
* Support for advanced features, such as VNET connectivity.

Both Consumption and Premium plans automatically add compute power when your code is running. Your app is scaled out when needed to handle load, and scaled down when code stops running. For the Consumption plan, you also don't have to pay for idle VMs or reserve capacity in advance.  

Premium plan provides additional features, such as premium compute instances, the ability to keep instances warm indefinitely, and VNet connectivity.

App Service plan allows you to take advantage of dedicated infrastructure, which you manage. Your function app doesn't scale based on events, which means is never scales down to zero. (Requires that [Always on](#always-on) is enabled.)

> [!NOTE]
> You can switch between Consumption and Premium plans by changing the plan property of the function app resource.

## <a name="hosting-plan-support"></a>Hosting plan support

Feature support falls into the following two categories:

* _Generally available (GA)_ : fully supported and approved for production use.
* _Preview_: not yet fully supported and approved for production use.

The following table indicates the current level of support for the three hosting plans, when running on either Windows or Linux:

| | План потребления | План "Премиум" | Dedicated plan |
|-|:----------------:|:------------:|:----------------:|
| Windows | Общая доступность | Общая доступность | Общая доступность |
| Linux | Общая доступность | Общая доступность | Общая доступность |

## <a name="consumption-plan"></a>План потребления

When you're using the Consumption plan, instances of the Azure Functions host are dynamically added and removed based on the number of incoming events. Этот бессерверный план автоматически масштабируется, и вы платите за вычислительные ресурсы только тогда, когда ваши функции запущены. В плане потребления функция должна выполняться в течение настроенного времени (время ожидания).

Плата начисляется на основе числа операций выполнения, времени выполнения и объема использованной памяти. Плата начисляется совокупно за использование всех функций приложения-функции. Дополнительные сведения см. на [странице цен на Функции Azure](https://azure.microsoft.com/pricing/details/functions/).

План потребления является планом размещения по умолчанию. Он включает следующие преимущества:

* вы платите, только если функции выполняются;
* масштабирование выполняется автоматически, даже во время периодов высокой нагрузки.

Function apps in the same region can be assigned to the same Consumption plan. There's no downside or impact to having multiple apps running in the same Consumption plan. Assigning multiple apps to the same consumption plan has no impact on resilience, scalability, or reliability of each app.

To learn more about how to estimate costs when running in a Consumption plan, see [Understanding Consumption plan costs](functions-consumption-costs.md).

## <a name="premium-plan"></a>Premium plan

When you're using the Premium plan, instances of the Azure Functions host are added and removed based on the number of incoming events just like the Consumption plan.  Premium plan supports the following features:

* Perpetually warm instances to avoid any cold start
* Подключение виртуальных сетей
* Unlimited execution duration
* Premium instance sizes (one core, two core, and four core instances)
* More predictable pricing
* High-density app allocation for plans with multiple function apps

Information on how you can configure these options can be found in the [Azure Functions premium plan document](functions-premium-plan.md).

Instead of billing per execution and memory consumed, billing for the Premium plan is based on the number of core seconds and memory used across needed and pre-warmed instances. At least one instance must be warm at all times per plan. This means that there is a minimum monthly cost per active plan, regardless of the number of executions. Keep in mind that all function apps in a Premium plan share pre-warmed and active instances.

Consider the Azure Functions premium plan in the following situations:

* Приложения-функции выполняются непрерывно или почти непрерывно.
* You have a high number of small executions and have a high execution bill but low GB second bill in the consumption plan.
* You need more CPU or memory options than what is provided by the Consumption plan.
* Your code needs to run longer than the [maximum execution time allowed](#timeout) on the Consumption plan.
* You require features that are only available on a Premium plan, such as VNET/VPN connectivity.

When running JavaScript functions on a Premium plan, you should choose an instance that has fewer vCPUs. For more information, see the [Choose single-core Premium plans](functions-reference-node.md#considerations-for-javascript-functions).  

## <a name="app-service-plan"></a>Dedicated (App Service) plan

Your function apps can also run on the same dedicated VMs as other App Service apps (Basic, Standard, Premium, and Isolated SKUs).

Consider an App Service plan in the following situations:

* У вас есть неиспользуемые виртуальные машины, в которых уже запущены другие экземпляры служб приложений.
* You want to provide a custom image on which to run your functions.

You pay the same for function apps in an App Service Plan as you would for other App Service resources, like web apps. Дополнительную информацию о том, как действует план службы приложений, см. в статье [Подробный обзор планов службы приложений Azure](../app-service/overview-hosting-plans.md).

With an App Service plan, you can manually scale out by adding more VM instances. You can also enable autoscale. Дополнительные сведения см. в статье [Масштабирование числа экземпляров вручную или автоматически](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). Вы можете также увеличивать масштаб, используя разные планы службы приложений. Дополнительные сведения см. в статье [Увеличение масштаба приложения в Azure](../app-service/manage-scale-up.md). 

При запуске функций JavaScript в плане службы приложений нужно выбрать план с меньшим количеством виртуальных ЦП. For more information, see [Choose single-core App Service plans](functions-reference-node.md#choose-single-vcpu-app-service-plans). 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

### <a name="always-on"></a> Always On

При использовании плана службы приложений следует включить параметр **Всегда включено**, чтобы ваше приложение-функция выполнялось правильно. В плане службы приложений среда выполнения функции переходит в состояние бездействия через несколько минут отсутствия активности, поэтому только триггеры HTTP могут на самом деле "пробудить" ваши функции. Параметр "Всегда включено" доступен только для плана службы приложений. В плане потребления платформа активирует приложения-функции автоматически.

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]


Даже с включенным параметром "Всегда включено" время ожидания выполнения для отдельных функций управляется с помощью параметра `functionTimeout` в файле проекта [host.json](functions-host-json.md#functiontimeout).

## <a name="determine-the-hosting-plan-of-an-existing-application"></a>Determine the hosting plan of an existing application

Чтобы определить план размещения, используемый приложением-функцией, на [портале Azure](https://portal.azure.com) просмотрите для приложения-функции раздел **План службы приложений/ценовая категория** на вкладке **Обзор**. Для планов службы приложений также указывается ценовая категория.

![Просмотр плана масштабирования на портале](./media/functions-scale/function-app-overview-portal.png)

Для определения плана можно также использовать Azure CLI, как показано ниже:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Если в результате выполнения команды возвращается значение `dynamic`, ваше приложение-функция находится в плане потребления. When the output from this command is `ElasticPremium`, your function app is in the Premium plan. All other values indicate different tiers of an App Service plan.

## <a name="storage-account-requirements"></a>Требования к учетной записи хранения

On any plan, a function app requires a general Azure Storage account, which supports Azure Blob, Queue, Files, and Table storage. Это связано с тем, что Функции Azure используют службу хранилища Azure для таких операций, как управление триггерами и ведение журнала выполнения функций, но некоторые учетные записи хранения не поддерживают очереди и таблицы. Эти учетные записи, которые включают учетные записи хранения только для больших двоичных объектов (включая хранилище класса Premium) и учетные записи хранения общего назначения с репликацией ZRS, отфильтровываются на основе выбранных вариантов **учетной записи хранения** при создании приложения-функции.

The same storage account used by your function app can also be used by your triggers and bindings to store your application data. However, for storage-intensive operations, you should use a separate storage account.   

<!-- JH: Does using a Premium Storage account improve perf? -->

Дополнительные сведения о типах учетных записей хранилища см. в разделе [Введение в службы хранилища Azure](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-and-premium-plans-work"></a>How the consumption and premium plans work

In the consumption and premium plans, the Azure Functions infrastructure scales CPU and memory resources by adding additional instances of the Functions host, based on the number of events that its functions are triggered on. Each instance of the Functions host in the consumption plan is limited to 1.5 GB of memory and one CPU.  An instance of the host is the entire function app, meaning all functions within a function app share resource within an instance and scale at the same time. Function apps that share the same consumption plan are scaled independently.  In the premium plan, your plan size will determine the available memory and CPU for all apps in that plan on that instance.  

Function code files are stored on Azure Files shares on the function's main storage account. При удалении основной учетной записи хранения приложения-функции, файлы кода функции будут удалены без возможности восстановления.

### <a name="runtime-scaling"></a>Масштабирование среды выполнения

Решение "Функции Azure" использует компонент, называемый *контроллером масштабирования*, чтобы отслеживать скорость событий и определять, необходимо ли выполнять масштабирование. Контроллер масштабирования использует эвристику для каждого типа триггеров. Например, при использовании триггера хранилища очередей Azure масштабирование выполняется в зависимости от длины запроса и возраста самого старого сообщения в очереди.

The unit of scale for Azure Functions is the function app. При масштабировании приложения-функции выделяются дополнительные ресурсы для выполнения нескольких экземпляров узла службы "Функции Azure". И наоборот, когда потребность в вычислительных ресурсах уменьшается, контроллер масштабирования удаляет экземпляры узла функции. Если никакие функции не выполняются, то число экземпляров в приложении-функции в конечном итоге сократится до нуля.

![Мониторинг событий и создание экземпляров с помощью контроллера масштабирования](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Основные сведения о действиях при масштабировании

Масштабирование может зависеть от ряда факторов и выполняться по-разному в зависимости от выбранного триггера и языка. There are a few intricacies of scaling behaviors to be aware of:

* Отдельное приложение-функция масштабируется максимум до 200 экземпляров. Один экземпляр может обрабатывать одновременно более одного сообщения или запроса, поэтому для количества одновременных выполнений не установлено ограничение.
* For HTTP triggers, new instances will only be allocated at most once every 1 second.
* For non-HTTP triggers, new instances will only be allocated at most once every 30 seconds.

Для различных триггеров также могут быть установлены разные ограничения масштабирования, а также ограничения, указанные в следующем документе:

* [Концентратор событий](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Рекомендации и шаблоны для масштабируемых приложений

В приложении-функции существует множество аспектов, влияющих на качество его масштабирования, в том числе конфигурация узла, используемый объем среды выполнения и эффективность использования ресурсов.  Дополнительные сведения см. в разделе [Рекомендации по масштабируемости](functions-best-practices.md#scalability-best-practices). Вам также следует учитывать поведение подключений при масштабировании приложения-функции. См. дополнительные сведения об [управлении подключениями в службе "Функции Azure"](manage-connections.md).

### <a name="billing-model"></a>Модель оплаты

Billing for the different plans is described in detail on the [Azure Functions pricing page](https://azure.microsoft.com/pricing/details/functions/). Использование вычисляется на уровне приложения-функции. При этом учитывается только время выполнения кода функции. Счета выставляются по следующим единицам:

* **Потребление ресурсов в гигабайтах в секунду (ГБ/с)** . Вычисляется как сочетание объема памяти и времени выполнения для всех функций, выполняемых в приложении-функции. 
* **Выполнения**. Вычисляются при каждом выполнении функции в ответ на событие.

Useful queries and information on how to understand your consumption bill can be found [on the billing FAQ](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ).

[Azure Functions pricing page]: https://azure.microsoft.com/pricing/details/functions

## <a name="service-limits"></a>Ограничения службы

The following table indicates the limits that apply to function apps when running in the various hosting plans:

[!INCLUDE [functions-limits](../../includes/functions-limits.md)]
