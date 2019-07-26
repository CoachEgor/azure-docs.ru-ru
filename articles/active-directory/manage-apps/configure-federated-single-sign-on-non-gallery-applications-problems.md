---
title: Проблема при настройке федеративного единого входа для приложения не из коллекции | Документы Майкрософт
description: В этом разделе приводятся решения распространенных проблем, которые могут возникнуть при настройке федеративного единого входа для пользовательского приложения SAML, которое отсутствует в коллекции приложений Azure AD
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: 99c5e4d99f45e2a642a46f7dc070fb7512ff4d73
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422559"
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Проблема при настройке федеративного единого входа для приложения не из коллекции

Если при настройке приложения возникла проблема, выполните следующие действия. Выполните все действия, описанные в статье [Настройка единого входа для приложений, которых нет в коллекции приложений Azure Active Directory](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery).

## <a name="cant-add-another-instance-of-the-application"></a>Невозможно добавить другой экземпляр приложения

Чтобы добавить второй экземпляр приложения, нужно выполнить следующие действия:

-   Настроить уникальный идентификатор для второго экземпляра. Вы не можете использовать тот же идентификатор, что и у первого экземпляра.

-   Настроить другой сертификат для второго экземпляра.

Если в приложении не поддерживается ни один из указанных вариантов, то второй экземпляр невозможно настроить.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Можно ли изменить формат идентификатора EntityID (идентификатора пользователя)?

Вы не можете изменить формат EntityID (идентификатора пользователя), который Azure AD отправляет приложению после проверки подлинности пользователя.

Azure AD выбирает формат атрибута NameID (идентификатор пользователя) на основе выбранного значения или формата, который был запрошен приложением в запросе проверки подлинности SAML. Дополнительные сведения см. в разделе "NameIDPolicy" статьи [Протокол единого входа SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest).

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Как получить метаданные приложения или сертификат в Azure AD?

Чтобы скачать метаданные приложения или сертификат из Azure AD, сделайте следующее:

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор** или **соадминистратор**.

2. Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3. В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4. В меню навигации Azure Active Directory слева выберите **Корпоративные приложения**.

5. Щелкните **Все приложения**, чтобы открыть полный список приложений.

   * Если нужное приложение отсутствует в списке, воспользуйтесь элементом управления **Фильтр** в верхней части списка **Все приложения**, указав в нем для параметра **Показать** значение **Все приложения**.

6. Выберите приложение, для которого был настроен единый вход.

7. После загрузки приложения выберите пункт **Единый вход** в меню навигации этого приложения слева.

8. Перейдите в раздел **Сертификат подписи SAML** и выберите значение в столбце **Скачать**. В зависимости от того, что именно требуется для настройки единого входа, вы сможете скачать XML-файл метаданных или сертификат.

Azure AD не предоставляет URL-адреса для получения метаданных. Метаданные можно получить только в виде XML-файла.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Не знаете, как настроить утверждения SAML, отправляемые в приложение

Чтобы узнать, как настроить атрибут утверждений SAML, отправляемых в приложение, ознакомьтесь с разделом [Сопоставление утверждений в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping).

## <a name="next-steps"></a>Следующие шаги
[Управление приложениями с помощью Azure Active Directory](what-is-application-management.md)
