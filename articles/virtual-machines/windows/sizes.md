---
title: Размеры виртуальных машин Windows в Azure
description: Список различных размеров виртуальных машин Windows в Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/06/2019
ms.author: jonbeck
ms.openlocfilehash: 857b60a36cdf3b6ed892199db8be77d16805f69e
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645962"
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Размеры виртуальных машин Windows в Azure

В этой статье описаны доступные размеры и разновидности виртуальных машин Azure, которые можно использовать для запуска приложений для Windows и рабочих нагрузок Windows. Здесь также предоставлены рекомендации по развертыванию, которые нужно учитывать при планировании использования этих ресурсов.  Также доступна версия этой статьи для [виртуальных машин Linux](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


| Тип                     | Размеры           |    Description       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Универсальные](sizes-general.md)          | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC  | Сбалансированное соотношение ресурсов ЦП и памяти. Идеальное решение для тестирования и разработки, небольших и средних баз данных, а также веб-серверов с малым и средним объемом трафика. |
| [Оптимизированные для вычислений](sizes-compute.md)        | Fsv2           | Высокое соотношение ресурсов ЦП и памяти. Подходят для веб-серверов со средним объемом трафика, сетевых устройств, пакетных процессов и серверов приложений.        |
| [Оптимизированные для памяти](sizes-memory.md)         | Esv3, Ev3, Easv4, Eav4, Mv2, M, DSv2, Dv2  | Высокое соотношение ресурсов памяти и ядра. Отлично подходят для серверов реляционных баз данных, кэша среднего и большого объема, а также выполняющейся в памяти аналитики.                 |
| [Оптимизированные для хранилища](sizes-storage.md)        | Lsv2                | Высокая пропускная способность и количество операций ввода-вывода. Идеальный вариант для работы с большими данными, с базами данных SQL и NoSQL, системами хранения данных и большими транзакционными базами данных.  |
| [GPU](sizes-gpu.md)            | NC, NCv2, NCv3, ND, NDv2 (Предварительная версия), NV, NVv3, NVv4 (Предварительная версия)| Специализированные виртуальные машины, предназначенные для ресурсоемкой отрисовки изображений и редактирования видео, а также для обучения моделей и формирования выводов с помощью глубокого обучения. Доступны виртуальные машины одним или несколькими GPU.       |
| [Для высокопроизводительных вычислений](sizes-hpc.md) | ХБ, HC, H | Виртуальные машины с самыми быстрыми и мощными ЦП, для которых можно настроить сетевые интерфейсы с высокой пропускной способностью (RDMA). |


<br> 

- Подробнее о ценах на различные размеры см. в разделе [Цены на виртуальные машины](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). 
- Сведения об общих ограничениях виртуальных машин Azure см. в статье [Подписка Azure, границы, квоты и ограничения службы](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- Стоимость хранилища рассчитывается отдельно в зависимости от количества страниц, используемых в учетной записи хранения. Дополнительные сведения см. на странице [Цены на хранилища Azure](https://azure.microsoft.com/pricing/details/storage/).
- Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.


## <a name="rest-api"></a>REST API

Сведения об использовании Rest API, чтобы получить сведения о размерах виртуальных машин, см. в следующих статьях:

- [List available virtual machine sizes for resizing](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes) (Вывод доступных размеров виртуальных машин для изменения размеров)
- [List available virtual machine sizes for a subscription](https://docs.microsoft.com/rest/api/compute/resourceskus/list) (Вывод доступных размеров виртуальных машин для подписки)
- [List available virtual machine sizes in an availability set](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes) (Вывод доступных размеров виртуальных машин в группе доступности)

## <a name="acu"></a>ACU

Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.

## <a name="benchmark-scores"></a>Результаты теста производительности

Узнайте больше о вычислительной мощности виртуальных машин Windows с помощью [результатов теста производительности CoreMark](compute-benchmark-scores.md).

## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о различных доступных размерах виртуальных машин.
- [Универсальные](sizes-general.md)
- [Оптимизированные для вычислений](sizes-compute.md)
- [Оптимизированные для памяти](../virtual-machines-windows-sizes-memory.md)
- [Оптимизированные для хранилища](../virtual-machines-windows-sizes-storage.md)
- [Оптимизированные для GPU](sizes-gpu.md)
- [Для высокопроизводительных вычислений](sizes-hpc.md)
- Описание серий A Standard, Dv1 (D1–4 и D11–14 v1) и A8–A11 приведено на странице [Предыдущее поколение](sizes-previous-gen.md).




