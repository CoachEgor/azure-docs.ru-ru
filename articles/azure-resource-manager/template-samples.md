---
title: Примеры шаблонов Azure Resource Manager
description: Примеры шаблонов Azure Resource Manager для развертывания функций управления, таких как роли и блокировки.
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 11/16/2018
ms.author: tomfitz
ms.openlocfilehash: e342507b584a405637fc6728dfcd6e49199a154f
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390178"
---
# <a name="azure-resource-manager-templates-for-management-features"></a>Шаблоны Azure Resource Manager для использования функций управления

В следующей таблице представлены ссылки на шаблоны Azure Resource Manager для функций, предоставляемых Resource Manager.

| | |
|-|-|
|**Назначения ролей**||
| [Назначение роли для группы ресурсов](https://github.com/Azure/azure-quickstart-templates/tree/master/101-rbac-builtinrole-resourcegroup)| Назначение встроенной роли пользователю для существующей группы ресурсов. |
| [Назначение роли для существующей виртуальной машины](https://github.com/Azure/azure-quickstart-templates/tree/master/101-rbac-builtinrole-virtualmachine)| Назначение встроенной роли пользователю для существующей виртуальной машины. |
| [Назначение роли для нескольких виртуальных машин](https://github.com/Azure/azure-quickstart-templates/tree/master/201-rbac-builtinrole-multipleVMs)| Назначение встроенных ролей пользователей для более чем одной виртуальной машины. |
| [Назначение роли для подписки Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/subscription-role-assigment)| Назначение роли пользователю для подписки Azure. |
|**Определение роли**||
| [Создание определения пользовательской роли](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-role-def)| Создание определения роли в подписке Azure. |
|**Блокировки ресурсов**||
| [Блокировка группы ресурсов](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment)| Создание группы ресурсов и применение блокировки **DoNotDelete** к группе ресурсов. Назначение роли участника пользователю. |
| | |
