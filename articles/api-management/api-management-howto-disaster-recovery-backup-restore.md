---
title: Внедрение аварийного восстановления с помощью функций архивации и восстановления в службе управления API Azure | Документация Майкрософт
description: Использование архивации и восстановления для выполнения аварийного восстановления в службе управления API Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: e0c02829a2fef6e281794fdba6c9fb5d9b8a736b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241701"
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Реализация аварийного восстановления с помощью функций резервного копирования и восстановления службы в Azure API Management

Опубликовав интерфейсы API и управляя ими с помощью службы управления API Azure, вы получаете возможности по обеспечению отказоустойчивости и организации инфраструктуры, которые в противном случае вам пришлось бы проектировать и внедрять вручную. Платформа Azure устраняет большую часть потенциальных сбоев при относительно небольших затратах.

Чтобы обеспечить восстановление в случае проблем с доступностью в регионе, в котором размещена ваша служба управления API, будьте готовы в любой момент создать ее в другом регионе. В зависимости от целевых показателей доступности и времени восстановления вам, возможно, потребуется создать резервную службу в одном или нескольких регионах. Кроме того, можно попробовать наладить постоянную синхронизацию ее конфигурации и содержимого с активной службой. Функция резервного копирования и восстановления службы служит необходимым фундаментом для реализации стратегии аварийного восстановления.

В этом руководстве показано, как проверять подлинность запросов к Azure Resource Manager, а также как создавать резервные копии экземпляров служб управления API и восстанавливать их.

> [!NOTE]
> Процесс резервного копирования и аварийного восстановления экземпляра службы управления API может использоваться для репликации экземпляров службы управления API, например для реализации промежуточного хранения.
>
> Срок действия каждой резервной копии истекает через 30 дней. Если вы попытаетесь восстановить резервную копию по истечении 30 дней, восстановление завершится сбоем и будет отображено сообщение `Cannot restore: backup expired`.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="authenticating-azure-resource-manager-requests"></a>Проверка подлинности запросов к диспетчеру ресурсов Azure

> [!IMPORTANT]
> Интерфейс REST API для резервного копирования и восстановления использует диспетчер ресурсов Azure и применяет механизм проверки подлинности, отличный от интерфейсов REST API, используемых для управления объектами службы управления API. В этом разделе описываются действия, необходимые для проверки подлинности запросов к диспетчеру ресурсов Azure. Дополнительные сведения см. в [справочнике REST API Azure](/rest/api/index).

Все задачи, которые выполняются в ресурсах с помощью Azure Resource Manager, необходимо аутентифицировать в Azure Active Directory. Для этого:

* добавьте приложение в клиент Azure Active Directory;
* настройте разрешения для добавленного приложения;
* получите маркер для проверки подлинности запросов к диспетчеру ресурсов Azure.

### <a name="create-an-azure-active-directory-application"></a>Создание приложения Azure Active Directory

