---
title: Доступ к прокси приложения Azure AD App в Microsoft Teams | Документы Майкрософт
description: Используйте прокси приложения Azure AD для доступа к локальным приложениям через Microsoft Teams.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/05/2017
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e9b043b27f624a3d9f38fb2299f9e49f0ffedda
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65783420"
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Доступ к приложениям в локальной среде через Microsoft Teams

Прокси приложения Azure Active Directory обеспечивает единый вход для локальных приложений, где бы вы ни находились, а Microsoft Teams упрощает процесс совместной работы. Интеграция этих двух решений означает, что пользователи могут эффективно работать с коллегами в любой ситуации. 

Пользователи могут добавлять облачные приложения в свои каналы Teams [с помощью вкладок](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), но что делать, если сайты SharePoint или средства планирования размещены в локальной среде? Решением является прокси приложения. Они могут добавлять приложения, публикуемые через прокси приложения в своих каналах с помощью тех же внешних URL-адресов, которые они всегда используют для удаленного доступа к своим приложениям. Так как для проверки подлинности прокси приложения использует Azure Active Directory, пользователям предоставляется единый вход.


## <a name="install-the-application-proxy-connector-and-publish-your-app"></a>Установка соединителя прокси приложения и публикация приложения

Если вы еще не сделали это, [настройте прокси приложения для своего клиента и установите соединитель](application-proxy-add-on-premises-application.md). Затем [опубликуйте свое локальное приложение](application-proxy-add-on-premises-application.md) для удаленного доступа. Во время публикации приложения запишите внешний URL-адрес — он потребуется при добавлении приложения в Teams.

Если у вас уже имеются опубликованные приложения, однако вы не помните их внешние URL-адреса, обратитесь к [порталу Azure](https://portal.azure.com). Войдите на портал и выберите **Azure Active Directory** > **Корпоративные приложения** > **Все приложения** > выберите свое приложение > **Прокси приложения**.

## <a name="add-your-app-to-teams"></a>Добавление приложения в Teams

После публикации приложения через прокси приложения сообщите пользователям о том, что они могут добавить его в виде вкладки прямо в свои каналы Teams. После этого приложение станет доступно для всех пользователей в команде. Для этого им потребуется выполнить следующие три шага:

1. Перейдите к каналу Teams, в который необходимо добавить это приложение, и нажмите **+** , чтобы добавить вкладку.

   ![Выберите "Добавить вкладку"](./media/application-proxy-integrate-with-teams/add-tab.png)

2. Выберите **Веб-сайт** на вкладке параметров.

   ![Добавление веб-сайта](./media/application-proxy-integrate-with-teams/website.png)

3. Присвойте вкладке имя и задайте внешний URL-адрес прокси приложения в качестве URL-адреса. 

   ![Настройка имени вкладки и URL-адреса](./media/application-proxy-integrate-with-teams/tab-name-url.png)

После того как один из участников команды добавит вкладку, она будет отображаться для всех пользователей в канале. Все пользователи, имеющие доступ к приложению, получают возможность единого входа с помощью учетных данных, которые они используют для доступа к Microsoft Teams. Для пользователей, у которых нет доступа к приложению, будет отображаться соответствующая вкладка в Teams, однако они будут заблокированы, пока вы не предоставите им разрешения на доступ к локальному приложению и его версии, опубликованной на портале Azure. 

## <a name="next-steps"></a>Дальнейшие действия

- Подробнее о [публикации локальных сайтов SharePoint](application-proxy-integrate-with-sharepoint-server.md) с помощью прокси приложения.
- Настройка приложений для использования [личных доменов](application-proxy-configure-custom-domain.md) для их внешних URL-адресов. 
