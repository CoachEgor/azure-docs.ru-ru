---
title: Включить AAD для интеграции Azure SSIS
description: В этой статье описывается включение аутентификации Azure Active Directory с использованием управляемого удостоверения для службы "Фабрика данных Azure" с целью создания среды выполнения интеграции Azure-SSIS.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.devlang: powershell
ms.topic: conceptual
author: swinarko
ms.author: sawinark
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 5/14/2019
ms.openlocfilehash: 70367a38fbf7b59486e2eaaf6c05634aa7575869
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260713"
---
# <a name="enable-azure-active-directory-authentication-for-azure-ssis-integration-runtime"></a>Включение аутентификации Azure Active Directory для среды выполнения интеграции Azure-SSIS

В этой статье показано, как включить активную каталог Azure (Azure AD) аутентификацию с управляемой идентификацией для вашей фабрики данных Azure (ADF) и использовать ее вместо обычных методов аутентификации (например, проверки подлинности S'L) для:

- Создайте от вашего имени сервер интеграции Azure-SSIS Integration Runtime (IR), который, в свою очередь, предоставит базу данных каталога SSIS (SSISDB) на сервере базы данных Azure s'L..

- Подключение к различным ресурсам Azure при запуске пакетов SSIS на ИК Azure-SSIS.

Для получения дополнительной информации об управляемой идентификации для вашего ADF, [см.](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity)

> [!NOTE]
>-  В этом сценарии аутентификация Azure AD с управляемой идентификацией для ADF используется только при создании и последующем запуске ИК SSIS, которая, в свою очередь, будет предоставлять и подключаться к SSISDB. Для выполнения пакетов SSIS ваш ИК SSIS по-прежнему будет подключаться к SSISDB с помощью проверки подлинности S'L с полностью управляемыми учетными записями, которые создаются во время подготовки SSISDB.
>-  Если вы уже создали SSIS IR с помощью проверки подлинности S'L, вы не можете перенастроить его для использования аутентификации Azure AD через PowerShell в это время, но вы можете сделать это через приложение Azure portal/ADF. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="enable-azure-ad-on-azure-sql-database"></a>Включение Azure AD в службе "База данных SQL Azure"

Сервер Базы данных SQL Azure поддерживает создание базы данных с использованием пользователя Azure AD. Для начала нужно создать группу Azure AD с управляемым удостоверением, членом которой станет ADF. Далее необходимо назначить одного из пользователей Azure AD администратором Active Directory для сервера Базы данных SQL Azure, а затем подключиться к этому серверу через SQL Server Management Studio (SSMS) под учетными данными этого пользователя. И в конечном итоге потребуется создать автономного пользователя, представляющего группу Azure AD, чтобы позволить Azure-SSIS IR использовать управляемое удостоверение для ADF с целью создания SSISDB от вашего имени.

### <a name="create-an-azure-ad-group-with-the-managed-identity-for-your-adf-as-a-member"></a>Создание группы Azure AD с управляемым удостоверением, членом которой станет ADF

Можно использовать существующую группу Azure AD или создать новую с помощью Azure AD PowerShell.

1.  Установите модуль [Azure AD PowerShell.](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2)

2.  Войдите с помощью командлета  `Connect-AzureAD` и выполните следующий командлет, чтобы создать группу и сохранить ее в переменной:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Результат будет выглядеть как в следующем примере и содержать значение переменной.

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Добавьте в группу управляемое удостоверение службы "Фабрика данных Azure". Вы можете следить за статьей [Managed identiy для Data Factory,](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) чтобы получить основной идентификатор управляемого объекта идентификации (например, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, но не использовать управляемый идентификационный идентификатор для этой цели).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Членство в группе можно также проверить позже.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="configure-azure-ad-authentication-for-azure-sql-database-server"></a>Настройка аутентификации Azure AD для сервера Базы данных SQL Azure

Для  [настройки и администрирования аутентификации Azure AD с использованием SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) выполните следующие действия.

1.  На портале Azure выберите **все сервисы** -> **серверов S'L** из левой навигации.

2.  Выберите сервер Базы данных SQL Azure, для которого нужно настроить аутентификацию Azure AD.

3.  В разделе колонки **Параметры** выберите **Администратор Active Directory**.

4.  На панели команд щелкните **Задать администратора**.

5.  Выберите учетную запись пользователя Azure AD для администратора сервера, а затем **выберите.**

6.  На панели команд нажмите кнопку **Сохранить**.

### <a name="create-a-contained-user-in-azure-sql-database-server-representing-the-azure-ad-group"></a>Создание на сервере Базы данных SQL Azure автономного пользователя, представляющего группу Azure AD

Для этого следующего шага вам нужна  [студия управления серверами Microsoft S'L](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)(SSMS).

1. Запустите SSMS.

2. В диалоге **Connect to Server** введите имя сервера базы данных Azure S'L в поле имени **сервера.**

3. В поле **аутентификации** выберите **Active Directory - Universal при поддержке MFA** (вы также можете использовать два других типа аутентификации Active Directory, [см. Настройка и управление аутентификацией Azure AD с помощью S'L).](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)

4. В поле **имени пользователя** введите имя учетной записи Azure AD, testuser@xxxonline.comкоторую вы установите в качестве администратора сервера, например,

5. выберите **Подключите** и завершите процесс вхинга.

6. В **обозревателе объектов** разверните папку **Базы данных** -> **Системные базы данных**.

7. Нажмите справа на **основной** базе данных и выберите **новый запрос.**

8. В окне запроса введите следующую команду T-S'S l и выберите **«Выполнить»** на панели инструментов.

   ```sql
   CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
   ```

   В результате выполнения команды должен быть создан автономный пользователь для представления группы.

9. Сотрите данные в окне запроса, введите следующую команду T-SQL и щелкните **Выполнить** на панели инструментов:

   ```sql
   ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
   ```

   После выполнения этой команды автономный пользователь сможет создать базу данных (SSISDB).

10. Если ваш SSISDB был создан с помощью проверки подлинности S'L и вы хотите переключиться на использование проверки подлинности Azure AD для доступа к ней, нажмите правой кнопкой мыши на базе данных **SSISDB** и выберите **новый запрос.**

11. В окне запроса введите следующую команду T-S'S l и выберите **«Выполнить»** на панели инструментов.

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    В результате выполнения команды должен быть создан автономный пользователь для представления группы.

12. Сотрите данные в окне запроса, введите следующую команду T-SQL и щелкните **Выполнить** на панели инструментов:

    ```sql
    ALTER ROLE db_owner ADD MEMBER [SSISIrGroup]
    ```

    В результате выполнения команды автономный пользователь получает права на доступ к SSISDB.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Включение Azure AD в службе "Управляемый экземпляр Базы данных SQL Azure"

Управляемый экземпляр Базы данных SQL Azure поддерживает создание базы данных с использованием управляемого удостоверения для ADF напрямую. Присоединять управляемое удостоверение для ADF к группе Azure AD или создавать автономного пользователя, представляющего эту группу в управляемом экземпляре, не требуется.

### <a name="configure-azure-ad-authentication-for-azure-sql-database-managed-instance"></a>Настройка аутентификации Azure AD для управляемого экземпляра Базы данных SQL Azure

Выполните действия в [provision An Azure Active Directory administratory для вашего управляемого экземпляра.](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-managed-instance)

### <a name="add-the-managed-identity-for-your-adf-as-a-user-in-azure-sql-database-managed-instance"></a>Добавление управляемого удостоверения для ADF в качестве пользователя управляемого экземпляра Базы данных SQL Azure

Для этого следующего шага вам нужна  [студия управления серверами Microsoft S'L](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)(SSMS).

1.  Запустите SSMS.

2.  Подключитесь к управляемой инстанции с помощью учетной записи S'L Server, которая является **sysadmin.** Это временное ограничение, которое будет удалено после того, как принципы сервера Azure AD (логины) для управляемой базы данных Azure S'L станут GA. Вы увидите следующую ошибку, если попытаетесь использовать учетную запись Ad Azure для создания входа: Msg 15247, Уровень 16, State 1, Line 1 Пользователь не имеет разрешения на выполнение этого действия.

3.  В **обозревателе объектов** разверните папку **Базы данных** -> **Системные базы данных**.

4.  Нажмите справа на **основной** базе данных и выберите **новый запрос.**

5.  В окне запроса выполните следующий скрипт T-S'L, чтобы добавить управляемый итог для вашего ADF в качестве пользователя

    ```sql
    CREATE LOGIN [{your ADF name}] FROM EXTERNAL PROVIDER
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{your ADF name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{your ADF name}]
    ```
    
    После выполнения этой команды управляемое удостоверение для ADF получит возможность для создания базы данных (SSISDB).

6.  Если ваш SSISDB был создан с помощью проверки подлинности S'L и вы хотите переключиться на использование проверки подлинности Azure AD для доступа к ней, нажмите правой кнопкой мыши на базе данных **SSISDB** и выберите **новый запрос.**

7.  В окне запроса введите следующую команду T-S'S l и выберите **«Выполнить»** на панели инструментов.

    ```sql
    CREATE USER [{your ADF name}] FOR LOGIN [{your ADF name}] WITH DEFAULT_SCHEMA = dbo
    ALTER ROLE db_owner ADD MEMBER [{your ADF name}]
    ```

    После выполнения этой команды управляемое удостоверение для ADF получит возможность для доступа к SSISDB.

## <a name="provision-azure-ssis-ir-in-azure-portaladf-app"></a>Подготовка Azure SSIS IR на портале Azure или в приложении ADF

При подготовке Azure-SSIS IR на портале Azure или в приложении ADF на странице **Параметры SQL** установите флажок **Использовать аутентификацию AAD с управляемым удостоверением для ADF**. На приведенном ниже снимке экрана показаны параметры для среды выполнения интеграции, в которой SSISDB находится на сервере Базы данных SQL Azure. Для среды выполнения интеграции, в которой SSISDB находится в управляемом экземпляре, параметры **Уровень службы базы данных каталога** и **Разрешить службам Azure доступ** не применяются, а остальные параметры будут такими же.

Дополнительные сведения о создании Azure-SSIS IR см. в статье [Создание среды выполнения интеграции Azure-SSIS в Фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Параметры среды выполнения интеграции Azure-SSIS](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-azure-ssis-ir-with-powershell"></a>Подготовка Azure-SSIS IR с помощью PowerShell

Чтобы подготовить к работе среду выполнения интеграции Azure-SSIS с помощью PowerShell, сделайте следующее:

1.  Установка модуля [Azure PowerShell.](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) 

2.  Не задавайте в скрипте параметр `CatalogAdminCredential`. Пример:

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
    ```

## <a name="run-ssis-packages-with-managed-identity-authentication"></a>Запуск пакетов SSIS с управляемой аутентификацией личности

При запуске пакетов SSIS на ИК Azure-SSIS можно использовать управляемую проверку подлинности личных данных для подключения к различным ресурсам Azure. В настоящее время мы уже поддерживаем управляемую аутентификацию идентификации в следующих менеджерах соединения.

- [диспетчер соединений OLE DB](https://docs.microsoft.com/sql/integration-services/connection-manager/ole-db-connection-manager#managed-identities-for-azure-resources-authentication)

- [ADO.NET менеджер подключения](https://docs.microsoft.com/sql/integration-services/connection-manager/ado-net-connection-manager#managed-identities-for-azure-resources-authentication)

- [Менеджер подключения к хранилищам Azure](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager#managed-identities-for-azure-resources-authentication)
