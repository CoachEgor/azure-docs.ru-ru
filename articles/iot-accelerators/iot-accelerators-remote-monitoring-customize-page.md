---
title: Добавление страницы в пользовательский интерфейс решения для удаленного мониторинга в Azure | Документация Майкрософт
description: В этой статье показано, как добавить новую страницу в веб-интерфейс акселератора решения для удаленного мониторинга.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: c90f4166bf88a8df18a93e84903c93461b904d2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "82187268"
---
# <a name="add-a-custom-page-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Добавление пользовательской страницы в веб-интерфейс акселератора решения для удаленного мониторинга

В этой статье показано, как добавить новую страницу в веб-интерфейс акселератора решения для удаленного мониторинга. Содержание статьи:

- Подготовка локальной среды разработки.
- Добавление новой страницы в веб-интерфейс.

Другие руководства расширяют этот сценарий и позволяют добавить дополнительные функции к новой странице.

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить действия, описанные в этом руководстве, необходимо установить следующее ПО на локальный компьютер разработки:

- [Git](https://git-scm.com/downloads);
- [Node.js](https://nodejs.org/download/)

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Подготовка локальной среды разработки для пользовательского интерфейса

Код пользовательского интерфейса акселератора решения для удаленного мониторинга реализуется с помощью платформы JavaScript [React](https://reactjs.org/). Исходный код можно найти в репозитории GitHub [веб-интерфейса для удаленного мониторинга](https://github.com/Azure/pcs-remote-monitoring-webui).

Чтобы внести и протестировать изменения в пользовательском интерфейсе, можно запустить их на локальном компьютере разработки. При необходимости локальную копию можно подключить к развернутому экземпляру акселератора решений, чтобы он мог взаимодействовать с реальными или имитированными устройствами.

Чтобы подготовить локальную среду разработки, клонируйте на локальный компьютер репозиторий [веб-интерфейса для удаленного мониторинга](https://github.com/Azure/pcs-remote-monitoring-webui) с помощью Git:

```cmd/sh
git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
```

## <a name="add-a-page"></a>Добавление страницы

Чтобы добавить страницу в веб-интерфейс, необходимо добавить исходные файлы, которые определяют эту страницу. Необходимо также изменить некоторые имеющиеся файлы, чтобы веб-интерфейс учитывал новую страницу.

### <a name="add-the-new-files-that-define-the-page"></a>Добавление новых файлов, которые определяют страницу

Чтобы приступить к работе, перейдите к папке **src/walkthrough/components/pages/basicPage**. В ней содержатся четыре файла, которые определяют простую страницу:

**basicPage.container.js**

[!code-javascript[Page container source](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.container.js?name=container "Page container source")]

**basicPage.js**

[!code-javascript[Basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.js?name=page "Basic page")]

**basicPage.scss**

[!code-javascript[Page styling](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.scss?name=styles "Page styling")]

**basicPage.test.js**

[!code-javascript[Test code for basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.test.js?name=test "Test code for basic page")]

Создайте папку **src/components/pages/example** и скопируйте в нее эти четыре файла.

### <a name="add-the-new-page-to-the-web-ui"></a>Добавление новой страницы в веб-интерфейс

Чтобы добавить новую страницу в веб-интерфейс, внесите в имеющиеся файлы описанные ниже изменения.

1. Добавьте новый контейнер страниц в файл **src/components/pages/index.js**:

    ```js
    export * from './example/basicPage.container';
    ```

1. (Необязательно.) Добавьте значок SVG для новой страницы. Дополнительные сведения см. в репозитории [webui/src/utilities/README.md](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/utilities/README.md). Вы можете использовать имеющийся SVG-файл.

1. Добавьте название страницы **public/locales/en/translations.json** в файл переводов. Веб-интерфейс использует [i18next](https://www.i18next.com/) для интернационализации.

    ```json
    "tabs": {
      "basicPage": "Example",
    },
    ```

1. Откройте файл **src/components/app.js**, который определяет страницу приложения верхнего уровня. Добавьте новую страницу в список операций импорта:

    ```javascript
    // Page Components
    import  {
      //...
      BasicPageContainer
    } from './pages';
    ```

1. В этом же файле добавьте новую страницу в массив `pagesConfig`. Задайте адрес `to` для маршрута, ссылку на значок SVG и добавленные ранее переводы, а также задайте `component` для контейнера страницы:

    ```js
    const pagesConfig = [
      //...
      {
        to: '/basicpage',
        exact: true,
        svg: svgs.tabs.example,
        labelId: 'tabs.basicPage',
        component: BasicPageContainer
      },
      //...
    ];
    ```

1. Добавьте в массив `crumbsConfig` новые элементы навигации:

    ```js
    const crumbsConfig = [
      //...
      {
        path: '/basicpage', crumbs: [
          { to: '/basicpage', labelId: 'tabs.basicPage' }
        ]
      },
      //...
    ];
    ```

    В этом примере страницы есть только один элемент навигации, но на других страницах может быть больше таких элементов.

Сохраните изменения. Веб-интерфейс с новой страницей готов к запуску.

### <a name="test-the-new-page"></a>Тестирование новой страницы

В командной строке перейдите к корню локальной копии репозитория и выполните следующие команды, чтобы установить необходимые библиотеки и локально запустить веб-интерфейс:

```cmd/sh
npm install
npm start
```

Предыдущая команда запускает пользовательский интерфейс локально по адресу `http://localhost:3000/dashboard`.

На панели мониторинга появятся сообщения об ошибках, если локальный экземпляр веб-интерфейса не будет подключен к развернутому экземпляру акселератора решений. Эти ошибки не влияют на возможность тестирования новой страницы.

Теперь можно изменить код во время локальной работы сайта. Веб-интерфейс будет обновляться динамически.

## <a name="optional-connect-to-deployed-instance"></a>[Необязательно] Подключение к развернутому экземпляру

При необходимости в облаке можно подключить локальную копию веб-интерфейса к акселератору решения для удаленного мониторинга:

1. Разверните **базовый** экземпляр акселератора решений с помощью CLI **PCS**. Запишите имя развертывания и учетные данные, указанные для виртуальной машины. Дополнительные сведения см. в статье [Развертывание предварительно настроенного решения для удаленного мониторинга с помощью интерфейса командной строки](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Используйте портал Azure или [AZ CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) , чтобы включить доступ по ПРОТОКОЛу SSH к виртуальной машине, на которой размещены микрослужбы в решении. Пример:

    ```azurecli
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    Включать доступ по протоколу SSH нужно только во время тестирования и разработки. Если вы включили протокол SSH, [то следует отключить его снова как можно скорее](../security/fundamentals/network-best-practices.md).

1. Используя портал Azure или [командную строку Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), найдите имя и общедоступный IP-адрес виртуальной машины. Пример:

    ```azurecli
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. Подключитесь к виртуальной машине по протоколу SSH, используя IP-адрес из предыдущего шага и учетные данные, указанные при запуске **PCS** для развертывания решения.

1. Чтобы разрешить локальному пользовательскому интерфейсу установить подключение, выполните следующие команды в оболочке Bash на виртуальной машине.

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. После завершения команды и запуска веб-сайта можно отключиться от виртуальной машины.

1. В локальной копии репозитория [веб-интерфейса для удаленного мониторинга](https://github.com/Azure/pcs-remote-monitoring-webui) измените **ENV-файл**, добавив URL-адрес развернутого решения.

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

## <a name="next-steps"></a>Дальнейшие действия

Из статьи вы узнали о ресурсах, которые могут помочь вам при настройке пользовательского веб-интерфейса в акселераторе решения для удаленного мониторинга.

Вы определили страницу. Теперь необходимо [добавить пользовательскую службу в веб-интерфейс акселератора решения для удаленного мониторинга](iot-accelerators-remote-monitoring-customize-service.md), которая извлекает данные для отображения в пользовательском интерфейсе.

Дополнительные сведения об ускорителе решений для удаленного мониторинга см. в разделе [архитектура удаленного мониторинга](iot-accelerators-remote-monitoring-sample-walkthrough.md).
