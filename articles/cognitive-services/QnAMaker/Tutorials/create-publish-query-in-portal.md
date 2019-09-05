---
title: Руководство по Создание, публикация и получение ответов в QnA Maker
titleSuffix: Azure Cognitive Services
description: Создайте базу знаний с вопросами и ответами из общедоступных часто задаваемых веб-вопросов. Сохраните, обучите и опубликуйте базу знаний. После публикации базы знаний отправьте вопрос и получите ответ с помощью команды cURL. Затем создайте бот и протестируйте его с помощью того же вопроса.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: diberry
ms.openlocfilehash: 6cc509b9f9a9087fbe832dbd35b3dfd8d60a6577
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70308067"
---
# <a name="tutorial-from-the-qna-maker-portal-create-a-knowledge-base"></a>Руководство по На портале QnA Maker создайте базу знаний.

Создайте базу знаний с вопросами и ответами из общедоступных часто задаваемых веб-вопросов. Сохраните, обучите и опубликуйте базу знаний. После публикации базы знаний отправьте вопрос и получите ответ с помощью команды cURL. Затем создайте бот и протестируйте его с помощью того же вопроса. 

Из этого руководства вы узнаете, как выполнять следующие задачи: 

> [!div class="checklist"]
> * Создание базы знаний на портале QnA Maker.
> * Просмотр, сохранение и обучение базы знаний.
> * Публикация базы знаний.
> * Использование cURL для отправки запроса в базу знаний
> * Создайте бот.
 

