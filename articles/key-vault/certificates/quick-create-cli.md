---
title: Краткое руководство. Настройка и просмотр сертификатов Azure Key Vault — Azure CLI
description: Краткое руководство по настройке и получению сертификата из Azure Key Vault с помощью Azure CLI
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019, devx-track-azurecli
ms.date: 09/03/2019
ms.author: mbaldwin
ms.openlocfilehash: 55a25f4f40668172aa8a9c80dd08d9fafcef78af
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94516482"
---
# <a name="quickstart-set-and-retrieve-a-certificate-from-azure-key-vault-using-azure-cli"></a>Краткое руководство. Настройка и получение сертификата из Azure Key Vault с помощью Azure CLI

Из этого краткого руководства вы узнаете, как создать хранилище ключей в Azure Key Vault с помощью Azure CLI. Azure Key Vault — это облачная служба, которая работает как защищенное хранилище секретов. Вы можете безопасно хранить ключи, пароли, сертификаты и другие секреты. Дополнительные сведения о хранилище ключей см. в статье [обзора](../general/overview.md). Azure CLI используется для создания ресурсов Azure и управления ими с помощью скриптов и команд. Создав ресурс, вы сможете сохранить в нем сертификат.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Для работы с этим кратким руководством требуется Azure CLI версии 2.0.4 или более поздней. Если вы используете Azure Cloud Shell, последняя версия уже установлена.

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов — это логический контейнер, в котором происходит развертывание ресурсов Azure и управление ими. В следующем примере создается группа ресурсов с именем *ContosoResourceGroup* в расположении *eastus*.

```azurecli
az group create --name "ContosoResourceGroup" --location eastus
```

## <a name="create-a-key-vault"></a>Создание хранилища ключей

Затем вы создадите Key Vault в группе ресурсов, созданной на предыдущем шаге. Необходимо будет указать следующие сведения:

- В этом руководстве мы используем **Contoso-vault2**. Укажите уникальное имя в тестировании.
- Имя группы ресурсов: **ContosoResourceGroup**.
- Расположение: **восточная часть США**.

```azurecli
az keyvault create --name "Contoso-Vault2" --resource-group "ContosoResourceGroup" --location eastus
```

В выходных данных командлета будут показаны свойства созданного Key Vault. Запишите значения двух указанных ниже свойств.

- **Имя хранилища.** В нашем примере это **Contoso-Vault2**. Вы будете использовать это имя для выполнения других команд Key Vault.
- **URI хранилища**. В нашем примере это https://contoso-vault2.vault.azure.net/. Необходимо, чтобы приложения, использующие ваше хранилище через REST API, использовали этот URI.

На этом этапе любые операции в этом хранилище ключей может выполнять только учетная запись Azure.

## <a name="add-a-certificate-to-key-vault"></a>Добавление сертификата в Key Vault

Чтобы добавить сертификат в хранилище, вам нужно выполнить несколько дополнительных действий. Этот сертификат может использоваться приложением. 

Выполните приведенные ниже команды, чтобы создать самозаверяющий сертификат с политикой по умолчанию — **ExampleCertificate**.

```azurecli
az keyvault certificate create --vault-name "Contoso-Vault2" -n ExampleCertificate -p "$(az keyvault certificate get-default-policy)"
```

Теперь сертификат, добавленный в Azure Key Vault, можно вызвать, используя его URI. Используйте **https://Contoso-Vault2.vault.azure.net/certificates/ExampleCertificate** , чтобы получить текущую версию. 

Чтобы просмотреть ранее сохраненный сертификат, выполните следующие действия:

```azurecli

az keyvault certificate show --name "ExampleCertificate" --vault-name "Contoso-Vault2"
```

Вы создали Key Vault, сохранили в нем сертификат и извлекли его.

## <a name="clean-up-resources"></a>Очистка ресурсов

Другие руководства в этой серии созданы на основе этого документа. Если вы планируете продолжить работу с последующими краткими руководствами и статьями, эти ресурсы можно не удалять.
Вы можете удалить ставшие ненужными группу ресурсов и все связанные с ней ресурсы, выполнив команду [az group delete](/cli/azure/group). Удалите ресурсы следующим образом:

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>Дальнейшие действия

Из этого краткого руководства вы узнали, как создать Key Vault и сохранить в нем сертификат. Дополнительные сведения о Key Vault и его интеграции в приложения см. в следующих статьях.

- [Обзор Azure Key Vault](../general/overview.md)
- Справочник по [командам az keyvault для Azure CLI](/cli/azure/keyvault?view=azure-cli-latest)
- [Рекомендации по Azure Key Vault](../general/best-practices.md)
