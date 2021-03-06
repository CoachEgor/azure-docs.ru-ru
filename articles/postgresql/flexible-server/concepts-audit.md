---
title: Ведение журнала аудита — база данных Azure для PostgreSQL — гибкий сервер
description: Основные понятия для ведения журнала аудита Пгаудит в базе данных Azure для PostgreSQL-гибкого сервера.
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: cf749f3aef10a0c67814722577f79906f447ffdb
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2020
ms.locfileid: "92532787"
---
# <a name="audit-logging-in-azure-database-for-postgresql---flexible-server"></a>Ведение журнала аудита в базе данных Azure для PostgreSQL — гибкого сервера

Ведение журнала аудита действий базы данных в базе данных Azure для PostgreSQL — гибкий сервер доступен через расширение аудита PostgreSQL: [пгаудит](https://www.pgaudit.org/). Пгаудит предоставляет подробные сведения о сеансе и (или) ведении журнала аудита объектов.

> [!IMPORTANT]
> База данных Azure для PostgreSQL — гибкий сервер в предварительной версии

Если вы хотите, чтобы журналы уровня ресурсов Azure для таких операций, как вычисление и масштабирование хранилища, см. в [журнале действий Azure](../../azure-monitor/platform/platform-logs-overview.md).

## <a name="usage-considerations"></a>Особенности использования
По умолчанию выписки из журналов pgAudit создаются вместе с регулярными выписками с использованием стандартного средства ведения журнала Postgres. В базе данных Azure для PostgreSQL-гибкого сервера можно настроить отправку всех журналов в Azure Monitor хранилище журналов для последующей аналитики в Log Analytics. Если вы включаете Azure Monitor ведение журнала ресурсов, журналы автоматически отправляются (в формате JSON) в хранилище Azure, концентраторы событий и (или) Azure Monitor журналы в зависимости от вашего выбора.

Чтобы узнать, как настроить ведение журнала для службы хранилища Azure, концентраторов событий или журналов Azure Monitor, посетите раздел Журналы ресурсов в [статье журналы сервера](concepts-logging.md).

## <a name="enabling-pgaudit"></a>Включение Пгаудит

Чтобы включить Пгаудит, необходимо подключиться к серверу с помощью клиента (например, psql) и включить расширение Пгаудит, выполнив следующую команду:
```SQL
CREATE EXTENSION pgaudit;
```

## <a name="pgaudit-settings"></a>Параметры Пгаудит

Пгаудит позволяет настроить ведение журнала аудита для сеанса или объекта. [Ведение журнала аудита сеансов](https://github.com/pgaudit/pgaudit/blob/master/README.md#session-audit-logging) создает подробные журналы выполненных инструкций. [Ведение журнала аудита объектов](https://github.com/pgaudit/pgaudit/blob/master/README.md#object-audit-logging) — это область аудита для конкретных связей. Можно выбрать один или оба типа ведения журнала. 

> [!NOTE]
> Параметры Пгаудит задаются глобально и не могут быть указаны на уровне базы данных или роли.

После [включения пгаудит](#enabling-pgaudit)можно настроить его параметры, чтобы начать ведение журнала. В [документации по пгаудит](https://github.com/pgaudit/pgaudit/blob/master/README.md#settings) содержится определение каждого параметра. Сначала проверьте параметры и убедитесь, что вы получаете ожидаемое поведение.

> [!NOTE]
> Если задано значение `pgaudit.log_client` On, журналы перенаправляются в клиентский процесс (например, psql), а не записываются в файл. Этот параметр обычно следует отключать. <br> <br>
> `pgaudit.log_level` включается, только если `pgaudit.log_client` включен.

> [!NOTE]
> В базе данных Azure для PostgreSQL-гибкого сервера `pgaudit.log` не может быть задано с помощью `-` сочетания знака (минус), как описано в документации пгаудит. Все обязательные классы инструкций (чтение, запись и т. д.) должны указываться отдельно.

## <a name="audit-log-format"></a>Формат журналов аудита
Каждая запись аудита обозначается `AUDIT:` рядом с началом строки журнала. Формат остальной части записи подробно описан в [документации по пгаудит](https://github.com/pgaudit/pgaudit/blob/master/README.md#format).

## <a name="getting-started"></a>Начало работы
Чтобы быстро приступить к работе, присвойте параметру значение `pgaudit.log` `WRITE` и откройте журналы сервера для просмотра выходных данных. 

## <a name="viewing-audit-logs"></a>Просмотр журналов аудита
Способ доступа к журналам зависит от выбранной конечной точки. Сведения о службе хранилища Azure см. в статье [учетная запись хранения журналов](../../azure-monitor/platform/resource-logs.md#send-to-azure-storage) . Сведения о концентраторах событий см. в статье [Streaming Azure Logs](../../azure-monitor/platform/resource-logs.md#send-to-azure-event-hubs) .

Для журналов Azure Monitor журналы отправляются в выбранную рабочую область. Журналы postgres используют режим сбора **AzureDiagnostics** , поэтому их можно запрашивать из таблицы AzureDiagnostics. Поля в таблице описаны ниже. Дополнительные сведения о запросах и предупреждениях см. в статье о [запросах Azure Monitor журналов](../../azure-monitor/log-query/log-query-overview.md) .

Этот запрос можно использовать для начала работы. Можно настроить оповещения на основе запросов.

Поиск всех записей Пгаудит в журналах postgres для определенного сервера за последний день
```kusto
AzureDiagnostics
| where LogicalServerName_s == "myservername"
| where TimeGenerated > ago(1d) 
| where Message contains "AUDIT:"
```

## <a name="next-steps"></a>Дальнейшие действия
- [Сведения о ведении журнала в базе данных Azure для PostgreSQL — гибком сервере](concepts-logging.md)
- [Узнайте, как настроить ведение журнала в базе данных Azure для PostgreSQL-гибкого сервера и получить доступ к журналам.](howto-configure-and-access-logs.md)