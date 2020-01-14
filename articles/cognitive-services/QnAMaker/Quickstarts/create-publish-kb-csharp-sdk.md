---
title: Краткое руководство. Клиентская библиотека QnA Maker для .NET
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве показано, как начать работу с клиентской библиотекой QnA Maker для .NET. Выполните приведенные здесь действия, чтобы установить пакет и протестировать пример кода для выполнения базовых задач.  QnA Maker представляет собой службу для отправки вопросов и получения ответов из частично структурированного содержимого, например документов с часто задаваемыми вопросами, URL-адресов и руководств по продуктам.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 12/16/2019
ms.author: diberry
ms.openlocfilehash: db45ff1a9114a661da90f4922003b35e1cfbe57a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75447568"
---
# <a name="quickstart-qna-maker-client-library-for-net"></a>Краткое руководство. Клиентская библиотека QnA Maker для .NET

Приступая к созданию решений с помощью клиентской библиотеки QnA Maker для .NET. Выполните приведенные здесь действия, чтобы установить пакет и протестировать пример кода для выполнения базовых задач.  QnA Maker представляет собой службу для отправки вопросов и получения ответов из частично структурированного содержимого, например документов с часто задаваемыми вопросами, URL-адресов и руководств по продуктам.

Клиентская библиотека QnA Maker для .NET:

* Создание базы знаний
* Управление базой знаний
* Публикация базы знаний
* Создание ответа из базы знаний

