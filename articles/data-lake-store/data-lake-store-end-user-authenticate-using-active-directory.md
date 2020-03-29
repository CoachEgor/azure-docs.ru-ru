---
title: Проверка подлинности пользователей в Azure Data Lake Storage 1-го поколения с помощью Azure Active Directory | Документы Майкрософт
description: Узнайте, как реализовать проверку подлинности пользователей в Azure Data Lake Storage 1-го поколения с помощью Azure Active Directory
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 4c2b774c304e46f9fc68f3beaf64218e614ecad1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "66234060"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Проверка подлинности пользователей в Azure Data Lake Storage 1-го поколения с помощью Azure Active Directory
> [!div class="op_single_selector"]
> * [Проверка подлинности конечных пользователей](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Аутентификация от службы к обслуживанию](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Storage 1-го поколения (ADLS 1-го поколения) использует Azure Active Directory для аутентификации. Прежде чем создать приложение, которое работает с Data Lake Storage 1-го поколения или Azure Data Lake Analytics, необходимо решить, как следует проверять подлинность с помощью Azure Active Directory (Azure AD). Доступно два основных варианта:

* Аутентификация пользователей (в этой статье)
* аутентификация между службами (выберите этот параметр в раскрывающемся списке выше).

Оба этих варианта позволят приложению получить маркер OAuth 2.0, который вкладывается в каждый запрос к Data Lake Storage 1-го поколения или Azure Data Lake Analytics.

Из этой статьи вы узнаете, как создать **собственное приложение Azure AD для аутентификации пользователей**. Инструкции по настройке приложения Azure AD для проверки подлинности между службами см. в статье [Проверка подлинности между службами в Data Lake Storage 1-го поколения с помощью Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Предварительные требования
* Подписка Azure. См. страницу [бесплатной пробной версии Azure](https://azure.microsoft.com/pricing/free-trial/).

* Идентификатор подписки. Его можно получить на портале Azure. Например, он доступен в колонке учетной записи Data Lake Storage 1-го поколения.
  
    ![Получение идентификатора подписки](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Доменное имя Azure AD. Его можно получить, наведя указатель мыши на правый верхний угол страницы портала Azure. На приведенном ниже снимке экрана указано имя домена **contoso.onmicrosoft.com**, а идентификатор GUID в скобках — это идентификатор клиента. 
  
    ![Получение домена AAD](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

* Идентификатор клиента Azure. Инструкции о том, как получить идентификатор клиента, см. в разделе [Получение идентификатора клиента](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).

## <a name="end-user-authentication"></a>Проверка подлинности конечных пользователей
Этот механизм проверки подлинности является рекомендуемым подходом, если вы хотите, чтобы конечный пользователь входил в приложение через Azure AD. Приложение будет иметь тот же уровень доступа к ресурсам Azure, что и вошедший пользователь. Пользователю необходимо периодически вводить свои учетные данные, чтобы у вашего приложения оставался доступ.

Результатом входиного ввека конечного пользователя является то, что вашему приложению предоставляется токен доступа и токен обновления. Маркер доступа вкладывается в каждый запрос к Data Lake Storage 1-го поколения и Data Lake Analytics и по умолчанию действителен в течение одного часа. Маркер обновления может использоваться для получения нового маркера доступа. По умолчанию он действителен до двух недель. Для входинивой системы конечным пользователем можно использовать два различных подхода.

### <a name="using-the-oauth-20-pop-up"></a>Использование всплывающего окна OAuth 2.0
Приложение может активировать всплывающее окно авторизации OAuth 2.0, в котором пользователь может ввести свои учетные данные. При необходимости это всплывающее окно также работает с процессом двухфакторной проверки подлинности (2FA) Azure AD. 

> [!NOTE]
> Этот метод еще не поддерживается в библиотеке аутентификации Azure AD (ADAL) для Java или Python.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Непосредственная передача учетных данных пользователя
Приложение могут напрямую предоставлять Azure AD учетные данные пользователя. Этот метод работает только для учетных записей на основе идентификаторов организации. Он не подходит для личных учетных записей пользователей или учетных записей на основе Live ID, включая те, которые заканчиваются на @outlook.com или @live.com. Кроме того, данный метод несовместим с учетными записями пользователей, для которых требуется двухфакторная проверка подлинности (2FA) Azure AD.

### <a name="what-do-i-need-for-this-approach"></a>Что мне нужно для применения этого подхода?
* Доменное имя Azure AD (см. предварительные требования в этой статье).
* Идентификатор клиента Azure AD. (см. предварительные требования в этой статье).
* **Собственное приложение** Azure AD.
* Идентификатор приложения для собственного приложения Azure AD.
* URI перенаправления для собственного приложения Azure AD.
* Настроенные делегированные разрешения.


## <a name="step-1-create-an-active-directory-native-application"></a>Шаг 1. Создание собственного приложения Active Directory

Создайте и настройте собственное приложение Azure AD для проверки подлинности пользователей в Data Lake Storage 1-го поколения с помощью Azure Active Directory. Инструкции см. в разделе [Создание приложения Azure AD](../active-directory/develop/howto-create-service-principal-portal.md).

При выполнении инструкций по ссылке обязательно выберите тип приложения **Native** (Собственный), как показано на снимке экрана ниже.

![Создание веб-приложения](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Создать родное приложение")

## <a name="step-2-get-application-id-and-redirect-uri"></a>Шаг 2. Получение идентификатора приложения и URI перенаправления

Ознакомьтесь с разделом [Получение идентификатора приложения и ключа проверки подлинности](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in), чтобы извлечь идентификатор приложения.

Чтобы получить URI перенаправления, выполните следующие действия.

1. На портале Azure выберите **Azure Active Directory**, щелкните **Регистрация приложений**, затем найдите и щелкните созданное собственное приложение Azure AD.

2. В колонке **Параметры** приложения щелкните **URI перенаправления**.

    ![Получение URI перенаправления](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Скопируйте отображенное значение.


## <a name="step-3-set-permissions"></a>Шаг 3. Установка разрешений

1. На портале Azure выберите **Azure Active Directory**, щелкните **Регистрация приложений**, затем найдите и щелкните созданное собственное приложение Azure AD.

2. В колонке **Параметры** приложения щелкните **Необходимые разрешения** и нажмите кнопку **Добавить**.

    ![Идентификатор клиента](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. В колонке **Добавить доступ через API** щелкните **Выбор API** > **Azure Data Lake** > **Выбрать**.

    ![Идентификатор клиента](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  В колонке **Добавить доступ через API** щелкните **Выбор разрешений**, установите флажок, чтобы предоставить **полный доступ к Data Lake Store**, и нажмите кнопку **Выбрать**.

    ![Идентификатор клиента](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Нажмите кнопку **Готово**.

5. Повторите последние два шага, чтобы также предоставить разрешения для **API управления службами Microsoft Azure**.
   
## <a name="next-steps"></a>Дальнейшие действия
В этой статье вы создали нативное приложение Azure AD и собрали необходимую информацию в клиентских приложениях, которые вы авториспользуете с помощью .NET SDK, Java SDK, REST API и т.д. Теперь вы можете перейти к следующим статьям, в которым говорится о том, как использовать веб-приложение Azure AD для предварительной проверки подлинности с помощью Data Lake Storage Gen1, а затем выполнения других операций в магазине.

* [Проверка подлинности пользователей в Data Lake Storage 1-го поколения с помощью Java SDK](data-lake-store-end-user-authenticate-java-sdk.md)
* [Проверка подлинности пользователей в Data Lake Storage 1-го поколения с помощью .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md)
* [Проверка подлинности пользователей в Data Lake Storage 1-го поколения с помощью Python](data-lake-store-end-user-authenticate-python.md)
* [Проверка подлинности пользователей в Data Lake Storage 1-го поколения с помощью REST API](data-lake-store-end-user-authenticate-rest-api.md)

