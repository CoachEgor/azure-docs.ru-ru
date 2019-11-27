---
title: Добавление ролей приложения в зарегистрированное Azure Active Directory приложении и получение их в токене
titleSuffix: Microsoft identity platform
description: Сведения о добавлении ролей приложения в приложение, зарегистрированное в Azure Active Directory, назначении ролей для пользователей и групп и их получение в утверждении `roles` в токене.
services: active-directory
documentationcenter: ''
author: kkrishna
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84275be5c85570a44d968dca8887b448fce09979
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2019
ms.locfileid: "74546840"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Практическое руководство. Добавление ролей приложения в приложение, зарегистрированное в Azure Active Directory, и их получение в токене

Управление доступом на основе ролей (RBAC) — это популярный механизм для принудительного применения авторизации в приложениях. При использовании RBAC администратор предоставляет разрешения ролям, а не отдельным пользователям или группам. Затем администратор может назначить роли для различных пользователей и групп, чтобы контролировать доступ к соответствующему содержимому и функциональным возможностям.

Используя RBAC с ролями приложений и утверждениями ролей, разработчики могут безопасно обеспечить авторизацию в приложениях, приложив немного усилий.

Другим подходом является использование групп Azure AD и утверждений групп, как показано в репозитории [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet). Группы и роли приложений Azure AD не являются взаимоисключающими. Их можно использовать совместно для обеспечения более точного управления доступом.

## <a name="declare-roles-for-an-application"></a>Объявление ролей для приложения

Эти роли приложения определяются на [портале Azure](https://portal.azure.com) в манифесте регистрации приложения.  Когда пользователь входит в приложение, Azure AD выдает утверждение `roles` для каждой отдельной роли пользователя и ролей, имеющихся у него как у члена группы.  Назначение пользователей и групп ролям можно выполнить через пользовательский интерфейс на портале или программным путем, используя [Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/azuread-identity-access-management-concept-overview).

### <a name="declare-app-roles-using-azure-portal"></a>Объявление ролей приложения, использующих портал Azure

1. Войдите на [портале Azure](https://portal.azure.com).
1. На верхней панели выберите свою учетную запись, а затем щелкните **Переключение каталога**.
1. Когда откроется панель **Каталог и подписка**, выберите клиент Active Directory, в котором необходимо зарегистрировать приложение, из списка **Favorites** (Избранное) или **Все каталоги**.
1. В меню навигации слева щелкните **Все службы** и выберите **Azure Active Directory**.
1. На панели **Azure Active Directory** выберите **Регистрация приложений**, чтобы просмотреть список всех приложений.
1. Выберите приложение, в котором требуется определить роли приложения. Затем выберите **Манифест**.
1. Измените манифест приложения. Для этого найдите параметр `appRoles` и добавьте все имеющиеся роли приложения.

     > [!NOTE]
     > Каждое определение роли приложения в этом манифесте должно иметь другой допустимый идентификатор GUID для свойства `id`. 
     > 
     > Свойство `value` каждого определения роли приложения должно точно соответствовать строкам, используемым в коде приложения. Свойство `value` не может содержать пробелы. Если это так, при сохранении манифеста появится сообщение об ошибке.
     
1. Сохраните манифест.

### <a name="examples"></a>Примеры

В следующем примере показана роль `appRoles`, которую можно назначить для `users`.

> [!NOTE]
>`id` должен быть уникальным идентификатором GUID.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-98f8-45fd-aa4a-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

> [!NOTE]
>`displayName` не может содержать пробелы.

Вы можете определить роли приложения, чтобы выбрать `users`, `applications` или обе эти роли. Если они доступны для `applications`, роли приложения отображаются как разрешения приложения в колонке **Необходимые разрешения**. В следующем примере показана роль приложения, предназначенная для `Application`.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "ConsumerApps",
      "id": "47fbb575-859a-4941-89c9-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

Число определенных ролей влияет на пределы манифеста приложения. Они подробно описаны на странице [ограничения манифеста](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#manifest-limits) .

### <a name="assign-users-and-groups-to-roles"></a>Назначение ролей для пользователей и групп

После добавления в приложение роли приложения эти роли можно назначать для пользователей и групп.

1. На панели **Azure Active Directory** выберите **Корпоративные приложения** в меню навигации слева **Azure Active Directory**.
1. Щелкните **Все приложения**, чтобы просмотреть полный список приложений.

     Если приложение не отображается, используйте различные фильтры в верхней части списка **Все приложения**, чтобы сократить список, или прокрутите его вниз и найдите приложение.

1. Выберите приложение, в котором для ролей необходимо назначить пользователей или группы безопасности.
1. Щелкните панель **Пользователи и группы** в меню навигации приложения слева.
1. Вверху списка **Пользователи и группы** нажмите кнопку **Добавить пользователя**, чтобы открыть панель **Добавление назначения**.
1. В области **Добавление назначения** щелкните **Пользователи и группы**.

     Отобразится список пользователей и групп безопасности, а также текстовое поле для поиска определенного пользователя или группы. На этом экране можно выбрать сразу несколько пользователей и групп.

1. Выбрав пользователей и группы, нажмите кнопку **Выбрать** внизу, чтобы перейти к следующей части.
1. Щелкните селектор **Выбор роли** в области **Добавление назначения**. Отобразятся все роли, объявленные ранее в манифесте приложения.
1. Выберите роль и нажмите кнопку **Выбрать**.
1. Нажмите кнопку **Назначить** внизу, чтобы завершить назначение пользователей и групп для приложения.
1. Убедитесь, что добавленные пользователи и группы отображаются в обновленном списке **Пользователи и группы**.

## <a name="more-information"></a>Дополнительные сведения

- [Authorization in a web app using Azure AD application roles &amp; role claims (Sample)](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) (Авторизация в веб-приложении с помощью ролей приложения и утверждений ролей в Azure AD (пример))
- [August 9: Using application roles and security groups in your apps (Video)](https://www.youtube.com/watch?v=V8VUPixLSiM) (9 августа. Использование ролей приложения и групп безопасности в приложениях (видео))
- [Azure Active Directory, now with Group Claims and Application Roles](https://cloudblogs.microsoft.com/enterprisemobility/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles) (Azure Active Directory с утверждениями групп и ролями приложения)
- [Манифест приложения Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)
- [Маркеры доступа AAD](access-tokens.md)
- [AAD`id_tokens`](id-tokens.md)
