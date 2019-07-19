---
title: Приступая к работе с Azure AD в проектах WebApi в Visual Studio
description: Как начать использовать Azure Active Directory в проектах WebApi после подключения или создания Azure AD с помощью подключенных служб Visual Studio
services: active-directory
ms.service: active-directory
ms.subservice: develop
author: ghogen
manager: douge
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 662e0a3e85684287c636b8d0b512b072c978dd3f
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320735"
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Начало работы с Azure Active Directory (проекты WebApi)

> [!div class="op_single_selector"]
> - [Начало работы](vs-active-directory-webapi-getting-started.md)
> - [Что произошло?](vs-active-directory-webapi-what-happened.md)

В этой статье приведены дополнительные рекомендации, которые потребуются вам, когда вы добавите Active Directory в проект ASP.NET WebAPI, последовательно выбрав **Проект > Подключенные службы** в Visual Studio. Если вы еще не добавили службу в проект, это можно сделать в любое время.

Изменения, которые вносятся в проект при добавлении подключенной службы, описаны в статье [Что произошло с моим проектом WebAPI в подключенной службе Visual Studio Azure Active Directory?](vs-active-directory-webapi-what-happened.md)

## <a name="requiring-authentication-to-access-controllers"></a>Требование проверки подлинности для доступа к контроллерам

Ко всем контроллерам в проекте добавлен атрибут `[Authorize]`. Этот атрибут обеспечивает аутентификацию пользователей перед доступом к интерфейсам API, определяемым этими контроллерами. Для анонимного доступа к контроллеру удалить с него этот атрибут. Если необходимо задать разрешения на более детальном уровне, примените атрибут к каждому методу, требующему проверки подлинности, а не к классу контроллера.

## <a name="next-steps"></a>Следующие шаги

- [Сценарии аутентификации в Azure Active Directory](authentication-scenarios.md)
- [Добавление возможности входа в веб-приложение ASP.NET с помощью учетной записи Майкрософт](quickstart-v1-aspnet-webapp.md)
