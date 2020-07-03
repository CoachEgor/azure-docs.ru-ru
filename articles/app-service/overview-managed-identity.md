---
title: Управляемые удостоверения
description: Узнайте, как управляемые удостоверения работают в службе приложений Azure и функциях Azure, как настроить управляемое удостоверение и создать маркер для серверного ресурса.
author: mattchenderson
ms.topic: article
ms.date: 04/14/2020
ms.author: mahender
ms.reviewer: yevbronsh
ms.openlocfilehash: 875d2bbebdfa95c6d180979399d876eb2afc01b4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "81392526"
---
# <a name="how-to-use-managed-identities-for-app-service-and-azure-functions"></a>Использование управляемых удостоверений в Службе приложений и Функциях Azure

> [!Important] 
> При переносе приложения между подписками или клиентами управляемые удостоверения для Службы приложений и Функций Azure не будут работать в надлежащем режиме. Для приложения нужно получить новое удостоверение, отключив службу и включив ее снова. См. раздел [Удаление удостоверения](#remove) ниже. Также нужно обновить политики доступа во всех зависимых ресурсах, чтобы они использовали новое удостоверение.

В этой статье показано, как создавать управляемое удостоверение для приложений Службы приложений и Функций Azure, а также как их использовать для получения доступа к другим ресурсам. Управляемое удостоверение из Azure Active Directory (Azure AD) позволяет приложению легко получать доступ к другим ресурсам, защищенным Azure AD, таким как Azure Key Vault. Удостоверения управляются платформой Azure, и для них не нужно подготавливать или изменять секреты. Дополнительные сведения об управляемых удостоверениях в Azure AD см. в статье [управляемые удостоверения для ресурсов Azure](../active-directory/managed-identities-azure-resources/overview.md).

Приложению можно предоставить два типа удостоверений:

- **Назначаемое системой удостоверение** привязывается к приложению и удаляется при удалении приложения. Приложение может иметь только одно назначаемое системой удостоверение.
- **Назначаемое пользователем удостоверение** — это автономный ресурс Azure, который можно назначить приложению. Приложение может иметь несколько назначаемых пользователем удостоверений.

## <a name="add-a-system-assigned-identity"></a>Добавление удостоверения, назначенного системой

Чтобы создать приложение с назначаемым системой удостоверением, необходимо задать дополнительное свойство в приложении.

### <a name="using-the-azure-portal"></a>Использование портала Azure

Чтобы настроить управляемое удостоверение на портале, сначала необходимо создать обычное приложение, а затем активировать соответствующую функцию.

1. Создайте приложение на портале обычным образом. Перейдите к нему на портале.

2. Если используется приложение-функция, перейдите к **функциям платформы**. Для других типов приложений прокрутите вниз до группы **параметров** в левой области навигации.

3. Выберите **удостоверение**.

4. На вкладке **Назначено системой** для параметра **Состояние** установите значение **Вкл**. Нажмите кнопку **Сохранить**.

    ![Управляемое удостоверение в Службе приложений](media/app-service-managed-service-identity/system-assigned-managed-identity-in-azure-portal.png)

### <a name="using-the-azure-cli"></a>Использование Azure CLI

Чтобы настроить управляемое удостоверение с помощью Azure CLI, используйте команду `az webapp identity assign` для имеющегося приложения. Примеры из этого раздела можно выполнять тремя способами:

- использовать [Azure Cloud Shell](../cloud-shell/overview.md) с портала Azure;
- Используйте внедренные Azure Cloud Shell с помощью кнопки "попробовать", расположенной в правом верхнем углу каждого блока кода ниже.
- [установить последнюю версию Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.31 или выше), если вы предпочитаете использовать локальную консоль CLI. 

Ниже описаны действия по созданию веб-приложения и присвоению удостоверения ему с помощью CLI:

1. Если вы используете Azure CLI в локальной консоли, сначала выполните вход в Azure с помощью команды [az login](/cli/azure/reference-index#az-login). Используйте учетную запись, связанную с подпиской Azure, в которой вы хотите развернуть приложение:

    ```azurecli-interactive
    az login
    ```

2. Создайте веб-приложение с помощью CLI. Дополнительные примеры использования CLI со службой приложений см. в статье [Примеры Azure CLI](../app-service/samples-cli.md).

    ```azurecli-interactive
    az group create --name myResourceGroup --location westus
    az appservice plan create --name myPlan --resource-group myResourceGroup --sku S1
    az webapp create --name myApp --resource-group myResourceGroup --plan myPlan
    ```

3. Выполните команду `identity assign`, чтобы создать удостоверение для этого приложения.

    ```azurecli-interactive
    az webapp identity assign --name myApp --resource-group myResourceGroup
    ```

### <a name="using-azure-powershell"></a>Использование Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Ниже описаны действия по созданию веб-приложения и присвоению удостоверения ему с помощью Azure PowerShell:

1. При необходимости установите Azure PowerShell с помощью инструкций, приведенных в разделе [руководств по Azure PowerShell](/powershell/azure/overview), а затем `Login-AzAccount` выполните команду, чтобы создать подключение к Azure.

2. Создайте веб-приложение с помощью Azure PowerShell. Дополнительные примеры применения Azure PowerShell со службой приложений см. в статье [Примеры сценариев Azure PowerShell](../app-service/samples-powershell.md).

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzResourceGroup -Name myResourceGroup -Location $location

    # Create an App Service plan in Free tier.
    New-AzAppServicePlan -Name $webappname -Location $location -ResourceGroupName myResourceGroup -Tier Free

    # Create a web app.
    New-AzWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName myResourceGroup
    ```

3. Выполните команду `Set-AzWebApp -AssignIdentity`, чтобы создать удостоверение для этого приложения.

    ```azurepowershell-interactive
    Set-AzWebApp -AssignIdentity $true -Name $webappname -ResourceGroupName myResourceGroup 
    ```

### <a name="using-an-azure-resource-manager-template"></a>Использование шаблона Azure Resource Manager

Шаблон Azure Resource Manager можно использовать для автоматизации развертывания ресурсов Azure. Дополнительные сведения о развертывании в службе приложений и Функциях см. в статьях [Предсказуемые подготовка и развертывание микрослужб в Azure](../app-service/deploy-complex-application-predictably.md) и [Автоматизация развертывания ресурсов приложения-функции для службы "Функции Azure"](../azure-functions/functions-infrastructure-as-code.md).

Любой ресурс типа `Microsoft.Web/sites` можно создать с помощью удостоверения, добавив следующее свойство в определение ресурса:

```json
"identity": {
    "type": "SystemAssigned"
}
```

> [!NOTE]
> Приложение может иметь назначаемое системой и назначаемое пользователем удостоверения одновременно. В этом случае для свойства `type` будет задано значение `SystemAssigned,UserAssigned`.

При добавлении назначаемого системой удостоверения Azure создает удостоверение для приложения и управляет им.

Например, веб-приложение может выглядеть следующим образом:

```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
    ]
}
```

Созданный сайт будет иметь следующие дополнительные свойства:

```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Свойство tenantId определяет, к какому клиенту Azure AD принадлежит удостоверение. principalId — это уникальный идентификатор для нового удостоверения приложения. В Azure AD субъект-служба имеет то же имя, которое вы присвоили службе приложений или экземпляру функций Azure.

