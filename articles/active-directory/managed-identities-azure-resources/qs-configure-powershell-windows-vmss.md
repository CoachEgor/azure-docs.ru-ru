---
title: Настройка управляемых удостоверений в масштабируемых наборах виртуальных машин с помощью PowerShell — Azure AD
description: Пошаговые инструкции по настройке управляемых удостоверений, назначаемых системой и назначаемых пользователем, в масштабируемом наборе виртуальных машин с помощью PowerShell.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 755aee312fd0492fd57a82cb7a437b04ebf72987
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "74547273"
---
# <a name="configure-managed-identities-for-azure-resources-on-virtual-machine-scale-sets-using-powershell"></a>Настройка управляемых удостоверений для ресурсов Azure в масштабируемых наборах виртуальных машин с помощью PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Функция управляемых удостоверений для ресурсов Azure предоставляет службам Azure автоматически управляемые удостоверения в Azure Active Directory. Это удостоверение можно использовать для аутентификации в любой службе, которая поддерживает аутентификацию Azure AD, не храня какие-либо учетные данные в коде. 

В этой статье вы узнаете, как с помощью PowerShell выполнять операции с управляемыми удостоверениями для ресурсов Azure в масштабируемом наборе виртуальных машин.
- Включение и отключение управляемого удостоверения, назначаемого системой, в масштабируемом наборе виртуальных машин
- Добавление и удаление управляемого удостоверения, назначаемого пользователем, в масштабируемом наборе виртуальных машин

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Предварительные требования

