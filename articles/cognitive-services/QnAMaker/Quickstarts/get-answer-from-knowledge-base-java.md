---
title: Краткое руководство. Получение ответа из базы знаний с помощью Java REST в службе QnA Maker
description: В этом кратком руководстве по Java REST описывается, как получить ответ из базы знаний программными средствами.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27, devx-track-java
ms.topic: how-to
ms.openlocfilehash: ff40bcb58c1bdca39360a236e46a70ce5cf1b158
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777607"
---
# <a name="quickstart-get-answers-to-a-question-from-a-knowledge-base-with-java"></a>Краткое руководство. получение ответов на вопрос из базы знаний с помощью Java

В этом кратком руководстве объясняется, как получить ответ из базы знаний QnA Maker программными средствами. База знаний содержит вопросы и ответы из [источника данных](../Concepts/knowledge-base.md), например часто задаваемые вопросы. [Вопросы](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) отправляются в службу QnA Maker. Результат содержит прогнозируемый [ответ](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties).

[Справочная документация](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime) | [Пример](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/blob/master/documentation-samples/quickstarts/get-answer/GetAnswer.java)

## <a name="prerequisites"></a>Предварительные требования

* [JDK SE](https://aka.ms/azure-jdks) (комплект разработчика Java, выпуск "Стандартный");
* В этом примере используется [HTTP-клиент](https://hc.apache.org/httpcomponents-client-ga/) Apache от HTTP Components. Вам нужно добавить в проект следующие библиотеки для HTTP-клиента Apache:
    * httpclient-4.5.3.jar;
    * httpcore-4.4.6.jar;
    * commons-logging-1.2.jar.
* [Visual Studio Code](https://code.visualstudio.com/)
* У вас должна быть [служба QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Чтобы получить ключ, выберите **Ключи** в разделе **Управление ресурсами** на панели мониторинга ресурса QnA Maker на портале Azure.
* Параметры страницы **Публикация**. Если у вас нет опубликованной базы знаний, создайте новую, импортируйте ее на странице **Параметры**, а затем опубликуйте ее. Можно скачать и использовать [эту простую базу знаний](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv).

    На странице параметров публикации представлены значения Host, EndpointKey и маршрута POST.

    ![Параметры публикации](../media/qnamaker-quickstart-get-answer/publish-settings.png)

## <a name="create-a-java-file"></a>Создание файла Java

Откройте VSCode, создайте файл с именем `GetAnswer.java` и добавьте следующий класс:

```Java
public class GetAnswer {

    public static void main(String[] args)
    {

    }
}
```

## <a name="add-the-required-dependencies"></a>Добавление необходимых зависимостей

В этом кратком руководстве используются классы Apache для HTTP-запросов. Над классом GetAnswer в верхней части файла `GetAnswer.java` добавьте необходимые зависимости в проект.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/QueryKB.java" id="dependencies":::

## <a name="add-the-required-constants"></a>Добавление необходимых констант

В верхней части класса `GetAnswer.java` добавьте необходимые константы для доступа к QnA Maker. Эти значения отобразятся на странице **публикации** после публикации базы знаний.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/QueryKB.java" id="constants":::

## <a name="add-a-post-request-to-send-question"></a>Добавление запроса POST для отправки вопроса

Следующий код создает HTTPS-запрос к API службы QnA Maker, чтобы отправить вопрос в базу знаний и получить ответ.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/QueryKB.java" id="post":::

Значение заголовка `Authorization` содержит строку `EndpointKey`.

Дополнительные сведения о [запросе](../how-to/metadata-generateanswer-usage.md#generateanswer-request) и [ответе](../how-to/metadata-generateanswer-usage.md#generateanswer-response).

## <a name="build-and-run-the-program"></a>Сборка и запуск проекта

Выполните сборку и запуск программы из командной строки. Она автоматически отправит запрос к API службы QnA Maker, а полученный ответ отобразится в окне консоли.

1. Выполните сборку файла:

    ```bash
    javac -cp "lib/*" GetAnswer.java
    ```

1. Запустите файл.

    ```bash
    java -cp ".;lib/*" GetAnswer
    ```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [QnA Maker (V4) REST API Reference](https://go.microsoft.com/fwlink/?linkid=2092179) (Справочник по API REST QnA Maker (V4))
