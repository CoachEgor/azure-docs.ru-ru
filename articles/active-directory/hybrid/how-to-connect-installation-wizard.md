---
title: Повторное выполнение мастера установки Azure AD Connect | Документация Майкрософт
description: Объясняется, как работает мастер установки при повторном запуске.
keywords: При повторном запуске мастера установки Azure AD Connect он позволяет настроить параметры обслуживания.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ff2caae7cb387f4f0d88cf059d01ad28861b9ad
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60348456"
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Синхронизация Azure AD Connect: Повторный запуск мастера установки
При первом запуске мастера установки Azure AD Connect выполняется пошаговая настройка установки. При повторном запуске мастера установки предлагается настроить параметры обслуживания.

Мастер установки можно найти в меню "Пуск" с именем **Azure AD Connect**.

![Меню "Пуск"](./media/how-to-connect-installation-wizard/startmenu.png)

При запуске мастера установки появится страница со следующими параметрами:

![Страница со списком дополнительных задач](./media/how-to-connect-installation-wizard/additionaltasks.png)

Если вы установили службы AD FS с Azure AD Connect, вам будет доступно еще больше возможностей. Дополнительные возможности служб AD FS описаны в разделе [Управление AD FS](how-to-connect-fed-management.md#manage-ad-fs).

Чтобы продолжить, выберите одну из задач и нажмите кнопку **Далее** .

> [!IMPORTANT]
> Когда открыт мастер установки, приостанавливаются все операции в модуле синхронизации. Убедитесь, что сразу после внесения изменений в конфигурацию вы завершили работу мастера установки.
>
>

## <a name="view-current-configuration"></a>Просмотр текущей конфигурации
Этот параметр позволяет быстро просмотреть текущие настройки параметров.

![Страница со списком всех параметров и их состоянием](./media/how-to-connect-installation-wizard/viewconfig.png)

Чтобы вернуться, нажмите кнопку **Назад** . Если выбрать **Выход**, окно мастера установки закроется.

## <a name="customize-synchronization-options"></a>Настройка параметров синхронизации
Этот параметр используется для изменения конфигурации синхронизации. Вы увидите набор параметров из пути установки пользовательской конфигурации. Они отобразятся, даже если изначально использовались параметры экспресс-установки.

* [Добавить дополнительные каталоги](how-to-connect-install-custom.md#connect-your-directories). Сведения об удалении каталога см. [здесь](how-to-connect-sync-service-manager-ui-connectors.md#delete).
* [Изменить фильтрацию доменов и подразделений](how-to-connect-install-custom.md#domain-and-ou-filtering).
* Удалить групповую фильтрацию.
* [Изменить дополнительные возможности](how-to-connect-install-custom.md#optional-features).

Другие параметры из начальной установки нельзя изменить, поэтому они недоступны. Доступны следующие параметры:

* Изменить атрибут, используемый для userPrincipalName и sourceAnchor.
* Изменить метод соединения для объектов из другого леса.
* Включить фильтрацию на основе группы.

## <a name="refresh-directory-schema"></a>Обновление схемы каталога
Этот параметр используется при изменении схемы в одном из локальных лесов AD DS. Например, когда выполнена установка Exchange или обновление до схемы Windows Server 2012 с объектами устройства. В этом случае необходимо настроить Azure AD Connect на повторное чтение схемы из AD DS и обновление кэша. Это действие также повторно создает правила синхронизации. Если добавить, например, схему Exchange, то правила синхронизации для Exchange добавятся в конфигурацию.

При выборе этого параметра отобразятся все каталоги в конфигурации. Можно оставить значение по умолчанию и обновить все леса или отменить выбор некоторых из них.

![Страница со списком всех каталогов в среде](./media/how-to-connect-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Настройка промежуточного режима
Этот параметр позволяет включить и отключить на сервере промежуточный режим. Дополнительные сведения о промежуточном режиме и его использовании см. [здесь](how-to-connect-sync-staging-server.md).

Рядом с параметром будет показано, включен или отключен промежуточный режим в данный момент:   
![Параметр, который также отображает текущее состояние промежуточного режима](./media/how-to-connect-installation-wizard/stagingmodecurrentstate.png)

Для изменения состояния выберите этот параметр и установите или снимите флажок.  
![Параметр, который также отображает текущее состояние промежуточного режима](./media/how-to-connect-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Изменение параметров входа пользователя
Этот параметр позволяет изменить метод входа пользователя, указав один из следующих: синхронизация хэша паролей, сквозная аутентификация или федерация. Нельзя изменить на значение **Не настраивать**.

Дополнительные сведения о входе пользователя см. [здесь](plan-connect-user-signin.md#changing-the-user-sign-in-method).

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о модели конфигурации, используемой в синхронизации Azure AD Connect, см. в статье о [принципах декларативной подготовки](concept-azure-ad-connect-sync-declarative-provisioning.md).

**Обзорные статьи**

* [Синхронизация Azure AD Connect: общие сведений о синхронизации и ее настройка](how-to-connect-sync-whatis.md)
* [Интеграция локальных удостоверений с Azure Active Directory](whatis-hybrid-identity.md)
