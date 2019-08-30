---
title: Подходы к миграции пользователей в Azure Active Directory B2C
description: Обсуждаются основные и расширенные понятия о миграции пользователей с помощью API Graph Azure AD, а также при необходимости с помощью Azure AD B2C настраиваемых политик.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/31/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c922799b650de7f921cc0493eb3feb2ad90b9d92
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/30/2019
ms.locfileid: "70183147"
---
# <a name="azure-active-directory-b2c-user-migration"></a>Azure Active Directory B2C Миграция пользователей

При переносе поставщика удостоверений на Azure Active Directory B2C (Azure AD B2C) также может потребоваться миграция учетных записей пользователей. В этой статье описывается процедура переноса имеющихся учетных записей пользователей из любого поставщика удостоверений в Azure AD B2C. Эта статья не предназначена для использования в качестве описательных, но она описывает несколько сценариев. За пригодность каждого подхода отвечает разработчик.

## <a name="user-migration-flows"></a>Потоки миграции пользователей

С Azure AD B2C можно выполнить миграцию пользователей с помощью [API Graph Azure AD][B2C-GraphQuickStart]. Процесс миграции пользователей подразделяется на два потока.

- **Действия, выполняемые перед миграцией.** Этот поток применяется, когда у вас есть свободный доступ к учетным данным пользователя (имя пользователя и пароль) или учетные данные зашифрованы, но их можно расшифровать. Этот процесс включает в себя считывание пользователей старого поставщика удостоверений и создание учетных записей в каталоге Azure AD B2C.

- **Действия, выполняемые перед миграцией, и сброс пароля.** Этот поток применяется, когда пароль пользователя недоступен. Пример:
  - Пароль сохраняется в формате хэша.
  - Пароль хранится в недоступном поставщике удостоверений. Старый поставщик удостоверений проверяет учетные данные пользователя путем вызова веб-службы.

В обоих случаях сначала требуется запустить процесс, выполняемый перед миграцией, считать пользователей из старого поставщика удостоверений, а затем создать учетные записи в каталоге Azure AD B2C. Если у вас нет пароля, учетная запись создается с помощью пароля, созданного случайным образом. Затем предложите пользователям изменить их пароли. Или Azure AD B2C просит пользователя сбросить пароль при первой регистрации.

## <a name="password-policy"></a>Политика паролей

Политика паролей Azure AD B2C (для локальных учетных записей) основана на политике Azure AD. Политики регистрации, входа в систему и сброса пароля Azure AD B2C используют строгую стойкость пароля и не имеют срока действия паролей. Дополнительные сведения см. в статье [Политики и ограничения для паролей в Azure Active Directory][AD-PasswordPolicies].

Если у переносимых учетных записей надежность пароля слабее, чем [высокая надежность пароля, обеспечиваемая Azure AD B2C][AD-PasswordPolicies], можно отключить требование надежного пароля. Чтобы изменить политику паролей по умолчанию, задайте для свойства `passwordPolicies` значение `DisableStrongPassword`. Например, можно изменить запрос создания пользователя следующим образом:

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-azure-ad-graph-api-to-migrate-users"></a>Шаг 1. Миграция пользователей с помощью Azure AD API Graph

Вы создадите учетную запись пользователя Azure AD B2C с помощью API Graph (с паролем или со случайным паролем). В этом разделе описывается процесс создания учетных записей пользователей в каталоге Azure AD B2C с помощью API Graph.

### <a name="step-11-register-your-application-in-your-tenant"></a>Шаг 1.1. Регистрация приложения в клиенте

Для взаимодействия с API Graph сначала необходимо иметь учетную запись службы с правами администратора. В Azure AD зарегистрируйте приложение и выполните аутентификацию в Azure AD. Учетные данные приложения такие: **идентификатор приложения** и **секрет приложения**. Приложение вызывает API Graph, действуя само по себе, а не под именем конкретного пользователя.

Сначала зарегистрируйте переносимое приложение в Azure AD. Затем создайте ключ приложения (секрет приложения) и настройте для приложения полномочия записи.

1. Войдите на [портале Azure][Portal].
1. Выберите фильтр **каталог и подписка** в верхней правой части портала.
1. Выберите каталог, содержащий клиент Azure AD B2C.
1. В меню слева выберите **Azure Active Directory** (*не* Azure AD B2C). Чтобы найти его, может потребоваться выбрать **все службы**.
1. Выберите **Регистрация приложений (устаревший)** .
1. Выберите **Регистрация нового приложения**.

   ![Выделенные элементы меню Azure Active Directory и Регистрация приложений](media/active-directory-b2c-user-migration/pre-migration-app-registration.png)

