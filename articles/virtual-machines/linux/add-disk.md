---
title: Добавление диска данных в виртуальную машину Linux с помощью Azure CLI
description: Узнайте, как добавить постоянный диск данных в виртуальную машину Linux с помощью Azure CLI.
author: roygara
manager: twooley
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 06/13/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: a80a1fe21ba0b40aebf9e426e3d49f499c2d2a21
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "78967983"
---
# <a name="add-a-disk-to-a-linux-vm"></a>Добавление диска к виртуальной машине Linux
Из этой статьи вы узнаете, как добавить в виртуальную машину постоянный диск, на котором можно хранить данные. Эти данные сохранятся даже после повторной подготовки виртуальной машины (например, в ходе обслуживания или изменения размера).


## <a name="attach-a-new-disk-to-a-vm"></a>Подключение нового диска к виртуальной машине

Если вам нужен новый пустой диск с данными в виртуальной машине, то используйте команду [az vm disk attach](/cli/azure/vm/disk?view=azure-cli-latest) с параметром `--new`. Если виртуальная машина находится в зоне доступности, то диск автоматически создается в одной зоне с виртуальной машиной. Дополнительные сведения см. в статье [Общие сведения о зонах доступности в Azure (предварительная версия)](../../availability-zones/az-overview.md). В следующем примере создается диск с именем *myDataDisk* размером 50 ГБ:

```azurecli
az vm disk attach \
   -g myResourceGroup \
   --vm-name myVM \
   --name myDataDisk \
   --new \
   --size-gb 50
```

## <a name="attach-an-existing-disk"></a>Подключение существующего диска

Чтобы подключить существующий диск, найдите идентификатор диска и укажите его в команде [az vm disk attach](/cli/azure/vm/disk?view=azure-cli-latest). В следующем примере выполняется запрос диска *myDataDisk* в группе ресурсов *myResourceGroup*, а затем этот диск подключается к виртуальной машине *myVM*:

```azurecli
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)

az vm disk attach -g myResourceGroup --vm-name myVM --name $diskId
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Подключение к виртуальной машине Linux для подключения нового диска

Чтобы разбить диск на разделы, отформатировать и подключить новый диск к виртуальной машине Linux, подключитесь к своей виртуальной машине по протоколу SSH. Дополнительные сведения см. в статье [Как использовать SSH с Linux в Azure](mac-create-ssh-keys.md). В следующем примере выполняется подключение к виртуальной машине с помощью общедоступной записи DNS *mypublicdns.westus.cloudapp.azure.com* и имени пользователя *azureuser*:

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

После подключения к виртуальной машине можно подключить диск. Сначала найдите диск, используя `dmesg` (для обнаружения нового диска можно использовать и другой способ). В следующем примере используется команда dmesg для фильтрации по дискам *SCSI*:

```bash
dmesg | grep SCSI
```

Вы должны увидеть результат, аналогичный приведенному ниже.

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

> [!NOTE]
> Рекомендуется использовать последние версии fdisk или частично, доступные для вашего дистрибутив.

В данном примере *sdc* — это диск, который нам нужен. Создайте разделы на диске с помощью `parted`. Если размер диска составляет 2 ТиБ или больше, необходимо использовать формат разделов GPT. Если размер диска не превышает 2 ТиБ, можно использовать формат разделов MBR или GPT. Если вы используете секционирование MBR, можно использовать `fdisk`. Установите этот диск как основной диск для раздела 1 и примите остальные значения по умолчанию. В следующем примере запускается процесс `fdisk` в */dev/sdc*:

```bash
sudo fdisk /dev/sdc
```

Используйте команду `n`, чтобы добавить новый раздел. В этом примере мы выбираем `p` для основного раздела и принимаем остальные значения по умолчанию. Результат будет выглядеть примерно так:

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Распечатайте таблицу разделов, введя `p`, а затем используйте `w`, чтобы записать таблицу на диск и выйти. Результат должен выглядеть примерно следующим образом:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```
Используйте приведенную ниже команду, чтобы обновить ядро:
```
partprobe 
```

