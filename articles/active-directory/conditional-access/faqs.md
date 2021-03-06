---
title: Azure Active Directory часто задаваемые вопросы о условном доступе | Документация Майкрософт
description: Получите ответы на часто задаваемые вопросы об условном доступе в Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: troubleshooting
ms.date: 10/16/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: 8043e85db1ee1aadc814e98db12cab10ec17e129
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2020
ms.locfileid: "92145230"
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Azure Active Directory часто задаваемые вопросы о условном доступе

## <a name="which-applications-work-with-conditional-access-policies"></a>Какие приложения работают с политиками условного доступа?

Сведения о приложениях, которые работают с политиками условного доступа, см. [в разделе приложения и браузеры, использующие правила условного доступа в Azure Active Directory](concept-conditional-access-cloud-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Применяются ли политики условного доступа к службе совместной работы B2B и гостевым пользователям?

Политики применяются для пользователей службы совместной работы B2B. Однако в некоторых случаях пользователь может не иметь возможности соответствовать требованиям политики. Например, организация гостевого пользователя может не поддерживать многофакторную проверку подлинности. 

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Распространяется ли политика SharePoint Online на OneDrive для бизнеса?

Да. Политика SharePoint Online также распространяется на OneDrive для бизнеса. Дополнительные сведения см. в статье [зависимости службы условного доступа](service-dependencies.md) и рассмотрите политики назначения для [приложения Office 365](concept-conditional-access-cloud-apps.md#office-365) .

## <a name="why-cant-i-set-a-policy-directly-on-client-apps-like-word-or-outlook"></a>Почему не удается задать политику непосредственно в клиентских приложениях, например Word или Outlook?

Политика условного доступа устанавливает требования к доступу к службе. Она применяется, когда происходит проверка подлинности для этой службы. Политика не устанавливается напрямую в клиентском приложении. Вместо этого она применяется, когда клиент вызывает службу. Например, политика, заданная в SharePoint, применяется к клиентам, вызывающим SharePoint. Политика, заданная в Exchange, применяется к Outlook. Дополнительные сведения см. в статье [зависимости службы условного доступа](service-dependencies.md) и рассмотрите политики назначения для [приложения Office 365](concept-conditional-access-cloud-apps.md#office-365) .

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Применяется ли политика условного доступа к учетным записям служб?

Политики условного доступа применяются ко всем учетным записям пользователей. Это относится и к учетным записям пользователей, которые используются как учетные записи служб. Часто учетная запись службы, которая запускается автоматически, не может удовлетворять требованиям политики условного доступа. Например, может потребоваться многофакторная проверка подлинности. Учетные записи служб можно исключить из политики с помощью [исключения пользователя или группы](concept-conditional-access-users-groups.md#exclude-users). 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Какова политика исключений по умолчанию для не поддерживаемых платформ?

В настоящее время политики условного доступа выборочно применяются для пользователей устройств iOS и Android. По умолчанию приложения на других платформах устройств не затрагиваются политикой условного доступа для устройств iOS и Android. Администратор клиента может переопределить глобальную политику, чтобы запретить доступ пользователям на платформах, которые не поддерживаются.

## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Как работают политики условного доступа для Microsoft Teams?

Microsoft Teams в значительной степени зависит от Exchange Online и SharePoint Online в основных сценариях производительности, таких как собрания, календари и совместное использование файлов. Политики условного доступа, заданные для этих облачных приложений, применяются к Microsoft Teams, когда пользователь подписывается непосредственно в Microsoft Teams.

Microsoft Teams также поддерживается отдельно в качестве облачного приложения в политиках условного доступа. Политики условного доступа, заданные для облачного приложения, применяются к Microsoft Teams при входе пользователя в систему. Однако без правильных политик для других приложений, таких как Exchange Online и SharePoint Online, пользователям по-прежнему может предоставляться к ним прямой доступ.

Клиенты рабочего стола Microsoft Teams для Windows и Mac поддерживают современные методы проверки подлинности. Современные методы проверки подлинности используют вход на основе библиотеки проверки подлинности Active Directory (ADAL) для клиентских приложений Microsoft Office на всех платформах.

Дополнительные сведения см. в статье [зависимости службы условного доступа](service-dependencies.md) и рассмотрите политики назначения для [приложения Office 365](concept-conditional-access-cloud-apps.md#office-365) .

## <a name="why-are-some-tabs-not-working-in-microsoft-teams-after-enabling-conditional-access-policies"></a>Почему некоторые вкладки не работают в Microsoft Teams после включения политик условного доступа?

После включения некоторых политик условного доступа в клиенте Microsoft Teams некоторые вкладки могут перестать работать на настольном клиенте, как и ожидалось. Однако затронутая функция вкладок при использовании веб-клиента Microsoft Teams. На затронутые вкладки могут входить Power BI, формы, VSTS, PowerApps и список SharePoint.

Чтобы просмотреть затронутые вкладки, необходимо использовать веб-клиент Teams в Microsoft Windows, Internet Explorer или Chrome с установленным расширением Accounts. Некоторые вкладки зависят от веб-аутентификации, которая не работает в Microsoft Teams Desktop Client, если включен условный доступ. Корпорация Майкрософт работает с партнерами, чтобы включить эти сценарии. На сегодняшний день мы включили сценарии, включающие планировщик, OneNote и Stream.

## <a name="next-steps"></a>Дальнейшие действия

- Чтобы настроить политики условного доступа для своей среды, см. статью [Планирование развертывания условного доступа](plan-conditional-access.md). 