[Справочная документация](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet) | [Исходный код библиотеки](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Knowledge.QnAMaker) | [Пакет (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker/) | [Образцы кода (C#)](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>предварительные требования

* Подписка Azure — [создайте бесплатную учетную запись](https://azure.microsoft.com/free/).
* Текущая версия [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="setting-up"></a>Настройка

### <a name="create-a-qna-maker-azure-resource"></a>Создание ресурса QnA Maker Azure

Ресурсами Azure, на которые вы подписаны, будет представлено семейство служб Azure Cognitive Services. Создайте ресурс с помощью [Create a Cognitive Services account using the Azure portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)(создания учетной записи Cognitive Services с помощью портала Azure) или [Create a Cognitive Services account using the Azure Command-Line Interface(CLI)](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) (создания учетной записи Cognitive Services с помощью интерфейса командной строки Azure (CLI)) на локальном компьютере.

После получения ключа и конечной точки своего ресурса [создайте переменные среды](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) для ключа с именами `QNAMAKER_SUBSCRIPTION_KEY`. Имя ресурса используется как часть URL-адреса конечной точки.

### <a name="create-a-new-c-application"></a>Создание нового приложения C#

Создайте консольное приложение .NET Core на C# в предпочитаемой интегрированной среде разработки или редакторе.

В окне консоли (cmd, PowerShell или Bash) выполните команду `dotnet new`, чтобы создать консольное приложение с именем `qna-maker-quickstart`. Эта команда создает простой проект Hello World на языке C# с одним файлом исходного кода: *Program.cs*.

```dotnetcli
dotnet new console -n qna-maker-quickstart
```

Измените каталог на созданную папку приложения. Чтобы создать приложение, выполните следующую команду:

```dotnetcli
dotnet build
```

Выходные данные сборки не должны содержать предупреждений или ошибок.

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-sdk"></a>Установка пакета SDK

В каталоге приложения установите клиентскую библиотеку QnA Maker для .NET с помощью следующей команды:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker --version 1.0.0
```

Если вы используете интегрированную среду разработки Visual Studio, клиентская библиотека доступна в виде загружаемого пакета NuGet.


## <a name="object-model"></a>Объектная модель

Клиент QnA Maker — это объект [Класс QnAMakerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet), который проходит проверку подлинности в Azure с помощью объекта Microsoft.Rest.ServiceClientCredentials, который содержит ваш ключ.

После создания клиента используйте [свойство QnAMakerClient.Knowledgebase](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase) для создания и публикации своей базы знаний, а также управления ею.

Управляйте своей базой знаний, отправляя объект JSON. Для немедленных операций метод обычно возвращает объект JSON, указав статус. Ответ для длительных операций — ИД операции. Вызовите [метод OperationsExtensions.GetDetailsAsync(IOperations, String, CancellationToken)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.operationsextensions.getdetailsasync?view=azure-dotnet) с ИД операции, чтобы определить [класс OperationStateType](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-dotnet).


## <a name="code-examples"></a>Примеры кода

Эти фрагменты кода показывают, как выполнить следующие действия с помощью клиентской библиотеки QnA Maker для .NET:

* [Создание базы знаний](#create-a-knowledge-base)
* [Обновление базы знаний](#update-a-knowledge-base)
* [Скачивание базы знаний](#download-a-knowledge-base)
* [Публикация базы знаний](#publish-a-knowledge-base)
* [Удаление базы знаний](#delete-a-knowledge-base)
* [Получение состояния операции](#get-status-of-an-operation)
* [Создание ответа из базы знаний](#generate-an-answer-from-the-knowledge-base)

## <a name="add-the-dependencies"></a>Добавление зависимостей

В каталоге проекта откройте файл **Program.cs** в предпочитаемом редакторе или интегрированной среде разработки. Замените существующий код `using` следующими директивами `using`.

[!code-csharp[Using statements](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=Dependencies)]

## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Проверка подлинности клиента для создания базы знаний

В методе **main** создайте переменную для ключа Azure своего ресурса, извлеченную из переменной среды с именем `QNAMAKER_SUBSCRIPTION_KEY`. Если вы создали переменную среды после запуска приложения, то для доступа к переменной следует закрыть и перезагрузить редактор, интегрированную среду разработки или оболочку, где эта переменная была запущена. Метод будет создано позже.

Теперь создайте объект [Класс ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.apikeyserviceclientcredentials?view=azure-dotnet) с помощью ключа и используйте его со своей конечной точкой, чтобы создать объект [Класс QnAMakerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet).

|Переменная среды|переменная|Пример|
|--|--|--|
|`QNAMAKER_SUBSCRIPTION_KEY`|`subscription_key`|Ключ — это строка из 32 символов, которая доступна на странице "Быстрый запуск" ресурса QnA Maker на портале Azure. Это не то же самое, что ключ конечной точки прогнозирования.|
|`QNAMAKER_HOST`|`Endpoint`| Конечная точка разработки (в формате `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`) содержит **имя ресурса**. Это не тот же URL-адрес, который используется для запроса конечной точки прогнозирования.|
||||

```csharp
var subscriptionKey = Environment.GetEnvironmentVariable("QNAMAKER_SUBSCRIPTION_KEY");
var client = new QnAMakerClient(new ApiKeyServiceClientCredentials(subscriptionKey)) { Endpoint = "https://<your-custom-domain>.api.cognitive.microsoft.com" };
```

## <a name="authenticate-the-runtime-for-generating-an-answer"></a>Проверка подлинности среды выполнения для создания ответа

В методе **main** создайте переменную проверки подлинности своего ресурса, извлеченного из переменных среды с именами `QNAMAKER_ENDPOINT_HOSTNAME` и `QNAMAKER_ENDPOINT_KEY`. Эти значения возвращаются при публикации базы знаний. После публикации эти параметры можно найти на странице **параметров** на портале QnA Maker.

Создайте [QnAMakerRuntimeClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerruntimeclient?view=azure-dotnet), чтобы отправить в базу знаний запрос для создания ответа или выполнять обучение по методу активного обучения.

[!code-csharp[Authenticate the runtime](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=EndpointKey)]

## <a name="create-a-knowledge-base"></a>Создание базы знаний

База знаний хранит пары вопросов и ответов для объекта [Класс CreateKbDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.createkbdto?view=azure-dotnet) из трех источников:

* Для **редакционного содержимого** используйте объект [Класс QnADTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadto?view=azure-dotnet).
* Для **файлов** используйте объект [Класс FileDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.filedto?view=azure-dotnet).
* Для **URL-адресов** используйте список строк.

Вызовите метод[KnowledgebaseExtensions.CreateAsync(IKnowledgebase, CreateKbDTO, CancellationToken)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.createasync?view=azure-dotnet), а затем передайте возвращенный ИД операции методу [Get status of an operation](#get-status-of-an-operation) (Получение статуса операции) для опроса состояния.

Последняя строка следующего кода возвращает ИД базы знаний из ответа от MonitorOoperation.

[!code-csharp[Create a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=CreateKB&highlight=29,30)]

Чтобы создать базу знаний, добавьте функцию [`MonitorOperation`](#get-status-of-an-operation) из предыдущего примера кода.

## <a name="update-a-knowledge-base"></a>Обновление базы знаний

Базу знаний можно обновить, передав ИД базы знаний и объекты DTO [Класс UpdateKbOperationDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdto?view=azure-dotnet), содержащий [класс UpdateKbOperationDTOAdd](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoadd?view=azure-dotnet), [класс UpdateKbOperationDTOUpdate](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoupdate?view=azure-dotnet) и [класс UpdateKbOperationDTODelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtodelete?view=azure-dotnet) в метод [KnowledgebaseExtensions.UpdateAsync(IKnowledgebase, String, UpdateKbOperationDTO, CancellationToken)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.updateasync?view=azure-dotnet). Используйте метод [Get status of an operation](#get-status-of-an-operation) (Получение состояния операции), чтобы определить, успешно ли выполнено обновление.

[!code-csharp[Update a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=UpdateKB&highlight=4,13)]

Чтобы обновить базу знаний, добавьте функцию [`MonitorOperation`](#get-status-of-an-operation) из предыдущего примера кода.

## <a name="download-a-knowledge-base"></a>Скачивание базы знаний

Используйте метод [KnowledgebaseExtensions.DownloadAsync(IKnowledgebase, String, String, CancellationToken)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.downloadasync?view=azure-dotnet), чтобы загрузить базу данных в виде списка [Класс QnADocumentsDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadocumentsdto?view=azure-dotnet). Это _не_ эквивалентно экспорту портала QnA Maker со страницы **настроек**, поскольку результатом этого метода не является файл TSV.

[!code-csharp[Download a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DownloadKB&highlight=2)]

## <a name="publish-a-knowledge-base"></a>Публикация базы знаний

Опубликуйте базу знаний, используя метод [KnowledgebaseExtensions.PublishAsync(IKnowledgebase, String, CancellationToken)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.publishasync?view=azure-dotnet). При этом берется текущая сохраненная и обученная модель, на которую ссылается ИД базы знаний, и публикуется ее в конечной точке.

[!code-csharp[Publish a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=PublishKB&highlight=2)]

## <a name="generate-an-answer-from-the-knowledge-base"></a>Создание ответа из базы знаний

Создайте ответ из опубликованной базы знаний с использованием метода [RuntimeClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase).[GenerateAnswerAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtimeextensions.generateanswerasync?view=azure-dotnet). Этот метод принимает идентификатор базы знаний и [QueryDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto?view=azure-dotnet). Перейдите к дополнительным свойствам QueryDTO, таким как [Top](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.top?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_QueryDTO_Top) и [Context](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.context?view=azure-dotnet), для использования в вашем чат-боте.

[!code-csharp[Generate an answer from a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=GenerateAnswer&highlight=2)]


## <a name="delete-a-knowledge-base"></a>Удаление базы знаний

Удалите базу знаний, используя метод [KnowledgebaseExtensions.DeleteAsync(IKnowledgebase, String, CancellationToken)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.deleteasync?view=azure-dotnet) с параметром ИД базы знаний.

[!code-csharp[Delete a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DeleteKB&highlight=2)]

## <a name="get-status-of-an-operation"></a>Получение состояния операции

Некоторые методы, такие как "create" и "update", могут занять достаточно времени, чтобы вместо ожидания завершения процесса был возвращен [класс операции](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation?view=azure-dotnet). Используйте [свойство Operation.OperationId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation.operationid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_Operation_OperationId) из операции для опроса (с логикой повторных попыток), чтобы определить состояние исходного метода.

_loop_ и _Task.Delay_ в следующем блоке кода используются для имитации логики повторных попыток. Они должны быть заменены собственной логикой повторных попыток.

[!code-csharp[Monitor an operation](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=MonitorOperation&highlight=10)]

## <a name="run-the-application"></a>Выполнение приложения

Запустите приложение с помощью команды `dotnet run` из каталога приложения.

Все фрагменты кода из этой статьи [доступны для скачивания](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py) и могут выполняться как один файл.

```dotnetcli
dotnet run
```

[Исходный код для этого краткого руководства](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/blob/master/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs) доступен в репозитории примеров C# для QnA Maker на сайте GitHub.

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите очистить и удалить подписку Cognitive Services, вы можете удалить ресурс или группу ресурсов. При этом удаляются все ресурсы, связанные с ней.

* [Портал](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
>[Руководство. Создание базы знаний и ответ на вопрос](../tutorials/create-publish-query-in-portal.md)

* [Что такое QnA Maker?](../Overview/overview.md)
* [Редактирование базы знаний](../how-to/edit-knowledge-base.md)
* [Get analytics on your knowledge base](../how-to/get-analytics-knowledge-base.md) (Получение аналитики по своей базе знаний)