Теперь запишите файловую систему в раздел, выполнив команду `mkfs`. Укажите тип файловой системы и имя устройства. В следующем примере создается файловая система *ext4* в разделе */dev/sdc1*, созданном на предыдущем шаге:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Вы должны увидеть результат, аналогичный приведенному ниже.

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Теперь создайте каталог для подключения файловой системы, используя `mkdir`. В следующем примере создается каталог в */datadrive*:

```bash
sudo mkdir /datadrive
```

Используйте `mount`, чтобы затем подключить файловую систему. В следующем примере раздел */dev/sdc1* подключается к точке подключения */datadrive*:

```bash
sudo mount /dev/sdc1 /datadrive
```

Чтобы обеспечить автоматическое повторное подключение диска после перезагрузки, его необходимо добавить в файл */etc/fstab*. Также для ссылки на диск настоятельно рекомендуется использовать в файле */etc/fstab* идентификатор UUID (глобальный уникальный идентификатор), а не просто имя устройства (например, */dev/sdc1*). Если операционная система обнаруживает ошибку диска во время загрузки, использование UUID позволяет избежать подключения ошибочного диска в это расположение. Остальные диски с данными затем получают те же идентификаторы устройств. Чтобы найти UUID нового диска, используйте служебную программу `blkid`:

```bash
sudo blkid
```

Результат должен быть аналогичным приведенному ниже:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> Некорректное изменение файла **/etc/fstab** может привести к невозможности загрузить систему. Если у вас есть сомнения, см. инструкции по правильному изменению этого файла в документации дистрибутива. Также рекомендуется перед внесением изменений создать резервную копию файла /etc/fstab.

Затем откройте файл */etc/fstab* в текстовом редакторе, как показано ниже:

```bash
sudo vi /etc/fstab
```

В этом примере используйте значение UUID для устройства */dev/sdc1*, созданного на предыдущих шагах, и точку подключения */datadrive*. Добавьте следующую строку в конец файла */etc/fstab* :

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Если вы позднее удалите диск данных без редактирования файла fstab, виртуальная машина может не загрузиться. Большинство дистрибутивов поддерживает параметры fstab *nofail* и (или) *nobootwait*. Эти параметры позволяют системе загружаться, даже если диск не подключится во время загрузки. Дополнительные сведения об этих параметрах см. в документации дистрибутива.
>
> Параметр *nofail* обеспечивает запуск виртуальной машины даже в том случае, если файловая система повреждена или отсутствует диск во время загрузки. Без этого параметра может возникнуть ситуация, описанная в записи блога [Cannot SSH to Linux VM due to FSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/) (Не удается подключиться к виртуальной машине Linux по протоколу SSH из-за ошибок FSTAB).
>
> Последовательная консоль виртуальной машины Azure может использоваться для доступа к виртуальной машине через консоль, если изменение fstab привело к сбою загрузки. Дополнительные сведения см. в [документации по последовательной консоли](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux).

### <a name="trimunmap-support-for-linux-in-azure"></a>Поддержка операций TRIM и UNMAP для Linux в Azure
Некоторые ядра Linux поддерживают операции TRIM и UNMAP для отмены неиспользуемых блоков на диске. Эту функцию особенно удобно использовать в хранилище уровня "Стандартный", чтобы сообщать Azure о том, что удаленные страницы больше не действительны и могут быть удалены. Это позволяет сократить затраты, если вы создаете большие файлы, а затем удаляете их.

Существует два способа включить поддержку операций TRIM в виртуальной машине Linux. Как обычно, обратитесь к документации дистрибутива, чтобы выбрать рекомендуемый метод.

* Используйте параметр подключения `discard` в */etc/fstab*. Ниже приведен пример.

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* В некоторых случаях параметр `discard` может негативно влиять на производительность. Кроме того, вы можете вручную выполнить команду `fstrim` из командной строки или добавить ее в crontab для регулярного выполнения.

    **Ubuntu**

    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```

    **RHEL или CentOS**

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Устранение неполадок

[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь с рекомендациями по [оптимизации производительности виртуальной машины Linux](optimization.md) , чтобы правильно настроить виртуальную машину Linux.
* Увеличьте емкость хранилища, добавив дополнительные диски, и [настройте RAID](configure-raid.md) для повышения производительности.
