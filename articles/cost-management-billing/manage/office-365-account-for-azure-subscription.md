---
title: Регистрация в Azure с помощью учетной записи Office 365 | Документация Майкрософт
description: Узнайте, как создать подписку Azure с помощью учетной записи Office 365
services: ''
documentationcenter: ''
author: JiangChen79
manager: adpick
editor: ''
tags: billing,top-support-issue
ms.assetid: 129cdf7a-2165-483d-83e4-8f11f0fa7f8b
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: banders
ms.openlocfilehash: 612c9d3e66a54e248c011e8dbedec2e2291b6a8f
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75988127"
---
# <a name="sign-up-for-an-azure-subscription-with-your-office-365-account"></a>Регистрация для получения подписки Azure с помощью учетной записи Office 365
Если у вас есть подписка Office 365, то вы можете использовать учетную запись Office 365 для создания подписки Azure. Войдите на [портал Azure](https://portal.azure.com/), используя имя пользователя и пароль Office 365. Если вы хотите настроить виртуальные машины или использовать другие службы Azure, то необходимо зарегистрироваться для получения подписки Azure. Вы можете предоставить другим пользователям доступ к своей подписке Azure, а также [использовать управление доступом на основе ролей для управления доступом к подписке и ресурсам Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

При наличии учетной записи Office 365 и подписки Azure см. статью [Связывание клиента Office 365 с подпиской Azure](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="get-an-azure-subscription-using-your-office-365-account"></a>Получение подписки Azure с помощью учетной записи Office 365

Зарегистрировавшись в Azure с помощью имени пользователя и пароля Office 365 вы сэкономьте время и предотвратите увеличение числа учетных записей.

1. Зарегистрируйтесь на сайте [Azure.com](https://account.azure.com/signup?offer=MS-AZR-0044p&appId=docs).
2. Войдите, используя имя пользователя и пароль Office 365. Используемой учетной записи не нужны права администратора. Если у вас есть несколько учетных записей Office 365, убедитесь, что используются учетные данные учетной записи Office 365, которую требуется связать с подпиской Azure.

   ![Снимок экрана, на котором показана страница входа.](./media/office-365-account-for-azure-subscription/billing-sign-in-with-office-365-account.png)

3. Введите необходимые сведения и завершите процесс регистрации. Некоторые данные можно не указывать, если у вас уже есть учетная запись Office 365.

    ![Снимок экрана, на котором показана форма регистрации.](./media/office-365-account-for-azure-subscription/billing-azure-sign-up-fill-information.png)

- Если необходимо добавить других пользователей организации в подписку Azure, то см. статью [Начало работы с управлением доступом на портале Azure](../../role-based-access-control/overview.md).

## <a id="more-about-subs">Справочная информация о подписках Azure и Office 365</a>
Office 365 и Azure используют службу Azure AD для управления пользователями и подписками. Считайте, что каталог Azure — это контейнер, в котором можно группировать пользователей и подписки. Чтобы использовать одну учетную запись пользователя для подписок Azure и Office 365, подписки Azure должны быть созданы в одном каталоге с подписками Office 365. Помните на следующие моменты.

* Подписка создается в каталоге.
* Пользователи принадлежат к каталогам.
* Подписка сохраняется в каталоге пользователя, который ее создал. Подписка Office 365 и Azure привязаны к одной учетной записи.
* Подписки Azure принадлежат отдельным пользователям в каталоге.
* Подписки Office 365 принадлежат самому каталогу. Пользователи с соответствующими разрешениями в каталоге могут управлять этими подписками.

![Снимок экрана, на котором показана связь между каталогом, пользователями и подписками.](./media/office-365-account-for-azure-subscription/19-background-information.png)

Дополнительные сведения см. в статье [Связь между подписками Azure и службой Azure Active Directory](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="need-help-contact-us"></a>Нужна помощь? Связаться с нами.

Если у вас есть вопросы или вам нужна помощь, [создайте запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).
