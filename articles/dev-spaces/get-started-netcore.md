---
title: 'Создание пространства разработки Kubernetes: Visual Studio Code и .NET Core'
services: azure-dev-spaces
ms.date: 09/26/2018
ms.topic: tutorial
description: В этом руководстве описано, как использовать Azure Dev Spaces и Visual Studio Code для отладки и быстрого выполнения итерации приложения .NET Core в службе Azure Kubernetes
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, service mesh, service mesh routing, kubectl, k8s
ms.openlocfilehash: bcdae06af8c3f6314e94ba35f748a0b4352339ca
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/25/2020
ms.locfileid: "77605362"
---
# <a name="create-a-kubernetes-dev-space-visual-studio-code-and-net-core-with-azure-dev-spaces"></a>Создание пространства разработки Kubernetes: Использование Visual Studio Code и .NET Core с Azure Dev Spaces

Из этого руководства вы узнаете, как выполнить следующие задачи:

- создание в Azure оптимизированной для разработки среды на основе Kubernetes — _пространства разработки_;
- итеративная разработка кода в контейнерах с помощью VS Code и командной строки;
- эффективная разработка и тестирование кода в среде командной работы.

> [!Note]
> **Если на каком-то этапе у вас возникли трудности**, см. раздел об [устранении неполадок](troubleshooting.md).

## <a name="install-the-azure-cli"></a>Установка Azure CLI
Для Azure Dev Spaces требуется минимальная настройка локального компьютера. Большая часть конфигурации среды разработки хранится в облаке и доступна для других пользователей. Начните со скачивания и запуска [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="sign-in-to-azure-cli"></a>Вход в Azure CLI
Войдите в Azure. В окне терминала введите следующую команду:

```cmd
az login
```

> [!Note]
> Если у вас нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free).

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Если у вас несколько подписок Azure...
Можно просматривать свои подписки, выполнив следующую команду: 

```cmd
az account list --output table
```

Найдите подписку со значением *True* для параметра *IsDefault*.
Если это не та подписка, которую нужно использовать, вы можете изменить подписку по умолчанию:

```cmd
az account set --subscription <subscription ID>
```

## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Создание и включение кластера Kubernetes для Azure Dev Spaces

В командной строке создайте группу ресурсов в [регионе, который поддерживает Azure Dev Spaces][supported-regions].

```cmd
az group create --name MyResourceGroup --location <region>
```

Чтобы создать кластер Kubernetes, выполните следующую команду:

```cmd
az aks create -g MyResourceGroup -n MyAKS --location <region> --generate-ssh-keys
```

Создание кластера занимает несколько минут.

### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Настройка кластера AKS для использования Azure Dev Spaces

