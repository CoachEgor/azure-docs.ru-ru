---
title: Пример PowerShell. Синхронизация данных между несколькими базами данных SQL Azure | Документация Майкрософт
description: Пример сценария PowerShell для синхронизации данных между несколькими базами данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 11b76374843743d3308b352c054ce9e8995f3543
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729216"
---
# <a name="use-powershell-to-sync-between-multiple-sql-databases"></a>Использование PowerShell для синхронизации данных между несколькими базами данных SQL
 
Этот пример PowerShell настраивает синхронизацию данных между несколькими базами данных SQL Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Чтобы установить и использовать PowerShell локально, для работы с этим руководством вам понадобится AZ PowerShell 1.4.0 или более поздней версии. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-az-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzAccount`, чтобы создать подключение к Azure.

Общие сведения о синхронизации данных SQL см. в статье [Синхронизация данных в нескольких облачных и локальных базах данных с помощью синхронизации данных SQL Azure](../sql-database-sync-data.md).

> [!IMPORTANT]
> Служба "Синхронизация данных SQL Azure" пока **не** поддерживает Управляемый экземпляры Базы данных SQL Azure.

## <a name="sample-script"></a>Пример скрипта

```powershell-interactive
# prerequisites: 
# 1. Create an Azure SQL Database from AdventureWorksLT sample database as hub database
# 2. Create an Azure SQL Database in the same region as sync database
# 3. Create an Azure SQL Database as member database
# 4. Update the parameters below before running the sample
#
using namespace Microsoft.Azure.Commands.Sql.DataSync.Model
using namespace System.Collections.Generic

# Hub database info
# Subscription id for hub database
$SubscriptionId = "subscription_guid"
# Resource group name for hub database
$ResourceGroupName = "ResourceGroup"
# Server name for hub database
$ServerName = "Server"
# Database name for hub database
$DatabaseName = "AdventureWorks"

# Sync database info
# Resource group name for sync database
$SyncDatabaseResourceGroupName = "ResourceGroup"
# Server name for sync database
$SyncDatabaseServerName = "Server"
# Sync database name
$SyncDatabaseName = "SyncDatabase"

# Sync group info
# Sync group name
$SyncGroupName = "SampleSyncGroup1"
# Conflict resolution Policy. Value can be HubWin or MemberWin
$ConflictResolutionPolicy = "HubWin"
# Sync interval in seconds. Value must be no less than 300
$IntervalInSeconds = 300

# Member database info
# Member name
$SyncMemberName = "member"
# Member server name
$MemberServerName = "MemberServer"
# Member database name
$MemberDatabaseName = "SyncDatabase1"
# Member database type. Value can be AzureSqlDatabase or SqlServerDatabase
$MemberDatabaseType = "AzureSqlDatabase"
# Sync direction. Value can be Bidirectional, Onewaymembertohub, Onewayhubtomember
$SyncDirection = "Bidirectional"

# Other info
# Temp file to save the sync schema
$TempFile = $env:TEMP+"\syncSchema.json"

# List of included columns and tables in quoted name
$IncludedColumnsAndTables =  "[SalesLT].[Address].[AddressID]",
                             "[SalesLT].[Address].[AddressLine2]",
                             "[SalesLT].[Address].[rowguid]",
                             "[SalesLT].[Address].[PostalCode]",
                             "[SalesLT].[ProductDescription]"
$MetadataList = [System.Collections.ArrayList]::new($IncludedColumnsAndTables)


Connect-AzAccount 
select-Azsubscription -SubscriptionId $SubscriptionId

# Use this section if it is safe to show password in the script.
# Otherwise, use the PromptForCredential
# $User = "username"
# $PWord = ConvertTo-SecureString -String "Password" -AsPlainText -Force
# $Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $PWord

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$ServerName+".database.windows.net", 
              "", 
              "")

