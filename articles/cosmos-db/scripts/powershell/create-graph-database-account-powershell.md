---
title: Сценарий Azure PowerShell для создания учетной записи API Gremlin для Azure Cosmos DB
description: Пример сценария Azure PowerShell для создания учетной записи API Gremlin для Azure Cosmos DB.
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.subservice: cosmosdb-graph
ms.devlang: PowerShell
ms.topic: sample
ms.date: 05/29/2019
ms.reviewer: sngun
ms.openlocfilehash: c53657995977164af895d2a05daa752ba14e2e00
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2019
ms.locfileid: "66474888"
---
# <a name="azure-cosmos-db-create-a-gremlin-api-account-using-powershell"></a>Azure Cosmos DB — Создание учетной записи API Gremlin с помощью PowerShell

В этом примере скрипта PowerShell создается учетная запись API Gremlin для Azure Cosmos DB. 

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../../powershell_scripts/cosmosdb/create-and-configure-graph-database/create-and-configure-graph-database.ps1 "Create an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a>Очистка развертывания

После выполнения примера сценария можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | Создает логический сервер, на котором размещена база данных или эластичный пул. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Удаляет группу ресурсов со всеми вложенными ресурсами. |
|||

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о Azure PowerShell см. в [документации по Azure PowerShell](https://docs.microsoft.com/powershell/).

Дополнительные примеры скриптов PowerShell для базы данных Azure Cosmos DB можно найти [здесь](../../powershell-samples.md).
