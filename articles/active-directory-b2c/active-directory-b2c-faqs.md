---
title: Часто задаваемые вопросы по Azure Active Directory B2C | Документация Майкрософт
description: Часто задаваемые вопросы по Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 0c837ce66656c0e8a6a15d359c76459b03c2d0dd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64722770"
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C: Часто задаваемые вопросы 
На этой странице вы найдете ответы на часто задаваемые вопросы об Azure Active Directory (Azure AD) B2C. Следите за обновлениями.

### <a name="why-cant-i-access-the-azure-ad-b2c-extension-in-the-azure-portal"></a>Почему я не могу получить доступ к расширению Azure AD B2C на портале Azure?
Существуют две основные причины, из-за которых расширение Azure AD может быть недоступно.  Для Azure AD B2C необходимо, чтобы пользователю в каталоге была назначена роль глобального администратора.  Обратитесь к администратору, если вы считаете, что вам необходимо предоставить доступ.  Если у вас есть права глобального администратора, убедитесь, что вы вошли в каталог Azure AD B2C, а не в каталог Azure Active Directory.  См. инструкции по [созданию клиента Azure AD B2C](tutorial-create-tenant.md).

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Можно ли использовать возможности Azure AD B2C в существующем клиенте Azure AD для работы с сотрудниками?
Azure AD и Azure AD B2C являются отдельными предложениями продуктов и не могут совместно существовать в одном клиенте.  Клиент Azure AD представляет организацию.  Клиент Azure AD B2C представляет коллекцию удостоверений для использования с приложениями проверяющей стороны.  С помощью пользовательских политик (в общедоступной предварительной версии) Azure AD B2C можно включить в федерацию с Azure AD, что позволяет выполнять проверку подлинности сотрудников в организации.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Можно ли использовать Azure AD B2C для входа в Office 365 с помощью учетных данных социальных сетей (Facebook и Google+)?
Azure AD B2C нельзя использовать для проверки подлинности пользователей Microsoft Office 365.  Azure AD — решение Майкрософт для управления доступом сотрудников к приложениям SaaS, которое содержит соответствующие функции, например доступ к лицензированию и условный доступ.  Azure AD B2C предоставляет платформу управления удостоверениями и доступом для создания веб-и мобильных приложений.  После настройки Azure AD B2C для включения в федерацию с клиентом Azure AD этот клиент может управлять доступом сотрудников к приложениям, которые используют Azure AD B2C.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Что такое локальные учетные записи в Azure AD B2C? Чем они отличаются от рабочих или учебных учетных записей в Azure AD?
В клиенте Azure AD пользователи, принадлежащие этому клиенту, выполняют вход с помощью адреса электронной почты в формате `<xyz>@<tenant domain>`.  `<tenant domain>` — один из проверенных доменов в клиенте или начальный домен `<...>.onmicrosoft.com`. Тип этой учетной записи — рабочая или учебная учетная запись.

В клиенте Azure AD B2C большинство приложений поддерживают вход с использованием любого электронного адреса (например, joe@comcast.net, bob@gmail.com, sarah@contoso.com или jim@live.com). Учетная запись такого типа называется локальной учетной записью.  В качестве локальных учетных записей мы также поддерживаем произвольные имена пользователей (например, vitaly, victor, marya или dmitry). Вы можете выбрать один из этих двух типов локальных учетных записей при настройке поставщиков удостоверений для Azure AD B2C на портале Azure. В клиенте Azure AD B2C щелкните **Поставщики удостоверений** и выберите **Имя пользователя** в разделе локальных учетных записей. 

Учетные записи пользователей для приложений обязательно нужно создавать с помощью потока пользователя регистрации или входа в систему либо же API Graph Azure AD. Учетные записи пользователей, созданные на портале Azure, используются только для администрирования клиента.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Какие поставщики удостоверений социальных сетей поддерживаются в настоящее время? Каких из них вы планируете поддерживать в будущем?
Сейчас мы поддерживаем Facebook, Google+, LinkedIn, Amazon, Twitter (предварительную версию), WeChat (предварительную версию), Weibo (предварительную версию) и QQ (предварительную версию). Мы добавим поддержку других популярных поставщиков удостоверений социальных сетей с учетом запросов клиентов.

