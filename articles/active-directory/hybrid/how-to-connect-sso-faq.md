---
title: 'Azure AD Connect выполняет следующие функции: простой единый вход — часто задаваемые вопросы | Документация Майкрософт'
description: Ответы на часто задаваемые вопросы о простом едином входе Azure Active Directory.
services: active-directory
keywords: что такое Azure AD Connect, установка Active Directory, необходимые компоненты для Azure AD, единый вход
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/14/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 709fb3be37850be37d6378652921ce26f4ff15fe
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60242231"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Простой единый вход Azure Active Directory: Часто задаваемые вопросы

В этой статье представлены часто задаваемые вопросы о простом едином входе Azure Active Directory. Следите за новым содержимым.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>С какими методами входа работает простой единый вход?

Простой единый вход можно использовать вместе с методами [синхронизация хэша паролей](how-to-connect-password-hash-synchronization.md) или [сквозной проверки подлинности](how-to-connect-pta.md). Однако эту функцию нельзя использовать со службами федерации Active Directory (AD FS).

## <a name="is-seamless-sso-a-free-feature"></a>Простой единый вход — это бесплатная функция?

Это бесплатный компонент, и для его использования не требуются платные выпуски Azure AD.

## <a name="is-seamless-sso-available-in-the-microsoft-azure-germany-cloudhttpswwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Доступен ли компонент "Простой единый вход" в облаке [Microsoft Azure — Германия](https://www.microsoft.de/cloud-deutschland) и [Azure для государственных организаций](https://azure.microsoft.com/features/gov/)?

№ Компонент "Простой единый вход" доступен только в международном экземпляре Azure AD.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Какие приложения используют возможность параметра `domain_hint` или `login_hint` простого единого входа?

Ниже приведен неполный список приложений, которые могут отправить эти параметры в Azure AD и тем самым обеспечить автоматический вход пользователей с помощью простого единого входа (то есть пользователю не требуется вводить имя пользователя или пароль):

| имя приложения; | Используемый URL-адрес приложения |
| -- | -- |
| Панель доступа | https:\//myapps.microsoft.com/contoso.com |
| Outlook on Web | https:\//outlook.office365.com/contoso.com |
| Порталы Office 365 | https:\//portal.office.com?domain_hint=contoso.com, https:\//www.office.com?domain_hint=contoso.com |

Кроме того, пользователи могут использовать автоматический вход, если приложение отправляет запросы на вход в конечные точки Azure AD настроить как клиенты — то есть https:\//login.microsoftonline.com/contoso.com/ <... > или https:\//login.microsoftonline.com/ < tenant_ID > / <... >, вместо Azure AD общую конечную точку — то есть https:\//login.microsoftonline.com/common/ <... >. Ниже приведен неполный список приложений, которые выполняют такие типы запросов на вход.

| имя приложения; | Используемый URL-адрес приложения |
| -- | -- |
| SharePoint Online | https:\//contoso.sharepoint.com |
| Портал Azure | https:\//portal.azure.com/contoso.com |

В приведенных выше таблицах замените contoso.com своим доменным именем, чтобы получить соответствующие URL-адреса приложений для своего клиента.

Если вы хотите использовать автоматический единый вход для других приложений, сообщите нам об этом в разделе отзывов.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Поддерживает ли простой единый вход `Alternate ID` в качестве имени пользователя, а не `userPrincipalName`?

Да. Поддерживает ли простой единый вход `Alternate ID` в качестве имени пользователя, если это настроено в Azure AD Connect, как показано [здесь](how-to-connect-install-custom.md). Не все приложения Office 365 поддерживают `Alternate ID`. Заявление о поддержке см. в документации на приложения.

## <a name="what-is-the-difference-between-the-single-sign-on-experience-provided-by-azure-ad-joinactive-directory-azureadjoin-overviewmd-and-seamless-sso"></a>В чем разница между технологией единого входа, предоставленной после [присоединения к Azure AD](../active-directory-azureadjoin-overview.md), и простым единым входом?

После [присоединения к Azure AD](../active-directory-azureadjoin-overview.md) пользователи могут воспользоваться возможностями единого входа, если их устройства зарегистрированы в Azure AD. Эти устройства не должны быть присоединены к домену. Возможность единого входа обеспечивается с помощью *основных токенов обновления* (*PRT*), а не протокола Kerberos. Лучше всего эта возможность работает на устройствах с Windows 10. В браузере Microsoft Edge единый вход выполняется автоматически. Эта возможность также работает в Chrome с использованием расширения браузера.

В клиенте можно настроить и технологию единого входа, предоставленную после присоединения к Azure AD, и простой единый вход. Эти две возможности дополняют друг друга. Если включены обе эти технологии, приоритет имеет единый вход, предоставленный после присоединения к Azure AD.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Я хочу зарегистрировать устройства, на которых не применяется Windows 10, с помощью Azure AD, без использования AD FS. Можно вместо этого использовать простой единый вход?

