---
title: Пример для CLI. Создание базы данных SQL Azure | Документация Майкрософт
description: Используйте этот пример сценария Azure CLI для создания базы данных SQL.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
manager: craigg
ms.author: sstein
ms.reviewer: ''
ms.date: 06/25/2019
ms.openlocfilehash: 2a5fb6fcd9b1f0c327df23741284ec0d5fc26c71
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447874"
---
# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>Создание отдельной базы данных SQL и настройка правила брандмауэра с помощью интерфейса командной строки

Этот пример сценария Azure CLI создает базу данных SQL Azure и настраивает правило брандмауэра уровня сервера. После успешного выполнения скрипта доступ к базе данных SQL можно получить из всех служб Azure, а также по настроенному IP-адресу.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh "Create SQL Database")]

## <a name="clean-up-deployment"></a>Очистка развертывания

Используйте следующую команду, чтобы удалить группу ресурсов и все связанные с ней ресурсы.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Создает сервер Базы данных SQL, на котором размещена отдельная база данных или пул эластичных баз данных. |
| [az sql server firewall create](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create) | Создает правило брандмауэра, чтобы разрешить доступ из введенного диапазона IP-адресов ко всем отдельным базам данных и эластичным пулам на сервере Базы данных SQL. |
| [az sql db create](/cli/azure/sql/db#az-sql-db-create) | Создает отдельную базу данных или пул эластичных баз данных. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры сценариев интерфейса командной строки для Базы данных SQL Azure см. в [документации по Базе данных SQL](../sql-database-cli-samples.md).
