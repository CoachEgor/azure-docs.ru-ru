---
title: Руководство по распаковке, подключению и разблокировке Диска Azure Data Box | Документация Майкрософт
description: Используйте это руководство, чтобы узнать, как настроить диск Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 08/28/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 15960c39878e0c8ae22f94197ed1937597f1ee02
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126126"
---
::: zone target="docs"

# <a name="tutorial-unpack-connect-and-unlock-azure-data-box-disk"></a>Руководство по Распаковка, подключение и разблокировка Диска Azure Data Box

Это руководство описывает, как распаковать, подключить и разблокировать диск Azure Data Box.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Распаковка диска Data Box.
> * Подключение к дискам и получение ключа доступа
> * Разблокировка дисков на клиенте Windows
> * Разблокировка дисков на клиенте Linux

::: zone-end

::: zone target="chromeless"

## <a name="unpack-connect-and-unlock-azure-data-box-disk"></a>Распаковка, подключение и разблокировка Диска Azure Data Box

::: zone-end

::: zone target="docs"

## <a name="prerequisites"></a>Предварительные требования

Перед тем как начать, убедитесь в следующем.

1. Вы завершили [руководство Заказ Диска Azure Data Box (предварительная версия)](data-box-disk-deploy-ordered.md).
2. Состояние ваших дисков и заданий на портале обновилось до **Доставлено**.
3. У вас есть клиентский компьютер, на котором можно установить средство разблокировки диска Data Box. На клиентском компьютере должны быть установлены:
    - [поддерживаемая операционная система](data-box-disk-system-requirements.md#supported-operating-systems-for-clients);
    - другое [необходимое программное обеспечение](data-box-disk-system-requirements.md#other-required-software-for-windows-clients), если это клиент Windows.  

## <a name="unpack-your-disks"></a>Распаковка дисков

 Выполните следующие действия для распаковки дисков.

1. Диски Data Box отправляются в небольшой транспортировочной коробке. Откройте коробку и удалите ее содержимое. Убедитесь, что в коробке имеется 1–5 твердотельных дисков (SSD) и соединительный кабель USB для каждого диска. Осмотрите коробку на наличие каких-либо доказательств незаконного изменения или любых других очевидных повреждений. 

    ![Транспортировочная упаковка диска Data Box](media/data-box-disk-deploy-set-up/data-box-disk-ship-package1.png)

2. Если транспортировочная коробка имеет следы незаконного изменения или серьезно повреждена, не открывайте ее. Обратитесь в службу поддержки Майкрософт, чтобы она помогла оценить, находятся ли диски в хорошем состоянии или их необходимо заменить.
3. Убедитесь, что коробка имеет прозрачную упаковку, содержащую транспортную этикетку (в соответствие с действующей маркировкой) для обратной отправки. Если эта этикетка утеряна или повреждена, всегда можно загрузить и распечатать новую с портала Azure. 

    ![Транспортировочная этикетка диска Data Box](media/data-box-disk-deploy-set-up/data-box-disk-package-ship-label.png)

4. Сохраните коробку и упаковочный пеноматериал на случай обратной отправки дисков.

## <a name="connect-to-disks-and-get-the-passkey"></a>Подключение к дискам и получение ключа доступа 

1. Используйте прилагаемый кабель для подключения диска к клиентскому компьютеру с поддерживаемой ОС, как указано в предварительных требованиях. 

    ![Подключение диска Data Box](media/data-box-disk-deploy-set-up/data-box-disk-connect-unlock.png)    
    
2. На портале Azure перейдите в раздел **Общие > Сведения об устройстве**. Используйте значок копирования, чтобы скопировать ключ доступа. Этот ключ доступа будет использоваться для разблокировки дисков.

    ![Ключ доступа для разблокировки диска Data Box](media/data-box-disk-deploy-set-up/data-box-disk-get-passkey.png) 

В зависимости от того, подключены ли вы к клиенту Windows или Linux, действия по разблокировке дисков будут отличаться.

## <a name="unlock-disks-on-windows-client"></a>Разблокировка дисков на клиенте Windows

Выполните следующие действия для подключения и разблокировки дисков.
     
1. На портале Azure перейдите в раздел **Общие > Сведения об устройстве**. 
2. Скачайте набор средств для диска Data Box, соответствующий клиенту Windows. Этот набор средств содержит 3 средства: разблокировки, проверки и раздельного копирования Диска Data Box. 

    В этой процедуре используется только средство разблокировки дисков. Два других средства будут использованы позднее.

    > [!div class="nextstepaction"]
    > [Скачать набор средств для диска Data Box для Windows](https://aka.ms/databoxdisktoolswin)         

3. Извлеките набор средств на тот же компьютер, который будет использоваться для копирования данных. 
4. Откройте окно командной строки или запустите Windows PowerShell от имени администратора на том же компьютере.
5. (Необязательно). Чтобы убедиться, что компьютер, используемый для разблокировки диска, соответствует требованиям операционной системы, выполните команду проверки системы. Результат выполнения команды показан ниже. 

    ```powershell
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /SystemCheck
    Successfully verified that the system can run the tool.
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ``` 

6. Запустите `DataBoxDiskUnlock.exe` и укажите ключ доступа, полученный в разделе [Подключение к дискам и получение ключа доступа](#connect-to-disks-and-get-the-passkey). Отобразится буква диска, присвоенная диску. Результат выполнения команды показан ниже.

    ```powershell
    PS C:\WINDOWS\system32> cd C:\DataBoxDiskUnlockTool\DiskUnlock
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe
    Enter the passkey :
    testpasskey1
    
    Following volumes are unlocked and verified.
    Volume drive letters: D:
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```

7. Повторяйте шаги по разблокировке при любых будущих повторных вставках диска. Воспользуйтесь командой `help`, если требуется помощь со средством разблокировки диска Data Box.   

    ```powershell
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /help
    USAGE:
    DataBoxUnlock /PassKey:<passkey_from_Azure_portal>
    
    Example: DataBoxUnlock /PassKey:<your passkey>
    Example: DataBoxUnlock /SystemCheck
    Example: DataBoxUnlock /Help
    
    /PassKey:        Get this passkey from Azure DataBox Disk order. The passkey unlocks your disks.
    /SystemCheck:    This option checks if your system meets the requirements to run the tool.
    /Help:           This option provides help on cmdlet usage and examples.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```  
8. Когда диск будет разблокирован, можно просмотреть содержимое диска.    

    ![Содержимое диска Data Box](media/data-box-disk-deploy-set-up/data-box-disk-content.png)

Если вы столкнетесь с проблемами при разблокировке дисков, ознакомьтесь со сведениями об [устранении неполадок при разблокировке](data-box-disk-troubleshoot-unlock.md). 

## <a name="unlock-disks-on-linux-client"></a>Разблокировка дисков на клиенте Linux

1. На портале Azure перейдите в раздел **Общие > Сведения об устройстве**. 
2. Скачайте набор средств для диска Data Box, соответствующий клиенту Linux.  

    > [!div class="nextstepaction"]
    > [Скачать набор средств для диска Data Box для Linux](https://aka.ms/databoxdisktoolslinux) 

3. На клиенте Linux откройте терминал. Перейдите в папку со скачанным программным обеспечением. Измените разрешения для файлов, чтобы можно было их выполнить. Введите следующую команду: 

    `chmod +x DataBoxDiskUnlock_x86_64` 
    
    `chmod +x DataBoxDiskUnlock_Prep.sh` 
 
    Результат выполнения команды показан ниже. Запустив команду chmod, убедитесь, что разрешения файлов изменены, выполнив команду `ls`. 
 
    ```
        [user@localhost Downloads]$ chmod +x DataBoxDiskUnlock_x86_64  
        [user@localhost Downloads]$ chmod +x DataBoxDiskUnlock_Prep.sh   
        [user@localhost Downloads]$ ls -l  
        -rwxrwxr-x. 1 user user 1152664 Aug 10 17:26 DataBoxDiskUnlock_x86_64  
        -rwxrwxr-x. 1 user user 795 Aug 5 23:26 DataBoxDiskUnlock_Prep.sh
    ```
4. Выполните скрипт, чтобы он установил все двоичные файлы, необходимые программному обеспечению для разблокировки диска Data Box. Используйте `sudo` для выполнения команды в качестве привилегированного пользователя (root). После успешной установки двоичных файлов вы увидите примечание об этом в окне терминала.

    `sudo ./DataBoxDiskUnlock_Prep.sh`

    Скрипт сначала проверит, работает ли на клиентском компьютере поддерживаемая операционная система. Результат выполнения команды показан ниже. 
 
    ```
    [user@localhost Documents]$ sudo ./DataBoxDiskUnlock_Prep.sh 
        OS = CentOS Version = 6.9 
        Release = CentOS release 6.9 (Final) 
        Architecture = x64 
    
        The script will install the following packages and dependencies. 
        epel-release 
        dislocker 
        ntfs-3g 
        fuse-dislocker 
        Do you wish to continue? y|n :|
    ```
    
 
5. Введите `y`, чтобы продолжить установку. Скрипт устанавливает следующие пакеты: 
   - **epel-release** — репозиторий, содержащий следующие три пакета. 
   - **dislocker и fuse-dislocker** — эти служебные программы помогают расшифровывать зашифрованные диски BitLocker. 
   - **ntfs-3g** — пакет, который помогает подключать тома с файловой системой NTFS. 
 
     После успешной установки пакетов в терминале отобразится соответствующее уведомление.     
     ```
     Dependency Installed: compat-readline5.x86 64 0:5.2-17.I.el6 dislocker-libs.x86 64 0:0.7.1-8.el6 mbedtls.x86 64 0:2.7.4-l.el6        ruby.x86 64 0:1.8.7.374-5.el6 
     ruby-libs.x86 64 0:1.8.7.374-5.el6 
     Complete! 
     Loaded plugins: fastestmirror, refresh-packagekit, security 
     Setting up Remove Process 
     Resolving Dependencies 
     --> Running transaction check 
     ---> Package epel-release.noarch 0:6-8 will be erased --> Finished Dependency Resolution 
     Dependencies Resolved 
     Package        Architecture        Version        Repository        Size 
     Removing:  epel-release        noarch         6-8        @extras        22 k 
     Transaction Summary                                 
     Remove        1 Package(s) 
     Installed size: 22 k 
     Downloading Packages: 
     Running rpmcheckdebug 
     Running Transaction Test 
     Transaction Test Succeeded 
     Running Transaction 
     Erasing : epel-release-6-8.noarch 
     Verifying : epel-release-6-8.noarch 
     Removed: 
     epel-release.noarch 0:6-8 
     Complete! 
     Dislocker is installed by the script. 
     OpenSSL is already installed.
     ```

6. Запустите средство разблокировки диска Data Box. Укажите ключ доступа с портала Azure, полученный в разделе [Подключение к дискам и получение ключа доступа](#connect-to-disks-and-get-the-passkey). При необходимости укажите список томов, зашифрованных BitLocker, чтобы разблокировать их. Ключ доступа и список томов следует указать в одинарных кавычках. 

    Введите следующую команду.
 
    "sudo ./DataBoxDiskUnlock_x86_64 /PassKey:<Your passkey from Azure portal>".          

    Пример выходных данных показан ниже. 
 
    ```
    [user@localhost Downloads]$ sudo ./DataBoxDiskUnlock_x86_64 /Passkey:’qwerqwerqwer’  
    
    START: Mon Aug 13 14:25:49 2018 
    Volumes: /dev/sdbl 
    Passkey: qwerqwerqwer 
    
    Volumes for data copy : 
    /dev/sdbl: /mnt/DataBoxDisk/mountVoll/ 
    END: Mon Aug 13 14:26:02 2018
    ```
    Отображается точка подключения тома, на который можно скопировать данные.

7. Повторяйте шаги по разблокировке диска при его будущих подключениях. Воспользуйтесь командой `help`, если требуется помощь со средством разблокировки диска Data Box. 
    
    `sudo ./DataBoxDiskUnlock_x86_64 /Help` 

    Пример выходных данных показан ниже. 
 
    ```
    [user@localhost Downloads]$ sudo ./DataBoxDiskUnlock_x86_64 /Help  
    START: Mon Aug 13 14:29:20 2018 
    USAGE: 
    sudo DataBoxDiskUnlock /PassKey:’<passkey from Azure_portal>’ 
    
    Example: sudo DataBoxDiskUnlock /PassKey:’passkey’ 
    Example: sudo DataBoxDiskUnlock /PassKey:’passkey’ /Volumes:’/dev/sdbl’ 
    Example: sudo DataBoxDiskUnlock /Help Example: sudo DataBoxDiskUnlock /Clean 
    
    /PassKey: This option takes a passkey as input and unlocks all of your disks. 
    Get the passkey from your Data Box Disk order in Azure portal. 
    /Volumes: This option is used to input a list of BitLocker encrypted volumes. 
    /Help: This option provides help on the tool usage and examples. 
    /Unmount: This option unmounts all the volumes mounted by this tool. 
   
    END: Mon Aug 13 14:29:20 2018 [user@localhost Downloads]$
    ```
    
8. Когда диск будет разблокирован, можно перейти к точке подключения и просмотреть содержимое диска. Теперь все готово для копирования данных в папки *BlockBlob* или *PageBlob*. 

    ![Содержимое диска Data Box](media/data-box-disk-deploy-set-up/data-box-disk-content-linux.png)


Если вы столкнетесь с проблемами при разблокировке дисков, ознакомьтесь со сведениями об [устранении неполадок при разблокировке](data-box-disk-troubleshoot-unlock.md). 

::: zone-end

::: zone target="chromeless"

1. Распакуйте диски и используйте прилагаемый кабель, чтобы подключить диск к клиентскому компьютеру.
2. Загрузите и извлеките набор средств Data Box Disk на тот же компьютер, который будет использоваться для копирования данных.

    > [!div class="nextstepaction"]
    > [Скачать набор средств для диска Data Box для Windows](https://aka.ms/databoxdisktoolswin)

    или
    > [!div class="nextstepaction"]
    > [Скачать набор средств для диска Data Box для Linux](https://aka.ms/databoxdisktoolslinux) 

3. Чтобы разблокировать диски на клиенте Windows, откройте окно командной строки или запустите Windows PowerShell от имени администратора на том же компьютере.

    - Введите следующую команду в той же папке, где установлено средство разблокировки Data Box Disk.

        ``` 
        .\DataBoxDiskUnlock.exe
        ```
    -  Получите ключ доступа, выбрав **Общие сведения > Сведения об устройстве** на портале Azure, и укажите его здесь. Отобразится буква диска, присвоенная диску. 
4. Откройте терминал, чтобы разблокировать диски на клиенте Linux. Перейдите в папку со скачанным программным обеспечением. Введите следующие команды для изменения разрешения для файлов, чтобы можно было их выполнить. 

    ```
    chmod +x DataBoxDiskUnlock_x86_64
    chmod +x DataBoxDiskUnlock_Prep.sh
    ``` 
    Выполните скрипт, чтобы установить все необходимые двоичные файлы.

    ```
    sudo ./DataBoxDiskUnlock_Prep.sh
    ```
    Запустите средство разблокировки диска Data Box. Получите ключ доступа, выбрав **Общие сведения > Сведения об устройстве** на портале Azure, и укажите его здесь. При необходимости укажите список томов, зашифрованных BitLocker, в одинарных кавычках, чтобы разблокировать их.

    ```
    sudo ./DataBoxDiskUnlock_x86_64 /PassKey:’<Your passkey from Azure portal>’
    ```      
5. Повторяйте шаги по разблокировке при любых будущих повторных вставках диска. Используйте команду справки, если требуется помощь со средством разблокировки диска Data Box.

Когда диск будет разблокирован, можно просмотреть содержимое диска.

Дополнительные сведения о том, как настроить диски, см. в разделе [Распаковка Диска Data Box](data-box-disk-deploy-set-up.md#unpack-your-disks).

Дополнительные сведения о том, как разблокировать диски, см. в разделе [Разблокировка дисков на клиенте Windows](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client).

::: zone-end

::: zone target="docs"

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве раскрыты следующие сведения о диске Azure Data Box.

> [!div class="checklist"]
> * Распаковка диска Data Box.
> * Подключение к дискам и получение ключа доступа
> * Разблокировка дисков на клиенте Windows
> * Разблокировка дисков на клиенте Linux


Перейдите к следующему руководству, чтобы узнать, как копировать данные на диск Data Box.

> [!div class="nextstepaction"]
> [Tutorial: Copy data to Azure Data Box Disk and verify](./data-box-disk-deploy-copy-data.md) (Руководство. Копирование данных на диск Data Box)

::: zone-end