Да, для этого сценария требуется версия 2.1 или более поздняя версия [клиента присоединения к рабочей области](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account"></a>Как можно сменить ключ расшифровки Kerberos компьютерной учетной записи `AZUREADSSOACC`?

Очень важно часто менять ключ расшифровки Kerberos компьютерной учетной записи `AZUREADSSOACC` (представляющей Azure AD) в локальном лесу AD.

>[!IMPORTANT]
>Мы настоятельно рекомендуем, чтобы вы меняли ключ расшифровки Kerberos хотя бы раз в 30 дней.

Выполните следующие действия на локальном сервере, на котором выполняется Azure AD Connect:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Шаг 1. Получение списка лесов AD, где включен простой единый вход

1. Сначала скачайте и установите [Azure PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Перейдите в папку `%programfiles%\Microsoft Azure Active Directory Connect`.
3. Импортируйте модуль PowerShell для простого единого входа с помощью следующей команды: `Import-Module .\AzureADSSO.psd1`.
4. Откройте PowerShell от имени администратора. В PowerShell вызовите `New-AzureADSSOAuthenticationContext`. Появится всплывающее окно для ввода учетных данных глобального администратора клиента.
5. Вызовите `Get-AzureADSSOStatus | ConvertFrom-Json`. Эта команда выводит список лесов AD (см. список "Домены"), в которых включена эта функция.

### <a name="step-2-update-the-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>Шаг 2. Обновить ключ расшифровки Kerberos в каждом лесу AD, в котором он был настроен.

1. Вызовите `$creds = Get-Credential`. При запросе введите свои учетные данные администратора домена для нужного леса AD.

   > [!NOTE]
   > Мы используем имя пользователя администратора домена, указанное в формате имени участника-пользователя (johndoe@contoso.com) или формате полного доменного имени учетной записи SAM (contoso\johndoe или contoso.com\johndoe), чтобы найти предполагаемый лес AD. Если вы используете полное доменное имя учетной записи SAM, то мы используем доменную часть имени пользователя, чтобы [найти контроллер домена администратора домена с помощью DNS](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx). Если же вы используете имя участника-пользователя, то мы [преобразуем его в полное доменное имя учетной записи SAM](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa), прежде чем найти соответствующий контроллер домена.

2. Вызовите `Update-AzureADSSOForest -OnPremCredentials $creds`. Эта команда обновляет ключ расшифровки Kerberos для компьютерной учетной записи `AZUREADSSOACC` в этом лесу AD и обновляет его в Azure AD.
3. Повторите предыдущие шаги для каждого леса AD, где настроена эта функция.

>[!IMPORTANT]
>_Не_ выполняйте команду `Update-AzureADSSOForest` более одного раза. В противном случае функция перестанет действовать, пока не истечет срок действия пользовательского билета Kerberos и пока локальный экземпляр Active Directory не выдаст новый билет.

## <a name="how-can-i-disable-seamless-sso"></a>Как отключить простой единый вход?

### <a name="step-1-disable-the-feature-on-your-tenant"></a>Шаг 1. Отключение функции в своем клиенте

#### <a name="option-a-disable-using-azure-ad-connect"></a>Вариант 1. Использование Azure AD Connect

1. Запустите Azure AD Connect, перейдите на страницу **Изменение параметров входа пользователя** и щелкните **Далее**.
2. Затем снимите флажок **Включить единый вход**. Продолжайте выполнять указания мастера.

После завершения работы мастера простой единый вход будет отключен на вашем клиенте. При этом вы увидите на экране следующее сообщение:

"Единый вход теперь отключен, но для полной очистки нужен ряд ручных действий. Подробнее".

Чтобы завершить очистку, выполните шаги 2 и 3 на локальном сервере с запущенным средством Azure AD Connect.

#### <a name="option-b-disable-using-powershell"></a>Вариант 2. Использование PowerShell

Выполните следующие действия на локальном сервере с запущенным средством Azure AD Connect:

1. Сначала скачайте и установите [Azure PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Перейдите в папку `%programfiles%\Microsoft Azure Active Directory Connect`.
3. Импортируйте модуль PowerShell для простого единого входа с помощью следующей команды: `Import-Module .\AzureADSSO.psd1`.
4. Откройте PowerShell от имени администратора. В PowerShell вызовите `New-AzureADSSOAuthenticationContext`. Появится всплывающее окно для ввода учетных данных глобального администратора клиента.
5. Вызовите `Enable-AzureADSSO -Enable $false`.

>[!IMPORTANT]
>Отключение простого единого входа с помощью PowerShell не изменит состояние в Azure AD Connect. Простой единый вход будет отображаться включенным на странице **Изменение параметров входа пользователя**.

### <a name="step-2-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Шаг 2. Получение списка лесов AD, где включен простой единый вход

Выполните задачи 1–4 ниже, если вы отключили простой единый вход с помощью Azure AD Connect. Если вы отключили простой единый вход с помощью PowerShell, сразу переходите к задаче 5 ниже.

1. Сначала скачайте и установите [Azure PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Перейдите в папку `%programfiles%\Microsoft Azure Active Directory Connect`.
3. Импортируйте модуль PowerShell для простого единого входа с помощью следующей команды: `Import-Module .\AzureADSSO.psd1`.
4. Откройте PowerShell от имени администратора. В PowerShell вызовите `New-AzureADSSOAuthenticationContext`. Появится всплывающее окно для ввода учетных данных глобального администратора клиента.
5. Вызовите `Get-AzureADSSOStatus | ConvertFrom-Json`. Эта команда выводит список лесов AD (см. список "Домены"), в которых включена эта функция.

### <a name="step-3-manually-delete-the-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>Шаг 3. Вручную удалите учетную запись компьютера `AZUREADSSOACCT` из каждого леса AD в списке, упомянутом выше.

## <a name="next-steps"></a>Дальнейшие действия

- [**Краткое руководство**](how-to-connect-sso-quick-start.md). Настройка и подготовка к работе простого единого входа Azure AD.
- [**Техническое руководство по сквозной проверке подлинности Azure Active Directory**](how-to-connect-sso-how-it-works.md). Сведения о том, как работает эта функция.
- [**Устранение неполадок**](tshoot-connect-sso.md). Узнайте, как устранить самые распространенные проблемы с этой функцией.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect). Отправка запросов новых функций.
