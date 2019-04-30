---
title: Просмотр и использование шаблона Azure Resource Manager виртуальной машины | Документация Майкрософт
description: Сведения об использовании шаблона Azure Resource Manager виртуальной машины для создания других виртуальных машин.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: spelluru
ms.openlocfilehash: 533770d98b146dea01e91e1249115c4b5c074b3c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62101577"
---
# <a name="create-virtual-machines-using-an-azure-resource-manager-template"></a>Создание виртуальных машин с использованием шаблона Azure Resource Manager 

При создании виртуальной машины в DevTest Labs на [портале Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040) вы можете просмотреть шаблон Azure Resource Manager, прежде чем сохранить виртуальную машину. Этот шаблон можно использовать как основу, чтобы создать больше виртуальных машин лаборатории с теми же настройками.

В этой статье сравниваются шаблоны Resource Manager для нескольких виртуальных машин и для одной виртуальной машины. Кроме того, вы узнаете, как просмотреть и сохранить шаблон при создании виртуальной машины.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Шаблоны Resource Manager для нескольких и одной виртуальной машины
Существует два способа создания виртуальных машин в DevTest Labs с помощью шаблона Resource Manager: подготовить ресурс Microsoft.DevTestLab/labs/virtualmachines или подготовить ресурс Microsoft.Compute/virtualmachines. Каждый из этих ресурсов используется в различных сценариях и требует разных разрешений.

- Шаблоны Resource Manager, использующие тип ресурса Microsoft.DevTestLab/labs/virtualmachines (как объявлено в свойстве resource в шаблоне), могут подготовить отдельные виртуальные машины лаборатории. Каждая виртуальная машина затем отображается в виде отдельного элемента в списке виртуальных машин DevTest Labs:

   ![Список виртуальных машин как отдельных элементов в списке виртуальных машин DevTest Labs](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Этот тип шаблона Resource Manager можно подготовить через команду Azure PowerShell **New AzResourceGroupDeployment** или с помощью команды Azure CLI **Создание развертывания группы az**. Для это требуется иметь разрешения администратора, поэтому пользователи, которым назначена роль пользователя DevTest Labs, не могут выполнить развертывание. 

- Шаблоны Resource Manager, использующие тип ресурса Microsoft.Compute/virtualmachines, могут подготовить несколько виртуальных машин, которые отображаются в качестве отдельной среды в списке виртуальных машин DevTest Labs:

   ![Список виртуальных машин как отдельных элементов в списке виртуальных машин DevTest Labs](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Виртуальные машины в одной среде могут находиться под общим управлением и использовать один и тот же жизненный цикл. Пользователи, которым назначена роль пользователя DevTest Labs, могут создавать среды с помощью этих шаблонов, при условии, что администратор настроил лабораторию нужным образом.

В оставшейся части статьи рассматриваются шаблоны Resource Manager, использующие Microsoft.DevTestLab/labs/virtualmachines. Они используются администраторами лаборатории для автоматизации создания виртуальной машины лаборатории (например, запрашиваемые виртуальные машины) или для создания образа золотого стандарта (например, фабрики образов).

В статье [Рекомендации по созданию шаблонов Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) содержится множество рекомендаций и указаний, облегчающих создание надежных и простых в использовании шаблонов Azure Resource Manager.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Просмотр и сохранение шаблона Azure Resource Manager виртуальной машины
1. Выполните действия, описанные в статье [Создание первой виртуальной машины в лаборатории в Azure DevTest Labs](tutorial-create-custom-lab.md#add-a-vm-to-the-lab), чтобы начать создание виртуальной машины.
1. Введите необходимые сведения для виртуальной машины и добавьте все необходимые для нее артефакты.
1. В нижней части окна настройки параметров выберите **View ARM template** (Просмотреть шаблон ARM).

   ![Кнопка View ARM template (Просмотреть шаблон ARM)](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Скопируйте и сохраните шаблон Resource Manager, чтобы позже использовать его для создания другой виртуальной машины.

   ![Сохранение шаблона Resource Manager для последующего использования](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Прежде чем использовать сохраненный шаблон Resource Manager, необходимо обновить раздел параметров. Вы можете создать файл parameter.json, который настраивает только параметры за пределами фактического шаблона Resource Manager. 

![Настройка параметров с помощью JSON-файла](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

Шаблон Resource Manager готов к использованию для [создания виртуальной машины](devtest-lab-create-environment-from-arm.md).

### <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения см. в статье [Создание сред со множеством виртуальных машин и ресурсов PaaS с помощью шаблонов Azure Resource Manager](devtest-lab-create-environment-from-arm.md).
* [Deploy a Resource Manager template to create a VM](devtest-lab-create-environment-from-arm.md#automate-deployment-of-environments) (Развертывание шаблона Resource Manager для создания виртуальной машины)
* Ознакомьтесь с кратким руководством по шаблонам Resource Manager для автоматизации работы в DevTest Labs в [общедоступном репозитории DevTest Labs на сайте GitHub](https://github.com/Azure/azure-quickstart-templates).