# Create a new sync group
Write-Host "Creating Sync Group"$SyncGroupName
New-AzSqlSyncGroup   -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -Name $SyncGroupName `
                            -SyncDatabaseName $SyncDatabaseName `
                            -SyncDatabaseServerName $SyncDatabaseServerName `
                            -SyncDatabaseResourceGroupName $SyncDatabaseResourceGroupName `
                            -ConflictResolutionPolicy $ConflictResolutionPolicy `
                            -DatabaseCredential $Credential

# Use this section if it is safe to show password in the script.
#$User = "username"
#$Password = ConvertTo-SecureString -String "password" -AsPlainText -Force
#$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $Password

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$MemberServerName, 
              "", 
              "")

# Add a new sync member
# You can add members from other subscriptions, you don't need to specify Subscription Id for the member
Write-Host "Adding member"$SyncMemberName" to the sync group"
New-AzSqlSyncMember   -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -SyncGroupName $SyncGroupName `
                            -Name $SyncMemberName `
                            -MemberDatabaseCredential $Credential `
                            -MemberDatabaseName $MemberDatabaseName `
                            -MemberServerName ($MemberServerName + ".database.windows.net") `
                            -MemberDatabaseType $MemberDatabaseType `
                            -SyncDirection $SyncDirection

# Refresh database schema from hub database
# Specify the -SyncMemberName parameter if you want to refresh schema from the member database
Write-Host "Refreshing database schema from hub database"
$StartTime= Get-Date
Update-AzSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                              -ServerName $ServerName `
                              -DatabaseName $DatabaseName `
                              -SyncGroupName $SyncGroupName


#Waiting for successful refresh

$StartTime=$StartTime.ToUniversalTime()
$timer=0
$timeout=90
# Check the log and see if refresh has gone through
Write-Host "Check for successful refresh"
$IsSucceeded = $false
While ($IsSucceeded -eq $false)
{
    Start-Sleep -s 10
    $timer=$timer+10
    $Details = Get-AzSqlSyncSchema -SyncGroupName $SyncGroupName -ServerName $ServerName -DatabaseName $DatabaseName -ResourceGroupName $ResourceGroupName
    if ($Details.LastUpdateTime -gt $StartTime)
      {
        Write-Host "Refresh was successful"
        $IsSucceeded = $true
      }
    if ($timer -eq $timeout) 
      {
              Write-Host "Refresh timed out"
        break;
      }
}



# Get the database schema 
Write-Host "Adding tables and columns to the sync schema"
$databaseSchema = Get-AzSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                                             -ServerName $ServerName `
                                             -DatabaseName $DatabaseName `
                                             -SyncGroupName $SyncGroupName `

$databaseSchema | ConvertTo-Json -depth 5 -Compress | Out-File "c:\tmp\databaseSchema"     
$newSchema = [AzureSqlSyncGroupSchemaModel]::new()
$newSchema.Tables = [List[AzureSqlSyncGroupSchemaTableModel]]::new();

# Add columns and tables to the sync schema
foreach ($tableSchema in $databaseSchema.Tables)
{
    $newTableSchema = [AzureSqlSyncGroupSchemaTableModel]::new()
    $newTableSchema.QuotedName = $tableSchema.QuotedName
    $newTableSchema.Columns = [List[AzureSqlSyncGroupSchemaColumnModel]]::new();
    $addAllColumns = $false
    if ($MetadataList.Contains($tableSchema.QuotedName))
    {
        if ($tableSchema.HasError)
        {
            $fullTableName = $tableSchema.QuotedName
            Write-Host "Can't add table $fullTableName to the sync schema" -foregroundcolor "Red"
            Write-Host $tableSchema.ErrorId -foregroundcolor "Red"
            continue;
        }
        else
        {
            $addAllColumns = $true
        }
    }
    foreach($columnSchema in $tableSchema.Columns)
    {
        $fullColumnName = $tableSchema.QuotedName + "." + $columnSchema.QuotedName
        if ($addAllColumns -or $MetadataList.Contains($fullColumnName))
        {
            if ((-not $addAllColumns) -and $tableSchema.HasError)
            {
                Write-Host "Can't add column $fullColumnName to the sync schema" -foregroundcolor "Red"
                Write-Host $tableSchema.ErrorId -foregroundcolor "Red"c            }
            elseif ((-not $addAllColumns) -and $columnSchema.HasError)
            {
                Write-Host "Can't add column $fullColumnName to the sync schema" -foregroundcolor "Red"
                Write-Host $columnSchema.ErrorId -foregroundcolor "Red"
            }
            else
            {
                Write-Host "Adding"$fullColumnName" to the sync schema"
                $newColumnSchema = [AzureSqlSyncGroupSchemaColumnModel]::new()
                $newColumnSchema.QuotedName = $columnSchema.QuotedName
                $newColumnSchema.DataSize = $columnSchema.DataSize
                $newColumnSchema.DataType = $columnSchema.DataType
                $newTableSchema.Columns.Add($newColumnSchema)
            }
        }
    }
    if ($newTableSchema.Columns.Count -gt 0)
    {
        $newSchema.Tables.Add($newTableSchema)
    }
}

