---
title: Руководство по Запись событий устройства из пространства Интернета вещей в Azure Digital Twins | Документация Майкрософт
description: В этом руководстве вы узнаете, как получать уведомления от пространств, интегрировав Azure Digital Twins с Logic Apps.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 11/12/2019
ms.openlocfilehash: 7700c61a978532a63fc5b3298d45b8e7041dba40
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74790345"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-by-using-logic-apps"></a>Руководство по Получение уведомлений от пространств Azure Digital Twins с использованием Logic Apps

Развернув экземпляр Azure Digital Twins, подготовив пространства и реализовав пользовательские функции для отслеживания определенных условий, вы можете по электронной почте уведомлять администратора офиса, когда возникают отслеживаемые условия.

В [первом руководстве](tutorial-facilities-setup.md) вы настроили пространственный граф воображаемого здания. Комната в здании содержит датчики движения, уровня углекислого газа и температуры. Во [втором руководстве](tutorial-facilities-udf.md) вы подготовили граф и определяемую пользователем функцию для отслеживания значений датчиков и активации уведомлений, когда помещение пустое, а температура и уровень углекислого газа не выходят за пределы обычных значений. 

В этом руководстве показано, как интегрировать эти уведомления с Azure Logic Apps для отправки сообщений, когда комната свободна. Администратор офиса может использовать эти сведения, чтобы помогать сотрудникам бронировать конференц-зал с самыми оптимальными условиями для работы.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Интеграция событий со службой "Сетка событий Azure".
> * Уведомление о событиях с помощью Logic Apps.

## <a name="prerequisites"></a>Предварительные требования

В этом руководстве предполагается, что вы [настроили](tutorial-facilities-setup.md) и [подготовили](tutorial-facilities-udf.md) экземпляр Azure Digital Twins. Прежде чем продолжить, убедитесь, что у вас есть:

