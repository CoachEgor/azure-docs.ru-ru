---
title: Обзор
description: Узнайте, как служба приложений Azure помогает разрабатывать и размещать веб-приложения.
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.topic: overview
ms.date: 04/30/2020
ms.custom: mvc, seodec18
ms.openlocfilehash: 619b5f16f5a913a4ec9bb1ebe2e7060fcac28d7d
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2020
ms.locfileid: "87421813"
---
# <a name="app-service-overview"></a>Обзор Службы приложений Azure

*Служба приложений Azure* — это служба на базе HTTP для размещения веб-приложений, интерфейсов REST API и серверной части мобильных решений. Вы можете выполнять разработку на привычном языке: .NET, .NET Core, Java, Ruby, Node.js, PHP или Python. Приложения работают и с легкостью масштабируются в средах Windows и Linux. Сведения о средах Linux см. в статье [Вводные сведения о службе приложений Azure на платформе Linux](containers/app-service-linux-intro.md). 

В службе приложений реализованы не только возможности Microsoft Azure для приложения, включая функции обеспечения безопасности, балансировки нагрузки, автоматического масштабирования и автоматизированного управления. Вы также можете воспользоваться такими преимуществами DevOps, как непрерывное развертывание из Azure DevOps, GitHub, Docker Hub и других источников, управление пакетами, а также возможность использования промежуточных сред, личного домена и TLS/SSL-сертификатов. 

В службе приложений плата начисляется за используемые вычислительные ресурсы Azure. Используемые вычислительные ресурсы определяются _планом службы приложений_, в котором выполняются ваши приложения. Дополнительные сведения см. в статье [Обзор планов службы приложений Azure](overview-hosting-plans.md).

## <a name="why-use-app-service"></a>Преимущества службы приложений

Ниже представлены некоторые ключевые функции службы приложений.

* **Поддержка нескольких языков и платформ**. Служба приложений полностью поддерживает ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP и Python. Кроме того, вы можете запускать [PowerShell и другие скрипты или исполняемые файлы](webjobs-create.md) в качестве фоновых служб.
* **Управляемая рабочая среда** — Служба приложений автоматически [устанавливает исправления и обеспечивает обслуживание ОС и языковых платформ](overview-patch-os-runtime.md). Сфокусируйтесь на написании эффективных приложений и предоставьте Azure управление платформой.
* **Оптимизация DevOps** — Настройка [непрерывной интеграции и непрерывного развертывания](deploy-continuous-deployment.md) с помощью Azure DevOps, GitHub, BitBucket, Docker Hub или Реестра контейнеров Azure. Повышайте уровень обновлений с помощью [тестовых и промежуточных сред](deploy-staging-slots.md). Управляйте приложениями в службе приложений с помощью оболочки [Azure PowerShell](/powershell/azure/) или [кроссплатформенного интерфейса командной строки (CLI)](/cli/azure/install-azure-cli).
* **Высокодоступное глобальное масштабирование.** [Увеличивайте](manage-scale-up.md) либо [уменьшайте](../monitoring-and-diagnostics/insights-how-to-scale.md) размер вручную или автоматически. Храните приложения в любом месте глобальной инфраструктуры центра обработки данных. При этом [соглашение об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/app-service/) гарантирует высокую доступность.
* **Подключение к платформам SaaS и локальным данным.** Доступно более 50 [соединителей](../connectors/apis-list.md) для корпоративных систем (например, SAP), служб SaaS (например, Salesforce) и популярных интернет-служб (например, Facebook). Получайте доступ к локальным данным с помощью [гибридных подключений](app-service-hybrid-connections.md) и [виртуальных сетей Azure](web-sites-integrate-with-vnet.md).
* **Безопасность и соответствие требованиям.** Служба приложений совместима со стандартами [ISO, SOC и PCI](https://www.microsoft.com/en-us/trustcenter). Выполняйте аутентификацию пользователей с помощью [Azure Active Directory](configure-authentication-provider-aad.md) или входа в учетные записи социальных сетей ([Google](configure-authentication-provider-google.md), [Facebook](configure-authentication-provider-facebook.md), [Twitter](configure-authentication-provider-twitter.md) и [Microsoft](configure-authentication-provider-microsoft.md)). Создавайте [ограничения IP-адресов](app-service-ip-restrictions.md) и [управляйте удостоверениями службы](overview-managed-identity.md).
* **Шаблоны приложений.** Вы можете выбрать любой шаблон приложения из обширного списка в [Azure Marketplace](https://azure.microsoft.com/marketplace/), например WordPress, Joomla и Drupal.
* **Интеграция с Visual Studio и Visual Studio Code.** Выделенные инструменты в Visual Studio и Visual Studio Code упрощают создание, развертывание и отладку приложений.
* **Функции API и мобильных приложений.** Служба приложений обеспечивает полную поддержку CORS для работы с RESTful API. Также она упрощает использование мобильных приложений, обеспечивая аутентификацию, автономную синхронизацию данных, отправку push-уведомлений и многое другое.
* **Независимый от сервера код.** Выполняйте фрагменты кода или скрипта по требованию без необходимости явно подготавливать и администрировать инфраструктуру. Платите только за время выполнения кода (см. статью [Документация по функциям Azure](/azure/azure-functions/)).

Помимо Службы приложений, Azure предлагает и другие службы, которые можно использовать для размещения веб-сайтов и веб-приложений. В большинстве случаев оптимальным вариантом является служба приложений.  Для реализации архитектуры микрослужб рекомендуется использовать [службу Azure Spring-Cloud](/azure/spring-cloud/) или [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric).  Если вам требуется дополнительный контроль над виртуальными машинами, на которых выполняется ваш код, рекомендуется использовать [Виртуальные машины Azure](https://azure.microsoft.com/documentation/services/virtual-machines/). Дополнительные сведения о выборе между этими службами Azure см. в статье [Сравнение службы приложений Azure, виртуальных машин, Service Fabric и облачных служб](overview-compare.md).

## <a name="next-steps"></a>Дальнейшие действия

Создайте первое веб-приложение.

> [!div class="nextstepaction"]
> [ASP.NET Core](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet-framework.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Ruby (в Linux)](containers/quickstart-ruby.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python (в Linux)](containers/quickstart-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)