> [!NOTE]
> Программная версия этого руководства доступна с полным решением в [**репозитории GitHub** Azure-Samples/cognitive-services-qnamaker-csharp](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="prerequisites"></a>Предварительные требования

В этом руководстве требуется имеющаяся [служба QnA Maker](../How-To/set-up-qnamaker-service-azure.md). 

## <a name="create-a-knowledge-base"></a>Создание базы знаний 

1. Войдите на портал [QnA Maker](https://www.qnamaker.ai). 

1. Выберите **Create a knowledge base** (Создание базы знаний) в верхнем меню.

    ![Снимок экрана с порталом QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-1.png)

1. Пропустите первый шаг, так как будет использоваться существующая служба QnA Maker. 

1. Выберите доступные параметры:  

    |Параметр|Назначение|
    |--|--|
    |Microsoft Azure Directory Id (Идентификатор каталога Microsoft Azure)|Этот идентификатор каталога Microsoft Azure связан с учетной записью, используемой для входа на портал Azure и QnA Maker. |
    |Azure Subscription name (Имя подписки Azure)|Учетная запись выставления счетов, в которой создан ресурс QnA Maker.|
    |Azure QnA Service (Служба Azure QnA)|Ваш имеющийся ресурс QnA Maker.|

    ![Снимок экрана с порталом QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. Введите имя базы знаний — `My Tutorial kb`.

    ![Снимок экрана с порталом QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. Настройте для базы знаний следующие параметры:  

    |Имя параметра|Значение параметра|Назначение|
    |--|--|--|
    |URL-адрес|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |Содержимое вопросов и ответов по указанному URL-адресу находится в формате: сначала вопрос, а потом ответ. QnA Maker может интерпретировать этот формат, чтобы извлечь вопросы и связанные с ними ответы.|
    |Файл |_Не используется в этом руководстве_|Этот файл позволяет отправить файлы для вопросов и ответов. |
    |Chit-chat (Беседа)|Понятное имя|Это придает [характер](../Concepts/best-practices.md#chit-chat) непринужденности общим вопросам и ответам. Эти вопросы и ответы можно изменить позже. |

    ![Снимок экрана с порталом QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Выберите **Create your KB** (Создать базу знаний), чтобы завершить процесс создания.

    ![Снимок экрана с порталом QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-save-and-train-the-knowledge-base"></a>Просмотр, сохранение и обучение базы знаний

1. Просмотрите вопросы и ответы. На первой странице находятся вопросы и ответы, размещенные по определенному URL-адресу. 

    ![Снимок экрана с порталом QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Выберите последнюю страницу вопросов и ответов в нижней части таблицы. На странице отображаются вопросы и ответы от типа личности, применяемого в беседе. 

1. На панели инструментов над списком вопросов и ответов щелкните значок **Параметры просмотра** , а затем выберите **Показывать метаданные**. Отобразятся теги метаданных для каждого вопроса и ответа. В вопросах в беседе, возможно, уже заданы метаданные **editorial: chit-chat**. Эти метаданные возвращаются в клиентское приложение вместе с выбранным ответом. Клиентское приложение, например чат-бот, может использовать эти отфильтрованные метаданные для определения дополнительной обработки или взаимодействия с пользователем.

    ![Снимок экрана с порталом QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Выберите **Save and train** (Сохранить и обучить) в верхней строке меню.

## <a name="publish-to-get-knowledge-base-endpoints"></a>Публикация для получения конечных точек базы знаний

Нажмите кнопку **Опубликовать** в верхнем меню. На странице публикации выберите команду **Опубликовать**.

![Снимок экрана с порталом QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

После публикации базы знаний отобразится конечная точка.

![Снимок экрана с параметрами конечной точки](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

Не закрывайте эту страницу **публикации**. Она понадобится вам позже для создания бота. 

## <a name="use-curl-to-query-for-an-faq-answer"></a>Использование cURL для запроса ответа из часто задаваемых вопросов

1. Выберите вкладку **cURL**. 

    ![Снимок экрана с вкладкой cURL](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Скопируйте текст на вкладке **cURL** и выполните его в терминале или командной строке с поддержкой cURL. Значение заголовка авторизации содержит текст `Endpoint` с символом пробела в конце, а затем ключ.

1. Замените `<Your question>` на `How large can my KB be?`. Это близко к вопросу `How large a knowledge base can I create?`, но не совсем то. QnA Maker применяет обработку естественного языка, чтобы определить, совпадают ли два вопроса.     

1. Выполните команду cURL и получите ответ JSON, включающий оценку и ответ. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   581  100   543  100    38    418     29  0:00:01  0:00:01 --:--:--   447{
      "answers": [
        {
          "questions": [
            "How large a knowledge base can I create?"
          ],
          "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.",
          "score": 42.81,
          "id": 2,
          "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
          "metadata": []
        }
      ]
    }
    
    ```

    Достоверность QnA Maker составляет 42,81 %.  

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Использование cURL для запроса ответа в беседе

1. В окне терминала с поддержкой cURL замените вопрос `How large can my KB be?` оператором завершения общения с ботом со стороны пользователя, например ответом `Thank you`.   

1. Выполните команду cURL и получите ответ JSON, включающий оценку и ответ. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   525  100   501  100    24    525     25 --:--:-- --:--:-- --:--:--   550{
      "answers": [
        {
          "questions": [
            "Thank you",
            "Thanks",
            "Thnx",
            "Kthx",
            "I appreciate it",
            "Thank you so much",
            "I thank you",
            "My sincere thank"
          ],
          "answer": "You're very welcome.",
          "score": 100.0,
          "id": 109,
          "source": "qna_chitchat_the_friend.tsv",
          "metadata": [
            {
              "name": "editorial",
              "value": "chitchat"
            }
          ]
        }
      ]
    }
   
    ```

    Так как вопрос `Thank you` точно соответствует вопросу в беседе, достоверность QnA Maker составляет 100 %. QnA Maker также возвращает все связанные вопросы и свойство метаданных, содержащее сведения о теге метаданных беседы.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Использование cURL для запроса ответа по умолчанию

На любой вопрос, на который QnA Maker не может достоверно ответить, возвращается ответ по умолчанию. Этот ответ настраивается на портале Azure. 

1. В окне терминала с поддержкой cURL замените `Thank you` значением `x`. 

1. Выполните команду cURL и получите ответ JSON, включающий оценку и ответ. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   186  100   170  100    16    272     25 --:--:-- --:--:-- --:--:--   297{
      "answers": [
        {
          "questions": [],
          "answer": "No good match found in KB.",
          "score": 0.0,
          "id": -1,
          "metadata": []
        }
      ]
    }
    ```
    
    QnA Maker возвращает оценку `0`. Это означает, что достоверного ответа нет, но также возвращается ответ по умолчанию. 

## <a name="create-a-knowledge-base-bot"></a>Создание чат-бота базы знаний

Дополнительные сведения см. в статье [Tutorial: Create a QnA Bot with Azure Bot Service v4](create-qna-bot.md) (Учебник: создание чат-бота QnA с помощью службы Azure Bot версии 4).

## <a name="clean-up-resources"></a>Очистка ресурсов

Закончив работу с чат-ботом базы знаний, удалите группу ресурсов, `my-tutorial-rg`, чтобы удалить все ресурсы Azure, созданные в процессе чат-ботов.

Когда вы закончите работу с базой знаний, на портале QnA Maker выберите **My knowledge bases** (Мои базы знаний). Затем выберите базу знаний **My Tutorial kb** и щелкните значок удаления справа в этой строке.  

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о поддерживаемых форматах файлов см. [здесь](../Concepts/data-sources-supported.md). 

Дополнительные сведения о [личностях](../Concepts/best-practices.md#chit-chat), применяемых в беседе.

Дополнительные сведения об ответе по умолчанию см. [здесь](../Concepts/confidence-score.md#no-match-found). 

> [!div class="nextstepaction"]
> [Создание чат-бота с помощью этой базы знаний](create-qna-bot.md)