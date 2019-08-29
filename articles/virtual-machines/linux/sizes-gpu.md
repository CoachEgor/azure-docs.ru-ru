---
title: Размеры виртуальных машин Linux в Azure — ускорение вычислений | Документация Майкрософт
description: Список различных размеров виртуальных машин Linux в Azure, оптимизированных для GPU. Сведения о количестве виртуальных ЦП, дисков данных и сетевых адаптеров, а также о пропускной способности хранилища и сети для размеров виртуальных машин этой серии.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/11/2019
ms.author: jonbeck
ms.openlocfilehash: ed8af91701a5ed77636bb86e8798981f37546f23
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70082131"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Размеры виртуальных машин, оптимизированных для GPU

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-distributions-and-drivers"></a>Поддерживаемые дистрибутивы и драйверы

Чтобы воспользоваться преимуществами GPU виртуальных машин Azure серии N под управлением Linux, необходимо установить графические драйверы GPU NVIDIA. [Расширение драйвера GPU NVIDIA](../extensions/hpccompute-gpu-linux.md) устанавливает необходимые драйверы CUDA или GRID NVIDIA на виртуальную машину серии N. Для установки расширения и управления им можно использовать портал Azure или такие инструменты, как Azure CLI и шаблоны Azure Resource Manager. Сведения о поддерживаемых дистрибутивах и этапах развертывания см. в [документации по расширению драйвера GPU NVIDIA](../extensions/hpccompute-gpu-linux.md). Общие сведения о расширениях виртуальных машин см. в статье [Расширения и компоненты виртуальных машин Azure](../extensions/overview.md).

Если вы решили установить драйверы GUP NVIDIA вручную, то сможете найти сведения о поддерживаемых дистрибутивах и действиях по установке и проверке в разделе [Установка драйвера GPU для виртуальных машин серии N под управлением Linux](n-series-driver-setup.md).


[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Не следует устанавливать X-сервер или другие системы, использующие драйвер `Nouveau`, на виртуальные машины NC под управлением Ubuntu. Перед установкой драйверов NVIDIA для GPU необходимо отключить драйвер `Nouveau`.  

## <a name="other-sizes"></a>Остальные размеры
- [Универсальные](sizes-general.md)
- [Оптимизированные для вычислений](sizes-compute.md)
- [Оптимизированные для памяти](sizes-memory.md)
- [Оптимизированные для хранилища](sizes-storage.md)
- [Для высокопроизводительных вычислений](sizes-hpc.md)
- [Предыдущие поколения](sizes-previous-gen.md)

## <a name="next-steps"></a>Следующие шаги
Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.