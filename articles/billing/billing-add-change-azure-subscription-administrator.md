---
title: Добавление или изменение администраторов подписки Azure | Документация Майкрософт
description: В этой статье описывается, как добавлять или изменять администратора подписки Azure с помощью управления доступом на основе ролей (RBAC).
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 000315a2d751a05d3e401ee1bb9f593c6e321194
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2019
ms.locfileid: "64922899"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Добавление или изменение администраторов подписки Azure

Чтобы управлять доступом к ресурсам Azure, требуется соответствующая роль администратора. Azure обладает системой авторизации "Управление доступом на основе ролей" (RBAC) вместе с несколькими встроенными ролями, которые можно выбирать. Их можно назначать различным областям, например группе управления, подписке или группе ресурсов.

Корпорация Майкрософт рекомендует применять RBAC для управления доступом к ресурсам. Если вы по-прежнему используете классическую модель развертывания и управляете классически ресурсами с помощью [модуля PowerShell для управления службами Azure](https://docs.microsoft.com/powershell/module/servicemanagement/azure), вам потребуется роль классического администратора. 

> [!TIP]
> Если же вы управляете классическими ресурсами только с помощью портала Azure, вам не потребуется использовать роль классического администратора.

Дополнительные сведения см. в статьях [Azure Resource Manager и классическое развертывание: сведения о моделях развертывания и состоянии ресурсов](../azure-resource-manager/resource-manager-deployment-model.md) и [Azure classic subscription administrators](../role-based-access-control/classic-administrators.md) (Классические администраторы подписки в Azure).

В этой статье описано, как добавлять или изменять роли администратора для пользователя с помощью RBAC в области подписки.

<a name="add-an-admin-for-a-subscription"></a>

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Предоставление пользователю прав администратора подписки

Чтобы предоставить пользователю права администратора подписки Azure, назначьте ему роль [Владелец](../role-based-access-control/built-in-roles.md#owner) (роль RBAC) в этой подписке. Роль "Владелец" предоставляет пользователю полный доступ ко всем ресурсам в подписке, включая право делегировать доступ другим пользователям. Этот процесс не отличается от любой процедуры назначения ролей.

1. На портале Azure откройте раздел [Подписки](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Щелкните подписку, для которой нужно предоставить доступ.

1. Щелкните **Управление доступом (IAM)** .

1. Щелкните вкладку **Назначения ролей**, чтобы просмотреть все назначения ролей для этой подписки.

    ![Снимок экрана, на котором показано назначение ролей](./media/billing-add-change-azure-subscription-administrator/role-assignments.png)

1. Щелкните **Добавить** > **Добавить назначение ролей**, чтобы открыть панель **Добавление назначения роли**.

    Если у вас нет прав назначать роли, функция будет неактивна.

1. В раскрывающемся списке **Роль** выберите роль **Владелец**.

1. В списке **Выбор** выберите пользователя. Если пользователь не отображается в списке, введите его имя в поле **Выбор**, чтобы найти в каталоге отображаемые имена и адреса электронной почты.

    ![Снимок экрана, на котором показана выбранная роль "Владелец"](./media/billing-add-change-azure-subscription-administrator/add-role.png)

1. Чтобы присвоить роль, щелкните **Сохранить**.

    Через несколько секунд пользователю будет назначена роль владельца в области действия подписки.

## <a name="next-steps"></a>Дополнительная информация

* [Что такое управление доступом на основе ролей (RBAC)?](../role-based-access-control/overview.md)
* [Сведения о различных ролях в Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [How to: Связывание или добавление подписки Azure в клиент Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).
* [Разрешения роли администратора в Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md)

## <a name="need-help-contact-support"></a>Требуется помощь? Обращение в службу поддержки

Если вам все еще нужна помощь, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), которая поможет быстро устранить проблему.
