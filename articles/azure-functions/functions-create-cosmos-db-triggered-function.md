---
title: Создание функции, активируемой с помощью Azure Cosmos DB
description: Используйте службу "Функции Azure" для создания бессерверной функции, которая вызывается при добавлении данных в базу данных в Azure Cosmos DB.
ms.assetid: bc497d71-75e7-47b1-babd-a060a664adca
ms.topic: how-to
ms.date: 10/02/2018
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 6045c61dc9837667bfaf01c685f687fcf5816e4c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80754212"
---
# <a name="create-a-function-triggered-by-azure-cosmos-db"></a>Создание функции, активируемой с помощью Azure Cosmos DB

Узнайте, как создать функцию, которая активируется при добавлении или изменении данных в Azure Cosmos DB. Дополнительные сведения об Azure Cosmos DB см. в статье [Azure Cosmos DB: обработка данных бессерверных баз данных с помощью службы "Функции Azure"](../cosmos-db/serverless-computing-database.md).

![Просмотр сообщения в журналах](./media/functions-create-cosmos-db-triggered-function/quickstart-completed.png)

## <a name="prerequisites"></a>предварительные требования

Для работы с этим руководством:

+ Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

> [!NOTE]
> [!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="create-an-azure-cosmos-db-account"></a>создание учетной записи Azure Cosmos DB;

Перед созданием триггера необходимо получить учетную запись Azure Cosmos DB с API SQL.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="create-an-azure-function-app"></a>Создание приложения-функции Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Затем создайте функцию в новом приложении-функции.

<a name="create-function"></a>

## <a name="create-azure-cosmos-db-trigger"></a>Создание триггера Azure Cosmos DB

1. Разверните приложение-функцию и нажмите кнопку **+** рядом с элементом **Функции**. Если это первая функция в приложении-функции, последовательно выберите **На портале** и **Продолжить**. Если нет, перейдите к шагу 3.

   ![Страница быстрого начала работы с функциями на портале Azure](./media/functions-create-cosmos-db-triggered-function/function-app-quickstart-choose-portal.png)

1. Последовательно выберите **Дополнительные шаблоны** и **Finish and view templates** (Закончить и просмотреть шаблоны).

    ![Быстрый запуск функций: просмотр дополнительных шаблонов](./media/functions-create-cosmos-db-triggered-function/add-first-function.png)

1. В поле поиска введите `cosmos` и выберите шаблон **триггера Azure Cosmos DB**.

1. Если появится соответствующий запрос, щелкните **Установить**, чтобы установить в приложение-функцию расширение Azure Cosmos DB. По завершении установки щелкните **Продолжить**.

    ![Установка расширений привязки](./media/functions-create-cosmos-db-triggered-function/functions-create-cosmos-db-trigger-portal.png)

1. Настройте новый триггер с параметрами, как указано в таблице под изображением.

    ![Создание функции, активируемой с помощью Azure Cosmos DB](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-settings.png)

    | Параметр      | Рекомендуемое значение  | Description                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Название** | По умолчанию | Используйте имя функции по умолчанию, предложенное шаблоном.|
    | **Подключение учетной записи Azure Cosmos DB** | Новый параметр | Щелкните **Создать**, выберите **подписку** и созданную ранее **учетную запись базы данных**, а затем нажмите кнопку **Выбрать**. Будет создан параметр приложения для подключения к учетной записи. Этот параметр используется в привязке для подключения к базе данных. |
    | **Имя контейнера** | Items | Имя контейнера для мониторинга. |
    | **Create lease container if it doesn't exist** (Создать контейнер аренд, если он не существует) | Флажок установлен | Контейнер не существует, создайте его. |
    | **Имя базы данных** | Задания | Имя базы данных с контейнером для отслеживания. |

1. Нажмите кнопку **Создать**, чтобы создать функцию, активируемую с помощью Azure Cosmos DB. После создания функции отображается код функции на основе шаблона.  

    ![Шаблон функции Cosmos DB на C#](./media/functions-create-cosmos-db-triggered-function/function-cosmosdb-template.png)

    Этот шаблон функции записывает в журналы количество документов и идентификатор первого документа.

Затем подключитесь к учетной записи Azure Cosmos DB и создайте контейнер `Items` в базе данных `Tasks`.

## <a name="create-the-items-container"></a>Создание контейнера элементов

1. Откройте второй экземпляр [портала Azure](https://portal.azure.com) на новой вкладке в браузере.

1. В левой части портала разверните панель значков, введите `cosmos` в поле поиска и выберите **Azure Cosmos DB**.

    ![Поиск службы Azure Cosmos DB](./media/functions-create-cosmos-db-triggered-function/functions-search-cosmos-db.png)

1. Выберите учетную запись Azure Cosmos DB, а затем выберите **обозреватель данных**. 

1. В разделе **API SQL**выберите базу данных **Задачи** и **Новый контейнер**.

    ![Создание контейнера](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-container.png)

1. На странице **Добавить контейнер** используйте параметры, перечисленные в таблице под изображением. 

    ![Определение контейнера "Задачи"](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-container2.png)

    | Параметр|Рекомендуемое значение|Description |
    | ---|---|--- |
    | **Идентификатор базы данных** | Задания |Имя новой базы данных. Должен соответствовать имени, заданному в привязке функции. |
    | **Идентификатор контейнера** | Items | Имя нового контейнера. Должен соответствовать имени, заданному в привязке функции.  |
    | **[Ключ секции](../cosmos-db/partition-data.md)** | /category|Ключ секции, который равномерно распределяет данные в каждой секции. Для создания высокопроизводительного контейнера важно выбрать правильный ключ раздела. | 
    | **Пропускная способность** |400 ЕЗ| Используйте значение по умолчанию. Чтобы сократить задержку, позже вы можете увеличить масштаб пропускной способности. |    

1. Нажмите кнопку **ОК**, чтобы создать контейнер "Элементы". Процесс создания контейнера может занять некоторое время.

После создания контейнера, указанного в привязке функции, функцию можно протестировать, добавив в новый контейнер элементы.

## <a name="test-the-function"></a>Проверка функции

1. Разверните новый контейнер **Элементы** в Data Explorer, выберите **Элементы**, а затем — **Новый элемент**.

    ![Создание элемента в контейнере "Элементы"](./media/functions-create-cosmos-db-triggered-function/create-item-in-container.png)

1. Замените содержимое нового элемента следующим содержимым, а затем выберите **Сохранить**.

        {
            "id": "task1",
            "category": "general",
            "description": "some task"
        }

1. На портале переключитесь на первую вкладку браузера, содержащую функцию. Разверните журналы функций и убедитесь, что новый документ активировал функцию. На экране будет видно, что значение идентификатора документа `task1` записано в журналы. 

    ![Просмотр сообщения в журналах](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-view-logs.png)

1. (Необязательно.) Перейдите обратно к документу, внесите изменения и нажмите кнопку **Изменить**. Затем вернитесь к журналам функций и убедитесь, что изменение также активировало функцию.

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Дальнейшие действия

Вы создали функцию, которая запускается при добавлении или изменении документов в Azure Cosmos DB. Дополнительные сведения о триггерах Azure Cosmos DB см. в статье [Привязки Azure Cosmos DB для службы "Функции Azure"](functions-bindings-cosmosdb.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
