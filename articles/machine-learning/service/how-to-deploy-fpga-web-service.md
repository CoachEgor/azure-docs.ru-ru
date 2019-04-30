---
title: Развертывание моделей в ППВМ
titleSuffix: Azure Machine Learning service
description: Узнайте, как развернуть веб-службу с моделью, работающей в ППВМ, с использованием Службы машинного обучения Azure, чтобы обеспечить получение выводов со сверхнизкой задержкой.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 1/29/2019
ms.custom: seodec18
ms.openlocfilehash: 7aa0e11ed47219829830369d17b300270d3fbffb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60819445"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Развертывание модели как веб-службы в ППВМ с помощью Службы машинного обучения Azure

Вы можете развернуть модель как веб-службу в [матрицах FPGA](concept-accelerate-with-fpgas.md).  При использовании FPGA обеспечивается сверхнизкая задержка даже в случае пакетов одного размера.  Модели, доступные сегодня:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16   

## <a name="prerequisites"></a>Технические условия

- Если у вас еще нет подписки Azure, создайте бесплатную учетную запись Azure, прежде чем начинать работу. Опробуйте [бесплатную или платную версию Службы машинного обучения Azure](https://aka.ms/AMLFree).

- Должны быть установлены рабочая область службы "Машинное обучение Azure" и пакет SDK Машинного обучения Azure для Python. Дополнительные сведения о получении этих необходимых компонентов см. в документе [Настройка среды разработки](how-to-configure-environment.md).
 
  - Ваша рабочая область должна находиться в регионе *Восточная часть США 2*.

  - Установите дополнительные компоненты.

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```

  - Сейчас поддерживается только версия tenorflow<=1.10, поэтому установите ее после завершения всех остальных установок.

    ```shell
    pip install "tensorflow==1.10"
    ```

## <a name="create-and-deploy-your-model"></a>Создание и развертывание модели
Создание конвейера для предварительной обработки входного изображения, сделать его характеристики с помощью ResNet-50 на FPGA, а затем запустите средства через обучена на наборе данных ImageNet классификатора.

Следуйте инструкциям по выполнению следующих процедур:

* Определение конвейера модели
* Развертывание модели
* Использовать развернутую модель.
* Удалить развернутые службы.

> [!IMPORTANT]
> Чтобы оптимизировать задержку и пропускную способность, ваш клиент должен находиться в том же регионе Azure, что и конечная точка.  В настоящее время API создаются в регионе Azure "Восточная часть США".



### <a name="preprocess-image"></a>Предварительная обработка изображения
Первый этап конвейера — предварительная обработка изображений.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="add-featurizer"></a>Добавление характеризатора
Инициализируйте модель и загрузите контрольную точку TensorFlow квантованной версии ResNet50 для использования в качестве характеризатора.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Добавление классификатора
Этот классификатор обучен с помощью набора данных ImageNet.

```python
classifier_output = model.get_default_classifier(feature_tensor)
```

### <a name="create-service-definition"></a>Создание определения службы
Определив предварительную обработку изображений, характеризатор и классификатор, который выполняется в службе, вы можете создать определение службы. Определение службы — это набор файлов, созданных на основе модели, которая развертывается в службе FPGA. Определение службы состоит из конвейера. Конвейер — это последовательность этапов, которые выполняются по порядку.  Поддерживаются этапы TensorFlow, Keras и BrainWave.  Этапы выполняются в службе по порядку: выходные данные каждого этапа используются как входные данные для следующего этапа.

Для создания этапа TensorFlow укажите сеанс, содержащий граф (в данном случае используется граф по умолчанию), и входные и выходные тензоры для этого этапа.  Эти данные используются для сохранения графа, чтобы его можно было выполнить в службе.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'model_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, feature_tensor, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Развертывание модели
Создайте службу на основе определения службы.  Ваша рабочая область должна находиться в регионе "Восточная часть США 2".

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(True)
```

### <a name="test-the-service"></a>Тестирование службы
Чтобы отправить изображение в API и проверить ответ, добавьте сопоставление из идентификатора выходного класса в имя класса ImageNet.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Вызовите службу и замените имя файла your-image.jpg ниже изображением с компьютера. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Удаление службы
Удалите службу.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>Защита веб-служб FPGA

Сведения о защите веб-служб FPGA см. в статье [Использование SSL для защиты веб-служб, развернутых с помощью Службы машинного обучения Azure](how-to-secure-web-service.md).


## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как [использовать модель машинного обучения, развернутую в виде веб-службы](how-to-consume-web-service.md).
