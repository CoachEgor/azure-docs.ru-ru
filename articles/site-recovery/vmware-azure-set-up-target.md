---
title: Подготовка целевого объекта репликации виртуальных машин VMware в Azure Site Recovery
description: В этой статье описано, как подготовить целевое окружение Azure к репликации виртуальных машин VMware в Azure.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/03/2019
ms.author: mayg
ms.openlocfilehash: c721234f2e1d806d51d31f3466e441bf8360f6b8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "73693166"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Подготовка целевой среды к аварийному восстановлению виртуальных машин VMware или физических серверов в Azure

В этой статье описано, как подготовить целевую среду Azure для запуска репликации виртуальных машин VMware или физических серверов в Azure.

## <a name="prerequisites"></a>Предварительные требования

В этой статье предполагается:
- Вы создали хранилище служб восстановления на [портал Azure](https://portal.azure.com "Портал Azure") для защиты исходных компьютеров.
- Вы настроили локальную среду для репликации исходных [виртуальных машин VMware](vmware-azure-set-up-source.md) или [физических серверов](physical-azure-set-up-source.md) в Azure.

## <a name="prepare-target"></a>Подготовка цели

**Выбрав цель защиты** и **подготовив источник**, следует перейти к **настройке цели**.

![Подготовка цели](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Подписка:** из раскрывающегося меню выберите подписку, в которую требуется реплицировать виртуальные машины или физические серверы.
2. **Модель развертывания:** выберите модель развертывания (классическую модель или модель Resource Manager).

На основе выбранной модели развертывания выполняется проверка, чтобы обеспечить наличие по крайней мере одной виртуальной сети в целевой подписке для репликации и отработки отказа виртуальной машины или физического сервера.

После успешного завершения проверки нажмите кнопку "OK", чтобы перейти к следующему шагу.

Если у вас нет виртуальной сети, ее можно создать, нажав кнопку **+ Network (+ сеть** ) в верхней части страницы.

## <a name="next-steps"></a>Дальнейшие действия
[Настройте параметры репликации](vmware-azure-set-up-replication.md).