# Convert sync schema to Json format
$schemaString = $newSchema | ConvertTo-Json -depth 5 -Compress

# workaround a powershell bug
$schemaString = $schemaString.Replace('"Tables"', '"tables"').Replace('"Columns"', '"columns"').Replace('"QuotedName"', '"quotedName"').Replace('"MasterSyncMemberName"','"masterSyncMemberName"')

# Save the sync schema to a temp file
$schemaString | Out-File $TempFile

# Update sync schema
Write-Host "Updating the sync schema"
Update-AzSqlSyncGroup  -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -Name $SyncGroupName `
                            -Schema $TempFile

$SyncLogStartTime = Get-Date

# Trigger sync manually
Write-Host "Trigger sync manually"
Start-AzSqlSyncGroupSync  -ResourceGroupName $ResourceGroupName `
                               -ServerName $ServerName `
                               -DatabaseName $DatabaseName `
                               -SyncGroupName $SyncGroupName

# Check the sync log and wait until the first sync succeeded
Write-Host "Check the sync log"
$IsSucceeded = $false
For ($i = 0; ($i -lt 300) -and (-not $IsSucceeded); $i = $i + 10)
{
    Start-Sleep -s 10
    $SyncLogEndTime = Get-Date
    $SyncLogList = Get-AzSqlSyncGroupLog  -ResourceGroupName $ResourceGroupName `
                                           -ServerName $ServerName `
                                           -DatabaseName $DatabaseName `
                                           -SyncGroupName $SyncGroupName `
                                           -StartTime $SyncLogStartTime.ToUniversalTime() `
                                           -EndTime $SyncLogEndTime.ToUniversalTime()
    if ($SynclogList.Length -gt 0)
    {
        foreach ($SyncLog in $SyncLogList)
        {
            if ($SyncLog.Details.Contains("Sync completed successfully"))
            {
                Write-Host $SyncLog.TimeStamp : $SyncLog.Details
                $IsSucceeded = $true
            }
        }
    }
}

if ($IsSucceeded)
{
    # Enable scheduled sync
    Write-Host "Enable the scheduled sync with 300 seconds interval"
    Update-AzSqlSyncGroup  -ResourceGroupName $ResourceGroupName `
                                -ServerName $ServerName `
                                -DatabaseName $DatabaseName `
                                -Name $SyncGroupName `
                                -IntervalInSeconds $IntervalInSeconds
}
else
{
    # Output all log if sync doesn't succeed in 300 seconds
    $SyncLogEndTime = Get-Date
    $SyncLogList = Get-AzSqlSyncGroupLog  -ResourceGroupName $ResourceGroupName `
                                           -ServerName $ServerName `
                                           -DatabaseName $DatabaseName `
                                           -SyncGroupName $SyncGroupName `
                                           -StartTime $SyncLogStartTime.ToUniversalTime() `
                                           -EndTime $SyncLogEndTime.ToUniversalTime()
    if ($SynclogList.Length -gt 0)
    {
        foreach ($SyncLog in $SyncLogList)
        {
            Write-Host $SyncLog.TimeStamp : $SyncLog.Details
        }
    }
}

