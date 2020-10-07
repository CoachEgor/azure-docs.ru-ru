---
title: Краткое руководство. QnA Maker с помощью интерфейсов API для Node.js
description: В этом кратком руководстве показано, как начать работу с REST API QnA Maker для Node.js. Выполните приведенные здесь действия, чтобы установить пакет и протестировать пример кода для выполнения базовых задач.  QnA Maker представляет собой службу для отправки вопросов и получения ответов из частично структурированного содержимого, например документов с часто задаваемыми вопросами, URL-адресов и руководств по продуктам.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27, devx-track-js
ms.topic: how-to
ms.openlocfilehash: ef17f08677d715292a8ee49621156d130e2f5a6b
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2020
ms.locfileid: "91777498"
---
# <a name="quickstart-qna-maker-rest-apis-for-nodejs"></a>Краткое руководство. QnA Maker интерфейсы API для Node.js

Начните работу с QnA Maker с REST API для Node.js. Выполните следующие действия, чтобы опробовать пример кода для базовых задач.  QnA Maker представляет собой службу для отправки вопросов и получения ответов из частично структурированного содержимого, например документов с часто задаваемыми вопросами, URL-адресов и руководств по продуктам.

Используйте QnA Maker с REST API для Node.js для:

* Создание базы знаний
* Замена базы знаний
* Публикация базы знаний
* Удаление базы знаний
* Скачивание базы знаний
* Получение состояния операции

[Справочная документация](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)  |  [ПримерыNode.js](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/rest-api)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure — [создайте бесплатную учетную запись](https://azure.microsoft.com/free/cognitive-services).
* Текущая версия [Node.js](https://nodejs.org).
* У вас должна быть [служба QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Чтобы получить ключ и конечную точку (включая имя ресурса), щелкните **Быстрый запуск** для ресурса на портале Azure.

## <a name="setting-up"></a>Настройка

### <a name="create-a-qna-maker-azure-resource"></a>Создание ресурса QnA Maker Azure

Ресурсами Azure, на которые вы подписаны, будет представлено семейство служб Azure Cognitive Services. Создайте ресурс с помощью [Create a Cognitive Services account using the Azure portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)(создания учетной записи Cognitive Services с помощью портала Azure) или [Create a Cognitive Services account using the Azure Command-Line Interface(CLI)](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) (создания учетной записи Cognitive Services с помощью интерфейса командной строки Azure (CLI)) на локальном компьютере.

После получения ключа от своего ресурса [создайте переменные среды](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) для ресурса с именами `QNAMAKER_RESOURCE_KEY` и `QNAMAKER_AUTHORING_ENDPOINT`. Используйте значения ключа и конечной точки, найденные на странице **быстрого запуска** ресурса на портале Azure.

### <a name="create-a-new-nodejs-application"></a>создание приложения Node.js;

В окне консоли (например, cmd, PowerShell или Bash) создайте новый каталог для приложения и перейдите в него.

```console
mkdir myapp && cd myapp
```

Выполните команду `npm init -y`, чтобы создать файл узла `package.json`.

```console
npm init -y
```

Добавьте NPM пакеты `reqeuestretry` и `request`:

```console
npm install requestretry request --save
```

## <a name="code-examples"></a>Примеры кода

Эти фрагменты кода показывают, как выполнить следующие действия с помощью QnA Maker с REST API для Node.js:

* [Создание базы знаний](#create-a-knowledge-base)
* [Замена базы знаний](#replace-a-knowledge-base)
* [Публикация базы знаний](#publish-a-knowledge-base)
* [Удаление базы знаний](#delete-a-knowledge-base)
* [Скачивание базы знаний](#download-the-knowledge-base)
* [Получение состояния операции](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Добавление зависимостей

Создайте файл с именем `rest-apis.js` и добавьте следующие зависимости.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="dependencies":::

## <a name="add-utility-functions"></a>Добавление служебных функций

Добавьте следующие служебные функции.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="utility":::

## <a name="add-azure-resource-information"></a>Добавление сведений о ресурсах Azure

Создайте переменные для конечной точки Azure и ключа ресурса. Если вы создали переменную среды после запуска приложения, для доступа к переменной следует закрыть и повторно открыть редактор, интегрированную среду разработки или оболочку, где эта переменная была запущена.

Задайте следующие значения среды:

* `QNAMAKER_RESOURCE_KEY` — **Ключ** — это строка из 32 символов, которая доступна в портал Azure на QnA Maker ресурсе на странице **быстрого запуска** . Это не то же самое, что ключ конечной точки прогнозирования.
* `QNAMAKER_AUTHORING_ENDPOINT`. Конечная точка разработки (в формате `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`) содержит **имя ресурса**. Это не тот же URL-адрес, который используется для запроса конечной точки прогнозирования.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="authorization":::

## <a name="create-a-knowledge-base"></a>Создание базы знаний

В базе знаний хранятся пары вопросов и ответов, созданные из объекта JSON:

* **Редакционный контент**.
* **Файлы** — локальные файлы, не требующие разрешения.
* **URL** -адреса — общедоступные URL.

Используйте [REST API для создания базы знаний](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="createKb":::

## <a name="replace-a-knowledge-base"></a>Замена базы знаний

Используйте [REST API для замены базы знаний](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/replace).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="replaceKb":::

## <a name="publish-a-knowledge-base"></a>Публикация базы знаний

Публикация базы знаний. Этот процесс делает базу знаний доступной из конечной точки прогнозирования HTTP-запросов.

Используйте [REST API для публикации базы знаний](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="publishKb":::

## <a name="download-the-knowledge-base"></a>Загрузка базы знаний

Используйте [REST API для загрузки базы знаний](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/download).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="downloadKb":::

## <a name="delete-a-knowledge-base"></a>Удаление базы знаний

После завершения использования базы знаний удалите ее.

Используйте [REST API для удаления базы знаний](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/delete).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="deleteKb":::

## <a name="get-status-of-an-operation"></a>Получение состояния операции

Длительные процессы, такие как процесс создания, возвращают идентификатор операции, который необходимо проверить с помощью отдельного вызова REST API. Эта функция принимает текст ответа "Создать". Важным ключом является `operationState`, который определяет, нужно ли продолжать опрос.

Используйте [REST API для отслеживания операций в базе знаний](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails).

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="operationDetails":::

## <a name="add-main-method"></a>Добавить метод Main

Добавьте приведенный ниже метод `main`.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/QnAMaker/rest/create-publish-kb.js" id="main":::

## <a name="run-the-application"></a>Выполнение приложения

Запустите приложение с командой `node rest-apis.js` из каталога приложения.

```console
node rest-apis.js
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите очистить и удалить подписку Cognitive Services, вы можете удалить ресурс или группу ресурсов. При этом удаляются все ресурсы, связанные с ней.

* [Портал](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
>[Учебник. Создание и ответ на базу знаний](../tutorials/create-publish-query-in-portal.md)

* [Что такое QnA Maker?](../Overview/overview.md)
* [Редактирование базы знаний](../how-to/edit-knowledge-base.md)
* [Get analytics on your knowledge base](../how-to/get-analytics-knowledge-base.md) (Получение аналитики по своей базе знаний)
* Исходный код для этого шаблона можно найти на портале [GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/blob/master/documentation-samples/quickstarts/rest-api/rest-api.js).
