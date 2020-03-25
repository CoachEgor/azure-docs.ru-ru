---
title: Примеры Azure PowerShell для Azure Cosmos DB. API MongoDB
description: Получение примеров Azure PowerShell для выполнения разных типичных задач в API Azure Cosmos DB для MongoDB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: ca93b4fa85f5ef3ab87d108242e9cbfbd12036ab
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "77521869"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-mongodb-api"></a>Примеры Azure PowerShell для Azure Cosmos DB с API MongoDB

В приведенной ниже таблице содержатся ссылки на примеры скриптов Azure PowerShell для Azure Cosmos DB для API MongoDB.

> [!NOTE]
> Сейчас можно создать только версию 3,2 (то есть ту, в которой для учетных записей используется конечная точка в формате `*.documents.azure.com`) приложения Azure Cosmos DB для учетных записей MongoDB с помощью PowerShell, интерфейса командной строки и шаблонов диспетчера ресурсов. Чтобы создать версию учетной записи 3.6, используйте портал Azure.

| | |
|---|---|
|[Создание учетной записи, базы данных и коллекции](scripts/powershell/mongodb/ps-mongodb-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Создание учетной записи Azure Cosmos, базы данных и коллекции. |
|[Получение списка либо возврат баз данных или коллекций](scripts/powershell/mongodb/ps-mongodb-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Получение списка либо возврат баз данных или коллекций. |
|[Получение показателя ЕЗ/с](scripts/powershell/mongodb/ps-mongodb-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Получение показателя ЕЗ/с для базы данных или коллекции. |
|[Обновление ЕЗ/с](scripts/powershell/mongodb/ps-mongodb-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Обновление ЕЗ/с для базы данных или коллекции. |
|[Обновление учетной записи или добавление региона](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Добавляет регион в учетную запись Cosmos. Также может использоваться для изменения других свойств учетной записи, но они должны вноситься отдельно от изменений регионов. |
|[Изменение приоритета отработки отказа или запуск отработки отказа](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Изменяет приоритет региональной отработки отказа для учетной записи Azure Cosmos или позволяет вручную запустить отработку отказа. |
|[Ключи или строки подключения учетной записи](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Получает первичный и вторичный ключи, строки подключения или воссоздает ключ учетной записи для учетной записи Azure Cosmos. |
|[Создание учетной записи Cosmos с правилами брандмауэра для IP-адресов](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Создает учетную запись Azure Cosmos с активными правилами брандмауэра для IP-адресов. |
|||
