---
title: Список совместимости с федерацией Azure AD
description: На этой странице указаны сторонние поставщики, которые можно использовать для реализации единого входа.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54f5090101c486562e33de56402db348c6038c8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "60244761"
---
# <a name="azure-ad-federation-compatibility-list"></a>Список совместимости с федерацией Azure AD
Azure Active Directory предоставляет возможность единого входа и обеспечивает повышенную безопасность доступа к приложениям для Office 365 и других служб Microsoft Online Services для гибридных и исключительно облачных реализаций без использования стороннего решения. Office 365, как и большинство служб Microsoft Online Services, интегрируется с Azure Active Directory для служб каталогов, аутентификации и авторизации. Azure Active Directory также предоставляет возможность единого входа в тысячи приложений SaaS и локальные веб-приложения. Список поддерживаемых приложений SaaS см. в [коллекции приложений](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) Azure Active Directory. 

## <a name="idp-validation"></a>Проверка IdP
Если ваша организация использует стороннее решение для федерации, то можно настроить единый вход для пользователей в локальной среде Active Directory с помощью служб Microsoft Online, например Office 365, при условии, что это стороннее решение для федерации совместимо с Azure Active Directory.  По вопросам совместимости обратитесь к поставщику удостоверений.  Если вы хотите просмотреть список поставщиков удостоверений, которые ранее были проверены корпорацией Майкрософт на совместимость с Azure AD, щелкните [здесь](https://www.microsoft.com/download/details.aspx?id=56843). 

>[!NOTE]
>Корпорация Майкрософт больше не предоставляет тестирование совместимости с Azure Active Directory для независимых поставщиков удостоверений. Если вы хотите проверить взаимодействие своего продукта, ознакомьтесь с этими [рекомендациями](https://www.microsoft.com/download/details.aspx?id=56843). 

## <a name="next-steps"></a>Next Steps

- [Интеграция локальных каталогов с Azure Active Directory](whatis-hybrid-identity.md)
- [Azure AD Connect и федерация](how-to-connect-fed-whatis.md)
