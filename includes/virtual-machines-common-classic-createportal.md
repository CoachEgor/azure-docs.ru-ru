---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 361d0ce5091d80198d47e4ad164f7cba8e21a55d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485296"
---
*Пользовательская* виртуальная машина означает просто виртуальную машину, созданную с помощью **рекомендуемого приложения** из **Marketplace**, потому что большую часть работы она выполняет за вас. Кроме того, вы можете выбирать значения параметров, которые включают в себя следующие элементы:

* подключение виртуальной машины к виртуальной сети;
* установка агента виртуальной машины Azure и расширений виртуальной машины Azure, например для защиты от вредоносных программ;
* добавление виртуальной машины в существующую облачную службу;
* добавление виртуальной машины в существующую учетную запись хранения;
* добавление виртуальной машины в группу доступности.

<!--
> [!IMPORTANT]
> If you want your virtual machine to use a virtual network so you can connect to it directly by host name or set up cross-premises connections, make sure that you specify the virtual network when you create the virtual machine. A virtual machine can be configured to join a virtual network only when you create the virtual machine. For details on virtual networks, see [Azure Virtual Network overview](../articles/virtual-network/virtual-networks-overview.md).
>
>
 -->

> [!IMPORTANT]
> Если требуется использовать виртуальную машину в виртуальной сети, то при создании виртуальной машины обязательно укажите виртуальную сеть.
> * Два преимущества использования виртуальной сети — это возможность напрямую подключаться к виртуальной машине и возможность создания распределенных подключений.
> * Виртуальную машину можно настроить для подключения к виртуальной сети только при ее создании. Дополнительные сведения о виртуальных сетях см. в статье [Обзор виртуальных сетей Azure](../articles/virtual-network/virtual-networks-overview.md).
>
>

## <a name="to-create-the-virtual-machine"></a>Создание виртуальной машины

<!-- Update_Description: update meta properties -->