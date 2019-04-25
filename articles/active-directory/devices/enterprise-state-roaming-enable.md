---
title: Включение службы Enterprise State Roaming в Azure Active Directory | Документация Майкрософт
description: Часто задаваемые вопросы о параметрах Enterprise State Roaming для устройств Windows. Служба Enterprise State Roaming представляет собой единое решение для всех устройств Windows и сокращает процесс настройки нового устройства.
services: active-directory
keywords: Enterprise State Roaming, облака Windows, включение службы Enterprise State Roaming
documentationcenter: ''
author: tanning
manager: daveba
editor: curtand
ms.subservice: devices
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97d0be1dfd77035a3522ef8a6b3f4ac93ae20741
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60353873"
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Включение службы Enterprise State Roaming в Azure Active Directory
Enterprise State Roaming доступна для любой организации с лицензий Azure AD Premium или Enterprise Mobility + Security (EMS). Дополнительные сведения о том, как получить подписку Azure AD, см. на [странице продукта Azure Active Directory](https://azure.microsoft.com/services/active-directory).

Если включить службу Enterprise State Roaming, ваша организация автоматически получит бесплатную ограниченную лицензию на службу защиты Azure Rights Management, которая включена в Azure Information Protection. В бесплатной подписке можно выполнять только шифрование и расшифровку параметров предприятия и данных приложений, которые синхронизируются в Enterprise State Roaming. Чтобы использовать все возможности службы Azure Rights Management, у вас должна быть [платная подписка](https://azure.microsoft.com/pricing/details/information-protection/).

## <a name="to-enable-enterprise-state-roaming"></a>Включение Enterprise State Roaming

1. Войдите в [Центр администрирования Azure AD](https://aad.portal.azure.com/)

1. Последовательно выберите **Azure Active Directory** &gt; **Устройства** &gt; **Enterprise State Roaming**.

1. Включите параметр **Пользователи могут выполнять синхронизацию параметров и данных разных устройств**. Дополнительные сведения см. в статье [о настройке параметров устройств](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).
  
   ![Изображение с настройкой параметра "Пользователи могут выполнять синхронизацию параметров и данных разных устройств"](./media/enterprise-state-roaming-enable/device-settings.png)
  
Службу Enterprise State Roaming могут использовать устройства Windows 10, которые прошли аутентификацию с удостоверением Azure AD. В качестве основного идентификатора для входа устройств, присоединенных к Azure AD, используется удостоверение Azure AD, поэтому дополнительная настройка не требуется. Если используются устройства с локальной версией службы Active Directory, ИТ-администратору следует изучить инструкции по [настройке устройств с гибридным присоединением к Azure Active Directory](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual-steps). 

## <a name="data-storage"></a>Хранилище данных
Данные Enterprise State Roaming размещаются в одном или нескольких [регионах Azure](https://azure.microsoft.com/regions/), которые лучше всего сочетаются со страной и (или) регионом, выбранным в экземпляре Azure Active Directory. Данные Enterprise State Roaming разделяются на три основные географические региона: Северная Америка, EMEA (Европа, Ближний Восток и Африка) и APAC (Азиатско-тихоокеанский регион). Данные Enterprise State Roaming для клиента находятся локально в географическом регионе и не реплицируются между регионами.  Например: 

Выбранная страна (или регион) | Где размещаются данные
---------------------|-------------------------
Страна Европы, Ближнего Востока или Африки (например, Франция или Замбия) | Один или несколько регионов Azure в Европе 
Страна в Северной Америке, например США или Канада | Один или несколько регионов Azure в США
Страна Азиатско-Тихоокеанского региона, например Австралия или Новая Зеландия | Один или несколько регионов Azure в Азии
Регионы Южной Америки и Антарктики | Один или несколько регионов Azure в США

Региональные настройки (страна или регион) задаются в процессе создания каталога Azure AD и не могут быть изменены в дальнейшем. Чтобы узнать, в каком расположении хранятся ваши данные, отправьте запрос в службу [поддержки Azure](https://azure.microsoft.com/support/options/).

## <a name="view-per-user-device-sync-status"></a>Просмотр состояния синхронизации устройств по пользователям
Выполните следующие действия, чтобы просмотреть отчет о состоянии синхронизации устройств по пользователям.

1. Войдите в [Центр администрирования Azure AD](https://aad.portal.azure.com/)

1. Выберите **Azure Active Directory** &gt; **Пользователи** &gt; **Все пользователи**.

1. Выберите пользователя, а затем элемент **Устройства**.

1. Чтобы увидеть состояние синхронизации, в пункте **Показать** выберите вариант **Devices syncing settings and app data** (Параметры синхронизации устройств и данные приложений).
  
   ![Изображение с настройками данных о синхронизации устройств](./media/enterprise-state-roaming-enable/sync-status.png)
  
1. Если для этого пользователя зарегистрированы устройства, для которых выполняется синхронизация, вы увидите их список, как показано ниже.
  
   ![Изображение таблицы с информацией о синхронизации устройств](./media/enterprise-state-roaming-enable/device-status-row.png)

## <a name="data-retention"></a>Хранение данных
Данные, синхронизированные с Microsoft Cloud с помощью службы Enterprise State Roaming, сохраняются в Azure, пока не будут удалены вручную или пока не станут устаревшими. 

### <a name="explicit-deletion"></a>Явное удаление
Данные удаляются, когда администратор Azure удаляет пользователя или каталог, или другим явным способом запрашивает удаление данных.

* **Удаление пользователя**. В период от 90 до 180 дней после удаления пользователя из Azure AD, удаляются все перемещаемые данные его учетной записи. 
* **Удаление каталога**. Весь каталог в Azure AD можно удалить всего в несколько щелчков. Все данные о параметрах этого каталога удаляются в период от 90 до 180 дней. 
* **Удаление по запросу**. Чтобы вручную удалить данные определенного пользователя или его данные параметров, администратору Azure AD необходимо отправить запрос в [службу поддержки Azure](https://azure.microsoft.com/support/). 

### <a name="stale-data-deletion"></a>Удаление устаревших данных
Устаревшими считаются данные, доступ к которым не осуществлялся в течение года ("период хранения"). Они могут быть удалены из Microsoft Cloud. Срок хранения может измениться, но не будет меньше, чем 90 дней. Такими данными могут выступать параметры Windows или приложений либо все параметры пользователя. Например: 

* Если к коллекции определенных параметров не осуществляется доступ (например, приложение удалено с устройства или группа параметров отключена на всех устройствах пользователя), по истечении периода хранения эта коллекция считается устаревшей и может быть удалена. 
* Если пользователь отключил синхронизацию параметров на всех устройствах, доступ к ним не будет осуществляться. Таким образом, как только период хранения истечет, все данные параметров пользователя станут устаревшими и могут быть удалены. 
* Если администратор каталога Azure AD отключит службу Enterprise State Roaming для всего каталога, синхронизация параметров всех пользователей в этом каталоге будет остановлена. По истечении периода хранения все данные параметров этих пользователей станут устаревшими и могут быть удалены. 

### <a name="deleted-data-recovery"></a>Восстановление удаленных данных
Политика хранения удаленных данных не настраивается. После окончательного удаления данных восстановить их невозможно. Но данные о параметрах удаляются только из Microsoft Cloud, а не с устройства пользователя. Если это устройство повторно подключится к службе Enterprise State Roaming, все параметры снова будут синхронизированы и сохранены в Microsoft Cloud.

## <a name="next-steps"></a>Дальнейшие действия

* [Обзор службы Enterprise State Roaming](enterprise-state-roaming-overview.md)
* [Часто задаваемые вопросы о перемещении параметров и данных](enterprise-state-roaming-faqs.md)
* [Group Policy and MDM settings for settings sync (Параметры групповой политики и управления мобильными устройствами)](enterprise-state-roaming-group-policy-settings.md)
* [Справочник по перемещаемым параметрам в Windows 10](enterprise-state-roaming-windows-settings-reference.md)
* [Устранение неполадок](enterprise-state-roaming-troubleshooting.md)