В окне командной строки Azure CLI введите приведенную ниже команду, используя группу ресурсов, в которую входит кластер AKS, и имя кластера AKS. Эта команда настраивает в кластере поддержку Azure Dev Spaces.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```
   
> [!IMPORTANT]
> Процесс настройки Azure Dev Spaces удалит пространство имен `azds` в кластере, если оно существует.

## <a name="get-kubernetes-debugging-for-vs-code"></a>Получение функции отладки Kubernetes для VS Code
Для разработчиков .NET Core и Node.js, которые используют VS Code, доступны широкие возможности, такие как функция отладки Kubernetes.

1. Если у вас не установлен язык [VS Code](https://code.visualstudio.com/Download), установите его.
1. Скачайте и установите расширение [VS Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) и расширения для работы с [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). Для каждого расширения щелкните "Установить" на странице расширения в Marketplace и еще раз — в VS Code.

## <a name="create-a-web-app-running-in-a-container"></a>Создание веб-приложения для работы в контейнере

В этом разделе вы создадите веб-приложение ASP.NET Core и запустите его в контейнере в Kubernetes.

### <a name="create-an-aspnet-core-web-app"></a>Создание веб-приложения ASP.NET Core
Клонируйте или скачайте [пример приложения для Azure Dev Spaces](https://github.com/Azure/dev-spaces). При работе с этой статьей используется код, доступный в каталоге *samples/dotnetcore/getting-started/webfrontend*.

## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Подготовка кода для разработки Docker и Kubernetes
На данный момент у вас есть базовое веб-приложение, которое можно запустить локально. Теперь вы упакуете его в контейнер, создав ресурсы, определяющие контейнер приложения и способ его развертывания в Kubernetes. Эту задачу легко выполнить с помощью Azure Dev Spaces: 

1. Запустите VS Code и откройте папку `webfrontend`. (Вы можете игнорировать любые запросы по умолчанию на добавление ресурсов отладки или восстановления проекта.)
1. Откройте встроенный терминал в VS Code (выберите **Представление > Интегрированный терминал**).
1. Запустите эту команду (убедитесь, что ваша текущая папка — **webfrontend**):

    ```cmd
    azds prep --enable-ingress
    ```

Команда `azds prep` Azure CLI создает ресурсы Docker и Kubernetes с параметрами по умолчанию:
* `./Dockerfile` описывает образ контейнера приложения и способ создания и запуска исходного кода в контейнере.
* В [диаграмме Helm](https://docs.helm.sh) в разделе `./charts/webfrontend` описано, как развернуть контейнер в Kubernetes.

> [!TIP]
> Azure Dev Spaces использует [Dockerfile и диаграмму Helm](how-dev-spaces-works.md#prepare-your-code) проекта для сборки и выполнения кода. Но вы можете изменить эти файлы, если нужно определить другой способ сборки и запуска проекта.

Сейчас изучать все содержимое этих файлов не требуется. Однако стоит отметить, что **те же ресурсы Kubernetes и Docker конфигурации как кода, могут использоваться для разработки и производства, обеспечивая тем самым лучшую согласованность в разных средах.**
 
С помощью команды `prep` также создается файл `./azds.yaml`. Он является файлом конфигурации для Azure Dev Spaces. Он предоставляет артефактам Docker и Kubernetes дополнительную конфигурацию, которая обеспечивает последовательную разработку в Azure.

## <a name="build-and-run-code-in-kubernetes"></a>Сборка и запуск кода в Kubernetes
Давайте запустим код! В окне терминала запустите следующую команду из **корневой папки кода**, webfrontend:

```cmd
azds up
```

Следите за выходными данными команды, и по мере ее выполнения вы заметите следующее:
- Исходный код синхронизируется в пространство разработки в Azure.
- Образ контейнера создается в Azure, как указано в ресурсах Docker в вашей папке кода.
- Создаются объекты Kubernetes, использующие образ контейнера в соответствии с диаграммой Helm в вашей папке кода.
- Отображаются сведения о конечной точке контейнера. В нашем случае нам необходим общедоступный URL-адрес HTTP.
- Если приведенные выше этапы выполнены успешно, после запуска контейнера должны отобразиться выходные данные `stdout` (и `stderr`).

> [!Note]
> При первом запуске команды `up` на выполнение этих шагов потребуется больше времени, но последующие запуски должны выполняться быстрее.

### <a name="test-the-web-app"></a>Тестирование веб-приложения
Проверьте выходные данные консоли на наличие сообщения *Приложение запущено*, подтверждающего выполнение команды `up`:

```
Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Microsoft (R) Build Engine version 15.9.20+g88f5fadfbe for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  webfrontend -> /src/bin/Debug/netcoreapp2.2/webfrontend.dll
  webfrontend -> /src/bin/Debug/netcoreapp2.2/webfrontend.Views.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:00.94
[...]
webfrontend-5798f9dc44-99fsd: Now listening on: http://[::]:80
webfrontend-5798f9dc44-99fsd: Application started. Press Ctrl+C to shut down.
```

Определите общедоступный URL-адрес для службы в выходных данных команды `up`. Он заканчивается на `.azds.io`. В нашем примере используется такой общедоступный URL-адрес: `http://webfrontend.1234567890abcdef1234.eus.azds.io/`.

Чтобы просмотреть веб-приложение, откройте общедоступный URL-адрес в браузере. Кроме того, обратите внимание, что выходные данные `stdout` и `stderr` передаются потоком в окно терминала *azds trace* при взаимодействии с веб-приложением. Вы также увидите сведения об отслеживании HTTP-запросов при их прохождении в системе. Это упрощает отслеживание сложных вызовов к нескольким службам во время разработки. Инструментарий, добавленный в Dev Spaces, предоставляет эту возможность отслеживания запросов.

![окно терминала azds trace](media/get-started-netcore/azds-trace.png)


> [!Note]
> Кроме общедоступного URL-адреса можно использовать альтернативный URL-адрес `http://localhost:<portnumber>`, который отображается в выходных данных консоли. Если вы используете URL-адрес localhost, может показаться, что контейнер выполняется локально, но на самом деле он выполняется в AKS. В Azure Dev Spaces используется *перенаправление портов* Kubernetes для сопоставления порта localhost с контейнером, запущенным в AKS. Это облегчает работу со службой на локальном компьютере.

### <a name="update-a-content-file"></a>Обновление файла содержимого
Azure Dev Spaces — это не просто среда выполнения кода в Kubernetes. Она позволяет быстро и итеративно видеть, как изменения вашего кода вступают в силу в среде Kubernetes в облаке.

