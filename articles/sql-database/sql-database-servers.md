---
title: Серверы Базы данных SQL Azure | Документация Майкрософт
description: Статья содержит сведения о серверах Базы данных SQL Azure и управлении ими.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 21d4e7bd3bf7453d2c770034728aedfdaa5ab85e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/16/2019
ms.locfileid: "65790171"
---
# <a name="azure-sql-database-servers-and-their-management"></a>Серверы Базы данных SQL Azure и управление ими

## <a name="what-is-an-azure-sql-database-server"></a>Что являет собою сервер Базы данных SQL Azure?

Сервер базы данных SQL — это логическая конструкция, которая выступает в качестве точки центра администрирования нескольких отдельных баз данных или баз данных в составе [пула](sql-database-elastic-pool.md), [имен для входа](sql-database-manage-logins.md), [правил брандмауэра](sql-database-firewall-configure.md), [правил аудита](sql-database-auditing.md), [политик обнаружения угроз](sql-database-threat-detection.md) и [групп отработки отказа](sql-database-auto-failover-group.md). Он может не располагаться в том же регионе, что и группа ресурсов. Сервер Базы данных SQL должен существовать еще до создания базы данных SQL Azure. Все базы данных, управляемые сервером Базы данных SQL, создаются в том же регионе, что и сервер.

Сервер Базы данных SQL отличается от экземпляра SQL Server, с которым вы могли работать в локальной среде. В частности, служба Базы данных SQL не дает никаких гарантий, связанных с расположением баз данных относительно сервера, который управляет ими, а также не предоставляет доступ или возможности на уровне экземпляра. В противоположность этому базы данных экземпляра в управляемом экземпляре размещены совместно таким же образом, что и SQL Server в локальной среде.

При создании сервера Базы данных SQL следует указать учетные данные и пароль входа на сервер, имеющие права администратора для базы данных master на этом сервере и всех баз данных, созданных на этом сервере. Эта начальная учетная запись является учетной записью входа SQL. База данных SQL Azure поддерживает аутентификацию SQL и аутентификацию Azure Active Directory. Дополнительную информацию об именах для входа и аутентификации можно найти в разделе [Предоставление доступа к базе данных и управление им](sql-database-manage-logins.md). Проверка подлинности Windows не поддерживается.

Сервер Базы данных SQL.

- Создается в рамках подписки Azure, но сам логический сервер и все его ресурсы можно переместить в другую подписку.
- Выступает в качестве родительского ресурса для баз данных, эластичных пулов и хранилищ данных.
- Предоставляет пространство имен для баз данных, эластичных пулов и хранилищ данных.
- Представляет собой логический контейнер со строгой семантикой времени существования (при удалении сервера удаляются также все расположенные на нем базы данных, эластичные пулы и хранилища данных).
- Участвует в [управлении доступом на основе ролей Azure](/azure/role-based-access-control/overview) (базы данных, эластичные пулы и хранилища данных, содержащиеся на сервере, наследуют его права доступа).
- Выступает в качестве элемента высокого приоритета при идентификации баз данных, эластичных пулов и хранилищ данных для управления ресурсами Azure (см. схему URL-адресов для баз данных и пулов).
- Выравнивает ресурсы в регионе.
- Предоставляет конечную точку подключения к базе данных (`<serverName>`.database.windows.net).
- Предоставляет доступ к метаданным ресурсов через динамические административные представления путем подключения к базе данных master.
- Обеспечивает область для политик управления, применяемых к базам данных: имена для входа, брандмауэр, аудит, обнаружение угроз и т. д.
- Ограничен квотой в рамках родительской подписки (по умолчанию шесть серверов на подписку). Ограничения подписки см. [здесь](../azure-subscription-service-limits.md).
- Обеспечивает область для квоты базы данных и квоты DTU или виртуальных ядер для ресурсов, содержащихся на нем (например, 45 000 DTU).
- Выступает в качестве области управления версиями для компонентов, включенных для его ресурсов.
- За счет возможностей входа в систему с использованием субъекта серверного уровня может управлять всеми базами данных на сервере.
- Может содержать имена входов, подобные именам входов экземпляров SQL Server в локальной среде с доступом к одной или нескольким базам данных на сервере. Можно предоставлять ограниченные права администратора. Дополнительные сведения см. в статье [Проверка подлинности и авторизация в базе данных SQL: предоставление доступа](sql-database-manage-logins.md).
- Параметры сортировки по умолчанию для всех баз данных, созданных на сервере Базы данных SQL, имеют значение `SQL_LATIN1_GENERAL_CP1_CI_AS`, где `LATIN1_GENERAL` означает английский язык (США) `CP1` — кодовую страницу 1252, `CI` означает, что учитывается регистр, а `AS` — что учитываются диакритические знаки.

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Управление серверами SQL Azure, базами данных и брандмауэрами с помощью портала Azure

