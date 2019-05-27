---
title: Назначение доступа к ресурсу Azure на основе управляемого удостоверения с помощью портала Azure
description: Пошаговые инструкции по назначению управляемого удостоверения на одном ресурсе для доступа к другому ресурсу с помощью портала Azure.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 958b3d72a3a8df4a3b67f62e7db788d7142ca667
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "66112923"
---
# <a name="assign-a-managed-identity-access-to-a-resource-by-using-the-azure-portal"></a>Назначение доступа на основе управляемого удостоверения для ресурса с помощью портала Azure

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

После настройки ресурса Azure с помощью управляемого удостоверения можно предоставить доступ на основе управляемого удостоверения другому ресурсу, как и любому субъекту безопасности. В этой статье показано, как предоставить виртуальной машине Azure или масштабируемому набору виртуальных машин доступ к учетной записи хранения Azure с помощью портала Azure.

## <a name="prerequisites"></a>Технические условия

- Если вы не работали с управляемыми удостоверениями для ресурсов Azure, изучите [общие сведения](overview.md). **Обратите внимание на [различие между управляемыми удостоверениями, назначаемыми системой и назначаемыми пользователями](overview.md#how-does-it-work)**.
- Если у вас нет учетной записи Azure, [зарегистрируйтесь для получения бесплатной пробной учетной записи](https://azure.microsoft.com/free/), прежде чем продолжать.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Назначение управляемому удостоверению прав доступа к другому ресурсу с помощью RBAC

После включения управляемого удостоверения для ресурса Azure, например [виртуальной машины Azure](qs-configure-portal-windows-vm.md) или [VMSS Azure](qs-configure-portal-windows-vmss.md), сделайте следующее.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетной записи, связанной с подпиской Azure, для которой вы настроили управляемое удостоверение.

2. Перейдите к нужному ресурсу, для которого требуется изменить управление доступом. В этом примере мы предоставляем виртуальной машине Azure доступ к учетной записи хранения, поэтому переходим к учетной записи хранения.

3. Перейдите на страницу ресурса **Управление доступом (IAM)**, затем выберите **+ Добавить назначение ролей**. Затем укажите **Роль**, щелкните **Назначение доступа к** и укажите соответствующую **Подписку**. В области критериев поиска отобразится ресурс. Выберите этот ресурс и нажмите кнопку **Сохранить**. 

   ![Снимок экрана "Управление доступом (IAM)"](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
     
## <a name="next-steps"></a>Дальнейшие действия

- [Что такое управляемые удостоверения для ресурсов Azure?](overview.md)
- Дополнительные сведения о [настройке управляемых удостоверений для ресурсов Azure на виртуальной машине с помощью портала Azure](qs-configure-portal-windows-vm.md).
- Дополнительные сведения о [настройке управляемых удостоверений для ресурсов Azure в масштабируемом наборе виртуальных машин с помощью портала Azure](qs-configure-portal-windows-vmss.md).