1. Войдите на [портале Azure](https://portal.azure.com).
2. Используя подписку, включающую экземпляр службы управления API, перейдите на вкладку **Регистрация приложений** в **Azure Active Directory** (Azure Active Directory > Управление/Регистрация приложений).

    > [!NOTE]
    > Если этот каталог не отображается в вашей учетной записи, обратитесь к администратору подписки Azure, чтобы получить необходимые разрешения для учетной записи.
3. Щелкните **Регистрация нового приложения**.

    Справа появится окно **Создание**. Здесь вам нужно ввести информацию о приложении AAD.
4. Введите имя приложения.
5. Выберите тип приложения **Машинный код**.
6. Введите в поле **URI перенаправления** любой URL-адрес, например `http://resources`. Это поле является обязательным, но введенное значение впоследствии не используется. Установите флажок, чтобы сохранить приложение.
7. Нажмите кнопку **Создать**.

### <a name="add-an-application"></a>Добавление приложения

1. После создания приложения щелкните **Параметры**.
2. Теперь щелкните **Требуемые разрешения**.
3. Щелкните **+Добавить**.
4. Нажмите кнопку **Выбор API**.
5. Выберите **Windows** **Azure Service Management API** (API управления службами Azure).
6. Нажмите кнопку **Выбрать**.

    ![Добавление разрешений](./media/api-management-howto-disaster-recovery-backup-restore/add-app.png)

7. Щелкните **Делегированные разрешения** рядом с только что добавленным приложением и установите флажок **Access Azure Service Management (preview)** (Доступ к управлению службами Azure (предварительная версия)).
8. Нажмите кнопку **Выбрать**.
9. Щелкните **Предоставить разрешение**

### <a name="configuring-your-app"></a>Настройка приложения

Прежде чем вызывать интерфейсы API, выполняющие резервное копирование и восстановление, необходимо получить маркер. В следующем примере для получения маркера используется пакет NuGet [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory).

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireTokenAsync("https://management.azure.com/", "{application id}", new Uri("{redirect uri}"), new PlatformParameters(PromptBehavior.Auto)).Result;

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Замените `{tenant id}`, `{application id}` и `{redirect uri}`, следуя приведенным инструкциям.

1. Замените `{tenant id}` идентификатором клиента для созданного приложения Azure Active Directory. Чтобы узнать этот идентификатор, выберите **Регистрация приложений** -> **Конечные точки**.

    ![Конечные точки][api-management-endpoint]
2. Замените `{application id}` значением, которое представлено на странице **Параметры**.
3. Замените значение `{redirect uri}` на значение с вкладки **URI перенаправления** приложения Azure Active Directory.

    Когда все значения будут указаны, этот пример кода должен вернуть примерно такой маркер:

    ![по маркеру][api-management-arm-token]

    > [!NOTE]
    > Срок действия маркера может истечь после определенного периода. Выполнить образец кода снова, чтобы создать новый маркер.

## <a name="calling-the-backup-and-restore-operations"></a>Вызов операций резервного копирования и восстановления

Интерфейсы REST API: [служба управления API — резервное копирование](/rest/api/apimanagement/2019-01-01/apimanagementservice/backup) и [служба управления API — восстановление](/rest/api/apimanagement/2019-01-01/apimanagementservice/restore).

Прежде чем выполнять операции резервного копирования и восстановления, описанные в следующих разделах, задайте заголовок запроса авторизации для вызова REST.

```csharp
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

### <a name="step1"> </a>Архивация службы управления API

Чтобы выполнить архивацию службы управления API, отправьте следующий HTTP-запрос:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}
```

где:

* `subscriptionId` — идентификатор подписки, содержащей службу управления API, для которой вы пытаетесь выполнить резервное копирование;
* `resourceGroupName` — имя группы ресурсов службы управления API Azure
* `serviceName` — имя службы управления API, резервное копирование которой вы выполняете, на момент ее создания;
* `api-version` замените `2018-06-01-preview`.

В тексте запроса укажите целевую учетную запись хранения Azure, ключ доступа, имя контейнера BLOB-объектов и имя резервной копии:

```json
{
  "storageAccount": "{storage account name for the backup}",
  "accessKey": "{access key for the account}",
  "containerName": "{backup container name}",
  "backupName": "{backup blob name}"
}
```

Для заголовка запроса `Content-Type` установите значение `application/json`.

Резервное копирование — это длительная операция, которая занимает несколько минут.  Если запрос выполнен успешно и запущен процесс резервного копирования, вы получите код состояния ответа `202 Accepted` с заголовком `Location`.  Чтобы отслеживать состояние операции, отправляйте запросы GET на URL-адрес, указанный в заголовке `Location` . Пока резервное копирование выполняется, вы получаете код состояния "202 Accepted". Код ответа `200 OK` указывает на то, что резервное копирование успешно завершено.

Отправляя запрос на резервное копирование, учитывайте следующие ограничения:

* **Контейнер**, указанный в теле запроса, **должен существовать**.
* Пока выполняется резервное копирование, **не вносите изменений в управление службами**, таких как повышение или понижение уровня SKU, смена доменного имени и т. д.
* Восстановление **резервной копии гарантируется только в течение 30 дней** с момента ее создания.
* **Данные об использовании**, применяемые при создании аналитических отчетов, **не включаются** в резервную копию. Для периодического извлечения аналитических отчетов с целью их дальнейшего помещения на хранение используйте интерфейс [REST API управления API Azure][Azure API Management REST API].
* Частота резервного копирования службы влияет на целевую точку восстановления. Чтобы максимально снизить этот показатель, мы советуем настроить регулярное резервное копирование, а также выполнять резервное копирование по требованию после внесения изменений в службу управления API.
* **Изменения**, внесенные в конфигурацию службы (интерфейсы API, политики, внешний вид портала разработчика) во время резервного копирования, **могут быть исключены из резервной копии и утеряны**.

### <a name="step2"> </a>Восстановление службы управления API

Чтобы восстановить службу управления API из ранее созданной резервной копии, отправьте следующий HTTP-запрос:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}
```

где:

* `subscriptionId` — идентификатор подписки, включающей в себя службу управления API, которую нужно восстановить из резервной копии;
* `resourceGroupName` — имя группы ресурсов, включающей в себя службу управления API Azure, в которую нужно восстановить резервную копию;
* `serviceName` — имя восстанавливаемой службы управления API на момент ее создания;
* `api-version` замените `2018-06-01-preview`.

В тексте запроса укажите расположение файла резервной копии, то есть добавьте целевую учетную запись хранения Azure, ключ доступа, имя контейнера больших двоичных объектов и имя резервной копии:

```json
{
  "storageAccount": "{storage account name for the backup}",
  "accessKey": "{access key for the account}",
  "containerName": "{backup container name}",
  "backupName": "{backup blob name}"
}
```

Для заголовка запроса `Content-Type` установите значение `application/json`.

Восстановление — это длительная операция, которая может занять до 30 минут и более. Если запрос выполнен успешно и процесс восстановления инициирован, вы получите код состояния ответа `202 Accepted` с заголовком `Location`. Чтобы отслеживать состояние операции, отправляйте запросы GET на URL-адрес, указанный в заголовке `Location` . Пока восстановление выполняется, вы получаете код состояния "202 Accepted". Код ответа `200 OK` указывает на то, что восстановление успешно завершено.

> [!IMPORTANT]
> **Номер SKU** восстанавливаемой службы **должен совпадать** с номером SKU, сохраненным в резервной копии.
>
> **Изменения**, внесенные в конфигурацию службы (например, интерфейсы API, политики, внешний вид портала разработчика) во время восстановления, **могут быть перезаписаны**.

<!-- Dummy comment added to suppress markdown lint warning -->

> [!NOTE]
> Операции резервного копирования и восстановления могут также выполняться с помощью PowerShell *резервного копирования AzApiManagement* и *AzApiManagement восстановления* команд соответственно.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы ознакомиться с другими способами резервного копирования и восстановления, ознакомьтесь со следующими ресурсами.

* [Репликация учетных записей управления API Azure](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
* [Automating API Management Backup and Restore with Logic Apps](https://github.com/Azure/api-management-samples/tree/master/tutorials/automating-apim-backup-restore-with-logic-apps) (Автоматизация управления API резервного копирования и восстановления с помощью Logic Apps)
* В статье [Управление API Azure: резервное копирование и восстановление конфигурации](https://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  *Стюарт подробно описывает альтернативный подход, который отличается от официальных рекомендаций, но тоже заслуживает внимания.*

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2

[Azure API Management REST API]: https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