1. Создайте приложение, сделав следующее:

   - В качестве **имени** используйте *B2CUserMigration* или любое другое.
   - В качестве **типа приложения**выберите **веб-приложение или API**.
   - Для **URL-адреса входа**используйте `https://localhost` (это не относится к этому приложению).
   - Нажмите кнопку **Создать**.

    После создания приложения отображается страница зарегистрированное **приложение** , в которой отображаются ее свойства.
1. Скопируйте **идентификатор приложения**и сохраните его для последующего использования.

### <a name="step-12-create-the-application-secret"></a>Шаг 1.2. Создание секрета приложения

1. На странице **зарегистрированное приложение** выберите **Параметры**.
1. Выберите **Ключи**.
1. В разделе **пароли**добавьте новый ключ (также известный как секрет клиента) с именем *миклиентсекрет* или другое имя, выберите срок действия, нажмите кнопку **сохранить**, а затем скопируйте значение ключа для последующего использования.

    ![Элемент меню "значение идентификатора приложения" и "ключи", выделенный в портал Azure](media/active-directory-b2c-user-migration/pre-migration-app-id-and-key.png)

### <a name="step-13-grant-administrative-permission-to-your-application"></a>Шаг 1.3. Предоставление административных разрешений для приложения

1. В меню **Параметры** выберите **необходимые разрешения**.
1. Выберите **Windows Azure Active Directory**.
1. В области **Разрешить доступ** в разделе **Разрешения приложений** выберите **Чтение и запись данных каталога**, а затем — **Сохранить**.
1. В области **необходимые разрешения** выберите **предоставить разрешения**и нажмите **кнопку Да**.

   ![Флажок "чтение и запись каталога", "Сохранить" и "предоставить" выделенные разрешения](media/active-directory-b2c-user-migration/pre-migration-app-registration-permissions.png)

Теперь у вас есть приложение с разрешением на создание, чтение и обновление пользователей из клиента Azure AD B2C.

### <a name="step-14-optional-environment-cleanup"></a>Шаг 1.4. Очистка среды (необязательно)

Чтение и запись данных каталога *не* предусматривают возможность удаления пользователей. Чтобы предоставить приложению эту возможность (для очистки среды), необходимо выполнить дополнительный шаг. Этот шаг заключается в запуске PowerShell для установки разрешений администратора учетной записи пользователя. В противном случае можно перейти к следующему разделу.

> [!IMPORTANT]
> Необходимо использовать учетную запись администратора клиента B2C, которая является *локальной* по отношению к клиенту B2C. Синтаксис имени учетной записи *—\@Admin contosob2c.onmicrosoft.com*.

В этом сценарии PowerShell, для которого требуется [модуль Azure AD PowerShell v2][AD-Powershell], выполните следующие действия.

1. Подключитесь к веб-службе. Для этого выполните командлет `Connect-AzureAD` в командной строке Windows PowerShell и предоставьте свои учетные данные.

1. Используйте **идентификатор приложения** для назначения приложению роли администратора учетной записи пользователя. Идентификаторы этих ролей известны, поэтому можно просто ввести **идентификатор приложения** в сценарий.

```powershell
# NOTE: This script REQUIRES the Azure AD PowerShell V2 module
#       https://docs.microsoft.com/powershell/azure/active-directory/install-adv2

Connect-AzureAD

$AppId = "<Your application ID>"

# Fetch Azure AD application to assign to role
$roleMember = Get-AzureADServicePrincipal -Filter "AppId eq '$AppId'"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add application to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
```

Измените значение `$AppId` на свой **идентификатор приложения** Azure AD.

## <a name="step-2-pre-migration-application-sample"></a>Шаг 2. Пример приложения перед началом миграции

Пример кода перед переносом можно найти в репозитории GitHub, поддерживаемом `azure-ad-b2c/user-migration` сообществом:

[Azure-AD-B2C/пользователь-миграция/Предварительная миграция][UserMigrationSample-code] GitHub

### <a name="step-21-edit-the-migration-data-file"></a>Шаг 2.1. Изменение файла данных миграции

Пример приложения использует JSON-файл, содержащий данные фиктивных пользователей. После успешного запуска примера можно изменить код для получения данных из собственной базы данных. Вы также можете экспортировать профиль пользователя в JSON-файл и настроить приложение для использования этого файла.

Чтобы изменить JSON-файл, откройте решение Visual Studio `AADB2C.UserMigration.sln`. В проекте `AADB2C.UserMigration` откройте файл `UsersData.json`.

