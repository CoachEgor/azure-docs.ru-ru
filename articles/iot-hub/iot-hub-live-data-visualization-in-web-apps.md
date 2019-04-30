---
title: Визуализация данных, поступающих c датчиков в реальном времени, из Центра Интернета вещей с помощью веб-приложений | Документация Майкрософт
description: Сведения о визуализации данных о температуре и влажности, собранных с датчиков и переданных в Центр Интернета вещей, с помощью веб-приложений в службе приложений Microsoft Azure.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: robinsh
ms.openlocfilehash: 070f37a969411cfc4caf5f2d2b089ccfae759ca2
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125718"
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a>Визуализация данных, поступающих от датчиков в реальном времени, из Центра Интернета вещей с помощью веб-приложений службы приложений Azure

![Комплексная схема](./media/iot-hub-live-data-visualization-in-web-apps/1_iot-hub-end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Что вы узнаете

Из этого руководства вы узнаете, как визуализировать данные датчиков, полученные в реальном времени и отправленные в Центр Интернета вещей, с помощью веб-приложения, размещенного в Azure. Если вы хотите визуализировать данные, поступающие с датчиков в реальном времени, в Центре Интернета вещей с помощью Power BI, см. сведения в [этой статье](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Что нужно сделать

* создадим веб-приложение на портале Azure;
* добавим группу потребителей, чтобы обеспечить доступ к данным в Центре Интернета вещей;
* настроим веб-приложение для чтения данных датчиков из Центра Интернета вещей;
* отправим созданное веб-приложение для размещения в Azure;
* откроем веб-приложение, чтобы просмотреть данные о температуре и влажности, полученные в реальном времени, из Центра Интернета вещей.

## <a name="what-you-need"></a>Необходимые элементы

* Завершить [онлайн-симулятор Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md) руководства или одним из руководств устройства; например, [Raspberry Pi с помощью node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Они охватывают следующие требования:

  * активная подписка Azure;
  * Центр Интернета вещей в подписке;
  * клиентское приложение, которое отправляет сообщения в Центр Интернета вещей.

* [Скачайте Git](https://www.git-scm.com/downloads).

## <a name="create-a-web-app"></a>Создание веб-приложения

1. На [портале Azure](https://portal.azure.com/) последовательно выберите **Создать ресурс** > **Интернет+мобильные устройства** > **Веб-приложение**.

2. Введите уникальное имя задания, проверьте подписку, укажите группу ресурсов и расположение, а затем выберите **Закрепить на панели мониторинга** и нажмите кнопку **Создать**.

   Мы рекомендуем выбрать от же расположение, что и для группы ресурсов. 
   
   ![Создание веб-приложения](./media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a>Настройка веб-приложения для чтения данных датчиков из Центра Интернета вещей

1. Откройте только что подготовленное веб-приложение.

2. Щелкните **Параметры приложения**, а затем в разделе **Параметры приложения** добавьте следующие пары "ключ — значение":

   | Ключ                                   | Value                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Из интерфейса командной строки Azure                                      |
   | Azure.IoT.IoTHub.ConsumerGroup        | Имя группы потребителей, добавленной в Центр Интернета вещей.  |
   | WEBSITE_NODE_DEFAULT_VERSION          | 8.9.4                                                        |

   ![Добавление параметров для веб-приложения с помощью пар "ключ — значение"](./media/iot-hub-live-data-visualization-in-web-apps/3_web-app-settings-key-value-azure.png)

3. В разделе **Общие параметры** выберите **Параметры приложения**, включите параметр **Веб-сокеты** и щелкните **Сохранить**.

   ![Включение параметра "Веб-сокеты"](./media/iot-hub-live-data-visualization-in-web-apps/4_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a>Отправка созданного веб-приложения для размещения в Azure

Теперь веб-приложение доступно в GitHub. В нем отображаются данные, поступающие с датчиков в реальном времени, из Центра Интернета вещей. Далее нам нужно настроить работу веб-приложения с репозиторием Git, скачать веб-приложение из GitHub и отправить его для размещения в Azure.

1. В веб-приложении щелкните **Варианты развертывания** > **Выбор источника** > **Локальный репозиторий Git** и нажмите кнопку **ОК**.

   ![Настройка развертывания веб-приложения для использования локального репозитория Git](./media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Щелкните **Учетные данные развертывания**, укажите имя пользователя и пароль, которые будут использоваться для подключения к репозиторию Git в Azure, а затем нажмите кнопку **Сохранить**.

3. Щелкните **Обзор** и запишите значение **URL-адреса клона Git**.

   ![Получение URL-адреса клона Git веб-приложения](./media/iot-hub-live-data-visualization-in-web-apps/6_web-app-git-clone-url-azure.png)

4. Откройте окно терминала или командной строки на локальном компьютере.

5. Скачайте веб-приложение из GitHub и отправьте его для размещения в Azure. Для этого выполните следующие команды:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > Значение параметра \<URL-адрес клона Git\> — это URL-адрес репозитория Git, полученный на странице **Обзор** веб-приложения.

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>Открытие веб-приложения, чтобы просмотреть данные о температуре и влажности, полученные в реальном времени, в Центре Интернета вещей

На странице **Обзор** щелкните URL-адрес, чтобы открыть веб-приложение.

![Получение URL-адреса веб-приложения](./media/iot-hub-live-data-visualization-in-web-apps/7_web-app-url-azure.png)

Данные о температуре и влажности, полученные в реальном времени, должны отображаться в Центре Интернета вещей.

![Страница веб-приложения с данными о температуре и влажности, полученными в реальном времени](./media/iot-hub-live-data-visualization-in-web-apps/8_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> Убедитесь, что пример приложения запущен на устройстве. Если это не так, отобразится пустая диаграмма. Вы можете обратиться к руководствам в статье [Подключение Raspberry Pi к Центру Интернета вещей Azure (Node.js)](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="next-steps"></a>Дальнейшие действия

Вы успешно использовали веб-приложение для визуализации данных датчика, полученных в реальном времени, из Центра Интернета вещей.

Дополнительные способы визуализации данных из Центра Интернета вещей Azure см. в статье [Визуализация данных, поступающих от датчиков в реальном времени, из Центра Интернета вещей с помощью Power BI](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]