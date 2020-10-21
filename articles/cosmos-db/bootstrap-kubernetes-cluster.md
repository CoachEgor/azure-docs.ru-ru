---
title: Использование Azure Kubernetes с Azure Cosmos DB
description: Сведения о начальной загрузке кластера Kubernetes в Azure, который использует Azure Cosmos DB (предварительная версия).
author: SnehaGunda
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/06/2019
ms.author: sngun
ms.openlocfilehash: dbfa780269f28e269fed51a25ec4255d18d8ab39
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2020
ms.locfileid: "92277886"
---
# <a name="how-to-use-azure-kubernetes-with-azure-cosmos-db-preview"></a>Использование Azure Kubernetes с Azure Cosmos DB (предварительная версия)

API etcd в Azure Cosmos DB позволяет использовать Azure Cosmos DB в качестве хранилища баз данных для Azure Kubernetes. Azure Cosmos DB реализует протокол проводной сети etcd, который позволяет серверам API основного узла использовать Azure Cosmos DB так же, как и к локально установленным etcd. API etcd в Azure Cosmos DB в настоящее время находится в режиме предварительной версии. Использование API etcd Azure Cosmos в качестве резервного хранилища для Kubernetes обеспечивает следующие преимущества: 

* Нет необходимости вручную настраивать etcd и управлять им.
* Высокий уровень доступности etcd, гарантированный Cosmos (99,99 % — в одном регионе, 99,999 % — в нескольких регионах).
* Эластичное масштабирование etcd.
* Обеспечение безопасности по умолчанию; готово к внедрению на предприятии.
* Лучшие в отрасли подробные Соглашения об уровне обслуживания.

Дополнительные сведения об API etcd в Azure Cosmos DB см. в [этой обзорной статье](etcd-api-introduction.md). В этой статье содержатся сведения об использовании [Azure Kubernetes Engine](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md) (aks-engine) для начальной загрузки кластера Kubernetes в Azure, который использует [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) вместо локально установленного и настроенного хранилища etcd. 

## <a name="prerequisites"></a>предварительные требования

1. Установите [последнюю версию](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) Azure CLI. Вы можете скачать и установить Azure CLI, соответствующий вашей операционной системе.