## <a name="add-a-user-assigned-identity"></a>Добавление удостоверения, назначенного пользователем

Для создания приложения с назначаемым пользователем удостоверением необходимо создать удостоверение, а затем добавить его идентификатор ресурса в конфигурацию приложения.

### <a name="using-the-azure-portal"></a>Использование портала Azure

Сначала необходимо создать ресурс назначаемого пользователем удостоверения.

1. Создайте ресурс назначаемого пользователем управляемого удостоверения в соответствии с [этими инструкциями](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

2. Создайте приложение на портале обычным образом. Перейдите к нему на портале.

3. Если используется приложение-функция, перейдите к **функциям платформы**. Для других типов приложений прокрутите вниз до группы **параметров** в левой области навигации.

4. Выберите **удостоверение**.

5. На вкладке **Назначенные пользователи** нажмите кнопку **Добавить**.

6. Найдите созданное ранее удостоверение и выберите его. Нажмите кнопку **Добавить**.

    ![Управляемое удостоверение в Службе приложений](media/app-service-managed-service-identity/user-assigned-managed-identity-in-azure-portal.png)

### <a name="using-an-azure-resource-manager-template"></a>Использование шаблона Azure Resource Manager

Шаблон Azure Resource Manager можно использовать для автоматизации развертывания ресурсов Azure. Дополнительные сведения о развертывании в службе приложений и Функциях см. в статьях [Предсказуемые подготовка и развертывание микрослужб в Azure](../app-service/deploy-complex-application-predictably.md) и [Автоматизация развертывания ресурсов приложения-функции для службы "Функции Azure"](../azure-functions/functions-infrastructure-as-code.md).

Любой ресурс типа `Microsoft.Web/sites` можно создать с помощью удостоверения, добавив следующий блок в определение ресурса, заменив `<RESOURCEID>` идентификатором ресурса необходимого удостоверения:

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {}
    }
}
```

> [!NOTE]
> Приложение может иметь назначаемое системой и назначаемое пользователем удостоверения одновременно. В этом случае для свойства `type` будет задано значение `SystemAssigned,UserAssigned`.

Добавление назначенного пользователем типа указывает Azure использовать назначенное пользователем удостоверение, указанное для вашего приложения.

Например, веб-приложение может выглядеть следующим образом:

```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]": {}
        }
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
    ]
}
```

Созданный сайт будет иметь следующие дополнительные свойства:

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {
            "principalId": "<PRINCIPALID>",
            "clientId": "<CLIENTID>"
        }
    }
}
```

