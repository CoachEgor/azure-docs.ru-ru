---
title: Импорт файла BACPAC для создания базы данных
description: Создайте базу данных SQL Azure, импортировав BACPAC-файл.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 06/20/2019
ms.openlocfilehash: 7db3f6f50745526876ef2ca6e3253f1931420f0f
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683246"
---
# <a name="quickstart-import-a-bacpac-file-to-a-database-in-azure-sql-database"></a>Быстрый запуск: Импортируйте файл BACPAC в базу данных в базе данных Azure S'L

С помощью файла [BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) вы можете импортировать базу данных SQL Server в Базу данных SQL Azure. Вы можете импортировать данные из файла `BACPAC`, которые хранятся в хранилище BLOB-объектов Azure (только цен. категория "Стандартный"), или из локального хранилища в локальное расположение. Чтобы развернуть скорость импорта, предоставив дополнительные ресурсы и ускоренную скорость доступа к ним, масштабируйте базу данных до более высокого уровня службы и объема вычислительных ресурсов во время процесса импорта. После завершения импорта эти значения можно уменьшить.

> [!NOTE]
> Уровень совместимости импортированной базы данных основан на уровне совместимости базы данных-источника.

> [!IMPORTANT]
> После импорта базы данных вы можете работать с ней при текущем уровне совместимости (уровень 100 для базы данных AdventureWorks2008R2) или при более высоком уровне. Дополнительные сведения о последствиях и параметрах работы базы данных с определенным уровнем совместимости см. в разделе [ALTER DATABASE (Transact-SQL) Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) (Уровень совместимости ALTER DATABASE (Transact-SQL)). Кроме того, в статье [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) вы можете получить сведения о дополнительных настройках уровня базы данных, связанных с уровнем совместимости.

## <a name="using-azure-portal"></a>Использование портала Azure

Просмотрите это видео, чтобы узнать, как импортировать файл BACPAC на портале Azure или продолжить чтение ниже:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Its-just-SQL-Restoring-a-database-to-Azure-SQL-DB-from-backup/player?WT.mc_id=dataexposed-c9-niner]

