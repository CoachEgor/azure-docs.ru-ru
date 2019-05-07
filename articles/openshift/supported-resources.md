---
title: Поддерживаемые ресурсы для Azure Red Hat OpenShift | Документация Майкрософт
description: Понять, какие регионы Azure и размеры виртуальных машин, поддерживаемых Microsoft Azure Red Hat OpenShift.
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 22cc195b7808ad4a9111aafcf883a68c51f3709f
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076110"
---
# <a name="azure-red-hat-openshift-resources"></a>Ресурсы Azure Red Hat OpenShift

В этом разделе перечислены регионы Azure и размеры виртуальных машин, поддерживаемых службой Microsoft Azure Red Hat OpenShift.

## <a name="azure-regions"></a>Регионы Azure

Кластеры Azure Red Hat OpenShift можно развернуть в следующих регионах Azure по всему миру.

|Регион|Код интерфейса командной строки|
|-|-|
|🇦🇺 Восточная Австралия|`australiaeast`|
|Центральная Канада 🇨🇦|`canadacentral`|
|Восточная Канада 🇨🇦|`canadaeast`|
|🇺🇸 восточная часть США|`eastus`|
|🇺🇸 Западная часть США|`westus`|
|🇪🇺 Западная Европа|`westeurope`|
|🇪🇺 Северная Европа|`northeurope`|

## <a name="virtual-machine-sizes"></a>Размер виртуальных машин

Ниже приведены поддерживаемые размеры виртуальных машин, можно указать для вычислительных узлов в кластере Azure Red Hat OpenShift.

> [!Important]
> Каждая виртуальная машина имеет разное количество дисков, которые можно подключить. Это может оказаться как очевидно как память или ЦП размер.
> Не во всех регионах доступны виртуальные машины всех размеров. Даже если API поддерживает размер вами, если размер недоступен в регионе, в указанную вами может появиться ошибка.
> См. в разделе [размеров текущий список поддерживаемых виртуальных Машин на регион](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) Дополнительные сведения.

## <a name="compute-node-sizes"></a>Вычисления размеров узлов

Azure Red Hat OpenShift REST API поддерживает следующие размеры вычислительных узлов.

|Размер|Виртуальных ЦП|ОЗУ|
|-|-|-|
|Standard D4s v3|4.|16 ГБ|
|Standard D8s v3|8|32 ГБ|
|Standard D16s v3|16|64 ГБ|
|Standard D32s v3|32|128 ГБ|
|-|-|-|
|Standard E4s v3|4.|32 ГБ|
|Standard E8s v3|8|64 ГБ|
|Standard E16s v3|16|128 ГБ|
|Standard E32s v3|32|256 ГБ|
|-|-|-|
|Standard F8s v2|8|16 ГБ|
|Standard F16s v2|16|32 ГБ|
|Standard F32s v2|32|64 ГБ|

## <a name="master-node-sizes"></a>Размеры главный узел

Следующий образец / размеров узлов инфраструктуры поддерживаются Azure Red Hat OpenShift REST API:

|Размер|Виртуальных ЦП|ОЗУ|
|-|-|-|
|Standard D4s v3|4.|16 ГБ|
|Standard D8s v3|8|32 ГБ|
|Standard D16s v3|16|64 ГБ|
|Standard D32s v3|32|128 ГБ|

## <a name="next-steps"></a>Дальнейшие действия

Попробуйте [создать кластер Azure Red Hat OpenShift](tutorial-create-cluster.md) руководства.