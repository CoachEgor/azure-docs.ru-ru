---
title: Создание Функций Azure на Linux с помощью пользовательского образа
description: Узнайте, как создавать функции Azure под управлением пользовательского образа Linux.
author: ggailey777
ms.author: glenga
ms.date: 06/25/2019
ms.topic: tutorial
ms.service: azure-functions
ms.custom: mvc
manager: gwallace
ms.openlocfilehash: 80f7185b69a7953656235d3bd622b7f61611de1a
ms.sourcegitcommit: d470d4e295bf29a4acf7836ece2f10dabe8e6db2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/02/2019
ms.locfileid: "70210177"
---
# <a name="create-a-function-on-linux-using-a-custom-image"></a>Создание функции на Linux с помощью пользовательского образа

Решение "Функции Azure" позволяет размещать в Linux собственные функции в пользовательском контейнере. Кроме того, вы можете [использовать для размещения контейнер по умолчанию в службе приложений Azure](functions-create-first-azure-function-azure-cli-linux.md). Эта функциональность требует [среды выполнения Функций 2.x](functions-versions.md).

Из этого руководства вы узнаете, как развернуть функции в Azure в виде пользовательского образа Docker. Этот метод полезен, если вам нужно настроить встроенный образ контейнера. Пользовательский образ может быть удобен, если вы используете конкретную версию языка или особые зависимости и (или) конфигурации, не поддерживаемые в рамках встроенного образа. Поддерживаемые базовые образы для Функций Azure находятся в [репозитории базовых образов Функций Azure](https://hub.docker.com/_/microsoft-azure-functions-base). 

Из этого руководства вы узнаете, как с помощью Azure Functions Core Tools создать функцию в пользовательском образе Linux. Вы публикуете этот образ в приложении-функции Azure, которое было создано с помощью Azure CLI. Позже вы обновите функцию для подключения к Хранилищу очередей Azure. Вы также включите.  

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание приложения-функции и Dockerfile с помощью Core Tools.
> * Сборка пользовательского образа с помощью Docker.
> * Публикация пользовательского образа в реестре контейнеров.
> * Создание учетной записи хранения Azure.
> * Создание плана размещения "Премиум".
> * Развертывание приложения-функции из концентратора Docker.
> * Добавление параметров приложения в приложение-функцию.
> * Включение непрерывного развертывания.
> * Включение мониторинга Application Insights.

Описанные далее действия можно выполнять на компьютерах с Mac, Windows или Linux. 

## <a name="prerequisites"></a>Предварительные требования

Перед выполнением этого примера вам понадобится следующее:

* Установка [основных инструментов Azure версии 2.x](functions-run-local.md#v2).

* Установка [Azure CLI]( /cli/azure/install-azure-cli). В этой статье требуется Azure CLI 2.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`.  
Можно также использовать [Azure Cloud Shell](https://shell.azure.com/bash).

* Активная подписка Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Создание локального проекта приложения-функции

Выполните следующую команду из командной строки, чтобы создать проект приложения-функции в папке `MyFunctionProj` текущего локального каталога. Для проекта Python [необходимо работать в виртуальном окружении](functions-create-first-function-python.md#create-and-activate-a-virtual-environment-optional).

```bash
func init MyFunctionProj --docker
```

При включении параметра `--docker` для проекта создается dockerfile. Этот файл используется, чтобы создать пользовательский контейнер, в котором необходимо запустить проект. Используемый базовый образ зависит от выбранного языка среды выполнения рабочей роли.  

При появлении запроса выберите среду выполнения рабочей роли из следующих языков:

* `dotnet`. Создает проект библиотеки классов .NET Core (.csproj).
* `node`. Создает проект JavaScript.
* `python`. Создает проект Python.  

При выполнении команды вы увидите выходные данные примерно следующего содержания.

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing Dockerfile
```

Чтобы перейти к новой папке проекта `MyFunctionProj`, используйте следующую команду.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

## <a name="build-the-image-from-the-docker-file"></a>Создание образа на основе файла Docker

Просмотрите _Dockerfile_ в корневой папке проекта. В этом файле описана среда для запуска приложения-функции на платформе Linux. Следующий пример — Dockerfile. Он создает контейнер, который запускает приложение-функцию в среде выполнения рабочей роли JavaScript (Node.js): 

```Dockerfile
FROM mcr.microsoft.com/azure-functions/node:2.0

ENV AzureWebJobsScriptRoot=/home/site/wwwroot
COPY . /home/site/wwwroot
```

> [!NOTE]
> Полный список поддерживаемых базовых образов для Функций Azure можно найти на [странице базового образа Функций Azure](https://hub.docker.com/_/microsoft-azure-functions-base).

### <a name="run-the-build-command"></a>Выполните команду `build`.
В корневой папке выполните команду [docker build](https://docs.docker.com/engine/reference/commandline/build/), указав имя `mydockerimage` и тег `v1.0.0`. Замените `<docker-id>` идентификатором вашей учетной записи Docker Hub. Эта команда отвечает за создание образа Docker для контейнера.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

При выполнении команды вы увидите выходные данные, которые предоставляются для среды выполнения рабочей роли JavaScript, примерно следующего содержания:

```bash
Sending build context to Docker daemon  17.41kB
Step 1/3 : FROM mcr.microsoft.com/azure-functions/node:2.0
2.0: Pulling from azure-functions/node
802b00ed6f79: Pull complete
44580ea7a636: Pull complete
73eebe8d57f9: Pull complete
3d82a67477c2: Pull complete
8bd51cd50290: Pull complete
7bd755353966: Pull complete
Digest: sha256:480e969821e9befe7c61dda353f63298f2c4b109e13032df5518e92540ea1d08
Status: Downloaded newer image for mcr.microsoft.com/azure-functions/node:2.0
 ---> 7c71671b838f
Step 2/3 : ENV AzureWebJobsScriptRoot=/home/site/wwwroot
 ---> Running in ed1e5809f0b7
Removing intermediate container ed1e5809f0b7
 ---> 39d9c341368a
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5e196215935a
Successfully built 5e196215935a
Successfully tagged <docker-id>/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>Локальное тестирование образа
Чтобы проверить работоспособность образа, выполните образ Docker в локальном контейнере. Выполните команду [​​docker run](https://docs.docker.com/engine/reference/commandline/run/), указав имя и тег образа. Обязательно укажите порт с помощью аргумента `-p`.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Пока пользовательский образ выполняется в локальном контейнере Docker, откройте в браузере адрес <http://localhost:8080> и убедитесь, что приложение-функция и контейнер работают правильно.

![Выполните локальное тестирование приложения-функции.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

При необходимости можно протестировать функцию снова, на этот раз в локальном контейнере с помощью следующего URL-адреса:

`http://localhost:8080/api/myhttptrigger?name=<yourname>`

Когда вы закончите проверку приложения-функции, остановите обработку в контейнере. Теперь этот пользовательский образ следует передать в учетную запись концентратора Docker.

## <a name="push-the-custom-image-to-docker-hub"></a>Отправка пользовательского образа в концентратор Docker

Реестр — это приложение, которое содержит образы и предоставляет образ служб и службы контейнеров. Чтобы сделать образ общедоступным, необходимо передать его в реестр. Docker Hub — это реестр образов Docker, который позволяет размещать ваши собственные репозитории, как общедоступные, так и частные.

Чтобы отправить образ, вам нужно выполнить вход в концентратор Docker с помощью команды [docker login](https://docs.docker.com/engine/reference/commandline/login/). Вместо `<docker-id>` укажите имя вашей учетной записи, а когда в консоли появится запрос, введите пароль. Другие возможности ввода пароля для концентратора Docker описаны в [документации по команде docker login](https://docs.docker.com/engine/reference/commandline/login/).

```bash
docker login --username <docker-id>
```

При успешном выполнении входа появится сообщение "Вход выполнен". После успешного входа примените команду [docker push](https://docs.docker.com/engine/reference/commandline/push/), чтобы передать образ в концентратор Docker.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Убедитесь, что передача завершилось успешно, проверив выходные данные команды.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from <docker-id>/mydockerimage
e7796c35add2: Mounted from <docker-id>/mydockerimage
ae9a05b85848: Mounted from <docker-id>/mydockerimage
45c86e20670d: Mounted from <docker-id>/mydockerimage
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 1796
```

Теперь вы можете использовать этот образ для развертывания приложения-функции в Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-premium-plan"></a>Создание плана "Премиум"

Размещение в Linux пользовательских контейнеров поддерживается в [выделенных планах (Службы приложений)](functions-scale.md#app-service-plan) и [планах "Премиум"](functions-premium-plan.md#features). В этом руководстве описано, как использовать план "Премиум", который можно масштабировать по мере необходимости. Дополнительные сведения о размещении см. в статье [Сравнение планов размещения для решения "Функции Azure"](functions-scale.md).

В следующем примере создается план "Премиум" с именем `myPremiumPlan` в ценовой категории **Эластичный премиум 1** (`--sku EP1`) в регионе "Западная часть США" (`-location WestUS`) в контейнере Linux (`--is-linux`).

```azurecli-interactive
az functionapp plan create --resource-group myResourceGroup --name myPremiumPlan \
--location WestUS --number-of-workers 1 --sku EP1 --is-linux
```

## <a name="create-and-deploy-the-custom-image"></a>Создание и развертывание пользовательского образа

Приложение-функция управляет выполнением функций в плане размещения. Чтобы создать приложение-функцию из образа, размещенного в концентраторе Docker, используйте команду [az functionapp create](/cli/azure/functionapp#az-functionapp-create).

В следующей команде замените `<app_name>` уникальным именем вашего приложения функции, а `<storage_name>` — именем учетной записи хранения. `<app_name>` используется по умолчанию в качестве домена DNS для приложения-функции. Поэтому это имя должно быть уникальным для всех приложений в Azure. Как и раньше, `<docker-id>` обозначает имя учетной записи Docker.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myPremiumPlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0
```

Параметр _deployment-container-image-name_ определяет образ, размещенный в концентраторе Docker, из которого нужно создать приложение-функцию. Используйте команду[az functionapp config container show](/cli/azure/functionapp/config/container#az-functionapp-config-container-show), чтобы просмотреть сведения об образе, используемом для развертывания. Используйте команду[az functionapp config container set](/cli/azure/functionapp/config/container#az-functionapp-config-container-set),чтобы развернуть из другого образа.

## <a name="configure-the-function-app"></a>Настройка приложения-функции

Функции нужно предоставить строку подключения для подключения к учетной записи хранения по умолчанию. Если вы публикуете пользовательский образ в учетной записи частного контейнера, задайте эти параметры приложения через переменные среды в файле Dockerfile, используя [инструкции ENV](https://docs.docker.com/engine/reference/builder/#env) или что-либо подобное.

В нашем случае `<storage_name>` — это имя созданной учетной записи хранения. Получите строку подключения, выполнив команду [az storage account show-connection-string](/cli/azure/storage/account). Добавьте эти параметры приложения в приложение-функцию с помощью команды [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set).

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_name> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

> [!NOTE]
> Если контейнер является закрытым, необходимо задать также следующие параметры приложения:  
> - DOCKER_REGISTRY_SERVER_USERNAME  
> - DOCKER_REGISTRY_SERVER_PASSWORD  
>
> Для получения этих значений необходимо остановить и запустить приложение-функцию.

Теперь вы можете проверить, как выполняются ваши функции на платформе Linux в Azure.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

## <a name="enable-continuous-deployment"></a>Включение непрерывного развертывания

Одним из преимуществ использования контейнеров является возможность автоматического развертывания обновлений при обновлении контейнеров в реестре. Включите непрерывное развертывание с помощью команды [az functionapp deployment container config](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-config).

```azurecli-interactive
az functionapp deployment container config --enable-cd \
--query CI_CD_URL --output tsv \
--name <app_name> --resource-group myResourceGroup
```

Эта команда возвращает URL-адрес веб-перехватчика после включения непрерывного развертывания. Чтобы вернуть этот URL-адрес, вы также можете использовать команду [az functionapp deployment container show-cd-url](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-show-cd-url). 

Скопируйте URL-адрес развертывания и перейдите в репозиторий DockerHub. Выберите вкладку **Веб-перехватчики**, введите **Имя веб-перехватчика**, вставьте свой URL-адрес в **URL-адрес веб-перехватчика**, а затем выберите знак плюс ( **+** ).

![Добавление веб-перехватчика в репозиторий DockerHub](media/functions-create-function-linux-custom-image/dockerhub-set-continuous-webhook.png)  

При установленном веб-перехватчике любые обновления связанного образа в DockerHub приводят к тому, что приложение-функция скачивает и устанавливает последний образ.

## <a name="enable-application-insights"></a>Включение Application Insights

Рекомендуемый способ наблюдения за выполнением этой функции — интеграция приложения-функции в Azure Application Insights. При создании приложения-функции на портале Azure эта интеграция выполняется по умолчанию. Тем не менее при создании приложения-функции с помощью Azure CLI его интеграции в Azure не происходит.

Чтобы настроить Application Insights для приложения-функции, сделайте следующее:

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Дополнительные сведения см. в статье [Мониторинг Функций Azure](functions-monitoring.md).

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * Создание приложения-функции и Dockerfile с помощью Core Tools.
> * Сборка пользовательского образа с помощью Docker.
> * Публикация пользовательского образа в реестре контейнеров.
> * Создание учетной записи хранения Azure.
> * Создание плана "Премиум" для Linux.
> * Развертывание приложения-функции из концентратора Docker.
> * Добавление параметров приложения в приложение-функцию.
> * Включение непрерывного развертывания.
> * Включение мониторинга Application Insights.

> [!div class="nextstepaction"] 
> См. подробнее о [параметрах развертывания функций в Azure](functions-deployment-technologies.md).
