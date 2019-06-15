---
title: Выполнение единого входа в приложения с помощью прокси приложения Azure AD | Документация Майкрософт
description: Включите единый вход для опубликованных локальных приложений с помощью прокси приложения Azure AD на портале Azure.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c19550adf500ba91462af12b4c5f7f5e38240e67
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65783516"
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Хранение паролей для единого входа с помощью прокси приложения

Прокси приложения Azure Active Directory помогает повысить производительность путем публикации локальных приложений, чтобы предоставить удаленным сотрудникам безопасный доступ к ним. На портале Azure также можно настроить единый вход (SSO) в эти приложения. Пользователям нужно только пройти аутентификацию в Azure AD, чтобы получить доступ к корпоративным приложениям без необходимости повторного входа.

Прокси приложения поддерживает несколько [режимов единого входа](what-is-single-sign-on.md#choosing-a-single-sign-on-method). Вход по паролю предназначен для приложений, использующих для аутентификации сочетание имени пользователя и пароля. Если настроить вход по паролю для приложения, то пользователям нужно будет один раз войти в локальное приложение. После этого Azure Active Directory сохранит информацию о входе и автоматически предоставит ее приложению, когда пользователи будут подключаться к нему удаленно. 

Вы должны были опубликовать и протестировать свое приложение с помощью прокси приложения. В противном случае выполните действия, описанные в статье [Публикация приложений с помощью прокси приложения Azure AD](application-proxy-add-on-premises-application.md), а затем вернитесь к этой статье. 

## <a name="set-up-password-vaulting-for-your-application"></a>Настройка хранения пароля для приложения

1. Войдите на [портал Azure](https://portal.azure.com) с использованием учетной записи администратора.
2. Выберите **Azure Active Directory** > **Корпоративные приложения** > **Все приложения**.
3. В списке выберите приложение, для которого необходимо настроить единый вход.  
4. Щелкните **Единый вход**.

   ![Выбор пункта "Единый вход"](./media/application-proxy-configure-single-sign-on-password-vaulting/select-sso.png)

5. В качестве режима единого входа выберите **Вход по паролю**.
6. В поле "URL-адрес входа" введите URL-адрес страницы, на которой пользователи вводят имена пользователей и пароли для входа в приложение извне корпоративной сети. Это может быть внешний URL-адрес, созданный при публикации приложения через прокси приложения. 

   ![Выбор входа по паролю и ввод URL-адрес](./media/application-proxy-configure-single-sign-on-password-vaulting/password-sso.png)

7. Щелкните **Сохранить**.

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Тестирование приложения

Перейдите по внешнему URL-адресу, настроенному для удаленного доступа к приложению. Выполните вход, используя учетные данные для этого приложения (или учетные данные тестовой учетной записи, для которой настроен доступ). После успешного входа вы сможете выйти из приложения, а затем вернуться в него, не вводя учетные данные снова. 

## <a name="next-steps"></a>Дальнейшие действия

- Прочитайте о других способах реализации [единого входа](what-is-single-sign-on.md).
- Изучите [вопросы безопасности при удаленном доступе к приложениям через прокси приложения Azure AD](application-proxy-security.md).
