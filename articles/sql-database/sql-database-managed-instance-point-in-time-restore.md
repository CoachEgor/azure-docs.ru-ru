---
title: Восстановление управляемого экземпляра на момент времени (PITR)
description: Восстановление базы данных SQL в управляемом экземпляре до предыдущей точки во времени.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, mathoma
ms.date: 08/25/2019
ms.openlocfilehash: 27f465e6864d0ff639e825c8a816d86648bd8853
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79268812"
---
# <a name="restore-a-sql-database-in-a-managed-instance-to-a-previous-point-in-time"></a>Восстановление базы данных SQL в управляемом экземпляре до предыдущей точки во времени

Используйте восстановление до точки во времени (PITR), чтобы создать базу данных в качестве копии другой базы данных за некоторое время в прошлом. В этой статье описывается, как выполнить восстановление базы данных на момент времени в управляемом экземпляре базы данных SQL Azure.

Восстановление до точки во времени полезно в сценариях восстановления, таких как инциденты, вызванные ошибками, неправильно загруженными данными или удалением важных данных. Его также можно использовать для тестирования или аудита. Файлы резервных копий хранятся в течение 7 – 35 дней, в зависимости от параметров базы данных.

Восстановление до точки во времени может восстановить базу данных:

- из существующей базы данных.
- из удаленной базы данных.
- к тому же управляемому экземпляру или другому управляемому экземпляру. 

## <a name="limitations"></a>Ограничения

Восстановление на момент времени в управляемый экземпляр имеет следующие ограничения.

- При восстановлении из одного управляемого экземпляра в другой оба экземпляра должны находиться в одной подписке и регионе. Восстановление между регионами и между подписками в настоящее время не поддерживается.
- Восстановление всего управляемого экземпляра на момент времени невозможно. В этой статье объясняется только то, что возможно: восстановление базы данных, размещенной на управляемом экземпляре, на момент времени.

> [!WARNING]
> Учитывайте размер хранилища управляемого экземпляра. В зависимости от размера восстанавливаемых данных может использоваться хранилище экземпляров. Если недостаточно места для восстанавливаемых данных, используйте другой подход.

В следующей таблице приведены сценарии восстановления на момент времени для управляемых экземпляров.

|           |Восстановление существующей базы данных в том же управляемом экземпляре| Восстановление существующей базы данных в другом управляемом экземпляре|Восстановить удаленную базу данных в том же управляемом экземпляре|Восстановить удаленную базу данных в другом управляемом экземпляре|
|:----------|:----------|:----------|:----------|:----------|
|**Портал Azure**| Да|нет |Да|нет|
|**Azure CLI**|Да |Да |нет|нет|
|**PowerShell**| Да|Да |Да|Да|

## <a name="restore-an-existing-database"></a>Восстановление существующей базы данных

Восстановите существующую базу данных в том же экземпляре с помощью портал Azure, PowerShell или Azure CLI. Чтобы восстановить базу данных на другой экземпляр, используйте PowerShell или Azure CLI, чтобы указать свойства целевого управляемого экземпляра и группы ресурсов. Если эти параметры не заданы, база данных будет восстановлена в существующий экземпляр по умолчанию. Портал Azure в настоящее время не поддерживает восстановление в другой экземпляр.

# <a name="portal"></a>[Портал](#tab/azure-portal)

