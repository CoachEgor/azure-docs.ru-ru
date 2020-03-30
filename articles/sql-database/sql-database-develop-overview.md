---
title: Обзор разработки приложений
description: Сведения о доступных библиотеках подключения и рекомендации для приложений, подключающихся к базе данных SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
ms.date: 11/14/2019
ms.openlocfilehash: 741906bbe9de68459b2e4a704a243fde4771b3a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067329"
---
# <a name="sql-database-application-development-overview"></a>Обзор разработки приложений базы данных SQL

В этой статье рассматриваются основные вопросы, которые разработчик должен учитывать при программировании подключения к базе данных SQL Azure. Эта статья относится ко всем моделям развертывания Баз данных SQL Azure (отдельная база данных, пул эластичных баз данных и управляемый экземпляр).

> [!TIP]
> Если необходимо настроить базу данных SQL Azure, ознакомьтесь с руководствами [Начало работы с отдельной базой данных в Базе данных SQL Azure](sql-database-single-database-quickstart-guide.md) и [Начало работы с управляемыми экземплярами Базы данных SQL Azure](sql-database-managed-instance-quickstart-guide.md).
>

## <a name="language-and-platform"></a>Язык и платформа

Для подключения и отправки запросов к базе данных SQL Azure подходят различные [языки и платформы](sql-database-connect-query.md). Вы можете найти [примеры приложений](https://azure.microsoft.com/resources/samples/?service=sql-database&sort=0), которые можно использовать для подключения к базе данных SQL Azure.

Вы можете использовать инструменты с открытым кодом, такие как [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) и [VS Code](https://code.visualstudio.com/). Кроме того, база данных SQL Azure поддерживает инструменты Майкрософт, например [Visual Studio](https://www.visualstudio.com/downloads/) и [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Кроме того, портал Azure, PowerShell и интерфейсы REST API дополнительно упростят вам работу.

## <a name="authentication"></a>Проверка подлинности

Доступ к Базе данных SQL Azure защищен с помощью механизмов входа в систему и брандмауэров. База данных SQL Azure поддерживает пользователей и имена для входа при аутентификации с помощью SQL Server и [Azure Active Directory](sql-database-aad-authentication.md). Имена входа Azure Active Directory доступны только в Управляемом экземпляре. 

Ознакомьтесь с дополнительными сведения об [управлении именами для входа и доступом к базам данных](sql-database-manage-logins.md).

## <a name="connections"></a>Соединения

В логике подключения к клиенту задайте для времени ожидания по умолчанию 30 секунд. Установленных изначально 15 секунд недостаточно, если подключение зависит от Интернета.

Если вы используете [пул подключений](https://msdn.microsoft.com/library/8xx3tyca.aspx), не забудьте закрыть экземпляр подключения, который ваша программа не использует активно и который не предполагается использовать повторно.

Избегайте длительных транзакций, потому что при любом сбое инфраструктуры или подключения может произойти откат транзакции. Если возможно, разделите транзакцию на несколько меньших транзакций и используйте [пакетную обработку, чтобы повысить производительность](sql-database-use-batching-to-improve-performance.md).

## <a name="resiliency"></a>Устойчивость

База данных SQL Azure — это облачная служба, где в базовой инфраструктуре или при обмене данными между облачными объектами могут происходить временные ошибки. Хотя База данных SQL Azure устойчива к транзитивным сбоям инфраструктуры, они могут повлиять на ваше подключение. Ваш код должен предусматривать возможность [повторного вызова](sql-database-connectivity-issues.md), если при подключении к Базе данных SQL возникает временная ошибка. В коде повторного вызова мы рекомендуем применять логику отсрочки, которая защищает базу данных SQL от перегрузки из-за одновременных повторных вызовов от нескольких клиентов. Логика повторных попыток зависит от [сообщений об ошибках для клиентских программ Базы данных SQL](troubleshoot-connectivity-issues-microsoft-azure-sql-database.md).

Дополнительные сведения о подготовке к запланированным событиям по обслуживанию базы данных SQL Azure см. в статье [Планирование событий обслуживания Azure в Базе данных SQL Azure](sql-database-planned-maintenance.md).

## <a name="network-considerations"></a>Рекомендации относительно сети

- На компьютере с вашей клиентской программой убедитесь, что брандмауэр разрешает исходящие TCP-соединения через порт 1433.  Дополнительные сведения: [Настройка брандмауэра Базы данных SQL Azure](sql-database-configure-firewall-settings.md).
- Если клиентская программа подключается к базе данных SQL, а клиент работает на виртуальной машине Azure, необходимо открыть на ней определенные диапазоны портов. Дополнительная информация: [Порты за 1433 для ADO.NET 4.5 и база данных S'L](sql-database-develop-direct-route-ports-adonet-v12.md).
- Клиентские подключения к базе данных SQL Azure иногда обходят прокси-сервер и взаимодействуют непосредственно с базой данных. Порты, отличные от 1433, становятся важными. Дополнительные сведения см. в статьях [Архитектура подключений к базе данных SQL Azure](sql-database-connectivity-architecture.md) и [Порты для ADO.NET 4.5, отличные от порта 1433](sql-database-develop-direct-route-ports-adonet-v12.md).
- Для конфигурации сетей для управляемого экземпляра см. [конфигурацию сети для управляемых экземпляров.](sql-database-howto-managed-instance.md#network-configuration)

## <a name="next-steps"></a>Дальнейшие действия

Исследуйте все [возможности базы данных S'L](sql-database-technical-overview.md).
