---
title: Архив сведений о новых возможностях Azure Active Directory | Документы Майкрософт
description: Заметки о выпуске "Новые возможности" в разделе "Обзор" для конкретного набора содержимого отображаются на протяжении шести месяцев. Через шесть месяцев элементы удаляются из основной статьи и помещаются в статью архива.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6fe7b2cefdd29f0497bbc18e969a77ac295ba42
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60246532"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Архив сведений о новых возможностях в Azure Active Directory

Первоначальная статья с [заметками о выпуске "Новые возможности"](whats-new.md) содержит информацию за последние шесть месяцев. Архивная статья содержит более старые данные.

В заметках о выпуске "Новые возможности" предоставляются следующие сведения:

- Последние выпуски.
- Известные проблемы
- Исправления ошибок
- Нерекомендуемые функции.
- Планы по изменениям.

---

## <a name="august-2018"></a>Август 2018 г.

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Изменения в диапазонах IP-адресов Azure Active Directory

**Тип:** план изменений  
**Категория службы:** Другие  
**Возможности продукта:** платформа

Мы вводим большие диапазоны IP-адресов в Azure Active Directory, то есть, если вы настроили диапазоны IP-адресов Azure AD для брандмауэров, маршрутизаторов или групп безопасности сети, вам потребуется их обновить. Мы выполняем это обновление, поэтому вам не придется менять конфигурацию диапазона IP-адресов брандмауэра, маршрутизатора или сети безопасности, когда Azure Active Directory добавляет новые конечные точки. 

Сетевой трафик перейдет в эти новые диапазоны в течение следующих двух месяцев. Чтобы продолжить непрерывное обслуживание, вам необходимо добавить эти обновленные значения в свои IP-адреса до 10 сентября 2018 года:

- 20.190.128.0/18 

- 40.126.0.0/18 