1. Войдите на [портал Azure](https://portal.azure.com). 
2. Перейдите к управляемому экземпляру и выберите базу данных, которую требуется восстановить.
3. На странице базы данных выберите **восстановить** .

    ![Восстановление базы данных с помощью портал Azure](media/sql-database-managed-instance-point-in-time-restore/restore-database-to-mi.png)

4. На странице **Восстановление** выберите точку для даты и времени, в которую необходимо восстановить базу данных.
5. Нажмите кнопку **подтвердить** , чтобы восстановить базу данных. Это действие запускает процесс восстановления, который создает новую базу данных и заполняет ее данными из исходной базы данных на указанный момент времени. Дополнительные сведения о процессе восстановления см. в разделе [время восстановления](sql-database-recovery-using-backups.md#recovery-time).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Если вы еще не установили Azure PowerShell, см. статью [Установка модуля Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Чтобы восстановить базу данных с помощью PowerShell, укажите значения параметров в следующей команде. Затем выполните команду:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source-database>"
$pointInTime = "2018-06-27T08:51:39.3882806Z"
$targetDatabase = "<Name of new database to be created>"

Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
```

Чтобы восстановить базу данных на другой управляемый экземпляр, также укажите имена целевой группы ресурсов и целевого управляемого экземпляра.  

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
                              -TargetResourceGroupName $targetResourceGroupName `
                              -TargetInstanceName $targetInstanceName 
```

Дополнительные сведения см. в разделе [RESTORE-азсклинстанцедатабасе](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Если вы еще не установили Azure CLI, см. статью [установка Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

Чтобы восстановить базу данных с помощью Azure CLI, укажите значения параметров в следующей команде. Затем выполните команду:

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```

Чтобы восстановить базу данных на другой управляемый экземпляр, также укажите имена целевой группы ресурсов и управляемого экземпляра.  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Подробное описание доступных параметров см. в [документации CLI по восстановлению базы данных в управляемом экземпляре](https://docs.microsoft.com/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore).

---

## <a name="restore-a-deleted-database"></a>восстановлением удаленной базы данных;

Восстановление удаленной базы данных можно выполнить с помощью PowerShell или портал Azure. Чтобы восстановить удаленную базу данных в том же экземпляре, используйте портал Azure или PowerShell. Чтобы восстановить удаленную базу данных в другом экземпляре, используйте PowerShell. 

### <a name="portal"></a>Портал 


Чтобы восстановить управляемую базу данных с помощью портал Azure, откройте страницу Обзор управляемого экземпляра и выберите **Удаленные базы данных**. Выберите удаленную базу данных, которую требуется восстановить, и введите имя новой базы данных, которая будет создана с помощью данных, восстановленных из резервной копии.

  ![Снимок экрана: восстановление удаленной базы данных экземпляра SQL Azure](./media/sql-database-recovery-using-backups/restore-deleted-sql-managed-instance-annotated.png)

### <a name="powershell"></a>PowerShell

Чтобы восстановить базу данных в том же экземпляре, обновите значения параметров, а затем выполните следующую команду PowerShell: 

```powershell-interactive
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$deletedDatabaseName = "<Source database name>"
$targetDatabaseName = "<target database name>"

$deletedDatabase = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
-InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName
```

Чтобы восстановить базу данных на другой управляемый экземпляр, также укажите имена целевой группы ресурсов и целевого управляемого экземпляра.

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName `
   -TargetResourceGroupName $targetResourceGroupName `
   -TargetInstanceName $targetInstanceName 
```

## <a name="overwrite-an-existing-database"></a>Перезаписать существующую базу данных

Чтобы перезаписать существующую базу данных, необходимо выполнить следующие действия.

1. Удалите существующую базу данных, которую требуется перезаписать.
2. Переименуйте базу данных, восстановленную до точки во времени, на имя удаленной базы данных.

### <a name="drop-the-original-database"></a>Удалить исходную базу данных

Базу данных можно удалить с помощью портал Azure, PowerShell или Azure CLI.

Можно также удалить базу данных, подключившись к управляемому экземпляру напрямую, запустив SQL Server Management Studio (SSMS), а затем выполнив следующую команду Transact-SQL (T-SQL):

```sql
DROP DATABASE WorldWideImporters;
```

Используйте один из следующих методов для подключения к базе данных в управляемом экземпляре:

- [SSMS/Azure Data Studio через виртуальную машину Azure](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Точка-сеть](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Общедоступная конечная точка](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portal"></a>[Портал](#tab/azure-portal)

В портал Azure выберите базу данных из управляемого экземпляра, а затем щелкните **Удалить**.

   ![Удаление базы данных с помощью портал Azure](media/sql-database-managed-instance-point-in-time-restore/delete-database-from-mi.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Используйте следующую команду PowerShell для удаления существующей базы данных из управляемого экземпляра:

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Используйте следующую команду Azure CLI, чтобы удалить существующую базу данных из управляемого экземпляра.

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---

### <a name="alter-the-new-database-name-to-match-the-original-database-name"></a>Изменить имя новой базы данных в соответствии с именем исходной базы данных

Подключитесь непосредственно к управляемому экземпляру и запустите SQL Server Management Studio. Затем выполните следующий запрос Transact-SQL (T-SQL). Запрос изменит имя восстановленной базы данных на удаленную базу данных, которую предполагается перезаписать.

```sql
ALTER DATABASE WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```

Используйте один из следующих методов для подключения к базе данных в управляемом экземпляре:

- [Виртуальная машина Azure](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Точка-сеть](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Общедоступная конечная точка](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>Дальнейшие действия

Сведения об [автоматической архивации](sql-database-automated-backups.md).
