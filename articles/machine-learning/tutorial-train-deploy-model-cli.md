---
title: Обучение и развертывание моделей с помощью интерфейса командной строки
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать расширение машинного обучения для Azure CLI обучения, регистрации и развертывания модели из командной строки.
ms.author: larryfr
author: Blackmist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: f920df20a8dc1cace76f641ce1c71f9b91a30bf4
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2020
ms.locfileid: "75867671"
---
# <a name="tutorial-train-and-deploy-a-model-from-the-cli"></a>Учебник. обучение и развертывание модели с помощью интерфейса командной строки
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

В этом руководстве используется расширение машинного обучения для Azure CLI обучения, регистрации и развертывания модели.

В сценариях обучения Python в этом учебнике используется [scikit —](https://scikit-learn.org/) обучение для обучения базовой модели. Основное внимание этого руководства не в сценариях или модели, а в процессе работы с Машинное обучение Azure используется интерфейс командной строки.

Узнайте, как выполнять следующие действия:

> [!div class="checklist"]
> * Установка расширения машинного обучения
> * Создание рабочей области машинного обучения Azure
> * Создание ресурса вычислений, используемого для обучения модели
> * Определение и регистрация набора данных, используемого для обучения модели
> * Начать обучающий запуск
> * Регистрация и скачивание модели
> * Развертывание модели в качестве веб-службы.
> * Оценка данных с помощью веб-службы

## <a name="prerequisites"></a>Технические условия

* Подписка Azure. Если у вас еще нет подписки Azure, создайте бесплатную учетную запись Azure, прежде чем начинать работу. Опробуйте [бесплатную или платную версию Машинного обучения Azure](https://aka.ms/AMLFree) уже сегодня.

* Для использования команд интерфейса командной строки в этом документе из **локальной среды**требуется [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

    Если вы используете [Azure Cloud Shell](https://azure.microsoft.com//features/cloud-shell/), интерфейс командной строки доступен через браузер и находится в облаке.

## <a name="download-the-example-project"></a>Скачать пример проекта

Для работы с этим руководством Скачайте проект [https://github.com/microsoft/MLOps](https://github.com/microsoft/MLOps) . Файлы в каталоге `examples/cli-train-deploy` используются в действиях, описанных в этом руководстве.

Чтобы получить локальную копию файлов, [скачайте zip-архив](https://github.com/microsoft/MLOps/archive/master.zip)или выполните следующую команду git, чтобы клонировать репозиторий:

```azurecli-interactive
git clone https://github.com/microsoft/MLOps.git
```

### <a name="training-files"></a>Файлы обучения

Каталог `examples/cli-train-deploy` из проекта содержит следующие файлы, которые используются при обучении модели:

* `.azureml\mnist.runconfig`: файл __конфигурации запуска__ . Этот файл определяет среду выполнения, необходимую для обучения модели. В этом примере он также подключает данные, используемые для обучения модели, в среде обучения.
* `scripts\train.py`: сценарий обучения. Этот файл обучает модель.
* `scripts\utils.py`: вспомогательный файл, используемый сценарием обучения.
* `.azureml\conda_dependencies.yml`: определяет зависимости программного обеспечения, необходимые для запуска сценария обучения.
* `dataset.json`: определение набора данных. Используется для регистрации набора данных MNIST в рабочей области Машинное обучение Azure.

### <a name="deployment-files"></a>Файлы развертывания

Репозиторий содержит следующие файлы, которые используются для развертывания обученной модели в качестве веб-службы.

* `aciDeploymentConfig.yml`: файл __конфигурации развертывания__ . Этот файл определяет среду размещения, необходимую для модели.
* `inferenceConfig.yml`: файл configuration__ вывода. Этот файл определяет программную среду, используемую службой для оценки данных с помощью модели.
* `score.py`: скрипт Python, который принимает входящие данные, оценивает его с помощью модели, а затем возвращает ответ.
* `scoring-env.yml`: зависимости conda, необходимые для запуска модели и `score.py` скрипта.
* `testdata.json`: файл данных, который можно использовать для тестирования развернутой веб-службы.

## <a name="connect-to-your-azure-subscription"></a>Подключение к подписке Azure

Существует несколько способов проверки подлинности в подписке Azure с помощью интерфейса командной строки. Самый простой — выполнить интерактивную проверку подлинности с помощью браузера. Чтобы выполнить аутентификацию в интерактивном режиме, откройте командную строку или терминал и используйте следующую команду:

```azurecli-interactive
az login
```

Если CLI сможет запустить браузер по умолчанию, он откроет в браузере страницу входа. В противном случае необходимо открыть браузер и выполнить инструкции из командной строки. В инструкциях входит обзор [https://aka.ms/devicelogin](https://aka.ms/devicelogin) и ввод кода авторизации.

## <a name="install-the-machine-learning-extension"></a>Установка расширения машинного обучения

Чтобы установить расширение машинного обучения, используйте следующую команду:

```azurecli-interactive
az extension add -n azure-cli-ml
```

Если вы получаете сообщение о том, что расширение уже установлено, используйте следующую команду для обновления до последней версии:

```azurecli-interactive
az extension update -n azure-cli-ml
```

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов — это базовый контейнер ресурсов на платформе Azure. При работе с Машинное обучение Azure группа ресурсов будет содержать рабочую область Машинное обучение Azure. Он также будет содержать другие службы Azure, используемые рабочей областью. Например, при обучении модели с помощью облачного ресурса вычислений этот ресурс создается в группе ресурсов.

Чтобы __создать новую группу ресурсов__, используйте следующую команду. Замените `<resource-group-name>` именем, которое будет использоваться для этой группы ресурсов. Замените `<location>` на регион Azure, который будет использоваться для этой группы ресурсов:

> [!TIP]
> Необходимо выбрать регион, в котором доступна Машинное обучение Azure. Дополнительные сведения см. в разделе [продукты, доступные по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service).

```azurecli-interactive
az group create --name <resource-group-name> --location <location>
```

Ответ этой команды аналогичен следующему JSON:

```json
{
  "id": "/subscriptions/<subscription-GUID>/resourceGroups/<resourcegroupname>",
  "location": "<location>",
  "managedBy": null,
  "name": "<resource-group-name>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

Дополнительные сведения о работе с группами ресурсов см. в разделе [AZ Group](https://docs.microsoft.com//cli/azure/group?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Создание рабочего пространства

Чтобы создать рабочую область, используйте следующую команду. Замените `<workspace-name>` именем, которое вы хотите использовать для этой рабочей области. Замените `<resource-group-name>` именем группы ресурсов:

```azurecli-interactive
az ml workspace create -w <workspace-name> -g <resource-group-name>
```

Выходные данные этой команды похожи на следующий код JSON:

```json
{
  "applicationInsights": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<application-insight-name>",
  "containerRegistry": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.containerregistry/registries/<acr-name>",
  "creationTime": "2019-08-30T20:24:19.6984254+00:00",
  "description": "",
  "friendlyName": "<workspace-name>",
  "id": "/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>",
  "identityPrincipalId": "<GUID>",
  "identityTenantId": "<GUID>",
  "identityType": "SystemAssigned",
  "keyVault": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.keyvault/vaults/<key-vault-name>",
  "location": "<location>",
  "name": "<workspace-name>",
  "resourceGroup": "<resource-group-name>",
  "storageAccount": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.storage/storageaccounts/<storage-account-name>",
  "type": "Microsoft.MachineLearningServices/workspaces",
  "workspaceid": "<GUID>"
}
```

## <a name="connect-local-project-to-workspace"></a>Подключение локального проекта к рабочей области

В терминале или командной строке используйте следующие команды, чтобы изменить каталоги на `cli-train-deploy` Directory, а затем подключиться к рабочей области:

```azurecli-interactive
cd ~/MLOps/examples/cli-train-deploy
az ml folder attach -w <workspace-name> -g <resource-group-name>
```

Выходные данные этой команды похожи на следующий код JSON:

```json
{
  "Experiment name": "model-training",
  "Project path": "/home/user/MLOps/examples/cli-train-deploy",
  "Resource group": "<resource-group-name>",
  "Subscription id": "<subscription-id>",
  "Workspace name": "<workspace-name>"
}
```

Эта команда создает файл `.azureml/config.json`, который содержит сведения, необходимые для подключения к рабочей области. Остальные команды `az ml`, используемые в этом руководстве, будут использовать этот файл, поэтому вам не нужно добавлять рабочую область и группу ресурсов во все команды.

## <a name="create-the-compute-target-for-training"></a>Создание целевого объекта вычислений для обучения

В этом примере для обучения модели используется Машинное обучение Azure COMPUTE кластер. Чтобы создать новый кластер вычислений, используйте следующую команду:

```azurecli-interactive
az ml computetarget create amlcompute -n cpu-cluster --max-nodes 4 --vm-size Standard_D2_V2
```

Выходные данные этой команды похожи на следующий код JSON:

```json
{
  "location": "<location>",
  "name": "cpu-cluster",
  "provisioningErrors": null,
  "provisioningState": "Succeeded"
}
```

Эта команда создает новый целевой объект вычислений с именем `cpu`, не более четырех узлов. Выбранный размер виртуальной машины предоставляет виртуальную машину с ресурсом GPU. Сведения о размере виртуальной машины см. в разделе [типы и размеры виртуальных машин].

> [!IMPORTANT]
> Имя целевого объекта вычислений (`cpu` в данном случае) важно; на него ссылается файл `.azureml/mnist.runconfig`, используемый в следующем разделе.

## <a name="define-the-dataset"></a>Определение набора данных

Для обучения модели можно предоставить обучающие данные с помощью набора данных. Чтобы создать набор данных из интерфейса командной строки, необходимо указать файл определения набора данных. Файл `dataset.json`, указанный в репозитории, создает новый набор данных с помощью данных MNIST. Создаваемый набор данных называется `mnist-dataset`.

Чтобы зарегистрировать набор данных с помощью файла `dataset.json`, используйте следующую команду:

```azurecli-interactive
az ml dataset register -f dataset.json --skip-validation
```

Выходные данные этой команды похожи на следующий код JSON:

```json
{
  "definition": [
    "GetFiles"
  ],
  "registration": {
    "description": "mnist dataset",
    "id": "a13a4034-02d1-40bd-8107-b5d591a464b7",
    "name": "mnist-dataset",
    "tags": {
      "sample-tag": "mnist"
    },
    "version": 1,
    "workspace": "Workspace.create(name='myworkspace', subscription_id='mysubscriptionid', resource_group='myresourcegroup')"
  },
  "source": [
    "http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz",
    "http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz",
    "http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz",
    "http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz"
  ]
}
```


> [!IMPORTANT]
> Скопируйте значение записи `id`, так как оно используется в следующем разделе.

Чтобы узнать более полный шаблон JSON-файла, описывающего набор данных, используйте следующую команду:
```azurecli-interactive
az ml dataset register --show-template
```

## <a name="reference-the-dataset"></a>Ссылка на набор данных

Чтобы сделать набор данных доступным в среде обучения, необходимо сослаться на него из файла runconfig. Файл `.azureml/mnist.runconfig` содержит следующие записи YAML:

```yaml
# The arguments to the script file.
arguments:
- --data-folder
- DatasetConsumptionConfig:mnist

.....

# The configuration details for data.
data:
  mnist:
# Data Location
    dataLocation:
# the Dataset used for this run.
      dataset:
# Id of the dataset.
        id: a13a4034-02d1-40bd-8107-b5d591a464b7
# the DataPath used for this run.
      datapath:
# Whether to create new folder.
    createOutputDirectories: false
# The mode to handle
    mechanism: mount
# Point where the data is download or mount or upload.
    environmentVariableName: mnist
# relative path where the data is download or mount or upload.
    pathOnCompute:
# Whether to overwrite the data if existing.
    overwrite: false
```

Измените значение записи `id` в соответствии со значением, возвращаемым при регистрации набора данных. Это значение используется для загрузки данных в целевой объект вычислений во время обучения.

Этот YAML выполняет следующие действия:

* Подключает набор данных (на основе идентификатора набора данных) в среде обучения и сохраняет путь к точке подключения в переменной среды `mnist`.
* Передает расположение данных (точку подключения) в среде обучения в скрипт, используя аргумент `--data-folder`.

Файл runconfig также содержит сведения, используемые для настройки среды, используемой обучающим запуском. Если просмотреть этот файл, вы увидите, что он ссылается на `cpu-compute`ный целевой объект вычислений, созданный ранее. В нем также указано количество узлов, используемых при обучении (`"nodeCount": "4"`), и содержится `"condaDependencies"` раздел, в котором перечислены пакеты Python, необходимые для запуска сценария обучения.

> [!TIP]
> Хотя можно вручную создать файл runconfig, в этом примере он был создан с помощью файла `generate-runconfig.py`, включенного в репозиторий. Этот файл получает ссылку на зарегистрированный набор данных, создает конфигурацию запуска программно, а затем сохраняет ее в файле.

Дополнительные сведения о файлах конфигурации запуска см. в разделе [Установка и использование целевых объектов вычислений для обучения модели](how-to-set-up-training-targets.md#create-run-configuration-and-submit-run-using-azure-machine-learning-cli)или ссылка на этот [JSON-файл](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json) , чтобы просмотреть полную схему для runconfig.

## <a name="submit-the-training-run"></a>Отправить обучающий запуск

Чтобы начать обучающий запуск на целевом `cpu-compute` вычислений, используйте следующую команду:

```azurecli-interactive
az ml run submit-script -c mnist -e myexperiment --source-directory scripts -t runoutput.json
```

Эта команда задает имя эксперимента (`myexperiment`). В эксперименте хранятся сведения об этом запуске в рабочей области.

Параметр `-c mnist` указывает файл `.azureml/mnist.runconfig`.

Параметр `-t` сохраняет ссылку на этот запуск в JSON-файле и будет использоваться на следующих шагах для регистрации и загрузки модели.

По мере выполнения процесса обучения он выполняет потоковую передачу информации из сеанса обучения на удаленном ресурсе вычислений. Часть информации аналогична приведенному ниже тексту.

```text
Predict the test set
Accuracy is 0.9185
```

Этот текст заносится в журнал из обучающего скрипта и отображает точность модели. Другие модели будут иметь разные метрики производительности.

Если изучить обучающий сценарий, то вы заметите, что он также использует альфа-значение при сохранении обученной модели для `outputs/sklearn_mnist_model.pkl`.

Модель была сохранена в каталоге `./outputs` в целевом объекте вычислений, где он был обучен. В этом случае это Машинное обучение Azure вычислительный экземпляр в облаке Azure. Процесс обучения автоматически отправляет содержимое каталога `./outputs` из целевого объекта вычислений, в котором выполняется обучение в рабочей области Машинное обучение Azure. Он хранится в рамках эксперимента (`myexperiment` в этом примере).

## <a name="register-the-model"></a>Регистрация модели

Чтобы зарегистрировать модель непосредственно из сохраненной версии в эксперименте, используйте следующую команду:

```azurecli-interactive
az ml model register -n mymodel -f runoutput.json --asset-path "outputs/sklearn_mnist_model.pkl" -t registeredmodel.json
```

Эта команда регистрирует `outputs/sklearn_mnist_model.pkl` файл, созданный обучающим запуском, в качестве новой регистрации модели с именем `mymodel`. `--assets-path` ссылается на путь в эксперименте. В этом случае сведения о эксперименте и запуске загружаются из файла `runoutput.json`, созданного командой обучения. `-t registeredmodel.json` создает JSON-файл, который ссылается на новую зарегистрированную модель, созданную этой командой, и используется другими командами интерфейса командной строки, работающими с зарегистрированными моделями.

Выходные данные этой команды похожи на следующий код JSON:

```json
{
  "createdTime": "2019-09-19T15:25:32.411572+00:00",
  "description": "",
  "experimentName": "myexperiment",
  "framework": "Custom",
  "frameworkVersion": null,
  "id": "mymodel:1",
  "name": "mymodel",
  "properties": "",
  "runId": "myexperiment_1568906070_5874522d",
  "tags": "",
  "version": 1
}
```

### <a name="model-versioning"></a>управления версиями моделей;

Обратите внимание на номер версии, возвращенный для модели. Версия увеличивается каждый раз при регистрации новой модели с таким именем. Например, можно скачать модель и зарегистрировать ее из локального файла с помощью следующих команд:

```azurecli-interactive
az ml model download -i "mymodel:1" -t .
az ml model register -n mymodel -p "sklearn_mnist_model.pkl"
```

Первая команда скачивает зарегистрированную модель в текущий каталог. Имя файла `sklearn_mnist_model.pkl`— это файл, на который указывает ссылка при регистрации модели. Вторая команда регистрирует локальную модель (`-p "sklearn_mnist_model.pkl"`) с тем же именем, что и Предыдущая регистрация (`mymodel`). На этот раз возвращенные данные JSON выводит версию как 2.

## <a name="deploy-the-model"></a>Развертывание модели

Чтобы развернуть модель, используйте следующую команду:

```azurecli-interactive
az ml model deploy -n myservice -m "mymodel:1" --ic inferenceConfig.yml --dc aciDeploymentConfig.yml
```

> [!NOTE]
> Может появиться предупреждение об ошибке "не удалось проверить существование Локалвебсервице". Вы можете спокойно проигнорировать это, так как вы не развертываете локальную веб-службу.

Эта команда развертывает новую службу с именем `myservice`, используя версию 1 модели, которая была зарегистрирована ранее.

Файл `inferenceConfig.yml` содержит сведения о том, как выполнять вывод, например сценарий записи (`score.py`) и зависимости программного обеспечения. Дополнительные сведения о структуре этого файла см. в статье [Схема конфигурации вывода](reference-azure-machine-learning-cli.md#inference-configuration-schema). Дополнительные сведения о скриптах ввода см. в разделе [Развертывание моделей с помощью машинное обучение Azure](how-to-deploy-and-where.md#prepare-to-deploy).

`aciDeploymentConfig.yml` описывает среду развертывания, используемую для размещения службы. Конфигурация развертывания зависит от типа вычислений, используемого для развертывания. В этом случае используется экземпляр контейнера Azure. Дополнительные сведения см. в статье [Схема конфигурации развертывания](reference-azure-machine-learning-cli.md#deployment-configuration-schema).

До завершения процесса развертывания займет несколько минут.

> [!TIP]
> В этом примере используется служба "экземпляры контейнеров Azure". Развертывания в ACI автоматически создают необходимый ресурс ACI. Если вместо этого развернуть в службу Azure Kubernetes, необходимо заранее создать кластер AKS и указать его как часть команды `az ml model deploy`. Пример развертывания в AKS см. в статье [развертывание модели в кластере службы Kubernetes Azure](how-to-deploy-azure-kubernetes-service.md).

Через несколько минут будет возвращена информация, аналогичная приведенной ниже JSON:

```json
ACI service creation operation finished, operation "Succeeded"
{
  "computeType": "ACI",
  {...ommitted for space...}
  "runtimeType": null,
  "scoringUri": "http://6c061467-4e44-4f05-9db5-9f9a22ef7a5d.eastus2.azurecontainer.io/score",
  "state": "Healthy",
  "tags": "",
  "updatedAt": "2019-09-19T18:22:32.227401+00:00"
}
```

### <a name="the-scoring-uri"></a>URI оценки

`scoringUri`, возвращенный из развертывания, является конечной точкой RESTFUL для модели, развернутой в виде веб-службы. Этот URI также можно получить с помощью следующей команды:

```azurecli-interactive
az ml service show -n myservice
```

Эта команда возвращает тот же документ JSON, включая `scoringUri`.

Конечную точку RESTFUL можно использовать для отправки данных в службу. Сведения о создании клиентского приложения, отправляющего данные в службу, см. в разделе [использование машинное обучение Azureной модели, развернутой в качестве веб-службы](how-to-consume-web-service.md) .

### <a name="send-data-to-the-service"></a>Отправка данных в службу

Хотя можно создать клиентское приложение для вызова конечной точки, CLI машинного обучения предоставляет программу, которая может использоваться в качестве тестового клиента. Используйте следующую команду, чтобы отправить данные из файла `testdata.json` в службу:

```azurecli-interactive
az ml service run -n myservice -d @testdata.json
```

> [!TIP]
> При использовании PowerShell используйте следующую команду:
>
> ```powershell
> az ml service run -n myservice -d `@testdata.json
> ```

Ответ команды аналогичен `[ 3 ]`.

## <a name="clean-up-resources"></a>Очистка ресурсов

> [!IMPORTANT]
> Созданные ресурсы могут использоваться в качестве необходимых компонентов при работе с другими руководствами по Машинному обучению Azure.

### <a name="delete-deployed-service"></a>Удаление развернутой службы

Если вы планируете продолжить использование рабочей области Машинное обучение Azure, но хотите избавиться от развернутой службы, чтобы снизить затраты, используйте следующую команду:

```azurecli-interactive
az ml service delete -n myservice
```

Эта команда возвращает документ JSON, содержащий имя удаленной службы. Удаление службы может занять несколько минут.

### <a name="delete-the-training-compute"></a>Удаление обучающих вычислений

Если вы планируете продолжить использовать рабочую область Машинное обучение Azure, но хотите избавиться от `cpu-compute`ного целевого объекта вычислений, созданного для обучения, используйте следующую команду:

```azurecli-interactive
az ml computetarget delete -n cpu
```

Эта команда возвращает документ JSON, содержащий идентификатор удаленного целевого объекта вычислений. Удаление целевого объекта вычислений может занять несколько минут.

### <a name="delete-everything"></a>Удаление всех ресурсов

Если вы не планируете использовать созданные ресурсы, удалите их, чтобы не взиматься дополнительная плата.

Чтобы удалить группу ресурсов и все ресурсы Azure, созданные в этом документе, используйте следующую команду. Замените `<resource-group-name>` именем группы ресурсов, созданной ранее.

```azurecli-interactive
az group delete -g <resource-group-name> -y
```

## <a name="next-steps"></a>Дальнейшие действия

В этом Машинное обучение Azure руководстве вы использовали CLI машинного обучения для следующих задач:

> [!div class="checklist"]
> * Установка расширения машинного обучения
> * Создание рабочей области машинного обучения Azure
> * Создание ресурса вычислений, используемого для обучения модели
> * Определение и регистрация набора данных, используемого для обучения модели
> * Начать обучающий запуск
> * Регистрация и скачивание модели
> * Развертывание модели в качестве веб-службы.
> * Оценка данных с помощью веб-службы

Дополнительные сведения об использовании интерфейса командной строки см. в статье [Использование расширения CLI для машинное обучение Azure](reference-azure-machine-learning-cli.md).
