---
title: Удаление доступа пользователя к приложению в Azure Active Directory
description: Узнайте, как удалить доступ пользователя к приложению в Azure Active Directory
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 11/02/2020
ms.author: kenwith
ms.openlocfilehash: 28b31d98f283dc957927ab2a35f0ab95bf066473
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "94654122"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Удаление доступа пользователя к приложению

В этой статье представлены сведения об удалении доступа пользователя к приложению.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Я хочу отменить доступ к приложению для определенного пользователя или группы

Чтобы удалить доступ к приложению, назначенный пользователю или группе, выполните действия, описанные в статье [Удаление назначения доступа к корпоративному приложению для пользователя или группы в предварительной версии Azure Active Directory](./assign-user-or-group-access-portal.md).

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Я хочу отключить доступ к приложению для всех пользователей

Чтобы отключить вход в приложение для всех пользователей, выполните действия, описанные в статье [Отключение входа пользователя в корпоративное приложение в предварительной версии Azure Active Directory](./disable-user-sign-in-portal.md).

## <a name="i-want-to-delete-an-application-entirely"></a>Я хочу полностью удалить приложение

В [серии примеров по управлению приложениями](delete-application-portal.md) содержатся рекомендации по удалению приложения из клиента Azure Active Directory.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Я хочу отключить все будущие операции пользователя по предоставлению согласия для всех приложений

Если вы отключите для всего каталога возможность предоставлять согласие, пользователь не сможет согласиться с условиями использования приложения. Администраторы могут по-прежнему предоставлять согласие от имени пользователя. Дополнительные сведения о согласии в приложениях и возможных мотивах для его отключения или предоставления см. в разделе [Получение согласия пользователя и администратора](../develop/howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent). См. также [Типы разрешений и согласие](../develop/v2-permissions-and-consent.md).

Чтобы **отключить все будущие операции по предоставлению согласия пользователя во всем каталоге**, выполните следующие инструкции:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**. 

3.  Щелкните **Корпоративные приложения** в меню навигации.

5.  Щелкните **Параметры пользователя**.

6.  Установите переключатель **Пользователи могут разрешать приложениям доступ к своим данным** на **нет** и нажмите кнопку "Сохранить".


## <a name="next-steps"></a>Дальнейшие действия

[Управление доступом к приложениям](what-is-access-management.md)