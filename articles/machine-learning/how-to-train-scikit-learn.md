---
title: Обучение scikit. изучение моделей машинного обучения
titleSuffix: Azure Machine Learning
description: Узнайте, как выполнять сценарии обучения scikit-учиться в масштабе предприятия с помощью класса Машинное обучение Azure SKlearn оценщика. Примеры сценариев классифицируют изображения диафрагмы для создания модели машинного обучения на основе набора данных диафрагмы scikit-Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: maxluk
author: maxluk
ms.date: 03/09/2020
ms.custom: seodec18, tracking-python
ms.openlocfilehash: 3669bfb10a990f042d1470fbabee19809a6785cc
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "87060705"
---
# <a name="build-scikit-learn-models-at-scale-with-azure-machine-learning"></a>Создание scikit. изучение моделей в масштабе с помощью Машинное обучение Azure
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Из этой статьи вы узнаете, как выполнять сценарии обучения scikit-учиться в масштабе предприятия с помощью класса [оценщика машинное обучение Azure SKlearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) . 

Примеры сценариев, приведенные в этой статье, используются для классификации изображений диафрагмы для создания модели машинного обучения на основе [набора данных диафрагмы](https://archive.ics.uci.edu/ml/datasets/iris)scikit.

Если вы научитесь изучать модель машинного обучения scikit-учиться с нуля или используете существующую модель в облаке, вы можете использовать Машинное обучение Azure для масштабирования заданий обучения с открытым исходным кодом с помощью эластичных облачных ресурсов. Вы можете создавать, развертывать, разворачивать и отслеживать модели производственного уровня с помощью Машинное обучение Azure.

## <a name="prerequisites"></a>Обязательные условия

Запустите этот код в любой из этих сред:
 - Вычислительная операция Машинного обучения Azure — загрузка или установка не требуется

    - Выполните инструкции из [учебника Настройка среды и рабочей области](tutorial-1st-experiment-sdk-setup.md) , чтобы создать выделенный сервер записной книжки, предварительно загруженный с помощью пакета SDK и примера репозитория.
    - В папке Learning Samples на сервере записной книжки найдите готовую и развернутую записную книжку, перейдя к этому каталогу: **практические советы по использованию azureml > ML-frameworks > scikit-learning > learning > Train-i Parameter-Learning-Deploy-with-sklearn** Folder.

 - Собственный сервер Jupyter Notebook

    - [Установите пакет SDK для машинное обучение Azure](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
    - [Создайте файл конфигурации рабочей области](how-to-configure-environment.md#workspace).
    - Скачайте набор данных и пример файла скрипта 
        - [набор данных IRI](https://archive.ics.uci.edu/ml/datasets/iris)
        - [train_iris. Корректировка](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn)
    - Вы также можете найти завершенную [Jupyter Notebook версию](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb) этого руководства на странице примеров GitHub. Записная книжка включает в себя развернутую секцию, охватывающую настройку интеллектуальной настройки, и получение лучшей модели по основным метрикам.

## <a name="set-up-the-experiment"></a>Настройка эксперимента

В этом разделе выполняется настройка обучающего эксперимента путем загрузки требуемых пакетов Python, инициализации рабочей области, создания эксперимента и отправки обучающих данных и сценариев обучения.

### <a name="import-packages"></a>Импорт пакетов

Сначала импортируйте необходимые библиотеки Python.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Инициализация рабочей области

[Машинное обучение Azure Рабочая область](concept-workspace.md) — это ресурс верхнего уровня для службы. Она предоставляет централизованное расположение для работы со всеми создаваемыми артефактами. В пакете SDK для Python можно получить доступ к артефактам рабочей области, создав [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) объект.

Создайте объект рабочей области из `config.json` файла, созданного в [разделе Предварительные требования](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-a-machine-learning-experiment"></a>Создание эксперимента в машинном обучении

Создайте эксперимент и папку для хранения сценариев обучения. В этом примере Создайте эксперимент с названием «sklearn-IRI».

```Python
project_folder = './sklearn-iris'
os.makedirs(project_folder, exist_ok=True)

experiment = Experiment(workspace=ws, name='sklearn-iris')
```

### <a name="prepare-training-script"></a>Подготовка сценария обучения

В этом руководстве сценарий обучения **train_iris. Корректировка** уже предоставлен. На практике вы можете использовать любой пользовательский сценарий обучения как есть и запускать его с помощью Azure ML без необходимости изменять код.

Чтобы использовать возможности отслеживания и метрик машинного обучения Azure, добавьте небольшой объем кода машинного обучения Azure в сценарий обучения.  В обучающем скрипте **train_iris. Корректировка** показано, как зарегистрировать некоторые метрики в машинном обучении Azure с помощью `Run` объекта в скрипте.

Предоставленный Скрипт обучения использует примеры данных из `iris = datasets.load_iris()` функции.  Для собственных данных может потребоваться выполнить такие действия, как [Передача набора данных и скриптов](how-to-train-keras.md#data-upload) , чтобы сделать данные доступными во время обучения.

Скопируйте сценарий обучения **train_iris. копировать** в каталог проекта.

```
import shutil
shutil.copy('./train_iris.py', project_folder)
```

## <a name="create-or-get-a-compute-target"></a>Создание или получение целевого объекта вычислений

Создайте целевой объект вычислений для выполнения задания scikit-учиться. Scikit — сведения поддерживают только один узел, вычислительные ресурсы ЦП.

Следующий код создает Машинное обучение Azure управляемого вычислений (Амлкомпуте) для удаленного обучающего ресурса. Создание Амлкомпуте занимает примерно 5 минут. Если Амлкомпуте с таким именем уже находится в рабочей области, этот код пропустит процесс создания.

```Python
cluster_name = "cpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

Дополнительные сведения о целевых объектах вычислений см. в статье [что такое целевые показатели вычислений](concept-compute-target.md) .

## <a name="create-a-scikit-learn-estimator"></a>Создание средства оценки scikit-учиться

Средство [оценки scikit-](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn?view=azure-ml-py) Training предоставляет простой способ запуска задания обучения scikit-учиться на целевом объекте вычислений. Он реализуется с помощью [`SKLearn`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) класса, который можно использовать для поддержки обучения ЦП с одним узлом.

Если для выполнения сценария обучения требуются дополнительные пакеты PIP или conda, можно установить пакеты в полученном образе DOCKER, передав их имена с помощью `pip_packages` `conda_packages` аргументов и.

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    '--kernel': 'linear',
    '--penalty': 1.0,
}

estimator = SKLearn(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train_iris.py',
                    pip_packages=['joblib']
                   )
```

> [!WARNING]
> Машинное обучение Azure запускает скрипты обучения, копируя весь исходный каталог. Если у вас есть конфиденциальные данные, которые не нужно передавать, используйте [файл. Ignore](how-to-save-write-experiment-files.md#storage-limits-of-experiment-snapshots) или не включайте его в исходный каталог. Вместо этого получите доступ к данным с помощью [хранилища](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py)данных.

Дополнительные сведения о настройке среды Python см. на [этой странице](how-to-use-environments.md). 

## <a name="submit-a-run"></a>Отправка запроса на выполнение

[Объект Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) предоставляет интерфейс для журнала выполнения во время выполнения задания и после его завершения.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

При выполнении выполнения он проходит следующие этапы.

- **Подготовка**. образ DOCKER создается в соответствии с оценкой TensorFlow. Образ отправляется в реестр контейнеров рабочей области и кэшируется для последующего выполнения. Журналы также передаются в журнал выполнения и могут быть просмотрены для отслеживания хода выполнения.

- **Масштабирование**. Кластер пытается выполнить масштабирование, если кластеру Batch AI требуется больше узлов для выполнения выполнения, чем в настоящее время доступно.

- **Выполняется**: все скрипты в папке Script передаются в целевой объект вычислений, хранилища данных подключаются или копируются, а entry_script выполняется. Выходные данные из STDOUT и папки/логс передаются в журнал выполнения и могут использоваться для наблюдения за выполнением.

- **Пост-обработка**. папка/Outputs для выполнения копируется в журнал выполнения.

## <a name="save-and-register-the-model"></a>Сохранение и регистрация модели

После обучения модель можно сохранить и зарегистрировать в рабочей области. Регистрация модели позволяет хранить и редактировать версии моделей в рабочей области для упрощения [развертывания и управления моделями](concept-model-management-and-deployment.md).

Добавьте следующий код в сценарий обучения train_iris. корректировки, чтобы сохранить модель. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Зарегистрируйте модель в рабочей области с помощью следующего кода. При указании параметров `model_framework` , `model_framework_version` и `resource_configuration` , развертывание модели без кода станет доступным. Это позволяет напрямую развернуть модель в качестве веб-службы из зарегистрированной модели, а [`ResourceConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.resource_configuration.resourceconfiguration?view=azure-ml-py) объект определяет ресурс вычислений для веб-службы.

```Python
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

model = run.register_model(model_name='sklearn-iris', 
                           model_path='outputs/model.joblib',
                           model_framework=Model.Framework.SCIKITLEARN,
                           model_framework_version='0.19.1',
                           resource_configuration=ResourceConfiguration(cpu=1, memory_in_gb=0.5))
```

## <a name="deployment"></a>Развертывание

Только что зарегистрированная модель может быть развернута точно так же, как и любая другая Зарегистрированная модель в Машинное обучение Azure, независимо от того, какой механизм оценки использовался для обучения. Руководство по развертыванию содержит раздел, посвященный регистрации моделей, но можно сразу перейти к [созданию целевого объекта вычислений](how-to-deploy-and-where.md#choose-a-compute-target) для развертывания, так как у вас уже есть Зарегистрированная модель.

### <a name="preview-no-code-model-deployment"></a>Образца Развертывание модели без кода

Вместо традиционного маршрута развертывания можно также использовать функцию развертывания без кода (Предварительная версия) для scikit — обучение. Развертывание модели без кода поддерживается для всех встроенных типов моделей scikit-учиться. Зарегистрировав модель, как показано выше с помощью `model_framework` параметров, `model_framework_version` и `resource_configuration` , можно просто использовать [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) статическую функцию для развертывания модели.

```python
web_service = Model.deploy(ws, "scikit-learn-service", [model])
```

Примечание. Эти зависимости включены в предварительно построенный контейнер вывода scikit-учиться.

```yaml
    - azureml-defaults
    - inference-schema[numpy-support]
    - scikit-learn
    - numpy
```

Полное [Описание процесса](how-to-deploy-and-where.md) развертывания в машинное обучение Azure более подробно.


## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы обучили и зарегистрировали модель scikit-учиться и узнали о вариантах развертывания. Дополнительные сведения о Машинное обучение Azure см. в других статьях.

* [Отслеживание метрик выполнения во время обучения](how-to-track-experiments.md)
* [Настройка гиперпараметров](how-to-tune-hyperparameters.md)
* [Эталонная архитектура для распределенного обучения глубокого обучения в Azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
