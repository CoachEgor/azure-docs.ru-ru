---
title: Развертывание моделей с помощью пользовательского образа DOCKER
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать пользовательский базовый образ DOCKER при развертывании моделей Машинное обучение Azure. Хотя Машинное обучение Azure предоставляет базовый образ по умолчанию, можно также использовать собственный базовый образ.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 08/22/2019
ms.openlocfilehash: 5d828ab59f790bab1003f0ad73fc7be1b77410bb
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044888"
---
# <a name="deploy-a-model-using-a-custom-docker-base-image"></a>Развертывание модели с помощью пользовательского базового образа DOCKER
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Узнайте, как использовать пользовательский базовый образ DOCKER при развертывании обученных моделей с помощью Машинное обучение Azure.

При развертывании обученной модели для веб-службы или устройства IoT Edge создается пакет, содержащий веб-сервер для обработки входящих запросов.

Машинное обучение Azure предоставляет базовый образ DOCKER по умолчанию, поэтому вам не нужно беспокоиться о его создании. Можно также использовать Машинное обучение Azure __среды__ для выбора конкретного базового образа или использовать предоставленный вами пользовательский.

Базовый образ используется в качестве начальной точки при создании изображения для развертывания. Он предоставляет базовую операционную систему и компоненты. После этого в образ добавляются дополнительные компоненты, такие как модель, среда conda и другие ресурсы, перед развертыванием.

Как правило, Пользовательский базовый образ создается, если вы хотите использовать DOCKER для управления зависимостями, поддерживать более строгий контроль над версиями компонентов или экономить время при развертывании. Например, может потребоваться стандартизировать определенную версию Python, Conda или другого компонента. Может также потребоваться установить программное обеспечение, необходимое для модели, в котором процесс установки занимает много времени. Установка программного обеспечения при создании базового образа означает, что вам не нужно устанавливать его для каждого развертывания.

> [!IMPORTANT]
> При развертывании модели нельзя переопределять основные компоненты, такие как веб-сервер или компоненты IoT Edge. Эти компоненты предоставляют известную рабочую среду, которая тестируется и поддерживается корпорацией Майкрософт.

> [!WARNING]
> Корпорация Майкрософт может не суметь устранить неполадки, вызванные пользовательским образом. При возникновении проблем может быть предложено использовать образ по умолчанию или один из образов, которые корпорация Майкрософт предоставляет, чтобы узнать, связана ли проблема с вашим образом.

Этот документ разбит на две части:

* Создание пользовательского базового образа. предоставляет сведения для администраторов и DevOps о создании пользовательского образа и настройке проверки подлинности в реестре контейнеров Azure с помощью Azure CLI и Машинное обучение CLI.
* Развертывание модели с помощью пользовательского базового образа. предоставляет сведения для специалистов по обработке и анализу данных, а также инженеров DevOps/ML по использованию пользовательских образов при развертывании обученной модели из пакета SDK Python или интерфейса командной строки ML.

## <a name="prerequisites"></a>Технические условия

