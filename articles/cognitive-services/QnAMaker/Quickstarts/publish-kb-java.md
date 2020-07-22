---
title: Краткое руководство. Публикация базы знаний, остальное, Java-QnA Maker
description: В этом кратком руководстве по началу работы с Java и REST описывается, как опубликовать базу знаний и создать конечную точку, которую можно вызвать в приложении или чат-боте.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: how-to
ms.openlocfilehash: e8df1fcc61d6b838c08f8daca9298e73820590f5
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/03/2020
ms.locfileid: "84342703"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-java"></a>Краткое руководство. Публикация базы знаний в QnA Maker с помощью Java

В этом кратком руководстве по REST описано, как программным способом опубликовать базу знаний. В процессе публикации последняя версия базы знаний передается в индекс выделенной службы "Когнитивный поиск Azure" и создается конечная точка, которую можно вызывать в приложении или чат-боте.

В этом кратком руководстве вызываются API службы QnA Maker:
* [Публикация.](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish) При использовании этого API в тексте запроса не нужно указывать какие-либо сведения.

## <a name="prerequisites"></a>Предварительные требования

* [JDK SE](https://aka.ms/azure-jdks) (комплект разработчика Java, выпуск "Стандартный");
* В этом примере используется [HTTP-клиент](https://hc.apache.org/httpcomponents-client-ga/) Apache от HTTP Components. Вам нужно добавить в проект следующие библиотеки для HTTP-клиента Apache:
    * httpclient-4.5.3.jar;
    * httpcore-4.4.6.jar;
    * commons-logging-1.2.jar.
* [Visual Studio Code](https://code.visualstudio.com/)
* У вас должна быть [служба QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Чтобы получить ключ и конечную точку (включая имя ресурса), щелкните **Быстрый запуск** для ресурса на портале Azure.
* Идентификатор базы знаний QnA Maker, который находится в URL-адресе в параметре строки запроса `kbid`, как показано ниже.

    ![Идентификатор базы знаний QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Если у вас еще нет базы знаний, создайте пример для этого краткого руководства по инструкциям из [этой статьи](create-new-kb-csharp.md).

> [!NOTE]
> Полный файл решения доступен из [репозитория GitHub **Azure-Samples/qnamaker-Services--Java** ](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/tree/master/documentation-samples/quickstarts/publish-knowledge-base).

## <a name="create-a-java-file"></a>Создание файла Java

Создайте в VSCode файл с именем `PublishKB.java`.

## <a name="add-the-required-dependencies"></a>Добавление необходимых зависимостей

В верхней части файла `PublishKB.java` над классом включите следующие строки, чтобы добавить необходимые зависимости в проект:

[!code-java[Add the required dependencies](~/samples-qnamaker-java/documentation-samples/quickstarts/publish-knowledge-base/PublishKB.java?range=1-13 "Add the required dependencies")]

## <a name="create-publishkb-class-with-main-method"></a>Создание класса PublishKB с помощью метода main

Включив необходимые зависимости, добавьте следующий класс:

```Go
public class PublishKB {

    public static void main(String[] args)
    {
    }
}
```

## <a name="add-required-constants"></a>Добавление необходимых констант

В методе **Main** добавьте необходимые константы для доступа к QnA Maker. Вместо примеров подставьте собственные значения.

[!code-java[Add the required constants](~/samples-qnamaker-java/documentation-samples/quickstarts/publish-knowledge-base/PublishKB.java?range=27-30 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Добавление запроса POST для публикации базы знаний

Включив константы, добавьте следующий код, который отправляет HTTPS-запрос к API службы QnA Maker, чтобы опубликовать базу знаний и получить ответ:

[!code-java[Add a POST request to publish knowledge base](~/samples-qnamaker-java/documentation-samples/quickstarts/publish-knowledge-base/PublishKB.java?range=32-44 "Add a POST request to publish knowledge base")]

Вызов API возвращает состояние 204 для успешной публикации без содержимого в тексте ответа. Код добавляет содержимое для ответов 204.

Этот ответ возвращается без изменений для любого другого ответа.

## <a name="build-and-run-the-program"></a>Сборка и запуск проекта

Выполните сборку и запуск программы из командной строки. Она автоматически отправит запрос к API службы QnA Maker, а полученный ответ отобразится в окне консоли.

1. Выполните сборку файла:

    ```bash
    javac -cp "lib/*" PublishKB.java
    ```

1. Запустите файл.

    ```bash
    java -cp ".;lib/*" PublishKB
    ```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Дальнейшие действия

Опубликовав базу знаний, вам нужно использовать [URL-адрес конечной точки для создания ответа](./get-answer-from-knowledge-base-java.md).

> [!div class="nextstepaction"]
> [QnA Maker (V4) REST API Reference](https://go.microsoft.com/fwlink/?linkid=2092179) (Справочник по API REST QnA Maker (V4))