![Часть файла Усерсдата. JSON, показывающая блоки JSON двух пользователей](media/active-directory-b2c-user-migration/pre-migration-data-file.png)

Как видите, файл содержит список сущностей пользователей. Каждая сущность пользователя имеет следующие свойства.

- email
- displayName
- firstName
- lastName
- пароль (может быть пустым).

> [!NOTE]
> Во время компиляции Visual Studio копирует файл в каталог `bin`.

### <a name="step-22-configure-the-application-settings"></a>Шаг 2.2. Настройка параметров приложения

В проекте `AADB2C.UserMigration` откройте файл *App.config*. Замените следующие параметры приложения собственными значениями.

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Client Secret Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
</appSettings>
```

> [!NOTE]
> - Использование строки подключения таблицы Azure описано в следующих разделах.
> - В качестве имени вашего клиента B2C используется домен, указанный при создании клиента. Это имя отображается на портале Azure. Имя клиента обычно заканчивается суффиксом *.onmicrosoft.com* (например, *contosob2c.onmicrosoft.com*).

### <a name="step-23-run-the-pre-migration-process"></a>Шаг 2.3. Запуск процесса подготовки к миграции

Щелкните решение `AADB2C.UserMigration` правой кнопкой мыши, а затем еще раз создайте пример кода. Если вам удастся выполнить это действие, в каталоге `AADB2C.UserMigration\bin\Debug\net461` появится исполняемый файл `UserMigration.exe`. Чтобы запустить процесс миграции, используйте один из следующих параметров командной строки:

- Чтобы **перенести пользователей с паролями**, выполните команду `UserMigration.exe 1`.

- Чтобы **перенести пользователей со случайными паролями**, выполните команду `UserMigration.exe 2`. Эта операция также создает сущность таблицы Azure. Далее настройте политику для вызова службы REST API. Служба использует таблицу Azure для отслеживания процесса миграции и управления им.

![Окно командной строки, в котором отображаются выходные данные команды Усермигратион. exe](media/active-directory-b2c-user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>Шаг 2.4. Проверка процесса подготовки к миграции

Для проверки миграции используйте один из следующих двух методов:

- Чтобы найти пользователя по отображаемому имени, воспользуйтесь порталом Azure.

   1. Откройте **Azure AD B2C**, а затем выберите **Пользователи**.
   1. В поле поиска введите отображаемое имя пользователя и просмотрите профиль пользователя.

- Для получения данных о пользователе по адресу электронной почты для входа используйте этот пример приложения:

   1. Выполните следующую команду:

      ```Console
          UserMigration.exe 3 {email address}
      ```

      > [!TIP]
      > Кроме того, вы можете получить данные пользователя по отображаемому имени, выполнив следующую команду: `UserMigration.exe 4 "<Display name>"`.

   1. Откройте файл UserProfile.json в редакторе JSON, чтобы просмотреть сведения о пользователе.

      ![Файл UserProfile. JSON открыт в редакторе Visual Studio Code](media/active-directory-b2c-user-migration/pre-migration-get-by-email2.png)

### <a name="step-25-optional-environment-cleanup"></a>Шаг 2.5. Очистка среды (необязательно)

Если вы хотите очистить клиент Azure AD и удалить пользователей из каталога Azure AD, выполните команду `UserMigration.exe 5`.

> [!NOTE]
> * Чтобы очистить клиент, настройте разрешения администратора учетной записи пользователя для приложения.
> * Пример приложения для миграции удаляет всех пользователей, указанных в файле JSON.

### <a name="step-26-sign-in-with-migrated-users-with-password"></a>Шаг 2.6. Выполнение входа для перенесенных пользователей (с паролем)

После выполнения процесса подготовки к миграции с паролями пользователей учетные записи готовы к использованию и пользователи могут выполнять вход в приложение с помощью Azure AD B2C. При отсутствии доступа к паролям пользователей перейдите к следующему разделу.

## <a name="step-3-help-users-reset-their-password"></a>Шаг 3. Помощь пользователям в сбросе пароля

Если вы переносите пользователей со случайными паролями, им необходимо сбросить пароль. Чтобы помочь им сбросить пароль, отправьте приветственное сообщение электронной почты со ссылкой для сброса пароля.

Чтобы получить ссылку на политику сброса паролей, выполните следующие действия. В этой процедуре предполагается, что вы ранее создали [пользовательскую политику](active-directory-b2c-get-started-custom.md)сброса пароля.

1. Выберите каталог, содержащий клиент Azure AD B2C, используя фильтр **каталогов и подписок** в правом верхнем углу [портал Azure](https://portal.azure.com).
1. Выберите **Azure AD B2C** в меню слева (или во **всех службах**).
1. В разделе **политики**выберите **инфраструктура процедур идентификации**.
1. Выберите политику сброса пароля. Например, *B2C_1A_PasswordReset*.
1. Выберите приложение в раскрывающемся списке **выберите приложение** .

    > [!NOTE]
    > Для **запуска теперь** требуется, чтобы в клиенте было зарегистрировано хотя бы одно приложение. Сведения о регистрации приложений см. в разделе [учебник. Зарегистрируйте приложение в Azure Active Directory B2C][B2C-AppRegister].

1. Скопируйте URL-адрес, показанный в текстовом поле **Запустить сейчас** , и отправьте его пользователям.

    ![Страница политики сброса пароля с выделенной конечной точкой запуска](media/active-directory-b2c-user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-the-user-migration-status"></a>Шаг 4. Изменение политики для проверки и установки состояния миграции пользователя (необязательно)

> [!NOTE]
> Чтобы проверить и изменить состояние миграции пользователей, необходимо использовать настраиваемую политику. Инструкции по настройке из раздела [Начало работы с пользовательскими политиками][B2C-GetStartedCustom] должны быть завершены.

При попытке пользователя войти без сброса пароля в первый раз политика должна возвращать дружественное сообщение об ошибке. Пример:

> *Your password has expired. To reset it, select the Reset Password link* (Срок действия пароля истек. Чтобы сбросить пароль, щелкните ссылку сброса пароля).

Этот необязательный шаг требует использования Azure AD B2C с пользовательскими политиками, как описано в статье [Azure Active Directory B2C: начало работы с настраиваемыми политиками][B2C-GetStartedCustom].

В этом разделе измените политику для проверки состояния миграции пользователя при выполнении входа. Если пользователь не изменил пароль, отправьте сообщение об ошибке HTTP 409. Пользователю будет предложено щелкнуть ссылку **Forgot your password?** (Забыли пароль?).

Чтобы отслеживать изменение пароля, используйте таблицу Azure. При запуске процесса подготовки к миграции с параметром командной строки `2` вы создаете сущность пользователя в таблице Azure. Ваша служба выполняет следующие действия:

- Когда выполняется вход, политика Azure AD B2C вызывает службу миграции RESTful, отправляя сообщение электронной почты в виде входного утверждения. Служба ищет адрес электронной почты в таблице Azure. Если адрес существует, служба выводит сообщение: *You must change password* (Требуется изменить пароль).

- Когда пользователь успешно изменит пароль, удалите сущность из таблицы Azure.

> [!NOTE]
> Мы используем таблицу Azure для упрощения примера. Состояние миграции можно хранить в любой базе данных или как пользовательское свойство в учетной записи Azure AD B2C.

### <a name="41-update-your-application-setting"></a>4.1. Обновление параметров приложения

1. Чтобы протестировать демоверсию RESTful API, откройте `AADB2C.UserMigration.sln` в Visual Studio.
1. В проекте откройте файл *Web. config.* `AADB2C.UserMigration.API` Замените параметр настроенным на [шаге 2.2](#step-22-configure-the-application-settings):

    ```json
    {
        "BlobStorageConnectionString": "{The Azure Blob storage connection string}",
        ...
    }
    ```

### <a name="step-42-deploy-your-web-application-to-azure-app-service"></a>Шаг 4.2. Развертывание веб-приложения в Службе приложений Azure

Щелкните правой кнопкой мыши `AADB2C.UserMigration.API` в обозревателе решений и выберите "Опубликовать...". Следуйте инструкциям по публикации в службе приложений Azure. Дополнительные сведения см. в статье [Развертывание локального репозитория Git в службе приложений Azure][AppService-Deploy].

### <a name="step-43-add-a-technical-profile-and-technical-profile-validation-to-your-policy"></a>Шаг 4.3. Добавление технического профиля и его проверки в политику

1. В обозревателе решений разверните узел "Элементы решения" и откройте файл политики *TrustFrameworkExtensions.xml*.
1. Измените поля `TenantId`, `PublicPolicyUri` и `<TenantId>` с `yourtenant.onmicrosoft.com` именем клиента.
1. В элементе замените все `ProxyIdentityExperienceFrameworkAppId` экземпляры и `IdentityExperienceFrameworkAppId` идентификаторы приложений, настроенные в разделе Приступая к [работе с настраиваемыми политиками][B2C-GetStartedCustom]. `<TechnicalProfile Id="login-NonInteractive">`
1. В узле `<ClaimsProviders>` найдите следующий фрагмент кода XML. Измените значение `ServiceUrl` для указания URL-адреса службы приложений Azure.

    ```XML
    <ClaimsProvider>
      <DisplayName>REST APIs</DisplayName>
      <TechnicalProfiles>

        <TechnicalProfile Id="LocalAccountSignIn">
          <DisplayName>Local account just in time migration</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/LoalAccountSignIn</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="email" />
          </InputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>

        <TechnicalProfile Id="LocalAccountPasswordReset">
          <DisplayName>Local account just in time migration</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/PasswordUpdated</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
          </InputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

