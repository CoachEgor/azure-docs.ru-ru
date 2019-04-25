---
title: Ограничение доступа к kubeconfig в Службе Azure Kubernetes (AKS)
description: Узнайте, как управлять доступом к файлу конфигурации Kubernetes (kubeconfig) для администраторов и пользователей кластера.
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 01/03/2019
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: 141aacc71d129bb45dc53774af876d5b07b7fc86
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466465"
---
# <a name="use-azure-role-based-access-controls-to-define-access-to-the-kubernetes-configuration-file-in-azure-kubernetes-service-aks"></a>Чтобы определить доступ к файлу конфигурации Kubernetes в службе Azure Kubernetes (AKS), используйте элементы управления доступом на основе ролей.

Вы можете взаимодействовать с кластерами Kubernetes с помощью инструмента `kubectl`. Azure CLI позволяет без труда получить учетные данные для доступа и сведения о конфигурации, чтобы подключиться к кластерам AKS с помощью `kubectl`. Чтобы ограничить доступ к сведениям о конфигурации Kubernetes (*kubeconfig*) и ограничить разрешения, можно использовать элементы управления доступом Azure на основе ролей (RBAC).

В этой статье показано, как назначить роли RBAC, чтобы ограничить доступ к сведениям о конфигурации для кластера AKS.

## <a name="before-you-begin"></a>Перед началом работы

В этой статье предполагается, что у вас есть кластер AKS. Если вам нужен кластер AKS, обратитесь к этому краткому руководству по работе с AKS [с помощью Azure CLI][aks-quickstart-cli] или [портала Azure][aks-quickstart-portal].

Для этой статьи требуется Azure CLI 2.0.53 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0][azure-cli-install].

## <a name="available-cluster-roles-permissions"></a>Доступные разрешения ролей кластера

При взаимодействии с кластером AKS с помощью инструмента `kubectl` используется файл конфигурации, который определяет сведения о подключении кластера. Этот файл конфигурации обычно хранится в расположении *~/.kube/config*. В файле *kubeconfig* можно определить несколько кластеров. Вы можете переключаться между кластерами с помощью команды [kubectl config use-context][kubectl-config-use-context].

Команда [az aks get-credentials][az-aks-get-credentials] позволяет получить учетные данные для доступа к кластеру AKS и объединить их в файл *kubeconfig*. Доступ к этим учетным данным также можно контролировать с помощью управления доступом Azure на основе ролей (RBAC). Эти роли Azure RBAC позволяют определить, кто может получить файл *kubeconfig* и какие он получит разрешения в пределах кластера.

Ниже описаны две встроенные роли:

* **Роль администратора кластера в Службе Azure Kubernetes**  
    * Разрешение доступа к вызову API *Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action*. Этот вызов API [позволяет перечислить учетные данные администратора кластера][api-cluster-admin].
    * Скачивание файла *kubeconfig* для роли *clusterAdmin*.
* **Роль пользователя кластера в Службе Azure Kubernetes**
    * Разрешение доступа к вызову API *Microsoft.ContainerService/managedClusters/listClusterUserCredential/action*. Этот вызов API [позволяет перечислить учетные данные пользователя кластера][api-cluster-user].
    * Скачивание файла *kubeconfig* для роли *clusterUser*.

## <a name="assign-role-permissions-to-a-user"></a>Назначение разрешений роли пользователю

Чтобы назначить пользователю одну из ролей Azure, необходимо получить идентификатор ресурса кластера AKS и идентификатор учетной записи пользователя. Команды в приведенном ниже примере позволяют выполнить следующие действия:

* Получить идентификатор ресурса кластера с помощью команды [az aks show][az-aks-show] для кластера с именем *myAKSCluster* в группе ресурсов *myResourceGroup*. При необходимости укажите имя вашей группы ресурсов и кластера.
* Получить идентификатор пользователя (с использованием команд [az account show][az-account-show] и [az ad user show][az-ad-user-show]).
* Назначить роль (с использованием команды [az role assignment create][az-role-assignment-create]).

В приведенном ниже примере назначаются *роли администратора кластера Службы Azure Kubernetes*:

