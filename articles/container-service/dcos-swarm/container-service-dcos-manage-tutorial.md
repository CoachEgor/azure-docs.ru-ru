---
title: Руководство по Службе контейнеров Azure — управление DC/OS (не рекомендуется)
description: Учебник по службе контейнеров Azure — управление DC/OS
author: iainfoulds
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 4212277dbdf29705152832f3830692b43b8d1297
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78402812"
---
# <a name="deprecated-azure-container-service-tutorial---manage-dcos"></a>Руководство по Службе контейнеров Azure — управление DC/OS (не рекомендуется)

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

DC/OS предоставляет распределенную платформу для запуска современных и контейнерных приложений. Служба контейнеров Azure упрощает и убыстряет подготовку кластера DC/OS для использования в рабочей среде. В этом кратком руководстве по началу работы описываются базовые шаги, необходимые для развертывания кластера DC/OS и запуска основной рабочей нагрузки.

> [!div class="checklist"]
> * Создание кластера DC/OS ACS
> * Подключение к кластеру
> * Установка интерфейса командной строки DC/OS
> * Развертывание приложения в кластере
> * Масштабирование приложения в кластере
> * Масштабирование узлов кластера DC/OS
> * Базовое управление DC/OS
> * Удаление кластера DC/OS

Для этого руководства требуется Azure CLI версии 2.0.4 или более поздней. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить обновление, см. статью [Установка Azure CLI]( /cli/azure/install-azure-cli). 

## <a name="create-dcos-cluster"></a>Создание кластера DC/OS

Сначала создайте группу ресурсов с помощью команды [az group create](/cli/azure/group#az-group-create). Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими. 

В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *westeurope*.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Затем создайте кластер DC/OS с помощью команды [az acs create](/cli/azure/acs#az-acs-create).

В следующем примере создается кластер DC/OS с именем *myDCOSCluster* и ключи SSH, если они еще не существуют. Чтобы использовать определенный набор ключей, используйте параметр `--ssh-key-value`.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

Через несколько минут выполнение команды завершается, и отображаются сведения о развертывании.

## <a name="connect-to-dcos-cluster"></a>Подключение к кластеру DC/OS

После создания кластера DC/OS доступ к нему возможен через туннель SSH. Выполните следующую команду, чтобы получить общедоступный IP-адрес главного кластера DC/OS. Этот IP-адрес сохраняется в переменной и будет использоваться на следующем шаге.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

Чтобы создать туннель SSH, выполните следующую команду и следуйте инструкциям на экране. Если порт 80 уже используется, команда завершится с ошибкой. Измените туннелированный порт на тот, который не используется, например на `85:localhost:80`. 

```console
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a>Установка интерфейса командной строки DC/OS

Установите интерфейс командной строки DC/OS с помощью команды [az acs dcos install-cli](/cli/azure/acs/dcos#az-acs-dcos-install-cli). Если вы используете Azure CloudShell, то интерфейс командной строки DC/OS уже установлен. При запуске интерфейса командной строки Azure в macOS или Linux может потребоваться выполнить эту команду с sudo.

```azurecli
az acs dcos install-cli
```

Прежде чем можно будет использовать интерфейс командной строки в кластере, необходимо настроить в нем туннель SSH. Для этого выполните следующую команду, при необходимости изменив порт.

```console
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Запуск приложения

Механизм планирования по умолчанию для кластера DC/OS ACS — Marathon. Marathon используется для запуска приложения и управления состоянием приложения в кластере DC/OS. Чтобы планировать приложение с помощью Marathon, создайте файл с именем **marathon app.json** и скопируйте в него следующее. 

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Выполните следующую команду для планирования работы приложения в кластере DC/OS.

```console
dcos marathon app add marathon-app.json
```

Чтобы просмотреть состояние развертывания для приложения, выполните следующую команду.

```console
dcos marathon app list
```

Когда значение столбца **TASKS** изменится с *0/1* на *1/1*, развертывание приложения завершено.

```output
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a>Масштабирование приложения Marathon

В предыдущем примере был создан один экземпляр приложения. Чтобы обновить это развертывание и создать три доступных экземпляра приложения, откройте файл **marathon-app.json** и измените значение свойства instance на 3.

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 3,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Обновите приложение с помощью команды `dcos marathon app update`.

```console
dcos marathon app update demo-app-private < marathon-app.json
```

Чтобы просмотреть состояние развертывания для приложения, выполните следующую команду.

```console
dcos marathon app list
```

Когда значение столбца **TASKS** изменится с *1/3* на *3/1*, развертывание приложения завершено.

```output
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a>Запуск приложения, доступного через Интернет

Кластер DC/OS ACS состоит из двух наборов узлов, один из которых открытый и доступен в Интернете, а другой закрытый и недоступен в Интернете. По умолчанию установлен набор закрытых узлов, который использовался в последнем примере.

Чтобы сделать приложение доступным в Интернете, разверните его в наборе общедоступных узлов. Для этого присвойте объекту `acceptedResourceRoles` значение `slave_public`.

Создайте файл с именем **nginx-public.json** и скопируйте в него следующее.

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

Выполните следующую команду для планирования работы приложения в кластере DC/OS.

```console
dcos marathon app add nginx-public.json
```

Получите общедоступный IP-адрес общедоступных агентов кластера DC/OS.

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

При переходе на этот адрес возвращается сайт NGINX по умолчанию.

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a>Масштабирование кластера DC/OS

В предыдущих примерах приложение масштабировалось до нескольких экземпляров. Инфраструктуру DC/OS также можно масштабировать для обеспечения большей или меньшей вычислительной мощности. Это делается с помощью команды [az acs scale](/cli/azure/acs#az-acs-scale). 

Чтобы просмотреть текущее число агентов DC/OS, используйте команду [az acs show](/cli/azure/acs#az-acs-show).

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

Чтобы увеличить число до 5, используйте команду [az acs scale](/cli/azure/acs#az-acs-scale). 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a>Удаление кластера DC/OS

Вы можете удалить ставшие ненужными группу ресурсов, кластер DC/OS и все связанные с ним ресурсы, выполнив команду [az group delete](/cli/azure/group#az-group-delete).

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Дальнейшие действия

В этом учебнике изучались базовые задачи управления DC/OS, включая следующие. 

> [!div class="checklist"]
> * Создание кластера DC/OS ACS
> * Подключение к кластеру
> * Установка интерфейса командной строки DC/OS
> * Развертывание приложения в кластере
> * Масштабирование приложения в кластере
> * Масштабирование узлов кластера DC/OS
> * Удаление кластера DC/OS

Перейдите к следующему учебнику, чтобы узнать о приложении балансировки нагрузки в DC/OS на платформе Azure. 

> [!div class="nextstepaction"]
> [Приложения балансировки нагрузки](container-service-load-balancing.md)