# Clean up deployment 
# Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
# Remove-AzResourceGroup -ResourceGroupName $SyncDatabaseResourceGroupName

```

## <a name="clean-up-deployment"></a>Очистка развертывания

После выполнения примера сценария можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду.

```powershell
Remove-AzResourceGroup -ResourceGroupName $ResourceGroupName
Remove-AzResourceGroup -ResourceGroupName $SyncDatabaseResourceGroupName
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [New-AzSqlSyncAgent](/powershell/module/az.sql/New-azSqlSyncAgent) |  Создает агент синхронизации. |
| [New-AzSqlSyncAgentKey](/powershell/module/az.sql/New-azSqlSyncAgentKey) |  Формирует ключ агента, связанный с агентом синхронизации. |
| [Get-AzSqlSyncAgentLinkedDatabase](/powershell/module/az.sql/Get-azSqlSyncAgentLinkedDatabase) |  Получает всю информацию для агента синхронизации. |
| [New-AzSqlSyncMember](/powershell/module/az.sql/New-azSqlSyncMember) |  Добавляет нового участника в группу синхронизации. |
| [Update-AzSqlSyncSchema](/powershell/module/az.sql/Update-azSqlSyncSchema) |  Обновляет сведения о схеме базы данных. |
| [Get-AzSqlSyncSchema](https://docs.microsoft.com/powershell/module/az.sql/Get-azSqlSyncSchema) |  Извлекает сведения о схеме базы данных. |
| [Update-AzSqlSyncGroup](/powershell/module/az.sql/Update-azSqlSyncGroup) |  Обновляет группу синхронизации. |
| [Start-AzSqlSyncGroupSync](/powershell/module/az.sql/Start-azSqlSyncGroupSync) | Активирует синхронизацию. |
| [Get-AzSqlSyncGroupLog](/powershell/module/az.sql/Get-azSqlSyncGroupLog) |  Проверяет журнал синхронизации. |
|||

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

Дополнительные примеры сценариев PowerShell для Базы данных SQL Azure можно найти в разделе [Примеры Azure PowerShell для Базы данных SQL Azure](../sql-database-powershell-samples.md).

Дополнительные сведения о синхронизации данных SQL:

-   Обзор: [Синхронизация данных в нескольких облачных и локальных базах данных с помощью функции синхронизации данных SQL Azure](../sql-database-sync-data.md).
-   Настройка синхронизации данных
    - На портале: [Руководство по настройке синхронизации данных SQL между базой данных SQL Azure и локальной базой данных SQL Server](../sql-database-get-started-sql-data-sync.md)
    - С помощью PowerShell
        -  [Использование PowerShell для синхронизации данных между базой данных SQL Azure и локальной базой данных SQL Server](sql-database-sync-data-between-azure-onprem.md)
-   Агент синхронизации данных: [Агент синхронизации данных для синхронизации данных SQL Azure](../sql-database-data-sync-agent.md).
-   Рекомендации: [Рекомендации по синхронизации данных SQL](../sql-database-best-practices-data-sync.md).
-   Мониторинг: [Мониторинг синхронизации данных SQL с помощью журналов Azure Monitor](../sql-database-sync-monitor-oms.md)
-   Устранение неполадок: [Устранение неполадок с синхронизацией данных SQL](../sql-database-troubleshoot-data-sync.md).
-   Обновление схемы синхронизации
    -   С помощью Transact-SQL: [Автоматическая репликация изменений схемы при синхронизации данных SQL Azure](../sql-database-update-sync-schema.md).
    -   С помощью PowerShell: [Использование PowerShell для обновления схемы синхронизации в существующей группе синхронизации](sql-database-sync-update-schema.md).

Дополнительные сведения о Базе данных SQL:

-   [Обзор Базы данных SQL](../sql-database-technical-overview.md)
-   [Управление жизненным циклом базы данных](https://msdn.microsoft.com/library/jj907294.aspx)