Группу ресурсов базы данных SQL Azure можно создать заблаговременно или при создании самого сервера. Существует несколько методов перехода к форме создания сервера SQL Server. Можно создать сервер SQL Server отдельно или при создании базы данных.

### <a name="create-a-blank-sql-database-server"></a>Создание пустого сервера Базы данных SQL

Чтобы создать сервер базы данных SQL Azure (без базы данных) с помощью [портала Azure](https://portal.azure.com), перейдите к пустой форме "SQL Server (логический сервер)".  

### <a name="create-a-blank-or-sample-sql-database"></a>Создание пустой базы данных SQL или примера базы данных SQL

Чтобы создать базу данных SQL Azure с помощью [портала Azure](https://portal.azure.com), перейдите к пустой форме "База данных SQL" и введите необходимую информацию. Группу ресурсов базы данных SQL Azure и сервер Базы данных SQL можно создать заблаговременно или при создании самой базы данных. Можно создать пустую базу данных или пример базы данных на основе Adventure Works LT.

  ![Создание базы данных — 1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Сведения о выборе ценовой категории для базы данных приведены в статьях о моделях приобретения на основе [единиц DTU](sql-database-service-tiers-dtu.md) и [виртуальных ядер](sql-database-service-tiers-vcore.md).

Чтобы создать управляемый экземпляр, см. статью [Краткое руководство. Создание Управляемого экземпляра Базы данных SQL Azure](sql-database-managed-instance-get-started.md).

### <a name="manage-an-existing-sql-server"></a>Управление существующим сервером SQL Server

Для управления существующим сервером перейдите к нему, воспользовавшись одним из множества способов. Например, это можно сделать со страницы определенной базы данных SQL, страницы **Серверы SQL Server** или страницы **Все ресурсы**.

Для управления существующей базой данных перейдите на страницу **Базы данных SQL** и щелкните нужную базу данных. На следующем снимке экрана показано, как можно начать настройку брандмауэра уровня сервера для базы данных на странице **Обзор** для базы данных.

   ![правило брандмауэра для сервера](./media/sql-database-get-started-portal/server-firewall-rule.png)

> [!IMPORTANT]
> Сведения о настройке свойств базы данных приведены в статьях о моделях приобретения на основе [единиц DTU](sql-database-service-tiers-dtu.md) и [виртуальных ядер](sql-database-service-tiers-vcore.md).
> [!TIP]
> Краткое руководство по работе с порталом Azure приведено в статье о [создании базы данных SQL Azure на портале Azure](sql-database-single-database-get-started.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Управление серверами SQL Azure, базами данных и брандмауэрами с помощью PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Модуль PowerShell Azure Resource Manager по-прежнему поддерживается базой данных SQL Azure, но все будущие разработки — для модуля Az.Sql. Для этих командлетов см. в разделе [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Аргументы для команд в модуле Az и в модуле AzureRm практически идентичны.

Для создания сервера Azure SQL, баз данных и брандмауэров и управления ими с помощью Azure PowerShell используйте приведенные ниже командлеты PowerShell. Если вам нужно выполнить установку или обновление PowerShell, см. статью [об установке модуля Azure PowerShell](/powershell/azure/install-az-ps). Изучите дополнительные сведения о создании [эластичных пулов](sql-database-elastic-pool.md) и управлении ими.

| Командлет | ОПИСАНИЕ |
| --- | --- |
|[New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase)|Создает базу данных. |
|[Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase)|Получает одну или несколько баз данных.|
|[Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)|Определяет свойства базы данных или перемещает ее в эластичный пул.|
|[Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase)|Удаляет базу данных.|
|[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)|Создает группу ресурсов.|
|[New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver)|Создает сервер.|
|[Get-AzSqlServer](/powershell/module/az.sql/get-azsqlserver)|Возвращает сведения о серверах.|
|[Set-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlserver)|Изменяет свойства сервера.|
|[Remove-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver)|Удаляет сервера.|
|[New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule)|Создает правило брандмауэра уровня сервера. |
|[Get-AzSqlServerFirewallRule](/powershell/module/az.sql/get-azsqlserverfirewallrule)|Получает правила брандмауэра для сервера.|
|[Set-AzSqlServerFirewallRule](/powershell/module/az.sql/set-azsqlserverfirewallrule)|Изменяет правило брандмауэра на сервере.|
|[Remove-AzSqlServerFirewallRule](/powershell/module/az.sql/remove-azsqlserverfirewallrule)|Удаляет правило брандмауэра с сервера.|
| Новый AzSqlServerVirtualNetworkRule | Создает [*правило виртуальной сети*](sql-database-vnet-service-endpoint-rule-overview.md) на основе подсети, которая является конечной точкой службы для виртуальной сети. |

> [!TIP]
> Краткое руководство по PowerShell приведено в статье [Краткое руководство. Создание базы данных SQL Azure на портале Azure](sql-database-single-database-get-started.md). Примеры скриптов PowerShell см. в статьях [Создание отдельной базы данных SQL и настройка правила брандмауэра с помощью PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) и [Мониторинг и масштабирование отдельной базы данных SQL с помощью PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Управление серверами SQL Azure, базами данных и брандмауэрами с помощью Azure CLI

Для создания сервера Azure SQL, баз данных и брандмауэров и управления ими с помощью [Azure CLI](/cli/azure) используйте приведенные ниже команды [Azure CLI для Базы данных SQL](/cli/azure/sql/db). Запускайте интерфейс командной строки в браузере с помощью [Cloud Shell](/azure/cloud-shell/overview) либо [установите](/cli/azure/install-azure-cli) его на платформе macOS, Linux или Windows. Изучите дополнительные сведения о создании [эластичных пулов](sql-database-elastic-pool.md) и управлении ими.

| Командлет | ОПИСАНИЕ |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az-sql-db-create) |Создает базу данных.|
|[az sql db list](/cli/azure/sql/db#az-sql-db-list)|Выводит список всех баз данных и хранилищ данных на сервере или всех баз данных в эластичном пуле.|
|[az sql db list-editions](/cli/azure/sql/db#az-sql-db-list-editions)|Выводит список доступных целевых служб и ограничений хранилища.|
|[az sql db list-usages](/cli/azure/sql/db#az-sql-db-list-usages)|Возвращает данные об использовании баз данных.|
|[az sql db show](/cli/azure/sql/db#az-sql-db-show)|Возвращает сведения о базе данных или хранилище данных.|
|[az sql db update](/cli/azure/sql/db#az-sql-db-update)|Обновляет базу данных.|
|[az sql db delete](/cli/azure/sql/db#az-sql-db-delete)|Удаляет базу данных.|
|[az group create](/cli/azure/group#az-group-create)|Создает группу ресурсов.|
|[az sql server create](/cli/azure/sql/server#az-sql-server-create)|Создает сервер.|
|[az sql server list](/cli/azure/sql/server#az-sql-server-list)|Выводит список серверов.|
|[az sql server list-usages](/cli/azure/sql/server#az-sql-server-list-usages)|Возвращает данные об использовании серверов.|
|[az sql server show](/cli/azure/sql/server#az-sql-server-show)|Возвращает сервер.|
|[az sql server update](/cli/azure/sql/server#az-sql-server-update)|Обновляет сервер.|
|[az sql server delete](/cli/azure/sql/server#az-sql-server-delete)|Удаляет сервер.|
|[az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Создает правило брандмауэра для сервера.|
|[az sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Выводит список правил брандмауэра на сервере.|
|[az sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Отображает сведения о правиле брандмауэра.|
|[az sql server firewall-rule update](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Обновляет правило брандмауэра.|
|[az sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Удаляет правило брандмауэра.|

> [!TIP]
> Краткое руководство по Azure CLI приведено в статье [Примеры Azure CLI для базы данных SQL Azure](sql-database-cli-samples.md). Образцы скриптов Azure CLI см. в статьях [Создание отдельной базы данных SQL и настройка правила брандмауэра с помощью интерфейса командной строки](scripts/sql-database-create-and-configure-database-cli.md) и [Мониторинг и масштабирование отдельной базы данных SQL Azure с помощью интерфейса командной строки](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Управление серверами SQL Azure, базами данных и брандмауэрами с помощью Transact-SQL

Для создания сервера Azure SQL, баз данных и брандмауэров и управления ими с помощью Transact-SQL используйте приведенные ниже команды T-SQL. Можно выполнить эти команды на портале Azure, в [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs) или любой другой программе, которая может подключаться к серверу Базы данных SQL Azure и передавать команды Transact-SQL. Ознакомьтесь с дополнительными сведениями об управлении [эластичными пулами](sql-database-elastic-pool.md).

> [!IMPORTANT]
> С помощью Transact-SQL невозможно создать или удалить сервер.
>

| Команда | ОПИСАНИЕ |
| --- | --- |
|[CREATE DATABASE (база данных SQL Azure)](/sql/t-sql/statements/create-database-azure-sql-database)|Создает базу данных. Для создания базы данных требуется подключение к базе данных master.|
| [ALTER DATABASE (база данных SQL Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Изменяет базу данных SQL Azure. |
|[ALTER DATABASE (хранилище данных SQL Azure)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Изменяет хранилище данных SQL Azure.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Удаляет базу данных.|
|[sys.database_service_objectives (база данных SQL Azure)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Возвращает сведения о выпуске (уровень служб), целевой службе (ценовую категорию), а также имя эластичного пула (при наличии) для базы данных SQL Azure или хранилища данных SQL Azure. В системе базы данных master на сервере Базы данных SQL Azure возвращает сведения обо всех базах данных. Для использования хранилища данных SQL Azure необходимо подключиться к базе данных master.|
|[sys.dm_db_resource_stats (база данных SQL Azure)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Возвращает сведения об использовании ЦП, операциях ввода-вывода и потреблении памяти для базы данных в службе "База данных SQL Azure". Новая строка создается каждые 15 секунд, даже если в базе данных не выполняется никаких действий.|
|[sys.resource_stats (база данных SQL Azure)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Возвращает сведения об использовании ЦП и хранилища для базы данных SQL Azure. Данные собираются и объединяются за пятиминутные интервалы.|
|[sys.database_connection_stats (база данных SQL Azure)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Содержит статистику о событиях подключения к базе данных в базе данных SQL, предоставляя общие сведения об успешных и неудачных попытках подключения. |
|[sys.event_log (база данных SQL Azure)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Возвращает сведения об успешных подключениях к базе данных в базе данных SQL Azure, а также о сбоях подключения и взаимоблокировках. С помощью этой информации можно отслеживать действия с базой данных в базе данных SQL или устранять возникшие неполадки.|
|[sp_set_firewall_rule (база данных SQL Azure)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Создает или обновляет параметры брандмауэра уровня сервера для сервера Базы данных SQL. Эта хранимая процедура доступна только в базе данных master для имени для входа субъекта серверного уровня. Правило брандмауэра уровня сервера можно будет создать с помощью Transact-SQL только после того, как пользователь, имеющий разрешения уровня Azure, создаст первое правило брандмауэра уровня сервера.|
|[sys.firewall_rules (база данных SQL Azure)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Возвращает сведения о параметрах брандмауэра уровня сервера, связанных с базой данных SQL Microsoft Azure.|
|[sp_delete_firewall_rule (база данных SQL Azure)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Удаляет параметры брандмауэра уровня сервера с сервера Базы данных SQL. Эта хранимая процедура доступна только в базе данных master для имени для входа субъекта серверного уровня.|
|[sp_set_database_firewall_rule (база данных SQL Azure)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Создает или обновляет правила брандмауэра уровня базы данных для базы данных SQL Azure или хранилища данных SQL. Можно настроить правила брандмауэра базы данных для базы данных master и пользовательских баз данных в Базе данных SQL. Правила брандмауэра базы данных полезны в тех случаях, когда применяются пользователи автономной базы данных. |
|[sys.database_firewall_rules (база данных SQL Azure)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Возвращает сведения о параметрах брандмауэра уровня базы данных, связанных с базой данных SQL Microsoft Azure. |
|[sp_delete_database_firewall_rule (база данных SQL Azure)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Удаляет параметр брандмауэра уровня базы данных из базы данных SQL Azure или хранилища данных SQL. |

> [!TIP]
> Инструкции по использованию SQL Server Management Studio в Microsoft Windows см. в статье [Краткое руководство. Подключение к базе данных SQL Azure и создание запросов к ней с помощью SQL Server Management Studio](sql-database-connect-query-ssms.md). Инструкции по использованию Visual Studio Code в macOS, Linux и Windows см. в статье [Краткое руководство. Подключение и запрос Базы данных SQL Azure с помощью Visual Studio Code](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>Управление серверами SQL Azure, базами данных и брандмауэрами с помощью REST API

Для создания сервера Azure SQL Server, баз данных и брандмауэров и управления используются приведенные ниже запросы REST API.

| Команда | ОПИСАНИЕ |
| --- | --- |
|[Серверы: создание или обновление](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Создает или обновляет сервер.|
|[Серверы: удаление](https://docs.microsoft.com/rest/api/sql/servers/delete)|Удаляет сервер SQL Server.|
|[Серверы: получение](https://docs.microsoft.com/rest/api/sql/servers/get)|Получает сервер.|
|[Серверы: вывод списка](https://docs.microsoft.com/rest/api/sql/servers/list)|Возвращает список серверов.|
|[Серверы: вывод списка по группе ресурсов](https://docs.microsoft.com/rest/api/sql/servers/listbyresourcegroup)|Возвращает список серверов в группе ресурсов.|
|[Серверы: обновление](https://docs.microsoft.com/rest/api/sql/servers/update)|Обновляет существующий сервер.|
|[Базы данных: создание или обновление](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)|Создает новую базу данных или обновляет имеющуюся.|
|[Базы данных: удаление](https://docs.microsoft.com/rest/api/sql/databases/delete)|Удаляет базу данных.|
|[Базы данных: получение](https://docs.microsoft.com/rest/api/sql/databases/get)|Получает базу данных.|
|[Базы данных: вывод списка по эластичному пулу](https://docs.microsoft.com/rest/api/sql/databases/listbyelasticpool)|Возвращает список баз данных в пуле эластичных баз данных.|
|[Базы данных: вывод списка по серверу](https://docs.microsoft.com/rest/api/sql/databases/listbyserver)|Возвращает список баз данных на сервере.|
|[Базы данных: обновление](https://docs.microsoft.com/rest/api/sql/databases/update)|Обновляет имеющуюся базу данных.|
|[Правила брандмауэра: создание и обновление](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate)|Создает или обновляет правило брандмауэра.|
|[Правила брандмауэра: удаление](https://docs.microsoft.com/rest/api/sql/firewallrules/delete)|Удаляет правило брандмауэра.|
|[Правила брандмауэра: получение](https://docs.microsoft.com/rest/api/sql/firewallrules/get)|Получает правило брандмауэра.|
|[Правила брандмауэра: вывод списка по серверу](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver)|Возвращает список правил брандмауэра.|

## <a name="next-steps"></a>Дальнейшие действия

- Чтобы узнать о переносе базы данных SQL Server в Azure, ознакомьтесь с разделом [Миграция базы данных SQL Server в базу данных SQL в облаке](sql-database-single-database-migrate.md).
- Дополнительные сведения о поддерживаемых функциях см. [здесь](sql-database-features.md).
