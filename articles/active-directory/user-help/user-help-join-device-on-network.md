---
title: Подключения рабочего устройства к сети организации в Azure Active Directory | Документация Майкрософт
description: Описание способов присоединения рабочего устройства к сети организации.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfd38e775ba418fbe31cafa8df5f37b45c3b0643
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60474539"
---
# <a name="join-your-work-device-to-your-organizations-network"></a>Присоединения рабочего устройства к сети организации
Присоедините свое рабочее устройство Windows 10 к сети вашей организации, чтобы вы могли получать доступ к потенциально ограниченным ресурсам.

## <a name="what-happens-when-you-join-your-device"></a>Действия, которые происходят при присоединении устройства
Во время присоединения устройства Windows 10 к сети организации будут выполнены следующие действия.

- Windows регистрирует устройство к сети вашей организации, позволяя доступ к ресурсам с помощью личной учетной записью. Затем Windows присоединяет зарегистрированное устройство к сети. Таким образом, вы можете использовать имя пользователя и пароль организации для входа в систему и доступа к ограниченным ресурсам.

- В зависимости от выбранных организацией параметров может появиться запрос на настройку двухэтапной проверки подлинности с применением [Многофакторной идентификации](multi-factor-authentication-end-user-first-time.md) или метода на основе [сведений для защиты](user-help-security-info-overview.md).

- В зависимости от выбранных организацией параметров вы можете быть автоматически зарегистрированы в службе управления мобильными устройствами, например в Microsoft Intune. Дополнительные сведения о регистрации в Microsoft Intune см. в статье [Регистрация устройства в Intune](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all).

- Затем произойдет автоматический вход в систему с использованием учетной записи организации.

## <a name="to-join-a-brand-new-windows-10-device"></a>Присоединение нового устройства Windows 10
Если ваше устройство новое и еще не настроено, вы можете пройти процесс Windows Out of Box Experience (OOBE), чтобы присоединить ваше устройство к сети.

1. Запустите новое устройство и начните процесс OOBE.

2. На странице **Войдите с помощью учетной записи Майкрософт** введите свой адрес электронной почты рабочей или учебной учетной записи.

    ![Окно входа в систему с адресом электронной почты](./media/user-help-join-device-on-network/join-device-oobe-signin.png)

3. В окне **Введите пароль** введите свой пароль.

    ![Окно ввода пароля](./media/user-help-join-device-on-network/join-device-oobe-password.png)

4. На мобильном устройстве подтвердите устройство, чтобы оно могло получить доступ к вашей учетной записи. 

    ![Окно уведомления на мобильном устройстве](./media/user-help-join-device-on-network/join-device-oobe-mobile.png)

5. Завершите процесс OOBE, включая настройку параметров конфиденциальности и Windows Hello (при необходимости).

    Теперь ваше устройство присоединено к сети вашей организации.

## <a name="to-make-sure-youre-joined"></a>Проверка состояния присоединения
Чтобы убедиться, что присоединение прошло успешно, нужно взглянуть на параметры.

1. Откройте **Параметры** и выберите **Учетные записи**.

    ![Пункт "Учетные записи" в окне параметров](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. Выберите **доступ к рабочей или учебной**и убедитесь, что вы видите текст, который говорит нечто вроде, **подключен к  *\<Ваша_Организация >* Azure AD**.

    ![Окно доступа к учетной записи места работы или учебного заведения с подключенной учетной записью Contoso](./media/user-help-join-device-on-network/join-device-oobe-verify.png)


## <a name="to-join-an-already-configured-windows-10-device"></a>Для присоединения уже настроенного устройства Windows 10
Если у вас есть устройство, которое уже настроено, вы можете выполнить следующие шаги, чтобы присоединить его к сети.

1. Откройте **Параметры** и выберите **Учетные записи**.

2. Выберите **Доступ к учетной записи места работы или учебного заведения**, а затем щелкните **Подключить**.

    ![Пункт "Доступ к учетной записи места работы или учебного заведения" и ссылка на подключение](./media/user-help-join-device-on-network/join-device-access-work-school-connect.png)

3. В окне **Setup a work or school account** (Настройка рабочей или учебной учетной записи) щелкните **Join this device to Azure Active Directory** (Присоединить это устройство к Azure Active Directory).

    ![Страница "Set up a work or school account" (Настройка рабочей или учебной учетной записи)](./media/user-help-join-device-on-network/join-device-setup-join-aad.png)

4. На странице **Let's get you signed in** (Выполнение входа в систему) введите адрес электронной почты (например, alain@contoso.com), а затем выберите **Далее**.

    ![Окно "Let's get you signed in" (Выполнение входа в систему)](./media/user-help-join-device-on-network/join-device-setup-get-signed-in.png)

5. В окне **Ввод пароля** введите пароль и щелкните **Вход**.

    ![Ввод пароля](./media/user-help-join-device-on-network/join-device-setup-password.png)

6. На мобильном устройстве подтвердите устройство, чтобы оно могло получить доступ к вашей учетной записи. 

    ![Окно уведомления на мобильном устройстве](./media/user-help-join-device-on-network/join-device-setup-mobile.png)

7. В окне **Убедитесь в том, что это ваша организация** проверьте сведения, а затем выберите **Присоединить**.

    ![Окно "Убедитесь в том, что это ваша организация"](./media/user-help-join-device-on-network/join-device-setup-confirm.png)

8. В окне **You're all set** (Все готово) щелкните **Готово**.

    ![Окно "You're all set" (Все готово)](./media/user-help-join-device-on-network/join-device-setup-finish.png)

## <a name="to-make-sure-youre-joined"></a>Проверка состояния присоединения
Чтобы убедиться, что присоединение прошло успешно, нужно взглянуть на параметры.

1. Откройте **Параметры** и выберите **Учетные записи**.

    ![Пункт "Учетные записи" в окне параметров](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. Выберите **доступ к рабочей или учебной**и убедитесь, что вы видите текст, который говорит нечто вроде, **подключен к  *\<Ваша_Организация >* Azure AD**.

    ![Окно доступа к учетной записи места работы или учебного заведения с подключенной учетной записью Contoso](./media/user-help-join-device-on-network/join-device-setup-verify.png)

## <a name="next-steps"></a>Дальнейшие действия
Когда вы присоедините свое устройство к сети вашей организации, вы сможете получать доступ ко всем своим ресурсам, используя сведения рабочей или учебной учетной записи.

- Если ваша организация хочет, чтобы вы зарегистрировали личное устройство, например, ваш телефон, перейдите к статье [Регистрация персонального устройства в сети организации](user-help-register-device-on-network.md).