```azurecli
# Get the resource ID of your AKS cluster
AKS_CLUSTER=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query id -o tsv)

# Get the account credentials for the logged in user
ACCOUNT_UPN=$(az account show --query user.name -o tsv)
ACCOUNT_ID=$(az ad user show --upn-or-object-id $ACCOUNT_UPN --query objectId -o tsv)

# Assign the 'Cluster Admin' role to the user
az role assignment create \
    --assignee $ACCOUNT_ID \
    --scope $AKS_CLUSTER \
    --role "Azure Kubernetes Service Cluster Admin Role"
```

При необходимости можно изменить предыдущее назначение на *роль пользователя кластера*.

В следующем примере выходных данных показано, что назначение ролей выполнено успешно:

```
{
  "canDelegate": null,
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/providers/Microsoft.Authorization/roleAssignments/b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "name": "b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "principalId": "946016dd-9362-4183-b17d-4c416d1f8f61",
  "resourceGroup": "myResourceGroup",
  "roleDefinitionId": "/subscriptions/<guid>/providers/Microsoft.Authorization/roleDefinitions/0ab01a8-8aac-4efd-b8c2-3ee1fb270be8",
  "scope": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-and-verify-the-configuration-information"></a>Получение и проверка сведений о конфигурации

Если роли RBAC назначены, получите определение *kubeconfig* для кластера AKS с помощью команды [az aks get-credentials][az-aks-get-credentials]. В приведенном ниже примере возвращаются учетные данные *--admin*, которые будут работать правильно при наличии *роли администратора кластера*:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Затем с помощью команды [kubectl config view][kubectl-config-view] можно проверить, показано ли в *context*, что применены сведения о конфигурации администратора:

```
$ kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://myaksclust-myresourcegroup-19da35-4839be06.hcp.chinaeast.azmk8s.io:443
  name: myAKSCluster
contexts:
- context:
    cluster: myAKSCluster
    user: clusterAdmin_myResourceGroup_myAKSCluster
  name: myAKSCluster-admin
current-context: myAKSCluster-admin
kind: Config
preferences: {}
users:
- name: clusterAdmin_myResourceGroup_myAKSCluster
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
    token: e9f2f819a4496538b02cefff94e61d35
```

## <a name="remove-role-permissions"></a>Удаление разрешений ролей

Чтобы удалить назначения роли, используйте команду [az role assignment delete][az-role-assignment-delete]. Укажите идентификатор учетной записи и идентификатор ресурса кластера, полученные с помощью предыдущих команд:

```azurecli
az role assignment delete --assignee $ACCOUNT_ID --scope $AKS_CLUSTER
```

## <a name="next-steps"></a>Дальнейшие действия

Чтобы повысить уровень безопасности при доступе к кластерам AKS, [интегрируйте аутентификацию Azure Active Directory][aad-integration].

<!-- LINKS - external -->
[kubectl-config-use-context]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config
[kubectl-config-view]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[azure-cli-install]: https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest
[az-aks-get-credentials]: https://docs.azure.cn/zh-cn/cli/aks?view=azure-cli-latest#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[api-cluster-admin]: https://docs.microsoft.com/rest/api/aks/managedclusters/listclusteradmincredentials
[api-cluster-user]: https://docs.microsoft.com/rest/api/aks/managedclusters/listclusterusercredentials
[az-aks-show]: https://docs.azure.cn/zh-cn/cli/aks?view=azure-cli-latest#az-aks-show
[az-account-show]: https://docs.azure.cn/zh-cn/cli/account?view=azure-cli-latest#az-account-show
[az-ad-user-show]: https://docs.azure.cn/zh-cn/cli/ad/user?view=azure-cli-latest#az-ad-user-show
[az-role-assignment-create]: https://docs.azure.cn/zh-cn/cli/role/assignment?view=azure-cli-latest#az-role-assignment-create
[az-role-assignment-delete]: https://docs.azure.cn/zh-cn/cli/role/assignment?view=azure-cli-latest#az-role-assignment-delete
[aad-integration]: aad-integration.md