- [Учетная запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Работающий экземпляр Digital Twins.
- [Примеры Digital Twins на C#](https://github.com/Azure-Samples/digital-twins-samples-csharp), скачанные и извлеченные на ваш рабочий компьютер.
- [Пакет SDK для .NET Core версии 2.1.403 или более поздней](https://www.microsoft.com/net/download) на компьютере разработки. Он нужен для запуска примера. Выполните команду `dotnet --version`, чтобы проверить, установлена ли необходимая версия.
- Учетная запись [Office 365](https://products.office.com/home) для отправки уведомлений по электронной почте.

> [!TIP]
> При подготовке нового экземпляра присвойте уникальное имя экземпляру Digital Twins.

## <a name="integrate-events-with-event-grid"></a>Интеграция событий со службой "Сетка событий"

В этом разделе вы установите службу [Сетка событий](../event-grid/overview.md) для сбора событий из Azure Digital Twins и перенаправления их в [обработчик событий](../event-grid/event-handlers.md), например в Logic Apps.

### <a name="create-an-event-grid-topic"></a>Создание раздела службы "Сетка событий"

[Раздел службы "Сетка событий"](../event-grid/concepts.md#topics) предоставляет интерфейс для маршрутизации событий, созданных определяемой пользователем функцией. 

1. Войдите на [портале Azure](https://portal.azure.com).

1. В области слева выберите **Создать ресурс**. 

1. Найдите и выберите **Раздел сетки событий**. Нажмите кнопку **Создать**.

1. Введите **имя** для раздела сетки событий и выберите **подписку**. Выберите **группу ресурсов**, которую вы использовали или создали для экземпляра Digital Twins, и **расположение**. Нажмите кнопку **Создать**. 

    [![Создание раздела службы "Сетка событий"](./media/tutorial-facilities-events/create-event-grid-topic.png)](./media/tutorial-facilities-events/create-event-grid-topic.png#lightbox)

1. Перейдите в раздел сетки событий из группы ресурсов, выберите пункт **Обзор** и скопируйте значение **конечной точки раздела** во временный файл. Вам понадобится этот URL-адрес в следующих разделах. 

1. Выберите **Ключи доступа** и скопируйте **Ключ 1** и **Ключ 2** во временный файл. Вам потребуются эти значения для создания конечной точки в следующем разделе.

    [![Ключи службы "Сетка событий"](./media/tutorial-facilities-events/event-grid-keys.png)](./media/tutorial-facilities-events/event-grid-keys.png#lightbox)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Создание конечной точки для раздела службы "Сетка событий"

1. В окне командной строки убедитесь, что вы находитесь в папке **occupancy quickstart\src** примера Digital Twins.

1. Откройте файл **actions\createEndpoints.yaml** в редакторе Visual Studio Code. Убедитесь, что в нем есть следующее содержимое:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: <Primary connection string for your Event Grid>
      secondaryConnectionString: <Secondary connection string for your Event Grid>
      path: <Event Grid Topic Name without https:// and /api/events, e.g. eventgridname.region.eventgrid.azure.net>
    ```

1. Замените заполнитель `<Primary connection string for your Event Grid>` значением **Ключ 1**.

1. Замените заполнитель `<Secondary connection string for your Event Grid>` значением **Ключ 2**.

1. Замените заполнитель **путем** к разделу сетки событий. Вы можете получить этот путь, удалив **https://** и пути к ресурсам в конце из URL-адреса **конечной точки раздела**. Он должен быть похожим на следующий формат: *имя вашей сетки событий.ваше расположение.eventgrid.azure.net*.

    > [!IMPORTANT]
    > Введите все значения без кавычек. Убедитесь, что в файле YAML после двоеточия есть по крайней мере один пробел. Вы также можете проверить содержимое файла YAML с помощью любого проверяющего элемента управления YAML в Интернете, например [этого](https://onlineyamltools.com/validate-yaml).

1. Сохраните и закройте файл. В командном окне выполните следующую команду и войдите в систему при появлении запроса. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Эта команда создает конечную точку для службы "Сетка событий". 

   [![Конечные точки для службы "Сетка событий"](./media/tutorial-facilities-events/dotnet-create-endpoints.png)](./media/tutorial-facilities-events/dotnet-create-endpoints.png#lightbox)

## <a name="notify-events-with-logic-apps"></a>Уведомление о событиях с помощью Logic Apps

Можете использовать службу [Azure Logic Apps](../logic-apps/logic-apps-overview.md), чтобы создавать автоматизированные задачи для событий, полученных от других служб. В этом разделе вы настроите Logic Apps на создание электронных уведомлений о событиях, отправляемых с пространственных датчиков с помощью [раздела службы "Сетка событий"](../event-grid/overview.md).

1. В левой области [портала Azure](https://portal.azure.com) выберите **Создать ресурс**.

1. Найдите и выберите новый ресурс **Приложение логики**. Нажмите кнопку **Создать**.

1. Введите **имя** для своего ресурса приложения логики, а затем выберите **подписку**, **группу ресурсов** и **расположение**. Нажмите кнопку **Создать**.

    [![Создание ресурса Logic Apps](./media/tutorial-facilities-events/create-logic-app.png)](./media/tutorial-facilities-events/create-logic-app.png#lightbox)

1. Откройте развернутый ресурс Logic Apps, а затем откройте панель **Конструктор приложений логики**. 

1. Выберите триггер **When an Event Grid resource event occurs** (Когда происходит событие ресурса службы "Сетка событий"). Разверните параметр **Сетка событий Azure** и войдите в клиент с использованием учетной записи Azure при появлении запроса. При появлении запроса выберите **Разрешить доступ** для ресурса службы "Сетка событий". Выберите **Продолжить**.

1. В окне **When an Event Grid event occurs (Preview)** (Когда происходит событие службы "Сетка событий") сделайте следующее: 
   
   a. Выберите **подписку**, которую вы использовали ранее для создания раздела службы "Сетка событий".

   b. Выберите **Microsoft.EventGrid.Topics** в качестве **типа ресурса**.

   c. Выберите ваш ресурс службы "Сетка событий" из раскрывающегося списка в поле **Имя ресурса**.

   [![Область "Конструктор приложений логики"](./media/tutorial-facilities-events/logic-app-resource-event.png)](./media/tutorial-facilities-events/logic-app-resource-event.png#lightbox)

1. Нажмите кнопку **Новый шаг**.

1. В окне **Выберите действие** сделайте следующее:

   a. Выполните поиск фразы **анализ json** и выберите действие **Анализ JSON**.

   b. В поле **Содержимое** выберите **Текст** из списка **Динамическое содержимое**.

   c. Выберите **Использовать пример полезной нагрузки, чтобы создать схему**. Вставьте следующие полезные данные JSON и нажмите кнопку **Готово**.

    ```JSON
    {
    "id": "32162f00-a8f1-4d37-aee2-9312aabba0fd",
    "subject": "UdfCustom",
    "data": {
      "TopologyObjectId": "20efd3a8-34cb-4d96-a502-e02bffdabb14",
      "ResourceType": "Space",
      "Payload": "\"Air quality is poor.\"",
      "CorrelationId": "32162f00-a8f1-4d37-aee2-9312aabba0fd"
    },
    "eventType": "UdfCustom",
    "eventTime": "0001-01-01T00:00:00Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/a382ee71-b48e-4382-b6be-eec7540cf271/resourceGroups/HOL/providers/Microsoft.EventGrid/topics/DigitalTwinEventGrid"
    }
    ```

    В этих полезных данных есть фиктивные значения. Служба Logic Apps использует пример полезных данных для создания *схемы*.

    [![Окно "Анализ JSON" в Logic Apps для службы "Сетка событий"](./media/tutorial-facilities-events/logic-app-parse-json.png)](./media/tutorial-facilities-events/logic-app-parse-json.png#lightbox)

1. Нажмите кнопку **Новый шаг**.

1. В окне **Выберите действие** сделайте следующее:

   a. Выберите **Управления > Условие** или найдите элемент **Условие** в списке **действий**. 

   b. В первом текстовом поле **Выберите значение** выберите **eventType** из списка **Динамическое содержимое** окна **Анализ JSON**.

   c. Во втором текстовом поле **Выберите значение** введите `UdfCustom`.

   [![Выбранные условия](./media/tutorial-facilities-events/logic-app-condition.png)](./media/tutorial-facilities-events/logic-app-condition.png#lightbox)

1. Внутри окна **Если истинно** сделайте следующее:

   a. Выберите **Добавить действие** и **Office 365 Outlook**.

   b. В списке **Действия** выберите действие **Отправка электронной почты (V2)** . Выберите **Вход** и используйте данные учетной записи электронной почты. При появлении запроса выберите **Разрешить доступ**.

   c. В поле **Кому** введите свой идентификатор электронной почты для получения уведомлений. В поле **Тема** введите текст **Уведомление Digital Twins о плохом качестве воздуха в пространстве**. Затем выберите **TopologyObjectId** из списка **Динамическое содержимое** в окне **Анализ JSON**.

   d. В поле **Текст** того же окна введите примерно такой текст: **В комнате обнаружено плохое качество воздуха, и температура должна быть отрегулирована**. Вы можете прорабатывать все до мелочей, используя элементы из списка **Динамическое содержимое**.

   [![Выбор действия "Отправить электронное письмо" в Logic Apps](./media/tutorial-facilities-events/logic-app-send-email.png)](./media/tutorial-facilities-events/logic-app-send-email.png#lightbox)

1. Нажмите кнопку **Сохранить** в верхней части панели **Конструктор приложений логики**.

1. Сымитируйте данные датчиков, перейдя в папку **device-connectivity** в примере Digital Twins в окне команд и запустив `dotnet run`.

Через несколько минут вы начнете получать электронные уведомления из ресурса Logic Apps. 

   [![Уведомление по электронной почте](./media/tutorial-facilities-events/logic-app-notification.png)](./media/tutorial-facilities-events/logic-app-notification.png#lightbox)

Чтобы отключить получение этих сообщений, перейдите к ресурсу Logic Apps на портале и выберите область **Обзор**. Выберите **Отключить**.

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите прекратить изучение Azure Digital Twins на этом этапе, можете удалить ресурсы, созданные во время работы с этим руководством:

1. На [портале Azure](https://portal.azure.com) в меню слева щелкните **Все ресурсы**, выберите группу ресурсов Digital Twins, а затем щелкните **Удалить**.

    > [!TIP]
    > Если при удалении экземпляра Digital Twins у вас возникла проблема, запустите обновление службы, в котором эта проблема исправлена. Затем повторите попытку удалить свой экземпляр.

2. При необходимости удалите примеры приложений на компьютере.

## <a name="next-steps"></a>Дополнительная информация

Вы можете приступить к следующему руководству, чтобы узнать, как визуализировать данные датчиков, а также анализировать тенденции и обнаруживать аномалии.

> [!div class="nextstepaction"]
> [Руководство. по визуализации и анализу событий пространств Azure Digital Twins с помощью Аналитики временных рядов](tutorial-facilities-analyze.md)

Вы также можете получить дополнительные сведения о пространственных интеллектуальных графах и моделях объектов в Azure Digital Twins:

> [!div class="nextstepaction"]
> [Understanding Digital Twins object models and spatial intelligence graph](concepts-objectmodel-spatialgraph.md) (Основные сведения об объектных моделях и пространственном интеллектуальном графе в Digital Twins)
