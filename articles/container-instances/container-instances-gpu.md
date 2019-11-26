---
title: Deploy GPU-enabled container instance
description: Learn how to deploy Azure container instances to run compute-intensive container apps using GPU resources.
ms.topic: article
ms.date: 04/17/2019
ms.openlocfilehash: ea3b0ccba2d84487356f4bbd404cec3af1d0979a
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2019
ms.locfileid: "74484181"
---
# <a name="deploy-container-instances-that-use-gpu-resources"></a>Развертывание экземпляров контейнеров, использующих ресурсы GPU

Чтобы выполнить определенные ресурсоемкие рабочие нагрузки в Экземплярах контейнеров Azure, разверните [группы контейнеров](container-instances-container-groups.md) с *ресурсами GPU*. Экземпляры контейнеров в группе могут получить доступ к одному графическому процессору NVIDIA Tesla или нескольким при выполнении рабочих нагрузок контейнеров, таких как CUDA и приложения глубокого обучения.

This article shows how to add GPU resources when you deploy a container group by using a [YAML file](container-instances-multi-container-yaml.md) or [Resource Manager template](container-instances-multi-container-group.md). You can also specify GPU resources when you deploy a container instance using the Azure portal.

> [!IMPORTANT]
> Сейчас эта функция доступна в предварительной версии, и применяются [некоторые ограничения](#preview-limitations). Предварительные версии предоставляются при условии, что вы принимаете [дополнительные условия использования][terms-of-use]. Некоторые аспекты этой функции могут быть изменены до выхода общедоступной версии.

## <a name="preview-limitations"></a>Ограничения предварительной версии

В режиме предварительной версии при использовании ресурсов GPU в группах контейнеров применяются следующие ограничения. 

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]

Со временем будут поддерживаться другие регионы.

**Supported OS types**: Linux only

**Additional limitations**: GPU resources can't be used when deploying a container group into a [virtual network](container-instances-vnet.md).

## <a name="about-gpu-resources"></a>Сведения о ресурсах GPU

### <a name="count-and-sku"></a>Количество и номер SKU

Чтобы использовать GPU в экземпляре контейнера, укажите *ресурс GPU* со следующими сведениями:

