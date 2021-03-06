---
title: Устранение защищенных оповещений LDAP в доменных службах Azure AD | Документация Майкрософт
description: Узнайте, как устранять и устранять распространенные предупреждения с помощью защищенного протокола LDAP для доменных служб Azure Active Directory.
services: active-directory-ds
author: justinha
manager: daveba
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/09/2020
ms.author: justinha
ms.openlocfilehash: 15c1f3a1731edf7b45061646d43688b4aacc6104
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2020
ms.locfileid: "96620313"
---
# <a name="known-issues-secure-ldap-alerts-in-azure-active-directory-domain-services"></a>Известные проблемы: защищенный протокол LDAP оповещений в доменных службах Azure Active Directory

Приложения и службы, использующие протокол LDAP для взаимодействия с доменными службами Azure Active Directory (Azure AD DS), можно [настроить для использования защищенного протокола LDAP](tutorial-configure-ldaps.md). Чтобы безопасный протокол LDAP работал правильно, должен быть открыт соответствующий сертификат и необходимые сетевые порты.

Эта статья поможет вам понять и устранить распространенные предупреждения с защищенным доступом LDAP в Azure AD DS.

## <a name="aadds101-secure-ldap-network-configuration"></a>AADDS101: защищенный протокол LDAP конфигурация сети

### <a name="alert-message"></a>Текст оповещения

*Защищенный протокол LDAP через Интернет включены для управляемого домена. Однако доступ к порту 636 не блокируется с помощью группы безопасности сети. Это может предоставить учетным записям пользователей в управляемом домене атаки методом подбора пароля.*

### <a name="resolution"></a>Разрешение

При включении защищенного протокола LDAP рекомендуется создать дополнительные правила, ограничивающие входящие подключения LDAP к определенным IP-адресам. Эти правила защищают управляемый домен от атак методом подбора. Чтобы обновить группу безопасности сети, чтобы ограничить доступ TCP-порта 636 для защищенного протокола LDAP, выполните следующие действия.

1. В портал Azure найдите и выберите **группы безопасности сети**.
1. Выберите группу безопасности сети, связанную с управляемым доменом, например *AADDS-contoso.com-NSG*, а затем выберите **правила безопасности для входящего трафика** .
1. Выберите **+ Добавить** , чтобы создать правило для TCP-порта 636. При необходимости выберите **Дополнительно** в окне, чтобы создать правило.
1. В раскрывающемся меню **источника** выберите *IP-адреса* . Введите исходные IP-адреса, которые требуется предоставить для доступа к защищенному трафику LDAP.
1. Выберите *любой* в качестве **назначения**, а затем введите *636* для **диапазонов целевых портов**.
1. Задайте **протокол** *TCP* и **действие** , которое необходимо *Разрешить*.
1. Укажите приоритет правила, а затем введите имя, например *рестриктлдапс*.
1. Когда все будет готово, нажмите кнопку **Добавить** , чтобы создать правило.

Работоспособность управляемого домена автоматически обновляется в течение двух часов и удаляет оповещение.

> [!TIP]
> TCP-порт 636 — это не единственное правило, необходимое для бесперебойной работы Azure AD DS. Дополнительные сведения см. в разделе [группы безопасности сети Azure AD DS и требуемые порты](network-considerations.md#network-security-groups-and-required-ports).

## <a name="aadds502-secure-ldap-certificate-expiring"></a>AADDS502. Срок действия сертификата защищенного протокола LDAP истекает

### <a name="alert-message"></a>Текст оповещения

*Срок действия защищенного сертификата LDAP для управляемого домена истекает [дата]].*

### <a name="resolution"></a>Разрешение

Создайте заменяющий сертификат защищенного протокола LDAP, выполнив действия по [созданию сертификата для защищенного протокола LDAP](tutorial-configure-ldaps.md#create-a-certificate-for-secure-ldap). Примените сертификат замены к Azure AD DS и распространите сертификат на все клиенты, подключающиеся по защищенному протоколу LDAP.

## <a name="next-steps"></a>Дальнейшие действия

Если проблемы сохраняются, [откройте запрос на поддержку в Azure][azure-support] для получения дополнительной помощи в устранении неполадок.

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
