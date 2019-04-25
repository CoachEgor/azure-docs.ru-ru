---
title: Настройка SharePoint Online и Exchange Online для условного доступа Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить SharePoint Online и Exchange Online для условного доступа Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: a90cd381dbe3feaad110c7f10ae328915c051d0a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60411949"
---
# <a name="how-to-set-up-sharepoint-online-and-exchange-online-for-azure-active-directory-conditional-access"></a>Практическое руководство. Настройка SharePoint Online и Exchange Online для условного доступа Azure Active Directory 

С помощью [условного доступа Azure Active Directory (Azure AD)](overview.md) можно контролировать доступ пользователей к облачным приложениям. Если вы хотите использовать условный доступ для управления доступом к SharePoint и Exchange Online, необходимо:

- проверить, поддерживается ли ваш сценарий условного доступа;
- предотвратить обход клиентскими приложениями принудительного применения политик условного доступа.   

В этой статье объясняется, как можно выполнить обе эти задачи.


## <a name="what-you-need-to-know"></a>Это важно знать

Условный доступ Azure AD можно использовать для защиты облачных приложений при поступлении запроса на аутентификацию из:

- веб-браузера;

- клиентского приложения, использующего [современную аутентификацию](https://support.office.com/article/Using-Office-365-modern-authentication-with-Office-clients-776c0036-66fd-41cb-8928-5495c0f9168a);

- Exchange ActiveSync 

Некоторые облачные приложения также поддерживают устаревшие протоколы аутентификации. Это относится, например, к SharePoint Online и Exchange Online. Когда клиентское приложение может использовать устаревший протокол аутентификации для доступа к облачному приложению, Azure AD не может принудительно применить политику условного доступа к этой попытке получения доступа. Чтобы предотвратить обход применяемых политик клиентским приложением, следует проверить, можно ли включить для затронутых облачных приложений только современную аутентификацию. 

Ниже приведены примеры клиентских приложений, не подходящих для условного доступа:

- Office 2010 и более ранних версий;

- Office 2013, если не включена современная аутентификация.



 
## <a name="control-access-to-sharepoint-online"></a>Управление доступом к SharePoint Online

Помимо современной аутентификации SharePoint Online также поддерживает устаревшие протоколы аутентификации. Если используются устаревшие протоколы аутентификации, то политики условного доступа для SharePoint не применяются к клиентам, которые не используют современную аутентификацию.

Устаревшие протоколы аутентификации, используемые для доступа к SharePoint, можно отключить с помощью командлета **[Set-SPOTenant](https://technet.microsoft.com/library/fp161390.aspx)**. 

    Set-SPOTenant -LegacyAuthProtocolsEnabled $false

## <a name="control-access-to-exchange-online"></a>Управление доступом к Exchange Online

При настройке политики условного доступа для Exchange Online необходимо проверить следующее:

- Exchange ActiveSync

- Устаревшие протоколы аутентификации



### <a name="exchange-activesync"></a>Exchange ActiveSync

Хотя Exchange Active Sync поддерживает современную аутентификацию, имеются некоторые ограничения поддержки сценариев условного доступа.

- При выборе **клиенты Exchange Active Sync** в политике нельзя настроить другие условия.  

    ![Платформы устройств](./media/conditional-access-for-exo-and-spo/05.png)

- Настройка обязательной Многофакторной идентификации не поддерживается.  

    ![Условный доступ](./media/conditional-access-for-exo-and-spo/01.png)

Для эффективной защиты доступа к Exchange Online из Exchange ActiveSync можно сделать следующее.

- Настройте поддерживаемую политику условного доступа, выполнив следующие действия.

    a. В качестве облачного приложения выберите только **Office 365 Exchange Online**.  

    ![Условный доступ](./media/conditional-access-for-exo-and-spo/04.png)

    2. Выберите **Exchange Active Sync** как **клиентское приложение**.  

    ![Платформы устройств](./media/conditional-access-for-exo-and-spo/03.png)

- Заблокируйте Exchange ActiveSync с помощью служб федерации Active Directory (AD FS).

        @RuleName = "Block Exchange ActiveSync"
        c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
        => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "false");




### <a name="legacy-authentication-protocols"></a>Устаревшие протоколы аутентификации

Помимо современной аутентификации Exchange Online также поддерживает устаревшие протоколы аутентификации. Если используются устаревшие протоколы аутентификации, то политики условного доступа для Exchange Online не применяются к клиентам, которые не используют современную аутентификацию.

Можно отключить устаревшие протоколы аутентификации для Exchange Online, установив соответствующие правила AD FS. Это позволит заблокировать доступ из:

- клиентов Office прежних версий, например Office 2013, не поддерживающих современную аутентификацию; 

- более ранних версий Office.


## <a name="set-up-ad-fs-rules"></a>Настройка правил AD FS

Вы можете использовать следующие правила авторизации выдачи, чтобы разрешить или заблокировать трафик на уровне AD FS. 

### <a name="block-legacy-traffic-from-the-extranet"></a>Блокирование трафика устаревших клиентов из экстрасети

Применяя следующие три правила: 

- можно включить доступ для:
    - трафика Exchange ActiveSync;
    - трафика браузера;
    - трафика современной аутентификации;
- можно заблокировать доступ для: 
    - устаревших клиентских приложений из экстрасети.

**Правило 1**

    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**Правило 2**

    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**Правило 3**

    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

### <a name="block-legacy-traffic-from-anywhere"></a>Блокирование трафика устаревших клиентов из любого расположения

Применяя следующие три правила:

- можно включить доступ для: 
    - трафика Exchange ActiveSync;
    - трафика браузера;
    - трафика современной аутентификации;
- можно заблокировать доступ для: 
    - устаревших приложений из любого расположения.

##### <a name="rule-1"></a>Правило 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Правило 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Правило 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в статье [Условный доступ в Azure Active Directory](overview.md).

См. инструкции по [настройке правил утверждений](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-claim-rules). 






