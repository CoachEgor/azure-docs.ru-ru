---
title: Сохранение файлов для Bash в Azure Cloud Shell | Документация Майкрософт
description: Пошаговое руководство по сохранению файлов Bash в Azure Cloud Shell.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.author: damaerte
ms.openlocfilehash: 0aa00af543a3d21db9b8ad0ed808a8bff0b534e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60200273"
---
[!INCLUDE [PersistingStorage-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-cloud-shell-storage-works"></a>Как работает хранилище Cloud Shell 
Cloud Shell сохраняет файлы с помощью обоих приведенных ниже методов. 
* Создание образа диска для каталога `$Home` для хранения всего содержимого этого каталога. Этот образ диска сохраняется как `acc_<User>.img` в `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, и для него выполняется автоматическая синхронизация изменений. 
* Подключение выбранного файлового ресурса как `clouddrive` в каталоге `$Home`, чтобы взаимодействовать с ним напрямую. `/Home/<User>/clouddrive` сопоставляется с `fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Все файлы в каталоге `$Home`, такие как ключи SSH, сохраняются в образе диска пользователя, который хранится в подключенном файловом ресурсе. Соблюдайте рекомендации при сохранении информации в каталоге `$Home` и подключенном файловом ресурсе.

## <a name="bash-specific-commands"></a>Команды для Bash

### <a name="use-the-clouddrive-command"></a>Использование команды `clouddrive`
С помощью Bash в Cloud Shell можно выполнить команду `clouddrive`, что позволит вручную обновить файловый ресурс, подключенный к Cloud Shell.
![Выполнение команды clouddrive](media/persisting-shell-storage/clouddrive-h.png)

### <a name="mount-a-new-clouddrive"></a>Подключение нового облачного диска

#### <a name="prerequisites-for-manual-mounting"></a>Предварительные требования для подключения вручную
Можно обновить файловый ресурс, который связан с Cloud Shell, с помощью команды `clouddrive mount`.

При подключении имеющегося файлового ресурса учетные записи хранения должны находится в выбранном регионе Cloud Shell. Чтобы получить расположение, выполните `env` в Bash и проверьте наличие переменной `ACC_LOCATION`.

#### <a name="the-clouddrive-mount-command"></a>Команда `clouddrive mount`

> [!NOTE]
> При подключении нового файлового ресурса создается пользовательский образ вашего каталога `$Home`. Предыдущий образ `$Home` сохраняется в предыдущем файловом ресурсе.

Выполните команду `clouddrive mount` со следующими параметрами:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Чтобы просмотреть дополнительные сведения, выполните `clouddrive mount -h`, как показано ниже.

![Выполнение команды clouddrive mount](media/persisting-shell-storage/mount-h.png)

### <a name="unmount-clouddrive"></a>Отключение облачного диска
Файловый ресурс, подключенный к Cloud Shell, можно отключить в любое время. Так как для Cloud Shell требуется подключенный файловый ресурс, вам будет предложено создать и подключить другой файловый ресурс в следующем сеансе.

1. Запустите `clouddrive unmount`.
2. Подтвердите запросы.

Файловый ресурс будет существовать до его удаления вручную. Cloud Shell больше не будет искать эту общую папку для последующих сеансов. Чтобы просмотреть дополнительные сведения, выполните `clouddrive unmount -h`, как показано ниже.

![Выполнение команды clouddrive unmount](media/persisting-shell-storage/unmount-h.png)

> [!WARNING]
> Хотя при выполнении этой команды ресурсы не удаляются, ручное удаление группы ресурсов, учетной записи хранения или файлового ресурса, сопоставленного с Cloud Shell, приведет к удалению образа диска каталога `$Home` и всех файлов в файловом ресурсе. Это действие невозможно отменить.

### <a name="list-clouddrive"></a>Вывод объектов `clouddrive`
Чтобы узнать, какой файловый ресурс подключен как `clouddrive`, выполните команду `df`. 

В пути к каталогу clouddrive указано имя учетной записи хранения и файловый ресурс в URL-адресе. Например: `//storageaccountname.file.core.windows.net/filesharename`

```
justin@Azure:~$ df
Filesystem                                          1K-blocks   Used  Available Use% Mounted on
overlay                                             29711408 5577940   24117084  19% /
tmpfs                                                 986716       0     986716   0% /dev
tmpfs                                                 986716       0     986716   0% /sys/fs/cgroup
/dev/sda1                                           29711408 5577940   24117084  19% /etc/hosts
shm                                                    65536       0      65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName 5368709120    64 5368709056   1% /home/justin/clouddrive
justin@Azure:~$
```
## <a name="powershell-specific-commands"></a>Команды для PowerShell

### <a name="list-clouddrive-azure-file-shares"></a>Выведение списка общих файловых ресурсов Azure `clouddrive`
Командлет `Get-CloudDrive` позволяет извлечь сведения о файловых ресурсах Azure, которые сейчас подключены к `clouddrive` в Cloud Shell. <br>
![Выполнение Get-CloudDrive](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

### <a name="unmount-clouddrive"></a>Отключение `clouddrive`
Файловый ресурс Azure, подключенный к Cloud Shell, можно отключить в любое время. В случае удаления файлового ресурса Azure вам будет предложено создать и подключить файловый ресурс Azure в следующем сеансе.

Командлет `Dismount-CloudDrive` позволяет отключить файловый ресурс Azure в текущей учетной записи хранения. Отключение `clouddrive` приводит к завершению текущего сеанса. Пользователю будет предложено создать и подключить файловый ресурс Azure во время следующего сеанса.
![Выполнение Dismount-CloudDrive](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [PersistingStorage-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

Примечание. Если вам нужно определить функцию в файле и вызвать ее с помощью командлетов PowerShell, необходимо добавить оператор "точка". Например: .\MyFunctions.ps1

## <a name="next-steps"></a>Дальнейшие действия
[Краткое руководство по Bash в Cloud Shell](quickstart.md) <br>
[Краткое руководство по использованию PowerShell в Azure Cloud Shell (предварительная версия)](quickstart-powershell.md) <br>
[Сведения о хранилище файлов Microsoft Azure](https://docs.microsoft.com/azure/storage/storage-introduction) <br>
[Использование тегов для организации ресурсов в Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
