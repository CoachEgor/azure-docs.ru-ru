---
title: Использование общей папки Azure в Windows | Документация Майкрософт
description: Узнайте, как использовать общую папку Azure в Windows и Windows Server.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 4fef6102ac2ee69926c1c56af338b6e92670dd71
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773106"
---
# <a name="use-an-azure-file-share-with-windows"></a>Использование общей папки Azure в Windows
[Файлы Azure](storage-files-introduction.md) — это простая в использовании облачная файловая система от корпорации Майкрософт. Общие папки Azure можно легко использовать в Windows и Windows Server. В этой статье рассматриваются рекомендации по использованию общей папки Azure в Windows и Windows Server.

Чтобы использовать общую папку Azure за пределами региона Azure, в котором она размещается, например локально или в другом регионе Azure, операционная система должна поддерживать протокол SMB 3.0. 

Общие папки Azure можно использовать в ОС Windows, работающей на виртуальной машине Azure или в локальной среде. В следующей таблице показано, какие версии операционной системы поддерживают доступ к общим папкам и в какой среде:

| Версия Windows        | Версия SMB | Возможность подключения на виртуальной машине Azure | Возможность подключения в локальной среде |
|------------------------|-------------|-----------------------|-----------------------|
| Windows Server 2019 | SMB 3.0 | Да | Да |
| Windows 10<sup>1</sup> | SMB 3.0 | Да | Да |
| Semi-Annual Channel<sup>2</sup> для Windows Server | SMB 3.0 | Да | Да |
| Windows Server 2016 | SMB 3.0 | Да | Да |
| Windows 8.1 | SMB 3.0 | Да | Да |
| Windows Server 2012 R2 | SMB 3.0 | Да | Да |
| Windows Server 2012 | SMB 3.0 | Да | Да |
| Windows 7<sup>3</sup>. | SMB 2.1 | Да | нет |
| Windows Server 2008 R2<sup>3</sup> | SMB 2.1 | Да | нет |

