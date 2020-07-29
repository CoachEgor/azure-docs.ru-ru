---
title: Управление доступом к удаленному мониторингу в Azure | Документация Майкрософт
description: Статья содержит сведения о настройке управления доступом на основе ролей (RBAC) в акселераторе решения для удаленного мониторинга
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.custom: has-adal-ref
ms.openlocfilehash: e41d3b47408d29a0463eed5f23117801be107c27
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85920723"
---
# <a name="configure-role-based-access-controls-in-the-remote-monitoring-solution-accelerator"></a>Настройка управления доступом на основе ролей в акселераторе решения для удаленного мониторинга

Здесь содержатся сведения о настройке элементов управления доступом на основе ролей в акселераторе решения для удаленного мониторинга. Элементы управления доступом на основе ролей позволяют ограничить доступ к определенным функциям решения для отдельных пользователей.

## <a name="default-settings"></a>Параметры по умолчанию

При первом развертывании решения для удаленного мониторинга существует две роли: **Администратор** и **Только для чтения**.

Любой пользователь с ролью **Администратор** имеет полный доступ к решению. Ему предоставляются все права, описанные ниже. Пользователь с ролью **Только для чтения** получает доступ на просмотр решения.

| Разрешение            | Административный | Только для чтения |
|----------------       |-------|-----------|
| Просмотр решения         | Да   | Да       |
| обновление оповещений;         | Да   | Нет        |
| удаление оповещений;         | Да   | Нет        |
| создание устройств;        | Да   | Нет        |
| обновление устройств;        | Да   | Нет        |
| удаление устройств;        | Да   | Нет        |
| создание групп устройств;  | Да   | Нет        |
| обновление групп устройств;  | Да   | Нет        |
| удаление групп устройств;  | Да   | Нет        |
| Создание правил          | Да   | Нет        |
| обновление правил;          | Да   | Нет        |
| удаление правил;          | Да   | Нет        |
| Создание заданий           | Да   | Нет        |
| управление обновлениями SIM. | Да   | Нет        |

Пользователю, который развертывает решение по умолчанию, назначается роль **Администратор**, и он является владельцем приложения Azure Active Directory. Как владелец приложения вы можете назначать роли другим пользователям на портале Azure. Если вы хотите, чтобы другой пользователь назначал роли в решении, необходимо указать его в качестве владельца приложения на портале Azure.

> [!NOTE]
> Пользователь, развернувший решение, является **единственным пользователем**, который может просматривать это решение сразу после создания. Чтобы предоставить другим пользователям доступ на просмотр приложения с ролью только для чтения, а также ролью администратора или пользователя, см. приведенные ниже инструкции по добавлению и удалению пользователей.

## <a name="add-or-remove-users"></a>Добавление или удаление пользователей

Как владелец приложения Azure Active Directory вы можете добавить пользователя в решение для удаленного мониторинга или же удалить его оттуда на портале Azure. В следующих шагах используется [корпоративное приложение Azure Active Directory](../active-directory/manage-apps/view-applications-portal.md), созданное при развертывании решения для удаленного мониторинга.

