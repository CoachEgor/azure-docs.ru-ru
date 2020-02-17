---
title: Azure Active Directory часто задаваемые вопросы о условном доступе | Документация Майкрософт
description: Получите ответы на часто задаваемые вопросы об условном доступе в Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6842338bd27e4bea3436f0b249380ab773d60de6
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2020
ms.locfileid: "77368092"
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Azure Active Directory часто задаваемые вопросы о условном доступе

## <a name="which-applications-work-with-conditional-access-policies"></a>Какие приложения работают с политиками условного доступа?

Сведения о приложениях, которые работают с политиками условного доступа, см. [в разделе приложения и браузеры, использующие правила условного доступа в Azure Active Directory](concept-conditional-access-cloud-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Применяются ли политики условного доступа к службе совместной работы B2B и гостевым пользователям?

Политики применяются для пользователей службы совместной работы B2B. Однако в некоторых случаях пользователь может не иметь возможности соответствовать требованиям политики. Например, организация гостевого пользователя может не поддерживать многофакторную проверку подлинности. 

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Распространяется ли политика SharePoint Online на OneDrive для бизнеса?

Да. Политика SharePoint Online также распространяется на OneDrive для бизнеса.

## <a name="why-cant-i-set-a-policy-directly-on-client-apps-like-word-or-outlook"></a>Почему не удается задать политику непосредственно в клиентских приложениях, например Word или Outlook?

Политика условного доступа устанавливает требования к доступу к службе. Она применяется, когда происходит проверка подлинности для этой службы. Политика не устанавливается напрямую в клиентском приложении. Вместо этого она применяется, когда клиент вызывает службу. Например, политика, заданная в SharePoint, применяется к клиентам, вызывающим SharePoint. Политика, заданная в Exchange, применяется к Outlook.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Применяется ли политика условного доступа к учетным записям служб?

Политики условного доступа применяются ко всем учетным записям пользователей. Это относится и к учетным записям пользователей, которые используются как учетные записи служб. Часто учетная запись службы, которая запускается автоматически, не может удовлетворять требованиям политики условного доступа. Например, может потребоваться многофакторная проверка подлинности. Учетные записи служб можно исключить из политики с помощью параметров управления политиками условного доступа. 

## <a name="are-microsoft-graph-apis-available-for-configuring-conditional-access-policies"></a>Доступны Microsoft Graph API для настройки политик условного доступа?

В настоящее время нет. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Какова политика исключений по умолчанию для не поддерживаемых платформ?

В настоящее время политики условного доступа выборочно применяются для пользователей устройств iOS и Android. По умолчанию приложения на других платформах устройств не затрагиваются политикой условного доступа для устройств iOS и Android. Администратор клиента может переопределить глобальную политику, чтобы запретить доступ пользователям на платформах, которые не поддерживаются.

## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Как работают политики условного доступа для Microsoft Teams?

Microsoft Teams в значительной степени зависит от Exchange Online и SharePoint Online в основных сценариях производительности, таких как собрания, календари и совместное использование файлов. Политики условного доступа, заданные для этих облачных приложений, применяются к Microsoft Teams, когда пользователь подписывается непосредственно в Microsoft Teams.

Microsoft Teams также поддерживается отдельно в качестве облачного приложения в Azure Active Directory политик условного доступа. Политики условного доступа, заданные для облачного приложения, применяются к Microsoft Teams при входе пользователя в систему. Однако без правильных политик для других приложений, таких как Exchange Online и SharePoint Online, пользователям по-прежнему может предоставляться к ним прямой доступ.

Клиенты рабочего стола Microsoft Teams для Windows и Mac поддерживают современные методы проверки подлинности. Современные методы проверки подлинности используют вход на основе библиотеки проверки подлинности Active Directory (ADAL) для клиентских приложений Microsoft Office на всех платформах.

## <a name="next-steps"></a>Следующие шаги

- Чтобы настроить политики условного доступа для своей среды, ознакомьтесь с [рекомендациями по условному доступу в Azure Active Directory](best-practices.md). 