Предыдущий технический профиль определяет одно входящее утверждение: `signInName` (отправить по электронной почте). Когда выполняется вход, утверждение отправляется в конечную точку RESTful.

После определения технического профиля для RESTful API укажите политике Azure AD B2C вызвать технический профиль. Фрагмент XML-кода переопределяет параметр `SelfAsserted-LocalAccountSignin-Email`, который определен в базовой политике. Фрагмент XML-кода также добавляет `ValidationTechnicalProfile` с ReferenceId, указывающим на технический профиль `LocalAccountUserMigration`.

### <a name="step-44-upload-the-policy-to-your-tenant"></a>Шаг 4.4. Отправка политики в клиент

1. На [портале Azure][Portal] переключитесь в [контекст клиента Azure AD B2C][B2C-NavContext] и выберите **Azure AD B2C**.
1. Выберите **Инфраструктура процедур идентификации**.
1. Выберите **Все политики**.
1. Щелкните **Отправить политику**.
1. Установите флажок **Перезаписать политику, если она существует**.
1. Отправьте файл *TrustFrameworkExtensions.xml* и немного подождите, чтобы удостовериться, что он прошел проверку.

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a>Шаг 4.5. Тестирование настраиваемой политики с помощью команды "Запустить сейчас"

1. Выберите **Azure AD B2C**, а затем выберите **инфраструктура процедур идентификации**.
1. Откройте *B2C_1A_signup_signin*, отправленную вами пользовательскую политику проверяющей стороны, а затем выберите **Run Now** (Запустить сейчас).
1. Введите учетные данные одного из перенесенных пользователей, а затем выберите **Вход**. В REST API должно отобразиться следующее сообщение об ошибке:

    ![Страница регистрации входа с сообщением об ошибке "изменение пароля"](media/active-directory-b2c-user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshoot-your-rest-api"></a>Шаг 4.6. Устранение неполадок REST API (необязательно)

Вы можете просматривать и отслеживать данные ведения журналов в режиме реального времени.

1. В меню параметров приложения RESTful в разделе **Мониторинг** выберите **Журналы диагностики**.
1. Установите для параметра **Вход в приложения (файловая система)** значение **Вкл.**
1. Установите для параметра **Уровень** значение **Verbose** (Подробный).
1. Нажмите кнопку **Сохранить**.

    ![Страница настройки журналов диагностики в портал Azure](media/active-directory-b2c-user-migration/pre-migration-diagnostic-logs.png)

1. В меню **параметров** выберите **Поток журналов**.
1. Проверьте выходные данные RESTful API.

> [!IMPORTANT]
> Журналы диагностики следует использовать только во время разработки и тестирования. Выходные данные RESTful API могут содержать конфиденциальные сведения, которые не должны быть раскрыты в рабочей среде.

## <a name="optional-download-the-complete-policy-files"></a>Скачивание полного текста файлов политики (необязательно)

После выполнения пошагового руководства Приступая к [работе с настраиваемыми политиками][B2C-GetStartedCustom] рекомендуется создать сценарий с помощью собственных файлов пользовательской политики. Для справки мы предоставили [примеры файлов политики][UserMigrationSample-policy].

[AD-PasswordPolicies]: https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy
[AD-Powershell]: https://docs.microsoft.com/powershell/azure/active-directory/install-adv2
[AppService-Deploy]: https://docs.microsoft.com/aspnet/core/tutorials/publish-to-azure-webapp-using-vs
[B2C-AppRegister]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration
[B2C-GetStarted]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started
[B2C-GetStartedCustom]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom
[B2C-GraphQuickStart]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet
[B2C-NavContext]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-navigate-to-b2c-context
[Portal]: https://portal.azure.com/
[UserMigrationSample-code]: https://github.com/azure-ad-b2c/user-migration/tree/master/pre-migration/source-code
[UserMigrationSample-policy]: https://github.com/azure-ad-b2c/user-migration/tree/master/pre-migration/policy
