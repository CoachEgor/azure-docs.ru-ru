---
title: Краткое руководство Azure по выполнению пакетного задания с помощью CLI
description: Быстро научитесь выполнять пакетное задание с помощью Azure CLI.
services: batch
author: ju-shim
manager: gwallace
ms.service: batch
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 07/03/2018
ms.author: jushiman
ms.custom: mvc
ms.openlocfilehash: 900bafd5b27fcb5021e9dae2a6bbc13d4e233a45
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029478"
---
# <a name="quickstart-run-your-first-batch-job-with-the-azure-cli"></a>Краткое руководство. Выполнение пакетного задания с помощью Azure CLI

Azure CLI используется для создания ресурсов Azure и управления ими из командной строки или с помощью скриптов. В этом кратком руководстве показано, как использовать Azure CLI для создания учетной записи пакетной службы, *пула* вычислительных узлов (виртуальных машин) и *заданий*, выполняющих *задачи* в пуле. Каждый образец задачи запускает основную команду на одном из узлов пула. Выполняя действия из этого краткого руководства, вы изучите основные понятия пакетной службы и сможете использовать ее с более реалистичными рабочими нагрузками в большем масштабе.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать CLI локально, для выполнения инструкций из этого руководства вам потребуется Azure CLI 2.0.20 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Создайте группу ресурсов с помощью команды [az group create](/cli/azure/group#az-group-create). Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими. 

В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *eastus2*.

```azurecli-interactive 
az group create \
    --name myResourceGroup \
    --location eastus2
```

## <a name="create-a-storage-account"></a>Создание учетной записи хранения

Учетную запись службы хранилища Azure можно связать с учетной записью пакетной службы. Хоть наличие учетной записи хранения и не является обязательным условием для работы с этим кратким руководством, она применяется при развертывании приложений и для хранения входных и выходных данных в большинстве реальных рабочих нагрузок. В группе ресурсов создайте учетную запись хранения с помощью команды [az storage account create](/cli/azure/storage/account#az-storage-account-create).

```azurecli-interactive
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus2 \
    --sku Standard_LRS
```

## <a name="create-a-batch-account"></a>Создание учетной записи Пакетной службы

Создайте учетную запись пакетной службы с помощью команды [az batch account create](/cli/azure/batch/account#az-batch-account-create). Для создания вычислительных ресурсов (пулов вычислительных узлов) и пакетных заданий необходима учетная запись.

В следующем примере создается учетная запись пакетной службы с именем *mybatchaccount* в группе ресурсов *myResourceGroup*. Она связывается с созданной учетной записью хранения.  

```azurecli-interactive 
az batch account create \
    --name mybatchaccount \
    --storage-account mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus2
```

Для создания пулов вычислений и заданий, а также управления ими необходимо выполнить проверку подлинности с помощью пакетной службы. Войдите в учетную запись с помощью команды [az batch account login](/cli/azure/batch/account#az-batch-account-login). После входа ваши команды `az batch` используют контекст учетной записи.

```azurecli-interactive 
az batch account login \
    --name mybatchaccount \
    --resource-group myResourceGroup \
    --shared-key-auth
```

## <a name="create-a-pool-of-compute-nodes"></a>Создание пула вычислительных узлов

Получив учетную запись пакетной службы, создайте образец пула вычислительных узлов Linux с помощью команды [az batch pool create](/cli/azure/batch/pool#az-batch-pool-create). В следующем примере создается пул *mypool* с 2 узлами размером *Standard_A1_v2* под управлением Ubuntu 16.04 LTS. Предлагаемый размер узла в этом кратком руководстве обеспечивает оптимальный баланс производительности и стоимости.
 
```azurecli-interactive
az batch pool create \
    --id mypool --vm-size Standard_A1_v2 \
    --target-dedicated-nodes 2 \
    --image canonical:ubuntuserver:16.04-LTS \
    --node-agent-sku-id "batch.node.ubuntu 16.04" 
```

В пакетной службе сразу же создается пул, но для выделения и запуска вычислительных узлов понадобится несколько минут. В течение этого времени пул находится в состоянии `resizing`. Чтобы увидеть состояние пула, выполните команду [az batch pool show](/cli/azure/batch/pool#az-batch-pool-show). Эта команда отображает все свойства пула, и вы можете запрашивать определенные свойства. С помощью следующей команды можно получить сведения о состоянии распределения пула:

```azurecli-interactive
az batch pool show --pool-id mypool \
    --query "allocationState"
```

Для создания задания и задач при изменении состояния пула выполните следующие шаги. Пул готов к выполнению задач, если состояние распределения — `steady` и все узлы работают. 

## <a name="create-a-job"></a>Создание задания

Получив пул, создайте задание, которое будет выполняться в нем.  Пакетное задание — это логическая группа для одной или нескольких задач. Задание включает в себя параметры, общие для задач (например, приоритет и пул для запуска задач). Создайте пакетное задание, используя команду [az batch job create](/cli/azure/batch/job#az-batch-job-create). В следующем примере создается задание *myjob* в пуле *mypool*. Изначально у задания нет задач.

```azurecli-interactive 
az batch job create \
    --id myjob \
    --pool-id mypool
```

## <a name="create-tasks"></a>Создание задач

Теперь с помощью команды [az batch task create](/cli/azure/batch/task#az-batch-task-create) создайте задачи для выполнения в задании. В этом примере создается четыре идентичные задачи. Каждая задача выполняет `command-line` для отображения переменных среды пакетной службы на вычислительном узле, а затем ждет 90 секунд. При использовании пакетной службы в этой командной строке задайте приложение или скрипт. В пакетной службе предусмотрен ряд способов для развертывания приложений и скриптов на вычислительных узлах.

Следующий сценарий Bash создает 4 параллельных задачи (с *mytask1* до *mytask4*).

```azurecli-interactive 
for i in {1..4}
do
   az batch task create \
    --task-id mytask$i \
    --job-id myjob \
    --command-line "/bin/bash -c 'printenv | grep AZ_BATCH; sleep 90s'"
done
```

В выходных данных этой команды предоставлены параметры для каждой из задач. Пакетная служба распределяет задачи по вычислительным узлам.

## <a name="view-task-status"></a>Представление состояния задачи

После создания задачи пакетная служба помещает ее в очередь для запуска в пуле. Если узел доступен для запуска, задание выполняется.

Просмотреть состояние пакетных задач можно с помощью команды [az batch task show](/cli/azure/batch/task#az-batch-task-show). В следующем примере показаны подробные сведения о задаче *mytask1*, выполняемой на одном из узлов пула.

```azurecli-interactive 
az batch task show \
    --job-id myjob \
    --task-id mytask1
```

Выходные данные команды включают в себя многие сведения, но обратите внимание на значение `exitCode` командной строки задачи и `nodeId`. Если `exitCode` имеет значение 0, это говорит об успешном завершении задачи командной строки. `nodeId` указывает идентификатор пула узла, в котором выполнялась задача.

## <a name="view-task-output"></a>Просмотр выходных данных задачи

Чтобы получить список файлов, созданных задачей на вычислительном узле, используйте команду [az batch task file list](/cli/azure/batch/task). Следующая команда создает список файлов, созданных при выполнении задачи *mytask1*: 

```azurecli-interactive 
az batch task file list \
    --job-id myjob \
    --task-id mytask1 \
    --output table
```

Результат аналогичен приведенному ниже:

```
Name        URL                                                                                         Is Directory      Content Length
----------  ------------------------------------------------------------------------------------------  --------------  ----------------
stdout.txt  https://mybatchaccount.eastus2.batch.azure.com/jobs/myjob/tasks/mytask1/files/stdout.txt  False                  695
certs       https://mybatchaccount.eastus2.batch.azure.com/jobs/myjob/tasks/mytask1/files/certs       True
wd          https://mybatchaccount.eastus2.batch.azure.com/jobs/myjob/tasks/mytask1/files/wd          True
stderr.txt  https://mybatchaccount.eastus2.batch.azure.com/jobs/myjob/tasks/mytask1/files/stderr.txt  False                     0

```

Чтобы скачать один из выходных файлов в локальный каталог, используйте команду [az batch task file download](/cli/azure/batch/task). В этом примере выходные данные задачи находятся в файле `stdout.txt`. 

```azurecli-interactive
az batch task file download \
    --job-id myjob \
    --task-id mytask1 \
    --file-path stdout.txt \
    --destination ./stdout.txt
```

Вы можете просмотреть содержимое файла `stdout.txt` в текстовом редакторе. В этом файле содержатся сведения о переменных среды пакетной службы, установленных на узле. При создании пакетных заданий вы можете ссылаться на них в командных строках задач, а также в приложениях и скриптах, запускаемых этими командными строками. Пример:

```
AZ_BATCH_TASK_DIR=/mnt/batch/tasks/workitems/myjob/job-1/mytask1
AZ_BATCH_NODE_STARTUP_DIR=/mnt/batch/tasks/startup
AZ_BATCH_CERTIFICATES_DIR=/mnt/batch/tasks/workitems/myjob/job-1/mytask1/certs
AZ_BATCH_ACCOUNT_URL=https://mybatchaccount.eastus2.batch.azure.com/
AZ_BATCH_TASK_WORKING_DIR=/mnt/batch/tasks/workitems/myjob/job-1/mytask1/wd
AZ_BATCH_NODE_SHARED_DIR=/mnt/batch/tasks/shared
AZ_BATCH_TASK_USER=_azbatch
AZ_BATCH_NODE_ROOT_DIR=/mnt/batch/tasks
AZ_BATCH_JOB_ID=myjobl
AZ_BATCH_NODE_IS_DEDICATED=true
AZ_BATCH_NODE_ID=tvm-257509324_2-20180703t215033z
AZ_BATCH_POOL_ID=mypool
AZ_BATCH_TASK_ID=mytask1
AZ_BATCH_ACCOUNT_NAME=mybatchaccount
AZ_BATCH_TASK_USER_IDENTITY=PoolNonAdmin
```
## <a name="clean-up-resources"></a>Очистка ресурсов
Если вы хотите продолжить изучение примеров и руководств по пакетной службе, используйте учетную запись пакетной службы и связанную учетную запись хранения, созданную при выполнении инструкций в этом кратком руководстве. Плата за использование самой пакетной службы не взимается.

Но если во время работы узлов используются пулы, плата взимается, даже если задания не запланированы. Если пул больше не нужен, удалите его, используя команду [az batch pool delete](/cli/azure/batch/pool#az-batch-pool-delete). При удалении пула удаляются все выходные данные задачи на узлах. 

```azurecli-interactive
az batch pool delete --pool-id mypool
```

Вы можете удалить ставшие ненужными группу ресурсов, учетную запись пакетной службы, пулы и все связанные с ними ресурсы, выполнив команду [az group delete](/cli/azure/group#az-group-delete). Удалите ресурсы следующим образом:

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Дальнейшие действия

В этом кратком руководстве вы создали учетную запись и пул пакетной службы, а также пакетное задание. Это задание запустило образцы задач, и вы просмотрели выходные данные на одном узле. Изучив основные понятия пакетной службы, вы сможете использовать ее с более реалистичными рабочими нагрузками в большем масштабе. Чтобы узнать больше о пакетной службе Azure, изучите следующие руководства. 


> [!div class="nextstepaction"]
> [Руководства по пакетной службе Azure](./tutorial-parallel-dotnet.md)
