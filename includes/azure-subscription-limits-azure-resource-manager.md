---
title: включение файла
description: включение файла
services: billing
author: rothja
ms.service: billing
ms.topic: include
ms.date: 04/22/2019
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 712b70960e09a9c2b0e7a998bc0bddbc28c1e112
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64733809"
---
| Resource | Ограничение по умолчанию | Максимальное ограничение |
| --- | --- | --- |
| Число ВМ на [подписку](../articles/billing-buy-sign-up-azure-subscription.md) |25 000<sup>1</sup> на регион. |25 000 на регион. |
| Общее количество ядер виртуальных машин на [подписку](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> на регион. | Обратитесь в службу поддержки. |
| Виртуальную Машину на ряд, например Dv2 и F, ядер на [подписки](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> на регион. | Обратитесь в службу поддержки. |
| [Соадминистраторы](../articles/billing-add-change-azure-subscription-administrator.md) на подписку |Без ограничений. |Без ограничений. |
| [Учетные записи хранения](../articles/storage/common/storage-quickstart-create-account.md) на подписку в каждом регионе |250 |250 |
| [Группы ресурсов](../articles/azure-resource-manager/resource-group-overview.md) на подписку |980 |980 |
| [Группы доступности](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) на подписку |2000 на регион. |2000 на регион. |
| Azure размер запроса API диспетчера ресурсов |4,194,304 байт. |4,194,304 байт. |
| Тегов для каждой подписки<sup>2</sup> |Без ограничений. |Без ограничений. |
| Уникальный тег вычисления для каждой подписки<sup>2</sup> | 10 000 | 10 000 |
| [Облачные службы](../articles/cloud-services/cloud-services-choose-me.md) на подписку |Н/Д<sup>3</sup> |Н/Д<sup>3</sup> |
| [Территориальные группы](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) на подписку |Н/Д<sup>3</sup> |Н/Д<sup>3</sup> |
| [Уровень подписки развертываний](../articles/azure-resource-manager/deploy-to-subscription.md) каждом месте | 800<sup>4</sup> | 800 |

<sup>1</sup>ограничения по умолчанию различаются по типу категории предложения, например бесплатной пробной версии и оплаты по мере использования и ряда, например Dv2, F и G.

<sup>2</sup>можно применять неограниченное число тегов для каждой подписки. Число тегов на ресурс или группу ресурсов ограничено значением 15. Resource Manager возвращает [список уникальное имя тега и значения](/rest/api/resources/tags) в подписке только в том случае, когда число тегов равно 10 000 или меньше. Вы по-прежнему можно найти ресурс по тегу Если это число превышает 10 000.  

<sup>3</sup>эти возможности больше не требуются для групп ресурсов Azure и Resource Manager.

<sup>4</sup>Если вы достигли предела в 800 развертываний, удалить развертывания из журнала, больше не требуются. Чтобы удалить развертывания уровня подписки, используйте [Remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) или [удаление развертывания az](/cli/azure/deployment?view=azure-cli-latest#az-deployment-delete).

> [!NOTE]
> Ядер виртуальных машин действует региональное общее ограничение. Они также имеют ограничение для региональных ряда на размер, например Dv2 и F. Эти ограничения применяются отдельно. Например, рассмотрим подписку с ограничением до 30 ядер виртуальных машин с восточной части США, 30 ядер серии A и 30 ядер серии D. Эту подписку можно развернуть 30 виртуальных машин A1 или 30 виртуальных машин D1 или сочетания этих двух не должно превышать 30 ядер. Пример сочетания — 10 виртуальных машин A1 и 20 виртуальных машин D1.  
> <!-- -->
> 
> 