<sup>1</sup>Windows 10, версий 1507, 1607, 1709, 1803, 1809, 1903, и 1909.  
<sup>2</sup>Windows Server, версий 1809, 1903, и 1909.  
<sup>3</sup>Обычная поддержка Майкрософт для Windows 7 и Windows Server 2008 R2 закончилась. Дополнительную поддержку обновлений для системы безопасности можно приобрести только с помощью программы [Расширенные обновления для системы безопасности (ESU)](https://support.microsoft.com/help/4497181/lifecycle-faq-extended-security-updates). Мы настоятельно рекомендуем отказаться от этих операционных систем.

> [!Note]  
> Мы всегда рекомендуем использовать последнюю версию KB для своей версии Windows.

## <a name="prerequisites"></a>Предварительные требования 
* **Имя учетной записи хранения**. Чтобы подключить общую папку Azure, вам понадобится имя учетной записи хранения.

* **Ключ учетной записи хранения**. Чтобы подключить общую папку Azure, вам понадобится первичный (или вторичный) ключ учетной записи хранения. В настоящее время ключи SAS для подключения не поддерживаются.

* **Открытый порт 445**. Для протокола SMB требуется, чтобы TCP-порт 445 был открыт. В противном случае установить подключение не получится. Вы можете проверить, блокирует ли ваш брандмауэр порт 445, с помощью командлета `Test-NetConnection`. Вы можете узнать о [различных способах устранения неполадок, связанных с блокированием порта 445, здесь](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems#cause-1-port-445-is-blocked).

    Следующий код PowerShell предполагает, что модуль Azure PowerShell установлен. Дополнительные сведения см. в разделе [Установка модуля Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps). Не забудьте заменить `<your-storage-account-name>` и `<your-resource-group-name>` соответствующими именами для вашей учетной записи хранения.

    ```powershell
    $resourceGroupName = "<your-resource-group-name>"
    $storageAccountName = "<your-storage-account-name>"

    # This command requires you to be logged into your Azure account, run Login-AzAccount if you haven't
    # already logged in.
    $storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName

    # The ComputerName, or host, is <storage-account>.file.core.windows.net for Azure Public Regions.
    # $storageAccount.Context.FileEndpoint is used because non-Public Azure regions, such as sovereign clouds
    # or Azure Stack deployments, will have different hosts for Azure file shares (and other storage resources).
    Test-NetConnection -ComputerName ([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) -Port 445
    ```

    Если установка прошла успешно, отобразятся следующие выходные данные.

    ```
    ComputerName     : <storage-account-host-name>
    RemoteAddress    : <storage-account-ip-address>
    RemotePort       : 445
    InterfaceAlias   : <your-network-interface>
    SourceAddress    : <your-ip-address>
    TcpTestSucceeded : True
    ```

    > [!Note]  
    > Вышеуказанная команда возвращает текущий IP-адрес учетной записи хранения. Не гарантируется, что IP-адрес будет постоянным. Он может измениться в любое время. Не следует жестко задавать этот IP-адрес в скриптах или в конфигурации брандмауэра. 

## <a name="using-an-azure-file-share-with-windows"></a>Использование общей папки Azure в Windows
Чтобы использовать общую папку Azure в Windows, вы должны либо подключить ее, то есть присвоить ей букву диска или путь точки подключения, либо получить доступ к ней через [UNC-путь](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx). 

В этой статье предполагается, что для доступа к общей папке используется ключ учетной записи хранения. Это ключ администратора для учетной записи хранения, включая разрешения администратора для всех файлов и папок в общей папке, к которой вы получаете доступ, и для всех общих папок и других ресурсов хранения (большие двоичные объекты, очереди, таблицы и т. д.), которые размещаются в этой учетной записи. Если этого недостаточно для вашей рабочей нагрузки, можно использовать [Синхронизацию файлов Azure](storage-sync-files-planning.md) или [проверку подлинности на основе удостоверений через SMB](storage-files-active-directory-overview.md).

Общим вариантом для переноса бизнес-приложений, которые ожидают наличия подключенной к Azure общей папки SMB, является использование общей папки Azure в качестве альтернативы для запуска выделенного файлового сервера Windows на виртуальной машине Azure. Одна из важных рекомендаций для успешного перевода бизнес-приложения на использование общей папки Azure заключается в том, что многие бизнес-приложения работают в контексте выделенной учетной записи службы с ограниченными системными правами, а не административной учетной записью виртуальной машины. Поэтому нужно обязательно подключить или сохранить учетные данные для общей папки Azure из контекста учетной записи службы, а не вашей учетной записи администратора.

### <a name="persisting-azure-file-share-credentials-in-windows"></a>Сохранение учетных данных общей папки Azure в Windows  
Программа [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) позволяет сохранить учетные данные учетной записи хранения в Windows. Это означает, что когда вы пытаетесь получить доступ к общей папке Azure через UNC-путь или подключаете общую папку Azure, вам не нужно указывать учетные данные. Чтобы сохранить данные учетной записи хранения, выполните следующие команды PowerShell, заменив `<your-storage-account-name>` и `<your-resource-group-name>` там, где это необходимо.

```powershell
$resourceGroupName = "<your-resource-group-name>"
$storageAccountName = "<your-storage-account-name>"

# These commands require you to be logged into your Azure account, run Login-AzAccount if you haven't
# already logged in.
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
$storageAccountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName

# The cmdkey utility is a command-line (rather than PowerShell) tool. We use Invoke-Expression to allow us to 
# consume the appropriate values from the storage account variables. The value given to the add parameter of the
# cmdkey utility is the host address for the storage account, <storage-account>.file.core.windows.net for Azure 
# Public Regions. $storageAccount.Context.FileEndpoint is used because non-Public Azure regions, such as sovereign 
# clouds or Azure Stack deployments, will have different hosts for Azure file shares (and other storage resources).
Invoke-Expression -Command ("cmdkey /add:$([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) " + `
    "/user:AZURE\$($storageAccount.StorageAccountName) /pass:$($storageAccountKeys[0].Value)")
```

Вы можете проверить, сохранила ли программа cmdkey данные учетной записи хранения, используя параметр list:

```powershell
cmdkey /list
```

Если учетные данные общей папки Azure сохранены успешно, вы увидите следующие выходные данные (в списке могут быть дополнительные ключи):

```
Currently stored credentials:

Target: Domain:target=<storage-account-host-name>
Type: Domain Password
User: AZURE\<your-storage-account-name>
```

Теперь вы можете подключить общую папку или получить к ней доступ без необходимости предоставления дополнительных учетных данных.

#### <a name="advanced-cmdkey-scenarios"></a>Расширенные сценарии cmdkey
Есть два дополнительных сценария использования cmdkey: сохранение учетных данных другого пользователя на компьютере, например данных учетной записи службы, и сохранение учетных данных на удаленном компьютере с помощью удаленного взаимодействия PowerShell.

Сохранить учетные данные для другого пользователя на компьютере очень просто: войдя в учетную запись, выполните следующую команду PowerShell:

```powershell
$password = ConvertTo-SecureString -String "<service-account-password>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "<service-account-username>", $password
Start-Process -FilePath PowerShell.exe -Credential $credential -LoadUserProfile
```

Откроется новое окно PowerShell в контексте пользователя вашей учетной записи службы (или учетной записи пользователя). Затем вы можете использовать программу cmdkey, как описано [выше](#persisting-azure-file-share-credentials-in-windows).

Однако сохранение учетных данных на удаленном компьютере с использованием удаленного взаимодействия PowerShell невозможно, так как cmdkey не разрешает доступ даже для дополнений к хранилищу учетных данных, когда пользователь выполняет удаленный вход с помощью PowerShell. Мы рекомендуем войти в систему с помощью [удаленного рабочего стола](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/windows).

### <a name="mount-the-azure-file-share-with-powershell"></a>Подключение общей папки Azure с помощью PowerShell
Выполните следующие команды в обычном (без повышенных прав) сеансе PowerShell для подключения общей папки Azure. Не забудьте заменить `<your-resource-group-name>`, `<your-storage-account-name>`, `<your-file-share-name>` и `<desired-drive-letter>` правильными данными.

```powershell
$resourceGroupName = "<your-resource-group-name>"
$storageAccountName = "<your-storage-account-name>"
$fileShareName = "<your-file-share-name>"

# These commands require you to be logged into your Azure account, run Login-AzAccount if you haven't
# already logged in.
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
$storageAccountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName
$fileShare = Get-AzStorageShare -Context $storageAccount.Context | Where-Object { 
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    throw [System.Exception]::new("Azure file share not found")
}

# The value given to the root parameter of the New-PSDrive cmdlet is the host address for the storage account, 
# <storage-account>.file.core.windows.net for Azure Public Regions. $fileShare.StorageUri.PrimaryUri.Host is 
# used because non-Public Azure regions, such as sovereign clouds or Azure Stack deployments, will have different 
# hosts for Azure file shares (and other storage resources).
$password = ConvertTo-SecureString -String $storageAccountKeys[0].Value -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "AZURE\$($storageAccount.StorageAccountName)", $password
New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\$($fileShare.StorageUri.PrimaryUri.Host)\$($fileShare.Name)" -Credential $credential -Persist
```

> [!Note]  
> Использование параметра `-Persist` в командлете `New-PSDrive` позволит повторно подключить общую папку при загрузке, только если учетные данные сохранены. Вы можете сохранить учетные данные, используя cmdkey, как [описано ранее](#persisting-azure-file-share-credentials-in-windows). 

При желании вы можете отключить общую папку Azure с помощью следующего командлета PowerShell.

```powershell
Remove-PSDrive -Name <desired-drive-letter>
```

### <a name="mount-the-azure-file-share-with-file-explorer"></a>Подключение общей папки Azure с помощью проводника
> [!Note]  
> Имейте в виду, что следующие инструкции приведены для Windows 10 и могут немного отличаться для более поздних версий. 

1. Откройте проводник. Это можно сделать, открыв меню "Пуск" или нажав клавиши WIN+E.

1. Перейдите к элементу **Этот компьютер** в левой области окна. Меню, доступные на ленте, изменятся. В меню "Компьютер" выберите раздел **Подключение сетевого диска**.
    
    ![Снимок экрана раскрывающегося меню "Подключение сетевого диска"](./media/storage-how-to-use-files-windows/1_MountOnWindows10.png)

1. Выберите букву диска и введите UNC-путь, формат UNC-пути — `\\<storageAccountName>.file.core.windows.net\<fileShareName>`. Например: `\\anexampleaccountname.file.core.windows.net\example-share-name`.
    
    ![Снимок экрана диалогового окна "Подключение сетевого диска"](./media/storage-how-to-use-files-windows/2_MountOnWindows10.png)

1. Используйте имя учетной записи хранения с префиксом `AZURE\` как имя пользователя и ключ учетной записи хранения в качестве пароля.
    
    ![Снимок экрана диалогового окна сетевых учетных данных](./media/storage-how-to-use-files-windows/3_MountOnWindows10.png)

1. Выберите общую папку Azure.
    
    ![Общая папка Azure теперь подключена](./media/storage-how-to-use-files-windows/4_MountOnWindows10.png)

1. Когда вы будете готовы отключить общую папку Azure, это можно сделать, щелкнув правой кнопкой мыши запись для общей папки в разделе проводника **Сетевые расположения** и выбрав параметр **Отключить**.

### <a name="accessing-share-snapshots-from-windows"></a>Доступ к моментальным снимкам общих файловых ресурсов из Windows
Если вы уже создали моментальные снимки общего файлового ресурса вручную или автоматически с помощью скрипта или такой службы, как Azure Backup, вы можете просмотреть предыдущие версии общего ресурса, каталога или определенного файла из подключенного общего файлового ресурса в Windows. Вы можете создать его с помощью [портала Azure](storage-how-to-use-files-portal.md), [Azure PowerShell](storage-how-to-use-files-powershell.md) или [Azure CLI](storage-how-to-use-files-cli.md).

#### <a name="list-previous-versions"></a>Вывод списка предыдущих версий
Перейдите к элементу или родительскому элементу, который требуется восстановить. Дважды щелкните для перехода к нужному каталогу. Щелкните правой кнопкой мыши и выберите в меню пункт **Свойства**.

![Контекстное меню для выбранного каталога](./media/storage-how-to-use-files-windows/snapshot-windows-previous-versions.png)

Выберите пункт **Предыдущие версии**, чтобы просмотреть список моментальных снимков общих ресурсов для этого каталога. Вывод списка может занять несколько секунд в зависимости от скорости сети и количества моментальных снимков в каталоге.

![Вкладка "Предыдущие версии"](./media/storage-how-to-use-files-windows/snapshot-windows-list.png)

Чтобы открыть тот или иной моментальный снимок, нажмите кнопку **Открыть**. 

![Открытый моментальный снимок](./media/storage-how-to-use-files-windows/snapshot-browse-windows.png)

#### <a name="restore-from-a-previous-version"></a>Восстановление из предыдущей версии
Нажмите кнопку **Восстановить**, чтобы рекурсивно скопировать содержимое всего каталога на момент создания моментального снимка общего ресурса в исходном расположении.

 ![Кнопка "Восстановить" в предупреждающем сообщении](./media/storage-how-to-use-files-windows/snapshot-windows-restore.png) 

## <a name="securing-windowswindows-server"></a>Обеспечение безопасности Windows или Windows Server
Чтобы подключить общую папку Azure в Windows, порт 445 должен быть доступен. Многие организации блокируют порт 445 из-за угроз безопасности, присущих SMB 1. SMB 1, также известный как CIFS (Common Internet File System), является устаревшим протоколом файловой системы, включенным в Windows и Windows Server. SMB 1 — это устаревший, неэффективный и, самое главное, небезопасный протокол. Хорошей новостью является то, что компонент "Файлы Azure" не поддерживает SMB 1, а все поддерживаемые версии Windows и Windows Server позволяют удалить или отключить этот протокол. Мы всегда [настоятельно рекомендуем](https://aka.ms/stopusingsmb1) удалять или отключать клиент и сервер SMB 1 в Windows перед использованием общей папки Azure в рабочей среде.

В следующей таблице представлена ​​подробная информация о состоянии SMB 1 для каждой версии Windows:

| Версия Windows                           | Состояние SMB 1 по умолчанию | Метод удаления или отключения       | 
|-------------------------------------------|----------------------|-----------------------------|
| Windows Server 2019                       | Выключено             | Удаление с помощью функции Windows |
| Windows Server версии 1709 или выше            | Выключено             | Удаление с помощью функции Windows |
| Windows 10 версии 1709 или выше                | Выключено             | Удаление с помощью функции Windows |
| Windows Server 2016                       | Активировано              | Удаление с помощью функции Windows |
| Windows 10 версии 1507, 1607 и 1703 | Активировано              | Удаление с помощью функции Windows |
| Windows Server 2012 R2                    | Активировано              | Удаление с помощью функции Windows | 
| Windows 8.1                               | Активировано              | Удаление с помощью функции Windows | 
| Windows Server 2012                       | Активировано              | Отключение с помощью реестра       | 
| Windows Server 2008 R2                    | Активировано              | Отключение с помощью реестра       |
| Windows 7                                 | Активировано              | Отключение с помощью реестра       | 

### <a name="auditing-smb-1-usage"></a>Аудит использования SMB 1
> Применяется к Windows Server 2019, Semi-Annual Channel для Windows Server (версии 1709 и 1803), Windows Server 2016, Windows 10 (версии 1507, 1607, 1703, 1709 и 1803), Windows Server 2012 R2 и Windows 8.1.

Прежде чем удалять SMB 1 в вашей среде, можете провести аудит использования SMB 1, чтобы узнать, повлияет ли это изменение на работу каких-либо клиентов. Если какие-либо запросы к папкам SMB выполняются с помощью SMB 1, событие аудита будет регистрироваться в журнале событий в разделе `Applications and Services Logs > Microsoft > Windows > SMBServer > Audit`. 

> [!Note]  
> Чтобы включить поддержку аудита в Windows Server 2012 R2 и Windows 8.1, требуется пакет обновлений не ниже указанного в [KB4022720](https://support.microsoft.com/help/4022720/windows-8-1-windows-server-2012-r2-update-kb4022720).

Чтобы включить аудит, выполните следующий командлет в сеансе PowerShell с повышенными правами:

```powershell
Set-SmbServerConfiguration –AuditSmb1Access $true
```

### <a name="removing-smb-1-from-windows-server"></a>Удаление SMB 1 из Windows Server
> Применяется к Windows Server 2019, Semi-Annual Channel для Windows Server (версии 1709 и 1803), Windows Server 2016 и Windows Server 2012 R2.

Чтобы удалить SMB 1 из экземпляра Windows Server, выполните следующий командлет в сеансе PowerShell с повышенными правами:

```powershell
Remove-WindowsFeature -Name FS-SMB1
```

Чтобы завершить процесс удаления, перезагрузите сервер. 

> [!Note]  
> Начиная с Windows 10 и Windows Server версии 1709, SMB 1 не установлен по умолчанию и имеет отдельные функции Windows для клиента SMB 1 и сервера SMB 1. Мы всегда рекомендуем удалять сервер SMB 1 (`FS-SMB1-SERVER`) и клиент SMB 1 (`FS-SMB1-CLIENT`).

### <a name="removing-smb-1-from-windows-client"></a>Удаление SMB 1 из клиента Windows
> Применяется к Windows 10 (версии 1507, 1607, 1703, 1709 и 1803) и Windows 8.1.

Чтобы удалить SMB 1 из клиента Windows, выполните следующий командлет в сеансе PowerShell с повышенными правами:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

Чтобы завершить процесс удаления, перезагрузите компьютер.

### <a name="disabling-smb-1-on-legacy-versions-of-windowswindows-server"></a>Отключение SMB 1 в устаревших версиях Windows и Windows Server
> Применяется к Windows Server 2012, Windows Server 2008 R2 и Windows 7.

SMB 1 нельзя полностью удалить в устаревших версиях Windows и Windows Server, но его можно отключить через реестр. Чтобы отключить SMB 1, создайте новый ключ реестра `SMB1` типа `DWORD` со значением `0` в `HKEY_LOCAL_MACHINE > SYSTEM > CurrentControlSet > Services > LanmanServer > Parameters`.

Вы можете легко выполнить это с помощью следующего командлета PowerShell:

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 –Force
```

После создания этого раздела реестра необходимо перезагрузить сервер, чтобы отключить SMB 1.

### <a name="smb-resources"></a>Ресурсы по SMB
- [Запись блога о прекращении использования SMB 1](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/)
- [Центр координации продуктов с SMB 1](https://blogs.technet.microsoft.com/filecab/2017/06/01/smb1-product-clearinghouse/)
- [Запись блога об обнаружении SMB 1 в своей среде с помощью DSCEA](https://blogs.technet.microsoft.com/ralphkyttle/2017/04/07/discover-smb1-in-your-environment-with-dscea/)
- [Запись блога об отключении SMB 1 в групповой политике](https://blogs.technet.microsoft.com/secguide/2017/06/15/disabling-smbv1-through-group-policy/)

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о службе файлов Azure см. по следующим ссылкам.
- [Планирование развертывания службы файлов Azure](storage-files-planning.md)
- [Часто задаваемые вопросы](../storage-files-faq.md)
- [Troubleshoot Azure File storage problems in Windows](storage-troubleshoot-windows-file-connection-problems.md) (Устранение неполадок хранилища файлов Azure в Windows)      
