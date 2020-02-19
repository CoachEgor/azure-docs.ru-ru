---
title: Безопасные эксперименты и вывод в виртуальной сети
titleSuffix: Azure Machine Learning
description: Узнайте, как обеспечить безопасность заданий экспериментирования и обучения, а кроме заданий вывода и оценки в Машинное обучение Azure в виртуальной сети Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 01/13/2020
ms.openlocfilehash: 6e5571604e6154408f2005ab4804b4270041e4cf
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444355"
---
# <a name="secure-azure-ml-experimentation-and-inference-jobs-within-an-azure-virtual-network"></a>Защита заданий экспериментирования и вывода машинного обучения Azure в виртуальной сети Azure
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

В этой статье вы узнаете, как защитить задания экспериментирования и обучения, а также задания определения и оценки в Машинное обучение Azure в виртуальной сети Azure.

**Виртуальная сеть** выступает в качестве границы безопасности, изолируя ресурсы Azure из общедоступного Интернета. Также вы можете подключить виртуальную сеть Azure к локальной сети. Присоединяясь к сетям, вы можете безопасно обучать модели и получать доступ к развернутым моделям для вывода.

Машинное обучение Azure полагается на другие службы Azure для ресурсов вычислений. Для обучения и развертывания моделей используются ресурсы вычислений или [целевые объекты вычислений](concept-compute-target.md). Целевые объекты можно создать в виртуальной сети. Например, виртуальную машину Майкрософт для обработки и анализа данных можно использовать для обучения модели, а затем развертывания модели в службе Azure Kubernetes Service (AKS). Дополнительные сведения о виртуальных сетях см. в статье [Общие сведения о виртуальной сети Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

В этой статье также содержатся подробные сведения о *дополнительных параметрах безопасности*, сведения, которые не требуются для основных или экспериментальных вариантов использования. В некоторых разделах этой статьи содержатся сведения о конфигурации для различных сценариев. Вам не нужно выполнять инструкции по порядку или целиком.

> [!TIP]
> Если не выдается специально, использование ресурсов, таких как учетные записи хранения или целевые объекты вычислений в виртуальной сети, будет работать с конвейерами машинного обучения, а не с конвейерными рабочими процессами, такими как запуски скриптов.

> [!WARNING]
> Корпорация Майкрософт не поддерживает использование конструктора Машинное обучение Azure или автоматизированного машинного обучения (из студии) с ресурсами внутри виртуальной сети.

## <a name="prerequisites"></a>предварительные требования

+ [Рабочая область](how-to-manage-workspace.md)машинное обучение Azure.

+ Общие сведения о [службе виртуальной сети Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) и [IP-сети](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm).

+ Предварительно Существующая виртуальная сеть и подсеть для использования с ресурсами вычислений.

## <a name="use-a-storage-account-for-your-workspace"></a>Использование учетной записи хранения для рабочей области

Чтобы использовать учетную запись хранения Azure для рабочей области в виртуальной сети, выполните следующие действия.

1. Создайте вычислительный ресурс (например, Машинное обучение вычислительного экземпляра или кластера) за виртуальной сетью или подключите вычислительный ресурс к рабочей области (например, кластер HDInsight, виртуальную машину или кластер службы Azure Kubernetes). Ресурс вычислений может быть для эксперимента или развертывания модели.

   Дополнительные сведения см. в разделах [использование машинное обучение вычислений](#amlcompute), [Использование виртуальной машины или кластера HDInsight](#vmorhdi)и [Использование Azure Kubernetes Service](#aksvnet) в этой статье.

1. В портал Azure перейдите к хранилищу, подключенному к рабочей области.

   [![хранилища, подключенного к рабочей области Машинное обучение Azure](./media/how-to-enable-virtual-network/workspace-storage.png)](./media/how-to-enable-virtual-network/workspace-storage.png#lightbox)

1. На странице Служба **хранилища Azure** выберите __брандмауэры и виртуальные сети__.

   ![Область "брандмауэры и виртуальные сети" на странице службы хранилища Azure в портал Azure](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)

1. На странице __брандмауэры и виртуальные сети__ выполните следующие действия.
    - Выберите __Выбранные сети__.
    - В разделе __виртуальные сети__щелкните ссылку __Добавить существующую виртуальную сеть__ . Это действие добавляет виртуальную сеть, в которой размещается вычисление (см. шаг 1).

        > [!IMPORTANT]
        > Учетная запись хранения должна находиться в той же виртуальной сети, что и вычисленные экземпляры или кластеры, используемые для обучения или вывода.

    - Установите флажок __разрешить доверенным службам Майкрософт доступ к этой учетной записи хранения__ .

    > [!IMPORTANT]
    > При работе с пакетом SDK для Машинное обучение Azure среда разработки должна иметь возможность подключения к учетной записи хранения Azure. Если учетная запись хранения находится в виртуальной сети, брандмауэр должен разрешить доступ с IP-адреса среды разработки.
    >
    > Чтобы разрешить доступ к учетной записи хранения, посетите __брандмауэры и виртуальные сети__ для учетной записи хранения *из веб-браузера на клиенте разработки*. Затем используйте флажок __Добавить IP-адрес клиента__ , чтобы добавить IP-адрес клиента в __диапазон адресов__. Можно также использовать поле __диапазон адресов__ , чтобы вручную ввести IP-адрес среды разработки. После добавления IP-адреса клиента он может получить доступ к учетной записи хранения с помощью пакета SDK.

   [![области "брандмауэры и виртуальные сети" в портал Azure](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png#lightbox)

> [!IMPORTANT]
> Вы можете разместить _учетную запись хранения по умолчанию_ для машинное обучение Azure или _учетные записи хранения, отличные от используемых по умолчанию_ , в виртуальной сети.
>
> Учетная запись хранения по умолчанию автоматически подготавливается при создании рабочей области.
>
> Для учетных записей хранения, отличных от по умолчанию, параметр `storage_account` в [функции`Workspace.create()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-) позволяет указать настраиваемую учетную запись хранения по идентификатору ресурса Azure.

## <a name="use-azure-data-lake-storage-gen-2"></a>Использование Azure Data Lake Storage Gen 2

Azure Data Lake Storage Gen 2 — это набор возможностей для аналитики больших данных, построенных в хранилище BLOB-объектов Azure. Он может использоваться для хранения данных, используемых для обучения моделей с Машинное обучение Azure. 

Чтобы использовать Data Lake Storage Gen 2 в виртуальной сети Машинное обучение Azure рабочей области, выполните следующие действия.

1. Создайте учетную запись Azure Data Lake Storage Gen 2. Дополнительные сведения см. [в статье Создание учетной записи хранения Azure Data Lake Storage 2-го поколения](../storage/blobs/data-lake-storage-quickstart-create-account.md).

1. Выполните шаги 2-4 в предыдущем разделе, чтобы разместить учетную запись в виртуальной сети, [используя учетную запись хранения для рабочей области](#use-a-storage-account-for-your-workspace).

При использовании Машинное обучение Azure с Data Lake Storage Gen 2 в виртуальной сети используйте следующие рекомендации.

* Если __для создания набора данных используется пакет SDK__, а система, в которой выполняется код, отсутствует __в виртуальной сети__, используйте параметр `validate=False`. Этот параметр пропускает проверку, которая завершается ошибкой, если система не находится в той же виртуальной сети, что и учетная запись хранения. Дополнительные сведения см. в описании метода [from_files ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) .

* При использовании Машинное обучение Azure вычислительного экземпляра или вычислительного кластера для обучения модели с помощью набора данных она должна находиться в той же виртуальной сети, что и учетная запись хранения.

## <a name="use-a-key-vault-instance-with-your-workspace"></a>Использование экземпляра хранилища ключей с рабочей областью

Экземпляр хранилища ключей, связанный с рабочей областью, используется Машинное обучение Azure для хранения следующих учетных данных:
* Строка подключения связанной учетной записи хранения
* Пароли для экземпляров репозитория контейнеров Azure
* Строки подключения к хранилищам данных

Чтобы использовать Машинное обучение Azure возможности экспериментов с Azure Key Vault за виртуальной сетью, выполните следующие действия.

1. Перейдите в хранилище ключей, связанное с рабочей областью.

   [![хранилище ключей, связанное с рабочей областью Машинное обучение Azure](./media/how-to-enable-virtual-network/workspace-key-vault.png)](./media/how-to-enable-virtual-network/workspace-key-vault.png#lightbox)

1. На странице **Key Vault** в левой области выберите __брандмауэры и виртуальные сети__.

   ![Раздел "брандмауэры и виртуальные сети" в области Key Vault](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)

1. На странице __брандмауэры и виртуальные сети__ выполните следующие действия.
    - В разделе __Разрешить доступ из__ щелкните __Выбранные сети__.
    - В разделе __виртуальные сети__выберите __Добавить существующие виртуальные сети__ , чтобы добавить виртуальную сеть, в которой находится вычисление экспериментов.
    - В разделе __разрешить доверенным службам Майкрософт обходить этот брандмауэр__выберите __Да__.

   [![раздел "брандмауэры и виртуальные сети" в области Key Vault](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png#lightbox)

<a id="amlcompute"></a>

## <a name="compute-instance"></a>Использование Вычислительная среда Машинного обучения

Чтобы использовать Машинное обучение Azure вычислительный экземпляр или вычислительный кластер в виртуальной сети, необходимо соблюдать следующие требования к сети.

> [!div class="checklist"]
> * Виртуальная сеть должна находиться в той же подписке и регионе, что и Рабочая область Машинное обучение Azure.
> * Подсеть, указанная для вычислительного экземпляра или кластера, должна иметь достаточное число неназначенных IP-адресов для размещения целевых виртуальных машин. Если в подсети недостаточно неназначенных IP-адресов, то кластер будет частично распределен.
> * Убедитесь, что политики безопасности или блокировки в подписке или группе ресурсов виртуальной сети ограничивают разрешения для управления виртуальной сетью. Если вы планируете защитить виртуальную сеть путем ограниченного трафика, оставьте некоторые порты открытыми для службы вычислений. Дополнительные сведения см. в разделе [Требуемые порты](#mlcports).
> * Если вы планируете разместить несколько вычислений или кластеров в одной виртуальной сети, может потребоваться увеличить квоту для одного или нескольких ресурсов.
> * Если учетные записи хранения Azure для рабочей области также защищены в виртуальной сети, они должны находиться в той же виртуальной сети, что и Машинное обучение Azure вычислительного экземпляра или кластера. 

> [!TIP]
> Машинное обучение вычислительный экземпляр или кластер автоматически выделяет дополнительные сетевые ресурсы в группе ресурсов, содержащей виртуальную сеть. Для каждого вычислительного экземпляра или кластера служба выделяет следующие ресурсы:
> 
> * Одна группа безопасности сети
> * Один общедоступный IP-адрес
> * Один балансировщик нагрузки
> 
> Эти ресурсы ограничены [квотами ресурсов](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) в подписке.


### <a id="mlcports"></a> Требуемые порты

Сейчас Вычислительная среда Машинного обучения использует пакетную службу Azure для подготовки виртуальных машин в указанной виртуальной сети. Это означает, что подсеть должна разрешать входящие подключения из пакетной службы. Это взаимодействие можно использовать для планирования выполнения на узлах Вычислительная среда Машинного обучения, а также для взаимодействия с хранилищем Azure и другими ресурсами. Пакетная служба добавляет группы безопасности сети (группы безопасности сети) на уровне сетевых интерфейсов (NIC), подключенных к виртуальным машинам. Эти группы безопасности сети автоматически настраивают правила для входящего и исходящего трафика, чтобы разрешить следующий трафик:

- Входящий TCP-трафик на портах 29876 и 29877 из __тега службы__ __батчнодеманажемент__.

    ![Правило входящего трафика, использующее тег службы Батчнодеманажемент](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)

- Используемых Входящий TCP-трафик через порт 22 для разрешения удаленного доступа. Используйте этот порт только в том случае, если вы хотите подключиться по протоколу SSH на общедоступном IP-адресе.

- Исходящий трафик в виртуальную сеть на любом порту.

- Исходящий трафик в Интернет на любом порту.

- Для вычислительного экземпляра входящий TCP-трафик через порт 44224 из __тега службы__ __азуремачинелеарнинг__.

Соблюдайте осторожность, если вы изменяете или добавляете правила для входящего и исходящего трафика в группах безопасности сети, настроенных для пакетной службы. Если NSG блокирует обмен данными с вычисленными узлами, Служба вычислений устанавливает для них состояние "непригоден для использования".

Вам не нужно указывать группы безопасности сети на уровне подсети, так как пакетная служба Azure настраивает собственный группы безопасности сети. Однако если указанная подсеть связана с группы безопасности сети или брандмауэром, настройте правила безопасности для входящего и исходящего трафика, как упоминалось ранее.

Конфигурация правила NSG в портал Azure показана на следующих изображениях:

[![правила NSG для входящих подключений для Вычислительная среда Машинного обучения](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png#lightbox)

![Правила исходящего NSG для Вычислительная среда Машинного обучения](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a id="limiting-outbound-from-vnet"></a>Ограничение исходящего подключения из виртуальной сети

Если вы не хотите использовать правила исходящего трафика по умолчанию и вы хотите ограничить исходящий доступ к виртуальной сети, выполните следующие действия.

- Запрет исходящего подключения к Интернету с помощью правил NSG.

- Для __вычислительного экземпляра__ или __вычислительного кластера__Ограничьте исходящий трафик следующими элементами:
   - Служба хранилища Azure с помощью __тега службы__ __Storage. RegionName__. Где `{RegionName}` — имя региона Azure.
   - Реестр контейнеров Azure с помощью __тега службы__ __азуреконтаинеррегистри. RegionName__. Где `{RegionName}` — имя региона Azure.
   - Машинное обучение Azure с помощью __тега службы__ __азуремачинелеарнинг__
   
- Для __вычислительного экземпляра__также добавьте следующие элементы:
   - Azure Resource Manager с помощью __тега службы__ __AzureResourceManager__
   - Azure Active Directory с помощью __тега службы__ __AzureActiveDirectory__

Конфигурация правила NSG на портал Azure показана на следующем рисунке:

[![правила исходящего NSG для Вычислительная среда Машинного обучения](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png#lightbox)

> [!NOTE]
> Если вы планируете использовать образы DOCKER по умолчанию, предоставляемые корпорацией Майкрософт, и включите управляемые пользователем зависимости, необходимо также использовать __тег службы__ __микрософтконтаинеррегистри. Region_Name__ (например, микрософтконтаинеррегистри. EastUS).
>
> Эта конфигурация необходима при наличии кода, аналогичного приведенным ниже, в рамках сценариев обучения.
>
> __Обучение RunConfig__
> ```python
> # create a new runconfig object
> run_config = RunConfiguration()
> 
> # configure Docker 
> run_config.environment.docker.enabled = True
> # For GPU, use DEFAULT_GPU_IMAGE
> run_config.environment.docker.base_image = DEFAULT_CPU_IMAGE 
> run_config.environment.python.user_managed_dependencies = True
> ```
>
> __Обучение средства оценки__
> ```python
> est = Estimator(source_directory='.',
>                 script_params=script_params,
>                 compute_target='local',
>                 entry_script='dummy_train.py',
>                 user_managed=True)
> run = exp.submit(est)
> ```

### <a name="user-defined-routes-for-forced-tunneling"></a>Пользовательские маршруты для принудительного туннелирования

Если вы используете принудительное туннелирование с Вычислительная среда Машинного обучения, добавьте [определяемые пользователем маршруты (определяемые пользователем маршруты)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) в подсеть, содержащую ресурс вычислений.

* Установите UDR для каждого IP-адреса, используемого пакетной службой Azure в регионе, где находятся ваши ресурсы. Эти определяемые пользователем маршруты позволяют пакетной службе взаимодействовать с вычисленными узлами для планирования задач. Чтобы получить список IP-адресов пакетной службы, используйте один из следующих методов.

    * Скачайте [диапазоны IP-адресов и теги службы Azure](https://www.microsoft.com/download/details.aspx?id=56519) и найдите в файле `BatchNodeManagement.<region>`, где `<region>` является вашим регионом Azure.

    * Для загрузки сведений используйте [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) . В следующем примере загружаются сведения об IP-адресе и отфильтровываются сведения для региона "Восточная часть США 2":

        ```azurecli-interactive
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='eastus2']"
        ```

* Исходящий трафик в службу хранилища Azure не должен блокироваться локальным сетевым устройством. В частности, URL-адреса находятся в форме `<account>.table.core.windows.net`, `<account>.queue.core.windows.net`и `<account>.blob.core.windows.net`.

При добавлении определяемые пользователем маршруты определите маршрут для каждого соответствующего префикса IP-адреса пакета и задайте для параметра __тип следующего прыжка__ значение __Интернет__. На следующем рисунке показан пример UDR в портал Azure:

![Пример UDR для префикса адреса](./media/how-to-enable-virtual-network/user-defined-route.png)

Дополнительные сведения см. [в статье Создание пула пакетной службы Azure в виртуальной сети](../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling).

### <a name="create-a-compute-cluster-in-a-virtual-network"></a>Создание кластера вычислений в виртуальной сети

Чтобы создать кластер Вычислительная среда Машинного обучения, выполните следующие действия.

1. В [портал Azure](https://portal.azure.com)выберите рабочую область машинное обучение Azure.

1. В разделе __приложение__ выберите пункт __Вычисление__и щелкните __Добавить вычисление__.

1. Чтобы настроить этот ресурс вычислений для использования виртуальной сети, выполните следующие действия.

    а. В качестве __конфигурации сети__выберите __Дополнительно__.

    b. В раскрывающемся списке __Группа ресурсов__ выберите группу ресурсов, содержащую виртуальную сеть.

    c. В раскрывающемся списке __Виртуальная сеть__ выберите виртуальную сеть, содержащую подсеть.

    d. В раскрывающемся списке __подсеть__ выберите подсеть для использования.

   ![Параметры виртуальной сети для Вычислительная среда Машинного обучения](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Вы также можете создать кластер Вычислительной среды Машинного обучения с помощью пакета SDK для Машинного обучения Azure. Следующий код создает новый кластер Вычислительной среды Машинного обучения в подсети `default`, размещенной в виртуальной сети с именем `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure virtual network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")

    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name=vnet_resourcegroup_name,
                                                           vnet_name=vnet_name,
                                                           subnet_name=subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

    # Wait for the cluster to be completed, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

По завершении процесса создания вы обучите модель с помощью кластера в эксперименте. Дополнительные сведения вы найдете в статье [Настройка целевых объектов вычислений для обучения моделей](how-to-set-up-training-targets.md).

## <a name="use-azure-databricks"></a>Использование Azure Databricks

Чтобы использовать Azure Databricks в виртуальной сети с рабочей областью, должны выполняться следующие требования.

> [!div class="checklist"]
> * Виртуальная сеть должна находиться в той же подписке и регионе, что и Рабочая область Машинное обучение Azure.
> * Если учетные записи хранения Azure для рабочей области также защищены в виртуальной сети, они должны находиться в той же виртуальной сети, что и кластер Azure Databricks.
> * В дополнение к __модулям "данные — частные__ " и " __кирпичы" — общедоступные__ подсети, используемые Azure Databricks, также требуется подсеть __по умолчанию__ , созданная для виртуальной сети.

Конкретные сведения об использовании Azure Databricks с виртуальной сетью см. в статье [развертывание Azure Databricks в виртуальной сети Azure](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html).

<a id="vmorhdi"></a>

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>Использование виртуальной машины или кластера HDInsight

> [!IMPORTANT]
> Машинное обучение Azure поддерживает только виртуальные машины под управлением Ubuntu.

Чтобы использовать виртуальную машину или кластер Azure HDInsight в виртуальной сети с рабочей областью, выполните следующие действия.

1. Создайте виртуальную машину или кластер HDInsight с помощью портал Azure или Azure CLI и вставьте кластер в виртуальную сеть Azure. Дополнительные сведения см. в следующих статьях:
    * [Создание и администрирование виртуальных сетей Azure для виртуальных машин Linux](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Расширение возможностей HDInsight с помощью виртуальной сети Azure](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network).

1. Чтобы разрешить Машинное обучение Azure взаимодействовать с портом SSH на виртуальной машине или в кластере, настройте запись источника для группы безопасности сети. Обычно для SSH используется порт 22. Чтобы разрешить трафик из этого источника, выполните следующие действия.

    * В раскрывающемся списке __источник__ выберите __тег службы__.

    * В раскрывающемся списке __тег исходной службы__ выберите __азуремачинелеарнинг__.

    * В раскрывающемся списке __диапазоны исходных портов__ выберите __*__ .

    * В раскрывающемся списке __назначение__ выберите __любой__.

    * В раскрывающемся списке __диапазоны портов назначения выберите значение__ __22__.

    * В разделе __протокол__выберите __любой__.

    * В разделе __действие__выберите __Разрешить__.

   ![Правила для входящих подключений для экспериментов на виртуальной машине или в кластере HDInsight в виртуальной сети](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Примите правила исходящего трафика по умолчанию для группы безопасности сети. Дополнительные сведения см. в описании стандартных правил безопасности в статье [о группах безопасности](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules).

    Если вы не хотите использовать правила исходящего трафика по умолчанию и вы хотите ограничить исходящий доступ к виртуальной сети, см. раздел [ограничение исходящего подключения в виртуальной сети](#limiting-outbound-from-vnet) .

1. Подключите виртуальную машину или кластер HDInsight к рабочей области Машинное обучение Azure. Дополнительные сведения вы найдете в статье [Настройка целевых объектов вычислений для обучения моделей](how-to-set-up-training-targets.md).

<a id="aksvnet"></a>

## <a name="use-azure-kubernetes-service-aks"></a>Применение службы Azure Kubernetes (AKS)

Чтобы добавить AKS в виртуальную сеть в рабочую область, выполните следующие действия.

> [!IMPORTANT]
> Прежде чем приступить к описанной ниже процедуре, выполните предварительные требования в разделе [Настройка расширенных сетевых возможностей в Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/configure-advanced-networking#prerequisites) и планирование IP-адресации для кластера.
>
> Экземпляр AKS и виртуальная сеть Azure должны находиться в одном регионе. Если вы защищаете учетные записи хранения Azure, используемые рабочей областью в виртуальной сети, они должны находиться в той же виртуальной сети, что и экземпляр AKS.

1. В [портал Azure](https://portal.azure.com)убедитесь, что NSG, управляющий виртуальной сетью, имеет правило входящего трафика, которое включено для машинное обучение Azure с помощью __Азуремачинелеарнинг__ в качестве **источника**.

    [![Машинное обучение Azure добавить панель вычислений](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png#lightbox)

1. Выберите рабочую область Машинное обучение Azure.

1. В разделе __приложение__ выберите пункт __Вычисление__и щелкните __Добавить вычисление__.

1. Чтобы настроить этот ресурс вычислений для использования виртуальной сети, выполните следующие действия.

    - В качестве __конфигурации сети__выберите __Дополнительно__.

    - В раскрывающемся списке __Группа ресурсов__ выберите группу ресурсов, содержащую виртуальную сеть.

    - В раскрывающемся списке __Виртуальная сеть__ выберите виртуальную сеть, содержащую подсеть.

    - В раскрывающемся списке __подсеть__ выберите подсеть.

    - В поле __диапазон адресов службы Kubernetes__ введите диапазон адресов службы Kubernetes. Этот диапазон адресов использует диапазон IP-адресов нотации, определяющий междоменную маршрутизацию (CIDR), чтобы определить IP-адреса, доступные для кластера. Он не должен пересекаться с диапазонами IP-адресов подсети (например, 10.0.0.0/16).

    - В поле __IP-адрес службы DNS Kubernetes__ введите IP-адрес службы DNS Kubernetes. Это IP-адрес, назначенный службе DNS Kubernetes. Он должен находиться в диапазоне адресов службы Kubernetes (например, 10.0.0.10).

    - В поле __адрес моста DOCKER__ введите адрес моста DOCKER. Этот IP-адрес, назначенный мосту Docker. Он не должен находиться в диапазонах IP-адресов подсети или в диапазоне номеров служб Kubernetes (например, 172.17.0.1/16).

   ![Машинное обучение Azure: параметры Вычислительная среда Машинного обучения виртуальной сети](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Убедитесь, что в группе NSG, управляющей виртуальной сетью, включено правило безопасности входящего трафика для конечной точки оценки, чтобы ее можно было вызывать извне виртуальной сети.
   > [!IMPORTANT]
   > Сохраните правила по умолчанию для исходящего трафика для группы безопасности сети. Дополнительные сведения см. в описании стандартных правил безопасности в статье [о группах безопасности](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules).

   [![правила безопасности для входящего трафика](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png#lightbox)

Вы также можете добавить службу Kubernetes Azure в виртуальную сеть с помощью пакета SDK для Машинное обучение Azure. Если у вас уже есть кластер AKS в виртуальной сети, подключите его к рабочей области, как описано в разделе [развертывание в AKS](how-to-deploy-and-where.md). Следующий код создает новый экземпляр AKS в подсети `default` виртуальной сети с именем `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace=ws,
                                  name="myaks",
                                  provisioning_configuration=config)
```

После завершения процесса создания можно выполнить вывод или оценку модели в кластере AKS за пределами виртуальной сети. Дополнительные сведения см. в статье [о развертывании в Службе Azure Kubernetes](how-to-deploy-and-where.md).

### <a name="use-private-ips-with-azure-kubernetes-service"></a>Использование частных IP-адресов со службой Kubernetes Azure

По умолчанию для развертываний AKS назначается общедоступный IP-адрес. При использовании AKS внутри виртуальной сети можно использовать вместо него частный IP-адрес. Частные IP-адреса доступны только из виртуальной сети или из Объединенных сетей.

Частный IP-адрес включается путем настройки AKS для использования _внутренней подсистемы балансировки нагрузки_. 

> [!IMPORTANT]
> Вы не можете включить частный IP-адрес при создании кластера службы Kubernetes Azure. Он должен быть включен в качестве обновления существующего кластера.

В следующем фрагменте кода показано, как **создать новый кластер AKS**, а затем обновить его для использования частного IP-или внутреннего балансировщика нагрузки:

```python
import azureml.core
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute import AksCompute, ComputeTarget

# Verify that cluster does not exist already
try:
    aks_target = AksCompute(workspace=ws, name=aks_cluster_name)
    print("Found existing aks cluster")

except:
    print("Creating new aks cluster")

    # Create AKS configuration
    prov_config = AksCompute.provisioning_configuration(location = "eastus2")
    # Set info for existing virtual network to create the cluster in
    prov_config.vnet_resourcegroup_name = "myvnetresourcegroup"
    prov_config.vnet_name = "myvnetname"
    prov_config.service_cidr = "10.0.0.0/16"
    prov_config.dns_service_ip = "10.0.0.10"
    prov_config.subnet_name = "default"
    prov_config.docker_bridge_cidr = "172.17.0.1/16"

    # Create compute target
    aks_target = ComputeTarget.create(workspace = ws, name = “myaks”, provisioning_configuration = prov_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
    
    # Update AKS configuration to use an internal load balancer
    update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", "default")
    aks_target.update(update_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
```

__Azure CLI__

```azurecli-interactive
az rest --method put --uri https://management.azure.com"/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.ContainerService/managedClusters/<aks-resource-id>?api-version=2018-11-19 --body @body.json
```

Содержимое файла `body.json`, на которое ссылается команда, аналогично следующему документу JSON:

```json
{ 
    "location": “<region>”, 
    "properties": { 
        "resourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.ContainerService/managedClusters/<aks-resource-id>", 
        "computeType": "AKS", 
        "provisioningState": "Succeeded", 
        "properties": { 
            "loadBalancerType": "InternalLoadBalancer", 
            "agentCount": <agent-count>, 
            "agentVmSize": "vm-size", 
            "clusterFqdn": "<cluster-fqdn>" 
        } 
    } 
} 
```

> [!NOTE]
> В настоящее время подсистема балансировки нагрузки не может быть настроена при выполнении операции __присоединения__ к существующему кластеру. Необходимо сначала подключить кластер, а затем выполнить операцию обновления, чтобы изменить подсистему балансировки нагрузки.

Дополнительные сведения об использовании внутренней подсистемы балансировки нагрузки с AKS см. в статье [использование внутренней подсистемы балансировки нагрузки со службой Kubernetes Azure](/azure/aks/internal-lb).

## <a name="use-azure-firewall"></a>Использование брандмауэра Azure

При использовании брандмауэра Azure необходимо настроить сетевое правило, разрешающее входящий и исходящий трафик из следующих адресов:

- `*.batchai.core.windows.net`
- `ml.azure.com`
- `*.azureml.ms`
- `*.experiments.azureml.net`
- `*.modelmanagement.azureml.net`
- `mlworkspace.azure.ai`
- `*.aether.ms`

При добавлении правила задайте для __протокола__ значение Any и порты для `*`.

Дополнительные сведения о настройке правила сети см. в статье [развертывание и настройка брандмауэра Azure](/azure/firewall/tutorial-firewall-deploy-portal#configure-a-network-rule).

## <a name="use-azure-container-registry"></a>Использование реестра контейнеров Azure

При использовании виртуальной сети с __машинное обучение Azure не размещайте реестр__ контейнеров Azure для рабочей области в виртуальной сети. Эта конфигурация не поддерживается.

## <a name="next-steps"></a>Дальнейшие действия

* [Настройка сред обучения](how-to-set-up-training-targets.md)
* [Где следует развертывать модели](how-to-deploy-and-where.md)
* [Безопасное развертывание моделей с помощью SSL](how-to-secure-web-service.md)
