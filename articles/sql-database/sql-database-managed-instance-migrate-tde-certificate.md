---
title: Перенос сертификата прозрачного шифрования данных (TDE) Управляемого экземпляра Базы данных Azure SQL | Документация Майкрософт
description: Сведения о переносе сертификата, защищающего ключ шифрования базы данных с прозрачным шифрованием данных, в Управляемый экземпляр Базы данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: carlrab, jovanpop
ms.date: 04/25/2019
ms.openlocfilehash: 6f9c1cefafdf6f7f33db3c5143e6b97b328fe699
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567416"
---
# <a name="migrate-certificate-of-tde-protected-database-to-azure-sql-database-managed-instance"></a>Перенос сертификата защищенной TDE базы данных в Управляемый экземпляр Базы данных SQL Azure

При миграции базы данных, защищенной с помощью [прозрачного шифрования данных (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption), в Управляемый экземпляр Базы данных SQL Azure с использованием параметра "Собственное восстановление" необходимо перед восстановлением базы данных перенести соответствующий сертификат из локального или IaaS SQL Server. В этой статье рассматривается процесс миграции сертификата в Управляемый экземпляр Базы данных SQL Azure вручную.

> [!div class="checklist"]
> * Сертификат обычно распространяется в виде файла обмена персональной информацией (.pfx)
> * Извлеките сертификат из файла в строку формата base-64
> * Загрузите его с помощью командлета PowerShell

Альтернативный вариант — это использование полностью управляемой службы для плавной миграции как защищенной TDE базы данных, так и соответствующего сертификата. Дополнительные сведения см. в разделе [Перенос SQL Server в Управляемый экземпляр базы данных SQL Azure с помощью DMS](../dms/tutorial-sql-server-to-managed-instance.md).

> [!IMPORTANT]
> Перенесенный сертификат используется только для восстановления базы данных, защищенной TDE. Вскоре после восстановления перенесенный сертификат заменяется другим предохранителем, либо управляемым службой сертификатом, либо асимметричным ключом из хранилища ключей в зависимости от типа прозрачного шифрования данных, заданного для экземпляра.

## <a name="prerequisites"></a>предварительные требования

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Модуль PowerShell Azure Resource Manager по-прежнему поддерживается базой данных SQL Azure, но вся будущая разработка предназначена для модуля AZ. SQL. Эти командлеты см. в разделе [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Аргументы для команд в модуле AZ и в модулях AzureRm существенно идентичны.

Чтобы выполнить действия, описанные в этой статье, необходимо следующее.

- Средство командной строки[Pvk2Pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx), установленное на локальном сервере или другом компьютере с доступом к сертификату, экспортированному как файл. Средство Pvk2Pfx является частью [набора драйверов Windows Корпоративный](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk), изолированной автономной среды командной строки.
- [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell) версии 5.0 или более поздней.
- Модуль Azure PowerShell [установлен и обновлен](https://docs.microsoft.com/powershell/azure/install-az-ps).
- [AZ. SQL Module](https://www.powershellgallery.com/packages/Az.Sql).
  Выполните следующие команды в PowerShell, чтобы установить или обновить модуль PowerShell.

   ```powershell
   Install-Module -Name Az.Sql
   Update-Module -Name Az.Sql
   ```

## <a name="export-tde-certificate-to-a-personal-information-exchange-pfx-file"></a>Экспортируйте сертификат TDE в файл обмена личной информацией (.pfx)

Сертификат можно экспортировать непосредственно из источника SQL Server или из хранилища сертификатов, если он хранится там.

### <a name="export-certificate-from-the-source-sql-server"></a>Экспорт сертификата из источника SQL Server

Следуйте инструкциям ниже, чтобы экспортировать сертификат с помощью SQL Server Management Studio и преобразовать его в формат PFX. В следующих шагах используются универсальные имена *TDE_Cert* и *full_path* для имен сертификата и файла и путей. Их должны заменить фактические имена.

1. В среде SSMS откройте новое окно запроса и подключитесь к источнику SQL Server.
2. Используйте следующий сценарий, чтобы перечислить защищенные TDE базы данных и получите имя сертификата, защищающего шифрование базы данных, которые подлежат миграции.

   ```sql
   USE master
   GO
   SELECT db.name as [database_name], cer.name as [certificate_name]
   FROM sys.dm_database_encryption_keys dek
   LEFT JOIN sys.certificates cer
   ON dek.encryptor_thumbprint = cer.thumbprint
   INNER JOIN sys.databases db
   ON dek.database_id = db.database_id
   WHERE dek.encryption_state = 3
   ```

   ![Список сертификатов TDE](./media/sql-database-managed-instance-migrate-tde-certificate/onprem-certificate-list.png)

3. Выполните следующее действие, чтобы экспортировать сертификат в пару файлов (.cer и .pvk), сохраняя данные открытого и закрытого ключа.

   ```sql
   USE master
   GO
   BACKUP CERTIFICATE TDE_Cert
   TO FILE = 'c:\full_path\TDE_Cert.cer'
   WITH PRIVATE KEY (
     FILE = 'c:\full_path\TDE_Cert.pvk',
     ENCRYPTION BY PASSWORD = '<SomeStrongPassword>'
   )
   ```

   ![создание резервной копии сертификата TDE](./media/sql-database-managed-instance-migrate-tde-certificate/backup-onprem-certificate.png)

4. Используйте консоль PowerShell для копирования сведений о сертификате из пары недавно созданных файлов в файл обмена личной информацией (.pfx) с помощью средства Pvk2Pfx.

   ```powershell
   .\pvk2pfx -pvk c:/full_path/TDE_Cert.pvk  -pi "<SomeStrongPassword>" -spc c:/full_path/TDE_Cert.cer -pfx c:/full_path/TDE_Cert.pfx
   ```

### <a name="export-certificate-from-certificate-store"></a>Экспорт SSL-сертификата из хранилища

Если сертификат хранится в хранилище сертификатов локального компьютера SQL Server, его можно экспортировать, выполнив следующие действия.

1. Чтобы открыть сертификат оснастки консоли управления Microsoft, откройте консоль PowerShell и выполните следующую команду.

   ```powershell
   certlm
   ```

2. В сертификате оснастки MMC расширьте путь "Личное -> Сертификаты", чтобы просмотреть список сертификатов

3. Щелкните правой кнопкой мыши сертификат и нажмите команду "Экспорт".

4. Следуйте указаниям мастера настроек, чтобы экспортировать сертификат и закрытый ключ в формате обмена личной информацией

## <a name="upload-certificate-to-azure-sql-database-managed-instance-using-azure-powershell-cmdlet"></a>Отправка сертификата в Управляемый экземпляр Базы данных SQL Azure с помощью командлета Azure PowerShell

1. Начните с шагов подготовки в PowerShell.

   ```powershell
   # Import the module into the PowerShell session
   Import-Module Az
   # Connect to Azure with an interactive dialog for sign-in
   Connect-AzAccount
   # List subscriptions available and copy id of the subscription target Managed Instance belongs to
   Get-AzSubscription
   # Set subscription for the session (replace Guid_Subscription_Id with actual subscription id)
   Select-AzSubscription Guid_Subscription_Id
   ```

2. После выполнения всех этапов подготовки, запустите следующие команды для передачи сертификата с кодировкой base-64 в целевой управляемый экземпляр.

   ```powershell
   $fileContentBytes = Get-Content 'C:/full_path/TDE_Cert.pfx' -Encoding Byte
   $base64EncodedCert = [System.Convert]::ToBase64String($fileContentBytes)
   $securePrivateBlob = $base64EncodedCert  | ConvertTo-SecureString -AsPlainText -Force
   $password = "SomeStrongPassword"
   $securePassword = $password | ConvertTo-SecureString -AsPlainText -Force
   Add-AzSqlManagedInstanceTransparentDataEncryptionCertificate -ResourceGroupName "<ResourceGroupName>" -ManagedInstanceName "<ManagedInstanceName>" -PrivateBlob $securePrivateBlob -Password $securePassword
   ```

Теперь сертификат доступен для указанного управляемого экземпляра и резервная копия соответствующей базы данных, защищенной TDE, может быть успешно восстановлена.

## <a name="next-steps"></a>Следующие шаги

В этой статье описано, как перенести сертификат, защищающий ключ шифрования базы данных с помощью прозрачного шифрования данных, из локального или IaaS SQL Server в Управляемый экземпляр Базы данных SQL Azure.

Дополнительные сведения о восстановлении резервной копии базы данных в Управляемом экземпляре Базы данных SQL Azure см. в разделе [Восстановление резервной копии базы данных в Управляемый экземпляр Базы данных SQL Azure](sql-database-managed-instance-get-started-restore.md).
