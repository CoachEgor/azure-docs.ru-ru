---
title: Динамика CRM Лазурный рынок
description: Настройка управления интересами в Dynamics CRM.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dsindona
ms.openlocfilehash: 524ae203a311d538431205bf8c6498de45aeb4d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280309"
---
# <a name="configure-lead-management-for-dynamics-crm-online"></a>Настройка управления потенциальными клиентами в Dynamics CRM Online

В этой статье описано, как настроить Dynamics CRM Online для работы с потенциальными клиентами.

## <a name="prerequisites"></a>Предварительные требования

Для выполнения шагов, описанных в этой статье, пользователю требуются следующие разрешения:
- Чтобы установить решение, вы должны быть администратором своего экземпляра Dynamics CRM Online.
- Чтобы создать учетную запись службы для управления потенциальными клиентами, вы должны быть администратором клиента.

<a name="install-the-solution"></a>Установка решения
--------------------

1.  Скачайте решение [Microsoft Marketplace Lead Writer](https://mpsapiprodwus.blob.core.windows.net/documentation/MicrosoftMarketplacesLeadIntegrationSolution_1_0_0_0_target_CRM_6.1_managed.zip) и сохраните его локально.

2.  Откройте Dynamics CRM Online и перейдите в раздел "Параметры".
    >[!NOTE]
    >Если вы не видите параметры, показанные на снимке экрана ниже, значит у вас нет нужных разрешений.
 
       ![Раздел параметров в Dynamics](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline1.png)

3.  Нажмите **Импорт** и выберите решение, скачанное на шаге 1.
 
    ![Кнопка импорта в Dynamics](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline2.png)

4.  Завершите установку решения.

## <a name="configure-user-permissions"></a>Настройка разрешений пользователя

Чтобы записывать данные о потенциальных клиентах в экземпляр Dynamics CRM, вы должны предоставить нам учетную запись службы и настроить для нее разрешения.

Чтобы создать учетную запись службы и назначить ей разрешения, выполните описанные ниже действия. Можно использовать **Azure Active Directory** или **Office 365**.

### <a name="azure-active-directory"></a>Azure Active Directory

Мы рекомендуем этот вариант, поскольку при его использовании вам не нужно будет обновлять имя пользователя и пароль, чтобы продолжать получать данные о потенциальных клиентах. Чтобы использовать Azure Active Directory, нужно предоставить идентификатор приложения, ключ приложения и идентификатор каталога, полученные из приложения Active Directory.

Чтобы настроить Azure Active Directory для Dynamics CRM, сделайте следующее:

1.  Войдите на [портал Azure](https://portal.azure.com/) и выберите службу Azure Active Directory.

2.  Выберите **Свойства,** а затем скопировать **идентификатор каталога**. Это идентификация учетной записи клиента, которую вам нужно использовать в облачном портале партнеров.

    ![Получение идентификатора каталога](./media/cloud-partner-portal-lead-management-instructions-dynamics/directoryid.png)

3.  Выберите **регистрации приложений,** а затем выберите **регистрацию новых приложений.**
4.  Введите имя приложения.
5.  В поле "Тип" выберите **Веб-приложение или API**.
6.  Укажите URL-адрес. Это поле не требуется для работы с потенциальными клиентами, но необходимо для создания приложения.
7. Выберите **Создать**.
8.  Теперь, когда ваше приложение зарегистрировано, выберите **Свойства,** а затем выберите **копию Ид приложения.** Эту информацию о связи вы будете использовать на портале Cloud Partner.
9.  В разделе "Свойства" настройте приложение как мультитенантное и нажмите **Сохранить**.

10. Нажмите **Ключи** и создайте новый ключ, указав для него *Срок действия неограничен*. Нажмите **Сохранить**, чтобы создать ключ. 
11. В меню "Ключи" скопируйте **значение ключа**. Сохраните это значение, потому что оно понадобится на портале Cloud Partner.
    
    ![Регистрация ключа для Dynamics](./media/cloud-partner-portal-lead-management-instructions-dynamics/registerkeys.png)
    
12. Нажмите **Необходимые разрешения**, а затем — **Добавить**. 
13. Укажите **Dynamics CRM Online** в качестве нового API и проверьте разрешение для *Access CRM Online as organization users* (Доступ к CRM Online в качестве пользователей организации).

14. В Dynamics CRM перейдите в раздел "Пользователи" и в раскрывающемся меню "Включенные пользователи" выберите **Пользователи приложения**.
    
    ![Пользователи приложения](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuserfirst.PNG)

15. Нажмите **Создать**, чтобы создать пользователя. Щелкните раскрывающийся список **User: Application User** (Пользователь: пользователь приложения).
    
    ![Добавление нового пользователя приложения](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuser.PNG)

16. В разделе **Новый пользователь** укажите имя и электронный адрес, которые будут использоваться для этого подключения. Вставьте **идентификатор приложения**, созданный на портале Azure.

     ![Настройка нового пользователя](./media/cloud-partner-portal-lead-management-instructions-dynamics/leadgencreateuser.PNG)

17. Перейдите в раздел "Параметры безопасности" этой статьи и завершите настройку подключения для этого пользователя.

### <a name="office-365"></a>Office 365

Если вы не хотите использовать Active Directory Azure, вы можете зарегистрировать нового пользователя в *центре админы Microsoft 365.* Вам нужно будет обновлять свое имя пользователя и пароль каждые 90 дней, чтобы продолжать получать данные о потенциальных клиентах.

Чтобы настроить Office 365 для Dynamics CRM, сделайте следующее:

1. Выполните вход в [центр администрирования Microsoft 365](https://admin.microsoft.com).

2. Выберите плитку **Admin.**

    ![Офис Онлайн Администратор](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline3.png)

3. Нажмите **Добавить пользователя**.

    ![Добавление пользователей](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline4.png)

4. Создайте пользователя для службы модуля записи интересов. Настройте следующие параметры:

    -   Укажите пароль и снимите флажок "Потребовать смену пароля при первом входе пользователя".
    -   Укажите для пользователя роль "Пользователь (без прав администратора)".
    -   Выберите лицензию на продукт, показанную на снимке экрана ниже. За выбранную лицензию будет начисляться плата. Это решение также работает с лицензией Dynamics CRM Online ценовой категории "Базовый".
    
    ![Настройка разрешений пользователя и лицензии](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline5.png)

## <a name="security-settings"></a>Параметры безопасности

На последнем шаге нужно разрешить созданному пользователю записывать данные о потенциальных клиентах.

1.  Войдите в Dynamics CRM Online.
2.  В разделе **Параметры** выберите **Безопасность**.
    
    ![Параметры безопасности](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline6.png)

3.  Выберите в списке **Разрешения пользователей** созданного пользователя и щелкните **Операции с ролями пользователей**. Установите флажок **Microsoft Marketplace Lead Writer**, чтобы назначить роль.

    ![Назначение роли пользователя](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline7.png)\

    >[!NOTE]
    >Эту роль создало импортированное решение, и ей предоставлены разрешения только на запись данных о потенциальных клиентах и на отслеживание версии решения для обеспечения совместимости.

4.  В разделе "Безопасность" нажмите **Роли безопасности** и найдите роль Microsoft Marketplace Lead Writer.
    
    ![Настройка безопасности для Lead Writer](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline10.jpg)\

5. Выберите вкладку **Core Records.** Включить создание/чтение/запись для пользовательского интерфейса Entity.

    ![Предоставление прав на создание, чтение и запись для пользователя](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline11.jpg)\

## <a name="wrap-up"></a>Заключение

Завершите настройку Dynamics CRM для управления потенциальными клиентами, добавив сведения о созданной учетной записи на портале Cloud Partner. Пример:

-   **Azure Active Directory** - **идентификатор приложения** (например, *23456052-aaaa-bbbb-8662-1234df56788f*), **идентификатор каталога** (например, *12345678-8af1-4asf-1234-12234d01db47*) и **ключ приложения** (например, *1234ABCDEDFRZ/G/FdY0aUABCEDcqhbLn/ST122345nBc=*).
-   **Office 365** - **URL-адрес** (например, *https://contoso.crm4.dynamics.com*), **имя пользователя** (например, *contoso\@contoso.onmicrosoft.com*) и **пароль** (например, *P\@ssw0rd*).