1. Найдите файл `./Views/Home/Index.cshtml` и внесите изменения в HTML. Например, измените [строку 73 `<h2>Application uses</h2>`](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/webfrontend/Views/Home/Index.cshtml#L73) строкой примерно такого содержания: 

    ```html
    <h2>Hello k8s in Azure!</h2>
    ```

1. Сохраните файл. Через несколько мгновений в окне терминала вы увидите сообщение о том, что файл в запущенном контейнере обновлен.
1. Вернитесь в браузер и обновите страницу. Должна отобразиться веб-страница с обновленным HTML.

Что произошло? Изменения файлов содержимого, таких как HTML и CSS, не требуют перекомпиляции в веб-приложении .NET Core, поэтому активная команда `azds up` автоматически синхронизирует любые измененные файлы содержимого в запущенный контейнер в Azure, чтобы можно было сразу же видеть изменения содержимого.

### <a name="update-a-code-file"></a>Обновление файла кода
Для обновления файлов кода требуется немного больше работы, так как приложение .NET Core должно перестроить и создать обновленные двоичные файлы приложений.

1. В окне терминала нажмите клавишу `Ctrl+C` (чтобы остановить `azds up`).
1. Откройте файл кода с именем `Controllers/HomeController.cs` и измените сообщение, которое будет отображаться на странице About (Сведения): `ViewData["Message"] = "Your application description page.";`
1. Сохраните файл.
1. Запустите `azds up` в окне терминала. 

Эта команда перестроит образ контейнера и повторно развернет диаграмму Helm. Чтобы увидеть, как изменения вашего кода вступили в силу в работающем приложении, перейдите в меню "Сведения" в веб-приложении.

Но есть еще один *более быстрый метод* разработки кода, который вы рассмотрите в следующем разделе. 

## <a name="debug-a-container-in-kubernetes"></a>Отладка контейнера в Kubernetes

В этом разделе используется VS Code для прямой отладки контейнера, работающего в Azure. Также вы узнаете, как быстрее вносить изменения, выполнять тестирование и запуск.

![](media/common/edit-refresh-see.png)

> [!Note]
> **Если на каком-то этапе у вас возникли трудности**, см. статью [Устранение неполадок](troubleshooting.md) или оставьте комментарий на этой странице.

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Инициализация ресурсов отладки с помощью расширения VS Code
Сначала необходимо настроить проект кода, чтобы редактор VS Code мог взаимодействовать со средой разработки в Azure. Расширение VS Code для Azure Dev Spaces содержит вспомогательную команду для настройки конфигурации отладки. 

Откройте **палитру команд** (с помощью меню **Вид | Палитра команд**), включите автоматическое завершение ввода и выберите эту команду: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. 

В папку `.vscode` будет добавлена конфигурация отладки для Azure Dev Spaces. Не следует путать эту команду с командой `azds prep`, которая позволяет настроить проект для развертывания.

![](media/common/command-palette.png)


### <a name="select-the-azds-debug-configuration"></a>Выбор конфигурации отладки AZDS
1. Чтобы открыть представление отладки, щелкните значок "Отладка" на **панели действия** сбоку VS Code.
1. Выберите **.NET Core Launch (AZDS)** (Запуск .NET Core (AZDS)) как активную конфигурацию отладки.

![](media/get-started-netcore/debug-configuration.png)

> [!Note]
> Если вы не видите никаких команд Azure Dev Spaces на палитре команд, убедитесь, что вы установили расширение VS Code для Azure Dev Spaces. Убедитесь, что рабочая область, которую вы открыли в VS Code, — это папка, содержащая файл azds.yaml.


### <a name="debug-the-container-in-kubernetes"></a>Отладка контейнера в Kubernetes
Нажмите клавишу **F5**, чтобы отладить свой код в Kubernetes.

Как и команда `up`, код синхронизируется со средой разработки, а контейнер создается и развертывается в Kubernetes. На этот раз, конечно, отладчик подключен к удаленному контейнеру.

> [!Tip]
> Строка состояния VS Code станет оранжевой. Это указывает на то, что отладчик подключен. Кроме того, появится интерактивный URL-адрес, который можно использовать для открытия сайта.

![](media/common/vscode-status-bar-url.png)

Установите точку останова в файле кода на стороне сервера, например в функции `About()` в исходном файле `Controllers/HomeController.cs`. Обновление страницы браузера инициирует срабатывание точки останова.

У вас есть полный доступ к отладочной информации, как если бы код выполнялся локально, например к стеку вызовов, локальным переменным, информации об исключениях и т. д.

### <a name="edit-code-and-refresh"></a>Изменение и обновление кода
С помощью активного отладчика отредактируйте код. Например, измените сообщение страницы About (Сведения) в `Controllers/HomeController.cs`. 

```csharp
public IActionResult About()
{
    ViewData["Message"] = "My custom message in the About page.";
    return View();
}
```

Сохраните файл и в области **действий отладки** нажмите кнопку **Restart** (Обновить). 

![](media/common/debug-action-refresh.png)

Вместо того, чтобы перестраивать и повторно развертывать новый образ контейнера при каждой правке кода, что часто занимает много времени, Azure Dev Spaces пошагово перекомпилирует код в существующем контейнере, чтобы ускорить цикл редактирования и отладки.

В браузере обновите веб-приложение и перейдите на страницу About (Сведения). Вы должны увидеть настраиваемое сообщение в пользовательском интерфейсе.

**Теперь у вас есть метод быстрой итерации кода и отладки непосредственно в Kubernetes**. Далее вы узнаете, как создать и вызвать второй контейнер.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Узнайте больше о разработке с использованием нескольких служб](multi-service-netcore.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service