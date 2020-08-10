---
title: Развертывание службы файлов Azure | Документы Майкрософт
description: Сведения о развертывании службы файлов Azure от начала до конца. Перенесите данные в службу файлов Azure. Автоматическое подключение к необходимым компьютерам или серверам.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 05/22/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 08bcb41dd8d9f4643b03d855960d8632b778ff84
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/10/2020
ms.locfileid: "88034504"
---
# <a name="how-to-deploy-azure-files"></a>Как развернуть службу файлов Azure
[Служба файлов Azure](storage-files-introduction.md) предлагает полностью управляемые общие файловые ресурсы в облаке, доступ к которым можно получить с помощью стандартного отраслевого протокола SMB. В этой статье демонстрируется практическое развертывание службы файлов Azure в вашей организации.

Перед выполнением действий в этой статье настоятельно рекомендуется прочесть статью [Planning for an Azure Files deployment](storage-files-planning.md) (Планирование развертывания службы файлов Azure).

## <a name="prerequisites"></a>Предварительные требования
В данной статье предполагается, что вы уже выполнили следующие действия:

- Создали учетную запись хранения Azure с необходимыми параметрами устойчивости шифрования в нужном регионе. Пошаговые инструкции по созданию учетной записи хранения см. в разделе о [создании учетной записи хранения](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
- Создайте общую папку Azure с требуемой квотой в учетной записи хранения. Пошаговые инструкции по созданию файлового ресурса см. в разделе о [создании файлового ресурса](storage-how-to-create-file-share.md).

## <a name="transfer-data-into-azure-files"></a>Передача данных в службу файлов Azure
Вы можете перенести существующие общие файловые ресурсы, например хранящиеся локально, в новую общую папку Azure. В этом разделе показано, как переместить данные в общую папку Azure с помощью нескольких популярных методов, подробных от [руководства по планированию](storage-files-planning.md#migration) .

### <a name="azure-file-sync"></a>Синхронизация файлов Azure
Служба "Синхронизация файлов Azure" позволяет централизованно хранить файловые ресурсы организации в службе файлов Azure, обеспечивая гибкость, производительность и совместимость локального файлового сервера. Это достигается путем преобразования серверов Windows Server в быстрый кэш общей папки Azure. Для локального доступа к данным вы можете использовать любой протокол (в том числе SMB, NFS и FTPS), доступный в Windows Server. Кроме того, вы можете создать любое число кэшей в любом регионе.

Службу "Синхронизация файлов Azure" можно использовать для переноса данных в общую папку Azure, даже если для долгосрочного использования не требуется механизм синхронизации. Дополнительные сведения об использовании службы "Синхронизация файлов Azure" для передачи данных в общую папку Azure можно найти в статьях [Планирование развертывания службы синхронизации файлов Azure (предварительная версия)](storage-sync-files-planning.md) и [Развертывание службы синхронизации файлов Azure (предварительная версия)](storage-sync-files-deployment-guide.md).

### <a name="azure-importexport"></a>Импорт и экспорт Microsoft Azure
Служба "Импорт и экспорт Azure" позволяет безопасно переносить большие объемы данных в общую папку Azure, отправляя жесткие диски в центр обработки данных Azure. Более подробный обзор службы см. в статье [Использование службы "Импорт и экспорт Microsoft Azure" для передачи данных в хранилище BLOB-объектов](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

> [!Note]  
> Сейчас служба импорта и экспорта Azure не поддерживает экспорт файлов из общей папки Azure.

Следующие шаги используются для импорта данных из локального расположения в общую папку Azure.

1. Получите необходимое количество жестких дисков для отправки в Azure. Жесткие диски могут иметь любой размер, но это должен быть диск SSD или HDD 2,5" или 3,5", поддерживающий стандарт SATA II или SATA III. 

2. Присоедините и подключите каждый диск к серверу или ПК, выполняющему передачу данных. Для обеспечения оптимальной производительности рекомендуется выполнять задание локального экспорта локально на сервере, который содержит данные. В некоторых случаях это может оказаться невозможным, например если файловый сервер, обрабатывающий данные, является устройством NAS. Тогда вполне приемлемым вариантом будет подключение каждого диска к ПК.

3. Убедитесь, что каждый диск находится в оперативном режиме, инициализирован и имеет назначенную букву диска. Чтобы перевести диск в оперативный режим, выполнить инициализацию и назначить букву диска, откройте оснастку MMC для управления дисками (diskmgmt.msc).

    - Чтобы перевести диск в оперативный режим (если это еще не сделано), щелкните правой кнопкой мыши диск в нижней области консоли MMC для управления дисками и выберите пункт "В сети".
    - Чтобы инициализировать диск, щелкните правой кнопкой мыши диск в нижней области (после перевода диска в оперативный режим) и выберите пункт "Инициализировать". При запросе выберите "GPT".

        ![Снимок экрана меню инициализации диска в консоли MMC управления дисками](media/storage-files-deployment-guide/transferdata-importexport-1.PNG)

    - Чтобы назначить диску букву диска, щелкните правой кнопкой мыши невыделенное пространство инициализированного диска в оперативном режиме и выберите пункт "Создать простой том". Теперь вы сможете назначить букву диска. Обратите внимание, что том форматировать не нужно — это будет сделано позже.

        ![Снимок экрана мастера создания простого тома в консоли MMC управления дисками](media/storage-files-deployment-guide/transferdata-importexport-2.png)

4. Создайте CSV-файл набора данных. CSV-файл набора данных представляет собой сопоставление пути к данным в локальной среде с нужной общей папкой Azure, в которую должны быть скопированы данные. Например, следующий CSV-файл набора данных сопоставляет локальный файловый ресурс ("F:\shares\scratch") с общей папкой Azure ("MyAzureFileShare"):
    
    ```
    BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
    "F:\shares\scratch\","MyAzureFileShare/",file,rename,"None",None
    ```

    С помощью учетной записи хранения можно указать несколько общих ресурсов. Дополнительные сведения см. в разделе [Подготовка CSV-файла набора данных](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

5. Создайте CSV-файл набора дисков. CSV-файл набора дисков содержит перечень дисков, доступных агенту локального экспорта. Например, следующий CSV-файл набора дисков перечисляет диски `X:`, `Y:` и `Z:`, которые будут использоваться в задании локального экспорта:

    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    X,Format,SilentMode,Encrypt,
    Y,Format,SilentMode,Encrypt,
    Z,Format,SilentMode,Encrypt,
    ```
    
    Дополнительные сведения см. в статье [Подготовка CSV-файла набора дисков](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

6. Используйте [средство WAImportExport](https://www.microsoft.com/download/details.aspx?id=55280) для копирования данных на один или несколько жестких дисков.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
    ```

    > [!Warning]  
    > Не изменяйте данные на жестких дисках или в файлах журналов после завершения подготовки этих дисков.

7. [Создайте задание импорта](../common/storage-import-export-data-to-files.md#step-2-create-an-import-job).
    
### <a name="robocopy"></a>Robocopy
Robocopy — это хорошо известное средство копирования, которое входит в состав Windows и Windows Server. Robocopy можно применять для передачи данных в службу файлов Azure путем локального подключения файлового ресурса и последующего использования подключенного расположения в качестве назначения в команде Robocopy. Использование Robocopy не представляет сложностей:

1. [Подключите файловый ресурс Azure](storage-how-to-use-files-windows.md). Для обеспечения оптимальной производительности рекомендуется локально подключить общую папку Azure к серверу, который содержит данные. В некоторых случаях это может оказаться невозможным, например если файловый сервер, обрабатывающий данные, является устройством NAS. Тогда вполне приемлемым вариантом будет подключение общей папки Azure к ПК. В этом примере для подключения общего файлового ресурса в командной строке используется `net use`:

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. Используйте `robocopy` в командной строке, чтобы перенести данные в общую папку Azure.

    ```
    robocopy <path-to-local-share> <path-to-azure-file-share> /E /Z /MT:32
    ```
    
    Robocopy располагает значительным количеством параметров для изменения поведения копирования. Дополнительные сведения см. на странице руководства по [Robocopy](https://technet.microsoft.com/library/cc733145.aspx).

### <a name="azcopy"></a>AzCopy
AzCopy — это служебная программа командной строки. Она предназначена для копирования данных из службы файлов Azure, хранилища BLOB-объектов Azure и обратно с помощью простых команд, обеспечивающих оптимальную производительность. Программа AzCopy проста в использовании:

1. Скачайте [последнюю версию AzCopy для Windows](https://aka.ms/downloadazcopy) или [Linux](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux#download-and-install-azcopy).
2. Используйте `azcopy` в командной строке, чтобы перенести данные в общую папку Azure. В Windows используется следующий синтаксис: 

    ```
    azcopy /Source:<path-to-local-share> /Dest:https://<storage-account>.file.core.windows.net/<file-share>/ /DestKey:<storage-account-key> /S
    ```

    В Linux синтаксис команды немного отличается:

    ```
    azcopy --source <path-to-local-share> --destination https://<storage-account>.file.core.windows.net/<file-share>/ --dest-key <storage-account-key> --recursive
    ```

    AzCopy располагает значительным количеством параметров для изменения поведения копирования. Дополнительные сведения см. в статьях об [AzCopy для Windows](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) и [AzCopy для Linux](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="automatically-mount-on-needed-pcsservers"></a>Автоматическое подключение к необходимым ПК или серверам
Чтобы заменить локальный файловый ресурс, рекомендуется предварительно подключить его к компьютерам, на которых он будет использоваться. Это можно сделать автоматически для списка компьютеров.

> [!Note]  
> При подключении общей папки Azure необходимо использовать ключ учетной записи хранения в качестве пароля, поэтому подключение следует выполнять только в доверенных средах. 

### <a name="windows"></a>Windows
Для выполнения команды подключения к нескольким ПК можно использовать PowerShell. В следующем примере `$computers` заполняется вручную, но можно создать список компьютеров для автоматического подключения. Например, эту переменную можно заполнить результатами из Active Directory.

```powershell
$computer = "MyComputer1", "MyComputer2", "MyComputer3", "MyComputer4"
$computer | ForEach-Object { Invoke-Command -ComputerName $_ -ScriptBlock { net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name> /PERSISTENT:YES } }
```

### <a name="linux"></a>Linux
Простой скрипт bash в сочетании с SSH может дать тот же результат, как показано в следующем примере. Переменная `$computer` оставляется для заполнения пользователем:

```
computer = ("MyComputer1" "MyComputer2" "MyComputer3" "MyComputer4")
for item in "${computer[@]}"
do
    ssh $item "sudo bash -c 'echo \"//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino\" >> /etc/fstab'", "sudo mount -a"
done
```

## <a name="next-steps"></a>Дальнейшие действия
- [Планирование развертывания службы синхронизации файлов Azure](storage-sync-files-planning.md)
- [Устранение неполадок в работе службы "Файлы Azure" в Windows](storage-troubleshoot-windows-file-connection-problems.md)
- [Устранение неполадок в работе службы "Файлы Azure" в Linux](storage-troubleshoot-linux-file-connection-problems.md)
