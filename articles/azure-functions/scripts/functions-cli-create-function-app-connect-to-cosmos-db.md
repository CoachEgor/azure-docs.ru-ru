---
title: Создание приложения-функции, использующего Azure Cosmos DB, с помощью Azure CLI
description: Пример скрипта Azure CLI для создания функции Azure, которая подключается к Azure Cosmos DB.
ms.topic: sample
ms.date: 07/03/2018
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 440767159ec1321d9b157f53408dbff8f9706eec
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498571"
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Создание функции Azure, которая подключается к Azure Cosmos DB

При помощи этого примера скрипта, написанного с использованием решения "Функции Azure", создается приложение-функция. Затем функция подключается к базе данных Azure Cosmos DB. Параметр созданного приложения с данными подключения можно использовать с [триггером службы Azure Cosmos DB или привязкой к ней](../functions-bindings-cosmosdb.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы используете интерфейс командной строки локально, убедитесь, что у вас установлена версия не ниже Azure CLI 2.0. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Пример скрипта

Этот пример создает приложение-функцию Azure, а также добавляет конечную точку и ключ доступа Cosmos DB в параметры приложения.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Описание скрипта

В этом сценарии используются следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Создайте группу ресурсов с расположением. |
| [az storage accounts create](/cli/azure/storage/account#az-storage-account-create) | Создание учетной записи хранения |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Создает приложение-функцию в бессерверном [плане потребления](../functions-scale.md#consumption-plan). |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Создает базу данных Azure Cosmos DB. |
| [az cosmosdb show](/cli/azure/cosmosdb#az-cosmosdb-show)| Возвращает подключение к учетной записи базы данных. |
| [az cosmosdb list-keys](/cli/azure/cosmosdb#az-cosmosdb-list-keys)| Возвращает ключи для базы данных. |
| [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) | Задает строку подключения в качестве параметра приложения в приложении-функции. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).

Дополнительные примеры сценариев Azure CLI для Функций Azure см. в [документации по Функциям Azure](../functions-cli-samples.md).




