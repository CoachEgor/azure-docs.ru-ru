---
title: 'Отладка и итерация в Kubernetes: Visual Studio Code и .NET Core'
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: quickstart
description: В этом кратком руководстве описано, как использовать Azure Dev Spaces и Visual Studio Code для отладки и быстрого выполнения итерации приложения .NET Core в службе Azure Kubernetes
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, service mesh, service mesh routing, kubectl, k8s
manager: gwallace
ms.openlocfilehash: b8ddf2e484b3d1525825f8df3b738091564b0a78
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/20/2020
ms.locfileid: "86512458"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-visual-studio-code-and-net-core---azure-dev-spaces"></a>Краткое руководство. Отладка и итерация в Kubernetes: Использование Visual Studio Code и .NET Core в Azure Dev Spaces

Из этого руководства вы узнаете, как выполнить следующие задачи:

- Настройка Azure Dev Spaces с помощью управляемого кластера Kubernetes в Azure.
- Итеративная разработка кода в контейнерах с помощью Visual Studio Code.
- Отладка кода в среде разработки с помощью Visual Studio Code.

Azure Dev Spaces также позволяет выполнять отладку и итерацию с помощью таких средств:
- [Java и Visual Studio Code](quickstart-java.md);
- [Node.js и Visual Studio Code](quickstart-nodejs.md);
- [.NET Core и Visual Studio](quickstart-netcore-visualstudio.md).

## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure. Если ее нет, можно создать [бесплатную учетную запись](https://azure.microsoft.com/free).
- [Средство Visual Studio Code](https://code.visualstudio.com/download).
- Расширения [Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) и [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) для Visual Studio Code установлены.
- [Установленный Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Создание кластера Службы Azure Kubernetes

Вам нужно создать кластер AKS в [поддерживаемом регионе][supported-regions]. Следующие команды создают группу ресурсов *MyResourceGroup* и кластер AKS *MyAKS*.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Включение Azure Dev Spaces в кластере AKS

С помощью команды `use-dev-spaces` включите Dev Spaces в кластере AKS и следуйте инструкциям на экране. Следующая команда включает Dev Spaces в кластере *MyAKS* в группе *MyResourceGroup* и создает пространство разработки *по умолчанию*.

> [!NOTE]
> Команда `use-dev-spaces` также установит интерфейс командной строки Azure Dev Spaces, если он еще не установлен. Интерфейс командной строки Azure Dev Spaces невозможно установить в Azure Cloud Shell.

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

```output
'An Azure Dev Spaces Controller' will be created that targets resource 'MyAKS' in resource group 'MyResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'MyAKS' in resource group 'MyResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

## <a name="get-sample-application-code"></a>Получение примера кода приложения

В этой статье описано, как использовать [пример приложения Azure Dev Spaces](https://github.com/Azure/dev-spaces), чтобы продемонстрировать применение Azure Dev Spaces.

Клонируйте приложение из GitHub.

```cmd
git clone https://github.com/Azure/dev-spaces
```

## <a name="prepare-the-sample-application-in-visual-studio-code"></a>Подготовка примера приложения в Visual Studio Code

Откройте Visual Studio Code, щелкните *Файл*, *Открыть...* , а затем перейдите в каталог *dev-spaces/samples/dotnetcore/getting-started/webfrontend* и щелкните *Открыть*.

Теперь проект *webfrontend* открыт в Visual Studio Code. Для запуска приложения в области разработки создайте ресурсы диаграмм Docker и Helm, используя расширение Azure Dev Spaces в палитре команд.

Чтобы открыть палитру команд в Visual Studio Code, щелкните *Представление*, а затем — *Палитра команд*. Начните вводить `Azure Dev Spaces` и щелкните `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.

![Подготовка файлов конфигурации для Azure Dev Spaces](./media/common/command-palette.png)

При появлении в Visual Studio Code запроса на настройку общедоступной конечной точки выберите `Yes`, чтобы включить общедоступную конечную точку.

![Выбор общедоступной конечной точки](media/common/select-public-endpoint.png)

Эта команда подготавливает проект для запуска в Azure Dev Spaces, создавая диаграмму Helm и Dockerfile. Она также создает каталог *.vscode* с конфигурацией отладки в корне проекта.

> [!TIP]
> Azure Dev Spaces использует [Dockerfile и диаграмму Helm](how-dev-spaces-works-prep.md#prepare-your-code) проекта для сборки и выполнения кода. Но вы можете изменить эти файлы, если нужно определить другой способ сборки и запуска проекта.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio-code"></a>Сборка и запуск кода в Kubernetes из Visual Studio Code

Щелкните значок *Отладка* слева, а затем *.NET Core Launch (AZDS)* (Запуск .NET Core (AZDS)) вверху.

![Снимок экрана: левый верхний угол окна Visual Studio Code Выделен значок отладки, заголовок панели слева — DEBUG (Отладка), а в раскрывающемся списке справа от заголовка отображается элемент ".NET Core Launch (AZDS)" (Запуск .NET Core (AZDS)).](media/get-started-netcore/debug-configuration.png)

Эта команда создает службы в Azure Dev Spaces и запускает их в режиме отладки. В окне *Терминал* внизу показаны выходные данные сборки и URL-адрес для службы, запущенной в Azure Dev Spaces. В *консоли отладки* показаны выходные данные журнала.

> [!Note]
> Если вы не видите никаких команд Azure Dev Spaces в *палитре команд*, убедитесь, что вы установили расширение [Visual Studio Code для Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds). Кроме того, убедитесь, что вы открыли каталог *dev-spaces/samples/dotnetcore/getting-started/webfrontend* в Visual Studio Code.

Вы увидите, что служба запущена, перейдя по общедоступному URL-адресу.

> [!Note]
> На начальном этапе общедоступный URL-адрес может возвращать ошибку *Bad Gateway* (Недопустимый шлюз). Подождите несколько секунд, прежде чем обновлять веб-страницу, и вы увидите, что служба запущена.

Щелкните *Отладка*, а затем выберите *Остановить отладку*, чтобы остановить отладчик.

## <a name="update-code"></a>Обновление кода

Чтобы развернуть обновленную версию службы, обновите любой файл в проекте и повторно выполните команду *.NET Core Launch (AZDS)* (Запуск .NET Core (AZDS)). Пример:

1. Если ваше приложение по-прежнему работает, нажмите кнопку *Debug* (Отладка), а затем — *Stop Debugging* (Остановить отладку), чтобы остановить ее.
1. Измените [строку 22 в `Controllers/HomeController.cs`](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/webfrontend/Controllers/HomeController.cs#L22) на такую:
    
    ```csharp
    ViewData["Message"] = "Your application description page in Azure.";
    ```

1. Сохраните изменения.
1. Повторно выполните команду *.NET Core Launch (AZDS)* (Запуск .NET Core (AZDS)).
1. Перейдите к запущенной службе и щелкните *О программе*.
1. Просмотрите внесенные изменения.
1. Нажмите кнопку *Debug* (Отладка), а затем — *Stop Debugging* (Остановить отладку), чтобы остановить приложение.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Настройка и использование точек останова для отладки

Запустите службу в режиме отладки с помощью команды *.NET Core Launch (AZDS)* (Запуск .NET Core (AZDS)).

Вернитесь в представление *Explorer*, щелкнув *Представление*, а затем — *Explorer*. Откройте `Controllers/HomeController.cs` и щелкните строку 22, чтобы расположить в ней курсор. Чтобы задать точку останова, нажмите клавишу *F9* или щелкните *Отладка* и *Переключить точку останова*.

Откройте службу в браузере. Вы увидите, что сообщение не отображается. Вернитесь в Visual Studio Code и обратите внимание, что строка 20 выделена. Заданная вами точка останова приостановила выполнение службы на строке 20. Чтобы возобновить работу службы, нажмите клавишу *F5* или щелкните *Отладка*, а затем *Продолжить*. Вернитесь в браузер. Вы увидите, что сообщение теперь отображается.

Во время выполнения службы в Kubernetes с присоединенным отладчиком у вас есть полный доступ к отладочным сведениям, включая стек вызовов, локальные переменные и данные об исключениях.

Удалите точку останова, поместив курсор в строке 22 в `Controllers/HomeController.cs` и нажав клавишу *F9*.

## <a name="update-code-from-visual-studio-code"></a>Обновление кода из Visual Studio Code

Пока служба выполняется в режиме отладки, обновите строку 22 в `Controllers/HomeController.cs`. Пример:

```csharp
ViewData["Message"] = "Your application description page in Azure while debugging!";
```

Сохраните файл. Выберите *Отладка*, а затем — *Restart Debugging* (Перезапустить отладку) или на *панели инструментов отладки* нажмите кнопку *Restart Debugging* (Перезапустить отладку).

![Панель инструментов "Отладка"представляет собой небольшую панель, которая отображается по центру вверху страницы (сразу под заголовком страницы). Кнопка "Перезапуск" выделена и представляет собой круглую стрелку. При наведении указателя на кнопку отображается надпись "Перезапустить (CTRL+SHIFT+F5)".](media/common/debug-action-refresh.png)

Откройте службу в браузере и обратите внимание, что отображается обновленное сообщение.

Вместо того, чтобы повторно выполнять сборку и развертывание нового образа контейнера при каждой правке кода, Azure Dev Spaces пошагово перекомпилирует код в существующем контейнере для ускорения цикла редактирования и отладки.

## <a name="clean-up-your-azure-resources"></a>Очистка ресурсов Azure

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как в Azure Dev Spaces можно разрабатывать более сложные приложения в нескольких контейнерах и как упростить совместную разработку, используя разные версии и ветви кода в разных средах. 

> [!div class="nextstepaction"]
> [Работа с несколькими контейнерами и командной разработкой](multi-service-netcore.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
