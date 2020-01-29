---
title: Аутентификация на основе сертификата в iOS — Azure Active Directory
description: Узнайте о поддерживаемых сценариях и требованиях к настройке аутентификации на основе сертификата в решениях на устройствах iOS.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: annaba
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2f9e7d71ab660c4df6f65d6bebe1d3854086bdd
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2019
ms.locfileid: "74848805"
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>Аутентификация на основе сертификата в Azure Active Directory на устройстве iOS

Для устройств iOS можно применять аутентификацию на основе сертификатов (CBA) в Azure Active Directory. При этом на устройстве используется сертификат клиента при подключении к:

* мобильным приложениям Office, таким как Microsoft Outlook и Microsoft Word;
* клиентам Exchange ActiveSync (EAS).

Настройка данной функции избавляет от необходимости ввода имени пользователя и пароля в определенных почтовых клиентах и приложениях Microsoft Office на мобильных устройствах.

В этой статье приведены требования и поддерживаемые сценарии для настройки CBA на устройстве iOS (Android) для пользователей клиентов в тарифных планах Office 365 корпоративный, бизнес, для образования, для US Gov организаций, Китая и Германии.

В тарифных планах Office 365 US Government Defense и Federal доступна предварительная версия этой функции.

## <a name="microsoft-mobile-applications-support"></a>Поддержка мобильных приложений Microsoft

| Приложения | Поддержка |
| --- | --- |
| Приложение Azure Information Protection |![Установите флажок, обозначающий поддержку для этого приложения][1] |
| Корпоративный портал Intune |![Установите флажок, обозначающий поддержку для этого приложения][1] |
| Microsoft Teams |![Установите флажок, обозначающий поддержку для этого приложения][1] |
| OneNote |![Установите флажок, обозначающий поддержку для этого приложения][1] |
| OneDrive |![Установите флажок, обозначающий поддержку для этого приложения][1] |
| Outlook |![Установите флажок, обозначающий поддержку для этого приложения][1] |
| Power BI |![Установите флажок, обозначающий поддержку для этого приложения][1] |
| Skype для бизнеса |![Установите флажок, обозначающий поддержку для этого приложения][1] |
| Word/Excel/PowerPoint |![Установите флажок, обозначающий поддержку для этого приложения][1] |
| Yammer |![Установите флажок, обозначающий поддержку для этого приложения][1] |

## <a name="requirements"></a>Требования

Устройство должно иметь операционную систему iOS 9 и более поздней версии.

Необходимо настроить сервер федерации.

Для приложений Office на iOS требуется Microsoft Authenticator.

Чтобы служба Azure Active Directory могла отзывать сертификат клиента, маркер AD FS должен иметь следующие утверждения:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (серийный номер сертификата клиента);
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (строка для издателя сертификата клиента).

Azure Active Directory добавляет эти утверждения в маркер обновления, если они доступны в маркере AD FS (или любом другом токене SAML). Когда требуется проверить маркер обновления, эта информация используется для проверки отзыва.

Рекомендуем обновить страницы ошибок AD FS вашей организации следующими сведениями:

* требованием установки Microsoft Authenticator для iOS;
* инструкциями о получении сертификата пользователя.

Дополнительные сведения см. в статье о [настройке страниц входа AD FS](https://technet.microsoft.com/library/dn280950.aspx).

Некоторые приложения Office (с поддержкой современной проверки подлинности) отправляют в Azure AD запрос с текстом *prompt=login*. По умолчанию Azure AD преобразует *prompt=login* в запросе к службам AD FS в текст *wauth=usernamepassworduri* (запрашивает у AD FS аутентификацию с помощью имени пользователя и пароля) и *wfresh=0* (указывает AD FS игнорировать состояние единого входа и выполнять аутентификацию заново). Чтобы включить проверку подлинности на основе сертификатов для этих приложений, необходимо изменить поведение Azure AD по умолчанию. Просто задайте для параметра *PromptLoginBehavior* в настройках федеративного домена значение *Отключено*.
Для выполнения этой задачи можно использовать командлет [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0):

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Поддержка клиентов Exchange ActiveSync

В iOS версии 9 и выше поддерживается собственный почтовый клиент iOS. Чтобы определить, поддерживается ли эта функция во всех остальных приложениях Exchange ActiveSync, обратитесь к разработчику приложения.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы настроить аутентификацию на основе сертификата в своей среде, ознакомьтесь с инструкциями в статье [Get started with certificate-based authentication in Azure Active Directory](../authentication/active-directory-certificate-based-authentication-get-started.md) (Приступая к работе с аутентификацией на основе сертификата в Azure Active Directory).

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
