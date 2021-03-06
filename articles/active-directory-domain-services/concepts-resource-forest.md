---
title: Концепции леса ресурсов для доменных служб Azure AD | Документация Майкрософт
description: Узнайте, что такое лес ресурсов в доменных службах Azure Active Directory и какие преимущества получает ваша организация в гибридной среде с ограниченными вариантами проверки подлинности пользователей или вопросами безопасности.
services: active-directory-ds
author: justinha
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2020
ms.author: justinha
ms.openlocfilehash: 79cf408bcb9060c247b97e6a81204c5a5517d384
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2020
ms.locfileid: "96620007"
---
# <a name="resource-forest-concepts-and-features-for-azure-active-directory-domain-services"></a>Основные понятия и функции лесов ресурсов для доменных служб Azure Active Directory

Azure Active Directory доменных служб (Azure AD DS) предоставляет возможность входа для устаревших локальных бизнес-приложений. Пользователи, группы и хэши паролей локальных и облачных пользователей синхронизируются с управляемым доменом Azure AD DS. Эти синхронизированные хэши паролей дают пользователям единый набор учетных данных, которые они могут использовать для локальных AD DS, Microsoft 365 и Azure Active Directory.

Хотя это защищенный метод с дополнительными преимуществами безопасности, некоторые организации не могут синхронизировать эти хэши паролей пользователей с Azure AD или Azure AD DS. Пользователи в организации могут не знать свой пароль, так как используют только проверку подлинности с помощью смарт-карты. Эти ограничения не позволяют некоторым организациям использовать Azure AD DS для переноса локальных классических приложений в Azure методом lift-and-shift.

Для решения этих задач и ограничений можно создать управляемый домен, использующий лес ресурсов. В этой статье объясняется, что собой представляют леса и как они доверяют другим ресурсам, чтобы обеспечить безопасный метод проверки подлинности.

## <a name="what-are-forests"></a>Что такое леса?

*Лес* — это логическая конструкция, используемая доменными службами Active Directory (AD DS) для группирования одного или нескольких *доменов*. Затем домены сохраняют объекты для пользователей или групп и предоставляют службы проверки подлинности.

В домене, управляемом AD DS Azure, лес содержит только один домен. Локальные леса AD DS часто содержат много доменов. В крупных организациях, особенно после слияния и приобретения, у вас может быть несколько локальных лесов, каждый из которых будет содержать несколько доменов.

По умолчанию управляемый домен создается в качестве леса *пользователя* . Лес этого типа синхронизирует все объекты из Azure AD, включая учетные записи пользователей, созданные в локальной среде AD DS. Учетные записи пользователей могут напрямую проходить проверку подлинности в управляемом домене, например для входа в виртуальную машину, присоединенную к домену. Лес пользователей работает, когда хэши паролей можно синхронизировать и пользователи не используют единственный метод входа, например проверку подлинности с помощью смарт-карты.

В лесу *ресурсов* управляемого домена пользователи проходят проверку подлинности одностороннего *доверия* леса из локальной AD DS. При таком подходе пользовательские объекты и хэши паролей не синхронизируются с управляемым доменом. Пользовательские объекты и учетные данные существуют только в локальных AD DS. Такой подход позволяет предприятиям размещать в Azure ресурсы и платформы приложений, которым требуется классическая проверка подлинности, например LDAPS, Kerberos или NTLM, при этом устраняются проблемы проверки подлинности.

Леса ресурсов также предоставляют возможность переноса приложений методом lift-and-shift по одному компоненту за раз. Многие устаревшие локальные приложения являются многоуровневыми, часто используют веб-сервер или внешний интерфейс и множество компонентов, связанных с базами данных. Эти уровни затрудняют перемещение всего приложения в облако за один шаг. С помощью лесов ресурсов вы можете переносить приложение в облако поэтапно, чтобы упростить задачу.

## <a name="what-are-trusts"></a>Что такое отношения доверия?

