---
title: Сброс пароля пользователя в Azure Active Directory | Документация Майкрософт
description: Инструкции по сбросу пароля пользователя с помощью Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4db6554e86cef61f2fc8e7a466919d2ce723f0e5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60248028"
---
# <a name="reset-a-users-password-using-azure-active-directory"></a>Сброс пароля пользователя в Azure Active Directory

Если пользователь забыл пароль, был заблокирован на устройстве или никогда не получал пароля, то администратор может сбросить его пароль.

>[!Note]
>Если клиент Azure AD находится не в домашнем каталоге пользователя, вы не сможете сбросить пароль. Это означает, что если пользователь выполняет вход в вашей организации, используя учетную запись из другой организации, учетную запись Майкрософт или учетную запись Google, вы не сможете сбросить пароль.<br><br>Если пользователь имеет Windows Server Active Directory в качестве центра проверки, пароль можно сбросить, только включив компонент обратной записи паролей.<br><br>Если пользователь имеет внешний Azure AD в качестве центра проверки, то пароль сбросить нельзя. Только сам пользователь или администратор внешнего Azure AD могут сбросить пароль.

>[!Note]
>Если вы не являетесь администратором и ищете инструкции по сбросу пароля своей рабочей или учебной учетной записи, ознакомьтесь с разделом [Сброс пароля к рабочей или учебной учетной записи](../user-help/active-directory-passwords-update-your-own-password.md).

## <a name="to-reset-a-password"></a>Процедура сброса пароля

1. Войдите в [портала Azure](https://portal.azure.com/) как администратор или администратор паролей. Дополнительные сведения о доступных ролях см. в статье [Назначение ролей администратора в Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md#available-roles)

2. Выберите **Azure Active Directory**, затем **Пользователи**, найдите и выберите пользователя, которому требуется сброс, и затем **Сброс пароля**.

    Вид страницы **Ален Шарон — профиль** с кнопкой **Сброс пароля**.

    ![Страница профиля пользователя с выделенной кнопкой сброса пароля](media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. На странице **Сброс пароля** нажмите **Сбросить пароль**.

    Будет автоматически сгенерирован новый пароль пользователя.

4. Скопируйте пароль и передайте его пользователю. Пользователю потребуется изменить пароль во время следующего входа в систему.

    >[!Note]
    >Срок действия временного пароля не ограничен. При следующем входе пользователя пароль будет по-прежнему действующим, независимо от того, сколько времени прошло с момента создания временного пароля.

## <a name="next-steps"></a>Дальнейшие действия

После сброса пароля пользователя можно выполнить следующие базовые операции:

- [Добавить или удалить пользователя](add-users-azure-active-directory.md)

- [Назначение ролей пользователям](active-directory-users-assign-role-azure-portal.md)

- [Добавить или изменить данные профиля](active-directory-users-profile-azure-portal.md)

- [Создать простую группу и добавить в нее участников](active-directory-groups-create-azure-portal.md)

Или можно выполнить более сложные сценарии пользователей, такие как назначение делегатов, использование политик и совместное использование учетных записей пользователей. Дополнительные сведения о других доступных действиях см. в [документации по управлению пользователями Azure Active Directory](../users-groups-roles/index.yml).
