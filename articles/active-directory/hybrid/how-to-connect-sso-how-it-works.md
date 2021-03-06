---
title: 'Azure AD Connect: простой единый вход — как это работает | Документы Майкрософт'
description: В этой статье описывается принцип работы функции простого единого входа в Azure Active Directory.
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
ms.topic: how-to
ms.date: 04/16/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: bde937adba8d2469390a6cf404f6cce8c5008e87
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86144704"
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Подробное техническое руководство по простому единому входу Azure Active Directory

В этой статье приводятся технические сведения о работе функции простого единого входа Azure Active Directory.

## <a name="how-does-seamless-sso-work"></a>Как работает простой единый вход?

Этот раздел состоит из трех частей.

1. Настройка функции простого единого входа.
2. Как транзакция единого входа пользователя в веб-браузере работает с простым единым входом.
3. Как транзакция единого входа пользователя в собственном клиенте работает с простым единым входом.

### <a name="how-does-set-up-work"></a>Как выполняется настройка?

Простой единый вход включается с помощью Azure AD Connect, как показано [здесь](how-to-connect-sso-quick-start.md). При включении функции выполняются следующие действия:

- Учетная запись компьютера ( `AZUREADSSOACC` ) создается в локальном Active Directory (AD) в каждом лесу AD, который синхронизируется с Azure AD (с помощью Azure AD Connect).
- Кроме того, для использования во время входа в Azure AD создаются несколько имен участников-служб (SPN) Kerberos.
- С помощью Azure AD безопасным образом предоставляется ключ расшифровки Kerberos учетной записи компьютера. Если существует несколько лесов AD, каждая учетная запись компьютера будет иметь собственный уникальный ключ расшифровки Kerberos.

>[!IMPORTANT]
> `AZUREADSSOACC`Учетная запись компьютера должна быть строго защищена по соображениям безопасности. Только администраторы домена должны иметь возможность управлять учетной записью компьютера. Убедитесь, что делегирование Kerberos для учетной записи компьютера отключено и что другие учетные записи в Active Directory не имеют разрешений на делегирование `AZUREADSSOACC` учетной записи компьютера. Храните учетную запись компьютера в подразделении (OU), где они защищены от случайного удаления и когда доступ имеют только администраторы домена. Ключ расшифровки Kerberos в учетной записи компьютера также должен рассматриваться как конфиденциальный. Настоятельно рекомендуется, чтобы вы [меняли ключ расшифровки Kerberos](how-to-connect-sso-faq.md) для учетной записи компьютера `AZUREADSSOACC` хотя бы раз в 30 дней.

>[!IMPORTANT]
> Простой единый вход поддерживает AES256_HMAC_SHA1, AES128_HMAC_SHA1 и RC4_HMAC_MD5 типы шифрования для Kerberos. Рекомендуется, чтобы для учетной записи AzureADSSOAcc $ было задано значение AES256_HMAC_SHA1 или один из типов AES vs. RC4 для дополнительной защиты. Тип шифрования хранится в атрибуте msDS-Суппортеденкриптионтипес учетной записи в Active Directory.  Если для параметра Тип шифрования учетной записи AzureADSSOAcc $ задано значение RC4_HMAC_MD5 и вы хотите изменить его на один из типов шифрования AES, убедитесь, что вы сначала перейдете за ключ расшифровки Kerberos учетной записи AzureADSSOAcc $, как описано в [документе вопросы и ответы](how-to-connect-sso-faq.md) по соответствующему вопросу, в противном случае простой единый вход не будет выполняться.

После завершения настройки функция простого входа в Azure AD работает так же, как любая другая функция входа, использующая встроенную проверку подлинности Windows (IWA).

### <a name="how-does-sign-in-on-a-web-browser-with-seamless-sso-work"></a>Как работает вход с функцией простого единого входа в веб-браузере?

Процесс входа в веб-браузере выглядит следующим образом.

