---
title: Руководство по использованию приложения ASP.NET со службой "База данных Azure SQL"
description: Узнайте, как развернуть приложение C# ASP.NET в Azure и в службу "База данных Azure SQL"
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/25/2018
ms.custom: mvc, devcenter, vs-azure, seodec18
ms.openlocfilehash: a1fdc9e37084b1e8064bf642ff84646149c32ee7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "87050671"
---
# <a name="tutorial-deploy-an-aspnet-app-to-azure-with-azure-sql-database"></a>Руководство по развертыванию приложения ASP.NET в Azure со службой "База данных SQL Azure"

[Служба приложений Azure](overview.md) — это служба веб-размещения с самостоятельной установкой исправлений и высоким уровнем масштабируемости. В этом руководстве показано, как развернуть управляемое данными приложение ASP.NET в Службе приложений, а затем подключить его к [Базе данных SQL Azure](../azure-sql/database/sql-database-paas-overview.md). Выполнив описанные здесь действия, вы получите приложение ASP.NET, запущенное в Azure и подключенное к Базе данных SQL.

![Опубликованное приложение ASP.NET в Службе приложений Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

В этом руководстве описано следующее:

> [!div class="checklist"]
>
> * Создание базы данных в службе "База данных SQL Azure"
> * Подключение приложения ASP.NET к базе данных SQL.
> * Развертывание приложения в Azure
> * Обновление модели данных и повторное развертывание приложения.
> * Потоковая передача журналов из Azure в окно терминала.
> * Управление приложением на портале Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством сделайте следующее:

Установите <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> с рабочей нагрузкой **ASP.NET и веб-разработка**.

Если вы уже установили Visual Studio, добавьте в него рабочие нагрузки. Для этого последовательно выберите **Инструменты** > **Get Tools and Features (Получить инструменты и функции)** .

## <a name="download-the-sample"></a>Скачивание примера приложения

* [Загрузите пример проекта](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).
* Извлеките (распакуйте) файл *dotnet-sqldb-tutorial-master.zip*.

Этот пример проекта содержит простое CRUD-приложение [ASP.NET MVC](https://www.asp.net/mvc), созданное на основе [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="run-the-app"></a>Запустите приложение

Откройте файл *dotnet-sqldb-tutorial-master/DotNetAppSqlDb.sln* в Visual Studio.

Введите `Ctrl+F5`, чтобы запустить приложение без отладки. Это приложение откроется в браузере по умолчанию. Щелкните ссылку **Создать**, чтобы создать несколько элементов *списка дел*.

![Диалоговое окно "Новый проект ASP.NET"](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Проверьте ссылки **Изменить**, **Сведения** и **Удалить**.

Чтобы подключиться к базе данных, приложение использует контекст базы данных. В этом примере контекст базы данных использует строку подключения `MyDbConnection`. Строка подключения определена в файле *Web.config*. Ссылка на нее также имеется в файле *Models/MyDatabaseContext.cs*. Имя строки подключения потребуется далее в этом руководстве при подключении приложения Azure к базе данных SQL.

## <a name="publish-aspnet-application-to-azure"></a>Публикация приложения ASP.NET в Azure

В **обозревателе решений** щелкните правой кнопкой мыши проект **DotNetAppSqlDb** и выберите **Опубликовать**.

![Публикация в обозревателе решений](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Выберите **Служба приложений Microsoft Azure** и нажмите кнопку **Опубликовать**.

![Публикация с помощью страницы обзора проекта](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

Во время публикации откроется диалоговое окно **Создать службу приложений**, с помощью которого вы можете создать все ресурсы Azure, необходимые для запуска приложения ASP.NET в Azure.

### <a name="sign-in-to-azure"></a>Вход в Azure

В диалоговом окне **Создать службу приложений** щелкните **Добавить новую учетную запись**, а затем выполните вход в подписку Azure. Если вы уже вошли в учетную запись Майкрософт, проверьте, содержит ли она подписку Azure. Если подписки нет, щелкните ее, чтобы добавить правильную учетную запись.

> [!NOTE]
> Если вы уже выполнили вход, пока не нажимайте кнопку **Создать**.

![Вход в Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

### <a name="configure-the-web-app-name"></a>Настройка имени веб-приложения

Вы можете использовать созданное имя веб-приложения или присвоить ему уникальное имя (допустимые символы: `a-z`, `0-9` и `-`). Это имя используется как часть URL-адреса по умолчанию для приложения (`<app_name>.azurewebsites.net`, где `<app_name>` — имя вашего веб-приложения). Оно должно быть глобально уникальным среди всех приложений Azure.

![Диалоговое окно "Создание службы приложений"](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a>Создание группы ресурсов

[!INCLUDE [resource-group](../../includes/resource-group.md)]

1. Рядом с **группой ресурсов** щелкните **Создать**.

   ![Рядом с группой ресурсов щелкните "Создать".](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

2. Присвойте группе ресурсов имя **myResourceGroup**.

### <a name="create-an-app-service-plan"></a>Создание плана службы приложений

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

1. Рядом с **планом службы приложений** щелкните **Создать**.

2. В диалоговом окне **Настроить план службы приложений** настройте новый план службы приложений, задав приведенные ниже параметры.

   ![Создание плана службы приложений](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

   | Параметр  | Рекомендуемое значение | Дополнительные сведения |
   | ----------------- | ------------ | ----|
   |**План службы приложений**| myAppServicePlan | [Планы службы приложений](../app-service/overview-hosting-plans.md) |
   |**Расположение**| Западная Европа | [Регионы Azure](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |
   |**Размер**| Бесплатный | [Ценовые категории](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)|

### <a name="create-a-server"></a>Создание сервера

Перед созданием базы данных необходимо создать [логический сервер SQL](../azure-sql/database/logical-servers.md). Логический сервер SQL — это логическая конструкция, которая содержит группу баз данных, которыми можно управлять как группой.

1. Щелкните **Создать базу данных SQL**.

   ![Создание базы данных SQL](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

2. В окне **Настройка базы данных SQL** нажмите кнопку **Создать** рядом с **SQL Server**.

   Создается уникальное имя сервера. Это имя используется как часть URL-адреса по умолчанию для сервера (`<server_name>.database.windows.net`). Оно должно быть уникальным на всех серверах в Azure SQL. Имя сервера можно изменить, но в рамках этого руководства используйте созданное значение.

3. Добавьте имя пользователя и пароль администратора. Требования к сложности пароля см. в статье [Политика паролей](/sql/relational-databases/security/password-policy).

   Запомните это имя пользователя и пароль. Они потребуются позже для управления сервером.

   > [!IMPORTANT]
   > Хотя пароль в строке подключения скрывается (в Visual Studio и в службе приложений), он все равно где-то сохраняется, что делает приложение уязвимым перед атаками. В таком случае служба приложений может использовать [управляемые удостоверения службы](overview-managed-identity.md), полностью устраняя необходимость хранить секреты в коде или конфигурации приложения. Дополнительные сведения см. в разделе [Дальнейшие действия](#next-steps).

   ![Создание сервера](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

4. Нажмите кнопку **ОК**. Пока не закрывайте диалоговое окно **Настроить базу данных SQL**.

### <a name="create-a-database-in-azure-sql-database"></a>Создание базы данных в службе "База данных SQL Azure"

1. В диалоговом окне **Настроить базу данных SQL**:

   * В поле **Имя базы данных** не изменяйте созданное по умолчанию имя.
   * В поле **Имя строки подключения** введите *MyDbConnection*. Это имя должно совпадать с именем строки подключения, указанном в *Models\MyDatabaseContext.cs*.
   * Щелкните **ОК**.

    ![Настройка базы данных](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

2. В диалоговом окне **Создать службу приложений** отобразятся настроенные ресурсы. Нажмите кнопку **Создать**.

   ![Созданные ресурсы](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

Когда мастер завершит создание ресурсов Azure, он опубликует приложение ASP.NET в Azure. Откроется браузер по умолчанию с URL-адресом развернутого приложения.

Добавьте несколько элементов списка дел.

![Опубликованное приложение ASP.NET в приложении Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Поздравляем! Вы запустили управляемое данными приложение ASP.NET в службе приложений Azure.

## <a name="access-the-database-locally"></a>Локальный доступ к базе данных

В **обозревателе объектов SQL Server** Visual Studio вы сможете легко просматривать свою новую базу данных и управлять ею.

### <a name="create-a-database-connection"></a>Создание подключения к базе данных

В меню **Представление** выберите **Обозреватель объектов SQL Server**.

В верхней части **обозревателя объектов SQL Server** нажмите кнопку **Добавить SQL Server**.

### <a name="configure-the-database-connection"></a>Настройка подключения к базе данных

В диалоговом окне **Подключение** разверните узел **Azure**. Здесь перечислены все экземпляры базы данных SQL в Azure.

Выберите ранее созданную базу данных. В нижней части автоматически появится созданное ранее подключение.

Введите созданный ранее пароль администратора базы данных и нажмите кнопку **Подключиться**.

![Настройка подключения к базе данных из Visual Studio](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>Разрешение клиентских подключений с вашего компьютера

Откроется диалоговое окно **Создать новое правило брандмауэра**. По умолчанию сервер разрешает подключаться к его базам данных только из служб Azure, таких как ваше приложение Azure. Чтобы подключаться к базе данных извне Azure, создайте правило брандмауэра на уровне сервера. Это правило разрешает подключения с общедоступного IP-адреса вашего локального компьютера.

В диалоговом окне уже указан общедоступный IP-адрес компьютера.

Убедитесь, что флажок **Добавить IP-адрес моего клиента** установлен, и щелкните **ОК**.

![Создание правила брандмауэра](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

После окончания настройки брандмауэра для экземпляра базы данных SQL ваше подключение появится в **обозревателе объектов SQL Server**.

В нем вы можете выполнять большинство распространенных операций с базой данных: выполнять запросы, создавать представления и хранимые процедуры и многое другое.

Разверните узел подключения, выберите **Базы данных** >  **&lt;ваша база данных >**  > **Таблицы**. Щелкните таблицу `Todoes` правой кнопкой мыши и выберите **Просмотреть данные**.

![Просмотр объектов базы данных SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>Изменение приложения с помощью Code First Migrations

Обновить базу данных и приложение в Azure можно с помощью знакомых инструментов Visual Studio. На этом шаге вы измените схему базы данных с помощью Code First Migrations в Entity Framework и опубликуете ее в Azure.

Дополнительные сведения об использовании Entity Framework Code First Migrations см. в статье [Getting Started with Entity Framework 6 Code First using MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application) (Начало работы с Entity Framework 6 Code First с помощью MVC 5).

### <a name="update-your-data-model"></a>Обновление модели данных

Откройте файл _Models\Todo.cs_ в редакторе кода. Добавьте в класс `ToDo` следующее свойство:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Локальный запуск Code First Migrations

Выполните несколько команд, чтобы обновить локальную базу данных.

В меню **Инструменты** выберите **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.

В окне консоли диспетчера пакетов включите Code First Migrations:

```powershell
Enable-Migrations
```

Добавьте миграцию:

```powershell
Add-Migration AddProperty
```

Обновите локальную базу данных:

```powershell
Update-Database
```

Введите `Ctrl+F5`, чтобы запустить приложение. Проверьте ссылки "Изменить", "Сведения" и "Создать".

Если приложение загружается без ошибок, Code First Migrations успешно включен. Однако ваша страница не изменилась, так как новое свойство все еще не используется в логике приложения.

### <a name="use-the-new-property"></a>Использование нового свойства

Внесите некоторые изменения в код, чтобы использовалось свойство `Done`. Для простоты мы изменим только представления `Index` и `Create`, чтобы просмотреть свойство в действии.

Откройте файл _Controllers\TodosController.cs_.

Найдите метод `Create()` в строке 52 и добавьте `Done` в список свойств атрибута `Bind`. Когда все будет готово, сигнатура метода `Create()` должна выглядеть следующим образом:

```csharp
public ActionResult Create([Bind(Include = "Description,CreatedDate,Done")] Todo todo)
```

Откройте файл _Views\Todos\Create.cshtml_.

В коде Razor вы должны увидеть элемент `<div class="form-group">`, который использует `model.Description`, и еще один элемент `<div class="form-group">`, который использует `model.CreatedDate`. Сразу после этих двух элементов добавьте еще один элемент `<div class="form-group">`, который использует `model.Done`:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

Откройте файл _Views\Todos\Index.cshtml_.

Найдите пустой элемент `<th></th>`. Добавьте следующий код Razor над этим элементом:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Найдите элемент `<td>`, который содержит вспомогательные методы `Html.ActionLink()`. _Над_ элементом `<td>` добавьте еще один элемент `<td>` со следующим кодом Razor:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Это все, что нужно сделать, чтобы увидеть изменения в представлениях `Index` и `Create`.

Введите `Ctrl+F5`, чтобы запустить приложение.

Теперь вы сможете добавить элемент списка дел и установить флажок **Готово**. После этого задание должно появиться на главной странице как выполненное. Помните, что в представлении `Edit` не отображается поле `Done`, так как вы не изменили представление `Edit`.

### <a name="enable-code-first-migrations-in-azure"></a>Включение Code First Migrations в Azure

Теперь, когда изменения в коде, включая миграцию базы данных, выполнены успешно, можно опубликовать изменения в приложение Azure, а также обновить Базу данных SQL для использования Code First Migrations.

Как и прежде, щелкните правой кнопкой мыши свой проект и выберите **Опубликовать**.

Щелкните **Настройка**, чтобы открыть параметры публикации.

![Открытие параметров публикации](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

В мастере нажмите кнопку **Далее**.

Убедитесь, что строка подключения для базы данных SQL в контексте **MyDatabaseContext (MyDbConnection)** заполнена. Возможно, вам потребуется выбрать базу данных **myToDoAppDb** из раскрывающегося списка.

Установите флажок **Выполнять Code First Migrations (при запуске приложения)** и нажмите кнопку **Сохранить**.

![Включение Code First Migrations в приложении Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>Публикация изменений

Включив Code First Migrations в приложении Azure, опубликуйте изменения кода.

На странице публикации щелкните **Опубликовать**.

Попробуйте добавить новые задачи, устанавливая флажок **Готово**. Эти задачи должны появляться на главной странице как выполненные.

![Приложение Azure после включения Code First Migration](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Все имеющиеся элементы списка дел по-прежнему отображаются. При повторной публикации приложения ASP.NET существующие данные в базе данных SQL не теряются. Кроме того, Code First Migrations изменяет только схему данных, оставляя существующие данные нетронутыми.

## <a name="stream-application-logs"></a>Потоковая передача журналов приложений

Сообщения трассировки можно передавать прямо из приложения Azure в Visual Studio.

Откройте файл _Controllers\TodosController.cs_.

Каждое действие начинается с метода `Trace.WriteLine()`. Этот код добавлен, чтобы показать, как добавить сообщения трассировки в приложение Azure.

### <a name="open-server-explorer"></a>Откройте обозреватель сервера

В меню **Представление** выберите **Обозреватель серверов**. Ведение журнала для приложения Azure можно включить в **обозревателе сервера**.

### <a name="enable-log-streaming"></a>Включение потоковой передачи журналов

В **обозревателе сервера** выберите **Azure** > **Служба приложений**.

Разверните группу ресурсов **myResourceGroup**, которая была создана при создании приложения Azure.

Щелкните приложение правой кнопкой мыши и выберите **Просмотреть журналы потоковой передачи**.

![Включение потоковой передачи журналов](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

Теперь журналы передаются в окно **Выходные данные**.

![Потоковая передача журналов в окне "Выходные данные"](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Однако сообщения трассировки пока не отображаются. Это объясняется тем, что когда вы в первый раз выбираете пункт меню **Просмотреть журналы потоковой передачи**, приложение устанавливает уровень трассировки в `Error`, при котором в журнал записываются только события ошибок (с помощью метода `Trace.TraceError()`).

### <a name="change-trace-levels"></a>Изменение уровней трассировки

Чтобы изменить уровень трассировки для вывода других сообщений трассировки, вернитесь в **обозреватель сервера**.

Снова щелкните приложение правой кнопкой мыши и выберите **Просмотр параметров**.

В раскрывающемся списке **Ведение журнала приложения (файловая система)** выберите **Подробно**. Выберите команду **Сохранить**.

![Измените уровень трассировки на "Подробно"](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> Вы можете поэкспериментировать с различными уровнями трассировки, чтобы посмотреть, какие типы сообщений отображаются для каждого уровня. Например, уровень **Информация** включает все журналы, созданные `Trace.TraceInformation()`, `Trace.TraceWarning()`, и `Trace.TraceError()`, но не включает журналы, созданные `Trace.WriteLine()`.

В браузере снова перейдите к приложению по адресу *http://&lt;имя вашего приложения>.azurewebsites.net*, а затем щелкните правой кнопкой приложение списка дел в Azure. Теперь сообщения трассировки передаются в окно **Выходные данные** в Visual Studio.

```console
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```

### <a name="stop-log-streaming"></a>Выключение потоковой передачи журналов

Чтобы остановить службу потоковой передачи журналов, нажмите кнопку **Остановить наблюдение** в окне **Выходные данные**.

![Выключение потоковой передачи журналов](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-app"></a>Управление приложением Azure

Перейдите на [портал Azure](https://portal.azure.com) для управления веб-приложением. Найдите в поиске и выберите **Службы приложений**.

![Поиск Служб приложений Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-portal-navigate-app-services.png)

Выберите имя приложения Azure.

![Переход к приложению Azure на портале](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

Вы попадете на страницу приложения.

По умолчанию на портале отображается страница **Обзор**. Здесь вы можете наблюдать за работой приложения. Вы также можете выполнять базовые задачи управления: обзор, завершение, запуск, перезагрузку и удаление. На вкладках в левой части страницы отображаются различные страницы конфигурации, которые можно открыть.

![Страница службы приложений на портале Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как выполнять следующие задачи:

> [!div class="checklist"]
>
> * Создание базы данных в службе "База данных SQL Azure"
> * Подключение приложения ASP.NET к базе данных SQL.
> * Развертывание приложения в Azure
> * Обновление модели данных и повторное развертывание приложения.
> * Потоковая передача журналов из Azure в окно терминала.
> * Управление приложением на портале Azure.

Переходите к следующему руководству, чтобы узнать, как без усилий обеспечить защиту подключения к базе данных SQL Azure.

> [!div class="nextstepaction"]
> [Безопасный доступ к Базе данных SQL с использованием управляемого удостоверения для ресурсов Azure](app-service-web-tutorial-connect-msi.md)

Дополнительные ресурсы

> [!div class="nextstepaction"]
> [Настройка приложения ASP.NET](configure-language-dotnet-framework.md)

Хотите оптимизировать и сократить ваши расходы на облако?

> [!div class="nextstepaction"]
> [Начните анализировать затраты с помощью службы "Управление затратами"](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)