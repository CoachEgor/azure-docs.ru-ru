---
title: Утверждать или отклонять запросы для ролей Azure AD в PIM — Azure Active Directory | Документация Майкрософт
description: Узнайте, как утверждать или отклонять запросы для ролей Azure AD в Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f83cb38567feb51ba7959ada7730d66ded677bf9
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476538"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-pim"></a>Утверждать или отклонять запросы для ролей Azure AD в PIM

С помощью Azure Active Directory (Azure AD) Privileged Identity Management (PIM), можно настроить роли, чтобы требовать утверждения для активации и выберите один или несколько пользователей или группы в качестве делегированных утверждающих лиц. Делегированные утверждающие лица имеют 24 часа на утверждение запросов. Если запрос не был утвержден в течение 24 часов, пользователю, наделенному соответствующим правом, необходимо повторно отправить запрос. Продолжительность 24-часового периода для утверждения нельзя изменить.

Выполните действия, описанные в этой статье, чтобы утверждать или отклонять запросы для ролей Azure AD.

## <a name="view-pending-requests"></a>Просмотр ожидающих запросов

Будучи делегированное утверждающее лицо вы получите уведомление по электронной почте при запрос роли Azure AD, ожидающего вашего утверждения. Эти ожидающие утверждения запросы можно просмотреть в PIM.

1. Войдите на [портале Azure](https://portal.azure.com/).

1. Откройте страницу **Azure AD Privileged Identity Management**.

1. Выберите **Роли Azure AD**.

1. Выберите **Утверждение запросов**.

    ![Роли Azure AD - утверждение запросов](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    Отобразится список запросов, ожидающих утверждения.

## <a name="approve-requests"></a>Утверждение запросов

1. Выберите запросы, которые требуется утвердить, а затем выберите **Утвердить**, чтобы открыть область "Утверждение выбранных запросов".

    ![Список запросов на утверждение с выделенным параметром утвердить](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. В области **Причина утверждения** введите причину.

    ![Утверждение выбранных запросов панели с причиной утвердить](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Нажмите кнопку **Утвердить**.

    Символ состояния будет обновлен с вашим разрешением.

    ![Утверждение выбранных запросов области после нажата кнопка "Утвердить"](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Отклонение запросов

1. Выберите запросы, которые требуется отклонить, а затем выберите **Запретить**, чтобы открыть область "Отклонение выбранных запросов".

    ![Утвердить запросы списка с выделенным параметром Deny](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. В области **Причина отклонения** введите причину.

    ![Запретить панели выбранных запросов с причиной deny](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Выберите **Отклонить**.

    Символ состояния будет обновлен с вашим отказом.

## <a name="next-steps"></a>Дальнейшие действия

- [Уведомления по электронной почте в PIM](pim-email-notifications.md)
- [Подтверждение или отклонение запросов для ролей ресурсов Azure в PIM](pim-resource-roles-approval-workflow.md)
