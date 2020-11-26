---
title: Управление группами ресурсов — Azure CLI
description: Используйте Azure CLI для управления группами ресурсов с помощью Azure Resource Manager. Показывает, как создавать, перечислять и удалять группы ресурсов.
author: mumian
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4a9a4ed4ebba7f6f2470bb9e7000a899ebc26323
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185816"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-cli"></a>Управление группами ресурсов Azure Resource Manager с помощью Azure CLI

Узнайте, как использовать Azure CLI с [Azure Resource Manager](overview.md) для управления группами ресурсов Azure. Сведения об управлении ресурсами Azure см. в статье [Управление ресурсами Azure с помощью Azure CLI](manage-resources-cli.md).

Другие статьи об управлении группами ресурсов:

- [Управление группами ресурсов Azure с помощью портал Azure](manage-resources-portal.md)
- [Управление группами ресурсов Azure с помощью Azure PowerShell](manage-resources-powershell.md)

## <a name="what-is-a-resource-group"></a>Что такое группа ресурсов

Группа ресурсов — это контейнер, содержащий связанные ресурсы для решения Azure. В группу ресурсов могут входить все ресурсы приложения или только те, которыми необходимо управлять совместно. Кроме того, пользователи могут выбрать оптимальный для своей организации способ распределения ресурсов в группах ресурсов. Как правило, ресурсы с общим жизненным циклом добавляют в одну и ту же группу ресурсов, чтобы их можно было легко развертывать, обновлять и удалять в виде группы.

В группе ресурсов хранятся метаданные о ресурсах. Исходя из этого, при указании расположения группы ресурсов вы определяете расположение метаданных. В целях обеспечения соответствия необходимо убедиться, что данные хранятся в определенном регионе.

В группе ресурсов хранятся метаданные о ресурсах. Указывая расположение группы ресурсов, вы определяете расположение метаданных.

## <a name="create-resource-groups"></a>Создание группы ресурсов

Следующая команда CLI создает группу ресурсов.

```azurecli-interactive
az group create --name demoResourceGroup --location westus
```

## <a name="list-resource-groups"></a>Перечислить группы ресурсов

В следующем сценарии CLI перечислены группы ресурсов в подписке.

```azurecli-interactive
az group list
```

Чтобы получить одну группу ресурсов, сделайте следующее:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group show --name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Удаление групп ресурсов

Следующий сценарий CLI удаляет группу ресурсов:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

Дополнительные сведения о том, как Azure Resource Manager упорядочивают удаление ресурсов, см. в разделе [Azure Resource Manager удаление группы ресурсов](delete-resource-group.md).

## <a name="deploy-resources-to-an-existing-resource-group"></a>Развертывание ресурсов в существующей группе ресурсов

См. раздел [развертывание ресурсов в существующей группе ресурсов](manage-resources-cli.md#deploy-resources-to-an-existing-resource-group).

## <a name="deploy-a-resource-group-and-resources"></a>Развертывание группы ресурсов и ресурсов

Вы можете создать группу ресурсов и развернуть ресурсы в группе с помощью шаблона диспетчер ресурсов. См. дополнительные сведения в разделе [Создание группы ресурсов и развертывание ресурсов](../templates/deploy-to-subscription.md#resource-groups).

## <a name="redeploy-when-deployment-fails"></a>Повторное развертывание при сбое развертывания

Эта функция также называется *откатом при ошибке*. Дополнительные сведения см. в разделе [Повторное развертывание при сбое развертывания](../templates/rollback-on-error.md).

## <a name="move-to-another-resource-group-or-subscription"></a>Переместить в другую группу ресурсов или подписку

Ресурсы в группе можно переместить в другую группу ресурсов. Дополнительные сведения см. в разделе [Перемещение ресурсов](manage-resources-cli.md#move-resources).

## <a name="lock-resource-groups"></a>Заблокировать группы ресурсов

Блокировка запрещает другим пользователям в организации случайно удалить или изменить критически важные ресурсы, такие как подписка Azure, Группа ресурсов или ресурс. 

Следующий скрипт блокирует группу ресурсов, чтобы группу ресурсов нельзя было удалить.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock create --name LockGroup --lock-type CanNotDelete --resource-group $resourceGroupName  
```

Следующий скрипт получает все блокировки для группы ресурсов:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock list --resource-group $resourceGroupName  
```

Следующий скрипт удаляет блокировку:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the lock name:" &&
read lockName &&
az lock delete --name $lockName --resource-group $resourceGroupName
```

Дополнительные сведения см. [в разделе Блокировка ресурсов с помощью Azure Resource Manager](lock-resources.md).

## <a name="tag-resource-groups"></a>Теги для групп ресурсов

К ресурсам и их группам можно добавлять теги. Это позволяет логически их упорядочивать. Дополнительные сведения см. [в статье Использование тегов для Организации ресурсов Azure](tag-resources.md#azure-cli).

## <a name="export-resource-groups-to-templates"></a>Экспорт групп ресурсов в шаблоны

После успешной настройки группы ресурсов может потребоваться просмотреть шаблон диспетчер ресурсов для группы ресурсов. Экспорт шаблона обеспечивает два преимущества:

- Автоматизируйте будущие развертывания решения, поскольку шаблон содержит всю полную инфраструктуру.
- Познакомьтесь с синтаксисом шаблона, просмотрев нотация объектов JavaScript (JSON), который представляет ваше решение.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group export --name $resourceGroupName  
```

Сценарий отображает шаблон на консоли.  Скопируйте код JSON и сохраните его как файл.

Функция экспорта шаблона не поддерживает экспорт ресурсов фабрики данных Azure. Дополнительные сведения о том, как можно экспортировать ресурсы фабрики данных, см. в статье [копирование или клонирование фабрики данных в фабрике данных Azure](../../data-factory/copy-clone-data-factory.md).

Чтобы экспортировать ресурсы, созданные с помощью классической модели развертывания, необходимо [перенести их в модель развертывания Диспетчер ресурсов](../../virtual-machines/migration-classic-resource-manager-overview.md).

Дополнительные сведения см. [в разделе один и несколько ресурсов экспорт в шаблон в портал Azure](../templates/export-template-portal.md).

## <a name="manage-access-to-resource-groups"></a>Управление доступом к группам ресурсов

Управление [доступом на основе ролей в Azure (Azure RBAC)](../../role-based-access-control/overview.md) — это способ управления доступом к ресурсам в Azure. Дополнительные сведения см. в статье [Добавление и удаление назначений ролей Azure с помощью Azure CLI](../../role-based-access-control/role-assignments-cli.md).

## <a name="next-steps"></a>Дальнейшие действия

- Сведения о Azure Resource Manager см. в разделе [Общие сведения о Azure Resource Manager](overview.md).
- Сведения о синтаксисе шаблона диспетчер ресурсов см. в разделе [Общие сведения о структуре и синтаксисе шаблонов Azure Resource Manager](../templates/template-syntax.md).
- Дополнительные сведения о разработке шаблонов см. в пошаговых [руководствах](../index.yml).
- Сведения о том, как просмотреть схемы шаблонов Azure Resource Manager, см. в разделе [Справочник по шаблонам](/azure/templates/).