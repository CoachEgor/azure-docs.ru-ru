---
title: Общие сведения об интеграции с предприятием B2B в Azure Logic Apps | Документация Майкрософт
description: Создание автоматизированных рабочих процессов B2B для решений корпоративной интеграции с помощью Azure Logic Apps и Пакета интеграции Enterprise
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.date: 09/08/2016
ms.openlocfilehash: 8c0e47f5bed0799b8536cecb38a85ed76185d0cd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60845839"
---
# <a name="overview-b2b-enterprise-integration-scenarios-in-azure-logic-apps-with-enterprise-integration-pack"></a>Обзор: Сценарии интеграции с предприятием B2B в Azure Logic Apps с помощью Пакета интеграции Enterprise

Для выполнения рабочих процессов "бизнес — бизнес" (B2B) и бесперебойного взаимодействия с Azure Logic Apps можно включить сценарии интеграции с помощью облачного решения корпорации Майкрософт, пакета интеграции Enterprise. Организации могут обмениваться электронными сообщениями, даже если они используют разные протоколы и форматы. Пакет преобразует разные форматы в единый формат, который корпоративные системы могут распознавать и обрабатывать. Организации могут обмениваться сообщениями с помощью стандартных протоколов, в том числе [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md) и [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md). Сообщения можно защитить с помощью шифрования и цифровой подписи.

Если вы уже работали с BizTalk Server или службами BizTalk Microsoft Azure, вам будет легко использовать функции интеграции Enterprise, так как принципы их работы похожи. Одно из основных отличий — в интеграции Enterprise используются учетные записи интеграции, которые упрощают хранение артефактов, используемых при обмене данными "бизнес-бизнес", и управление ими. 

В контексте архитектуры пакет интеграции Enterprise основывается на интеграции учетных записей. Эти учетные записи являются облачными контейнерами, которые хранят все ваши артефакты, такие как схемы, партнеры, сертификаты, карты и соглашения. Эти артефакты можно использовать для проектирования, развертывания и обслуживания приложений B2B, а также создания рабочих процессов B2B для приложений логики. Прежде чем использовать артефакты, необходимо связать свою учетную запись интеграции с приложением логики. После этого приложение логики будет иметь доступ к артефактам учетной записи интеграции.

## <a name="why-should-you-use-enterprise-integration"></a>Для чего следует использовать интеграцию Enterprise?

* Благодаря интеграции Enterprise можно хранить все артефакты в одном расположении — в своей учетной записи интеграции.
* Вы можете создавать рабочие процессы B2B и реализовать интеграцию с приложениями SaaS сторонних поставщиков, локальными приложениями или пользовательскими приложениями, используя ядро Azure Logic Apps и все его соединители.
* Вы можете создать пользовательский код для приложения логики с помощью функций Azure.

## <a name="how-to-get-started-with-enterprise-integration"></a>Как приступить к работе с интеграцией Enterprise?

Создавать приложения B2B и управлять ими с помощью пакета интеграции Enterprise можно в конструкторе приложений логики на **портале Azure**. Вы также можете управлять приложениями логики с помощью [PowerShell](https://docs.microsoft.com/powershell/module/az.logicapp).

Ниже описаны действия, которые необходимо выполнить перед созданием приложений на портале Azure.

![Изображение для обзора](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Каковы наиболее распространенные сценарии применения?

Интеграция Enterprise поддерживает следующие отраслевые стандарты:

* EDI — электронный обмен данными;
* EAI — интеграция приложений.

## <a name="heres-what-you-need-to-get-started"></a>Вот что необходимо для немедленного начала работы.

* Подписка Azure с учетной записью интеграции.
* Visual Studio 2015 для создания карт и схем.
* [Microsoft Azure Logic Apps Enterprise Integration Tools for Visual Studio 2015 2.0.](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a>Попробуйте сейчас

[Разверните полностью работоспособный пример приложения логики для обмена данными AS2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive), который использует функции B2B Azure Logic Apps.

## <a name="learn-more"></a>Подробнее
* [Соглашения](../logic-apps/logic-apps-enterprise-integration-agreements.md "Узнайте о соглашениях интеграции Enterprise")
* [Сценарии "бизнес — бизнес"](../logic-apps/logic-apps-enterprise-integration-b2b.md "Узнайте, как создавать приложения логики с возможностями \"бизнес — бизнес\"")  
* [Сертификаты](logic-apps-enterprise-integration-certificates.md "Узнайте о сертификатах интеграции Enterprise")
* [Кодирование и декодирование неструктурированных файлов](logic-apps-enterprise-integration-flatfile.md "Узнайте, как кодировать и декодировать содержимое неструктурированных файлов")  
* [Учетные записи интеграции](../logic-apps/logic-apps-enterprise-integration-accounts.md "Узнайте больше об учетных записях интеграции")
* [Карты](../logic-apps/logic-apps-enterprise-integration-maps.md "Узнайте о картах интеграции Enterprise")
* [Партнеры](logic-apps-enterprise-integration-partners.md "Узнайте о партнерах интеграции Enterprise")
* [Схемы](logic-apps-enterprise-integration-schemas.md "Узнайте о схемах интеграции Enterprise")
* [Проверка сообщений XML](logic-apps-enterprise-integration-xml.md "Узнайте, как проверять сообщения XML с помощью приложений логики")
* [Преобразование XML-файла](logic-apps-enterprise-integration-transform.md "Узнайте о картах интеграции Enterprise")
* [Корпоративные соединители интеграции](../connectors/apis-list.md "Узнайте о соединителях пакета интеграции Enterprise.")
* [Метаданные учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-metadata.md "Узнайте о метаданных учетной записи интеграции")
* [Мониторинг сообщений "бизнес — бизнес"](logic-apps-monitor-b2b-message.md "Узнайте больше о мониторинге сообщений \"бизнес — бизнес\"")
* [Отслеживание сообщений B2B в Azure Monitor журналы](logic-apps-track-b2b-messages-omsportal.md "Дополнительные сведения об отслеживании сообщений B2B в журналах Azure Monitor")