PrincipalId — это уникальный идентификатор для удостоверения, используемого для администрирования Azure AD. ClientId — это уникальный идентификатор для нового удостоверения приложения, который используется для указания того, какое удостоверение использовать во время вызовов среды выполнения.

## <a name="obtain-tokens-for-azure-resources"></a>Получение маркеров для ресурсов Azure

Приложение может использовать управляемое удостоверение для получения маркеров для доступа к другим ресурсам, защищенным Azure AD, например Azure Key Vault. Эти маркеры представляют приложение, получающее доступ к ресурсам, а не конкретного пользователя приложения. 

Чтобы разрешить доступ из приложения, необходимо настроить целевой ресурс. Например, если вы запрашиваете маркер для доступа к Key Vault, необходимо добавить политику доступа, включающую в себя удостоверение приложения. В противном случае вызовы Key Vault будут отклонены, даже если они включают маркеры. Чтобы узнать больше о том, какие ресурсы поддерживают маркеры Azure Active Directory, см. сведения в разделе о [службах Azure, поддерживающих аутентификацию Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

> [!IMPORTANT]
> Серверные службы для управляемых удостоверений поддерживают кэш для URI каждого ресурса около 8 часов. Если вы обновляете политику доступа к определенному целевому ресурсу и сразу получаете маркер для этого ресурса, вы можете продолжать получать кэшированный маркер с устаревшими разрешениями до истечения срока действия этого маркера. В настоящее время невозможно принудительно обновить маркер.

Существует простой протокол REST для получения маркера в службе приложений и Функциях Azure. Его можно использовать для всех приложений и языков. Для .NET и Java пакет Azure SDK предоставляет абстракцию по этому протоколу и упрощает процесс разработки в локальной среде.

### <a name="using-the-rest-protocol"></a>Использование протокола REST

Приложение с управляемым удостоверением содержит две заданные переменные среды:

- IDENTITY_ENDPOINT — URL-адрес локальной службы маркеров.
- IDENTITY_HEADER — заголовок, используемый для устранения атак с подделкой запросов на стороне сервера (ССРФ). Это значение меняется платформой.

**IDENTITY_ENDPOINT** — это локальный URL-адрес, по которому приложение может запрашивать маркеры. Чтобы получить маркер для ресурса, отправьте запрос HTTP GET к этой конечной точке, задав следующие параметры:

> | Имя параметра    | В     | Описание                                                                                                                                                                                                                                                                                                                                |
> |-------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | resource          | query  | URI ресурса Azure AD ресурса, для которого должен быть получен маркер. Это может быть URI одной из [служб Azure, которая поддерживает аутентификацию Azure AD,](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) или любой другой URI ресурса.    |
> | api-version       | query  | Версия API маркеров, которая будет использоваться. Используйте "2019-08-01" или более позднюю версию.                                                                                                                                                                                                                                                                 |
> | ЗАГОЛОВОК X-IDENTITY- | Заголовок | Значение переменной среды IDENTITY_HEADER. Заголовок, который используется при устранении атак с подделкой серверных запросов (SSRF).                                                                                                                                                                                                    |
> | client_id         | query  | Используемых Идентификатор клиента назначаемого пользователем удостоверения. Не может использоваться для запроса, включающего `principal_id`, `mi_res_id`или `object_id`. Если пропущены все параметры`client_id`ID `principal_id`( `object_id`,, `mi_res_id`и), используется назначенное системой удостоверение.                                             |
> | principal_id      | query  | Используемых ИДЕНТИФИКАТОР субъекта назначаемого пользователем удостоверения. `object_id`— это псевдоним, который можно использовать вместо него. Не может использоваться для запроса, который включает client_id, mi_res_id или object_id. Если пропущены все параметры`client_id`ID `principal_id`( `object_id`,, `mi_res_id`и), используется назначенное системой удостоверение. |
> | mi_res_id         | query  | Используемых Идентификатор ресурса Azure назначаемого пользователем удостоверения. Не может использоваться для запроса, включающего `principal_id`, `client_id`или `object_id`. Если пропущены все параметры`client_id`ID `principal_id`( `object_id`,, `mi_res_id`и), используется назначенное системой удостоверение.                                      |

> [!IMPORTANT]
> При попытке получить маркеры для назначенных пользователю удостоверений необходимо включить одно из дополнительных свойств. В противном случае служба токенов попытается получить маркер для назначенного системой удостоверения, которое может быть или не существует.

Успешный ответ 200 — OK включает текст JSON со следующими свойствами:

> | Имя свойства. | Описание                                                                                                                                                                                                                                        |
> |---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | access_token  | Запрошенный маркер доступа. Вызывающая веб-служба может использовать этот маркер для проверки подлинности принимающей веб-службы.                                                                                                                               |
> | client_id     | Идентификатор клиента используемого удостоверения.                                                                                                                                                                                                       |
> | expires_on    | Период времени после истечения срока действия маркера доступа. Дата представляется как количество секунд с 1970-01-01T0:0:0Z в формате UTC (соответствует утверждению `exp` маркера).                                                                                |
> | not_before    | Период времени, когда маркер доступа вступает в силу и может быть принят. Дата представляется как количество секунд с 1970-01-01T0:0:0Z в формате UTC (соответствует утверждению `nbf` маркера).                                                      |
> | resource      | Ресурс, для которого был запрошен маркер доступа, соответствует параметру строки запроса `resource`.                                                                                                                               |
> | token_type    | Обозначает значение типа маркера. Единственный тип, поддерживаемый Azure AD — это Фбеарер. Дополнительные сведения о токенах носителя см. [в разделе инфраструктура авторизации OAuth 2,0: использование токена носителя (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |

Этот ответ аналогичен [ответу на запрос маркера доступа между службами Azure AD](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#service-to-service-access-token-response).

> [!NOTE]
> Старая версия этого протокола, использующая версию API "2017-09-01", использовала `secret` заголовок вместо `X-IDENTITY-HEADER` и принимала только `clientid` свойство для назначенного пользователю. Он также возвращает `expires_on` в формате метки времени. MSI_ENDPOINT можно использовать в качестве псевдонима для IDENTITY_ENDPOINT, а MSI_SECRET можно использовать в качестве псевдонима для IDENTITY_HEADER.

### <a name="rest-protocol-examples"></a>Примеры протокола REST

Пример запроса может выглядеть следующим образом:

```http
GET /MSI/token?resource=https://vault.azure.net&api-version=2019-08-01 HTTP/1.1
Host: localhost:4141
X-IDENTITY-HEADER: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```

Пример ответа может выглядеть следующим образом:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "access_token": "eyJ0eXAi…",
    "expires_on": "1586984735",
    "resource": "https://vault.azure.net",
    "token_type": "Bearer",
    "client_id": "5E29463D-71DA-4FE0-8E69-999B57DB23B0"
}
```

### <a name="code-examples"></a>Примеры кода

# <a name="net"></a>[.NET](#tab/dotnet)

> [!TIP]
> Для языков .NET можно также использовать пакет [Microsoft.Azure.Services.AppAuthentication](#asal), вместо того чтобы создавать этот запрос самостоятельно.

```csharp
private readonly HttpClient _client;
// ...
public async Task<HttpResponseMessage> GetToken(string resource)  {
    var request = new HttpRequestMessage(HttpMethod.Get, 
        String.Format("{0}/?resource={1}&api-version=2019-08-01", Environment.GetEnvironmentVariable("IDENTITY_ENDPOINT"), resource));
    request.Headers.Add("X-IDENTITY-HEADER", Environment.GetEnvironmentVariable("IDENTITY_HEADER"));
    return await _client.SendAsync(request);
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const rp = require('request-promise');
const getToken = function(resource, cb) {
    let options = {
        uri: `${process.env["IDENTITY_ENDPOINT"]}/?resource=${resource}&api-version=2019-08-01`,
        headers: {
            'X-IDENTITY-HEADER': process.env["IDENTITY_HEADER"]
        }
    };
    rp(options)
        .then(cb);
}
```

# <a name="python"></a>[Python](#tab/python)

```python
import os
import requests

identity_endpoint = os.environ["IDENTITY_ENDPOINT"]
identity_header = os.environ["IDENTITY_HEADER"]

def get_bearer_token(resource_uri):
    token_auth_uri = f"{identity_endpoint}?resource={resource_uri}&api-version=2019-08-01"
    head_msi = {'X-IDENTITY-HEADER':identity_header}

    resp = requests.get(token_auth_uri, headers=head_msi)
    access_token = resp.json()['access_token']

    return access_token
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:IDENTITY_ENDPOINT + "?resource=$resourceURI&api-version=2019-08-01"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"X-IDENTITY-HEADER"="$env:IDENTITY_HEADER"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```

---

### <a name="using-the-microsoftazureservicesappauthentication-library-for-net"></a><a name="asal"></a>Использование библиотеки Microsoft.Azure.Services.AppAuthentication для .NET

Самый простой способ для приложений и функций .NET работать с управляемыми удостоверениями заключается в использовании пакета Microsoft.Azure.Services.AppAuthentication. Эта библиотека также позволяет локально тестировать код на компьютере разработки с использованием учетной записи пользователя из Visual Studio, [Azure CLI](/cli/azure) или встроенной проверки подлинности Active Directory. Дополнительные сведения о параметрах локальной разработки с помощью этой библиотеки см. в [справочнике Microsoft.Azure.Services.AppAuthentication]. В этом разделе показано, как начать работу с библиотекой в коде.

1. Добавьте ссылки на пакет [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) и другие пакеты NuGet в приложение. В примерах ниже также используется [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

2. Добавьте приведенный ниже код в приложение, указав необходимый целевой ресурс. В этом примере показано два способа работы с Azure Key Vault:

    ```csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;
    // ...
    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
    // OR
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
    ```

Дополнительные сведения о пакете Microsoft.Azure.Services.AppAuthentication и операциях, которые он предоставляет, см. в [справочнике Microsoft.Azure.Services.AppAuthentication ] и [примерах службы приложений и хранилищах ключей с управляемым удостоверением службы .NET](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

### <a name="using-the-azure-sdk-for-java"></a>Использование пакета Azure SDK для Java

Для приложений и функций Java проще всего работать с управляемым удостоверением с помощью [пакета Azure SDK для Java](https://github.com/Azure/azure-sdk-for-java). В этом разделе показано, как начать работу с библиотекой в коде.

1. Добавьте ссылку на [библиотеку Azure SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure). Для проектов Maven этот фрагмент кода можно добавить в `dependencies` раздел файл POM проекта:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure</artifactId>
        <version>1.23.0</version>
    </dependency>
    ```

2. Используйте `AppServiceMSICredentials` объект для проверки подлинности. В этом примере показано, как можно использовать этот механизм для работы с Azure Key Vault.

    ```java
    import com.microsoft.azure.AzureEnvironment;
    import com.microsoft.azure.management.Azure;
    import com.microsoft.azure.management.keyvault.Vault
    //...
    Azure azure = Azure.authenticate(new AppServiceMSICredentials(AzureEnvironment.AZURE))
            .withSubscription(subscriptionId);
    Vault myKeyVault = azure.vaults().getByResourceGroup(resourceGroup, keyvaultName);

    ```


## <a name="remove-an-identity"></a><a name="remove"></a>Удаление удостоверения

Чтобы удалить назначаемое системой удостоверение, нужно отключить возможность с помощью портала, PowerShell или интерфейса командной строки точно так же, как и при его создании. Назначаемые пользователем удостоверения можно удалить по отдельности. Чтобы удалить все удостоверения, задайте для типа значение "нет" в [шаблоне ARM](#using-an-azure-resource-manager-template):

```json
"identity": {
    "type": "None"
}
```

Удаление удостоверения, назначенного системой таким образом, также приведет к его удалению из Azure AD. Назначенные системой удостоверения также автоматически удаляются из Azure AD при удалении ресурса приложения.

> [!NOTE]
> Также вы можете установить параметр приложения WEBSITE_DISABLE_MSI, который отключает локальную службу маркеров. Но при этом само удостоверение сохранится, а управляемое удостоверение будет отображаться как "включенное". По этой причине мы рекомендуем не использовать такой параметр.

## <a name="next-steps"></a>Дальнейшие шаги

> [!div class="nextstepaction"]
> [Безопасное подключение Базы данных Azure SQL с использованием управляемого удостоверения службы](app-service-web-tutorial-connect-msi.md)

[Справочник Microsoft.Azure.Services.AppAuthentication]: https://go.microsoft.com/fwlink/p/?linkid=862452
