---
title: Разработка с помощью Java в Kubernetes с использованием Azure Dev Spaces
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
ms.author: zarhoads
ms.date: 03/22/2019
ms.topic: quickstart
description: Быстрая разработка в Kubernetes с использованием контейнеров, микрослужб и Java в Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Java, Helm, service mesh, service mesh routing, kubectl, k8s
manager: jeconnoc
ms.openlocfilehash: 26efa17ee699aed87ecfbbd21e7880e7538de4ea
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979130"
---
# <a name="quickstart-develop-with-java-on-kubernetes-using-azure-dev-spaces"></a>Краткое руководство. Разработка с помощью Java в Kubernetes с использованием Azure Dev Spaces

Из этого руководства вы узнаете, как выполнить следующие задачи:

- Настройка Azure Dev Spaces с помощью управляемого кластера Kubernetes в Azure.
- Итеративная разработка кода в контейнерах с помощью Visual Studio Code и командной строки.
- Отладка кода в среде разработки с помощью Visual Studio Code.


## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure. Если ее нет, можно создать [бесплатную учетную запись](https://azure.microsoft.com/free).
- [Средство Visual Studio Code](https://code.visualstudio.com/download).
- Расширения [Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) и [отладчик Java для Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds) для Visual Studio Code установлены.
- [Установленный Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Инструмент Maven установлен и настроен](https://maven.apache.org).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Создание кластера Службы Azure Kubernetes

Вам нужно создать кластер AKS в [поддерживаемом регионе](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams). Следующие команды создают группу ресурсов *MyResourceGroup* и кластер AKS *MyAKS*.

```cmd
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --disable-rbac --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Включение Azure Dev Spaces в кластере AKS

С помощью команды `use-dev-spaces` включите Dev Spaces в кластере AKS и следуйте инструкциям на экране. Следующая команда включает Dev Spaces в кластере *MyAKS* в группе *MyResourceGroup* и создает пространство разработки *по умолчанию*.

```cmd
$ az aks use-dev-spaces -g MyResourceGroup -n MyAKS

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

Клонируйте приложение из GitHub и перейдите в каталог *dev-spaces/samples/java/getting-started/webfrontend*:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/java/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>Подготовка приложения

Создайте ресурсы диаграмм Docker и Helm для выполнения приложения в Kubernetes с помощью команды `azds prep`:

```cmd
azds prep --public
```

Вы должны выполнить команду `prep` в каталоге *dev-spaces/samples/java/getting-started/webfrontend*, чтобы правильно создать ресурсы диаграмм Docker и Helm.

## <a name="build-and-run-code-in-kubernetes"></a>Сборка и запуск кода в Kubernetes

Создайте код в AKS с помощью команды `azds up` и выполните его:

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...3s
Installing Helm chart...8s
Waiting for container image build...28s
Building container image...
Step 1/8 : FROM maven:3.5-jdk-8-slim
Step 2/8 : EXPOSE 8080
Step 3/8 : WORKDIR /usr/src/app
Step 4/8 : COPY pom.xml ./
Step 5/8 : RUN /usr/local/bin/mvn-entrypoint.sh     mvn package -Dmaven.test.skip=true -Dcheckstyle.skip=true -Dmaven.javadoc.skip=true --fail-never
Step 6/8 : COPY . .
Step 7/8 : RUN mvn package -Dmaven.test.skip=true -Dcheckstyle.skip=true -Dmaven.javadoc.skip=true
Step 8/8 : ENTRYPOINT ["java","-jar","target/webfrontend-0.1.0.jar"]
Built container image in 37s
Waiting for container...57s
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available at http://localhost:54256
...
```

Вы можете увидеть выполнение службы, открыв общедоступный URL-адрес, который отображается в выходных данных команды `azds up`. В этом примере общедоступный URL-адрес — *http://webfrontend.1234567890abcdef1234.eus.azds.io/* .

Если остановить команду `azds up` с помощью сочетания клавиш *CTRL+C*, служба продолжит выполнятся в AKS, а общедоступный URL-адрес останется доступным.

## <a name="update-code"></a>Обновление кода

Чтобы развернуть обновленную версию службы, обновите любой файл в проекте и повторно выполните команду `azds up`. Например: 

1. Если `azds up` по-прежнему выполняется, нажмите клавиши *CTRL+C*.
1. Измените [строку 19 в `src/main/java/com/ms/sample/webfrontend/Application.java`](https://github.com/Azure/dev-spaces/blob/master/samples/java/getting-started/webfrontend/src/main/java/com/ms/sample/webfrontend/Application.java#L19):
    
    ```java
    return "Hello from webfrontend in Azure!";
    ```

1. Сохраните изменения.
1. Повторно выполните команду `azds up`:

    ```cmd
    $ azds up
    Using dev space 'default' with target 'MyAKS'
    Synchronizing files...1s
    Installing Helm chart...3s
    Waiting for container image build...
    ...    
    ```

1. Перейдите к запущенной службе и просмотрите внесенные изменения.
1. Чтобы остановить команду `azds up`, нажмите клавиши *CTRL+C*.

## <a name="enable-visual-studio-code-to-debug-in-kubernetes"></a>Включение Visual Studio Code для отладки в Kubernetes

Откройте Visual Studio Code, щелкните *Файл*, *Открыть...* , а затем перейдите в каталог *dev-spaces/samples/java/getting-started/webfrontend* и щелкните *Открыть*.

Теперь у вас есть проект *webfrontend*, открытый в Visual Studio Code. Это та же служба, которая запускается с помощью команды `azds up`. Для отладки этой службы в AKS используйте Visual Studio Code, а не `azds up`. При этом нужно подготовить этот проект, чтобы использовать Visual Studio Code для взаимодействия с вашим рабочим пространством.

Чтобы открыть палитру команд в Visual Studio Code, щелкните *Представление*, а затем — *Палитра команд*. Начните вводить `Azure Dev Spaces` и щелкните `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.

![Подготовка файлов конфигурации для Azure Dev Spaces](./media/common/command-palette.png)

Когда Visual Studio Code также предложит настроить базовые образы и открытый порт, выберите значение `Azul Zulu OpenJDK for Azure (Free LTS)` для базового образа и значение `8080` для открытого порта.

![Выбор базового образа](media/get-started-java/select-base-image.png)

![Выбор открытого порта](media/get-started-java/select-exposed-port.png)

Эта команда подготавливает проект для выполнения в Azure Dev Spaces напрямую из Visual Studio Code. Она также создает каталог *.vscode* с конфигурацией отладки в корне проекта.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio"></a>Сборка и запуск кода в Kubernetes из Visual Studio

Щелкните значок *Отладка* слева, а затем *Launch Java Program (AZDS)* (Запустить программу на Java (AZDS)) сверху.

![Запуск программы на Java](media/get-started-java/debug-configuration.png)

Эта команда создает службы в Azure Dev Spaces и запускает их в режиме отладки. Окно *Терминал* внизу показывает выходные данные сборки и URL-адрес для службы, запущенной в Azure Dev Spaces. В *консоли отладки* показаны выходные данные журнала.

> [!Note]
> Если вы не видите никаких команд Azure Dev Spaces в *палитре команд*, убедитесь, что вы установили расширение [Visual Studio Code для Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds). Кроме того, убедитесь, что вы открыли каталог *dev-spaces/samples/java/getting-started/webfrontend* в Visual Studio Code.

Щелкните *Отладка*, а затем выберите *Остановить отладку*, чтобы остановить отладчик.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Настройка и использование точек останова для отладки

Запустите службу в режиме отладки с помощью *Launch Java Program (AZDS)* (Запуск программы на Java (AZDS)).

Вернитесь в представление *Explorer*, щелкнув *Представление*, а затем — *Explorer*. Откройте `src/main/java/com/ms/sample/webfrontend/Application.java` и щелкните строку 19, чтобы расположить в ней курсор. Чтобы задать точку останова, нажмите клавишу *F9* или щелкните *Отладка* и *Переключить точку останова*.

Откройте службу в браузере. Вы увидите, что сообщение не отображается. Вернитесь в Visual Studio Code. Вы увидите, что строка 19 выделена. Заданная вами точка останова приостановила выполнение службы на строке 19. Чтобы возобновить работу службы, нажмите клавишу *F5* или щелкните *Отладка*, а затем *Продолжить*. Вернитесь в браузер. Вы увидите, что сообщение теперь отображается.

Во время выполнения службы в Kubernetes с присоединенным отладчиком у вас есть полный доступ к отладочным сведениям, включая стек вызовов, локальные переменные и данные об исключениях.

Удалите точку останова, поместив курсор в строке 19 в `src/main/java/com/ms/sample/webfrontend/Application.java` и нажав клавишу *F9*.

## <a name="update-code-from-visual-studio-code"></a>Обновление кода из Visual Studio Code

Пока служба выполняется в режиме отладки, обновите строку 19 в `src/main/java/com/ms/sample/webfrontend/Application.java`. Например: 
```java
return "Hello from webfrontend in Azure while debugging!";
```

Сохраните файл. Выберите *Отладка*, а затем — *Restart Debugging* (Перезапустить отладку) или на *панели инструментов отладки* нажмите кнопку *Restart Debugging* (Перезапустить отладку).

![Обновление отладки](media/get-started-java/debug-action-refresh.png)

Откройте службу в браузере и обратите внимание, что отображается обновленное сообщение.

Вместо того, чтобы повторно выполнять сборку и развертывание нового образа контейнера при каждой правке кода, Azure Dev Spaces пошагово перекомпилирует код в существующем контейнере для ускорения цикла редактирования и отладки.

## <a name="clean-up-your-azure-resources"></a>Очистка ресурсов Azure

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Дополнительная информация

Узнайте, как в Azure Dev Spaces можно разрабатывать более сложные приложения в нескольких контейнерах и как упростить совместную разработку, используя разные версии и ветви кода в разных средах.

> [!div class="nextstepaction"]
> [Работа с несколькими контейнерами и командной разработкой](multi-service-java.md)
