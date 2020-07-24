---
title: Управление ключами учетной записи хранения с помощью Azure Key Vault и Azure CLI
description: Ключи учетной записи хранения обеспечивают тесную интеграцию между Azure Key Vault и доступом на основе ключей к учетной записи хранения Azure.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
ms.subservice: secrets
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/18/2019
ms.openlocfilehash: e8f8a333c880850b239fbaba1ea405b94a1460e8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "87076730"
---
# <a name="manage-storage-account-keys-with-key-vault-and-the-azure-cli"></a>Управление ключами учетной записи хранения с помощью Key Vault и Azure CLI

Учетная запись хранения Azure использует учетные данные, состоящие из имени учетной записи и ключа. Ключ создается автоматически и используется как пароль, а не как криптографический ключ. Key Vault управляет ключами учетной записи хранения путем периодического повторного создания их в учетной записи хранения и предоставляет маркеры подписанного общего доступа для делегированного доступа к ресурсам в вашей учетной записи хранения.

Вы можете использовать функцию ключа управляемой учетной записи хранения Key Vault, чтобы перечислить ключи (синхронизировать) с помощью учетной записи хранения Azure, а также периодически создавать их (циклически переключать). Вы можете управлять ключами как для учетных записей хранения, так и для классических учетных записей хранения.

При использовании функции ключа управляемой учетной записи хранения учитывайте следующие моменты.

- Значения ключа никогда не возвращаются в ответ на вызывающий объект.
- Ключи учетной записи хранения должны управлять только Key Vault. Не управляйте ключами самостоятельно и Избегайте конфликтов с Key Vault процессами.
- Только один объект Key Vault должен управлять ключами учетной записи хранения. Не разрешать управление ключами из нескольких объектов.
- Вы можете запросить Key Vault управления учетной записью хранения с помощью субъекта-пользователя, но не с субъектом-службой.
- Повторное создание ключей с помощью только Key Vault. Не создавайте повторно ключи своей учетной записи хранения вручную.

Мы рекомендуем использовать интеграцию службы хранилища Azure с Azure Active Directory (Azure AD), облачной службой управления удостоверениями и доступом Майкрософт. Интеграция Azure AD доступна для [больших двоичных объектов и очередей Azure](../../storage/common/storage-auth-aad.md), а также предоставляет доступ на основе маркеров OAuth2 к службе хранилища Azure (как Azure Key Vault).

Azure AD позволяет проверять подлинность клиентского приложения с помощью удостоверения приложения или пользователя, а не учетных данных учетной записи хранения. Вы можете использовать [управляемое удостоверение Azure AD](/azure/active-directory/managed-identities-azure-resources/) при запуске в Azure. Управляемые удостоверения изменяют необходимость проверки подлинности клиента и сохраняют учетные данные в приложении или вместе с ним.

Azure AD использует управление доступом на основе ролей (RBAC) для управления авторизацией, которая также поддерживается Key Vault.

## <a name="service-principal-application-id"></a>Идентификатор приложения субъекта-службы

