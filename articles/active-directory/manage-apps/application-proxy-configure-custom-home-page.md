---
title: Настройка пользовательской домашней страницы для опубликованных приложений с помощью прокси приложения Azure AD | Документация Майкрософт
description: Основные сведения о соединителях прокси приложения Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/08/2017
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0880ad2ab02fad574f5204741b0fa03e4ef0338
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60443299"
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Настройка пользовательской домашней страницы для опубликованных приложений с помощью прокси приложения Azure AD

В этой статье описывается настройка приложений для направления пользователей на пользовательскую домашнюю страницу. При публикации приложения с помощью прокси приложения задается внутренний URL-адрес, но иногда он не является той страницей, на которую пользователи должны попадать в первую очередь. Задайте пользовательскую домашнюю страницу, чтобы пользователи переходили на нужную страницу при обращении к приложениям. Пользователи будут видеть эту домашнюю страницу при обращении к приложению из панели доступа Azure Active Directory или средства запуска приложений Office 365.

Пользователь, открывающий это приложение, по умолчанию перенаправляется на корневой URL-адрес домена опубликованного приложения. Обычно целевой страницей считается URL-адрес домашней страницы приложения. Используйте модуль Azure AD PowerShell, чтобы определить URL-адреса пользовательской домашней страницы, когда пользователи должны переходить на определенную страницу в приложении. 

Ниже приведен один из примеров, подтверждающих необходимость настройки пользовательской домашней страницы для компании.
- В корпоративной сети, пользователи могут `https://ExpenseApp/login/login.aspx` на вход и доступ к вашему приложению.
- Так как у вас есть другие ресурсы, например изображения, которые требуются прокси приложения для доступа к на верхнем уровне структуры папок, публикации приложения с помощью `https://ExpenseApp` как внутренний URL-адрес.
- Внешний URL-адрес по умолчанию `https://ExpenseApp-contoso.msappproxy.net`, которому не переходят на страницу входа в систему пользователей.  
- Задайте `https://ExpenseApp-contoso.msappproxy.net/login/login.aspx` как URL-адрес домашней страницы. 

>[!NOTE]
>Когда вы предоставляете пользователям доступ к опубликованным приложениям, эти приложения отображаются на [панели доступа Azure AD](../user-help/active-directory-saas-access-panel-introduction.md) и в [средстве запуска приложений Office 365](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).

## <a name="before-you-start"></a>Перед началом работы

Прежде чем указывать URL-адрес домашней страницы, учтите следующие требования:

* Указанный путь должен содержать URL-адрес поддомена корневого домена.

  Если URL-адрес корневого домена, например, https://apps.contoso.com/app1/, то настраиваемый URL-адрес домашней страницы должен начинаться с https://apps.contoso.com/app1/.

* Когда вы вносите изменения в опубликованное приложение, URL-адрес домашней страницы может вернуться к значению по умолчанию. Поэтому при дальнейшем обновлении приложения следует повторно проверить URL-адрес домашней страницы и изменить его при необходимости.

## <a name="change-the-home-page-in-the-azure-portal"></a>Изменение домашней страницы на портале Azure

1. Войдите на [портал Azure](https://portal.azure.com) с использованием учетной записи администратора.
2. Перейдите в раздел **Azure Active Directory** > **Регистрация приложений** и выберите приложение в списке. 
3. В параметрах выберите **Свойства**.
4. Укажите новый путь в поле **URL-адрес домашней страницы**. 

   ![Указание нового URL-адреса домашней страницы](./media/application-proxy-configure-custom-home-page/homepage.png)

5. Нажмите кнопку **Сохранить**.

## <a name="change-the-home-page-with-powershell"></a>Изменение домашней страницы с помощью PowerShell

### <a name="install-the-azure-ad-powershell-module"></a>Установка модуля Azure AD PowerShell

Прежде чем настраивать URL-адрес пользовательской домашней страницы с помощью PowerShell, установите модуль Azure AD PowerShell. Этот пакет можно скачать из [коллекции PowerShell](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), в которой используется конечная точка API Graph. 

Для установки пакета выполните следующие действия.

1. Откройте стандартное окно PowerShell и выполните следующую команду.

    ```powershell
     Install-Module -Name AzureAD
    ```

    Если вы запускаете команду не от имени администратора, используйте параметр `-scope currentuser`.
2. Во время установки выберите ответ **Y** (Да) на предложение установить два пакета с сайта Nuget.org. Требуются оба пакета. 

### <a name="find-the-objectid-of-the-app"></a>Поиск идентификатора ObjectID для приложения

Получите ObjectID приложения и затем найдите приложение по его домашней странице.

1. В том же окне PowerShell импортируйте модуль Azure AD.

    ```powershell
    Import-Module AzureAD
    ```

2. Войдите в модуль Azure AD в качестве администратора клиента.

    ```powershell
    Connect-AzureAD
    ```

3. Найдите приложение по URL-адресу домашней страницы. Этот URL-адрес можно найти на портале, выбрав **Azure Active Directory** > **Корпоративные приложения** > **Все приложения**. В этом примере используется *sharepoint-iddemo*.

    ```powershell
    Get-AzureADApplication | Where-Object { $_.Homepage -like "sharepoint-iddemo" } | Format-List DisplayName, Homepage, ObjectID
    ```

4. Вы должны получить результат, похожий на приведенный ниже. Скопируйте GUID ObjectID для использования в следующем разделе.

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-the-home-page-url"></a>Изменение URL-адреса домашней страницы

Создайте URL-адрес домашней страницы и обновите приложение, указав это значение. Выполните эти команды в том же окне PowerShell. Если вы используете новое окно PowerShell, то повторите вход в модуль Azure AD, выполнив командлет `Connect-AzureAD`. 

1. Убедитесь, что вы нашли правильное приложение, затем замените значение *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* значением ObjectID, которое вы скопировали в предыдущем разделе.

    ```powershell
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

   Убедившись, что это нужное приложение, вы можете изменить для него адрес домашней страницы, как описано далее.

2. Создайте пустой объект приложения, в котором будут храниться ваши изменения. Эта переменная содержит значения, которые необходимо обновить. На этом шаге ничто не создается.

    ```powershell
    $appnew = New-Object "Microsoft.Open.AzureAD.Model.Application"
    ```

3. Задайте нужное значение для URL-адреса домашней страницы. Это значение должно быть путем к поддомену опубликованного приложения. Например, если изменить URL-адрес домашней страницы с `https://sharepoint-iddemo.msappproxy.net/` на `https://sharepoint-iddemo.msappproxy.net/hybrid/`, пользователи приложения перейдут непосредственно на пользовательскую домашнюю страницу.

    ```powershell
    $homepage = "https://sharepoint-iddemo.msappproxy.net/hybrid/"
    ```

4. Внесите изменения с помощью GUID (ObjectID), который был скопирован на шаге 1 "Поиск идентификатора ObjectID для приложения".

    ```powershell
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```

5. Чтобы проверить, внесены ли изменения, перезапустите приложение.

    ```powershell
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Все изменения, которые вы внесли в приложение, могут сбросить URL-адрес домашней страницы. В случае сброса URL-адреса домашней страницы повторите шаги в этом разделе, чтобы снова задать его.

## <a name="next-steps"></a>Дальнейшие действия

- [Настройка удаленного доступа к SharePoint с помощью прокси приложения Azure AD](application-proxy-integrate-with-sharepoint-server.md)
- [Включение прокси приложения на портале Azure](application-proxy-add-on-premises-application.md)