---
title: Включение ведения журнала в Машинное обучение Azure
description: Узнайте, как включить ведение журнала в Машинное обучение Azure с помощью пакета ведения журнала Python по умолчанию, а также с помощью функций, характерных для пакета SDK.
ms.author: larryfr
author: BlackMist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.date: 03/05/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: d4fb08a618c6dfb35f3f8d154af6820c92d62781
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/28/2020
ms.locfileid: "87320142"
---
# <a name="enable-logging-in-azure-machine-learning"></a>Включение ведения журнала в Машинное обучение Azure
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Пакет SDK Python для Машинного обучения Azure позволяет настроить ведение журнала, используя стандартный пакет ведения журнала для Python или собственные функции пакета SDK. Можно вести журналы как локально, так и в рабочей области на портале. Журналы предоставляют разработчикам сведения о состоянии приложения в реальном времени и могут помочь в диагностике ошибок или предупреждений. Из этой статьи вы узнаете:

> [!div class="checklist"]
> * как включить ведение журнала для обучаемых моделей и целевых объектов вычислений;
> * Создание образа.
> * как включить ведение журнала для развернутых моделей:
> * как задать параметры `logging` в Python.

[Создайте рабочую область Машинного обучения Azure](how-to-manage-workspace.md). Для получения дополнительных сведений о пакете SDK воспользуйтесь [руководством](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) .

## <a name="training-models-and-compute-target-logging"></a>Ведение журнала для обучаемых моделей и целевых объектов вычислений

Есть несколько способов настроить ведение журнала для процесса обучения моделей. Представленные здесь примеры иллюстрируют несколько распространенных шаблонов разработки. Вы можете без труда настроить сохранение данных об обучении в облачную рабочую область, используя функцию `start_logging` для класса `Experiment`.

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

Дополнительные функции ведения журнала см. в справочной документации по классу [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) .

Чтобы сохранять сведения о состоянии приложения в ходе обучения в локальный журнал, используйте параметр `show_output`. Подробное ведение журналов позволяет получить сведения об обучении, а также об удаленных ресурсах и целевых объектах вычислений. С помощью приведенного ниже кода можно настроить сохранение данных в журнал при отправке эксперимента.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

Этот же параметр вы можете указать в функции `wait_for_completion` для финального выполнения.

```python
run.wait_for_completion(show_output=True)
```

Кроме того, пакет SDK поддерживает стандартный пакет ведения журнала Python для некоторых сценариев обучения. В следующем примере для объекта `AutoMLConfig` включается уровень ведения журнала `INFO`.

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task='regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

Кроме того, вы можете использовать параметр `show_output` при создании постоянного целевого объекта вычислений. Укажите этот параметр в функции `wait_for_completion`, чтобы настроить ведение журнала во время создания целевого объекта вычислений.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(
    workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-for-deployed-models"></a>Ведение журнала для развернутых моделей

Чтобы получить журналы из ранее развернутой веб-службы, загрузите службу и вызовите функцию `get_logs()`. Эти журналы могут содержать подробные сведения об ошибках, возникающих во время развертывания.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

Кроме того, вы можете сохранять пользовательскую трассировку стека веб-службы, включив Application Insights. Это позволит отслеживать время запроса и отклика, частоту сбоев и исключения. Вызовите функцию `update()` для существующей веб-службы, чтобы включить Application Insights.

```python
service.update(enable_app_insights=True)
```

Дополнительные сведения см. в статье [мониторинг и получение данных из конечных точек веб-службы ML](how-to-enable-app-insights.md).

## <a name="python-native-logging-settings"></a>Параметры собственного ведения журнала Python

Некоторые журналы, создаваемые пакетом SDK, могут содержать сообщения об ошибках с предложением установить уровень ведения журнала DEBUG. Чтобы изменить уровень ведения журнала, добавьте в скрипт приведенный ниже код.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="next-steps"></a>Дальнейшие действия

* [Мониторинг и сбор данных из конечных точек веб-службы Машинного обучения](how-to-enable-app-insights.md)
