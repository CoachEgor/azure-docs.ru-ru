---
title: Проверка доступа для ролей ресурсов Azure в PIM — Azure Active Directory | Документация Майкрософт
description: Узнайте, как проверить доступ ролей ресурсов Azure в Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ede412292caf682c3078002d23e1956bd68d1378
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602199"
---
# <a name="review-access-to-azure-resource-roles-in-pim"></a>Проверка доступа для ролей ресурсов Azure в PIM
Azure Active Directory (Azure AD) Privileged Identity Management (PIM) упрощает управление привилегированным доступом к ресурсам в Azure. 

Если вам назначена роль администратора, администратор привилегированных ролей вашей организации может попросить вас регулярно подтверждать, что эта роль по-прежнему требуется вам для работы. Вы можете получить сообщение электронной почты, содержащее ссылку для подтверждения, или перейти непосредственно на [портал Azure](https://portal.azure.com). Следуйте указаниям в этой статье, чтобы выполнить самостоятельную проверку назначенных вам ролей.

Если вы администратор привилегированных ролей и вас интересуют функции проверки доступа, дополнительные сведения см. в статье [Как запустить проверку доступа в управлении привилегированными пользователями Azure AD](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Добавление приложения для управления привилегированными пользователями
Для выполнения проверки можно использовать приложение "Управление привилегированными пользователями" (PIM) Azure AD на [портале Azure](https://portal.azure.com/). Если у вас нет приложения на портале, выполните следующие действия, чтобы начать работу.

1. Войдите на [портале Azure](https://portal.azure.com/).
2. Щелкните свое имя пользователя в правом верхнем углу портала Azure и выберите каталог, с которым будете работать.
3. Выберите **Все службы** и введите **Azure AD Privileged Identity Management** в текстовом поле *Фильтр*.
4. Установите флажок **Закрепить на панели мониторинга** и щелкните **Создать**. Откроется приложение управления привилегированными пользователями.

## <a name="approve-or-deny-access"></a>Утверждение или отклонение доступа
При утверждении или отклонении доступа вы просто сообщаете проверяющему, используете ли вы еще эту роль. Чтобы оставаться в этой роли, выберите **Утвердить**, а когда доступ вам больше не требуется — **Запретить**. Состояние изменяется, только если рецензент применяет результаты.

Выполните следующие действия, чтобы найти проверку доступа и завершить ее.
1. Перейдите в приложение Azure AD PIM.
2. Выберите колонку **Проверка доступа**.

   ![Снимок экрана приложения PIM с выбранной колонкой "Обзор"](media/pim-resource-roles-perform-access-review/rbac-access-review-complete.png)

3. Выберите проверку, которую необходимо завершить. 
4. Выберите **Утвердить** или **Запретить**. Возможно, в текстовом поле **Указание причины** потребуется ввести причину своего решения.

   ![Снимок экрана страницы просмотра подробностей](media/pim-resource-roles-perform-access-review/rbac-access-review-choice.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Выполнение проверки доступа из моих ролей Azure AD в PIM](pim-how-to-perform-security-review.md)
