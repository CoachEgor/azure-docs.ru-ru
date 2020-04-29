---
title: Создание в Azure функции, активируемой по таймеру
description: Узнайте, как создать в Azure функцию, которая выполняется по определенному расписанию.
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.topic: how-to
ms.date: 03/28/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: e100a2d3a3718b302a44cbdecf462a99d9c823e0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "80756500"
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Создание в Azure функции, активируемой по таймеру

Узнайте, как создать [независимую от сервера](https://azure.microsoft.com/solutions/serverless/) функцию, которая выполняется на основе определенного расписания с помощью Функций Azure.

![Создание приложения-функции на портале Azure](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>предварительные требования

Для работы с этим руководством:

+ Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="create-an-azure-function-app"></a>Создание приложения-функции Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Приложение-функция успешно создана.](./media/functions-create-first-azure-function/function-app-create-success.png)

Затем создайте функцию в новом приложении-функции.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Создание функции, активируемой по таймеру

1. Разверните приложение-функцию и нажмите кнопку **+** рядом с элементом **Функции**. Если это первая функция в приложении-функции, последовательно выберите **На портале** и **Продолжить**. Если нет, перейдите к шагу 3.

   ![Страница быстрого начала работы с функциями на портале Azure](./media/functions-create-scheduled-function/function-app-quickstart-choose-portal.png)

2. Последовательно выберите **Дополнительные шаблоны** и **Finish and view templates** (Закончить и просмотреть шаблоны).

    ![Быстрый запуск функций: просмотр дополнительных шаблонов](./media/functions-create-scheduled-function/add-first-function.png)

3. В поле поиска введите `timer` и настройте новый триггер с параметрами, как указано в таблице под изображением.

    ![Создайте функцию, активируемую по таймеру, на портале Azure.](./media/functions-create-scheduled-function/functions-create-timer-trigger-2.png)

    | Параметр | Рекомендуемое значение | Description |
    |---|---|---|
    | **Название** | По умолчанию | Определяет имя функции, активируемой по таймеру. |
    | **Расписание** | 0 \*/1 \* \* \* \* | [Выражение CRON](functions-bindings-timer.md#ncrontab-expressions) с шестью полями, в котором запланировано ежеминутное выполнение функции. |

4. Нажмите кнопку **Создать**. Будет создана функция на выбранном вами языке, которая будет выполняться ровно каждую минуту.

5. Проверьте выполнение, просмотрев записанные в журналах сведения трассировки.

    ![Средство просмотра журналов Функций на портале Azure.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Теперь потребуется изменить расписание функции, чтобы она выполнялась раз в час, а не раз в минуту.

## <a name="update-the-timer-schedule"></a>Обновление расписания таймера

1. Разверните вашу функцию и щелкните **Интеграция**. Здесь вы определяете входные и выходные привязки для вашей функции, а также задаете расписание. 

2. Введите в поле **Расписание** новое часовое значение `0 0 */1 * * *`, а затем щелкните **Сохранить**.  

![Обновление расписания таймера функций на портале Azure](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Теперь функция будет выполняться в начале каждого часа.

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Дальнейшие действия

Вы создали функцию, которая выполняется на основе расписания. Дополнительные сведения о триггерах см.в статье [Триггеры таймера для службы "Функции Azure"](functions-bindings-timer.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
