---
title: Подключение Azure NetApp Files томов для виртуальных машин
description: Узнайте, как подключить или отключить том для виртуальных машин Windows или виртуальных машин Linux в Azure.
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: 3457adf67fa067a154eef008b08d8cfcc1d9eaa0
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/31/2019
ms.locfileid: "75551545"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Подключение или отключение тома для виртуальных машин Windows или Linux 

При необходимости вы можете подключить или отключить том для виртуальных машин Windows или Linux.  Инструкции по подключению для виртуальных машин Linux доступны в службе Azure NetApp Files.  

1. Щелкните колонку **тома** , а затем выберите том, для которого необходимо установить подключение. 
2. Нажмите кнопку **подключить инструкции** из выбранного тома, а затем следуйте инструкциям по подключению тома. 

    ![Инструкции по подключению NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Инструкции по подключению SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)
    
    Если вы используете Нфсв 4.1, для подключения файловой системы используйте следующую команду: `sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

3. Если вы хотите, чтобы том NFS был автоматически подключен при запуске или перезагрузке виртуальной машины Azure, добавьте запись в файл `/etc/fstab` на узле. 

    Например:  `$ANFIP:/$FILEPATH        /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

    * `$ANFIP` — IP-адрес тома Azure NetApp Files, который находится в колонке свойства тома.
    * `$FILEPATH` — путь экспорта тома Azure NetApp Files.
    * `$MOUNTPOINT` — это каталог, созданный на узле Linux, который используется для подключения экспорта NFS.

4. Если вы хотите подключить том к Windows с помощью NFS:

    а. Сначала подключите том на виртуальной машине UNIX или Linux.  
    b. Выполните команду `chmod 777` или `chmod 775` для тома.  
    c. Подключите том через клиент NFS в Windows.

## <a name="next-steps"></a>Дальнейшие действия

* [Настройка домена Нфсв 4.1 по умолчанию для Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md)
* [Вопросы и ответы по NFS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs)