В Azure AD B2C также добавлена поддержка [пользовательских политик](active-directory-b2c-overview-custom.md).  Эти [пользовательские политики](active-directory-b2c-overview-custom.md) позволяют разработчикам создавать собственные политики, которые с использованием любого поставщика удостоверений поддерживают [OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) или SAML. 

Начните работу с пользовательскими политиками, ознакомившись с [начальным пакетом пользовательской политики](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack).

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Можно ли настроить в разделе "Области" сбор дополнительной информации о клиентах от различных поставщиков удостоверений социальных сетей?
№ Области по умолчанию, используемые для поддерживаемых поставщиков удостоверений социальных сетей:

* Facebook: адрес электронной почты;
* Google+: адрес электронной почты;
* Учетная запись Майкрософт: профиль электронной почты OpenID
* Amazon: профиль;
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Нужно ли запускать приложение в Azure, чтобы оно работало с Azure AD B2C?
Нет, приложение можно разместить где угодно (в облаке или локально). Для взаимодействия с Azure AD B2C необходима только возможность отправки и получения HTTP-запросов в общедоступных конечных точках.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>У меня есть несколько клиентов Azure AD B2C. Как управлять ими на портале Azure?
Прежде чем открыть Azure AD B2C в меню слева на портале Azure, перейдите в каталог, которым вы хотите управлять.  Чтобы переключаться между каталогами, щелкните идентификатор в правом верхнем углу портала Azure, а затем выберите каталог в отобразившемся раскрывающемся меню.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Как настроить проверочные сообщения электронной почты (содержание сообщения и поле "От:"), которые отправляет служба Azure AD B2C?
Вы можете использовать [функцию фирменной символики](../active-directory/fundamentals/customize-branding.md) , чтобы настроить содержимое проверочных электронных сообщений. В частности, можно настроить следующие два элемента электронного сообщения:

* **баннер с логотипом**: отображается в правом нижнем углу;
* **цвет фона**: отображается вверху.

    ![Снимок экрана с настроенным проверочным электронным сообщением](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Подпись электронного сообщения содержит имя клиента Azure AD B2C, которое было указано при создании клиента Azure AD B2C. Это имя можно изменить, выполнив приведенные ниже инструкции.

1. Войдите на [портал Azure](https://portal.azure.com/) как глобальный администратор.
1. Откройте колонку **Azure Active Directory**.
1. Щелкните вкладку **Свойства**.
1. Измените поле **Имя**.
1. Щелкните **Сохранить** в верхней части страницы.

В настоящее время нет возможности изменить значение поля "От:" электронного сообщения.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Как перенести в Azure AD B2C существующие имена пользователей, пароли и профили из своей базы данных?
Средство миграции можно создать с помощью API Graph Azure AD. Дополнительные сведения см. в [руководстве по переносу пользователей](active-directory-b2c-user-migration.md).

### <a name="what-password-user-flow-is-used-for-local-accounts-in-azure-ad-b2c"></a>Какой поток пользователя с использованием паролей применяется для локальных учетных записей в Azure AD B2C?
Поток пользователя с использованием паролей Azure AD B2C для локальных учетных записей основан на политике для Azure AD. В потоках пользователей для регистрации, входа и сброса пароля в Azure AD B2C предусмотрено использование надежного пароля, срок действия которого не истекает. Дополнительные сведения см. в статье [Политика паролей в Azure AD](/previous-versions/azure/jj943764(v=azure.100)). Сведения о блокировках учетных записей и паролей см. в статье [Предотвращение угроз в Azure Active Directory B2C](active-directory-b2c-reference-threat-management.md).

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Можно ли с помощью Azure AD Connect перенести в Azure AD B2C удостоверения клиентов, хранящиеся в локальном каталоге Active Directory?
Нет служба Azure AD Connect не предназначена для работы с Azure AD B2C. Вы можете использовать [API Graph Azure AD](active-directory-b2c-devquickstarts-graph-dotnet.md) для перемещения пользователя.  Дополнительные сведения см. в [руководстве по переносу пользователей](active-directory-b2c-user-migration.md).

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Может ли приложение открыть страницы Azure AD B2C в iFrame?
Нет, по соображениям безопасности страницы Azure AD B2C не открываются в iFrame.  Наша служба соединяется с браузером, чтобы запретить iFrame.  В целом сообщество специалистов по компьютерной безопасности и спецификация OAUTH2 не рекомендуют использовать элементы IFrame для идентификации из-за риска кликджекинга.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Работает ли служба Azure AD B2C с системами CRM, например Microsoft Dynamics?
Сейчас реализована интеграция с порталом Microsoft Dynamics 365.  Дополнительные сведения см. в статье [Параметры поставщика Azure AD B2C для порталов](https://docs.microsoft.com/dynamics365/customer-engagement/portals/azure-ad-b2c).

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Работает ли Azure AD B2C с программой SharePoint On-Premises 2016 или более ранних версий?
Azure AD B2C не используется для предоставления общего доступа внешнему партнеру SharePoint; для этого вам нужно решение [Azure AD B2B](https://cloudblogs.microsoft.com/enterprisemobility/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview/).

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Использовать службу Azure AD B2C или B2B для управления внешними удостоверениями?
Прочтите статью о том, как применять соответствующие функции в сценариях с использованием [внешних удостоверений](../active-directory/active-directory-b2b-compare-external-identities.md) .

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Какие функции отчетности и аудита есть в Azure AD B2C? Соответствуют ли они аналогичным функциям в Azure AD Premium?
Нет, Azure AD B2C не поддерживает набор отчетов, используемый в Azure AD Premium. Однако между ними есть много общего:

* **В отчетах о входе в систему** представлены записи и краткие сведения о каждом входе.
* **Отчеты аудита** содержат сведения о действиях администратора и приложения. 
* **Отчеты об использовании** содержат сведения о количестве пользователей, числе входов в систему и количестве событий MFA. 

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Можно ли локализовать пользовательский интерфейс страниц Azure AD B2C? Какие языки поддерживаются?
 Да!  Узнайте о функции [настройки языка](active-directory-b2c-reference-language-customization.md), доступной в общедоступной предварительной версии.  Мы предоставляем переводы для 36 языков, и вы можете переопределить любую строку в соответствии со своими потребностями.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Могу ли я использовать свои URL-адреса на страницах регистрации и входа в Azure AD B2C? Например, можно ли изменить URL-адрес с login.microsoftonline.com на login.contoso.com?
В настоящее время нет. Мы планируем добавить эту функцию в будущем. Проверка домена на вкладке **Домены** на портале Azure не подходит для достижения этой цели.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Как удалить клиент Azure AD B2C?
Для удаления клиента Azure AD B2C выполните следующие действия.

1. Удалите все маршруты пользователей (политики) в клиенте Azure AD B2C.
1. Удалите все приложения, которые были зарегистрированы в клиенте Azure AD B2C.
1. Теперь войдите на [портал Azure](https://portal.azure.com/) под именем администратора подписки. (Используйте ту же рабочую или учебную учетную запись либо учетную запись Майкрософт, которая использовалась для регистрации в Azure.)
1. Переключитесь на клиент Azure AD B2C, который нужно удалить.
2. Перейдите к меню Active Directory слева.
3. Выберите **Пользователи и группы**.
4. Затем выберите всех пользователей (кроме пользователя, от имени которого вы вошли, — администратора подписки). В нижней части страницы щелкните **Удалить**, а затем при появлении запроса нажмите кнопку **Да**.
5. Щелкните **Регистрация приложений**.
6. Выберите приложение с именем **b2c-extensions-app**. Щелкните **Удалить** и нажмите **Да** при появлении запроса.
7. Щелкните **Обзор**.
8. Щелкните **Удалить каталог**. Для завершения процесса следуйте инструкциям на экране.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Можно ли получить Azure AD B2C в составе Enterprise Mobility Suite?
Нет, Azure AD B2C — это служба Azure с оплатой по мере использования, не входящая в предложение Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Как сообщать о проблемах с Azure AD B2C?
Ознакомьтесь со статьей [Azure Active Directory B2C: регистрация запросов в службу поддержки](active-directory-b2c-support.md).
