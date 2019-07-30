---
title: Примеры сценариев Azure CLI для базы данных SQL | Документация Майкрософт
description: Примеры сценариев Azure CLI для создания серверов, эластичных пулов, баз данных, а также брандмауэров Базы данных SQL Azure и управления ими.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 02/03/2019
ms.openlocfilehash: ad4706ce038feb316d238c1cabc12b27621c8085
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443328"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Примеры Azure CLI для Базы данных SQL Azure

Базу данных SQL Azure можно настроить с помощью <a href="/cli/azure">Azure CLI</a>.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI]( /cli/azure/install-azure-cli).

## <a name="single-database--elastic-pools"></a>Отдельная база данных и эластичные пулы

В следующей таблице содержатся ссылки на примеры сценариев Azure CLI для Базы данных SQL Azure.

| |  |
|---|---|
|**Создание отдельной базы данных и эластичного пула**||
| [Создание отдельной базы данных и настройка правила брандмауэра](scripts/sql-database-create-and-configure-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Этот сценарий Azure CLI создает отдельную базу данных SQL Azure и настраивает правило брандмауэра на уровне сервера. |
| [Создание эластичных пулов и перемещение баз данных в пуле](scripts/sql-database-move-database-between-pools-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Этот сценарий Azure CLI создает эластичные пулы SQL и перемещает базы данных SQL Azure в составе пулов, а также изменяет объемы вычислительных ресурсов.|
|**Масштабирование отдельной базы данных и эластичного пула**||
| [Масштабирование отдельной базы данных](scripts/sql-database-monitor-and-scale-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Этот пример сценария интерфейса командной строки масштабирует отдельную базу данных SQL Azure до другого объема вычислительных ресурсов после запроса на получение сведений о размере базы данных. |
| [Масштабирование эластичного пула](scripts/sql-database-scale-pool-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Этот пример сценария интерфейса командной строки масштабирует эластичный пул SQL, изменяя объем его вычислительных ресурсов.  |
|**Группы отработки отказа**||
| [Добавление отдельных баз данных в группу отработки отказа](scripts/sql-database-add-single-db-to-failover-group-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Этот скрипт CLI создает базу данных и группу отработки отказа, добавляет базу данных в эту группу и тестирует отработку отказа на сервер-получатель.|
|||

См. дополнительные сведения об [API управления отдельной базой данных с помощью Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases).

## <a name="managed-instance"></a>Управляемый экземпляр

В следующей таблице содержатся ссылки на примеры скриптов Azure CLI для Управляемого экземпляра Базы данных SQL Azure.

| |  |
|---|---|
| [Создание Управляемого экземпляра](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/) | В этом скрипте CLI показано, как создать Управляемый экземпляр. |
| [Обновление Управляемого экземпляра](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/) | В этом скрипте CLI показано, как обновить Управляемый экземпляр. |
| [Перемещение базы данных в другой Управляемый экземпляр](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) | В этом скрипте CLI показано, как восстановить резервную копию базы данных из одного экземпляра в другой. |
|||

См. дополнительные сведения об [API управления Управляемым экземпляром с помощью Azure CLI](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances). Дополнительные примеры можно найти [здесь](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).