- Если вы не работали с управляемыми удостоверениями для ресурсов Azure, изучите [общие сведения](overview.md). **Обратите внимание на [различие между управляемыми удостоверениями, назначаемыми системой и назначаемыми пользователями](overview.md#how-does-the-managed-identities-for-azure-resources-work)**.
- Если у вас нет учетной записи Azure, [зарегистрируйтесь для получения бесплатной пробной учетной записи](https://azure.microsoft.com/free/), прежде чем продолжать.
- Для выполнения операций управления, описанных в этой статье, учетной записи требуются следующие назначения управления доступом на основе ролей Azure:

    > [!NOTE]
    > Дополнительные назначения ролей в каталоге Azure AD не требуются.

    - [Участник виртуальных машин](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) для создания масштабируемого набора виртуальных машин, а также для включения и удаления управляемого удостоверения, назначаемого системой, и (или) управляемого удостоверения, назначаемого пользователем, из масштабируемого набора виртуальных машин.
    - Роль [Участник управляемого удостоверения](/azure/role-based-access-control/built-in-roles#managed-identity-contributor): для создания управляемого удостоверения, назначаемого пользователем.
    - Роль [Оператор управляемого удостоверения](/azure/role-based-access-control/built-in-roles#managed-identity-operator): для назначения и удаления управляемого удостоверения, назначаемого пользователем, в масштабируемом наборе виртуальных машин.
- Установите [последнюю версию Azure PowerShell](/powershell/azure/install-az-ps), если это еще не сделано. 

## <a name="system-assigned-managed-identity"></a>Управляемое удостоверение, назначаемое системой

В этом разделе вы узнаете, как включить и удалить управляемое удостоверение, назначаемое системой, с помощью Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Включение управляемого удостоверения, назначаемого системой, во время создания масштабируемого набора виртуальных машин Azure

Чтобы создать масштабируемый набор виртуальных машин с включенным управляемым удостоверением, назначаемым системой, сделайте следующее.

1. См. *Пример 1* в статье Справочник по командлетам [New-азвмссконфиг](/powershell/module/az.compute/new-azvmssconfig) , чтобы создать масштабируемый набор виртуальных машин с управляемым удостоверением, назначенным системой.  Добавьте параметр `-IdentityType SystemAssigned` в командлет `New-AzVmssConfig`:

    ```powershell
    $VMSS = New-AzVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```



## <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Включение управляемого удостоверения, назначаемого системой, в существующем масштабируемом наборе виртуальных машин Azure

Если нужно включить управляемое удостоверение, назначаемое системой, в существующем масштабируемом наборе виртуальных машин Azure, сделайте следующее.

1. Войдите в Azure, используя команду `Connect-AzAccount`. Используйте учетную запись, связанную с подпиской Azure, которая содержит масштабируемый набор виртуальных машин. Учетная запись должна принадлежать роли, которая предоставляет разрешения на запись в масштабируемом наборе виртуальных машин, например "Участник виртуальных машин".

   ```powershell
   Connect-AzAccount
   ```

2. Сначала получите свойства масштабируемого набора виртуальных машин с помощью [`Get-AzVmss`](/powershell/module/az.compute/get-azvmss) командлета. Затем, чтобы включить управляемое удостоверение, назначаемое системой, используйте параметр `-IdentityType` в командлете [Update-AzVmss](/powershell/module/az.compute/update-azvmss).

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```



### <a name="disable-the-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Отключение управляемого удостоверения, назначаемого системой, в масштабируемом наборе виртуальных машин Azure

Если в масштабируемом наборе виртуальных машин не требуется управляемое удостоверение, назначаемое системой, но по-прежнему требуются управляемые удостоверения, назначаемые пользователем, используйте следующий командлет.

1. Войдите в Azure, используя команду `Connect-AzAccount`. Используйте учетную запись, связанную с подпиской Azure, которая содержит виртуальную машину. Учетная запись должна принадлежать роли, которая предоставляет разрешения на запись в масштабируемом наборе виртуальных машин, например "Участник виртуальных машин".

2. Выполните следующий командлет:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Если имеется масштабируемый набор виртуальных машин, для которого не требуется управляемое удостоверение, назначаемое системой, и у которого нет управляемых удостоверений, назначаемых пользователем, используйте следующие команды.

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-managed-identity"></a>Управляемое удостоверение, назначаемое пользователем

В этом разделе вы узнаете, как добавить и удалить управляемое удостоверение, назначаемое пользователем, в масштабируемом наборе виртуальных машин с помощью Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Задание управляемого удостоверения, назначаемого пользователем, во время создания масштабируемого набора виртуальных машин Azure

Сейчас не поддерживается создание масштабируемого набора виртуальных машин с управляемым удостоверением, назначаемым пользователем, с помощью PowerShell. Сведения о добавлении управляемого удостоверения, назначаемого пользователем, в существующий масштабируемый набор виртуальных машин приведены в следующем разделе. Загляните сюда позже, чтобы проверить наличие новой информации.

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Задание управляемого удостоверения, назначаемого пользователем, для существующего масштабируемого набора виртуальных машин Azure

Чтобы задать управляемое удостоверение, назначаемое пользователем, для существующего масштабируемого набора виртуальных машин Azure, выполните следующее.

1. Войдите в Azure, используя команду `Connect-AzAccount`. Используйте учетную запись, связанную с подпиской Azure, которая содержит масштабируемый набор виртуальных машин. Учетная запись должна принадлежать роли, которая предоставляет разрешения на запись в масштабируемом наборе виртуальных машин, например "Участник виртуальных машин".

   ```powershell
   Connect-AzAccount
   ```

2. Сначала получите свойства масштабируемого набора виртуальных машин с помощью командлета `Get-AzVM`. Затем, чтобы задать управляемое удостоверение, назначаемое пользователем, для масштабируемого набора виртуальных машин, используйте параметры `-IdentityType` и `-IdentityID` в командлете [Update-AzVmss](/powershell/module/az.compute/update-azvmss). Замените `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` собственными значениями.

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Удаление управляемого удостоверения, назначаемого пользователем, из масштабируемого набора виртуальных машин Azure

Если у масштабируемого набора виртуальных машин несколько управляемых удостоверений, назначаемых пользователем, с помощью приведенных ниже команд можно удалить все удостоверения, кроме последнего. Не забудьте заменить значения параметров `<RESOURCE GROUP>` и `<VIRTUAL MACHINE SCALE SET NAME>` собственными. `<USER ASSIGNED IDENTITY NAME>` — это имя управляемого удостоверения, назначаемого пользователем, которое следует оставить в масштабируемом наборе виртуальных машин. Эти сведения можно получить в разделе удостоверений масштабируемого набора виртуальных машин с помощью команды `az vmss show`.

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED IDENTITY NAME>"
```
Если в масштабируемом наборе виртуальных машин нет управляемого удостоверения, назначаемого системой, и вы хотите удалить из него все управляемые удостоверения, назначаемые пользователем, используйте следующую команду.

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Если в масштабируемом наборе виртуальных машин есть управляемые удостоверения, как назначаемое системой, так и назначаемые пользователем, вы можете удалить все управляемые удостоверения, назначаемые пользователем, переключившись на использование только управляемого удостоверения, назначаемого системой.

```powershell 
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Дальнейшие действия

- [Обзор управляемых удостоверений для ресурсов Azure](overview.md).
- Ниже приведены комплексные краткие руководства по созданию виртуальных машин Azure:
  
  - [Создание виртуальной машины Windows с помощью PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Создание виртуальной машины Linux с помощью PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















