---
title: Создание учетной записи на портале Azure
description: Узнайте, как создать учетную запись пакетной службы Azure на портале Azure для обработки крупных параллельных рабочих нагрузок в облаке.
ms.topic: how-to
ms.date: 06/10/2020
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1205de2b800588b735aeb20d388ba4b64bc6b078
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "84711346"
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Создание учетной записи пакетной службы на портале Azure

В этом разделе показано, как создать учетную запись пакетной службы Azure на [портал Azure](https://portal.azure.com), выбрав Свойства учетной записи, соответствующие вашему сценарию вычислений. Вы также узнаете, где найти важные свойства учетной записи, такие как ключи доступа и URL-адреса учетных записей.

Дополнительные сведения об учетных записях пакетной службы см. в статье [Рабочий процесс и ресурсы пакетной службы](batch-service-workflow-features.md).

## <a name="create-a-batch-account"></a>Создание учетной записи Пакетной службы

[!INCLUDE [batch-account-mode-include](../../includes/batch-account-mode-include.md)]

1. Войдите на [портал Azure](https://portal.azure.com).

1. Выберите **создать ресурс** **, а затем** выберите Среда выполнения и **Пакетная служба**.

1. Введите параметры **новой учетной записи пакетной службы**. Укажите следующие сведения:

    ![Создание учетной записи Пакетной службы][account_portal]

    а. **Подписка**: Подписка, в которой будет создана учетная запись пакетной службы. Если у вас только одна подписка, она будет выбрана по умолчанию.

    b. **Группа ресурсов.** Существующая группа ресурсов для новой учетной записи пакетной службы (при необходимости можно создать новую группу ресурсов).

    c. **Имя учетной записи**. Имя учетной записи должно быть уникальным в пределах региона Azure, где будет создана учетная запись (описание параметра **Расположение** см. ниже). Имя учетной записи может содержать только строчные буквы и цифры, а его длина должна быть не меньше 3 и не больше 24 символов.

    d. **Расположение.** Регион Azure, в котором будет создана учетная запись пакетной службы. Будут отображаться только те регионы, которые поддерживаются для вашей подписки и группы ресурсов.

    д) **Учетная запись хранения**. Необязательная учетная запись хранения Azure, которая будет связана с учетной записью пакетной службы. Для наилучшей производительности рекомендуется использовать учетную запись хранения общего назначения версии 2. Все параметры учетной записи хранения в пакетной службе см. в [обзоре функций пакетной службы](accounts.md#azure-storage-accounts). На портале выберите существующую учетную запись хранения или создайте новую.

      ![Создание учетной записи хранения][storage_account]

    е) **Режим выделения пула**. На вкладке параметров **Расширенные** вы можете указать для режима распределения пула значение **Пакетная служба** или **Пользовательская подписка**. В большинстве случаев можно использовать значение по умолчанию — **Пакетная служба**.

      ![Режим распределения пула пакетной службы][pool_allocation]

1. Выберите **Создать**, чтобы создать учетную запись.

## <a name="view-batch-account-properties"></a>Просмотр свойств учетной записи пакетной службы

После создания учетной записи выберите ее для доступа к параметрам и свойствам учетной записи. Все параметры и свойства учетной записи можно просмотреть с помощью меню слева.

> [!NOTE]
> Имя учетной записи пакетной службы является идентификатором и его нельзя изменить. Если необходимо изменить имя учетной записи пакетной службы, удалите учетную запись и создайте новую с нужным именем.

![Страница учетной записи пакетной службы на портале Azure][account_blade]

* **Имя, URL-адрес и ключи учетной записи пакетной службы**. При разработке приложения с использованием [API-интерфейсов пакетной службы](batch-apis-tools.md#azure-accounts-for-batch-development) вам понадобится URL-адрес и ключ учетной записи для доступа к ресурсам пакетной службы. (Пакетная служба также поддерживает аутентификацию на основе Azure Active Directory.)

    Чтобы просмотреть сведения о доступе к учетной записи пакетной службы, выберите **Ключи**.

    ![Ключи учетной записи пакетной службы на портале Azure][account_keys]

* Чтобы просмотреть имя и ключи учетной записи хранения, связанной с учетной записью пакетной службы, выберите **Учетная запись хранения**.

* Чтобы просмотреть квоты ресурсов, которые применяются для учетной записи пакетной службы, выберите **Квоты**. Подробные сведения см. в статье [Квоты и ограничения пакетной службы](batch-quota-limit.md).

## <a name="additional-configuration-for-user-subscription-mode"></a>Дополнительная настройка для режима пользовательской подписки

Если вы решили создать учетную запись пакетной службы в режиме пользовательской подписки, сначала выполните приведенные ниже дополнительные инструкции.

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Предоставление пакетной службе Azure доступа к подписке (одноразовая операция)

При первом создании учетной записи пакетной службы в режиме пользовательской подписки нужно зарегистрировать подписку в пакетной службе. (Если вы уже сделали это, перейдите к следующему разделу.)

1. Войдите на [портал Azure](https://portal.azure.com).

1. Последовательно выберите пункты **Все службы** > **Подписки** и выберите подписку, которую нужно использовать для учетной записи пакетной службы.

1. На странице **Подписки** выберите **Поставщики ресурсов** и выполните поиск **Microsoft.Batch**. Убедитесь, что поставщик ресурсов **Microsoft.Batch** зарегистрирован в подписке. Если он не зарегистрирован, выберите ссылку **Зарегистрировать**.

    ![Регистрация поставщика Microsoft.Batch][register_provider]

1. На странице **Подписка** выберите **Управление доступом (IAM)**  > **Назначения ролей** > **Добавление назначения ролей**.

    ![Управление доступом к подписке][subscription_access]

1. На странице **Добавление назначения ролей** выберите роль **участник** или **владелец** , а затем найдите API пакетной службы. Продолжайте поиск для каждой из этих строк, пока не найдете API:
    1. **MicrosoftAzureBatch**;
    1. **Microsoft Azure Batch**. Более новые клиенты Azure AD могут использовать это имя.
    1. **ddbf3205-c6bd-46ae-8127-60eb93363864** — это идентификатор API пакетной службы.

1. Выберите найденный API пакетной службы и выберите **Сохранить**.

    ![Добавление разрешений для пакетной службы][add_permission]

### <a name="create-a-key-vault"></a>Создание хранилища ключей

В режиме пользовательской подписки требуется [Azure Key Vault](../key-vault/general/overview.md) . Key Vault должны находиться в той же подписке и регионе, что и создаваемая учетная запись пакетной службы. 

1. На [портале Azure](https://portal.azure.com) выберите **Создать** > **Безопасность** > **Key Vault**.

1. На странице **создание Key Vault** введите имя Key Vault и создайте группу ресурсов в нужном регионе для учетной записи пакетной службы. Для остальных параметров оставьте значения по умолчанию, а затем выберите **Создать**.

При создании учетной записи пакетной службы в режиме пользовательской подписки укажите **подписку пользователя** в качестве режима выделения пула, выберите Key Vault и установите флажок, чтобы предоставить пакетной службе Azure доступ к Key Vault.

Если вы предпочитаете предоставить доступ к Key Vault вручную, перейдите к разделу **политики доступа** в Key Vault, выберите **Добавить политику доступа** и выполните поиск по запросу **Пакетная служба Microsoft Azure**. После выбора необходимо настроить **разрешения секрета** с помощью раскрывающегося меню. Пакетной службе Azure необходимо предоставить минимум разрешений на **получение**, **перечисление**, **установку** и **удаление**.

![Разрешения секрета для пакетной службы Azure](./media/batch-account-create-portal/secret-permissions.png)

> [!NOTE]
> Убедитесь, что флажки **Виртуальные машины Azure для развертывания** и **Azure Resource Manager для развертывания шаблонов** установлены в разделе **Политики доступа** для связанного ресурса **Key Vault**.
>
> ![Обязательная политика доступа Key Vault](./media/batch-account-create-portal/key-vault-access-policy.png)

### <a name="configure-subscription-quotas"></a>Настройка квот для подписки

Для учетных записей пакетной службы подписки на пользователя необходимо задать квоты ядра вручную. Квоты пакетная служба (цен. категория "Стандартный") Core не применяются к учетным записям в режиме пользовательской подписки.

1. На [портале Azure](https://portal.azure.com) выберите учетную запись пакетной службы в режиме пользовательской подписки, чтобы отобразить параметры и свойства учетной записи.
1. В меню слева выберите **Квоты**, чтобы просмотреть и настроить квоты ядер, связанные с учетной записью пакетной службы.

Дополнительные сведения о квотах ядра для режима пользовательской подписки см. в статье [квоты и ограничения пакетной службы](batch-quota-limit.md).

## <a name="other-batch-account-management-options"></a>Другие параметры управления учетной записью пакетной службы

Помимо портала Azure, создавать учетные записи пакетной службы и управлять ими можно с помощью следующих средств:

* [командлеты PowerShell для пакетной службы;](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Библиотека .NET для управления пакетной службой](batch-management-dotnet.md)

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте подробнее о [рабочем процессе и основных ресурсах пакетной службы](batch-service-workflow-features.md), таких как пулы, узлы, задания и задачи.
* Здесь приведены основные сведения о разработке приложений с поддержкой пакетной службы с помощью [клиентской библиотеки .NET для пакетной службы](quick-run-dotnet.md) или [Python](quick-run-python.md). В этом кратком руководстве рассматривается пример приложения, которое использует пакетную службу для выполнения рабочей нагрузки на нескольких вычислительных узлах и службу хранилища Azure для помещения файла рабочей нагрузки на промежуточное хранение и обработку и его извлечения.

[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch-account-portal.png
[pool_allocation]: ./media/batch-account-create-portal/batch-pool-allocation.png
[account_keys]: ./media/batch-account-create-portal/batch-account-keys.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[register_provider]: ./media/batch-account-create-portal/register_provider.png
