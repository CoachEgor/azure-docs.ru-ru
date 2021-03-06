---
title: Устранение проблем с проверкой подлинности и авторизацией в концентраторах событий Azure
description: В этой статье содержатся сведения об устранении неполадок проверки подлинности и авторизации в концентраторах событий Azure.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 368fd8efda1b828f99bc41da0743768989c1a601
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92329616"
---
# <a name="troubleshoot-authentication-and-authorization-issues---azure-event-hubs"></a>Устранение проблем с проверкой подлинности и авторизацией в концентраторах событий Azure
В статье [Устранение неполадок подключения](troubleshooting-guide.md) приведены советы по устранению проблем с подключением к концентраторам событий Azure. В этой статье содержатся советы и рекомендации по устранению неполадок проверки подлинности и авторизации в концентраторах событий Azure. 

## <a name="if-you-are-using-azure-active-directory"></a>Если используется Azure Active Directory
Если вы используете Azure Active Directory (Azure AD) для проверки подлинности и авторизации в концентраторах событий Azure, убедитесь, что удостоверение, обращающееся к концентратору событий, является членом правой **роли Azure** в правой **области ресурсов** (группа потребителей, концентратор событий, пространство имен, Группа ресурсов или подписка).

### <a name="azure-roles"></a>Роли Azure
- [Владелец данных концентраторов событий Azure](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-owner) для полного доступа к ресурсам концентраторов событий.
- [Отправитель данных концентраторов событий Azure](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-receiver) для доступа к отправке.
- [Приемник данных концентраторов событий Azure](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-sender) для получения доступа.

Встроенные роли реестра схемы см. в разделе [роли реестра схемы](schema-registry-overview.md#azure-role-based-access-control).

### <a name="resource-scopes"></a>Области ресурсов
- **Группа потребителей**. в этой области Назначение ролей применяется только к этой сущности. Сейчас портал Azure не поддерживает назначение роли Azure субъекту безопасности на этом уровне. 
- **Концентратор событий**. Назначение ролей применяется к сущности концентратора событий и группе потребителей под ней.
- **Пространство имен**: назначение ролей охватывает всю топологию концентраторов событий в пространстве имен и связанную с ней группу потребителей.
- **Группа ресурсов**. Назначение ролей применяется ко всем ресурсам концентраторов событий в группе ресурсов.
- **Подписка**. Назначение ролей применяется ко всем ресурсам концентраторов событий во всех группах ресурсов в подписке.

Дополнительные сведения см. в следующих статьях:

- [Проверка подлинности приложения с Azure Active Directory для доступа к ресурсам концентраторов событий](authenticate-application.md)
- [Авторизация доступа к ресурсам концентраторов событий с помощью Azure Active Directory](authorize-access-azure-active-directory.md)

## <a name="if-you-are-using-shared-access-signatures-sas"></a>Если используются подписанные URL-адрес (SAS)
Если вы используете [SAS](authenticate-shared-access-signature.md), выполните следующие действия. 

- Убедитесь, что используется правильный ключ SAS. В противном случае используйте правильный ключ SAS.
- Убедитесь, что ключ имеет правильные разрешения (отправка, получение или управление). Если нет, используйте ключ с нужным разрешением. 
- Проверьте, истек ли срок действия ключа. Рекомендуется обновить SAS до истечения срока действия. Если между клиентом и узлами службы концентраторов событий есть неравномерное распределение времени, срок действия маркера проверки подлинности может истечь до того, как клиент выдает его. Текущая реализация. отклонение по часам в течение 5 минут, то есть клиент обновляет маркер за 5 минут до истечения срока его действия. Поэтому, если отклонение часов превышает 5 минут, клиент может наблюдать за периодическими ошибками проверки подлинности.
- Если для параметра **время начала SAS** установлено значение **сейчас**, то в течение первых нескольких минут могут возникать временные сбои из-за неравномерного распределения времени (разница в текущем времени на разных компьютерах). Задайте время начала не менее 15 минут в прошлом или не устанавливайте его вообще. То же самое относится и к времени окончания срока действия. 

Дополнительные сведения см. в следующих статьях: 

- [Проверка подлинности с помощью подписанных URL-адресов (SAS)](authenticate-shared-access-signature.md). 
- [Авторизация доступа к ресурсам концентраторов событий с помощью подписанных URL](authorize-access-shared-access-signature.md)

## <a name="next-steps"></a>Дальнейшие действия

См. следующие статьи:

* [Устранение неполадок с подключением](troubleshooting-guide.md)
