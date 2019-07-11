---
title: Краткое руководство. Настройка и получение секрета из Azure Key Vault с помощью веб-приложения .NET | Документация Майкрософт
description: В этом кратком руководстве вы настроите и получите секрет из Azure Key Vault с помощью веб-приложения .NET.
services: key-vault
author: msmbaldwin
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 01/02/2019
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 4f9fff41e4b9043c271d656583fb8b9a11ff3a7a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052784"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-net-web-app"></a>Краткое руководство. Настройка и получение секрета из Azure Key Vault с помощью веб-приложения .NET

В этом кратком руководстве описано, как в веб-приложении Azure настроить чтение данных из Azure Key Vault с помощью управляемых удостоверений для ресурсов Azure. С помощью Key Vault можно защитить данные. Вы узнаете, как выполнять следующие задачи:

* Создать хранилище ключей.
* сохранение секрета в хранилище ключей;
* получение секрета из хранилища ключей;
* создание веб-приложения Azure;
* активация [управляемого удостоверения службы](../active-directory/managed-identities-azure-resources/overview.md) для веб-приложения;
* предоставление разрешений, необходимых веб-приложению для чтения данных из хранилища ключей.

Прежде чем мы продолжим, ознакомьтесь с разделом [Основные понятия](key-vault-whatis.md#basic-concepts) для Key Vault.

>[!NOTE]
>Key Vault — это центральный репозиторий для хранения секретов программным способом. Но чтобы воспользоваться возможностями хранилища ключей, приложения и пользователи должны сначала пройти в нем аутентификацию, т. е. предоставить секрет. В соответствии с рекомендациями по безопасности первый секрет должен периодически меняться. 
>
>Благодаря [управляемым удостоверениям служб для ресурсов Azure](../active-directory/managed-identities-azure-resources/overview.md) приложения, работающие в Azure, получают удостоверение, которым автоматически управляет Azure. Это помогает устранить *проблему введения секрета*, чтобы пользователи и приложения могли следовать рекомендациям и не беспокоиться об изменении первого секрета.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Предварительные требования

* Действия для ОС Windows.
  * [Visual Studio 2019](https://www.microsoft.com/net/download/windows) с указанными ниже рабочими нагрузками:
    * ASP.NET и веб-разработка.
    * Кроссплатформенная разработка .NET Core.
  * [Пакет SDK для .NET Core 2.1 или более поздней версии](https://www.microsoft.com/net/download/windows).

* Для компьютеров Mac.
  * Дополнительные сведения см. в статье [Новые возможности Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).

* Для всех платформ.
  * Git ([скачать](https://git-scm.com/downloads)).
  * Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) версии 2.0.4 или более поздней. Он доступен для Windows, Mac и Linux.

## <a name="log-in-to-azure"></a>Вход в Azure

Чтобы войти в Azure с помощью Azure CLI, введите следующую команду:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Создайте группу ресурсов с помощью команды [az group create](/cli/azure/group#az-group-create). Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими.

Выберите имя группы ресурсов для заполнителя.
В следующем примере создается группа ресурсов в регионе "Восточная часть США":

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

Созданная группа ресурсов будет использоваться далее в этой статье.

## <a name="create-a-key-vault"></a>Создайте хранилище ключей.

Теперь создайте хранилище ключей в группе ресурсов, созданной на предыдущем шаге. Введите следующие сведения:

* Имя хранилища ключей: это должна быть строка, состоящая из 3–24 символов, которые содержат: 0–9, a–z, A–Z и дефис (-).
* Имя группы ресурсов.
* Расположение. **Восточная часть США**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

На этом этапе любые операции в этом хранилище ключей может выполнять только учетная запись Azure.

## <a name="add-a-secret-to-the-key-vault"></a>Добавление секрета в хранилище ключей

Теперь мы добавим секрет, чтобы продемонстрировать этот процесс. Вы можете хранить здесь строки подключения SQL и прочие сведения, которые должны храниться безопасно, но быть доступны для приложения.

Введите следующие команды, чтобы создать секрет с именем **AppSecret** в хранилище ключей. Этот секрет будет хранить значение **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Чтобы просмотреть содержащееся в секрете значение в виде обычного текста, введите следующее:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Эта команда отображает секретные сведения, включая URI. Выполнив эти действия, вы сохраните в хранилище ключей секрет со значением URI. Запишите эти сведения. Они потребуются вам на следующем шаге.

## <a name="clone-the-repo"></a>Клонирование репозитория

Клонируйте репозиторий, чтобы создать локальную копию, в которой можно изменить источник. Выполните следующую команду:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>Открытие и изменение решения

Измените файл program.cs, чтобы запустить образец с конкретным именем хранилища ключей:

1. Перейдите в папку key-vault-dotnet-core-quickstart.
2. Откройте в Visual Studio 2019 файл key-vault-dotnet-core-quickstart.sln.
3. Откройте файл Program.cs и измените *YourKeyVaultName* заполнителя на имя хранилища ключей, созданного ранее.

Это решение использует библиотеки NuGet [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) и [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

## <a name="run-the-app"></a>Запуск приложения

В главном меню Visual Studio 2019 выберите **Отладка** > **Запуск без отладки**. Когда откроется браузер, перейдите в нем на страницу **About** (Сведения). Отображается значение **AppSecret**.

## <a name="publish-the-web-application-to-azure"></a>Публикация веб-приложения в Azure

Опубликуйте это приложение в Azure, чтобы увидеть его в качестве живого веб-приложения, а также убедиться, что вы можете получить значение секрета:

1. В Visual Studio выберите проект **key-vault-dotnet-core-quickstart**.
2. Выберите **Опубликовать** > **Начало**.
3. Создайте новую **службу приложений**, а затем выберите **Опубликовать**.
4. Измените имя приложения на **keyvaultdotnetcorequickstart**.
5. Нажмите кнопку **Создать**.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-a-managed-identity-for-the-web-app"></a>Включение управляемого удостоверения для веб-приложения

Azure Key Vault позволяет обеспечить безопасное хранение учетных данных, а также других ключей и секретов, но для их получения код должен выполнять аутентификацию в Key Vault. [Управляемые удостоверения для ресурсов Azure](../active-directory/managed-identities-azure-resources/overview.md) упрощают решение этой задачи, предоставляя службам Azure автоматически управляемое удостоверение в Azure Active Directory (Azure AD). Это удостоверение можно использовать для аутентификации в любой службе, которая поддерживает аутентификацию Azure AD, включая Key Vault, не храня какие-либо учетные данные в коде.

В Azure CLI выполните команду assign-identity, чтобы создать удостоверение для приложения:

   ```azurecli
   az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
   ```

>[!NOTE]
>Команда в данной процедуре выполняет те же действия, что и **включение** параметра **Identity / System assigned** (Назначаемое системой удостоверение) в свойствах веб-приложения на портале.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Назначение разрешений приложению для чтения секретов из Key Vault

Запишите выходные данные при публикации приложения в Azure. Они должны быть представлены в следующем формате.
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Затем выполните следующую команду, используя имя вашего хранилища ключей и значение **PrincipalId**:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

Теперь при запуске приложения должно отображаться извлеченное значение секрета. В указанной команде вы предоставляете удостоверению службы приложений разрешения на операции **get** и **list** в хранилище ключей.

## <a name="clean-up-resources"></a>Очистка ресурсов
Удалите группу ресурсов, виртуальную машину и все связанные ресурсы, если они вам больше не нужны. Для этого выберите группу ресурсов для хранилища ключей и щелкните **Удалить**.

Удалите хранилище ключей с помощью команды [az keyvault delete](https://docs.microsoft.com/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-delete).

```azurecli
az keyvault delete --name
                   [--resource-group]
                   [--subscription]
```

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Что такое хранилище ключей Azure?](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)
