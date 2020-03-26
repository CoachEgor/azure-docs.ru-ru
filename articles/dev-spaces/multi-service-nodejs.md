---
title: 'Выполнение нескольких зависимых служб: Visual Studio Code и Node.js'
services: azure-dev-spaces
ms.date: 11/21/2018
ms.topic: tutorial
description: В этом руководстве описано, как использовать Azure Dev Spaces и Visual Studio Code для отладки приложения Node.js с несколькими службами в службе Azure Kubernetes.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, service mesh, service mesh routing, kubectl, k8s
ms.openlocfilehash: a5fa0aae3a966dd96ee95e6bcafc3b2eec4e6837
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "75438295"
---
# <a name="running-multiple-dependent-services-nodejs-and-visual-studio-code-with-azure-dev-spaces"></a>Выполнение нескольких зависимых служб: Использование Node.js и Visual Studio Code в Azure Dev Spaces

Из этого руководства вы узнаете, как разрабатывать приложения на базе нескольких служб с помощью Azure Dev Spaces. Также здесь описываются некоторые дополнительные преимущества использования Dev Spaces.

## <a name="call-a-service-running-in-a-separate-container"></a>Вызов службы, запущенной в отдельном контейнере

В этом разделе вы создадите вторую службу, `mywebapi`, к которой будет обращаться `webfrontend`. Каждая служба будет выполняться в отдельных контейнерах. Затем вы выполните отладку в обоих контейнерах.

![](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>Открытие примера кода для *mywebapi*.
У вас уже должен быть пример кода для `mywebapi` в рамках этого руководства. Он находится в папке `samples` (в противном случае перейдите по адресу https://github.com/Azure/dev-spaces и выберите **Clone or Download** (Клонировать или загрузить), чтобы загрузить репозиторий GitHub). Код для этого раздела находится в папке `samples/nodejs/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Запуск *mywebapi*
1. Откройте папку `mywebapi` в *отдельном окне VS Code*.
1. Откройте **палитру команд** (с помощью меню **Вид | Палитра команд**), включите автоматическое завершение ввода и выберите эту команду: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Не следует путать эту команду с командой `azds prep`, которая позволяет настроить проект для развертывания.
1. Нажмите клавишу F5 и подождите, пока выполнится сборка и развертывание службы. Вы узнаете, что все готово, когда в консоли отладки появится сообщение об *ожидании передачи данных через порт 80*.
1. Запишите URL-адрес конечной точки. Он будет иметь приблизительно такой вид: `http://localhost:<portnumber>`. **Совет. Строка состояния VS Code станет оранжевой и в ней появится интерактивный URL-адрес.** Может показаться, что контейнер выполняется локально, но фактически он выполняется в вашей среде разработки в Azure. В адресе указано localhost, потому что для `mywebapi` не определены общедоступные конечные точки и доступ осуществляется только в пределах экземпляра Kubernetes. Для вашего удобства и упрощения взаимодействия с закрытой службой на локальном компьютере служба Azure Dev Spaces создает временный туннель SSH для контейнера, запущенного в Azure.
1. Когда `mywebapi` будет готово, откройте в браузере адрес localhost. Вы увидите ответ от службы `mywebapi` ("Hello from mywebapi").


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Запрос из *webfrontend* к *mywebapi*
Давайте теперь напишем код в `webfrontend`, который отправляет запрос к `mywebapi`.
1. Перейдите в окно VS Code для `webfrontend`.
1. Добавьте следующие строки кода в верхней части `server.js`:
    ```javascript
    var request = require('request');
    ```

3. *Замените* код для обработчика GET `/api`. При обработке запроса он, в свою очередь, обращается к `mywebapi`, а затем возвращает результаты из обеих служб.

    ```javascript
    app.get('/api', function (req, res) {
       request({
          uri: 'http://mywebapi',
          headers: {
             /* propagate the dev space routing header */
             'azds-route-as': req.headers['azds-route-as']
          }
       }, function (error, response, body) {
           res.send('Hello from webfrontend and ' + body);
       });
    });
    ```
   1. *Удалите* строку `server.close()` в конце `server.js`

В предыдущем примере код перенаправляет заголовок `azds-route-as` из входящего запроса в исходящий. Позже вы увидите, как это помогает повысить производительность при коллективной разработке.

### <a name="debug-across-multiple-services"></a>Отладка в нескольких службах
1. На этом этапе служба `mywebapi` по-прежнему должна выполняться с подключенным отладчиком. Если это не так, нажмите клавишу F5 в проекте `mywebapi`.
1. Установите точку останова в обработчике GET по умолчанию `/`[в строке 8 `server.js`](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/mywebapi/server.js#L8).
1. В проекте `webfrontend` установите точку останова до отправки запроса GET к `http://mywebapi`.
1. Нажмите клавишу F5 в проекте `webfrontend`.
1. Откройте веб-приложение и выполните пошаговое прохождение кода в обеих службах. В веб-приложении должно отобразиться объединенное сообщение от двух служб: "Hello from webfrontend and Hello from mywebapi".

### <a name="well-done"></a>Все готово!
Теперь у вас есть многоконтейнерное приложение, где каждый контейнер можно разрабатывать и развертывать отдельно.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Коллективная разработка с помощью Dev Spaces](team-development-nodejs.md)