[Портал Azure поддерживает создание ](https://portal.azure.com) *только* отдельной базы данных в Базе данных SQL Azure и *только* из файла BACPAC, который хранится в хранилище BLOB-объектов Azure.

Преобразование базы данных в [управляемый экземпляр](sql-database-managed-instance.md) из файла BACPAC с использованием Azure PowerShell в настоящее время не поддерживается. Вместо этого используйте студию управления серверами s'L или S'LPackage.

> [!NOTE]
> Машины, обрабатывающие импорт/экспорт запросы, подаваемые через портал Azure или PowerShell, должны хранить файл BACPAC, а также временные файлы, генерируемые Рамочной системой приложений Data-Tier (DacFX). Требуемое дисковое пространство значительно варьируется между базами данных с одинаковым размером и может потребовать дискового пространства до 3 раз больше базы данных. Машины, запускаемые для запроса на импорт/экспорт, имеют только 450 ГБ локального дискового пространства. В результате некоторые запросы могут `There is not enough space on the disk`выполнить ошибку. В этом случае, обходной путь заключается в запуске sqlpackage.exe на машине с достаточным пространством локального диска. Мы рекомендуем использовать SqlPackage для импорта/экспорта баз данных размером более 150 ГБ, чтобы избежать этой проблемы.

1. Чтобы импортировать из файла BACPAC в новую отдельную базу данных с помощью портала Azure, откройте требуемую страницу сервера базы данных, а затем выберите **Импорт базы данных** на панели инструментов.  

   ![База данных import1](./media/sql-database-import/sql-server-import-database.png)

1. Сначала выберите учетную запись хранения и контейнер для файла BACPAC, а затем — сам файл BACPAC из которого необходимо выполнить импорт.

1. Укажите размер новой базы данных (обычно он такой же, как и у источника) и учетные данные для целевого сервера SQL Server. Список значений, доступных для новой базы данных SQL Azure, см. в статье [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current).

   ![База данных import2](./media/sql-database-import/sql-server-import-database-settings.png)

1. Нажмите кнопку **ОК**.

1. Чтобы отслеживать ход выполнения импорта, откройте страницу сервера базы данных. В разделе **Параметры** выберите **Журнал импорта и экспорта**. При успешном выполнении импорта отображается состояние **Завершено**.

   ![Состояние импорта базы данных](./media/sql-database-import/sql-server-import-database-history.png)

1. Выберите **Базы данных SQL**, чтобы проверить активность базы данных на сервере базы данных, и убедитесь, что для новой базы данных задано состояние **В сети**.

## <a name="using-sqlpackage"></a>Использование SqlPackage

Импорт базы данных SQL Server с помощью программы командной строки [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) описывается в разделе [Параметры и свойства импорта](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). В состав SqlPackage входят последние версии [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) и [SQL Server Data Tools для Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Кроме того, новую версию [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) можно скачать в Центре загрузки Майкрософт.

Вместо портала Azure рекомендуется использовать SqlPackage для масштабирования и обеспечения производительности в большинстве рабочих сред. Сведения о миграции из SQL Server в Базу данных SQL Azure с помощью файлов `BACPAC` см. в блоге команды Помощника по Azure для пользователей SQL Server [здесь](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Приведенная ниже команда SqlPackage импортирует базу данных **AdventureWorks2008R2** из локального хранилища на сервер Базы данных SQL Azure, который называется **mynewserver20170403**. Эта команда создает базу данных **myMigratedDatabase** с уровнем служб **Премиум** и целевым уровнем служб **P6**. Подставьте соответствующие значения для своей среды.

```cmd
sqlpackage.exe /a:import /tcs:"Data Source=<serverName>.database.windows.net;Initial Catalog=<migratedDatabase>;User Id=<userId>;Password=<password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Чтобы подключиться к серверу Базы данных SQL, управляя отдельной базой данных при включенном корпоративном брандмауэре, необходимо открыть порт 1433 в брандмауэре. Чтобы подключиться к управляемому экземпляру, необходимо обладать [подключением типа "точка — сеть"](sql-database-managed-instance-configure-p2s.md) или подключением Express Route.

В этом примере показано, как импортировать базу данных с помощью SqlPackage, используя универсальную проверку подлинности Active Directory.

```cmd
sqlpackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="using-powershell"></a>Использование PowerShell

> [!NOTE]
> [Управляемый экземпляр](sql-database-managed-instance.md) в настоящее время не поддерживает миграцию базы данных в базу данных экземпляров из файла BACPAC с помощью Azure PowerShell. Чтобы импортировать в управляемый экземпляр, используйте SQL Server Management Studio или SQLPackage.

> [!NOTE]
> Машины, обрабатывающие импорт/экспорт запросы, представленные через портал или Powershell, должны хранить файл bacpac, а также временные файлы, генерируемые Рамочной системой применения Data-Tier (DacFX). Требуемое дисковое пространство значительно варьируется среди DB с одинаковым размером и может занять до 3 раз от размера базы данных. Машины, запускаемые для запроса на импорт/экспорт, имеют только 450 ГБ локального дискового пространства. В результате некоторые запросы могут выполнить ошибку "На диске недостаточно места". В этом случае, обходной путь заключается в запуске sqlpackage.exe на машине с достаточным пространством локального диска. При импорте/экспорте баз данных больше, чем 150 ГБ, используйте SqlPackage, чтобы избежать этой проблемы.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

> [!IMPORTANT]
> Модуль PowerShell Azure Resource Manager (RM) по-прежнему поддерживается базой данных Azure S'L, но все будущие разработки предназначены для модуля Az.Sql. Модуль AzureRM будет получать исправления ошибок по крайней мере до декабря 2020 года.  Аргументы для команд в модуле Az и в модулях Azrm существенно идентичны. Подробнее об их совместимости читайте [в новом модуле Azure PowerShell Az.](/powershell/azure/new-azureps-module-az)

Для отправки запроса на импортную базу данных в службу базы данных Azure S'L воспользуйтесь смдlet [New-AzSqlDatabaseImport.](/powershell/module/az.sql/new-azsqldatabaseimport) Операция импорта может занять некоторое время в зависимости от размера базы данных.

```powershell
$importRequest = New-AzSqlDatabaseImport -ResourceGroupName "<resourceGroupName>" `
    -ServerName "<serverName>" -DatabaseName "<databaseName>" `
    -DatabaseMaxSizeBytes "<databaseSizeInBytes>" -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzStorageAccountKey `
        -ResourceGroupName "<resourceGroupName>" -StorageAccountName "<storageAccountName>").Value[0] `
        -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
        -Edition "Standard" -ServiceObjectiveName "P6" `
        -AdministratorLogin "<userId>" `
        -AdministratorLoginPassword $(ConvertTo-SecureString -String "<password>" -AsPlainText -Force)
```

Для проверки хода импорта можно использовать cmdlet [Get-AzSqlDatabaseImportExportStatus.](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) Запуск cmdlet сразу после запроса обычно возвращается `Status: InProgress`. Импорт завершен, когда `Status: Succeeded`вы видите.

```powershell
$importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink

[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress") {
    $importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}

[Console]::WriteLine("")
$importStatus
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Используйте команду [az-sql-db-import,](/cli/azure/sql/db#az-sql-db-import) чтобы отправить запрос на базу данных импорта в службу базы данных Azure S'L. Операция импорта может занять некоторое время в зависимости от размера базы данных.

```azurecli
# get the storage account key
az storage account keys list --resource-group "<resourceGroup>" --account-name "<storageAccount>"

az sql db import --resource-group "<resourceGroup>" --server "<server>" --name "<database>" `
    --storage-key-type "StorageAccessKey" --storage-key "<storageAccountKey>" `
    --storage-uri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -u "<userId>" -p "<password>"
```

* * *

> [!TIP]
> Еще один пример сценария приведен в разделе [Импорт базы данных из BACPAC-файла](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="limitations"></a>Ограничения

- Импорт базы данных в эластичный пул не поддерживается. Однако вы можете сначала выполнить импорт данных в отдельную базу данных, а затем переместить эту базу данных в эластичный пул.
- Служба экспорта импорта не работает при установлении доступа к службам Azure. Однако вы можете обойти проблему, вручную запустив sqlpackage.exe из Azure VM или выполняя экспорт непосредственно в коде, используя API DACFx.

## <a name="import-using-wizards"></a>Импорт с помощью мастера

Импорт также можно выполнить с помощью следующих мастеров:

- [мастер импорта приложения уровня данных в SQL Server Management Studio](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard);
- [Мастер импорта и экспорта серверов S'L](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Следующие шаги

- Чтобы узнать, как подключиться к импортируемой [Quickstart: Azure SQL Database: Use SQL Server Management Studio to connect and query data](sql-database-connect-query-ssms.md)базе данных S'L и задать запрос, см.
- Сведения о миграции из SQL Server в Базу данных SQL Azure с использованием BACPAC-файлов см. в [блоге группы консультирования клиентов SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/Migrating-from-SQL-Server-to-Azure-SQL-Database-using-Bacpac/ba-p/305407).
- Описание процесса миграции базы данных SQL Server в базу данных SQL Azure, включая рекомендации по его использованию, см. в [этой статье](sql-database-single-database-migrate.md).
- Чтобы узнать, как безопасно управлять ключами к хранилищу данных и подписанными URL-адресами и совместно их использовать, ознакомьтесь с [руководством по безопасности службы хранилища Azure](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
