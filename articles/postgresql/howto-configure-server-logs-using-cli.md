---
title: Управление журналами - Azure CLI - База данных Azure для PostgreS'L - Единый сервер
description: В этой статье описывается, как настроить и получить доступ к журналам сервера (.log файлы) в базе данных Azure для PostgreS-L - Единый сервер с помощью Azure CLI.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: be679be91d49516bd2f6c672eb53640cfad2ae2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "74763578"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Настройка журналов сервера и получение к ним доступа с помощью Azure CLI
Вы можете скачать журналы ошибок сервера PostgreSQL с помощью интерфейса командной строки (Azure CLI). Но доступ к журналам транзакций не поддерживается. 

## <a name="prerequisites"></a>Предварительные требования
Прежде чем приступить к выполнению этого руководства, необходимы следующие компоненты:
- [База данных Azure для сервера PostgreS'L](quickstart-create-server-database-azure-cli.md)
- Программа командной строки [Azure CLI 2.0](/cli/azure/install-azure-cli) или Azure Cloud Shell в браузере

## <a name="configure-logging"></a>Настройка журнала
Можно настроить на сервере доступ к журналам запросов и журналам ошибок. Журналы ошибок могут содержать сведения об автоматической очистке, подключениях и контрольных точках.
1. Включите ведение журнала.
2. Чтобы включить ведение журнала запросов, обновите **log\_statement** и **log\_min\_duration\_statement**.
3. Обновите срок хранения.

Дополнительные сведения см. в статье [Настройка параметров конфигурации сервера с помощью Azure CLI](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs"></a>Списки журналов
Чтобы получить список доступных файлов журналов для сервера, выполните команду [az postgres server-logs list](/cli/azure/postgres/server-logs).

Можно отобразить список файлов журнала для сервера **mydemoserver.postgres.database.azure.com** в группе ресурсов **myresourcegroup**. Затем направьте список файлов журнала в текстовый файл с именем **log\_files\_list.txt**.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Скачивание журналов с сервера в локальную среду
Команда [az postgres server-logs download](/cli/azure/postgres/server-logs) позволяет скачать отдельные файлы журналов для сервера. 

В следующем примере в локальную среду скачивается определенный файл журнала для сервера **mydemoserver.postgres.database.azure.com** в группе ресурсов **myresourcegroup**.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Дальнейшие действия
- См. дополнительные сведения о [журналах сервера в базе данных Azure для PostgreSQL](concepts-server-logs.md).
- См. дополнительные сведения о [настройке параметров конфигурации сервера с помощью Azure CLI](howto-configure-server-parameters-using-cli.md).
