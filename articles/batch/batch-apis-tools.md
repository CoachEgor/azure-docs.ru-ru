---
title: API-интерфейсы и инструменты для разработчиков. Пакетная служба Azure | Документация Майкрософт
description: Сведения об API-интерфейсах и средствах, доступных для разработки решений с помощью пакетной службы Azure.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 00d2a74946957f690979eec1d3a03a9b766299d8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025883"
---
# <a name="overview-of-batch-apis-and-tools"></a>Общие сведения об API-интерфейсах и средствах пакетной службы

Обычно обработка параллельных рабочих нагрузок, использующих пакетную службу Azure, выполняется программным способом с помощью API-интерфейсов пакетной службы. Ваше клиентское приложение или служба могут использовать API-интерфейсы пакетной службы для взаимодействия с пакетной службой. С помощью API-интерфейсов пакетной службы можно создавать пулы вычислительных узлов виртуальные машины или облачные службы, а также управлять этими ресурсами. Вы можете запланировать выполнение заданий и задач на этих узлах. 

Вы можете эффективно обрабатывать крупномасштабные рабочие нагрузки в своей организации или предоставлять внешние интерфейсы служб клиентам, чтобы они могли выполнять задания и задачи (по требованию или по расписанию) на одном, сотнях или тысячах узлов. Кроме того, пакетную службу Azure можно использовать как часть более крупного рабочего процесса под управлением таких средств, как [фабрика данных Azure](../data-factory/transform-data-using-dotnet-custom-activity.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> Дополнительные сведения о возможностях, которые предоставляет API пакетной службы, см. в статье [Обзор функций пакетной службы Azure](batch-api-basics.md).
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Учетные записи Azure для разработки с помощью пакетной службы
При разработке решений с использованием пакетной службы требуются следующие учетные записи в вашей подписке Azure:

* **Учетная запись пакетной службы** — это ресурсы пакетной службы Azure, в том числе пулы, вычислительные узлы, задания и задачи, связанные с [учетной записью пакетной службы](batch-api-basics.md#account) Azure. Когда приложение отправляет запрос к пакетной службе, выполняется проверка подлинности запроса с использованием имени учетной записи пакетной службы Azure, URL-адреса учетной записи и ключа доступа или токена Azure Active Directory. Вы можете [создать учетную запись пакетной службы](batch-account-create-portal.md) на портале Azure или программным способом.
* **Учетная запись хранения** — Пакетная служба включает встроенную поддержку работы с файлами в [службе хранилища Azure][azure_storage]. При работе с пакетной службой хранилище BLOB-объектов Azure используется преимущественно не только для промежуточного хранения файлов программ и данных (запускаются и обрабатываются задачами соответственно), но и для хранения выходных данных (результаты выполнения задач). Параметры учетной записи хранения в пакетной службе см. в [обзоре функций пакетной службы](batch-api-basics.md#azure-storage-account).

## <a name="batch-service-apis"></a>API-интерфейсы пакетной службы

Приложения и службы могут напрямую вызывать REST API или использовать следующие клиентские библиотеки для выполнения рабочих нагрузок пакетной службы Azure и управления ими.

| API | Справочные материалы по API | Загрузить | Учебник | Примеры кода | Подробнее |
| --- | --- | --- | --- | --- | --- |
| **Пакетная служба (REST)** |[docs.microsoft.com][batch_rest] |Н/Д |- |- | [Поддерживаемые версии](/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Пакетная служба (.NET)** |[docs.microsoft.com][api_net] |[NuGet][api_net_nuget] |[Руководство](tutorial-parallel-dotnet.md) |[GitHub][api_sample_net] | [Заметки о выпуске](https://aka.ms/batch-net-dataplane-changelog) |
| **Пакетная служба Python** |[docs.microsoft.com][api_python] |[PyPI][api_python_pypi] |[Руководство](tutorial-parallel-python.md)|[GitHub][api_sample_python] | [Файл сведений](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Node.js для пакетной службы** |[docs.microsoft.com][api_nodejs] |[npm][api_nodejs_npm] |[Руководство](batch-nodejs-get-started.md) |- | [Файл сведений](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Java для пакетной службы** |[docs.microsoft.com][api_java] |[Maven][api_java_jar] |- |[Файл сведений][api_sample_java] | [Файл сведений](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>API-интерфейсы для управления пакетной службой

API-интерфейсы Azure Resource Manager для пакетной службы предоставляют программный доступ к учетным записям пакетной службы. Применяя эти API, можно программно управлять учетными записями пакетной службы, квотами, пакетами приложений и другими ресурсами через поставщик Microsoft.Batch.  

| API | Справочные материалы по API | Загрузить | Учебник | Примеры кода |
| --- | --- | --- | --- | --- |
| **REST для управления пакетной службой** |[docs.microsoft.com][api_rest_mgmt] |Н/Д |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Библиотека .NET для управления пакетной службой** |[docs.microsoft.com][api_net_mgmt] |[NuGet][api_net_mgmt_nuget] | [Руководство](batch-management-dotnet.md) |[GitHub][api_sample_net] |
| **Python для управления пакетной службой** |[docs.microsoft.com][api_python_mgmt] |[PyPI][api_python_mgmt_pypi] |- |- |
| **Node.js для управления пакетной службой** |[docs.microsoft.com][api_nodejs_mgmt] |[npm][api_nodejs_mgmt_npm] |- |- | 
| **Java для управления пакетной службой** |- |[Maven][api_java_mgmt_jar] |- |- |
## <a name="batch-command-line-tools"></a>Программы командной строки пакетной службы

Эти программы командной строки обеспечивают ту же функциональность, что и API-интерфейсы пакетной службы и службы управления пакетной службой. 

* [Командлеты PowerShell для пакетной][batch_ps]службы. командлеты пакетной службы Azure в модуле [Azure PowerShell](/powershell/azure/overview) позволяют управлять ресурсами пакетной службы с помощью PowerShell.
* [Azure CLI](/cli/azure) — это кроссплатформенный набор средств, который обеспечивает взаимодействие с разными службами Azure, включая пакетную службу и службу управления пакетной службой, с помощью команд оболочки. Дополнительные сведения об использовании Azure CLI с пакетной службой см. в статье [Управление ресурсами пакетной службы с помощью Azure CLI](batch-cli-get-started.md).

## <a name="other-tools-for-application-development"></a>Другие средства для разработки приложений

Ниже приведены дополнительные средства, которые можно использовать для создания и отладки приложений и служб пакетной службы.

* [Портал Azure][portal]. Вы можете создавать, отслеживать и удалять пулы, задания и задачи пакетной службы в портал Azure. Во время выполнения заданий можно просмотреть сведения о состоянии этих и других ресурсов, а также скачать файлы из вычислительных узлов в пулах. Например, при устранении неполадок можно скачать файл `stderr.txt` задачи, завершившейся сбоем. Кроме того, можно скачать файлы удаленного рабочего стола, которые можно использовать для входа на вычислительные узлы.
* [Azure Batch Explorer][batch_labs]: Batch Explorer (ранее именуемый BatchLabs) — это бесплатное, полнофункциональное автономное клиентское средство, помогающее создавать, отлаживать и отслеживать приложения пакетной службы Azure. Скачайте [пакет установки](https://azure.github.io/BatchExplorer/) для Mac, Linux или Windows.
* Пакетная служба [Azure Shipyard](https://github.com/Azure/batch-shipyard)— это средство, помогающее подготавливать, выполнять и отслеживать пакетную обработку на основе контейнера и рабочие нагрузки HPC в пакетной службе Azure.
* [Обозреватель службы хранилища Azure][storage_explorer]. Хотя и не является средством пакетной службы Azure, обозреватель службы хранилища является еще одним ценным средством, которое можно использовать при разработке и отладке решений пакетной службы.

## <a name="additional-resources"></a>Дополнительные ресурсы

- Дополнительные сведения о регистрации событий приложения пакетной службы см. в статье [События журнала для диагностики и мониторинга решений пакетной службы](batch-diagnostics.md). Ссылки на события, которые происходят в пакетной службе, см. в статье [Пакетная аналитика](batch-analytics.md).
- Сведения о переменных среды для вычислительных узлов см. в статье [Переменные среды вычислительного узла пакетной службы Azure](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Дальнейшие действия

* Информация, необходимая для тех, кто готовится использовать пакетную службу, доступна в статье [Обзор функций пакетной службы для разработчиков](batch-api-basics.md). Эта статья содержит дополнительные подробные сведения о таких ресурсах пакетной службы, как пулы, узлы, задания, задачи и многие функции API, которые можно использовать при создании приложения пакетной службы.
* Сведения об использовании C# и библиотеки .NET для пакетной службы при обработке простой рабочей нагрузки с помощью стандартного рабочего процесса пакетной службы см. в статье [Начало работы с библиотекой пакетной службы Azure для .NET](tutorial-parallel-dotnet.md). Кроме того, доступны [версия Python](tutorial-parallel-python.md) и [руководство Node.js](batch-nodejs-get-started.md).
* Скачайте [примеры кода на сайте GitHub][github_samples] , чтобы узнать, C# как и язык Python могут взаимодействовать с пакетной службой для планирования и обработки образцов рабочих нагрузок.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: /java/api/overview/azure/batch
[api_java_mgmt]: /java/api/overview/azure/batch/managementapi
[api_java_jar]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_java_mgmt_jar]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: /dotnet/api/overview/azure/batch/
[api_net_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Batch/
[api_rest_mgmt]: /rest/api/batchmanagement/
[api_net_mgmt]: /dotnet/api/overview/azure/batch/management
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: /javascript/api/overview/azure/batch/client
[api_nodejs_mgmt]: /javascript/api/overview/azure/batch/management
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_nodejs_mgmt_npm]: https://www.npmjs.com/package/azure-arm-batch
[api_python]: /python/api/overview/azure/batch/client
[api_python_mgmt]: /python/api/overview/azure/batch/management
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_python_mgmt_pypi]: https://pypi.python.org/pypi/azure-mgmt-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/module/az.batch/
[batch_rest]: /rest/api/batchservice/
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_labs]: https://azure.github.io/BatchExplorer/
[storage_explorer]: https://storageexplorer.com/
[portal]: https://portal.azure.com
