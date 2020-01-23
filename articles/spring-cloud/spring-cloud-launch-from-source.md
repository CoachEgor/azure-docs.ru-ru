---
title: Краткое руководство. Запуск приложения Spring Cloud из исходного кода
description: В этом кратком руководстве вы узнаете, как запустить приложение Azure Spring Cloud непосредственно из исходного кода
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/30/2019
ms.author: brendm
ms.openlocfilehash: 8edadbf946a28239ae4d28d56b3dccd77b451a71
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2020
ms.locfileid: "76277159"
---
# <a name="quickstart-launch-your-spring-cloud-application-from-source-code"></a>Краткое руководство. Запуск приложения Spring Cloud из исходного кода

Azure Spring Cloud позволяет легко выполнять приложения микрослужб Spring Cloud в Azure.

Azure Spring Cloud позволяет запускать приложение непосредственно из исходного кода Java или из предварительно созданного JAR-файла. В этой статье описаны шаги, которые необходимо для этого выполнить.

Из этого руководства вы узнаете, как выполнить следующие задачи:

> [!div class="checklist"]
> * подготовка экземпляра службы к работе;
> * задание сервера конфигурации для экземпляра;
> * локальное выполнение сборки приложения для микрослужб;
> * развертывание каждой микрослужбы;
> * назначение общедоступной конечной точки для приложения.

## <a name="prerequisites"></a>Предварительные требования

