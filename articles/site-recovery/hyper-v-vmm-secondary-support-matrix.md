---
title: Матрица поддержки аварийного восстановления виртуальных машин Hyper-V в облаках VMM на дополнительный сайт с помощью службы Azure Site Recovery | Документация Майкрософт
description: Общие сведения о поддержке репликации виртуальных машин Hyper-V в облаках VMM на дополнительный сайт с помощью Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: raynew
ms.openlocfilehash: 60ca12e5b362a37eb9f85c9a0d1fc23ca99e9edc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60362345"
---
# <a name="support-matrix-for-disaster-recovery-of-hyper-v-vms-to-a-secondary-site"></a>Матрица поддержки аварийного восстановления виртуальных машин Hyper-V на дополнительный сайт

В этой статье перечислены поддерживаемые компоненты при использовании [Azure Site Recovery](site-recovery-overview.md) service для репликации виртуальных машин Hyper-V, управляемых в облаках System Center Virtual Machine Manager (VMM) на дополнительный сайт. Если нужно реплицировать виртуальные машины Hyper-V в Azure, ознакомьтесь с [этой таблицей поддержки](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Вы можете выполнить репликацию на дополнительный сайт, только если управление узлами Hyper-V осуществляется в облаках VMM.

  

## <a name="host-servers"></a>Серверы узлов

**Операционная система** | **Дополнительные сведения**
--- | ---
Windows Server 2012 R2 | На сервере должны быть установлены последние обновления.
Windows Server 2016 |  Сейчас облака VMM 2016, сочетающие узлы Windows Server 2016 и 2012 R2, не поддерживаются.<br/><br/> Развернутые компоненты, обновленные с System Center 2012 R2 VMM 2012 R2 до System Center 2016, сейчас не поддерживаются.


## <a name="replicated-vm-support"></a>Поддержка реплицируемых виртуальных машин

В следующей таблице указаны сведения о поддержке операционных систем для компьютеров, реплицируемых с помощью Site Recovery. В поддерживаемой операционной системе может выполняться любая рабочая нагрузка.

**Версия Windows** | **Hyper-V (с VMM)**
--- | ---
Windows Server 2016 | Любая операционная система на виртуальной машине, [поддерживаемая Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) на базе Windows Server 2016 
Windows Server 2012 R2 | Любая операционная система на виртуальной машине, [поддерживаемая Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) на базе Windows Server 2012 R2

## <a name="linux-machine-storage"></a>Хранилище компьютера Linux

Можно реплицировать только компьютеры Linux со следующими характеристиками хранилища:

- файловая система: EXT3, ETX4, ReiserFS, XFS;
- программное обеспечение Multipath — Device Mapper;
- диспетчер томов: LVM2.
- Физические серверы с хранилищем контроллера HP CCISS не поддерживаются.
- Файловая система ReiserFS поддерживается только в SUSE Linux Enterprise Server 11 с пакетом обновления 3 (SP3).

## <a name="network-configuration---hostguest-vm"></a>Конфигурация сети — узел или гостевые виртуальные машины

**Конфигурация** | **Поддерживаются**  
--- | --- 
Узел — объединение сетевых адаптеров | Yes 
Узел — виртуальная локальная сеть | Yes 
Узел — IPv4 | Yes 
Узел — IPv6 | Нет  
Гостевая виртуальная машина — объединение сетевых адаптеров | Нет 
Гостевая виртуальная машина — IPv4 | Yes
Гостевая виртуальная машина — IPv6 | Нет 
Гостевая виртуальная машина (Windows или Linux) — статический IP-адрес | Yes
Гостевая виртуальная машина — несколько сетевых адаптеров | Yes


## <a name="storage"></a>Хранилище

### <a name="host-storage"></a>Хранилище узла

**Устройство или система хранения (узел)** | **Поддерживаются**
--- | --- 
NFS | Н/Д
SMB 3.0 |  Yes
Сеть SAN (iSCSI) | Yes
Многопутевой (MPIO) | Yes

### <a name="guest-or-physical-server-storage"></a>Хранилища гостевого или физического сервера

**Конфигурация** | **Поддерживаются**
--- | --- | 
VMDK |  Н/Д
VHD (VHDX) | Да (до 16 дисков)
Виртуальная машина 2-го поколения | Yes
Общий диск кластера | Нет 
Зашифрованный диск | Нет 
UEFI| Н/Д
NFS | Нет 
SMB 3.0 | Нет 
RDM | Н/Д
Диск > 1 ТБ | Yes
Том с чередующимся диском > 1 ТБ<br/><br/> Диспетчер логических томов | Yes
Дисковые пространства | Yes
"Горячее" добавление или удаление диска | Нет 
Исключение диска | Yes
Многопутевой (MPIO) | Yes

## <a name="vaults"></a>Хранилища

**Действие** | **Поддерживаются**
--- | --- 
Перемещение хранилищ между группами ресурсов (в пределах подписок и между ними) |  Нет 
Перемещение хранилища, сети или виртуальных машин Azure между группами ресурсов (в пределах подписок и между ними) | Нет 

## <a name="azure-site-recovery-provider"></a>Поставщик Azure Site Recovery

Поставщик координирует обмен данными между серверами VMM. 

**Актуальная** | **Обновления**
--- | --- 
5.1.19 ([доступна на портале](https://aka.ms/downloaddra)) | [Новейшие функции и последние исправления](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>Дальнейшие действия

[Репликация виртуальных машин Hyper-V из облаков VMM на вторичный сайт VMM](tutorial-vmm-to-vmm.md)

