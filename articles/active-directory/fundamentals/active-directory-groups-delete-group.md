---
title: Удаление группы с помощью Azure Active Directory | Документация Майкрософт
description: Инструкции по удалению группы с помощью Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdba55e0655a13e65e403f5da73fcb69db5dbca5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "68561916"
---
# <a name="delete-a-group-using-azure-active-directory"></a>Удаление группы с помощью Azure Active Directory
Удаление группы в Azure Active Directory (Azure AD) может потребоваться по разным причинам. Вот наиболее распространенные из них:

- Неправильно задается **Тип группы** с неверным параметром.

- По ошибке создана неправильная или повторяющаяся группа. 

- Группа больше не нужна.

## <a name="to-delete-a-group"></a>Удаление группы
1. Войдите на [портал Azure](https://portal.azure.com) с учетной записью глобального администратора каталога.

2. Выберите **Azure Active Directory**, а затем щелкните **Группы**.

3. На странице **Группы — Все группы** найдите и выберите группу, которую нужно удалить. В этом примере мы выберем группу **MDM policy — East**.

    ![Страница "Группы — Все группы" с выделенным именем группы](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. На странице **Обзор MDM policy — East** нажмите **Удалить**.

    Группа будет удалена из вашего клиента Azure Active Directory.

    ![Страница "Обзор MDM policy — East" с выделенной командой "Удалить"](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Дальнейшие действия

- Если вы по ошибке удалили группу, ее можно создать заново. Дополнительные сведения см. в статье [Как создать простую группу и добавить в нее участников](active-directory-groups-create-azure-portal.md).

- Если вы по ошибке удалили группу Office 365, ее можно восстановить. Дополнительные сведения см. в статье [Восстановление удаленной группы Office 365](../users-groups-roles/groups-restore-deleted.md).