>[!Note]
> Сейчас предоставляется общедоступная предварительная версия Azure Spring Cloud. Предложения общедоступной предварительной версии позволяют клиентам экспериментировать с новыми функциями до официального выпуска.  Общедоступные предварительные версии функций и служб не предназначены для использования в рабочей среде.  Чтобы получить дополнительные сведения о поддержке на этапе использования предварительных версий, ознакомьтесь с разделом [Вопросы и ответы](https://azure.microsoft.com/support/faq/) или оформите [запрос на поддержку](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

Прежде чем начать, убедитесь, что подписка Azure имеет необходимые зависимости:

1. [установите Git](https://git-scm.com/);
2. [установите JDK версии 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html);
3. [установите Maven 3.0 или более поздней версии](https://maven.apache.org/download.cgi);
4. [Установка Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Регистрация для получения подписки Azure](https://azure.microsoft.com/free/)

> [!TIP]
> Azure Cloud Shell — это бесплатная интерактивная оболочка, с помощью которой можно выполнять действия, описанные в этой статье.  Она содержит предварительно установленные общие инструменты Azure вместе с новейшими версиями Git, JDK, Maven и Azure CLI. Если вы вошли в подписку Azure, запустите [Azure Cloud Shell](https://shell.azure.com) на сайте shell.azure.com.  Дополнительные сведения об Azure Cloud Shell см. в [нашей документации](../cloud-shell/overview.md)

## <a name="install-the-azure-cli-extension"></a>Установка расширения Azure CLI

Установите расширение Azure Spring Cloud для Azure CLI с помощью следующей команды:

```Azure CLI
az extension add --name spring-cloud
```

## <a name="provision-a-service-instance-using-the-azure-cli"></a>Подготовка экземпляра службы к работе с помощью Azure CLI

Войдите в Azure CLI и выберите активную подписку. Выбранная активная подписка должна быть добавлена в список разрешений для Azure Spring Cloud.

```Azure CLI
az login
az account list -o table
az account set --subscription
```

Откройте окно Azure CLI и выполните приведенные ниже команды, чтобы подготавливать экземпляр Azure Spring Cloud к работе. Обратите внимание, что на этом этапе Azure Spring Cloud требуется назначить общедоступный домен.

```azurecli
    az spring-cloud create -n <resource name> -g <resource group name> --is-public true
```

Развертывание экземпляра службы займет около пяти минут.

Задайте стандартные имена группы ресурсов и кластера с помощью следующих команд:

```azurecli
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```

> [!div class="nextstepaction"]
> [У меня есть проблема](https://www.research.net/r/javae2e?tutorial=asc-source-quickstart&step=provision)

## <a name="create-the-spring-cloud-application"></a>Создание приложения Spring Cloud

Следующая команда создает в подписке приложение Spring Cloud.  При этом создается пустая служба Spring Cloud, в которую можно передать приложение.

```azurecli
az spring-cloud app create -n <app-name>
```

## <a name="deploy-your-spring-cloud-application"></a>Развертывание приложения Spring Cloud

Приложение можно развернуть из предварительно созданного JAR-файла либо из репозитория Gradle или Maven.  Инструкции для каждого варианта приведены ниже.

### <a name="deploy-a-built-jar"></a>Развертывание из созданного JAR-файла

Чтобы выполнить развертывание из JAR-файла, созданного на локальном компьютере, при сборке должен создаваться [расширенный JAR-файл](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-build.html#howto-create-an-executable-jar-with-maven).

Развертывание расширенного JAR-файла в активном развертывании

```azurecli
az spring-cloud app deploy -n <app-name> --jar-path <path-to-fat-JAR>
```

Развертывание расширенного JAR-файла в конкретном развертывании

```azurecli
az spring-cloud app deployment create --app <app-name> -n <deployment-name> --jar-path <path-to-built-jar>
```

### <a name="deploy-from-source-code"></a>Развертывание из исходного кода

Для сборки проекта Azure Spring Cloud использует [kpack](https://github.com/pivotal/kpack).  Вы можете использовать Azure CLI для передачи исходного кода, выполнить сборку проекта с помощью kpack и развернуть его в целевом приложении.

> [!WARNING]
> Проект должен создать только один JAR-файл с записью `main-class` в `MANIFEST.MF` в `target` (для развертываний Maven) или `build/libs` (для развертываний Gradle).  Несколько JAR-файлов с записями `main-class` приведут к сбою развертывания.

Для проектов с одним модулем Maven или Gradle:

```azurecli
cd <path-to-maven-or-gradle-source-root>
az spring-cloud app deploy -n <app-name>
```

Для проектов Maven или Gradle с несколькими модулями повторите эти действия для каждого модуля:

```azurecli
cd <path-to-maven-or-gradle-source-root>
az spring-cloud app deploy -n <app-name> --target-module <relative-path-to-module>
```

### <a name="show-deployment-logs"></a>Отображение журналов развертывания

Проверьте журналы сборки kpack с помощью следующей команды:

```azurecli
az spring-cloud app show-deploy-log -n <app-name> [-d <deployment-name>]
```

> [!NOTE]
> В журналах kpack будет отображаться только последнее развертывание, если оно создано из исходного кода с помощью kpack.

> [!div class="nextstepaction"]
> [У меня есть проблема](https://www.research.net/r/javae2e?tutorial=asc-source-quickstart&step=deploy)

## <a name="assign-a-public-endpoint-to-gateway"></a>Назначение общедоступной конечной точки шлюзу

1. Откройте страницу **Панель мониторинга приложения**.
2. Выберите приложение `gateway`, чтобы отобразить страницу **Сведения о приложении**.
3. Выберите **Назначить домен**, чтобы назначить общедоступную конечную точку шлюзу. Для этого может потребоваться несколько минут. 
4. Введите назначенный общедоступный IP-адрес в браузере, чтобы просмотреть работающее приложение.

> [!div class="nextstepaction"]
> [У меня есть проблема](https://www.research.net/r/javae2e?tutorial=asc-source-quickstart&step=public-endpoint)

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнить следующие действия:

> [!div class="checklist"]
> * подготовка экземпляра службы к работе;
> * задание сервера конфигурации для экземпляра;
> * локальное выполнение сборки приложения для микрослужб;
> * развертывание каждой микрослужбы;
> * Измерение переменных среды для приложений
> * Назначение общедоступного IP-адреса для шлюза приложения

> [!div class="nextstepaction"]
> [Подготовка приложения Azure Spring Cloud к развертыванию](spring-cloud-tutorial-prepare-app-deployment.md)

Дополнительные примеры доступны на GitHub: [Примеры для Azure Spring Cloud](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
