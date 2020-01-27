---
title: 'Руководство по Службе контейнеров Azure: развертывание приложения (не рекомендуется)'
description: 'Руководство по Службе контейнеров Azure: развертывание приложения'
author: iainfoulds
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 7e10fd982c19e45be8c5da4ffc7f7248276352c1
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2020
ms.locfileid: "76275490"
---
# <a name="deprecated-run-applications-in-kubernetes"></a>Запуск приложений в Kubernetes (не рекомендуется)

> [!TIP]
> Обновленная версия этого руководства, в котором используется служба Azure Kubernetes — [Руководство. Запуск приложений в службе Azure Kubernetes (AKS)](../../aks/tutorial-kubernetes-deploy-application.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

В этом руководстве (часть 4 из 7) выполняется развертывание примера приложения в кластер Kubernetes. В частности, рассматриваются такие шаги:

> [!div class="checklist"]
> * обновление файлов манифестов Kubernetes;
> * выполнение приложения в Kubernetes;
> * Тестирование приложения

В последующих руководствах описывается, как развернуть и обновить приложение, а также настроить Log Analytics для мониторинга кластера Kubernetes.

Для работы с этим руководством требуется понимание основных концепций Kubernetes. Подробные сведения см. в [документации по Kubernetes](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Перед началом

В предыдущих руководствах приложение упаковывалось в образ контейнера, далее этот образ отправлялся в реестр контейнеров Azure, после чего создавался кластер Kubernetes. 

Для работы с этим руководством необходимо предварительно создать файл манифеста Kubernetes `azure-vote-all-in-one-redis.yml`. Этот файл был скачан вместе с исходным кодом приложения в предыдущем руководстве. Проверьте, клонировали ли вы репозиторий и изменили ли каталоги на клонированный репозиторий.

Если вы не выполнили эти действия, вы можете ознакомиться со статьей [Создание образов контейнеров для использования со службой контейнеров Azure](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="update-manifest-file"></a>Обновление файла манифеста

В этом руководстве для хранения образа контейнера использовался реестр контейнеров Azure (ACR). Перед запуском приложения необходимо обновить имя сервера входа ACR в файле манифеста Kubernetes.

Получите имя сервера входа ACR, выполнив команду [az acr list](/cli/azure/acr#az-acr-list).

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Предварительно созданный файл манифеста содержит имя сервера входа `microsoft`. Откройте этот файл в любом текстовом редакторе. В этом примере файл открыт в `vi`.

```bash
vi azure-vote-all-in-one-redis.yml
```

Замените `microsoft` именем сервера входа ACR. Это значение можно найти в строке **47** файла манифеста.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Сохраните файл и закройте его.

## <a name="deploy-application"></a>Развертывание приложения

Используйте команду [kubectl create](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create), чтобы запустить приложение. Эта команда анализирует файл манифеста и создает заданные объекты Kubernetes.

```azurecli-interactive
kubectl create -f azure-vote-all-in-one-redis.yml
```

Выходные данные:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Тестирование приложения

Создается [служба Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/), которая открывает доступ к приложению через Интернет. Это может занять несколько минут. 

Чтобы отслеживать ход выполнения, используйте команду [kubectl get service](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) с аргументом `--watch`.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Изначально для параметра **EXTERNAL-IP** службы `azure-vote-front` отображается состояние `pending`. Как только для параметра "EXTERNAL-IP" состояние `pending` изменится на `IP address`, используйте команду `CTRL-C`, чтобы остановить процесс отслеживания kubectl.

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

Чтобы увидеть приложение, откройте в браузере внешний IP-адрес.

![Схема кластера Kubernetes в Аzure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве приложение Vote Azure было развернуто в кластере Kubernetes Службы контейнеров Azure. Вам предстоят следующие задачи:  

> [!div class="checklist"]
> * скачивание файлов манифестов Kubernetes;
> * запуск приложения в Kubernetes;
> * тестирование приложения.

Перейдите к следующему руководству, чтобы узнать о масштабировании приложения Kubernetes и базовой инфраструктуры Kubernetes. 

> [!div class="nextstepaction"]
> [Масштабирование pod и инфраструктуры Kubernetes](./container-service-tutorial-kubernetes-scale.md)