1. Пользователь пытается получить доступ к веб-приложению (например, веб-приложению Outlook — https://outlook.office365.com/owa/) с корпоративного устройства, присоединенного к домену, в корпоративной сети.
2. Если пользователь еще не выполнил вход, он перенаправляется на страницу входа в Azure AD.
3. Пользователь вводит свое имя на странице входа в Azure AD.

   >[!NOTE]
   >Для [некоторых приложений](./how-to-connect-sso-faq.md) шаги 2 и 3 пропускаются.

4. С помощью JavaScript, выполняемого в фоновом режиме, Azure AD запрашивает у браузера билет Kerberos (возвращается ответ 401 — не авторизовано).
5. В свою очередь браузер запрашивает в Active Directory билет для учетной записи компьютера `AZUREADSSOACC` (которая представляет Azure AD).
6. Active Directory находит учетную запись компьютера и возвращает браузеру билет Kerberos, зашифрованный с использованием секрета этой учетной записи компьютера.
7. Браузер пересылает билет Kerberos, полученный от Active Directory, в Azure AD.
8. Azure AD расшифровывает билет Kerberos, который содержит удостоверение пользователя, выполнившего вход в корпоративное устройство, с помощью полученного ранее общего ключа.
9. После анализа Azure AD либо возвращает маркер приложению, либо предлагает пользователю выполнить дополнительную проверку, например многофакторную проверку подлинности.
10. При успешном входе в систему пользователь может получить доступ к приложению.

На схеме ниже показаны все соответствующие компоненты и шаги.

![Простой единый вход: процедура для веб-приложения](./media/how-to-connect-sso-how-it-works/sso2.png)

Простой единый вход — ситуативно-обусловленная функция. Это означает, что если в ней происходит сбой, процедура входа выполняется стандартно, то есть пользователь, как и прежде, должен просто ввести пароль для входа.

### <a name="how-does-sign-in-on-a-native-client-with-seamless-sso-work"></a>Как работает вход с функцией простого единого входа в собственном клиенте?

Процесс входа в собственном клиенте выглядит следующим образом.

1. Пользователь пытается получить доступ к собственному приложению (например, клиенту Outlook) с корпоративного устройства, присоединенного к домену, в корпоративной сети.
2. Если пользователь еще не выполнил вход, то собственное приложение получает его имя пользователя из сеанса Windows на устройстве.
3. Приложение отправляет это имя пользователя в Azure AD и получает конечную точку MEX WS-Trust клиента. Конечная точка WS-Trust используется только функцией простого единого входа и не является общей реализацией протокола WS-Trust в Azure AD.
4. Затем приложение отправляет запрос к конечной точке MEX WS-Trust, чтобы определить, доступна интегрированная конечная точка аутентификации. Интегрированная конечная точка аутентификации используется только функцией простого единого входа.
5. Если шаг 4 завершается успешно, то выдается запрос защиты Kerberos.
6. Если приложению удалось получить билет Kerberos, оно пересылает его на интегрированную конечную точку аутентификации Azure AD.
7. Служба Azure AD расшифровывает билет Kerberos и проверяет его.
8. Azure AD выполняет вход пользователя и выдает токен SAML для приложения.
9. Приложение отправляет токен SAML на конечную точку маркера OAuth 2 Azure AD.
10. Azure AD проверяет токен SAML и выдает приложению маркер доступа и маркер обновления для указанного ресурса, а также маркер идентификации.
11. Пользователь получает доступ к ресурсу приложения.

На схеме ниже показаны все соответствующие компоненты и шаги.

![Простой единый вход: процедура для собственного приложения](./media/how-to-connect-sso-how-it-works/sso14.png)

## <a name="next-steps"></a>Дальнейшие действия

- [**Краткое руководство**](how-to-connect-sso-quick-start.md). Настройка и подготовка к работе простого единого входа Azure AD.
- [**Часто задаваемые вопросы**](how-to-connect-sso-faq.md). Ответы на часто задаваемые вопросы.
- [**Устранение неполадок**](tshoot-connect-sso.md). Узнайте, как устранить самые распространенные проблемы с этой функцией.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect). Отправка запросов новых функций.
