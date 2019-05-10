---
title: 'Управление репликами чтения для базы данных Azure для PostgreSQL: один сервер с помощью Azure CLI'
description: 'Сведения об управлении чтения реплик в базе данных Azure для PostgreSQL: один сервер с помощью Azure CLI.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 9730faf3191ef2e2bd0b6c3caddefa0492b33fc5
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510237"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli"></a>Создание и управление ими чтения реплик с помощью Azure CLI

В этой статье вы узнаете, как создавать и управлять ими чтения реплик в базе данных Azure для PostgreSQL с помощью Azure CLI. Дополнительные сведения о репликах чтения см. в [этой статье](concepts-read-replicas.md).

> [!NOTE]
> Интерфейс командной строки Azure пока не поддерживает создание реплик в другом регионе с главного сервера. Чтобы создать реплику между регионами, используйте [портала Azure](howto-read-replicas-portal.md).

## <a name="prerequisites"></a>Технические условия
- [Сервер службы "База данных Azure для PostgreSQL"](quickstart-create-server-up-azure-cli.md) в качестве главного.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этой статьей вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы узнать, какая установлена версия, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="prepare-the-master-server"></a>Подготовка главного сервера
Приведенные ниже действия позволяют подготовить главный сервер в ценовой категории "Общего назначения" или "Оптимизированная для операций в памяти".

На главном сервере задайте для параметра `azure.replication_support` значение **REPLICA**. При изменении этого параметра static, чтобы изменения вступили в силу требуется перезагрузка сервера.

1. Задайте `azure.replication_support` к РЕПЛИКЕ.

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

2. Перезагрузка для применения изменений на сервер.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

## <a name="create-a-read-replica"></a>Создание реплики чтения

[Az postgres server реплике создайте](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) команды необходимо задать следующие параметры:

| Параметр | Пример значения | Описание  |
| --- | --- | --- |
| resource-group | myresourcegroup |  Группа ресурсов, в которой будут создаваться на сервер-реплику.  |
| name | mydemoserver реплики | Имя нового сервера реплики, который создается. |
| source-server | mydemoserver | Имя или ресурса идентификатор существующего главного сервера для репликации из. |

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Если вы не задали `azure.replication_support` параметр **РЕПЛИКИ** на общего назначения или оптимизированной для памяти главного сервера и перезапустите сервер, то возникнет ошибка. Завершения этих этапов, прежде чем создавать реплики.

Реплика создается с той же конфигурацией сервера, что и у главного сервера. После создания реплики вы можете независимо от главного сервера изменять следующие ее параметры: поколение вычислительных ресурсов, число виртуальных ядер, объем хранилища и период хранения резервных копий. Изменить также можно ценовую категорию (за исключением уровня "Базовый").

> [!IMPORTANT]
> Прежде чем изменить параметры конфигурации на главном сервере, установите на каждой реплике такие же или более высокие значения. Это позволит реплике справляться с нагрузкой, соответствующей новым параметрам главного сервера.

## <a name="list-replicas"></a>Список реплик
Список реплик главного сервера можно просмотреть с помощью [список реплик az postgres server](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list) команды.

```azurecli-interactive
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

## <a name="stop-replication-to-a-replica-server"></a>Остановка репликации на сервер-реплику
Вы можете остановить репликацию между главным и чтения реплик с помощью [az postgres server реплике прекращают](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop) команды.

Остановленную репликацию отменить невозможно. Реплика чтения становится отдельным сервером, который поддерживает операции чтения и записи. Это сервер нельзя снова преобразовать в реплику.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

## <a name="delete-a-master-or-replica-server"></a>Удаление сервера или основная реплика
Чтобы удалить сервер или основная реплика, используйте [az postgres server delete](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-delete) команды.

При удалении главного сервера репликация всех реплик чтения останавливается. Реплики чтения становятся автономными серверами, которые начинают поддерживать операции чтения и записи.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Дальнейшие действия
См. дополнительные сведения о [репликах чтения в Базе данных Azure для PostgreSQL](concepts-read-replicas.md).