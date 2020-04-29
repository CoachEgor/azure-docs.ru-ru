---
title: Создание функции, активируемой сообщениями очереди, в Azure
description: Создавайте независимые от сервера функции, активируемые сообщениями, отправленными в очередь службы хранилища Azure.
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.topic: how-to
ms.date: 10/01/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 98f0290aad9971bdb0c0b265d96e96d8ac34b99d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80756516"
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Создание функции, активируемой хранилищем очередей Azure

Узнайте, как создать функцию, которая активируется при отправке сообщений в очередь Службы хранилища Azure.

![Просмотр сообщения в журналах](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>предварительные требования

- Скачайте и установите [обозреватель хранилищ Microsoft Azure](https://storageexplorer.com/).

- Подписка Azure. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начать работу.

## <a name="create-an-azure-function-app"></a>Создание приложения-функции Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Приложение-функция успешно создана.](./media/functions-create-first-azure-function/function-app-create-success.png)

Затем создайте функцию в новом приложении-функции.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Создание функции, активируемой очередью

1. Разверните приложение-функцию и нажмите кнопку **+** рядом с элементом **Функции**. Если это первая функция в приложении-функции, последовательно выберите **На портале** и **Продолжить**. Если нет, перейдите к шагу 3.

   ![Страница быстрого начала работы с функциями на портале Azure](./media/functions-create-storage-queue-triggered-function/function-app-quickstart-choose-portal.png)

1. Последовательно выберите **Дополнительные шаблоны** и **Finish and view templates** (Закончить и просмотреть шаблоны).

    ![Быстрый запуск функций: просмотр дополнительных шаблонов](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

1. В поле поиска введите `queue` и выберите шаблон **Триггер очереди**.

1. Если появится соответствующий запрос, щелкните **Установить**, чтобы установить в приложение-функцию расширение службы хранилища Azure и другие необходимые зависимости. По завершении установки щелкните **Продолжить**.

    ![Установка расширений привязки](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)

1. Затем используйте настройки, указанные в таблице под изображением.

    ![Настройка функции, активируемой очередью хранилища](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal-2.png)

    | Параметр | Рекомендуемое значение | Description |
    |---|---|---|
    | **Название** | Уникальное для вашего приложения-функции | Имя функции, активируемой очередью. |
    | **Имя очереди**   | myqueue-items    | Имя очереди для подключения к вашей учетной записи хранения. |
    | **Подключение к учетной записи хранения** | AzureWebJobsStorage | Вы можете использовать подключение к учетной записи хранения, которое уже используется вашим приложением-функцией, или создать его.  |    

1. Щелкните **Создать**, чтобы создать функцию.

Затем необходимо подключиться к своей учетной записи хранения Azure и создать очередь службы хранилища **myqueue-items**.

## <a name="create-the-queue"></a>Создание очереди

1. Щелкните **Интегрировать** в своей функции, затем разверните узел **Документация** и скопируйте **имя учетной записи** и **ключ учетной записи**. Эти учетные данные используются для подключения к учетной записи хранения в Обозревателе службы хранилища Azure. Если вы уже подключились к учетной записи хранения, перейдите к шагу 4.

    ![Получение учетных данных для подключения к учетной записи хранения.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)

1. Запустите инструмент [Обозреватель службы хранилища Microsoft Azure](https://storageexplorer.com/), щелкните значок подключения слева, выберите **Use a storage account name and key** (Использовать имя и ключ учетной записи хранения), а затем щелкните **Далее**.

    ![Запуск инструмента "Обозреватель учетной записи хранения"](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. Введите **имя учетной записи** и **ключ учетной записи** из шага 1, щелкните **Далее**, а затем — **Подключить**.

    ![Введите учетные данные хранилища и подключитесь.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Разверните подключенную учетную запись хранения, щелкните правой кнопкой мыши **Очереди**, а затем щелкните **Создать очередь**, введите `myqueue-items` и нажмите клавишу ВВОД.

    ![Создание очереди службы хранилища](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Теперь, когда вы создали очередь хранилища, испытайте функцию, добавив сообщение в очередь.

## <a name="test-the-function"></a>Проверка функции

1. На портале Azure перейдите к вашей функции, в нижней части страницы разверните **Журналы** и убедитесь, что потоковая передача журналов не остановлена.

1. Разверните свою учетную запись в Обозреватель службы хранилища, узел **Очереди** и **myqueue-items**, а затем щелкните **Добавить сообщение**.

    ![Добавление сообщения в очередь](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. Введите сообщение Hello World в поле **Текст сообщения** и щелкните **ОК**.

1. Подождите несколько секунд, а затем вернитесь в журналы функции и убедитесь, что новое сообщение было считано из очереди.

    ![Просмотр сообщения в журналах](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. Вернитесь в обозреватель хранилища, щелкните **Обновить** и убедитесь, что сообщение было обработано и больше не находится в очереди.

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Дальнейшие действия

Вы создали функцию, которая выполняется при добавлении сообщения в очередь хранилища. Дополнительные сведения о триггерах хранилища очередей см. в статье [Привязки очередей службы хранилища для Функций Azure](functions-bindings-storage-queue.md).

Теперь, когда вы создали первую функцию, давайте добавим к ней выходную привязку, которая позволяет записывать сообщения в другую очередь.

> [!div class="nextstepaction"]
> [Добавление сообщений в очередь службы хранилища Azure с помощью Функций](functions-integrate-storage-queue-output-binding.md)