Организациям, имеющим более одного домена, часто требуется, чтобы у пользователей был доступ к общим ресурсам в другом домене. Для доступа к этим общим ресурсам пользователи из одного домена должны пройти проверку подлинности в другом домене. Чтобы обеспечить эти возможности проверки подлинности и авторизации для клиентов и серверов в разных доменах, между двумя доменами должно быть установлено *доверие*.

Когда отношения доверия установлены, механизмы проверки подлинности для каждого домена доверяют проверки подлинности другого домена. Отношения доверия обеспечивают управляемый доступ к общим ресурсам в домене ресурсов (*доверяющий домен*) путем проверки того, что входящие запросы на проверку подлинности поступают из доверенного центра (*доверенного* домена). Отношения доверия действуют как мосты, которые пропускают только проверенные запросы проверки подлинности между доменами.

Способ передачи запросов на проверку подлинности в рамках отношений доверия зависит от настройки. Отношения доверия можно настроить одним из следующих способов:

* **Одностороннее** — обеспечивает доступ из доверенного домена к ресурсам в доверяющем домене.
* **Двустороннее** — обеспечивает доступ из одного домена к ресурсам второго.

Отношения доверия также можно настроить на обработку дополнительных отношений одним из следующих способов:

* **Нетранзитивные** — доверие существует только между двумя доменами.
* **Транзитивные** — доверие автоматически распространяется на любые другие домены, которым доверяет один из партнеров.

В некоторых случаях отношения доверия автоматически устанавливаются при создании доменов. В других случаях необходимо выбрать тип доверия и явно установить соответствующие связи. Конкретные типы отношений доверия и структура этих отношений доверия зависят от способа организации AD DS каталога и от того, соотносятся ли разные версии Windows к сети.

## <a name="trusts-between-two-forests"></a>Отношения доверия между двумя лесами

Вы можете расширить доверительные отношения доменов в пределах одного леса на другой лес, вручную создав одностороннее или двустороннее доверие лесов. Доверие леса — это транзитивное доверие, существующее только между корневым доменом одного леса и корневым доменом второго.

* Одностороннее доверие лесов позволяет всем пользователям в одном лесу доверять всем доменам в другом.
* Двустороннее доверие между лесами образует транзитивное отношение доверия между всеми доменами в обоих лесах.

Транзитивность доверия между лесами распространяется только на два леса-партнера. Доверие между лесами не распространяется на другие леса, которым доверяет один из партнеров.

![Схема доверия леса между Azure AD DS и локальными AD DS](./media/concepts-resource-forest/resource-forest-trust-relationship.png)

Можно создать различные конфигурации доверия домена и леса в зависимости от структуры AD DS Организации. Azure AD DS поддерживает только односторонние отношения доверия между лесами. В этой конфигурации ресурсы в управляемом домене могут доверять всем доменам в локальном лесу.

## <a name="supporting-technology-for-trusts"></a>Поддержка технологии для отношений доверия

Отношения доверия используют различные службы и функции, такие как DNS, для поиска контроллеров домена в партнерских лесах. Отношения доверия также зависят от протоколов проверки подлинности NTLM и Kerberos и механизмов управления доступом и авторизации на основе Windows, чтобы обеспечить безопасную инфраструктуру связи в AD DS доменах и лесах. Следующие службы и компоненты помогают поддерживать успешные отношения доверия.

### <a name="dns"></a>DNS

AD DS требуется DNS для расположения и именования контроллера домена. Для успешной работы AD DS предоставляется следующая поддержка DNS:

* Служба разрешения имен, которая позволяет сетевым узлам и службам находить контроллеры домена.
* Структура именования, которая позволяет организации отражать организационную структуру в именах доменов службы каталогов.

Обычно развертывается пространство имен домена DNS, отражающее пространство имен домена AD DS. Если перед развертыванием AD DS существует существующее пространство имен DNS, пространство имен DNS обычно секционируется для AD DS, а также создается поддомен DNS и делегирование для корня леса AD DS. Затем добавляются дополнительные доменные имена DNS для каждого AD DS дочернего домена.