1. Войдите на [портал Azure](https://portal.azure.com).

1. Проверьте, [находится ли пользователь в используемом каталоге](../active-directory/fundamentals/add-users-azure-active-directory.md). Нужный каталог выбирается при входе на сайт [Акселераторы решений Интернета вещей Microsoft Azure](https://www.azureiotsolutions.com/Accelerators). Имя каталога отображается в верхнем правом углу [страницы](https://www.azureiotsolutions.com/Accelerators).

1. Найдите **корпоративное приложение** для своего решения на портале Azure. Затем отфильтруйте список, задав для параметра **Тип приложения** значение **Все приложения**. Выполните поиск приложения по его имени. В качестве имени приложения используется имя вашего решения для удаленного мониторинга. На следующем снимке экрана в качестве имен решения и приложения указано **contoso-rm4**.

    ![Корпоративное приложение](media/iot-accelerators-remote-monitoring-rbac/appregistration.png)

1. Проверьте, являетесь ли вы владельцем приложения, щелкнув приложение, а затем — **Владельцы**. На следующем снимке экрана владельцем приложения **contoso-rm4** является **администратор Contoso**:

    ![Владельцы](media/iot-accelerators-remote-monitoring-rbac/owners.png)

    Если вы не являетесь владельцем, вам необходимо попросить существующего владельца добавить вас в список. Только владельцы могут назначать другим пользователям роли приложения, такие как **Администратор** или **Только чтение**.

1. Чтобы просмотреть список пользователей, которым назначены роли в приложении, щелкните **Пользователи и группы**.

1. Чтобы добавить пользователя, щелкните **+ Добавить пользователя**, а затем — **Пользователи и группы, Не выбрано**, чтобы выбрать пользователя из каталога.

1. Чтобы назначить пользователю роль, щелкните **Выбор ролей, Не выбрано** и выберите для пользователя либо роль **Администратор**, либо **Только чтение**. Щелкните **Выбрать** и нажмите кнопку **Назначить**.

    ![Выбрать роль](media/iot-accelerators-remote-monitoring-rbac/selectrole.png)

1. Теперь пользователь может получить доступ к решению для удаленного мониторинга с разрешениями, определяемыми ролью.

1. Вы можете удалить пользователей из приложения на странице **Пользователи и группы** на портале.

## <a name="create-a-custom-role"></a>Создание настраиваемой роли

Решение для удаленного мониторинга при первом развертывании включает в себя роли **Администратор** и **Только чтение**. Вы можете добавлять настраиваемые роли с разными наборами разрешений. Чтобы определить настраиваемую роль, сделайте следующее:

- Добавьте новую роль в приложение на портале Azure.
- Определите политику для новой роли в микрослужбе проверки подлинности и авторизации.
- Обновите веб-интерфейс решения.

### <a name="define-a-custom-role-in-the-azure-portal"></a>Определение настраиваемой роли на портале Azure

В следующих шагах описано, как добавить роль в приложение в Azure Active Directory. В этом примере вы создаете новую роль, которая позволяет участникам создавать, обновлять и удалять устройства в решении для удаленного мониторинга.

1. Найдите **регистрацию приложения** для своего решения на портале Azure. В качестве имени приложения используется имя вашего решения для удаленного мониторинга. На следующем снимке экрана в качестве имен решения и приложения указано **contoso-rm4**.

    ![Регистрация приложения](media/iot-accelerators-remote-monitoring-rbac/app-registration-2.png)

1. Выберите свое приложение и щелкните **Манифест**. Отобразятся две имеющиеся [роли приложения](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles), определенные для приложения:

    ![Просмотр манифеста](media/iot-accelerators-remote-monitoring-rbac/view-manifest.png)

1. Измените манифест, чтобы добавить роль **ManageDevices**, как показано в следующем фрагменте. Для идентификатора новой роли вам понадобится уникальная строка, такая как глобальный уникальный идентификатор (GUID). Вы можете создать новый GUID с помощью службы, например [Online GUID Generator](https://www.guidgenerator.com/):

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Admin",
        "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "isEnabled": true,
        "description": "Administrator access to the application",
        "value": "Admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Read Only",
        "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "isEnabled": true,
        "description": "Read only access to device information",
        "value": "ReadOnly"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "ManageDevices",
        "id": "ADD A NEW GUID HERE",
        "isEnabled": true,
        "description": "A new role for the application.",
        "value": "ManageDevices"
      }
    ],
    ```

    Сохраните изменения.

### <a name="define-a-policy-for-the-new-role"></a>Определение политики для новой роли

После добавления роли в приложение на портале Azure вам необходимо определить политику в файле [role.json](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/auth/Services/data/policies/roles.json) для роли, которая назначает разрешения, необходимые для управления устройствами.

1. Клонируйте репозиторий [Remote Monitoring Microservices](https://github.com/Azure/remote-monitoring-services-dotnet) из GitHub на локальный компьютер.

1. Измените файл **auth/Services/data/policies/roles.json**, чтобы добавить политику для роли **ManageDevices**, как показано в следующем фрагменте. Значения **ID** и **Role** должны соответствовать определению роли в манифесте приложения из предыдущего раздела. Список разрешенных действий позволяет пользователю с ролью **ManageDevices** создавать, обновлять и удалять устройства, подключенные к решению:

    ```json
    {
    "Items": [
      {
        "Id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "Role": "admin",
        "AllowedActions": [
          "UpdateAlarms",
          "DeleteAlarms",
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices",
          "CreateDeviceGroups",
          "UpdateDeviceGroups",
          "DeleteDeviceGroups",
          "CreateRules",
          "UpdateRules",
          "DeleteRules",
          "CreateJobs",
          "UpdateSimManagement"
        ]
      },
      {
        "Id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "Role": "readOnly",
        "AllowedActions": []
      },
      {
        "Id": "GUID FROM APP MANIFEST",
        "Role": "ManageDevices",
        "AllowedActions": [
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices"
        ]
      }
    ]
    }
    ```

1. По завершении изменения файла **Services/data/policies/role.json** перестройте и снова разверните микрослужбу проверки подлинности и авторизации в акселератор решений.

### <a name="how-the-web-ui-enforces-permissions"></a>Применение разрешений веб-интерфейсом

Веб-интерфейс использует [микрослужбу проверки подлинности и авторизации](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth), чтобы определить, какие действия пользователь может выполнять и какие элементы управления отображаются в пользовательском интерфейсе. Например, если ваше решение называется **contoso-rm4**, веб-интерфейс извлекает список разрешенных действий для текущего пользователя, отправляя следующий запрос:

```http
http://contoso-rm4.azurewebsites.net/v1/users/current
headers:
X-Source: true
Authorization: Bearer <JWT Token from ADAL>
```

Для пользователя с именем **Device Manager** в роли **ManageDevices** ответ включает в себя следующий JSON:

```json
{
  "Id": "userIdExample",
  "Email": "devicemanager@contoso.com",
  "Name": "Device Manager",
  "AllowedActions": [
    "CreateDevices",
    "UpdateDevices",
    "DeleteDevices"
  ]
}
```

В следующем фрагменте из [deviceDelete.js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/pages/devices/flyouts/deviceDelete/deviceDelete.js) в [веб-интерфейсе](https://github.com/Azure/pcs-remote-monitoring-webui/) показано декларативное применение разрешений:

```json
<FlyoutContent>
  <Protected permission={permissions.deleteDevices}>
    <form className="device-delete-container" onSubmit={this.deleteDevices}>
      ...
    </form>
  </Protected>
</FlyoutContent>
```

Дополнительные сведения см. в статье [Protected Components](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/shared/protected/README.md) (Защищенные компоненты). Вы можете определить дополнительные разрешения в файле [authModel.js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/services/models/authModels.js).

### <a name="how-the-microservices-enforce-permissions"></a>Применение разрешений микрослужбами

Микрослужбы также проверяют разрешения для защиты от несанкционированных запросов API. Когда микрослужба получает запрос API, она декодирует и проверяет токен JWT, чтобы получить идентификатор пользователя и разрешения, связанные с ролью пользователя.

В следующем фрагменте кода из файла [DevicesController.cs](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/iothub-manager/WebService/v1/Controllers/DevicesController.cs) в [микрослужбе IoTHub Manager](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager) показано применение разрешений:

```csharp
[HttpDelete("{id}")]
[Authorize("DeleteDevices")]
public async Task DeleteAsync(string id)
{
    await this.devices.DeleteAsync(id);
}
```

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы узнали, как элементы управления доступом на основе ролей реализуются в акселераторе решения для удаленного мониторинга.

Сведения об управлении доступом к обозревателю аналитики временных рядов в акселераторе решения для удаленного мониторинга см. в разделе [Настройка элементов управления доступом для обозревателя службы "Аналитика временных рядов"](iot-accelerators-remote-monitoring-rbac-tsi.md).

Более подробные сведения об акселераторе решения для удаленного мониторинга см. в статье [Архитектура предварительно настроенного решения для удаленного мониторинга](iot-accelerators-remote-monitoring-sample-walkthrough.md).

Дополнительные сведения о настройке решения для удаленного мониторинга см. в статье [Настройка и повторное развертывание микрослужбы](iot-accelerators-microservices-example.md).
<!-- Next tutorials in the sequence -->