1. Установите [последнюю версию](https://github.com/Azure/aks-engine/releases) Azure Kubernetes Engine. Инструкции по установке для различных операционных систем см. на странице [Azure Kubernetes Engine](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md#install-aks-engine). Вам потребуются действия, описанные в разделе **Установка ядра AKS** в связанном документе. После загрузки Извлеките ZIP-файл.

   Модуль Azure Kubernetes (**aks-engine**) создает шаблоны Azure Resource Manager для кластеров Kubernetes в Azure. Входными данными для aks-engine является файл определения кластера, который содержит описание желаемого кластера, включая оркестратор, функции и агенты. Структура входных файлов аналогична общедоступному API для Azure Kubernetes Service.

1. API etcd в Azure Cosmos DB в настоящее время находится в режиме предварительной версии. Зарегистрируйтесь,перейдя по адресу https://aka.ms/cosmosetcdapi-signup, чтобы использовать предварительную версию. После отправки формы вашей подписке будет разрешено использовать API Azure Cosmos etcd. 

## <a name="deploy-the-cluster-with-azure-cosmos-db"></a>Развертывание кластера с помощью Azure Cosmos DB

1. Откройте окно командной строки и войдите в учетную запись Azure с помощью следующей команды:

   ```azurecli-interactive
   az login 
   ```

1. Если у вас несколько подписок, переключитесь на подписку, которая была разрешена для Azure Cosmos DB API etcd. Вы можете переключиться на нужную подписку с помощью следующей команды:

   ```azurecli-interactive
   az account set --subscription "<Name of your subscription>"
   ```
1. Затем создайте группу ресурсов, в которой вы будете развертывать ресурсы, необходимые для кластера Azure Kubernetes. Создайте группу ресурсов в регионе centralus. Группа ресурсов не обязательно должна находиться в регионе centralus, однако API etcd Azure Cosmos в настоящее время доступен для развертывания только в этом регионе. Поэтому лучше всего связать кластер Kubernetes с экземпляром etcd Cosmos.

   ```azurecli-interactive
   az group create --name <Name> --location "centralus"
   ```

1. Затем создайте субъект-службу для кластера Azure Kubernetes, чтобы он мог взаимодействовать с ресурсами, содержащимися в той же группе ресурсов. Вы можете создать субъект-службу с помощью Azure CLI, PowerShell или портала Azure. В этом примере он создается с помощью Azure CLI.

   ```azurecli-interactive
   az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<Your_Azure_subscription_ID>/resourceGroups/<Your_resource_group_name>"
   ```
   Эта команда выводит сведения о субъект-службе, например:
   
   ```cmd
   Retrying role assignment creation: 1/36
   {
     "appId": "8415a4e9-4f83-46ca-a704-107457b2e3ab",
     "displayName": "azure-cli-2019-04-19-19-01-46",
     "name": "http://azure-cli-2019-04-19-19-01-46",
     "password": "102aecd3-5e37-4f3d-8738-2ac348c2e6a7",
     "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
   }
   ```
   
   Запишите значения, указанные в полях **appId** и **password**, так как вы будете использовать эти параметры на следующих шагах. 

1. В командной строке перейдите в папку, содержащую исполняемый файл Azure Kubernetes Engine. Например, в командной строке вы можете перейти в следующую папку:

   ```cmd
   cd "\aks-engine-v0.36.3-windows-amd64\aks-engine-v0.36.3-windows-amd64"
   ```

1. Откройте любой текстовый редактор и определите шаблон Resource Manager, который развертывает кластер Azure Kubernetes с помощью API etcd Azure Cosmos DB. Скопируйте следующее определение JSON в текстовый редактор и сохраните файл как `apiModel.json`:

   ```json

   {
    "apiVersion": "vlabs",
    "properties": {
        "orchestratorProfile": {
            "orchestratorType": "Kubernetes",
            "kubernetesConfig": {
                "useManagedIdentity": false
            }
        },
        "masterProfile": {
            "count": 1,
            "dnsPrefix": "",
            "vmSize": "Standard_D2_v3",
            "cosmosEtcd": true
        },
        "agentPoolProfiles": [
            {
                "name": "agent",
                "count": 1,
                "vmSize": "Standard_D2_v3",
                "availabilityProfile": "AvailabilitySet"
            }
        ],
        "linuxProfile": {
            "adminUsername": "azureuser",
            "ssh": {
                "publicKeys": [
                    {
                        "keyData": ""
                    }
                ]
            }
        }
    }
   }
   ```

   В файле определения JSON или кластера ключевым параметром является **"cosmosEtcd": true**. Запишите его. Этот параметр находится в свойствах masterProfile. Он указывает на развертывание для использования API etcd Azure Cosmos вместо обычного хранилища etcd. 

1. Разверните кластер Azure Kubernetes, использующий Azure Cosmos DB, выполнив следующую команду:

   ```cmd
   aks-engine deploy \
     --subscription-id <Your_Azure_subscription_ID> \
     --client-id <Service_ principal_appId> \
     --client-secret <Service_ principal_password> \
     --dns-prefix <Region_unique_dns_name > \
     --location centralus \
     --resource-group <Resource_Group_Name> \
     --api-model <Fully_qualified_path_to_the_template_file>  \
     --force-overwrite
   ```

   Azure Kubernetes Engine использует определение кластера, в котором указаны желаемая форма, размер и конфигурация Azure Kubernetes. С помощью определения кластера можно включить ряд функций. В этом примере вы будете использовать следующие параметры:

   * **идентификатор подписки:** Идентификатор подписки Azure, в которой включен Azure Cosmos DB API etcd.
   * **идентификатор клиента:** AppId субъекта-службы. Идентификатор `appId` возвращен в качестве выходных данных на шаге 4.
   * **Секрет клиента:** Пароль субъекта-службы или пароль, созданный случайным образом. Это значение было возвращено в качестве выходных данных в параметре password на шаге 4. 
   * **dnsPrefix:** Уникальное для региона DNS-имя. Это значение будет частью имени узла (например, myprod1, staging).
   * **Расположение:**  Расположение, в котором должен быть развернут кластер, в настоящее время поддерживается только "centralus".

   > [!Note]
   > В настоящее время API etcd Azure Cosmos доступен для развертывания только в регионе centralus. 
 
   * **модель API:** Полный путь к файлу шаблона.
   * **принудительно перезаписать:** Этот параметр используется для автоматической перезаписи существующих файлов в выходном каталоге.
 
   В следующей команде представлен пример развертывания:

   ```cmd
   aks-engine deploy \
     --subscription-id 1234fc61-1234-1234-1234-be1234d12c1b \
     --client-id 1234a4e9-4f83-46ca-a704-107457b2e3ab \
     --client-secret 123aecd3-5e37-4f3d-8738-2ac348c2e6a7 \
     --dns-prefix aks-sg-test \
     --location centralus \
     --api-model "C:\Users\demouser\Downloads\apiModel.json"  \
     --force-overwrite
   ```

## <a name="verify-the-deployment"></a>Проверка развертывания

Развертывание шаблона занимает несколько минут. После успешного развертывания в командной строке отобразятся следующие выходные данные:

```cmd
WARN[0006] apimodel: missing masterProfile.dnsPrefix will use "aks-sg-test"
WARN[0006] --resource-group was not specified. Using the DNS prefix from the apimodel as the resource group name: aks-sg-test
INFO[0025] Starting ARM Deployment (aks-sg-test-546247491). This will take some time...
INFO[0587] Finished ARM Deployment (aks-sg-test-546247491). Succeeded
```

Группа ресурсов теперь содержит следующие ресурсы: виртуальная машина, учетная запись Azure Cosmos (API etcd), виртуальная сеть, группа доступности и другие ресурсы, необходимые для кластера Kubernetes. 

Имя учетной записи Azure Cosmos будет соответствовать указанному префиксу DNS с добавлением k8s. Вашей учетной записи Azure Cosmos будет автоматически предоставлена база данных **EtcdDB** и контейнер **EtcdData**. В контейнере будут храниться все данные, связанные с etcd. Контейнеру предоставляется определенное количество единиц запроса, и вы сможете [масштабировать (увеличивать/уменьшать) пропускную способность](scaling-throughput.md) на основе рабочей нагрузки. 

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь со сведениями о [работе с базами данных, контейнерами и элементами Azure Cosmos](account-databases-containers-items.md).
* Ознакомьтесь со сведениями об [оптимизации затрат на подготовленную пропускную способность](optimize-cost-throughput.md).

