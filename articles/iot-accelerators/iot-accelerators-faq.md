---
title: Часто задаваемые вопросы об акселераторах решений для Интернета вещей Azure | Документация Майкрософт
description: В этой статье содержатся ответы на часто задаваемые вопросы по акселераторам решений IoT. Он содержит ссылки на репозитории GitHub.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: 7a2b167f90b4ec79f5fa515ded1c676ca7085e72
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "75647719"
---
# <a name="frequently-asked-questions-for-iot-solution-accelerators"></a>Часто задаваемые вопросы об акселераторах решений для Интернета вещей

См. также вопросы и ответы по вопросам [удаленного мониторинга](iot-accelerators-faq-rm-v2.md) , связанные с [подключенной фабрикой](iot-accelerators-faq-cf.md) .

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerators"></a>Где можно найти исходный код акселераторов решений?

Исходный код хранится в следующих репозиториях GitHub:

* [акселератор решения для удаленного мониторинга (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet);
* [акселератор решения для удаленного мониторинга (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java);
* [акселератор решений для прогнозного обслуживания](https://github.com/Azure/azure-iot-predictive-maintenance);
* [акселератор решения "Подключенная фабрика"](https://github.com/Azure/azure-iot-connected-factory).

### <a name="what-sdks-can-i-use-to-develop-device-clients-for-the-solution-accelerators"></a>Какие пакеты SDK можно использовать для разработки клиентов устройств для акселераторов решений?

Ссылки на пакеты SDK для устройств Интернета вещей для других языков (C, .NET, Java, Node.js, Python) можно найти в репозиториях GitHub для [пакетов SDK Microsoft Azure для Интернета вещей](https://github.com/Azure/azure-iot-sdks).

Если вы используете устройство DevKit, ресурсы и примеры можно найти в репозитории GitHub для [пакета SDK для DevKit IoT](https://github.com/Microsoft/devkit-sdk).

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-solution-accelerators"></a>Доступна ли новая архитектура микрослужб для всех трех типов акселераторов решений?

Сейчас архитектура микрослужб используется только в решении для удаленного мониторинга, так как в нем охватывается самый широкий сценарий.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Каковы преимущества новой архитектуры на базе микрослужб с открытым кодом в новом обновлении?

За последние два года облачная архитектура значительно улучшилась. Микрослужбы стали оптимальным вариантом, который обеспечивает масштабируемость и гибкость без снижения скорости разработки. В некоторых службах Майкрософт по умолчанию используется шаблон архитектуры, который обеспечивает высокую надежность и масштабируемость. Майкрософт воплощает эти знания на практике, предлагая акселераторы решений для клиентов.

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-azure-ad-tenant-how-do-i-complete-this-task"></a>Я являюсь администратором службы и хочу изменить сопоставление каталогов подписки и конкретного клиента Azure AD. Как выполнить эту задачу?

Ознакомьтесь с разделом [Добавление существующей подписки в каталог Azure AD](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Я хочу сменить администратора службы и соадминистратора при входе в систему с использованием рабочей учетной записи.

См. справочную статью [Смена администратора службы и соадминистратора при входе в систему с использованием учетной записи организации](https://azure.microsoft.com/support/changing-service-admin-and-co-admin).

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Почему появляется ошибка «У вашей учетной записи нет необходимых разрешений для создания решения. Обратитесь к администратору учетной записи или попробуйте использовать другую учетную запись»

Руководствуйтесь следующей схемой:

![Блок-схема разрешений](media/iot-accelerators-faq/flowchart.png)

> [!NOTE]
> Вы выполнили проверку, которая подтверждает ваши права глобального администратора на клиенте Azure AD и соадминистратора подписки, но ошибка не устранена. В таком случае администратор учетной записи должен удалить этого пользователя и повторно назначить необходимые разрешения в указанном порядке. Сначала добавить пользователя в качестве глобального администратора, а затем — пользователя в качестве соадминистратора подписки Azure. Если проблемы не удается устранить, обратитесь в службу [справки и поддержки](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Почему эта ошибка появляется при наличии подписки Azure? "Для создания предварительно настроенных решений требуется подписка Azure. Вы можете создать бесплатную пробную учетную запись всего за несколько минут".

Если точно известно, что у вас есть подписка Azure, проверьте сопоставление клиентов для своей подписки и убедитесь, что в раскрывающемся списке выбран правильный клиент. Если клиент выбран правильно, следуйте приведенной выше схеме и проверьте сопоставление подписки и этого клиента Azure AD.

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>Где можно найти сведения о предыдущей версии решения для удаленного мониторинга?

Предыдущая версия акселератора решения для удаленного мониторинга была известна как предварительно сконфигурированное решение удаленного мониторинга IoT Suite. Архивную документацию можно найти по адресу [https://docs.microsoft.com/previous-versions/azure/iot-suite/](https://docs.microsoft.com/previous-versions/azure/iot-suite/) .

### <a name="is-the-new-solution-accelerator-available-in-the-same-geographic-region-as-the-existing-solution"></a>Доступен ли новый акселератор решений в том же географическом регионе, что и существующее решение?

Да, новое решение для удаленного мониторинга доступно в том же географическом регионе.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-solution-accelerator-in-azureiotsolutionscom"></a>В чем разница между удалением группы ресурсов в портал Azure и нажатием кнопки Удалить в Solution Accelerator в azureiotsolutions.com?

* Если удалить Solution Accelerator в [azureiotsolutions.com](https://www.azureiotsolutions.com/), удаляются все ресурсы, развернутые при создании акселератора решений. Если вы добавляли в группу ресурсов дополнительные ресурсы, они также будут удалены.
* Если удалить группу ресурсов на [портале Azure](https://portal.azure.com), будут удалены ресурсы в этой группе. Вам также придется удалить приложение Azure Active Directory, связанное с акселератором решений.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-solution-accelerators"></a>Можно ли и дальше использовать существующие системы на базе акселераторов решений для Интернета вещей Azure?

Да. Любое имеющееся решение продолжает работать в подписке Azure, а исходный код остается доступным на GitHub.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Сколько экземпляров центров Интернета вещей можно подготовить в рамках одной подписки?

По умолчанию [в рамках одной подписки можно подготовить 10 экземпляров Центра Интернета вещей](../azure-resource-manager/management/azure-subscription-service-limits.md#iot-hub-limits). Вы можете отправить [запрос в службу поддержки Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade), чтобы увеличить этот лимит. Так как каждый акселератор решений подготавливает новый Центр Интернета вещей, в рамках одной подписки можно подготовить не более 10 акселераторов решений.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Сколько экземпляров Azure Cosmos DB можно подготовить в рамках одной подписки?

Пятьдесят. Вы можете отправить [запрос в службу поддержки Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade), чтобы увеличить это количество, но по умолчанию в рамках одной подписки можно подготовить только 50 экземпляров Cosmos DB.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Сколько бесплатных API-интерфейсов Карт Bing можно подготовить в рамках одной подписки?

Два. В рамках подписки Azure можно создать только две карты Bing уровня 1 с внутренними транзакциями для корпоративных планов. Решение для удаленного мониторинга по умолчанию подготавливается с помощью плана уровня 1 с внутренними транзакциями. Иными словами, используя одну подписку, в которую не вносились изменения, вы можете подготовить не более двух решений для удаленного мониторинга.

### <a name="can-i-create-a-solution-accelerator-if-i-have-microsoft-azure-for-dreamspark"></a>Можно ли создать акселератор решений, если я использую Microsoft Azure для DreamSpark?

> [!NOTE]
> Microsoft Azure для DreamSpark теперь называется Microsoft Imagine для учащихся.

Сейчас вы не можете создать акселератор решений с использованием учетной записи [Microsoft Azure для DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/). Но вы можете всего за пару минут создать [бесплатную пробную учетную запись Azure](https://azure.microsoft.com/free/), которая позволит создать акселератор решений.

### <a name="how-do-i-delete-an-azure-ad-tenant"></a>Как удалить клиент Azure AD?

Ознакомьтесь с записью блога Эрика Голпа (Eric Golpe) [Walkthrough of Deleting an Azure AD Tenant](https://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx) (Пошаговое руководство по удалению клиента Azure AD).

### <a name="next-steps"></a>Дальнейшие шаги

Вы также можете ознакомиться с другими функциями и возможностями акселераторов решений для Интернета вещей:

* [Изучите возможности акселератора решений для удаленного мониторинга](quickstart-remote-monitoring-deploy.md)
* [Обзор акселератора решений для прогнозного обслуживания](iot-accelerators-predictive-overview.md)
* [Развертывание акселератора решения "Подключенная фабрика"](quickstart-connected-factory-deploy.md)
* [Все аспекты безопасности Интернета вещей](/azure/iot-fundamentals/iot-security-ground-up)
