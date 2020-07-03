---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 675ad278cb8bdc0ced4eff3bd77572f44c9808fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2020
ms.locfileid: "68857483"
---
В этом разделе описывается обновление кода в существующем проекте серверной части мобильных приложений, которое позволит отправлять push-уведомления при каждом добавлении нового элемента. Этот процесс реализуется с помощью [шаблонов](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) Центров уведомлений, включая отправку push-уведомлений между разными платформами. Разные клиенты регистрируются для обмена push-уведомлениями с помощью шаблонов; одно такое универсальное push-уведомление можно получать на всех клиентских платформах.

Выберите одну из процедур ниже, которая соответствует типу вашего серверного проекта: [серверный проект .NET](#dotnet) или [серверный проект Node.js](#nodejs).

### <a name="net-back-end-project"></a><a name="dotnet"></a>Серверный проект .NET

1. В Visual Studio щелкните правой кнопкой мыши серверный проект. Затем выберите **Управление пакетами NuGet**. Найдите `Microsoft.Azure.NotificationHubs`, а затем нажмите кнопку **Установить**. Этот процесс устанавливает библиотеку Центров уведомлений для отправки уведомлений из серверной части.
2. В проекте сервера открытые **контроллеры** > **TodoItemController.cs**. Затем добавьте следующие операторы using:

    ```csharp
    using System.Collections.Generic;
    using Microsoft.Azure.NotificationHubs;
    using Microsoft.Azure.Mobile.Server.Config;
    ```

3. В метод **PostTodoItem** добавьте следующий код после вызова **InsertAsync**.  

    ```csharp
    // Get the settings for the server project.
    HttpConfiguration config = this.Configuration;
    MobileAppSettingsDictionary settings =
        this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

    // Get the Notification Hubs credentials for the mobile app.
    string notificationHubName = settings.NotificationHubName;
    string notificationHubConnection = settings
        .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

    // Create a new Notification Hub client.
    NotificationHubClient hub = NotificationHubClient
    .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

    // Send the message so that all template registrations that contain "messageParam"
    // receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
    Dictionary<string,string> templateParams = new Dictionary<string,string>();
    templateParams["messageParam"] = item.Text + " was added to the list.";

    try
    {
        // Send the push notification and log the results.
        var result = await hub.SendTemplateNotificationAsync(templateParams);

        // Write the success result to the logs.
        config.Services.GetTraceWriter().Info(result.State.ToString());
    }
    catch (System.Exception ex)
    {
        // Write the failure result to the logs.
        config.Services.GetTraceWriter()
            .Error(ex.Message, null, "Push.SendAsync Error");
    }
    ```

    При вставке нового элемента отправляется шаблонное уведомление, содержащее item.Text.

4. Повторная публикация серверного проекта

### <a name="nodejs-back-end-project"></a><a name="nodejs"></a>Серверный проект Node.js

1. Настройка бэкэнд-проекта.
2. Замените существующий код в файле todoitem.js следующим кодом:

    ```javascript
    var azureMobileApps = require('azure-mobile-apps'),
    promises = require('azure-mobile-apps/src/utilities/promises'),
    logger = require('azure-mobile-apps/src/logger');

    var table = azureMobileApps.table();

    table.insert(function (context) {
    // For more information about the Notification Hubs JavaScript SDK,
    // see https://aka.ms/nodejshubs.
    logger.info('Running TodoItem.insert');

    // Define the template payload.
    var payload = '{"messageParam": "' + context.item.text + '" }';  

    // Execute the insert. The insert returns the results as a promise.
    // Do the push as a post-execute action within the promise flow.
    return context.execute()
        .then(function (results) {
            // Only do the push if configured.
            if (context.push) {
                // Send a template notification.
                context.push.send(null, payload, function (error) {
                    if (error) {
                        logger.error('Error while sending push notification: ', error);
                    } else {
                        logger.info('Push notification sent successfully!');
                    }
                });
            }
            // Don't forget to return the results from the context.execute().
            return results;
        })
        .catch(function (error) {
            logger.error('Error while running context.execute: ', error);
        });
    });

    module.exports = table;  
    ```

    При вставке нового элемента отправляется шаблонное уведомление, содержащее item.Text.

3. При редактировании этого файла на локальном компьютере повторно опубликуйте серверный проект.