Мы настоятельно рекомендуем не удалять старые диапазоны IP-адресов, пока весь сетевой трафик не переместится на новые диапазоны. Сведения о переносе и о том, когда вы сможете удалить старые диапазоны, см. в разделе [URL-адреса и диапазоны IP-адресов Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Уведомление об изменении: коды авторизации больше не будут доступны для повторного использования 

**Тип:** план изменений  
**Категория службы:** аутентификация (вход)  
**Возможности продукта:** Аутентификация пользователей

С 15 ноября 2018 г. в Azure AD прекращается поддержка использованных кодов аутентификации для приложений. Это изменение системы безопасности помогает Azure AD соответствовать спецификации OAuth и будет применяться для конечных точек версии 1 и 2.

Если ваше приложение повторно использует коды проверки подлинности для получения маркеров для нескольких ресурсов, мы рекомендуем использовать код для получения маркера обновления, а затем использовать этот маркер для получения дополнительных маркеров для других ресурсов. Коды проверки подлинности можно использовать только один раз, однако маркеры обновления можно использовать несколько раз для нескольких ресурсов. У приложения, которое пытается повторно использовать код аутентификации в потоке кода OAuth, возникнет ошибка invalid_grant.

Это и другие изменения в работе с протоколами описаны на странице [с полным списком новых возможностей для аутентификации](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Конвергентное управление сведениями для защиты для самостоятельного сброса пароля (SSPR) и Многофакторной идентификации (MFA)

**Тип:** Новая функция  
**Категория службы:** SSPR  
**Возможности продукта:** Аутентификация пользователей
 
Эта новая функция позволяет пользователям управлять своими сведениями для защиты (например, номером телефона, мобильным приложением и т. д.) для SSPR и MFA в одном расположении и интерфейсе (ранее все выполнялось в двух разных расположениях).

Этот конвергентный интерфейс доступен для пользователей, использующих либо SSPR, либо MFA. Кроме того, если организация не требует регистрацию MFA или SSPR в принудительном порядке, пользователи все равно могут зарегистрировать в сведениях для защиты разрешенные организацией методы MFA или SSPR через портал My Apps.

Это общедоступная предварительная версия. Администраторы могут включить новую функцию (при необходимости) для выбранной группы или всех пользователей в клиенте. Дополнительные сведения о конвергентном интерфейсе см. в [этой записи блога](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/).

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Новый параметр "HTTP-Only cookies" в приложениях Azure AD Application Proxy

**Тип:** Новая функция  
**Категория службы:** Прокси приложения  
**Возможности продукта:** Контроль доступа

В приложениях Application Proxy доступен новый параметр под названием **HTTP-Only Cookies** (Только файлы cookie HTTP). Он позволяет обеспечить дополнительную безопасность, включив флаг HTTPOnly в заголовок ответа HTTP для файлов cookie сеанса и доступа к Application Proxy, запрещая доступ к файлу cookie с клиентского скрипта и предотвращая такие действия, как копирование или изменение файла cookie. Хотя этот флаг ранее не использовался, ваши файлы cookie всегда шифровались и передавались с использованием SSL-соединения, что обеспечивало защиту от неправильных изменений.

Этот параметр несовместим с приложениями, использующими элементы ActiveX, например удаленный рабочий стол. Если вы находитесь в подобной ситуации, рекомендуем отключить этот параметр.

Дополнительные сведения о параметре HTTP-Only Cookies см. в статье [Публикация приложений с помощью Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Управление привилегированными пользователями (PIM) для ресурсов Azure поддерживает типы ресурсов группы управления

**Тип:** Новая функция  
**Категория службы:** Управление привилегированными пользователями (PIM)  
**Возможности продукта:** Управление привилегированными пользователями (PIM)
 
Теперь параметры активации и назначения JIT могут применяться к типам ресурсов группы управления так же, как для подписки, групп ресурсов и ресурсов (таких как виртуальные машины, службы приложений и т. д.). Кроме того, любой пользователь с ролью, которая предоставляет доступ с правами администратора к группе управления, может обнаружить этот ресурс и управлять им в PIM.

Дополнительные сведения о PIM и ресурсах Azure см. в статье [Обнаружение ресурсов Azure и управление ими с помощью управления привилегированными пользователями](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources).
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Функция доступа к приложениям (предварительная версия) обеспечивает более быстрый доступ к порталу Azure AD

**Тип:** Новая функция  
**Категория службы:** Управление привилегированными пользователями (PIM)  
**Возможности продукта:** Управление привилегированными пользователями (PIM)
 
Сегодня при активации роли с помощью PIM может потребоваться более 10 минут, чтобы разрешения вступили в силу. Если вы решите использовать функцию доступа к приложениям, которая в настоящее время находится в режиме общедоступной предварительной версии, администраторы смогут получить доступ к порталу Azure AD, как только будет выполнен запрос активации.

В настоящее время функция доступа к приложениям поддерживает только интерфейс портала Azure AD и ресурсы Azure. Дополнительные сведения о PIM и доступе к приложениям см. в статье [Что такое Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>В коллекции приложений Azure AD доступны новые федеративные приложения (август 2018 г.)

**Тип:** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** интеграция сторонних продуктов
 
В августе 2018 года мы добавили эти 16 новых приложений с поддержкой федерации в коллекцию приложений:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [Bridgeline Unbound](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Sauce Labs - Mobile and Web Testing](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [Meta Networks Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [Way We Do](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [ProMaster (by Inlogik)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - Expense reports](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 Live Chat](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Дополнительные сведения об этих приложениях см. в статье [Интеграция приложений SaaS с Azure Active Directory](https://aka.ms/appstutorial). Дополнительные сведения о том, как добавить приложение в коллекцию приложений Azure AD см. в [этой статье](https://aka.ms/azureadapprequest).

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Теперь в Azure AD Application Proxy доступна собственная поддержка Tableau

**Тип:** Измененная функция  
**Категория службы:** Прокси приложения  
**Возможности продукта:** Контроль доступа

Благодаря переходу с использования OpenID Connect на протокол предоставления кода OAuth 2.0 для протокола предварительной проверки подлинности вам больше не нужно выполнять дополнительную настройку для использования Tableau с Application Proxy. Кроме того, это изменение протокола позволяет Application Proxy поддерживать более современные приложения, используя только перенаправления HTTP, которые обычно поддерживаются в тегах JavaScript и HTML.

Дополнительные сведения о собственной поддержке Tableau в Azure AD Application Proxy см. в [этой записи блога](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Поддержка новой возможности: добавление Google в качестве поставщика удостоверений для гостевых пользователей B2B в Azure Active Directory (предварительная версия)

**Тип:** Новая функция  
**Категория службы:** B2B  
**Возможности продукта:** B2B или B2C

Настроив федерацию с Google в своей организации, вы можете разрешить приглашенным пользователям Gmail использовать ваши общедоступные приложения и ресурсы со своей учетной записью Google, то есть им не придется создавать личную учетную запись Майкрософт (MSA) или учетную запись AAD.

Это общедоступная предварительная версия. Дополнительные сведения о федерации Google см. статье [Добавление Google в качестве поставщика удостоверений для гостевых пользователей B2B](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Июль 2018 г.

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Усовершенствования уведомлений по электронной почте Azure Active Directory

**Тип:** Измененная функция  
**Категория службы:** Другие  
**Возможности продукта:** управление жизненным циклом удостоверений
 
В сообщениях электронной почты Azure Active Directory (Azure AD) теперь отображается обновленный дизайн, изменения адреса электронной почты отправителя и отображаемого имени отправителя при их отправке из следующих служб.
 
- Проверки доступа Azure AD
- Azure AD Connect Health, 
- защиту идентификации Azure AD 
- Azure AD Privileged Identity Management
- Уведомления об истечении срока действия корпоративного приложения
- Уведомления об подготовке к работе корпоративного приложения
 
Уведомления будут отправляться по электронной почте со следующего адреса и отображаемого имени.

- Адрес электронной почты: azure-noreply@microsoft.com
- Отображаемое имя: Microsoft Azure.
 
Примеры некоторых новых дизайнов писем и дополнительную информацию см. в разделе [Email notifications in Azure AD PIM](https://go.microsoft.com/fwlink/?linkid=2005832) (Уведомления по электронной почте в Azure AD PIM).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Журналы действий Azure AD теперь доступны через Azure Monitor

**Тип:** Новая функция  
**Категория службы:** Отчеты  
**Возможности продукта:** мониторинг и создание отчетов

Журналы действий Azure AD теперь доступны в общедоступной предварительной версии для Azure Monitor (служба мониторинга всей платформы Azure). Azure Monitor предлагает долгосрочное хранение и простую интеграцию, а также следующие улучшения:

- долгосрочное хранение за счет перенаправления файлов журналов в собственную учетную запись хранения Azure;

- простая интеграция SIEM без необходимости написания или сохранения настраиваемых сценариев;

- простая интеграция с собственными настраиваемыми решениями, инструментами аналитики или решениями по управлению инцидентами.

Дополнительные сведения об этих новых возможностях см. в записи блога, посвященной [общедоступной предварительной версии журналов действий Azure AD в системе диагностики Azure Monitor](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/), а также статье [Журналы действий Azure AD в Azure Monitor (предварительная версия)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Сведения об условном доступе, добавленные в отчет о входе в Azure AD

**Тип:** Новая функция  
**Категория службы:** Отчеты  
**Возможности продукта:** безопасность и защита идентификации
 
Это обновление позволяет увидеть, оценка каких политик выполняется, когда пользователь входит в систему, а также результат политики. Кроме того, в отчет теперь входит тип клиентского приложения, используемого пользователем, поэтому вы можете идентифицировать трафик устаревшего протокола. В записях отчетов теперь также можно выполнить поиск по идентификатору корреляции, который можно найти в сообщении об ошибке пользователя и использовать для идентификации и устранения неполадок соответствующего запроса на вход.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Просмотр устаревших способов аутентификации через журналы действий входа

**Тип:** Новая функция  
**Категория службы:** Отчеты  
**Возможности продукта:** мониторинг и создание отчетов
 
С появлением поля **Клиентское приложение** в журналах действий входа клиенты теперь могут видеть, какие пользователи применяют устаревшие способы аутентификации. Клиенты смогут получить доступ к этим данным с помощью API MS Graph для входов или журналов действий входа на портале Azure AD. Там же можно использовать элемент управления **Клиентское приложение** для фильтрации устаревших способов аутентификации. Дополнительные сведения см. в документации.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Новые федеративные приложения, доступные в коллекции приложений Azure AD (июль 2018 г.)

**Тип:** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** интеграция сторонних продуктов
 
В июле 2018 года мы добавили 16 новых приложений с поддержкой федерации в коллекцию приложений:

[Innovation Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [Certain Admin SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), PSUC Staging, [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [Screencast-O-Matic](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial), PowerSchool Unified Classroom, [Eli Onboarding](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [Bomgar Remote Support](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial), [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [SmartLPA](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial), [Skills Base](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial).

Дополнительные сведения об этих приложениях см. в статье [Интеграция приложений SaaS с Azure Active Directory](https://aka.ms/appstutorial). Дополнительные сведения о том, как добавить приложение в коллекцию приложений Azure AD см. в [этой статье](https://aka.ms/azureadapprequest).

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Новые возможности интеграции для подготовки пользователей с приложениями SaaS — июль 2018 г.

**Тип:** Новая функция  
**Категория службы:** подготовка приложений  
**Возможности продукта:** интеграция сторонних продуктов
 
Служба Azure AD предоставляет возможность автоматизировать создание, обслуживание и удаление удостоверений пользователей в приложениях SaaS, таких как Dropbox, Salesforce, ServiceNow и т. д. В июле 2018 года мы добавили поддержку подготовки пользователей в следующие приложения в коллекции приложений Azure AD:

- [Cisco Spark](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-spark-provisioning-tutorial)

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Список всех приложений, которые поддерживают подготовку пользователей в коллекции Azure AD, см. в статье [Интеграция приложений SaaS с Azure Active Directory](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Connect Health для синхронизации — более простой способ исправить ошибки синхронизации повторяющихся и потерянных атрибутов

**Тип:** Новая функция  
**Категория службы:** AD Connect  
**Возможности продукта:** мониторинг и создание отчетов
 
Azure AD Connect Health предоставляет функцию самостоятельного устранения ошибок, позволяющую выявить и исправить ошибки синхронизации. С помощью этой функции можно устранять ошибки синхронизации повторяющихся атрибутов и ошибки потерянных из Azure AD объектов. Этот способ диагностики обеспечивает следующие преимущества:

- Сужает круг ошибок синхронизации повторяющихся атрибутов, предоставляя нужные исправления.

- Применяет исправление для выделенных сценариев Azure AD, устраняя ошибки за одно действие.

- Для включения и использования этой функции не требуется обновление или настройка.

Дополнительные сведения см. в статье [Диагностика и устранение ошибок синхронизации повторяющихся атрибутов](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors).

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Визуальные обновления для событий входа MSA и Azure AD

**Тип:** Измененная функция  
**Категория службы:** Azure AD  
**Возможности продукта:** Аутентификация пользователей

Мы обновили пользовательский интерфейс для входа в веб-службы Майкрософт, такие как Office 365 и Azure. Благодаря этому изменению экраны стали менее загроможденными и более понятными. Дополнительные сведения об этом изменении см. в записи блога, посвященной [предстоящим улучшениям интерфейса входа Azure AD](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/).

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Новый выпуск Azure AD Connect — июль 2018 г.

**Тип:** Измененная функция  
**Категория службы:** подготовка приложений  
**Возможности продукта:** управление жизненным циклом удостоверений

Последний выпуск Azure AD Connect включает в себя: 

- исправления ошибок и обновления функций поддержки; 

- общедоступную версию интеграции между Ping и Federate;

- обновления для последней версии клиента SQL 2012. 

Дополнительные сведения об этом обновлении см. в статье [Azure AD Connect. История выпусков версий](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history).

---

### <a name="updates-to-the-terms-of-use-tou-end-user-ui"></a>Обновления пользовательского интерфейса с условиями использования (ToU)

**Тип:** Измененная функция  
**Категория службы:** Условия использования  
**Возможности продукта:** Система управления

В пользовательском интерфейсе с условиями использования мы обновляем строку принятия.

**Текущий текст.** "Чтобы получить доступ к ресурсам [имя_клиента], вы должны принять условия использования".<br>**Новый текст.** "Чтобы получить доступ к ресурсам [имя_клиента], вы должны прочитать условия использования".

**Текущий текст.** "Выбирая параметр "Принять", вы подтверждаете, что соглашаетесь со всеми вышеупомянутыми условиями использования".<br>**Новый текст.** "Нажмите кнопку "Принять", чтобы подтвердить, что вы прочитали и поняли условия использования".

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Сквозная проверка подлинности поддерживает устаревшие протоколы и приложения

**Тип:** Измененная функция  
**Категория службы:** аутентификация (вход)  
**Возможности продукта:** Аутентификация пользователей
 
Сквозная проверка подлинности теперь поддерживает устаревшие протоколы и приложения. Теперь полностью поддерживаются следующие ограничения:

- Входы пользователей в устаревшие клиентские приложения Office (Office 2010 и Office 2013) без необходимости применения современной проверки подлинности.

- Общий доступ к календарю и получение сведений о доступности в гибридных средах Exchange поддерживается только для Office 2010.

- Входы пользователей в клиентские приложения Skype для бизнеса без необходимости применения современной проверки подлинности.

- Вход пользователей в PowerShell версии 1.0.

- Программа регистрации устройств Apple (Apple DEP) с помощью помощника по настройке iOS. 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Управление конвергированными сведениями для защиты для самостоятельного сброса пароля и Многофакторной идентификации

**Тип:** Новая функция  
**Категория службы:** SSPR  
**Возможности продукта:** Аутентификация пользователей

Эта новая функция позволяет пользователям управлять своими сведениями для защиты (например, номером телефона, адресом электронной почты, мобильным приложением и т. д.) для самостоятельного сброса пароля (SSPR) и Многофакторной идентификации (MFA) в одном интерфейсе. Пользователям больше не нужно регистрировать одни и те же сведения для защиты для SSPR и MFA в двух разных интерфейсах. Этот новый интерфейс также применяется к пользователям с SSPR или MFA.

Если организация не применяет регистрацию MFA или SSPR, пользователи могут зарегистрировать свои сведения для защиты через портал **My Apps**. Оттуда они могут зарегистрировать любые методы, разрешенные для MFA или SSPR. 

Это общедоступная предварительная версия. Администраторы могут включить новую функцию (при необходимости) для выбранной группы пользователей или всех пользователей в клиенте.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Использование приложения Microsoft Authenticator для проверки идентификации при сбросе пароля

**Тип:** Измененная функция  
**Категория службы:** SSPR  
**Возможности продукта:** Аутентификация пользователей

Эта функция позволяет пользователям, не являющимся администраторами, подтвердить свою личность при сбросе пароля с помощью уведомления или кода от приложения Microsoft Authenticator (или любого другого приложения проверки подлинности). Когда администратор включит этот метод самостоятельного сброса пароля, пользователи, зарегистрировавшие мобильное приложение через aka.ms/mfasetup или aka.ms/setupsecurityinfo, смогут использовать свое мобильное приложение как метод проверки при сбросе пароля.

Уведомление от мобильного приложения можно включить только как часть политики, которая требует двух методов для сброса пароля.

---

## <a name="june-2018"></a>Июнь 2018 г.

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Уведомление об изменении: исправление безопасности потока делегированной авторизации для приложений, использующих API журналов действий Azure AD

**Тип:** план изменений  
**Категория службы:** Отчеты  
**Возможности продукта:** мониторинг и создание отчетов

Из-за более строгих требований к обеспечению безопасности нам пришлось внести изменения в разрешения для приложений, использующих поток делегированной авторизации для доступа к [API для журналов действий Azure AD](https://aka.ms/aadreportsapi). Это изменение вступило в силу **26 июня 2018 г**.

Если какое-либо приложение использует API для журналов действий Azure AD, сделайте следующее, чтобы убедиться, что приложение будет работать корректно после внесения изменения.

**Обновление разрешений для приложения**

1. Войдите на портал Azure и выберите **Azure Active Directory**, а затем выберите **Регистрация приложений**.
2. Выберите приложение, использующее API для журналов действий Azure AD, щелкните **Параметры**, а затем — **Необходимые разрешения** и API **Windows Azure Active Directory**.
3. В области **Необходимые разрешения** колонки **Разрешить доступ** установите флажок рядом с пунктом **Read directory** (Чтение каталога), а затем выберите **Сохранить**.
4. Щелкните **Предоставить разрешения** и затем выберите **Да**.
    
    >[!Note]
    >Необходимо быть глобальным администратором для предоставления разрешений приложению.

Дополнительные сведения см. в разделе [Предоставление разрешений](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) статьи "Предварительные требования для доступа к API отчетов Azure Active Directory".

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>Настройка параметров TLS для подключения к службам Azure AD, чтобы соответствовать требованиям стандарта PCI DSS

**Тип:** Новая функция  
**Категория службы:** Н/Д  
**Возможности продукта:** платформа

Transport Layer Security (TLS) — это протокол, обеспечивающий конфиденциальность и целостность данных между двумя взаимодействующими приложениями, и наиболее широко используемый сегодня протокол безопасности.

[Совет по стандартам безопасности индустрии платежных карт](https://www.pcisecuritystandards.org/) определил, что ранние версии TLS и SSL нужно перестать использовать в пользу новых более безопасных протоколов приложений, соответствующих требованиям, начиная с **30 июня 2018 года**. Это изменение означает, что при подключении к службам Azure AD и требовании соответствия стандарту PCI DSS необходимо отключить TLS 1.0. Доступно несколько версий TLS, но TLS 1.2 — это последняя версия, доступная для служб Azure Active Directory. Корпорация Майкрософт рекомендует перейти непосредственно на TLS 1.2 для комбинаций "клиент — сервер" и "браузер — сервер".

Устаревшие браузеры могут не поддерживать более новые версии TLS, такие как TLS 1.2. Чтобы узнать, какие версии TLS поддерживаются в веб-браузере, перейдите на сайт [Qualys SSL Labs](https://www.ssllabs.com/) и выберите **Test your browser** (Протестировать браузер). Мы рекомендуем вам перейти на последнюю версию веб-браузера и предпочтительно включить только TLS 1.2.

**Включение TLS 1.2 с помощью браузера**

- **Microsoft Edge и Internet Explorer (оба используют Internet Explorer)**

    1. Откройте Internet Explorer, выберите **Сервис** > **Свойства браузера** > **Дополнительно**.
    2. В области **Безопасность** выберите **Использовать TLS 1.2**, а затем нажмите кнопку **ОК**.
    3. Закройте все окна браузера и перезапустите Internet Explorer. 

- **Google Chrome**

    1. Откройте браузер Google Chrome, введите в адресной строке *chrome://settings/* и нажмите клавишу **ВВОД**.
    2. Разверните параметры **Дополнительные**, перейдите в область **Система** и выберите **Настройки прокси-сервера**.
    3. В области **Свойства: Интернет** выберите вкладку **Дополнительно**, перейдите в область **Безопасность** и выберите **Использовать TLS 1.2**, а затем нажмите кнопку **ОК**.
    4. Закройте все окна браузера и перезапустите Google Chrome.

- **Mozilla Firefox**

    1. Откройте Firefox, введите в адресной строке *about:config*, а затем нажмите клавишу **ВВОД**.
    2. Найдите термин *TLS*, а затем выберите запись **security.tls.version.max**.
    3. Задайте значение **3**, чтобы браузер использовал версию TLS 1.2, а затем нажмите кнопку **ОК**.

        >[!NOTE]
        >Версия Firefox 60.0 поддерживает TLS 1.3, поэтому также можно задать для security.tls.version.max значение **4**.

    4. Закройте все окна браузера и перезапустите Mozilla Firefox.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Новые федеративные приложения, доступные в коллекции приложений Azure AD (июнь 2018 г.)

**Тип:** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** интеграция сторонних продуктов
 
В июне 2018 года мы добавили эти 15 новых приложений с поддержкой федерации в коллекцию приложений:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [Settling music](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [бизнес-приложение с включенным токеном SAML 1.1](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Endpoint Backup](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh Networks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho One](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [Локальная среда SharePoint](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [ForeSee CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial), [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Дополнительные сведения об этих приложениях см. в статье [Интеграция приложений SaaS с Azure Active Directory](https://aka.ms/appstutorial). Дополнительные сведения о том, как добавить приложение в коллекцию приложений Azure AD см. в [этой статье](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Защита паролей Azure AD доступна в общедоступной предварительной версии

**Тип:** Новая функция  
**Категория службы:** Защита идентификации  
**Возможности продукта:** Аутентификация пользователей

Защиту паролей Azure AD можно использовать, чтобы исключить легко угадываемые пароли из вашей среды. Исключение этих паролей помогает снизить риск компрометации в случае атаки с подбором пароля.

В частности защита паролей Azure AD помогает вам:

- защитить учетные записи вашей организации как в Azure AD, так и в Windows Server Active Directory (AD); 
- предотвратить использование пользователями паролей из списка 500 наиболее часто используемых паролей и более 1 миллиона вариантов замены этих паролей;
- управлять защитой паролей Azure AD из одного расположения на портале Azure AD для Azure AD и локального экземпляра Windows Server AD.

Дополнительные сведения о защите паролей Azure AD см. в статье [Исключение неправильных паролей в организации](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Шаблон политики условного доступа "все гости", созданный при формировании условий использования

**Тип:** Новая функция  
**Категория службы:** Условия использования  
**Возможности продукта:** Система управления

Во время формирования условий использования также создается шаблон политики условного доступа для всех гостей и всех приложений. Этот новый шаблон политики применяет только что созданные условия использования, упрощая создание и применение процесса для гостей.

Дополнительные сведения см. в статье [Функция "Условия использования Azure Active Directory" (предварительная версия)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Пользовательский шаблон политики условного доступа, созданный при формировании условий использования

**Тип:** Новая функция  
**Категория службы:** Условия использования  
**Возможности продукта:** Система управления

Во время формирования условий использования также создается пользовательский шаблон политики условного доступа. Этот новый шаблон политики позволяет создать условия использования и немедленно перейти к колонке создания политики условного доступа без необходимости вручную переходить по порталу.

Дополнительные сведения см. в статье [Функция "Условия использования Azure Active Directory" (предварительная версия)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Новое комплексное руководство по развертыванию Многофакторной идентификации Azure

**Тип:** Новая функция  
**Категория службы:** Другие  
**Возможности продукта:** безопасность и защита идентификации
 
Мы выпустили новые пошаговые инструкции по развертываю Многофакторной идентификации Azure в организации.

Чтобы просмотреть руководство по развертыванию MFA, перейдите к репозиторию [Identity Deployment Guides](https://aka.ms/DeploymentPlans) в GitHub. Чтобы оставить отзыв о руководстве по развертыванию, используйте [форму обратной связи плана развертывания](https://aka.ms/deploymentplanfeedback). Если у вас возникли вопросы о руководствах по развертыванию, свяжитесь с нами на сайте [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Делегированные роли управления приложением Azure AD в общедоступной предварительной версии

**Тип:** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** Контроль доступа

Теперь администратор может делегировать задачи управления приложением без присвоения роли глобального администратора. Новые роли и возможности:

- **Новые стандартные роли администратора Azure AD:**

    - **Администратор приложений.** Предоставляет возможность управлять всеми аспектами всех приложений, в том числе регистрацией, параметрами единого входа, назначением приложений и лицензированием, параметрами прокси-сервера приложений и согласием (за исключением ресурсов Azure AD).

    - **Администратор облачных приложений.** Предоставляет все возможности администратора приложений, за исключением управления прокси-сервером приложений, так как он не предоставляет локальный доступ.

    - **Разработчик приложений.** Предоставляет возможность создавать регистрации приложений, даже если отключен параметр, **позволяющий пользователям регистрировать приложения**.

- **Владение (настраивается для регистрации отдельного приложения и для приложения отдельного предприятия, аналогично процессу группового владения:**
 
    - **Владелец регистрации приложения.** Предоставляет возможность управлять всеми аспектами имеющейся регистрации приложения, включая манифест приложения и добавление дополнительных владельцев.

    - **Владелец корпоративного приложения.** Предоставляет возможность управлять многими аспектами собственных корпоративных приложений, в том числе параметрами единого входа, назначением приложений и согласием (за исключением ресурсов Azure AD).

Дополнительные сведения об общедоступной предварительной версии см. в записи блога [Hallelujah! Azure AD delegated application management roles are in public preview!](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) (Делегированные роли управления приложением Azure AD в общедоступной предварительной версии) . Дополнительные сведения о ролях и разрешениях см. в статье [Назначение ролей администратора в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---

## <a name="may-2018"></a>Май 2018 г.

### <a name="expressroute-support-changes"></a>Изменения в поддержке ExpressRoute

**Тип.** план изменений  
**Категория службы:** аутентификация (вход)  
**Возможности продукта.** платформа  

Предложения модели "программное обеспечение как услуга", такие как Azure Active Directory (Azure AD), лучше всего работают напрямую через Интернет, не требуя ExpressRoute или любых других частных VPN-туннелей. По этой причине с **1 августа 2018** мы прекращаем поддержку ExpressRoute для служб Azure AD с помощью общедоступного пиринга Azure и сообществ Azure в пиринге корпорации Майкрософт. Во всех службах, на которые повлияет это изменение, трафик Azure AD будет постепенно переходить от ExpressRoute к Интернету.

Мы изменяем способ предоставления поддержки, но понимаем, что в некоторых ситуациях вам может понадобиться выделенный набор каналов для трафика проверки подлинности. Поэтому в Azure AD будут и дальше поддерживаться ограничения диапазона IP-адресов каждого клиента с использованием ExpressRoute и служб, в которых уже используется пиринг Майкрософт, в сообществе "Другие онлайн-службы Office 365" Если затронуты ваши службы, но вам требуется ExpressRoute, необходимо сделать следующее:

- **Если вы используете общедоступный пиринг Azure.** Необходимо перейти к пирингу Майкрософт и зарегистрироваться в сообществе **Другие онлайн-службы Office 365 (12076:5100)**. Дополнительные сведения см. в статье [Переход с общедоступного пиринга на пиринг Майкрософт](https://docs.microsoft.com/azure/expressroute/how-to-move-peering).

- **Если вы используете пиринг Майкрософт.** Зарегистрируйтесь в сообществе **Другие онлайн-службы Office 365 (12076:5100)**. Дополнительные сведения о требованиях к маршрутизации см. в разделе [Поддержка сообществ BGP](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) статьи о требованиях к маршрутизации ExpressRoute.

Если вам необходимо продолжить использование выделенных каналов, потребуется поговорить со службой технической поддержки учетных записей Майкрософт о том, как получить разрешение на использование сообщества**Другие онлайн-службы Office 365 (12076:5100)**. Контрольный совет MS Office проверит, нужны ли вам эти каналы, и убедится, что вы понимаете технические последствия их использования. Несанкционированные подписки, пытающиеся создать фильтры маршрутов для Office 365, получат сообщение об ошибке. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>API Microsoft Graph для администрирования условий использования

**Тип.** Новая функция  
**Категория службы:** Условия использования  
**Возможности продукта.** Опыт разработчика
 
Мы добавили API Microsoft Graph для администрирования условий использования Azure AD. Вы можете создавать, изменять и удалять объект условий использования.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Добавление мультитенантной конечной точки Azure AD в качестве поставщика удостоверений в Azure AD B2C

**Тип.** Новая функция  
**Категория службы.** B2C — управление удостоверениями потребителей  
**Возможности продукта.** B2B/B2C
 
Теперь с помощью пользовательских политик вы можете добавить обычную конечную точку Azure AD в качестве поставщика удостоверений в Azure AD B2C. Так у вас будет единая точка входа для всех пользователей Azure AD, которые входят в ваши приложения. Дополнительные сведения см. в статье [Настройка входа для мультитенантного Azure Active Directory с помощью пользовательских политик в Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Использование внутренних URL-адресов для доступа к приложениям из любого расположения с помощью компонента "Расширение защищенного входа в мои приложения" и Azure AD Application Proxy

**Тип.** Новая функция  
**Категория службы:** мои приложения  
**Возможности продукта:** Единый вход
 
Теперь пользователи могут получить доступ к приложениям через внутренние URL-адреса даже за пределами корпоративной сети с помощью компонента "Расширение защищенного входа в мои приложения" для Azure AD. Эта возможность поддерживается в любых приложениях, опубликованных с помощью Azure AD Application Proxy, и браузерах, на которых также установлено расширение "Панель доступа". Возможность перенаправления URL-адресов включается автоматически, когда пользователь входит в расширение. Расширение можно скачать для браузеров [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367) и [Firefox](https://go.microsoft.com/fwlink/?linkid=866366).

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory — данные в Европе для европейских клиентов

**Тип.** Новая функция  
**Категория службы:** Другие  
**Возможности продукта:** GoLocal

Для соблюдения законов о конфиденциальности и другого европейского законодательства данные клиентов из Европы должны оставаться в Европе и не реплицироваться за пределы европейских центров обработки данных. См. дополнительные сведения о том, [какая информация об удостоверениях будет храниться в Европе и за ее пределами](https://go.microsoft.com/fwlink/?linkid=872328). 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Новые возможности интеграции для подготовки пользователей с приложениями SaaS — май 2018 г.

**Тип.** Новая функция  
**Категория службы:** подготовка приложений  
**Возможности продукта:** интеграция сторонних продуктов
 
Служба Azure AD предоставляет возможность автоматизировать создание, обслуживание и удаление удостоверений пользователей в приложениях SaaS, таких как Dropbox, Salesforce, ServiceNow и т. д. В мае 2018 года мы добавили поддержку подготовки пользователей в следующие приложения в коллекции приложений Azure AD:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Cornerstone OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Список всех приложений, которые поддерживают подготовку пользователей, в коллекции Azure AD см. в статье [https://aka.ms/appstutorial](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Функция проверки доступа Azure AD для групп и приложений теперь поддерживает повторяющиеся проверки

**Тип.** Новая функция  
**Категория службы.** Проверки доступа  
**Возможности продукта.** Система управления
 
Функция проверки доступа для групп и приложений стала общедоступной в Azure AD Premium P2.  Администраторы смогут настраивать автоматическое повторение операций проверки доступа для членств в группах и назначений приложений с регулярным интервалом, например ежемесячно или ежеквартально.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Журналы действий Azure AD (вход и аудит) теперь доступны через MS Graph

**Тип.** Новая функция  
**Категория службы:** Отчеты  
**Возможности продукта:** Мониторинг и создание отчетов
 
Журналы действий Azure AD, в том числе журналы входа и аудита, теперь доступны через MS Graph. Мы предоставили через MS Graph две конечных точки для доступа к этим журналам. Ознакомьтесь с [документацией](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) по программному доступу к API отчетов Azure AD. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>Улучшения процедуры активации B2B и возможности покинуть организацию

**Тип.** Новая функция  
**Категория службы:** B2B  
**Возможности продукта:** B2B/B2C

**Оперативная активация.** Когда вы предоставляете гостевому пользователю ресурс для общего доступа с помощью API B2B, вам не нужно отправлять специальное сообщение с приглашением. В большинстве случаев гостевой пользователь может обратиться к ресурсу, после чего выполняется оперативная процедура активации. Больше никаких недоразумений из-за пропущенных сообщений электронной почты. Больше никаких вопросов "А вы щелкнули ссылку активации, предоставленную системой?" Это означает, что когда SPO использует диспетчер приглашений, облачные вложения могут иметь одинаковый канонический URL-адрес для всех пользователей — как внутренних, так и внешних — в любом состоянии активации.

**Современная процедура активации.** Больше никакой целевой страницы активации с разделением экрана. Пользователи увидят современный согласованный интерфейс с заявлением о конфиденциальности приглашающей организации, как это происходит для приложений сторонних разработчиков.

**Гостевые пользователи могут покинуть организацию.** Когда сотрудничество с организацией подойдет к концу, пользователь может самостоятельно покинуть ее. Больше никаких звонков администратору приглашающей организации с просьбой об удалении и никаких запросов в службу поддержки.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Новые федеративные приложения, доступные в коллекции приложений Azure AD — май 2018 г.

**Тип.** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** Интеграция сторонних продуктов
 
В мае 2018 года мы добавили 18 новых приложений с поддержкой федерации в нашу коллекцию приложений:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), Infogix Data3Sixty Govern, [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Captivate Prime](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [Montage Online](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial), [まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [Arc Publishing - SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial).

Дополнительные сведения об этих приложениях см. в статье [Интеграция приложений SaaS с Azure Active Directory](https://aka.ms/appstutorial).

Дополнительные сведения о том, как добавить приложение в коллекцию приложений Azure AD см. в [этой статье](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Новое пошаговое руководство по развертыванию для Azure Active Directory

**Тип.** Новая функция  
**Категория службы:** Другие  
**Возможности продукта:** Каталог
 
Новое пошаговое руководство по развертыванию Azure Active Directory, которое включает такие темы, как самостоятельный сброс пароля (SSPR), единый вход (SSO), условный доступ, прокси-сервер приложения, подготовка пользователей, службы федерации Active Directory для сквозной проверки подлинности и ADFS для синхронизации хэша паролей.

Чтобы просмотреть руководство по развертыванию, перейдите к репозиторию [Identity Deployment Guides](https://aka.ms/DeploymentPlans) в GitHub. Чтобы оставить отзыв о руководстве по развертыванию, используйте [форму обратной связи плана развертывания](https://aka.ms/deploymentplanfeedback). Если у вас возникли вопросы о руководствах по развертыванию, свяжитесь с нами на сайте [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="enterprise-applications-search---load-more-apps"></a>Поиск корпоративных приложений — загрузка дополнительных приложений

**Тип.** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** Единый вход
 
У вас возникают трудности с поиском приложений или субъектов-служб? Теперь вы можете загружать больше приложений в списке всех корпоративных приложений. По умолчанию отображается 20 приложений. Теперь можно щелкнуть **Загрузить больше**, чтобы просмотреть дополнительные приложения. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>Майский выпуск AADConnect содержит общедоступную предварительную версию функции интеграции с PingFederate, важные обновления для системы безопасности, множество исправлений ошибок и новые средства для устранения неполадок. 

**Тип.** Измененная функция  
**Категория службы.** AD Connect  
**Возможности продукта.** Управление жизненным циклом удостоверений
 
Майский выпуск AADConnect содержит общедоступную предварительную версию функции интеграции с PingFederate, важные обновления для системы безопасности, множество исправлений ошибок и новые средства для устранения неполадок. См. [заметки о выпуске](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190).

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Проверки доступа Azure AD: автоматическое применение

**Тип.** Измененная функция  
**Категория службы.** Проверки доступа  
**Возможности продукта.** Система управления

Функция проверки доступа для групп и приложений стала общедоступной в Azure AD Premium P2. Администратор может автоматически применить изменения рецензента к соответствующей группе или соответствующему приложению после завершения проверки доступа. Кроме того, он может указать, что происходит с доступом пользователя, если рецензенты не ответили, а именно: запрет доступа, сохранение доступа или следование рекомендациям системы. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-responsemode-for-new-apps"></a>Маркеры идентификации теперь невозможно возвращать с помощью режима ответа query для новых приложений. 

**Тип.** Измененная функция  
**Категория службы:** аутентификация (вход)  
**Возможности продукта:** Аутентификация пользователей
 
Приложения, созданные 25 апреля 2018 года или позднее, больше не могут запрашивать значение для **id_token** с использованием режима ответа **query**.  Это обеспечивает соответствие Azure AD спецификациям OIDC и помогает снизить уязвимость ваших приложений перед атаками.  Приложения, созданные до 25 апреля 2018 года, могут использовать режим ответа **query** с типом запроса **id_token**.  При запросе значения id_token из AAD возвращается ошибка **AADSTS70007: query не является поддерживаемым значением для режима response_mode при запросе токена**.

Значения **fragment** и **form_post** для режимов ответа по-прежнему доступны при создании новых объектов приложения (например, для использования прокси приложения). Вам следует использовать одно из этих значений response_mode, прежде чем создавать приложения.  

---
 
## <a name="april-2018"></a>Апрель 2018 г. 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Общедоступная версия маркера доступа Azure AD B2C

**Тип.** Новая функция  
**Категория службы.** B2C — управление удостоверениями потребителей  
**Возможности продукта.** B2B/B2C 

Теперь доступ к интерфейсам веб-API, защищенным Azure AD B2C, можно получать с помощью маркеров доступа. Эта функция переходит от общедоступной предварительной версии к общедоступной версии. Улучшен пользовательский интерфейс для настройки приложений Azure AD B2C и веб-API, а также внесены другие незначительные улучшения.
 
Дополнительные сведения см. в статье [Azure AD B2C: запрос маркеров доступа](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>Тестирование конфигурации единого входа для приложений на основе SAML

**Тип.** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** Единый вход

При настройке приложений с функцией единого входа на основе SAML вы можете протестировать интеграцию на странице конфигурации. Если в процессе входа возникнет ошибка, ее можно указать в интерфейсе тестирования, и Azure AD предоставит вам пошаговые инструкции по решению конкретной проблемы.

Дополнительные сведения можно найти в разделе 

- [Настройка единого входа для приложений, которых нет в коллекции приложений Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Отладка единого входа на основе SAML в приложения в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Условия использования Azure AD теперь включают отчеты "на пользователя"

**Тип.** Новая функция  
**Категория службы:** Условия использования  
**Возможности продукта.** Соответствие нормативным требованиям
 
Администраторы теперь могут выбрать заданные условия использования и просмотреть список всех пользователей, которые приняли эти условия, а также дату и время этого события.

Дополнительные сведения см. в статье [Функция "Условия использования Azure Active Directory"](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health. Обнаружение ненадежных IP-адресов для защиты блокировки экстрасети AD FS 

**Тип.** Новая функция  
**Категория службы:** Другие  
**Возможности продукта.** Мониторинг и создание отчетов

Connect Health теперь поддерживает обнаружение IP-адресов, для которых превышается пороговое количество неудачных попыток входа с именем пользователя и паролем в час или в день. Эта функция предоставляет следующие возможности:

- Полный отчет, который содержит IP-адрес и число неудачных попыток входа и формируется каждый час или каждый день с настраиваемым пороговым значением.
- Оповещения по электронной почте, которые появляются, когда для конкретного IP-адреса превышается порог неудачных попыток входа с именем пользователя и паролем в час или в день.
- Возможность скачать отчет для подробного анализа данных

Дополнительные сведения см. в разделе об [отчете по ненадежным IP-адресам](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Простая настройка приложений с помощью файла метаданных или URL-адреса

**Тип.** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** Единый вход

На странице приложений Enterprise администраторы могут отправить файл метаданных SAML для настройки входа на основе SAML для приложений из коллекции AAD и не входящих в нее.

Кроме того, URL-адрес метаданных федерации приложения Azure AD можно использовать для настройки единого входа в целевом приложении.

Дополнительные сведения см. в статье [Настройка единого входа для приложений, которых нет в коллекции приложений Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Выпущена общедоступная версия функции "Условия использования"

**Тип.** Новая функция  
**Категория службы:** Условия использования  
**Возможности продукта.** Соответствие нормативным требованиям
 

Функция "Условия использования" Azure AD перемещена из общедоступной предварительной версии в общедоступную версию.

Дополнительные сведения см. в статье [Функция "Условия использования Azure Active Directory"](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Предоставление или отзыв приглашений пользователям B2B из отдельных организаций

**Тип.** Новая функция  
**Категория службы:** B2B  
**Возможности продукта:** B2B/B2C
 

Теперь можно указать, с какими партнерскими организациями вы хотите совместно использовать данные и совместно работать в службе совместной работы Azure AD B2B. Для этого можно создать список определенных разрешенных или запрещенных доменов. При блокировке домена с помощью этих возможностей сотрудники больше не смогут отправлять приглашения пользователям из этого домена.

Это помогает контролировать доступ к ресурсам, обеспечивая при этом удобную работу для утвержденных пользователей.

Функция совместной работы B2B доступна для всех клиентов Azure Active Directory. Ее можно использовать в сочетании с функциями Azure AD Premium, такими как условный доступ и защита идентификации для более детального контроля над тем, когда и как внешние бизнес-пользователи выполняют вход и получают доступ.

Дополнительные сведения см. в статье [Предоставление или отзыв приглашений пользователям B2B из отдельных организаций](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Новые федеративные приложения доступны в коллекции приложений Azure AD

**Тип.** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** Интеграция сторонних продуктов

В апреле 2018 года мы добавили 13 новых приложений с поддержкой федерации в нашу коллекцию приложений:

Criterion HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [Secret Server (локальный)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [Dynamic Signal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [OrgChart Now](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Performance Monitor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial), [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), Shelf и [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial).

Дополнительные сведения об этих приложениях см. в статье [Интеграция приложений SaaS с Azure Active Directory](https://aka.ms/appstutorial).

Дополнительные сведения о том, как добавить приложение в коллекцию приложений Azure AD см. в [этой статье](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Предоставление пользователям B2B в Azure AD доступа к локальным приложениям (общедоступная предварительная версия)

**Тип.** Новая функция  
**Категория службы:** B2B  
**Возможности продукта:** B2B/B2C

Теперь организации, использующие возможности совместной работы в Azure Active Directory (AAD) B2B для приглашения гостевых пользователей из партнерских организаций в AAD, могут предоставлять таким пользователям B2B доступ к локальным приложениям. В таких приложениях нужно использовать аутентификацию на основе SAML или встроенную проверку подлинности Windows (IWA) с ограниченным делегированием Kerberos (KCD).

Дополнительные сведения см. в статье [Предоставление пользователям B2B в Azure AD доступа к локальным приложениям](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Получение руководств по интеграции единого входа из Azure Marketplace

**Тип.** Измененная функция  
**Категория службы:** Другие  
**Возможности продукта.** Интеграция сторонних продуктов

Если приложение, указанное в [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) поддерживает единый вход на основе SAML, то, щелкнув **Получить**, можно открыть руководство по интеграции, связанное с этим приложением. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Повышение производительности автоматической подготовки пользователей Azure AD в приложениях SaaS

**Тип.** Измененная функция  
**Категория службы:** подготовка приложений  
**Возможности продукта:** Интеграция сторонних продуктов
 
Ранее клиенты, которые с помощью соединителей подготавливали пользователей Azure Active Directory для приложений SaaS (например, Salesforce, ServiceNow и Box), могли испытывать снижение производительности. Так происходило, если в их клиентах Azure AD содержалось свыше 100 000 совокупных пользователей и групп, и для определения пользователей, которых следует подготовить, они использовали назначения пользователей и групп.

2 апреля 2018 года мы развернули значительные улучшения производительности для службы подготовки Azure AD, которые существенно сокращают время, необходимое на выполнение начальной синхронизации Azure Active Directory с целевыми приложениями SaaS.

В результате многие клиенты, у которых начальная синхронизация с приложениями занимала много дней или никогда не завершалась, сейчас завершают ее в течение нескольких минут или часов.

Дополнительные сведения см. в разделе [Что происходит при подготовке](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning).

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Самостоятельный сброс пароля с экрана блокировки Windows 10 для компьютеров, присоединенных к гибридной среде Azure AD

**Тип.** Измененная функция  
**Категория службы:** Самостоятельный сброс пароля  
**Возможности продукта:** Аутентификация пользователей
 
Мы обновили компонент Windows 10 SSPR, добавив поддержку компьютеров, присоединенных к гибридной среде Azure AD. Эта функция доступна в Windows 10 RS4 и дает пользователям возможность сбрасывать пароль с экрана блокировки на компьютере с Windows 10. Эту функцию могут применять пользователи, которые включены и зарегистрированы для самостоятельного сброса пароля.

Дополнительные сведения см. в статье [Сброс пароля Azure AD на экране входа](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>Март 2018 г.
 
### <a name="certificate-expire-notification"></a>Уведомление об истечении срока действия сертификата

**Тип.** исправление  
**Категория службы.** корпоративные приложения  
**Возможности продукта:** Единый вход
 
Azure AD отправляет уведомление о сертификатах с истекающим сроком действия для приложений из коллекции или не из коллекции. 

Некоторые пользователи не получают уведомления о корпоративных приложениях, для которых настроен единый вход для на основе SAML. Эта проблема была устранена. Azure AD отправляет уведомление для сертификатов, срок действия которых истекает через 7, 30 и 60 дней. Это событие отображается в журналах аудита. 

Дополнительные сведения можно найти в разделе 

- [Управление сертификатами для федеративного единого входа в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Отчеты о действиях аудита на портале Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Поставщики удостоверений Twitter и GitHub в Azure AD B2C

**Тип.** Новая функция  
**Категория службы.** B2C — управление удостоверениями потребителей  
**Возможности продукта.** B2B/B2C
 
Теперь в Azure AD B2C можно добавить Twitter или GitHub в качестве поставщика удостоверений. От общедоступной предварительной версии Twitter переходит к общедоступной версии. Для GitHub выпускается общедоступная предварительная версия.

Дополнительные сведения см. в статье [Что такое служба совместной работы Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Ограничение доступа в браузере с помощью Intune Managed Browser и условного доступа на основе приложений Azure AD для iOS и Android

**Тип.** Новая функция  
**Категория службы.** условный доступ;  
**Возможности продукта.** Безопасность и защита удостоверений
 
**Сейчас на этапе общедоступной предварительной версии**

**Единый вход в Intune Managed Browser.** Ваши сотрудники могут использовать единый вход через собственные клиенты (например, Microsoft Outlook) и Intune Managed Browser для всех подключенных приложений Azure AD.

**Поддержка условного доступа в Intune Managed Browser.** Теперь вы можете обязать сотрудников использовать Intune Managed Browser с соблюдением политик условного доступа на основе приложений.

Дополнительные сведения об этом см. в нашем [блоге](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Дополнительные сведения можно найти в разделе 

- [Условный доступ на основе приложений Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Управление доступом в Интернет с помощью политик Managed Browser в Microsoft Intune](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>Командлеты для прокси приложения в общедоступной версии модуля PowerShell

**Тип.** Новая функция  
**Категория службы:** Прокси приложения  
**Возможности продукта:** Контроль доступа
 
Теперь в общедоступной версии модуля PowerShell поддерживаются командлеты для прокси приложения. Для этого требуется своевременно обновлять модули PowerShell. Если используемая версия модуля не будет обновляться более одного года, то некоторые командлеты могут перестать работать. 

Дополнительные сведения см. в разделе [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Поддержка собственных клиентов Office 365 службой простого единого входа с помощью неинтерактивного протокола

**Тип.** Новая функция  
**Категория службы.** аутентификация (вход)  
**Возможности продукта:** Аутентификация пользователей
 
Пользователь, применяющий собственные клиенты Office 365 (версии 16.0.8730.xxxx и более поздней версии), может использовать автоматический вход с помощью простого единого входа. Поддержка этой возможности обеспечивается за счет добавления в Azure AD неинтерактивного протокола (WS-Trust).

Дополнительные сведения см. в разделе [Как работает вход с функцией простого единого входа в собственном клиенте?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Пользователи могут использовать автоматический вход с помощью простого единого входа, если приложение отправляет запросы на вход на клиентские конечные точки Azure AD

**Тип.** Новая функция  
**Категория службы.** аутентификация (вход)  
**Возможности продукта:** Аутентификация пользователей
 
Пользователи могут использовать автоматический вход с помощью простого единого входа, если приложение (например, `https://contoso.sharepoint.com`) отправляет запросы на вход на клиентские конечные точки Azure AD, т. е. `https://login.microsoftonline.com/contoso.com/<..>` или `https://login.microsoftonline.com/<tenant_ID>/<..>`, вместо обычной конечной точки Azure AD (`https://login.microsoftonline.com/common/<...>`).

Дополнительные сведения см. в разделе [Устранение неполадок с простым единым входом Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Необходимо добавить в параметры зоны интрасети пользователей только один URL-адрес Azure AD вместо двух (как было раньше) для развертывания простого единого входа

**Тип.** Новая функция  
**Категория службы.** аутентификация (вход)  
**Возможности продукта:** Аутентификация пользователей
 
Чтобы развернуть простой единый вход для пользователей, необходимо добавить только один URL-адрес Azure AD в параметры зоны интрасети пользователей с помощью групповой политики Active Directory: `https://autologon.microsoftazuread-sso.com`. Ранее клиентам требовалось добавить два URL-адреса.

Дополнительные сведения см. в разделе [Устранение неполадок с простым единым входом Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Новые федеративные приложения доступны в коллекции приложений Azure AD

**Тип.** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** Интеграция сторонних продуктов

В марте 2018 года мы добавили 15 новых приложений с поддержкой федерации в нашу коллекцию приложений:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Assistant by FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Дополнительные сведения об этих приложениях см. в статье [Интеграция приложений SaaS с Azure Active Directory](https://aka.ms/appstutorial).

Дополнительные сведения о том, как добавить приложение в коллекцию приложений Azure AD см. в [этой статье](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>Выпущена общедоступная версия PIM для ресурсов Azure

**Тип.** Новая функция  
**Категория службы:** Управление привилегированными пользователями (PIM)  
**Возможности продукта:** Управление привилегированными пользователями (PIM)
 
Теперь при использовании управления привилегированными пользователями Azure AD для ролей каталога можно применять возможности временного ограничения доступа и назначения для таких ролей ресурсов Azure, как подписки, группы ресурсов, виртуальные машины, а также любого другого ресурса, поддерживаемого Azure Resource Manager. Вы можете применять Многофакторную идентификацию при JIT-активации ролей и планировать активацию в соответствии с утвержденными периодами изменений. Кроме того, в этом выпуске добавлены усовершенствования, отсутствующие в общедоступной предварительной версии, включая обновленный пользовательский интерфейс, рабочие процессы утверждения и возможность расширения ролей с истекающим сроком действия и обновления ролей с истекшим сроком действия.

Дополнительные сведения см. в статье [PIM для ресурсов Azure (предварительная версия)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Добавление необязательных утверждений для токенов приложений (предварительная версия)

**Тип.** Новая функция  
**Категория службы.** аутентификация (вход)  
**Возможности продукта:** Аутентификация пользователей
 
Теперь приложение Azure AD может запрашивать пользовательские или необязательные утверждения в токенах JWT или SAML.  Это утверждения о пользователе или клиенте, которые не включены по умолчанию в токен из-за ограничения размера или применимости.  В настоящий момент доступна общедоступная предварительная версия этой функции для приложений Azure AD в конечных точках версии 1.0 и версии 2.0.  Ознакомьтесь с соответствующей документацией, чтобы получить сведения о том, какие утверждения могут быть добавлены и как изменить манифест приложения, чтобы запросить их.  

Дополнительные сведения см. в статье [Optional claims in Azure AD (preview)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims) (Необязательные утверждения в Azure AD (предварительная версия)).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD поддерживает PKCE для более безопасных потоков OAuth

**Тип.** Новая функция  
**Категория службы.** аутентификация (вход)  
**Возможности продукта:** Аутентификация пользователей
 
Документация по Azure AD была обновлена с учетом поддержки PKCE, что обеспечивает более безопасную связь во время потока предоставления кода авторизации OAuth 2.0.  S256 и code_challenge без шифрования поддерживаются в конечных точках версии 1.0 и версии 2.0. 

Дополнительные сведения см. в разделе [Запрос кода авторизации](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Поддержка подготовки всех значений атрибутов пользователя, доступных в API Get_Workers Workday

**Тип.** Новая функция  
**Категория службы:** подготовка приложений  
**Возможности продукта:** Интеграция сторонних продуктов
 
Общедоступная предварительная версия функции подготовки входящих данных из Workday в Active Directory и Azure AD теперь поддерживает возможность извлечения и подготовки всех значений атрибутов в API Get_Workers Workday. Добавлена поддержка сотен дополнительных стандартных и пользовательских атрибутов, не входящих в состав первоначальной версии соединителя подготовки входящих данных Workday.

Дополнительные сведения можно найти в разделе  [Настройка списка атрибутов пользователя Workday](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Изменение динамического членства в группе на статическое (и наоборот)

**Тип.** Новая функция  
**Категория службы.** Управление группами  
**Возможности продукта:** Совместная работа
 
Можно изменить способ управления членством в группе. Это удобно, если требуется сохранить имя и идентификатор группы в системе, чтобы все существующие ссылки на эту группу по-прежнему были действительны. При создании новой группы эти ссылки потребовалось бы обновить.
Мы обновили центр администрирования Azure AD, чтобы поддержать эту функцию. Теперь клиенты могут изменять динамическое членство в существующих группах на статическое (и наоборот). Существующие командлеты PowerShell по-прежнему доступны.

Дополнительные сведения см. в разделе [правила динамического членства для групп в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Улучшенный выход при простом едином входе

**Тип.** Измененная функция  
**Категория службы:** аутентификация (вход)  
**Возможности продукта:** Аутентификация пользователей
 
Ранее, если пользователи даже после явного выхода из приложения, защищенного Azure AD, пытались получить доступ к приложению Azure AD в своей корпоративной сети с собственных устройств, присоединенных к домену, они автоматически повторяли вход с помощью простого единого входа. Благодаря данному изменению поддерживается выход.  Это позволяет пользователям выбрать другую или ту же учетную запись Azure AD для входа вместо того, чтобы автоматически входить с помощью простого единого входа.

Дополнительные сведения см. в разделе [Устранение неполадок с простым единым входом Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso).
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Выпущен соединитель прокси приложения версии 1.5.402.0

**Тип.** Измененная функция  
**Категория службы:** Прокси приложения  
**Возможности продукта.** Безопасность и защита удостоверений
 
Эта версия соединителя постепенно разворачивалась в ноябре. Данная новая версия соединителя включает в себя следующие изменения.

- Теперь соединитель задает файлы cookie уровня домена, а не уровня поддомена. Это обеспечивает более удобный единый вход и позволяет избежать избыточных запросов аутентификации.
- Поддержка запросов на поблочное кодирование.
- Улучшенный мониторинг работоспособности соединителей. 
- Устранено несколько ошибок и повышена стабильность.

Дополнительные сведения см. в разделе [Сведения о соединителях прокси приложения Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Февраль 2018 г.
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Улучшенная навигация для управления пользователями и группами

**Тип.** план изменений  
**Категория службы.** Управление каталогом  
**Возможности продукта:** Каталог

Упрощена навигация для управления пользователями и группами. Теперь вы можете переходить из обзора каталога непосредственно к списку всех пользователей с удобным доступом к списку удаленных пользователей, а также к списку всех групп с удобным доступом к параметрам управления группами. Кроме того, на странице обзора каталога можно выполнить поиск пользователя, группы, корпоративного приложения или регистрацию приложения. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Доступность журналов входов в систему и отчетов об аудите в Microsoft Azure, обслуживаемой 21Vianet (21Vianet в Azure для Китая)

**Тип.** Новая функция  
**Категория службы.** Azure Stack  
**Возможности продукта.** Мониторинг и создание отчетов

Отчеты журнала действий Azure AD теперь доступны в Microsoft Azure, обслуживаемой экземплярами 21Vianet (21Vianet в Azure для Китая). Сюда относятся следующие журналы:

- **Журналы действий входов в систему.** Журналы всех входов в систему, связанные с вашим клиентом.

- **Журналы самостоятельного аудита паролей.** Все журналы аудита SSPR.

- **Журналы аудита управления каталогами.** Все журналы аудита, связанные с управлением каталогами, например управлением пользователями, управлением приложениями и другими видами управления.

Из этих журналов можно получить важные сведения о том, как работает ваша среда. Полученные данные позволят вам выполнять следующее:

- определять, как приложения и службы используются пользователями;

- устранить неполадки, влияющие на работу пользователей.

Дополнительные сведения об использовании этих отчетов см. в статье [Отчеты Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Просмотр отчетов об активности Azure AD с использованием роли "Читатель отчетов" (без прав администратора роли)

**Тип.** Новая функция  
**Категория службы:** Отчеты  
**Возможности продукта:** Мониторинг и создание отчетов

Прислушиваясь к предложениям клиентов предоставить ролям без прав администратора доступ к журналам действий Azure AD, мы предоставили пользователям, которым назначена роль "Читатель отчетов", доступ к действиям, связанным с входом и аудитом, на портале Azure, а также с помощью наших API Graph. 

Дополнительные сведения об использовании этих отчетов см. в статье [Отчеты Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Утверждение EmployeeID, доступное в виде атрибута пользователя и идентификатора пользователя

**Тип.** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** Единый вход
 
Утверждение **EmployeeID** можно настроить как идентификатор пользователя и атрибут пользователя для пользователей-участников и гостей B2B в приложении с входом на основе SAML из пользовательского интерфейса корпоративного приложения.

Дополнительные сведения см. в статье [Настройка утверждений, выпущенных в токене SAML для корпоративных приложений в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Упрощенное управление приложениями с использованием подстановочных знаков в Azure AD Application Proxy

**Тип.** Новая функция  
**Категория службы:** Прокси приложения  
**Возможности продукта.** Аутентификация пользователей
 
Чтобы облегчить процесс развертывания приложений и сократить административные расходы, мы теперь поддерживаем возможность публикации приложений с использованием подстановочных знаков. Чтобы опубликовать приложение с подстановочным знаком, можно выполнить стандартный поток публикации приложения, но во внутренних и внешних URL-адресах использовать подстановочный знак.

Дополнительные сведения см. в статье [Приложения с подстановочным знаком в прокси приложения Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard).

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Новые командлеты для поддержки конфигурации прокси-сервера приложения

**Тип.** Новая функция  
**Категория службы:** Прокси приложения  
**Возможности продукта.** платформа

В последнем выпуске модуля предварительной версии AzureAD PowerShell содержатся новые командлеты, позволяющие клиентам настраивать приложения прокси-сервера приложения с помощью PowerShell.

Ниже перечислены новые командлеты. 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Новые командлеты для поддержки конфигурации групп

**Тип.** Новая функция  
**Категория службы:** Прокси приложения  
**Возможности продукта.** платформа

В последнем выпуске модуля AzureAD PowerShell содержатся командлеты для управления группами в Azure AD. Эти командлеты ранее были доступны в модуле AzureADPreview и теперь добавлены в модуль AzureAD

К командлетам группы, которые теперь входят в общедоступную версию, относятся: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Доступна новая версия Azure AD Connect

**Тип.** Новая функция  
**Категория службы.** AD Sync  
**Возможности продукта.** платформа
 
Azure AD Connect предпочтительней использовать для синхронизации данных между Azure AD и локальными источниками данных, включая Windows Server Active Directory и LDAP.

>[!Important]
>В этой сборке представлены изменения схем и правил синхронизации. После обновления служба синхронизации Azure AD Connect запустит действия полного импорта и полной синхронизации. Сведения о том, как изменить эту реакцию на событие, см. в разделе [Как отложить полную синхронизацию после обновления](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Этот выпуск содержит следующие обновления и изменения.

**Исправленные проблемы**

- Устранены ошибки с временным окном выполнения фоновых задач для страницы фильтрации разделов при переходе на следующую страницу.

- Устранена ошибка, вызывавшая нарушение прав доступа во время выполнения настраиваемого действия ConfigDB.

- Устранена ошибка восстановления после превышения времени ожидания подключения к SQL.

- Устранена ошибка, из-за которой сертификаты с подстановочными знаками SAN не проходили предварительную проверку.

- Устранена ошибка, из-за которой выполнение файла miiserver.exe завершалось сбоем при экспорте соединителя AAD.

- Устранена ошибка, когда при вводе неправильного пароля выполнялся вход в контроллер домена при запуске мастера AAD Connect для изменения конфигурации.

**Новые функции и внесенные улучшения**
 
- Телеметрия приложений. Администратор может включать и отключать этот класс данных.

- Данные о работоспособности Azure AD. Для управления параметрами работоспособности администратору необходимо зайти на портал работоспособности. После изменения политики службы агенты прочитают и применят ее.

- Добавлены действия настройки обратной записи устройств и индикатор выполнения для инициализации страницы.

- Усовершенствована общая диагностика, предоставляемая в отчете HTML, и полный сбор данных, предоставляемых в ZIP-файле или отчете HTML.

- Усовершенствована надежность автоматического обновления и добавлена дополнительная телеметрия для определения работоспособности сервера.

- Ограничены доступные разрешения привилегированных учетных записей в учетной записи соединителя AD. Мастер ограничивает разрешения привилегированных учетных записей для новых установок после создания учетной записи MSOL. Изменения влияют на быстрые установки и выборочные установки с автоматически создаваемой учетной записью.

- Изменен установщик. Теперь ему не нужно разрешение сопоставления безопасности для чистой установки AAD Connect.

- Новая служебная программа для устранения неполадок с синхронизацией определенного объекта. В настоящее время служебная программа проверяет следующие вещи:

    - Несоответствие UserPrincipalName между синхронизированными объектом пользователя и учетной записью пользователя в клиенте Azure AD.
  
    - Фильтруется ли объект после синхронизации при фильтрации домена.
  
    - Фильтруется ли объект после синхронизации при фильтрации организационной единицы.

- Новая служебная программа для синхронизации текущего хэша паролей, хранящегося в локальном каталоге Active Directory для конкретной учетной записи пользователя. Служебная программа не требует изменения пароля. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Добавлены приложения, поддерживающие политики Защиты приложений Intune для использования с условным доступом на основе приложения Azure AD

**Тип.** Измененная функция  
**Категория службы.** условный доступ;  
**Возможности продукта.** Безопасность и защита удостоверений

Мы добавили больше приложений, которые поддерживают условный доступ на основе приложений. Теперь вы можете получить доступ к Office 365 и другим облачным приложениям, подключенным к Azure AD, с помощью этих клиентских приложений.

К концу февраля будут добавлены следующие приложения:

- Microsoft Power BI

- Microsoft Launcher;

- Microsoft Invoicing.

Дополнительные сведения можно найти в разделе 

- [Требование утвержденного клиентского приложения](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Условный доступ на основе приложений Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Условия использования обновлены для работы с мобильным приложением 

**Тип.** Измененная функция  
**Категория службы:** Условия использования  
**Возможности продукта.** Соответствие нормативным требованиям

При отображении условий использования необходимо щелкнуть **Проблемы с просмотром? Щелкните здесь**. При выборе этой ссылки условия использования откроются, адаптировавшись под ваше устройство. Независимо от размера шрифта в документе или размера экрана устройства, документ можно увеличить, чтобы прочитать нужный фрагмент. 

---
 
## <a name="january-2018"></a>Январь 2018 г.
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Новые федеративные приложения доступны в коллекции приложений Azure AD 

**Тип.** Новая функция  
**Категория службы:** корпоративные приложения  
**Возможности продукта:** Интеграция сторонних продуктов

В январе 2018 года в коллекцию приложений были добавлены следующие новые приложения с поддержкой федерации:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [программное обеспечение управления конфиденциальностью OneTrust](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), федеративный каталог IriusRisk и [Fidelity NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Дополнительные сведения об этих приложениях см. в статье [Интеграция приложений SaaS с Azure Active Directory](https://aka.ms/appstutorial).

Дополнительные сведения о том, как добавить приложение в коллекцию приложений Azure AD см. в [этой статье](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Вход в систему с обнаруженным дополнительным риском

**Тип.** Новая функция  
**Категория службы:** Защита идентификации  
**Возможности продукта.** Безопасность и защита удостоверений

Аналитические данные, которые вы получаете для обнаруженного события риска, привязаны к вашей подписке Azure AD. Выпуск Azure AD Premium P2 позволяет получать самые подробные сведения обо всех основных обнаружениях.

В выпуске Azure AD Premium P1 обнаружения, которые не покрывает лицензия, отображаются как событие риска "Вход в систему с обнаруженным дополнительным риском".

Дополнительные сведения см. в статье [События риска Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Скрытие приложений Office 365 на панели доступа пользователя

**Тип.** Новая функция  
**Категория службы:** мои приложения  
**Возможности продукта:** Единый вход

Теперь вы можете лучше управлять тем, как приложения Office 365 отображаются на панелях доступа пользователя, с помощью нового пользовательского параметра. Он позволяет уменьшить количество приложений, которые отображаются на панелях доступа пользователей, если вы предпочитаете, чтобы приложения Office отображались только на портале Office. Этот параметр находится в разделе **параметров пользователя** и называется **Пользователи могут видеть приложения Office 365 только на портале Office 365**.

Дополнительные сведения см. в статье [Скрытие приложения в пользовательском интерфейсе Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Простой вход в приложения, поддерживающие единый вход с помощью пароля, непосредственно из URL-адреса приложения 

**Тип.** Новая функция  
**Категория службы:** мои приложения  
**Возможности продукта:** Единый вход

Расширение браузера "Мои приложения" теперь доступно с помощью удобного инструмента, который дает возможность выполнять единый вход в "Мои приложения" через ярлык в вашем браузере. После установки появится значок в виде вафли в браузере, который предоставляет быстрый доступ к приложениям. Теперь пользователи могут воспользоваться следующими преимуществами:

- Выполнять прямой вход в приложения с единым входом на основе пароля на странице входа в приложение.
- Запускать любое приложение с помощью функции быстрого поиска.
- Использовать ярлыки для недавно использовавшихся приложений из расширения.
- Расширение доступно для браузеров Microsoft Edge, Chrome и Firefox.
 
Дополнительные сведения см. в разделе [Расширение защищенного входа на страницу "Мои приложения"](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Интерфейс администрирования Azure AD на классическом портале Azure больше не поддерживается

**Тип.** Не рекомендуется   
**Категория службы.** Azure AD  
**Возможности продукта.** Каталог

Начиная с 8 января 2018 года интерфейс администрирования Azure AD на классическом портале Azure больше не поддерживается. Это произошло в связи с прекращением поддержки самого классического портала Azure. В будущем следует использовать [Центр администрирования Azure AD](https://aad.portal.azure.com) для задач администрирования Azure AD, выполняемых с помощью портала.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>Начиная с 8 января 2018 года прекращена поддержка веб-портала.

**Тип.** Не рекомендуется  
**Категория службы.** Azure AD  
**Возможности продукта.** Каталог
 
Начиная с 8 января 2018 года прекращена поддержка веб-портала PhoneFactor. Этот портал использовался для администрирования сервера MFA, но эти функции были перенесены на портал Azure по адресу portal.azure.com. 

Конфигурация MFA размещена в **Azure Active Directory \> MFA Server** (Сервер MFA)
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Неподдерживаемые отчеты Azure AD

**Тип.** Не рекомендуется  
**Категория службы.** Отчеты  
**Возможности продукта.** Управление жизненным циклом удостоверений  


С выходом общедоступной версии новой консоли администрирования Azure Active Directory и новых API-интерфейсов, доступных для отчетов о действиях и безопасности, API-интерфейсы отчетов в конечной точке /reports были удалены 31 декабря 2017 года.

**Доступные возможности**

В рамках перехода на новую консоль администратора мы создали 2 новых программных интерфейса для извлечения журналов действий Azure AD. В новом наборе программных интерфейсов расширены функции фильтрации, сортировки, аудита и действий входа. Доступ к данным, ранее доступным в отчетах по безопасности, теперь можно получить с помощью API событий риска защиты удостоверений в Microsoft Graph.

Дополнительные сведения можно найти в разделе 

- [Приступая к работе с API отчетов Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Начало работы с защитой идентификации Azure Active Directory и Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Декабрь 2017 г.

### <a name="terms-of-use-in-the-access-panel"></a>Условия использования на панели доступа

**Тип.** Новая функция  
**Категория службы.** Условия использования  
**Возможности продукта.** Соответствие нормативным требованиям
 
Теперь можно перейти к панели доступа и просмотреть условия использования, которые были приняты ранее.

Выполните следующие действия.

1. Перейдите на [портал MyApps](https://myapps.microsoft.com) и войдите в систему.

2. В правом верхнем углу щелкните свое имя, а затем в списке выберите **Профиль**. 

3. В **профиле** выберите **Ознакомьтесь с условиями использования**. 

4. Здесь можно просмотреть принятые вами условия использования. 

Дополнительные сведения см. в статье [Функция "Условия использования Azure Active Directory" (предварительная версия)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Новый вход в систему Azure AD

**Тип.** Новая функция  
**Категория службы.** Azure AD  
**Возможности продукта.** Аутентификация пользователей
 
Пользовательские интерфейсы системы идентификации пользователей Azure AD и Майкрософт были перепроектированы таким образом, чтобы они имели единый внешний вид. Кроме того, страница входа в Azure AD сначала собирает имя пользователя, а затем учетные данные на втором экране.

Дополнительные сведения см. в записи блога [The new Azure AD Signin Experience is now in Public Preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/) (Новый интерфейс входа в Azure AD теперь находится в общедоступной предварительной версии).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Меньше запросов для входа. Новая функция "оставаться в системе" для входа в Azure AD

**Тип.** Новая функция  
**Категория службы.** Azure AD  
**Возможности продукта.** Аутентификация пользователей
 
Флажок **Оставаться в системе** на странице входа в Azure AD был заменен новым запросом, который появляется после прохождения проверки подлинности. 

Если на такой запрос ответить **Да**, служба предоставит постоянный токен обновления. То же самое происходило при установке флажка **Оставаться в системе** в старой версии. Для федеративных клиентов этот запрос отображается после выполнения проверки подлинности в службе федерации.

Дополнительные сведения см. в разделе [Fewer sign-in prompts: The new "keep me signed in" experience for Azure AD is in preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/) (Меньше запросов для входа. Новая функция "оставаться в системе" для входа в Azure AD в предварительной версии) 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Добавление конфигурации для требования развертывания условий использования перед их принятием

**Тип.** Новая функция  
**Категория службы.** Условия использования  
**Возможности продукта.** Соответствие нормативным требованиям
 
Функция для администраторов, позволяющая требовать развертывания условий использования перед их принятием пользователем.

**Включите** или **отключите** параметр "Требовать, чтобы пользователи развернули условия использования". Если выбран вариант **Вкл.**, чтобы принять соглашение, пользователям сначала нужно просмотреть условия.

Дополнительные сведения см. в статье [Функция "Условия использования Azure Active Directory" (предварительная версия)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Активация только соответствующих назначений ролей

**Тип.** Новая функция  
**Категория службы:** Управление привилегированными пользователями (PIM)  
**Возможности продукта:** Управление привилегированными пользователями (PIM)
 
Вы можете использовать ограниченную активацию для активации подходящих назначений ролей ресурсов Azure с меньшей автономией, чем исходные настройки по умолчанию. Предположим, вы назначены владельцем подписки в своем клиенте. С помощью ограниченной активации вы можете активировать роль владельца для максимум пяти ресурсов, содержащихся в рамках подписки (например, группы ресурсов и виртуальные машины). Ограничение активации может снизить вероятность выполнения нежелательных изменений в критических ресурсах Azure.

Дополнительные сведения см. в статье [Что такое Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Новые федеративные приложения в коллекции приложений Azure AD

**Тип.** Новая функция  
**Категория службы.** Приложения Enterprise  
**Возможности продукта.** Интеграция сторонних продуктов

В декабре 2017 года мы добавили эти новые приложения с поддержкой федерации в нашу коллекцию приложений:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD integration](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO for Bamboo by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO for Bitbucket by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD Integration.

Дополнительные сведения об этих приложениях см. в статье [Интеграция приложений SaaS с Azure Active Directory](https://aka.ms/appstutorial).

Дополнительные сведения о том, как добавить приложение в коллекцию приложений Azure AD см. в [этой статье](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Рабочий процесс утверждения ролей каталога Azure AD

**Тип.** Измененная функция  
**Категория службы.** Управление привилегированными пользователями (PIM)  
**Возможности продукта:** Управление привилегированными пользователями (PIM)
 
Рабочий процесс утверждения ролей каталога Azure AD доступен в общедоступной версии.

С помощью рабочего процесса утверждения администраторы привилегированной роли могут требовать от кандидатов на участников роли запрашивать активацию привилегированной роли, прежде чем они смогут ее использовать. Многим пользователям и группам могут быть делегированы полномочия на утверждение. Кандидаты на участников роли получат уведомления, когда они будут утверждены и их роль будет активирована.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Сквозная аутентификация. Поддержка Skype для бизнеса

**Тип.** Измененная функция  
**Категория службы:** аутентификация (вход)  
**Возможности продукта.** Аутентификация пользователей

Сквозная проверка подлинности теперь поддерживает вход пользователей в клиентские приложения Skype для бизнеса, которые поддерживают современную проверку подлинности, включая сетевые и гибридные топологии. 

Дополнительные сведения см. в статье [Топологии Skype для бизнеса, поддерживаемые современной проверкой подлинности](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Обновления Azure AD Privileged Identity Management для Azure RBAC (предварительная версия)

**Тип.** Измененная функция  
**Категория службы.** Управление привилегированными пользователями (PIM)  
**Возможности продукта:** Управление привилегированными пользователями (PIM)
 
Теперь, когда вышло обновление общедоступной предварительной версии Azure AD Privileged Identity Management (PIM) для управления доступом на основе ролей (RBAC) Azure, вы можете:

* Использовать Just Enough Administration (JEA).
* Требовать утверждения для активации ролей ресурсов.
* Планировать активацию ролей в будущем, которая требует одобрения как для ролей Azure AD, так и для ролей Azure RBAC.
 
Дополнительные сведения см. в статье [PIM для ресурсов Azure (предварительная версия)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>Ноябрь 2017 г.
 
### <a name="access-control-service-retirement"></a>Прекращение поддержки службы контроля доступа

**Тип.** план изменений  
**Категория службы.** Служба контроля доступа  
**Возможности продукта.** Служба контроля доступа 

Использование службы контроля доступа Azure Active Directory (также называется просто службой контроля доступа) будет прекращено в конце 2018 г. Дополнительные сведения, включая подробное расписание и общее руководство по переходу, будут предоставлены в ближайшие недели. Вы можете оставить на этой странице комментарии с любыми вопросами, касающимися службы контроля доступа, и член нашей команды ответит вам.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Предоставление доступа через браузер к управляемому браузеру Intune 

**Тип.** план изменений  
**Категория службы:** Условный доступ  
**Возможности продукта:** Безопасность и защита удостоверений

Вы можете предоставлять доступ через браузер к Office 365 и другим облачным приложениям, подключенным к Azure AD, используя Intune Managed Browser в качестве утвержденного приложения. 

Теперь можно настраивать следующее условие для условного доступа на основе приложений.

**Клиентские приложения.** "Обзор"

**Каков эффект изменения?**

В настоящее время при использовании этого условия доступ заблокирован. Если доступна предварительная версия, для всех операций доступа потребуется приложение управляемого браузера. 

Ищите сведения об этой возможности в предстоящих записях в блогах и заметках о выпуске. 

Дополнительные сведения см. в статье [Условный доступ в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Новые утвержденные клиентские приложения для условного доступа на основе приложения Azure AD

**Тип:** план изменений  
**Категория службы:** Условный доступ  
**Возможности продукта:** безопасность и защита идентификации

Следующие приложения добавлены в список [утвержденных клиентских приложений](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/);
- Microsoft StaffHub

Дополнительные сведения можно найти в разделе 

- [Требование утвержденного клиентского приложения](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Условный доступ на основе приложений Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Поддержка условий использования для нескольких языков

**Тип.** Новая функция    
**Категория службы.** Условия использования  
**Возможности продукта.** Соответствие нормативным требованиям

Теперь администраторы могут создавать новые условия использования, содержащие несколько документов в формате PDF. Вы можете отметить эти документы соответствующими языками. Пользователям будут показаны PDF-файлы на том языке, который соответствует их предпочтениям. Если документа на их языке нет, отображается язык по умолчанию.

---
 
### <a name="real-time-password-writeback-client-status"></a>Состояние обратной записи паролей клиента в реальном времени

**Тип.** Новая функция  
**Категория службы.** Самостоятельный сброс пароля  
**Возможности продукта.** Аутентификация пользователей

Теперь вы можете просмотреть состояние обратной записи паролей клиента в локальной среде. Этот параметр доступен в разделе **Интеграция с локальной средой** на странице [Сброс пароля](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset). 

Если возникли проблемы с подключением к клиенту обратной записи в локальной среде, появится сообщение об ошибке, содержащее следующую информацию:

- сведения о том, почему не удается подключиться к клиенту обратной записи в локальной среде;
- ссылка на документацию, которая помогает устранить проблему. 

Дополнительные сведения см. в разделе [Интеграция с локальной средой](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Условный доступ на основе приложений Azure AD 
 
**Тип.** Новая функция  
**Категория службы.** Azure AD  
**Возможности продукта.** Безопасность и защита удостоверений

Теперь с помощью [условного доступа на основе приложений Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) вы можете предоставить доступ к Office 365 и другим облачным приложениям, подключенным к Azure AD, [утвержденным клиентским приложениям](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement), поддерживающим политики защиты приложений Intune. Политики защиты приложений Intune используются для настройки и защиты данных компании в этих клиентских приложениях.

Объединив политики условного доступа [на основе приложения](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) и [на основе устройства](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications), вы сможете защитить данные корпоративных и личных устройств.

Следующие условия и элементы управления теперь доступны для использования с условным доступом на основе приложения:

**Условие поддерживаемой платформы**

- iOS
- Android

**Условие клиентских приложений**

- Мобильные приложения и настольные клиенты

**Контроль доступа**

- Требование утвержденного клиентского приложения

Дополнительные сведения см. в статье [Условный доступ на основе приложений Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Управление устройствами Azure AD на портале Azure

**Тип.** Новая функция  
**Категория службы.** Регистрация устройств и управление ими  
**Возможности продукта.** Безопасность и защита удостоверений

Теперь все устройства, подключенные к Azure AD, и действия, связанные с устройствами, размещены в одном месте. На портале Azure доступен новый интерфейс администрирования для управления удостоверениями и параметрами устройств. В этом выпуске вы можете:

- просматривать все устройства, доступные для условного доступа в Azure AD;
- просматривать свойства, включая гибридные устройства, присоединенные к Azure AD;
- находить ключи BitLocker для устройств, присоединенных к Azure AD, чтобы управлять устройствами с помощью Intune и т. д.;
- управлять параметрами, связанными с устройствами Azure AD.

Дополнительные сведения см. в статье [Управление устройствами с помощью портала Azure](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Поддержка macOS в качестве платформы устройства для условного доступа Azure AD 

**Тип.** Новая функция    
**Категория службы.** Условный доступ  
**Возможности продукта:** Безопасность и защита удостоверений 

Теперь вы можете включить (или исключить) macOS в качестве платформы устройств в политике условного доступа Azure AD. Добавив macOS в качестве поддерживаемой платформы устройства, вы можете:

- **Регистрировать устройства macOS и управлять ими в Intune.** Подобно другим платформам, таким как iOS и Android, приложение корпоративного портала доступно для единой регистрации устройств macOS. Вы можете использовать новое приложение корпоративного портала для macOS, чтобы зарегистрировать устройство в Intune и Azure AD.
- **Гарантировать соблюдение устройствами macOS политик соответствия вашей организации, определенных в Intune.** В Intune на портале Azure вы можете настроить соответствующие политики для устройств macOS. 
- **Предоставлять доступ к приложениям Azure AD только совместимым устройствам macOS.** При создании политики условного доступа macOS доступен как отдельный вариант платформы устройства. Теперь можно создавать политики условного доступа для macOS для целевого приложения, настроенного в Azure.

Дополнительные сведения можно найти в разделе 

- [Создание политики соответствия устройств для устройств macOS (предварительная версия) при помощи Intune](https://aka.ms/macoscompliancepolicy)
- [Условный доступ в Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Расширение NPS для Многофакторной идентификации Azure 

**Тип.** Новая функция    
**Категория службы.**  Многофакторная Идентификация  
**Возможности продукта.** Аутентификация пользователей

Расширение сервера политики сети для Многофакторной идентификации Azure добавляет в инфраструктуру проверки подлинности возможности Многофакторной идентификации на основе облака с использованием имеющихся серверов. Расширение NPS позволяет добавить телефонный звонок, текстовое сообщение или запрос на подтверждение в мобильном приложении в существующий процесс аутентификации. Можно обойтись без установки, настройки и поддержания новых серверов. 

Это расширение было создано для организаций, которым необходимо защитить VPN-подключения, не развертывая сервер Многофакторной идентификации Azure. Расширение NPS выполняет функции адаптера между RADIUS и облачной службой Azure MFA, предоставляя второй фактор проверки подлинности для федеративных или синхронизированных пользователей.

Дополнительные сведения см. в статье [Интеграция существующей инфраструктуры NPS с Многофакторной идентификацией Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Восстановление или окончательное удаление удаленных пользователей

**Тип.** Новая функция    
**Категория службы.** Управление пользователями  
**Возможности продукта.** Каталог 

Теперь в центре администрирования Azure AD вы можете:

- Восстановить удаленного пользователя. 
- Окончательно удалить пользователя.

**Для этого:**

1. В центре администрирования Azure AD в разделе **Управление** выберите [Все пользователи](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All). 

2. В списке **Показать** выберите **Недавно удаленные пользователи**. 

3. Выберите одного или нескольких недавно удаленных пользователей, а затем восстановите их или удалите без возможности восстановления.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Новые утвержденные клиентские приложения для условного доступа на основе приложения Azure AD
 
**Тип.** Измененная функция  
**Категория службы.** Условный доступ  
**Возможности продукта:** Безопасность и защита удостоверений

Следующие приложения добавлены в список [утвержденных клиентских приложений](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Планировщик (Майкрософт);
- Azure Information Protection 

Дополнительные сведения можно найти в разделе 

- [Требование утвержденного клиентского приложения](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Условный доступ на основе приложений Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Возможность выбора между элементами управления в политике условного доступа 

**Тип.** Измененная функция    
**Категория службы.** Условный доступ  
**Возможности продукта:** Безопасность и защита удостоверений
 
Теперь доступна возможность выбора (необходимо выбрать один из элементов управления) элементов управления условного доступа. Эта функция позволяет создавать политики со значением "или" между элементами управления доступом. Например, эта функция позволяет создать политику, требующую от пользователя входа в систему с помощью Многофакторной идентификации или использования соответствующего устройства.

Дополнительные сведения см. в статье [Элементы управления условным доступом в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).
 
---

### <a name="aggregation-of-real-time-risk-events"></a>Агрегирование событий риска в реальном времени

**Тип.** Измененная функция    
**Категория службы.** Защита идентификации  
**Возможности продукта.** Безопасность и защита удостоверений

В службе защиты идентификации Azure AD все события риска в реальном времени, возникшие на одном IP-адресе в определенный день, теперь агрегируются для каждого типа событий риска. Это изменение ограничивает объем событий риска, не влияя на безопасность пользователя.

Функция базового обнаружения в реальном времени работает каждый раз, когда пользователь выполняет вход. Если вы настроили для политики безопасности входа Многофакторную идентификацию или блокирование доступа, она все равно будет срабатывать при каждом подозрительном входе.
 
---
 
## <a name="october-2017"></a>Октябрь 2017 г.

### <a name="deprecate-azure-ad-reports"></a>Неподдерживаемые отчеты Azure AD

**Тип.** план изменений  
**Категория службы:** Отчеты  
**Возможности продукта.** Управление жизненным циклом удостоверений  

Портал Azure предлагает:

- новую консоль администрирования Azure AD;
- новые программные интерфейсы (API) для отчетов по действиям и безопасности.
 
Благодаря этим новым возможностям API отчетов на конечной точке /reports были выведены из эксплуатации 10 декабря 2017 г. 

---

### <a name="automatic-sign-in-field-detection"></a>Автоматическое определение полей входа в систему

**Тип.** исправление   
**Категория службы.** мои приложения  
**Возможности продукта.** Единый вход  

Azure AD поддерживает автоматическое определение поля входа для приложений, преобразовывающих для просмотра поле имени пользователя и пароля HTML. Эти шаги описаны в разделе [Определение полей входа в систему для приложения вручную](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). Эту возможность можно найти путем добавления приложения *не из коллекции* на страницу **корпоративных приложений** на [портале Azure](https://aad.portal.azure.com). Кроме того, можно настроить режим **единого входа** в это новое приложение с помощью **единого входа по паролю** путем введения URL-адреса и сохранения страницы.
 
Из-за сбоя в службе эта функциональность была временно отключена. Сейчас эта проблема устранена и автоматическое обнаружение поля входа снова доступно.

---

### <a name="new-multi-factor-authentication-features"></a>Новые возможности Многофакторной идентификации

**Тип.** Новая функция  
**Категория службы.** Многофакторная Идентификация  
**Возможности продукта.** Безопасность и защита удостоверений  

Многофакторная проверка подлинности (MFA) является важной частью процесса защиты организации. Чтобы сделать учетные данные более адаптивными, а работу — простой, были добавлены следующие возможности: 

- Влияние интеграции многофакторной аутентификации на отчет о входе в Azure AD, включая программный доступ к результатам многофакторной аутентификации.
- Более тесная интеграция конфигурации многофакторной проверки подлинности с конфигурацией Azure AD на портале Azure.

В общедоступной предварительной версии управление и составление отчетов многофакторной проверки подлинности являются неотъемлемой частью настройки основных компонентов Azure AD. Теперь вы можете управлять функциональными возможностями портала управления многофакторной проверки подлинности в Azure AD.

Дополнительные сведения см. в статье [Справочник по созданию отчетов многофакторной проверки подлинности на портале Azure](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 

---

### <a name="terms-of-use"></a>Условия использования

**Тип.** Новая функция  
**Категория службы.** Условия использования  
**Возможности продукта.** Соответствие нормативным требованиям  

Вы можете использовать условия использования Azure AD для предоставления пользователям такой информации, как соответствующие заявления об отказе для юридических требований и требования к соответствию.

Условия использования Azure AD можно применять в следующих сценариях:

- общие условия использования для всех пользователей в вашей организации;
- определенные условия использования на основе атрибутов пользователя (например, для медсестер и врачей или местных и международных сотрудников, что выполняется с помощью динамических групп);
- определенные условия использования для доступа к важным для бизнеса приложениям, например Salesforce.

Дополнительные сведения см. в статье [Функция "Условия использования Azure Active Directory" (предварительная версия)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="enhancements-to-privileged-identity-management"></a>Улучшение управления привилегированными пользователями

**Тип.** Новая функция  
**Категория службы:** Управление привилегированными пользователями (PIM)  
**Возможности продукта:** Управление привилегированными пользователями (PIM)  

С помощью службы Azure AD Privileged Identity Management (PIM) можно администрировать, контролировать и отслеживать доступ к ресурсам Azure (предварительная версия) в пределах организации:

- Подписки
- Группы ресурсов
- Виртуальные машины 

Все ресурсы на портале Azure, который использует функции Azure RBAC, могут воспользоваться всеми преимуществами управления безопасностью и жизненным циклом, которые может предложить Azure AD Privileged Identity Management.

Дополнительные сведения см. в статье [PIM для ресурсов Azure (предварительная версия)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Проверки доступа

**Тип.** Новая функция  
**Категория службы.** Проверки доступа  
**Возможности продукта.** Соответствие нормативным требованиям  

Проверки доступа (предварительная версия) позволяют организациям эффективно управлять членством в группе и доступом к корпоративным приложениям. 

- Проверки доступа позволяют повторно оценивать, действительно ли гостевым пользователям нужен доступ к приложениям и членство в группах. Сведения, предоставляемые проверками доступа, позволяют рецензентам решать, нужен ли гостевым пользователям постоянный доступ.
- С их помощью можно повторно подтвердить доступ сотрудников к приложениям и их членство в группах.

Можно собирать элементы управления проверки доступа в программы, соответствующие вашей организации, чтобы отслеживать доступ для приложений, чувствительных к риску или нарушениям соответствия.

Дополнительные сведения см. в статье [Проверки доступа Azure AD (предварительная версия)](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Скрытие сторонних приложений в разделе "Мои приложения" и от средства запуска приложения Office 365

**Тип.** Новая функция  
**Категория службы:** мои приложения  
**Возможности продукта.** Единый вход  

Теперь можно эффективнее управлять приложениями, которые отображаются на пользовательском портале, с помощью нового свойства **Скрыть приложение**. Это помогает в случаях, когда плитки приложений отображаются для серверных служб или плиток-дубликатов и перегружают средства запуска пользовательских приложений. Переключатель находится в разделе **Свойства** в стороннем приложении. Он называется **Visible to user?** (Видимый для пользователя?). Скрыть приложение можно также программным способом с помощью PowerShell. 

Дополнительные сведения см. в статье [Скрытие приложения в пользовательском интерфейсе Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Доступные возможности**

 В рамках перехода на новую консоль администратора доступны два новых программных интерфейса для извлечения доступных журналов действий Azure AD. В новом наборе программных интерфейсов расширены функции фильтрации, сортировки, аудита и действий входа. Доступ к данным, ранее доступным в отчетах по безопасности, теперь можно получить с помощью API событий риска защиты удостоверений в Microsoft Graph.


## <a name="september-2017"></a>Сентябрь 2017 г.

### <a name="hotfix-for-identity-manager"></a>Исправления для Identity Manager

**Тип.** Измененная функция  
**Категория службы.** Identity Manager  
**Возможности продукта.** Управление жизненным циклом удостоверений  

Накопительный пакет исправлений (сборка 4.4.1642.0) для Identity Manager 2016 с пакетом обновления 1 стал доступен с 25 сентября 2017 года. Этот накопительный пакет исправлений:

- устраняет проблемы и добавляет улучшения;
- является накопительным обновлением, которое заменяет все обновления сборки Identity Manager 2016 с пакетом обновления 1 от Identity Manager 2016 версии 4.4.1459.0; 
- требует наличия сборки Identity Manager 2016 версии 4.4.1302.0. 

Дополнительные сведения см. в статье [Доступен накопительный пакет исправлений (сборка 4.4.1642.0) для пакета обновления 1 для Microsoft Identity Manager 2016](https://support.microsoft.com/help/4021562). 

---
