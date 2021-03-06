---
title: Назначение или удаление лицензий Azure Active Directory | Документация Майкрософт
description: Инструкции по назначению или удалению лицензий Azure Active Directory для пользователей и групп.
services: active-directory
author: ajburnle
manager: daveba
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
ms.date: 12/14/2020
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: b6900647acf7182529f34c8cc065dbb039de38be
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2020
ms.locfileid: "97504412"
---
# <a name="assign-or-remove-licenses-in-the-azure-active-directory-portal"></a>Назначение или удаление лицензий на портале Azure Active Directory

Многие службы Azure Active Directory (Azure AD) нуждаются в лицензировании каждого из пользователей или групп (а также связанных членов) для этой службы. Только пользователи с активными лицензиями смогут получить доступ и использовать лицензированные службы Azure AD, для которых это верно. Лицензии применяются для каждого клиента и не переносятся в другие клиенты. 

## <a name="available-license-plans"></a>Доступные планы лицензирования

Для службы Azure AD доступно несколько планов лицензирования, в том числе:

- Azure AD уровня "Бесплатный"

- Azure AD Premium P1

- Azure AD Premium P2

Конкретные сведения о каждом плане лицензии и связанных с ней сведениях о лицензировании см. в разделе [какая лицензия необходима?](https://azure.microsoft.com/pricing/details/active-directory/) Чтобы зарегистрироваться в планах лицензий Azure AD Premium, см. [здесь](./active-directory-get-started-premium.md).

Некоторые службы Майкрософт доступны не во всех регионах. Прежде чем назначать лицензию группе, вы должны указать **Место использования** для всех участников. Это значение можно задать в разделе **Azure Active Directory &gt; Пользователи &gt; Профиль &gt; Параметры** в Azure AD. Любой пользователь, расположение которого не указано, наследует расположение Организации Azure AD.

## <a name="view-license-plans-and-plan-details"></a>Просмотр планов лицензии и сведений о плане

Вы можете просматривать доступные планы обслуживания, включая отдельные лицензии, проверять сроки действия, ожидающие окончания, и просматривать количество доступных назначений.

### <a name="to-find-your-service-plan-and-plan-details"></a>Поиск плана обслуживания и сведений о плане

1. Войдите в [портал Azure](https://portal.azure.com/) с помощью учетной записи администратора лицензий в вашей организации Azure AD.

1. Выберите **Azure Active Directory**, а затем щелкните **Лицензии**.

    :::image type="content" source="media/license-users-groups/license-details-blade.png" alt-text="Страница лицензий с количеством приобретенных служб и назначенными лицензиями":::

1. Выберите **все продукты** , чтобы просмотреть страницу все продукты и просмотреть **итоговые**, **назначенные**, **Доступные** и **просроченные номера скоростей** для ваших планов лицензирования.

    :::image type="content" source="media/license-users-groups/license-products-blade-with-products.png" alt-text="Страница &quot;службы&quot; — с планами лицензий на службы — связанные сведения о лицензиях":::

    > [!NOTE]
    > Числа определяются следующим образом: 
    > - Всего: общее количество приобретенных лицензий
    > - Назначено: число лицензий, назначенных пользователям
    > - Доступно: количество лицензий, доступных для назначения, включая срок действия которых скоро истекает
    > - Срок действия скоро истекает: число лицензий, срок действия которых скоро истекает

1. Выберите имя плана, чтобы просмотреть его лицензированных пользователей и группы.

## <a name="assign-licenses-to-users-or-groups"></a>Назначение лицензий пользователям или группам

Убедитесь, что все пользователи, которым необходимо использовать лицензированные службы Azure AD, имеют соответствующую лицензию. Вы можете добавить права на лицензирование пользователям или ко всей группе.

### <a name="to-assign-a-license-to-a-user"></a>Назначение лицензии пользователю

1. На странице **продукты** выберите имя плана лицензирования, который вы хотите назначить пользователю.

    ![Страница "службы" с выделенным планом лицензирования службы](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. На странице Обзор плана лицензирования выберите **назначить**.

    ![Страница "службы" с выделенным параметром "назначить"](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. На странице **Назначить** выберите **Пользователи и группы**, а затем найдите и выберите пользователя, которому вы хотите назначить лицензию.

    ![Страница "Назначить лицензию" с выделенными параметрами "Поиск" и "Выбрать"](media/license-users-groups/assign-license-blade-with-highlight.png)

1. Выберите **Варианты назначения**, включите нужные варианты лицензии и нажмите **OK**.

    ![Страница параметров лицензии со всеми параметрами, доступными в плане лицензии](media/license-users-groups/license-option-blade-assignments.png)

    Страница **Назначить лицензию** обновится, и вы увидите выбранного пользователя и настроенные назначения.

    > [!NOTE]
    > Некоторые службы Майкрософт доступны не во всех регионах. Прежде чем назначать лицензию, вы должны указать для пользователя **Место использования**. Это значение можно задать в разделе **Azure Active Directory &gt; Пользователи &gt; Профиль &gt; Параметры** в Azure AD. Любой пользователь, расположение которого не указано, наследует расположение Организации Azure AD.

1. Выберите **Назначить**.

    Пользователь добавляется в список лицензированных пользователей и получает доступ к входящим в лицензию службам Azure AD.
    > [!NOTE]
    > Лицензии также можно назначать непосредственно пользователю на странице **лицензий** пользователя. Если пользователю назначена лицензия с помощью членства в группе и вы хотите назначить эту же лицензию непосредственно пользователю, это можно сделать только на странице **продукты** , упомянутой в шаге 1.

### <a name="to-assign-a-license-to-a-group"></a>Назначение лицензии группе

1. На странице **продукты** выберите имя плана лицензирования, который вы хотите назначить пользователю.

    ![Колонка "продукты" с выделенным планом лицензирования продукта](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. На странице **Azure Active Directory Premium (план 2)** выберите **Назначить**.

    ![Страница "Продукты" с выделенным вариантом "Назначить"](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. На странице **Назначить** выберите **Пользователи и группы**, а затем найдите и выберите группу, которой вы хотите назначить лицензию.

    ![Страница "Назначение лицензии" с выделенным поиском и выбор параметров 2](media/license-users-groups/assign-group-license-blade-with-highlight.png)

1. Выберите **Варианты назначения**, включите нужные варианты лицензии и нажмите **OK**.

    ![Страница "лицензионный режим" со всеми параметрами, доступными в лицензионном плане 2](media/license-users-groups/license-option-blade-group-assignments.png)

    Страница **Назначить лицензию** обновится, и вы увидите выбранного пользователя и настроенные назначения.

1. Выберите **Назначить**.

    Группа будет добавлена в список лицензированных групп, и все ее участники получат доступ к входящим в лицензию службам Azure AD.

## <a name="remove-a-license"></a>Удаление лицензии

Вы можете удалить лицензию на странице пользователя Azure AD на странице "Обзор группы" для назначения группы или на странице **лицензий** Azure AD, чтобы просмотреть пользователей и группы для лицензии.

### <a name="to-remove-a-license-from-a-user"></a>Удаление лицензии с пользователя

1. На странице **лицензированные пользователи** для плана обслуживания выберите пользователя, который больше не должен иметь лицензию. Например, _Alain Charon_.

1. Выберите **Удалить лицензию**.

    ![Страница "Лицензированные пользователи" с выделенным параметром "Удалить лицензию"](media/license-users-groups/license-products-user-blade-with-remove-option-highlight.png)

> [!IMPORTANT]
> Лицензии, которые пользователь наследует от группы, нельзя удалить напрямую. Вместо этого нужно удалить пользователя из группы, от которой наследуется эта лицензия.

### <a name="to-remove-a-license-from-a-group"></a>Удаление лицензии для группы

1. На странице **лицензированные группы** для плана лицензирования выберите группу, которая больше не должна иметь лицензию.

1. Выберите **Удалить лицензию**.

    ![Страница лицензированных групп с выделенным вариантом "удалить лицензию" 2](media/license-users-groups/license-products-group-blade-with-remove-option-highlight.png)
    
    > [!NOTE]
    > Если локальная учетная запись пользователя, синхронизированная с Azure AD, выходит за пределы области действия синхронизации или при удалении синхронизации, то пользователь обратимо удаляется в Azure AD. В этом случае лицензии, назначенные пользователю напрямую или через лицензирование на основе группы, будут помечены как **приостановленные** , а не **удалены**.

## <a name="next-steps"></a>Дальнейшие действия

После назначения лицензий можно выполнять следующие операции:

- [Поиск и устранение проблем с назначением лицензий](../enterprise-users/licensing-groups-resolve-problems.md)

- [Добавление лицензированных пользователей в группу для лицензирования](../enterprise-users/licensing-groups-migrate-users.md)

- [Сценарии, ограничения и известные проблемы при использовании групп для управления лицензированием в Azure Active Directory](../enterprise-users/licensing-group-advanced.md)

- [Добавление или изменение данных профиля](active-directory-users-profile-azure-portal.md)