* Рабочая группа Машинное обучение Azure. Дополнительные сведения см. в статье [Создание рабочей области](how-to-manage-workspace.md) .
* [Пакет SDK для машинное обучение Azure](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py). 
* [Интерфейс командной строки Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Расширение CLI для машинное обучение Azure](reference-azure-machine-learning-cli.md).
* [Реестр контейнеров Azure](/azure/container-registry) или другой реестр DOCKER, доступный в Интернете.
* В действиях, описанных в этом документе, предполагается, что вы знакомы с созданием и использованием объекта __конфигурации вывода__ в рамках развертывания модели. Дополнительные сведения см. в подразделе «Подготовка к развертыванию» раздела о том, [где нужно развернуть и как](how-to-deploy-and-where.md#prepare-to-deploy).

## <a name="create-a-custom-base-image"></a>Создание пользовательского базового образа

В этом разделе предполагается, что вы используете реестр контейнеров Azure для хранения образов DOCKER. При планировании создания пользовательских образов для Машинное обучение Azure используйте следующий контрольный список:

* Будет ли использоваться реестр контейнеров Azure, созданный для Машинное обучение Azure рабочей области или автономного реестра контейнеров Azure?

    При использовании образов, хранящихся в __реестре контейнеров для рабочей области__, не нужно проходить проверку подлинности в реестре. Проверка подлинности обрабатывается рабочей областью.

    > [!WARNING]
    > Реестр контейнеров Azure для рабочей области __создается при первом обучении или развертывании модели__ с помощью рабочей области. Если вы создали новую рабочую область, но не прошли обучение или не создали модель, для этой рабочей области не будет существовать реестра контейнеров Azure.

    Сведения о получении имени реестра контейнеров Azure для рабочей области см. в разделе [Получение имени реестра](#getname) контейнера этой статьи.

    При использовании образов, хранящихся в __автономном реестре контейнеров__, необходимо настроить субъект-службу, имеющий по крайней мере доступ на чтение. Затем укажите идентификатор субъекта-службы (имя пользователя) и пароль для всех, кто использует образы из реестра. Исключением является то, что реестр контейнеров становится общедоступным.

    Сведения о создании закрытого реестра контейнеров Azure см. в разделе [создание закрытого реестра контейнеров](/azure/container-registry/container-registry-get-started-azure-cli).

    Сведения об использовании субъектов-служб с реестром контейнеров Azure см. в статье [Проверка подлинности реестра контейнеров Azure с помощью субъектов-служб](/azure/container-registry/container-registry-auth-service-principal).

* Реестр контейнеров Azure и сведения об образе. Укажите имя образа для любого пользователя, который должен его использовать. Например, образ с именем `myimage`, хранящийся в реестре с именем `myregistry`, упоминается как `myregistry.azurecr.io/myimage` при использовании изображения для развертывания модели.

* Требования к образу. Машинное обучение Azure поддерживает только образы DOCKER, которые предоставляют следующее программное обеспечение:

    * Ubuntu 16,04 или более поздней версии.
    * Conda 4.5. # или более поздней версии.
    * Python 3.5. # или 3.6. #.

<a id="getname"></a>

### <a name="get-container-registry-information"></a>Получение сведений о реестре контейнеров

В этом разделе вы узнаете, как получить имя реестра контейнеров Azure для рабочей области Машинное обучение Azure.

> [!WARNING]
> Реестр контейнеров Azure для рабочей области __создается при первом обучении или развертывании модели__ с помощью рабочей области. Если вы создали новую рабочую область, но не прошли обучение или не создали модель, для этой рабочей области не будет существовать реестра контейнеров Azure.

Если вы уже обучили или развернули модели с помощью Машинное обучение Azure, для рабочей области был создан реестр контейнеров. Чтобы найти имя этого реестра контейнеров, выполните следующие действия.

1. Откройте новую оболочку или командную строку и выполните следующую команду для проверки подлинности в подписке Azure:

    ```azurecli-interactive
    az login
    ```

    Следуйте инструкциям на экране, чтобы пройти проверку подлинности в подписке.

2. Используйте следующую команду, чтобы вывести реестр контейнеров для рабочей области. Замените `<myworkspace>` именем рабочей области Машинное обучение Azure. Замените `<resourcegroup>` группой ресурсов Azure, которая содержит рабочую область:

    ```azurecli-interactive
    az ml workspace show -w <myworkspace> -g <resourcegroup> --query containerRegistry
    ```

    [!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

    Эта команда возвращает следующую информацию:

    ```text
    /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ContainerRegistry/registries/<registry_name>
    ```

    Значение `<registry_name>` — это имя реестра контейнеров Azure для рабочей области.

### <a name="build-a-custom-base-image"></a>Создание пользовательского базового образа

В этом разделе приводится пошаговое руководство по созданию пользовательского образа DOCKER в реестре контейнеров Azure.

1. Создайте новый текстовый файл с именем `Dockerfile`и используйте в качестве содержимого следующий текст:

    ```text
    FROM ubuntu:16.04

    ARG CONDA_VERSION=4.5.12
    ARG PYTHON_VERSION=3.6

    ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
    ENV PATH /opt/miniconda/bin:$PATH

    RUN apt-get update --fix-missing && \
        apt-get install -y wget bzip2 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

    RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-${CONDA_VERSION}-Linux-x86_64.sh -O ~/miniconda.sh && \
        /bin/bash ~/miniconda.sh -b -p /opt/miniconda && \
        rm ~/miniconda.sh && \
        /opt/miniconda/bin/conda clean -tipsy

    RUN conda install -y conda=${CONDA_VERSION} python=${PYTHON_VERSION} && \
        conda clean -aqy && \
        rm -rf /opt/miniconda/pkgs && \
        find / -type d -name __pycache__ -prune -exec rm -rf {} \;
    ```

2. Из оболочки или командной строки используйте следующую команду для проверки подлинности в реестре контейнеров Azure. Замените `<registry_name>` именем реестра контейнеров, в котором вы хотите сохранить образ:

    ```azurecli-interactive
    az acr login --name <registry_name>
    ```

3. Чтобы отправить Dockerfile и построить его, используйте следующую команду. Замените `<registry_name>` именем реестра контейнеров, в котором вы хотите сохранить образ:

    ```azurecli-interactive
    az acr build --image myimage:v1 --registry <registry_name> --file Dockerfile .
    ```

    В процессе сборки данные передаются в потоковую передачу в командную строку. Если сборка выполнена успешно, появится сообщение следующего вида:

    ```text
    Run ID: cda was successful after 2m56s
    ```

Дополнительные сведения о создании образов с помощью реестра контейнеров Azure см. в статье [Сборка и запуск образа контейнера с использованием задач реестра контейнеров Azure](https://docs.microsoft.com/azure/container-registry/container-registry-quickstart-task-cli) .

Дополнительные сведения о передаче существующих образов в реестр контейнеров Azure см. в разделе [Отправка первого образа в частный реестр контейнеров DOCKER](/azure/container-registry/container-registry-get-started-docker-cli).

## <a name="use-a-custom-base-image"></a>Использовать пользовательский базовый образ

Чтобы использовать пользовательский образ, вам потребуются следующие сведения:

* __Имя образа__. Например, `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` — это путь к базовому образу DOCKER, предоставляемому корпорацией Майкрософт.
* Если образ находится в __частном репозитории__, вам потребуются следующие сведения:

    * __Адрес__реестра. Например, `myregistry.azureecr.io`.
    * __Имя пользователя__ и __пароль__ субъекта-службы, которые имеют доступ на чтение реестра.

    Если эти сведения отсутствуют, обратитесь к администратору реестра контейнеров Azure, который содержит образ.

### <a name="publicly-available-base-images"></a>Общедоступные базовые образы

Корпорация Майкрософт предоставляет несколько образов DOCKER в общедоступном репозитории, который можно использовать с действиями, описанными в этом разделе.

| Изображение | Description |
| ----- | ----- |
| `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` | Базовый образ для Машинное обучение Azure |
| `mcr.microsoft.com/azureml/onnxruntime:latest` | Содержит среду выполнения ONNX для загрузки ЦП |
| `mcr.microsoft.com/azureml/onnxruntime:latest-cuda` | Содержит среду выполнения ONNX и CUDA для GPU |
| `mcr.microsoft.com/azureml/onnxruntime:latest-tensorrt` | Содержит среду выполнения ONNX и Тенсоррт для GPU |
| `mcr.microsoft.com/azureml/onnxruntime:latest-openvino-vadm ` | Содержит среду выполнения ONNX и Опенвино для<sup> </sup> архитектуры Intel концепция Accelerator на основе мовидиус<sup>TM</sup> мириадкс впус |
| `mcr.microsoft.com/azureml/onnxruntime:latest-openvino-myriad` | Содержит среду выполнения ONNX и Опенвино для<sup> </sup> USB-накопителей Intel мовидиус<sup>TM</sup> . |

Дополнительные сведения о базовых образах среды выполнения ONNX см. в [разделе ONNX Runtime dockerfile](https://github.com/microsoft/onnxruntime/blob/master/dockerfiles/README.md) в репозитории GitHub.

> [!TIP]
> Так как эти образы являются общедоступными, при их использовании не нужно указывать адрес, имя пользователя или пароль.

Дополнительные сведения см. в разделе [машинное обучение Azure Containers](https://github.com/Azure/AzureML-Containers).

> [!TIP]
>__Если ваша модель обучена в машинное обучение Azure вычислений__с помощью пакета SDK для машинное обучение Azure __версии 1.0.22 или более поздней__ , образ создается во время обучения. Чтобы узнать имя этого образа, используйте `run.properties["AzureML.DerivedImageName"]`. В следующем примере показано, как использовать это изображение:
>
> ```python
> # Use an image built during training with SDK 1.0.22 or greater
> image_config.base_image = run.properties["AzureML.DerivedImageName"]
> ```

### <a name="use-an-image-with-the-azure-machine-learning-sdk"></a>Использование образа с пакетом SDK для Машинное обучение Azure

Чтобы использовать образ, хранящийся в **реестре контейнеров Azure для вашей рабочей области**, или **общий доступ к реестру контейнеров**, задайте следующие атрибуты [среды](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) :

+ `docker.enabled=True`
+ `docker.base_image`. Укажите в реестре и путь к образу.

```python
from azureml.core.environment import Environment
# Create the environment
myenv = Environment(name="myenv")
# Enable Docker and reference an image
myenv.docker.enabled = True
myenv.docker.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"
```

Чтобы использовать образ из __закрытого реестра контейнеров__ , который отсутствует в рабочей области, необходимо использовать `docker.base_image_registry`, чтобы указать адрес репозитория, а также имя пользователя и пароль:

```python
# Set the container registry information
myenv.docker.base_image_registry.address = "myregistry.azurecr.io"
myenv.docker.base_image_registry.username = "username"
myenv.docker.base_image_registry.password = "password"

myenv.inferencing_stack_version = "latest"  # This will install the inference specific apt packages.

# Define the packages needed by the model and scripts
from azureml.core.conda_dependencies import CondaDependencies
conda_dep = CondaDependencies()
# you must list azureml-defaults as a pip dependency
conda_dep.add_pip_package("azureml-defaults")
myenv.python.conda_dependencies=conda_dep
```

Необходимо добавить azureml-Defaults с Version > = 1.0.45 в качестве зависимости PIP. Этот пакет содержит функции, необходимые для размещения модели в качестве веб-службы. Необходимо также задать для свойства inferencing_stack_version в среде значение "latest", при этом будут установлены определенные пакеты APT, необходимые для веб-службы. 

Определив среду, используйте ее с объектом [инференцеконфиг](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) , чтобы определить среду вывода, в которой будет выполняться модель и веб-служба.

```python
from azureml.core.model import InferenceConfig
# Use environment in InferenceConfig
inference_config = InferenceConfig(entry_script="score.py",
                                   environment=myenv)
```

На этом этапе можно продолжить развертывание. Например, следующий фрагмент кода может развернуть веб-службу локально с помощью конфигурации вывода и пользовательского образа:

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Дополнительные сведения о развертывании см. в разделе [Развертывание моделей с помощью машинное обучение Azure](how-to-deploy-and-where.md).

Дополнительные сведения о настройке среды Python см. в статье [создание сред для обучения и развертывания и управление ими](how-to-use-environments.md). 

### <a name="use-an-image-with-the-machine-learning-cli"></a>Использование образа с интерфейсом командной строки Машинное обучение

> [!IMPORTANT]
> В настоящее время Машинное обучение CLI может использовать образы из реестра контейнеров Azure для вашей рабочей области или общедоступных репозиториев. Он не может использовать образы из автономных закрытых реестров.

При развертывании модели с помощью Машинное обучение CLI вы предоставляете файл конфигурации вывода, который ссылается на пользовательский образ. В следующем документе JSON показано, как ссылаться на изображение в общедоступном реестре контейнеров:

```json
{
   "entryScript": "score.py",
   "runtime": "python",
   "condaFile": "infenv.yml",
   "extraDockerfileSteps": null,
   "sourceDirectory": null,
   "enableGpu": false,
   "baseImage": "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda",
   "baseImageRegistry": "mcr.microsoft.com"
}
```

Этот файл используется с командой `az ml model deploy`. Параметр `--ic` используется для указания файла конфигурации вывода.

```azurecli
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
```

Дополнительные сведения о развертывании модели с помощью интерфейса командной строки ML см. в разделе "Регистрация модели, профилирование и развертывание" статьи [расширение CLI для машинное обучение Azure](reference-azure-machine-learning-cli.md#model-registration-profiling-deployment) .

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте больше о том, [где развертывать и как](how-to-deploy-and-where.md).
* Узнайте, как [обучать и развертывать модели машинного обучения с помощью Azure pipelines](/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops).
