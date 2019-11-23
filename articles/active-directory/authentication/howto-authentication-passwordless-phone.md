---
title: Passwordless sign-in with the Microsoft Authenticator app - Azure Active Directory
description: Enable passwordless sign-in to Azure AD using the Microsoft Authenticator app (preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: a76aa0ca7cbda3f2db564c220ba12fec60f60509
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74381863"
---
# <a name="enable-passwordless-sign-in-with-the-microsoft-authenticator-app-preview"></a>Enable passwordless sign-in with the Microsoft Authenticator app (preview)

Приложение Microsoft Authenticator позволяет войти в любую учетную запись Azure AD, не используя пароль. Как и аналогичная технология [Windows Hello для бизнеса](/windows/security/identity-protection/hello-for-business/hello-identity-verification), Microsoft Authenticator использует аутентификацию на основе ключей, привязывает учетные данные пользователя к определенному устройству и применяет биометрические данные или ПИН-код. This authentication method can be used on any device platform, including mobile, and with any app or website that integrates with Microsoft authentication libraries. 

![Example of a browser sign-in asking for user to approve the sign-in](./media/howto-authentication-passwordless-phone/phone-sign-in-microsoft-authenticator-app.png)

Instead of seeing a prompt for a password after entering a username, a person who has enabled phone sign-in from the Microsoft Authenticator app will see a message telling them to tap a number in their app. Пользователь должен передать этот номер в приложение, нажать кнопку подтверждения и предоставить ПИН-код или биометрические данные, чтобы завершить процесс аутентификации.

> [!NOTE]
> This capability has been in the Microsoft Authenticator app since March of 2017, so there is a possibility that when the policy is enabled for a directory, users may encounter this flow immediately, and see an error message if they have not been enabled by policy. Учитывайте это и не забудьте предупредить пользователей.

## <a name="prerequisites"></a>Технические условия

- Azure Multi-Factor Authentication, with push notifications allowed as a verification method 
- Последняя версия Microsoft Authenticator на устройствах под управлением iOS версии 8.0 или более поздней версии или Android 6.0 или более поздней версии.

> [!NOTE]
> If you enabled the previous Microsoft Authenticator app passwordless sign-in preview using Azure AD PowerShell, it was enabled for your entire directory. If you enable using this new method, it will supercede the PowerShell policy. We recommend enabling for all users in your tenant via the new Authentication Methods, otherwise users not in the new policy will no longer be able to log in passwordlessly. 

## <a name="enable-passwordless-authentication-methods"></a>Enable passwordless authentication methods

### <a name="enable-the-combined-registration-experience"></a>Enable the combined registration experience

Registration features for passwordless authentication methods rely on the combined registration preview. Follow the steps in the article [Enable combined security information registration (preview)](howto-registration-mfa-sspr-combined.md), to enable the combined registration preview.

### <a name="enable-passwordless-phone-sign-in-authentication-methods"></a>Enable passwordless phone sign-in authentication methods

1. Войдите на [портал Azure](https://portal.azure.com)
1. Найдите и выберите *Azure Active Directory*. Select **Security** > **Authentication methods** > **Authentication method policy (Preview)**
1. Under **Passwordless phone sign-in**, choose the following options
   1. **Enable** - Yes or No
   1. **Target** - All users or Select users
1. **Save** to set the new policy

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>User registration and management of Microsoft Authenticator app

1. Перейдите на сайт [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo).
1. Sign in if not already
1. Add an authenticator app by clicking **Add method**, choosing **Authenticator app**, and clicking **Add**
1. Follow the instructions to install and configure the Microsoft Authenticator app on your device
1. Click **Done** to complete Authenticator MFA app setup flow. 
1. In **Microsoft Authenticator**, choose **Enable phone sign-in** from the account drop-down menu
1. Follow the instructions in the app to finish registering for passwordless phone sign-in. 

Organizations can point their users to the article [Sign in with your phone, not your password](../user-help/microsoft-authenticator-app-phone-signin-faq.md) for further assistance setting up in the Microsoft Authenticator app and enabling phone sign-in.

## <a name="sign-in-with-passwordless-credential"></a>Sign in with passwordless credential

На этапе общедоступной предварительной версии у вас нет возможности обязать пользователей создавать или использовать новый тип учетных данных. A user will only encounter passwordless sign-in once an admin has enabled their tenant **and** the user has updated their Microsoft Authenticator app to enable phone sign-in.

After typing your username on the web and selecting **Next**, users are presented with a number and are prompted in their Microsoft Authenticator app to select the appropriate number to authenticate instead of using their password. 

![Example of a browser sign-in using the Microsoft Authenticator app](./media/howto-authentication-passwordless-phone/web-sign-in-microsoft-authenticator-app.png)

## <a name="known-issues"></a>Известные проблемы

### <a name="user-is-not-enabled-by-policy-but-still-has-passwordless-phone-sign-in-method-in-microsoft-authenticator"></a>User is not enabled by policy but still has passwordless phone sign-in method in Microsoft Authenticator

It is possible that a user has at some point created a passwordless phone sign-in credential in their current Microsoft Authenticator app, or on an earlier device. Once an admin enables the authentication method policy for passwordless phone sign-in, any user with a credential registered, will start to experience the new sign-in prompt, regardless of whether they have been enabled to use the policy or not. If the user has not been allowed to use the credential by policy, they will see an error after completing the authentication flow. 

The admin can choose to enable the user to use passwordless phone sign-in, or the user must remove the method. If the user no longer has the registered device, they can go to [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) and remove it. If they are still using the Authenticator for MFA, they can choose **Disable phone sign-in** from within the Microsoft Authenticator.  

### <a name="ad-fs-integration"></a>AD FS integration

Когда пользователь включит учетные данные Microsoft Authenticator без пароля, аутентификация этого пользователя по умолчанию всегда будет выполняться через уведомление для подтверждения. Эта логика предотвращает перенаправление пользователя гибридного клиента в ADFS для проверки входа, если этот пользователь не выбрал действие "Использовать свой пароль". Этот процесс также будут обходить любые локальные политики условного доступа и потоки сквозной проверки подлинности. 

If a user has an unanswered passwordless phone sign-in verification pending and attempts to sign in again, the user may be taken to ADFS to enter a password instead.  

### <a name="azure-mfa-server"></a>Сервер Azure MFA

End users who are enabled for MFA through an organization’s on-premises Azure MFA server can still create and use a single passwordless phone sign in credential. Если пользователь введет одну учетную запись в несколько (5 и более) установок Microsoft Authenticator, это может привести к ошибке.  

### <a name="device-registration"></a>Регистрация устройства

One of the prerequisites to create this new strong credential, is that the device, where the Microsoft Authenticator app is installed, must also be registered within the Azure AD tenant to an individual user. Due to current device registration restrictions, a device can only be registered in a single tenant. Это означает, что в приложении Microsoft Authenticator можно настроить только одну рабочую или учебную учетную запись для входа с телефона.

> [!NOTE]
> Device registration is not the same as device management or "MDM." It only associates a device ID and a user ID together in the Azure AD directory.  

## <a name="next-steps"></a>Дальнейшие действия

[What is passwordless?](concept-authentication-passwordless.md)

[Сведения о регистрации устройств](../devices/overview.md#getting-devices-in-azure-ad)

[Сведения о многофакторной идентификации Azure](../authentication/howto-mfa-getstarted.md)