* **Count** - The number of GPUs: **1**, **2**, or **4**.
* **SKU** - The GPU SKU: **K80**, **P100**, or **V100**. Каждый номер SKU сопоставляется с графическим процессором NVIDIA Tesla в одном из следующих семейств виртуальных машин с поддержкой GPU Azure:

  | SKU | Семейство виртуальных машин |
  | --- | --- |
  | K80 | [NC](../virtual-machines/linux/sizes-gpu.md#nc-series) |
  | P100 | [NCv2](../virtual-machines/linux/sizes-gpu.md#ncv2-series) |
  | V100 | [NCv3](../virtual-machines/linux/sizes-gpu.md#ncv3-series) |

[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

When deploying GPU resources, set CPU and memory resources appropriate for the workload, up to the maximum values shown in the preceding table. These values are currently larger than the CPU and memory resources available in container groups without GPU resources.  

### <a name="things-to-know"></a>Полезная информация

* **Время развертывания**. Создание группы контейнеров, содержащей ресурсы GPU, занимает **8–10 минут**. Это связано с затратой времени на подготовку и настройку виртуальной машины с поддержкой GPU в Azure. 

* **Цены**. Аналогично группам контейнеров без ресурсов GPU, Azure выставляет счета за ресурсы, потребляемые в течение *длительности использования* группы контейнеров с ресурсами GPU. Длительность использования рассчитывается с момента извлечения первого образа контейнера до момента завершения работы группы контейнеров. Она не включает время, затрачиваемое на развертывание группы контейнеров.

  См. сведения о [ценах](https://azure.microsoft.com/pricing/details/container-instances/).

* **Драйверы CUDA**. Экземпляры контейнеров с ресурсами GPU предварительно снабжены драйверами NVIDIA CUDA и средами выполнения контейнеров, поэтому вы можете использовать образы контейнеров, разработанные для рабочих нагрузок CUDA.

  We support CUDA 9.0 at this stage. For example, you can use following base images for your Docker file:
  * [nvidia/cuda:9.0-base-ubuntu16.04](https://hub.docker.com/r/nvidia/cuda/)
  * [tensorflow/tensorflow: 1.12.0-gpu-py3](https://hub.docker.com/r/tensorflow/tensorflow)
    
## <a name="yaml-example"></a>Пример YAML

Чтобы добавить группы GPU, можно развернуть группу контейнеров с помощью [YAML-файла](container-instances-multi-container-yaml.md). Скопируйте следующий YAML-файл в новый файл с именем *gpu-deploy-aci.yaml*, а затем сохраните его. Этот YAML-файл создает группу контейнеров с именем *gpucontainergroup*, в которой указывается экземпляр контейнера с графическим процессором K80. Экземпляр запускает пример приложения сложения векторов CUDA. Запросов ресурсов достаточно для запуска рабочей нагрузки.

```YAML
additional_properties: {}
apiVersion: '2018-10-01'
name: gpucontainergroup
properties:
  containers:
  - name: gpucontainer
    properties:
      image: k8s-gcrio.azureedge.net/cuda-vector-add:v0.1
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
          gpu:
            count: 1
            sku: K80
  osType: Linux
  restartPolicy: OnFailure
```

Deploy the container group with the [az container create][az-container-create] command, specifying the YAML file name for the `--file` parameter. Необходимо указать имя группы ресурсов и расположение группы контейнеров, которое поддерживает ресурсы GPU, например *eastus*.  

```azurecli
az container create --resource-group myResourceGroup --file gpu-deploy-aci.yaml --location eastus
```

Развертывание занимает несколько минут. Затем контейнер запускается и выполняет операцию сложения векторов CUDA. Run the [az container logs][az-container-logs] command to view the log output:

```azurecli
az container logs --resource-group myResourceGroup --name gpucontainergroup --container-name gpucontainer
```

Выходные данные:

```Console
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```

## <a name="resource-manager-template-example"></a>Пример шаблона Resource Manager

Чтобы развернуть группу контейнеров с ресурсами GPU, также можно воспользоваться [шаблоном Resource Manager](container-instances-multi-container-group.md). Начните с создания файла с именем `gpudeploy.json` и скопируйте в него приведенный ниже код JSON. This example deploys a container instance with a V100 GPU that runs a [TensorFlow](https://www.tensorflow.org/) training job against the MNIST dataset. Запросов ресурсов достаточно для запуска рабочей нагрузки.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "containerGroupName": {
        "type": "string",
        "defaultValue": "gpucontainergrouprm",
        "metadata": {
          "description": "Container Group name."
        }
      }
    },
    "variables": {
      "containername": "gpucontainer",
      "containerimage": "microsoft/samples-tf-mnist-demo:gpu"
    },
    "resources": [
      {
        "name": "[parameters('containerGroupName')]",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2018-10-01",
        "location": "[resourceGroup().location]",
        "properties": {
            "containers": [
            {
              "name": "[variables('containername')]",
              "properties": {
                "image": "[variables('containerimage')]",
                "resources": {
                  "requests": {
                    "cpu": 4.0,
                    "memoryInGb": 12.0,
                    "gpu": {
                        "count": 1,
                        "sku": "V100"
                  }
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "restartPolicy": "OnFailure"
        }
      }
    ]
}
```

Разверните шаблон с помощью команды [az group deployment create][az-group-deployment-create]. Необходимо указать имя группы ресурсов, созданной в регионе с поддержкой ресурсов GPU, например *eastus*.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file gpudeploy.json
```

Развертывание занимает несколько минут. Затем контейнер запустится и выполнит задание TensorFlow. Run the [az container logs][az-container-logs] command to view the log output:

```azurecli
az container logs --resource-group myResourceGroup --name gpucontainergrouprm --container-name gpucontainer
```

Выходные данные:

```Console
2018-10-25 18:31:10.155010: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2018-10-25 18:31:10.305937: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: ccb6:00:00.0
totalMemory: 11.92GiB freeMemory: 11.85GiB
2018-10-25 18:31:10.305981: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: ccb6:00:00.0, compute capability: 3.7)
2018-10-25 18:31:14.941723: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.097
Accuracy at step 10: 0.6993
Accuracy at step 20: 0.8208
Accuracy at step 30: 0.8594
...
Accuracy at step 990: 0.969
Adding run metadata for 999
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Так как использование ресурсов GPU может быть дорогостоящим, следите, чтобы ваши контейнеры не работали в течение длительного времени. Monitor your containers in the Azure portal, or check the status of a container group with the [az container show][az-container-show] command. Пример.

```azurecli
az container show --resource-group myResourceGroup --name gpucontainergroup --output table
```

Когда завершите работу с созданными экземплярами контейнеров, удалите их с помощью следующих команд:

```azurecli
az container delete --resource-group myResourceGroup --name gpucontainergroup -y
az container delete --resource-group myResourceGroup --name gpucontainergrouprm -y
```

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь со сведениями о развертывании группы контейнеров с помощью [файла YAML](container-instances-multi-container-yaml.md) или [шаблона Resource Manager](container-instances-multi-container-group.md).
* Ознакомьтесь со статьей [Размеры виртуальных машин, оптимизированных для GPU](../virtual-machines/linux/sizes-gpu.md).


<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
