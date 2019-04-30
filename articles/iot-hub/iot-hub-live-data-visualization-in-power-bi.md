---
title: Визуализация данных, поступающих от датчиков в реальном времени, из Центра Интернета вещей с помощью Power BI | Документация Майкрософт
description: Сведения о визуализации данных о температуре и влажности, собранных с датчиков и переданных в Центр Интернета вещей Azure, с помощью Power BI.
author: robinsh
keywords: визуализация данных, полученных в реальном времени, визуализация интерактивных данных, визуализация данных датчиков
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: robinsh
ms.openlocfilehash: 7c770aced36e4c90f654de8d31c12d55ad80c8d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60780127"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Визуализация данных, поступающих от датчиков в реальном времени, из Центра Интернета вещей с помощью Power BI

![Комплексная схема](./media/iot-hub-live-data-visualization-in-power-bi/1_end-to-end-diagram.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Что вы узнаете

Вы узнаете, как с помощью Power BI визуализировать данные, которые в реальном времени поступают с датчиков в Центр Интернета вещей Azure. Сведения о том, как визуализировать данные в центре Интернета вещей с помощью веб-приложений, см. в статье [Визуализация данных, поступающих от датчиков в реальном времени, из Центра Интернета вещей с помощью веб-приложений службы приложений Azure](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Что нужно сделать

* добавим группу потребителей, чтобы обеспечить доступ к данным в Центре Интернета вещей;

* Создать, настроить и запустить задание Stream Analytics, чтобы передавать данные из центра Интернета вещей в учетную запись Power BI.

* Создать отчет Power BI и опубликовать его, чтобы визуализировать данные.

## <a name="what-you-need"></a>Необходимые элементы

* Завершить [онлайн-симулятор Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md) руководства или одним из руководств устройства; например, [Raspberry Pi с помощью node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Они охватывают следующие требования:
  
  * Активная подписка Azure.
  * Центр Интернета вещей Azure в подписке;
  * клиентское приложение, которое отправляет сообщения в Центр Интернета вещей Azure.

* Учетная запись Power BI. Доступна [бесплатная пробная версия](https://powerbi.microsoft.com/).

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Создание, настройка и выполнение заданий Stream Analytics

Сначала давайте создадим задание Stream Analytics. После этого мы определим входные и выходные данные, а также запрос на получение данных.

### <a name="create-a-stream-analytics-job"></a>Создание задания Stream Analytics

1. На [портале Azure](https://portal.azure.com) последовательно выберите **Создать ресурс** > **"Интернет вещей"** > **Stream Analytics job** (Задание Stream Analytics).

2. Введите представленные ниже сведения для задания.

   **Имя задания.** Имя задания. Оно должно быть глобально уникальным.

   **Группа ресурсов.** Выберите ту же группу ресурсов, которую использует центр Интернета вещей.

   **Расположение.** Выберите то же расположение, которое использует группа ресурсов.

   **Закрепить на панели мониторинга.** Установите этот флажок, чтобы из панели мониторинга быстро получать доступ к центру Интернета вещей.

   ![Создание задания Stream Analytics в Azure](./media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

3. Нажмите кнопку **Создать**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Добавление входных данных в задание Stream Analytics

1. Откройте задание Stream Analytics.

2. В разделе **Топология задания** щелкните **Входные данные**.

3. На панели **Входные данные** щелкните **Добавить потоковый вход**, а затем введите следующие данные.

   **Входной псевдоним.** Уникальный псевдоним для входных данных. Выберите **Ввести параметры центра IoT вручную** ниже.

   **Источник**. Выберите **Центр Интернета вещей**.
   
   **Конечная точка**. Щелкните **Обмен сообщениями**.

   **Группа потребителей.** Выберите созданную вами группу потребителей.

4. Нажмите кнопку **Создать**.

   ![Добавление входных данных в задание Stream Analytics в Azure](./media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Добавление выходных данных в задание Stream Analytics

1. В разделе **Топология задания** щелкните **Выходные данные**.

2. На панели **Выходные данные** щелкните **Добавить**, **Power BI**, а затем введите сведения, указанные ниже.

   **Выходной псевдоним.** Уникальный псевдоним для выходных данных.

   **Рабочая область группы**. Выберите рабочую область целевой группы.

   **Имя набора данных**. Введите имя набора данных.

   **Имя таблицы.** Введите имя таблицы.

3. Щелкните **Авторизовать**, а затем выполните вход в учетную запись Power BI.

4. Нажмите кнопку **Создать**.

   ![Добавление выходных данных в задание Stream Analytics в Azure](./media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Настройка запроса задания Stream Analytics

1. В разделе **Топология задания** щелкните **Запрос**.

2. Замените значение `[YourInputAlias]` значением псевдонима входных данных задания.

3. Замените значение `[YourOutputAlias]` значением псевдонима выходных данных задания.

4. Выберите команду **Сохранить**.

   ![Добавление запроса в задание Stream Analytics в Azure](./media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>Выполнение задания Stream Analytics

В задании Stream Analytics щелкните **Запуск** > **Сейчас** > **Запустить**. После успешного запуска состояние задания **Остановлено** изменится на **Выполняется**.

![Выполнение задания Stream Analytics в Azure](./media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Создание отчета Power BI и его публикация для визуализации данных

1. Убедитесь, что пример приложения запущен на устройстве. Если нет, вы можете обратиться к руководствам в статье [Подключение Raspberry Pi к Центру Интернета вещей Azure (Node.js)](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).

2. Выполните вход в учетную запись [Power BI](https://powerbi.microsoft.com/en-us/).

3. Щелкните рабочую область,которую вы использовали, **Моя рабочая область**.

4. Нажмите кнопку **Наборы данных**.

   Вы должны увидеть набор данных, указанный при создании выходных данных для задания Stream Analytics.

5. В созданном наборе данных щелкните **Добавить отчет** (первый значок справа от имени набора данных).

   ![Создание отчета Microsoft Power BI](./media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

6. Создайте график для отображения данных температуры в реальном времени за определенный период времени.

   1. Добавьте график на страницу создания отчета.

   2. В области **Поля** разверните таблицу, указанную при создании выходных данных для задания Stream Analytics.
   
   3. Перетащите элемент **EventEnqueuedUtcTime** на **ось** в области **Визуализации**.
   
   4. Перетащите элемент **температура** в раздел **Значения** той же области.

      График создан. Ось Х отображает дату и время в часовом поясе UTC. Ось Y отображает данные температуры, полученные от датчика.

      ![Добавление графика данных температуры в отчет Microsoft Power BI](./media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

7. Создайте другой график для отображения влажности в реальном времени за определенный период времени. Выполните действия, аналогичные действиям выше, и перетащите **EventEnqueuedUtcTime** на ось Х, а **влажность** на ось Y.

   ![Добавление графика данных влажности в отчет Microsoft Power BI](./media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

8. Нажмите кнопку **Сохранить**, чтобы сохранить отчет.

9. Нажмите кнопку **Отчеты** на левой панели и выберите отчет, который вы только что создали.

10. Щелкните **Файл** > **Опубликовать в Интернете**.

11. Щелкните **Создать код внедрения**, а затем выберите **Опубликовать**.

Вы получите ссылку на отчет, которой вы сможете поделиться, чтобы предоставить доступ к отчету, а также фрагмент кода для интеграции отчета на страницу блога или веб-сайта.

![Публикация отчета Microsoft Power BI](./media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Корпорация Майкрософт также предлагает [мобильные приложения Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) для просмотра информационных панелей и отчетов Power BI, а также взаимодействия с ними.

## <a name="next-steps"></a>Дальнейшие действия

Вы успешно использовали Power BI для визуализации данных, полученных от датчика в режиме реального времени, из Центра Интернета вещей Azure.

Но это не единственный способ для визуализации данных из Центра Интернета вещей Azure. Дополнительные сведения см. в статье [Визуализация данных, поступающих от датчиков в реальном времени, из Центра Интернета вещей с помощью веб-приложений Azure](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