DNS также используется для поддержки расположения AD DS контроллеров домена. Зоны DNS заполняются записями ресурсов DNS, которые позволяют узлам и службам сети разрешать обнаружение AD DS контроллеров домена.

### <a name="applications-and-net-logon"></a>Приложения и сетевой вход в систему

Приложения и служба сетевого входа в систему являются компонентами модели распределенного канала безопасности Windows. Приложения, интегрированные с Windows Server и AD DS используют протоколы проверки подлинности для взаимодействия со службой сетевого входа в систему, чтобы можно было установить защищенный путь, который может быть установлен для проверки подлинности.

### <a name="authentication-protocols"></a>Протоколы проверки подлинности

AD DS DCs проверяет подлинность пользователей и приложений с помощью одного из следующих протоколов:

* **Протокол проверки подлинности Kerberos версии 5**
    * Протокол проверки подлинности Kerberos версии 5 по умолчанию используется локальными компьютерами под управлением Windows и поддерживает сторонние операционные системы. Этот протокол указан в RFC 1510 и полностью интегрирован с AD DS, SMB, HTTP и удаленным вызовом процедур (RPC), а также клиентскими и серверными приложениями, использующими эти протоколы.
    * Если используется протокол Kerberos, серверу не нужно обращаться к контроллеру домена. Вместо этого клиент получает билет для сервера, запрашивая его у контроллера домена в домене учетной записи сервера. Затем сервер проверяет билет без консультации с другими центрами.
    * Если какой-либо из компьютеров, участвующий в транзакции, не поддерживает протокол Kerberos версии 5, используется протокол NTLM.

* **Протокол проверки подлинности NTLM**
    * Протокол NTLM — это классический сетевой протокол проверки подлинности, используемый в старых операционных системах. В целях совместимости он используется AD DS доменами для обработки запросов проверки подлинности сети, поступающих из приложений, предназначенных для более ранних клиентов и серверов под управлением Windows, и операционных систем сторонних производителей.
    * Если между клиентом и сервером используется протокол NTLM, сервер должен обратиться к службе проверки подлинности домена на контроллере домена, чтобы проверить учетные данные клиента. Сервер выполняет проверку подлинности клиента, пересылая учетные данные клиента контроллеру домена в домене учетной записи клиента.
    * При соединении доверия между двумя AD DS доменами или лесами запросы на проверку подлинности, созданные с помощью этих протоколов, можно маршрутизировать для предоставления доступа к ресурсам в обоих лесах.

## <a name="authorization-and-access-control"></a>Проверка подлинности и управление доступом

Технологии авторизации и доверия работают совместно, чтобы обеспечить безопасную инфраструктуру связи в AD DS доменах или лесах. Авторизация определяет уровень доступа пользователя к ресурсам в домене. Отношения доверия упрощают междоменную авторизацию пользователей, предоставляя путь для проверки подлинности пользователей в других доменах, чтобы их запросы к общим ресурсам в этих доменах можно было авторизовать.

Когда запрос на проверку подлинности, сделанный в доверяющем домене, проверяется доверенным доменом, он передается целевому ресурсу. Затем целевой ресурс определяет, следует ли авторизовать конкретный запрос, сделанный пользователем, службой или компьютером, в доверенном домене на основе конфигурации контроля доступа.

Отношения доверия предоставляют механизм для проверки запросов проверки подлинности, передаваемых в доверяющий домен. Механизмы управления доступом на компьютере ресурса определяют окончательный уровень доступа, предоставленный запрашивающему объекту в доверенном домене.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о доверии см. в статье [Как отношения доверия между лесами работают в Azure AD DS?][concepts-trust]

Чтобы приступить к созданию управляемого домена с лесом ресурсов, см. статью [Создание и Настройка управляемого домена AD DS Azure][tutorial-create-advanced]. Затем можно [создать исходящее доверие лесов для локального домена][create-forest-trust].

<!-- LINKS - INTERNAL -->
[concepts-trust]: concepts-forest-trust.md
[tutorial-create-advanced]: tutorial-create-instance-advanced.md
[create-forest-trust]: tutorial-create-forest-trust.md