Клиент Azure AD предоставляет каждому зарегистрированному приложению субъект- [службу](/azure/active-directory/develop/developer-glossary#service-principal-object). Субъект-служба выступает в качестве идентификатора приложения, который используется во время настройки авторизации для доступа к другим ресурсам Azure через RBAC.

Key Vault — это приложение Майкрософт, которое предварительно зарегистрировано во всех клиентах Azure AD. Key Vault регистрируется в одном и том же ИДЕНТИФИКАТОРе приложения в каждом облаке Azure.

| Клиенты | Облако | Идентификатор приложения |
| --- | --- | --- |
| Azure AD | Azure для государственных организаций | `7e7c393b-45d0-48b1-a35e-2905ddf8183c` |
| Azure AD | общедоступный пиринг Azure; | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |
| Другое  | Любой | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |

## <a name="prerequisites"></a>Обязательные условия

Для работы с этим руководством необходимо сначала выполнить следующие действия.

- [Установка Azure CLI](/cli/azure/install-azure-cli).
- [Создайте хранилище ключей.](quick-create-cli.md)
- [Создайте учетную запись хранения Azure](../../storage/common/storage-account-create.md?tabs=azure-cli). Имя учетной записи хранения должно содержать только строчные буквы и цифры. Длина имени должна составлять от 3 до 24 символов.
      
## <a name="manage-storage-account-keys"></a>Управление ключами учетной записи хранения

### <a name="connect-to-your-azure-account"></a>Подключение к учетной записи Azure

Проверьте подлинность сеанса Azure CLI с помощью команд [AZ login](/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) .

```azurecli-interactive
az login
``` 

### <a name="give-key-vault-access-to-your-storage-account"></a>Предоставление Key Vault доступа к учетной записи хранения

Воспользуйтесь командой Azure CLI [AZ Role назначение Create](/cli/azure/role/assignment?view=azure-cli-latest) , чтобы предоставить Key Vault доступ к вашей учетной записи хранения. Укажите следующие значения параметров для команды:

- `--role`: Передайте роль службы оператора "оператор ключа" для учетной записи хранения. Эта роль ограничивает область доступа учетной записью хранения. Для классической учетной записи хранения передайте "роль службы оператора ключа классической учетной записи хранения".
- `--assignee`: Передайте значение " https://vault.azure.net ", которое является URL-адресом для Key Vault в общедоступном облаке Azure. (Для использования Azure Говермент Cloud используйте "--асинжее-Object-ID". см. раздел [Service PRINCIPAL ID приложения](#service-principal-application-id).)
- `--scope`: Передайте идентификатор ресурса учетной записи хранения, который находится в форме `/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<YourStorageAccountName>` . Чтобы найти идентификатор подписки, воспользуйтесь командой Azure CLI [AZ Account List](/cli/azure/account?view=azure-cli-latest#az-account-list) . чтобы найти имя учетной записи хранения и группу ресурсов учетной записи хранения, воспользуйтесь командой Azure CLI [AZ Storage Account List](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-list) .

```azurecli-interactive
az role assignment create --role "Storage Account Key Operator Service Role" --assignee 'https://vault.azure.net' --scope "/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<YourStorageAccountName>"
 ```
### <a name="give-your-user-account-permission-to-managed-storage-accounts"></a>Предоставление разрешения учетной записи пользователя для управляемых учетных записей хранения

Используйте командлет Azure CLI [AZ keyvault-Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) , чтобы обновить политику доступа Key Vault и предоставить учетной записи хранения разрешения на доступ к ней.

```azurecli-interactive
# Give your user principal access to all storage account permissions, on your Key Vault instance

az keyvault set-policy --name <YourKeyVaultName> --upn user@domain.com --storage-permissions get list delete set update regeneratekey getsas listsas deletesas setsas recover backup restore purge
```

Обратите внимание, что разрешения для учетных записей хранения не доступны на странице "Политики доступа" учетной записи хранения на портале Azure.
### <a name="create-a-key-vault-managed-storage-account"></a>Создание управляемой учетной записи хранения Key Vault

 Создайте Key Vault управляемую учетную запись хранения с помощью команды Azure CLI [AZ keyvault Storage](/cli/azure/keyvault/storage?view=azure-cli-latest#az-keyvault-storage-add) . Задайте период повторного создания 90 дней. Когда вы намерены время смены, KeyVault повторно создает ключ, который не является активным, а затем устанавливает вновь созданный ключ как активный. Только один из ключей используется для выпусков маркеров SAS в любой момент времени. это активный ключ. Укажите следующие значения параметров для команды:

- `--vault-name`: Передайте имя хранилища ключей. Чтобы найти имя хранилища ключей, воспользуйтесь командой Azure CLI [AZ keyvault List](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-list) .
- `-n`: Передайте имя учетной записи хранения. Чтобы найти имя учетной записи хранения, используйте команду [AZ Storage Account list](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-list) Azure CLI.
- `--resource-id`: Передайте идентификатор ресурса учетной записи хранения, который находится в форме `/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<YourStorageAccountName>` . Чтобы найти идентификатор подписки, воспользуйтесь командой Azure CLI [AZ Account List](/cli/azure/account?view=azure-cli-latest#az-account-list) . чтобы найти имя учетной записи хранения и группу ресурсов учетной записи хранения, воспользуйтесь командой Azure CLI [AZ Storage Account List](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-list) .
   
 ```azurecli-interactive
az keyvault storage add --vault-name <YourKeyVaultName> -n <YourStorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id "/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<YourStorageAccountName>"
 ```

## <a name="shared-access-signature-tokens"></a>Токены подписи общего доступа

Вы также можете попросить Key Vault создать маркеры подписи общего доступа. Подпись общего доступа обеспечивает делегированный доступ к ресурсам в вашей учетной записи хранения. Вы можете предоставить клиентам доступ к ресурсам в вашей учетной записи хранения без предоставления общего доступа к ключам учетной записи. Подписанный URL-доступ обеспечивает безопасный способ предоставления общего доступа к ресурсам хранилища без ущерба для ключей учетной записи.

Команды в этом разделе выполняются в следующих действиях.

- Задайте определение подписанного URL для общего доступа `<YourSASDefinitionName>` . Определение задается в Key Vault управляемой учетной записью хранения `<YourStorageAccountName>` в хранилище ключей `<YourKeyVaultName>` .
- Создание маркера подписанного URL-имени учетной записи для служб BLOB-объектов, файлов, таблиц и очередей. Маркер создается для служб типов ресурсов, контейнеров и объектов. Маркер создается со всеми разрешениями, по протоколу HTTPS и с указанными датами начала и окончания.
- Задайте определение подписанного общего доступа к хранилищу Key Vault в хранилище. Определение содержит универсальный код ресурса (URI) шаблона созданного маркера подписи общего доступа. Определение имеет тип подписанного общего доступа `account` и является допустимым в течение N дней.
- Убедитесь, что подписанный URL-доступ сохранен в хранилище ключей в качестве секрета.

### <a name="create-a-shared-access-signature-token"></a>Создание маркера подписи общего доступа

Создайте определение подписи общего доступа с помощью команды Azure CLI [AZ Storage Account Generate-SAS](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas) . Для этой операции требуются `storage` `setsas` разрешения и.


```azurecli-interactive
az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name <YourStorageAccountName> --account-key 00000000
```
После успешного выполнения операции скопируйте выходные данные.

```console
"se=2020-01-01&sp=***"
```

Эти выходные данные будут переданы `--template-uri` параметру на следующем шаге.

### <a name="generate-a-shared-access-signature-definition"></a>Создание определения подписи общего доступа

Используйте команду Azure CLI [AZ keyvault Storage SAS-Definition Create](/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#az-keyvault-storage-sas-definition-create) , передав выходные данные из предыдущего шага в `--template-uri` параметр, чтобы создать определение подписи общего доступа.  Можно указать имя для `-n` параметра.

```azurecli-interactive
az keyvault storage sas-definition create --vault-name <YourKeyVaultName> --account-name <YourStorageAccountName> -n <YourSASDefinitionName> --validity-period P2D --sas-type account --template-uri <OutputOfSasTokenCreationStep>
```

### <a name="verify-the-shared-access-signature-definition"></a>Проверка определения подписи общего доступа

Вы можете проверить, что определение подписанного URL в хранилище ключей сохранено, с помощью Azure CLI [AZ keyvault Secret List](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-list) и [AZ keyvault Secret показывать](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) команды.

Сначала найдите определение подписи общего доступа в хранилище ключей, выполнив команду [AZ keyvault Secret List](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-list) .

```azurecli-interactive
az keyvault secret list --vault-name <YourKeyVaultName>
```

Секрет, соответствующий определению SAS, будет иметь следующие свойства:

```console
    "contentType": "application/vnd.ms-sastoken-storage",
    "id": "https://<YourKeyVaultName>.vault.azure.net/secrets/<YourStorageAccountName>-<YourSASDefinitionName>",
```

Теперь можно использовать команду [AZ keyvault Secret Показать](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) и `id` свойство, чтобы просмотреть содержимое этого секрета.

```azurecli-interactive
az keyvault secret show --vault-name <YourKeyVaultName> --id <SasDefinitionID>
```

В выходных данных этой команды строка определения SAS будет отображаться как `value` .


## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [ключах, секретах и сертификатах](https://docs.microsoft.com/rest/api/keyvault/).
- Ознакомьтесь с статьями в [блоге Azure Key Vault Team](https://blogs.technet.microsoft.com/kv/).
- См. справочную документацию по [AZ keyvault Storage](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest) .
