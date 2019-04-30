---
title: Удаление инструмента заданий обработки эластичных баз данных
description: Узнайте, как удалять компоненты заданий обработки эластичных баз данных с помощью портала Azure или PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 12f923724616378a6ea6c83a947bd5362840a697
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61435413"
---
# <a name="uninstall-elastic-database-jobs-components"></a>Удаление компонентов заданий обработки эластичных баз данных.


[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]


**Задания обработки эластичных баз данных** можно удалить с помощью портала Azure или PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Удаление компонентов заданий обработки эластичных баз данных с помощью портала Azure
1. Откройте [портал Azure](https://portal.azure.com/).
2. Перейдите к подписке, которая содержит компоненты **заданий обработки эластичных баз данных** , то есть подписку, в которой они были установлены.
3. Щелкните **Обзор** и **Группы ресурсов**.
4. Выберите группу ресурсов с именем __ElasticDatabaseJob.
5. Удалите ее.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Удаление компонентов заданий обработки эластичных баз данных с помощью PowerShell
1. Откройте командную строку Microsoft Azure PowerShell, перейдите в подкаталог \tools в папке Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x и введите команду **cd \tools**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools
2. Запустите сценарий PowerShell .\UninstallElasticDatabaseJobs.ps1.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1

Или просто запустите следующий сценарий, если для установки компонентов использовались значения по умолчанию:

```powershell
$ResourceGroupName = "__ElasticDatabaseJob"
Switch-AzureMode AzureResourceManager

$resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
if(!$resourceGroup)
{
     Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
     return
}

Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
Remove-AzureResourceGroup -Name $ResourceGroupName -Force
Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job components are now uninstalled."
```

## <a name="next-steps"></a>Дальнейшие действия
Сведения о повторной установке службы можно найти в статье [Установка компонентов службы заданий обработки эластичных баз данных](sql-database-elastic-jobs-service-installation.md)

Общие сведения о заданиях обработки эластичных баз данных см. в статье [Управление масштабируемыми облачными базами данных](sql-database-elastic-jobs-overview.md).
