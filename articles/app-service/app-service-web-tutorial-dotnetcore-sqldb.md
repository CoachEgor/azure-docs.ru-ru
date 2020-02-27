---
title: Руководство по Использование ASP.NET Core с Базой данных SQL
description: Узнайте, как создать приложение .NET Core, работающее в службе приложений Azure, с подключением к базе данных SQL.
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 28dc5131366c54db30e9233ea1061cc9bc7a54ce
ms.sourcegitcommit: 0a9419aeba64170c302f7201acdd513bb4b346c8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77500074"
---
# <a name="tutorial-build-an-aspnet-core-and-sql-database-app-in-azure-app-service"></a>Руководство по Создание приложения ASP.NET Core и Базы данных SQL в Службе приложений Azure

> [!NOTE]
> В этой статье мы развернем приложение в службе приложений на платформе Windows. Дополнительные сведения о развертывании в Службе приложений в _Linux_ см. в руководстве по [созданию приложения .NET Core с Базой данных SQL в Службе приложений Azure в Linux](./containers/tutorial-dotnetcore-sqldb-app.md).
>

[Служба приложений](overview.md) — это служба веб-размещения с самостоятельной установкой исправлений и высоким уровнем масштабируемости в Azure. В этом руководстве показано, как создать приложение .NET Core и подключить его к Базе данных SQL. После выполнения всех действий у вас будет приложение .NET Core MVC, работающее в службе приложений.

![Приложение, работающее в службе приложений](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание базы данных SQL в Azure.
> * Подключение приложения .NET Core к базе данных SQL.
> * Развертывание приложения в Azure
> * Обновление модели данных и повторное развертывание приложения.
> * Потоковая передача журналов диагностики из Azure.
> * Управление приложением на портале Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством сделайте следующее:

* [установите Git](https://git-scm.com/);
* [установите .NET Core](https://www.microsoft.com/net/core/).

## <a name="create-local-net-core-app"></a>Создание локального приложения .NET Core

На этом шаге вы настроите локальный проект .NET Core.

### <a name="clone-the-sample-application"></a>Клонирование примера приложения

Откройте окно терминала и c помощью команды `cd` перейдите в рабочий каталог.

Выполните следующие команды, чтобы клонировать пример репозитория и заменить его на корневой репозиторий.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

Этот пример проекта содержит простое приложение CRUD, созданное на основе [Entity Framework Core](https://docs.microsoft.com/ef/core/).

### <a name="run-the-application"></a>Выполнение приложения

Выполните указанные ниже команды, чтобы установить необходимые пакеты, перенести базы данных и запустить приложение.

```bash
dotnet restore
dotnet ef database update
dotnet run
```

Откройте браузер и перейдите по адресу `http://localhost:5000`. Щелкните ссылку **Создать**, чтобы создать несколько элементов _списка дел_.

![Успешное подключение к базе данных SQL](./media/app-service-web-tutorial-dotnetcore-sqldb/local-app-in-browser.png)

Чтобы остановить приложение .NET Core в любое время, нажмите `Ctrl+C` в окне терминала.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>Создание рабочей базы данных SQL

На этом шаге вы создадите базу данных SQL в Azure. При развертывании приложения в Azure используется эта облачная база данных.

В этом руководстве для создания базы данных SQL используется [База данных SQL Azure](/azure/sql-database/).

### <a name="create-a-resource-group"></a>Создание группы ресурсов

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>Создание логического сервера базы данных SQL

В Cloud Shell создайте логический сервер базы данных SQL с помощью команды [`az sql server create`](/cli/azure/sql/server?view=azure-cli-latest#az-sql-server-create).

Замените заполнитель *\<server_name>* уникальным именем базы данных SQL. Это имя используется как часть конечной точки базы данных SQL (`<server_name>.database.windows.net`), поэтому оно должно быть уникальным для логических серверов в Azure. В нем могут использоваться только строчные буквы, цифры и дефис (-). Его длина должна быть от 3 до 50 знаков. Кроме того, замените *\<db_username >* на имя пользователя, а *\<db_password >*  — на пароль по своему усмотрению. 


```azurecli-interactive
az sql server create --name <server_name> --resource-group myResourceGroup --location "West Europe" --admin-user <db_username> --admin-password <db_password>
```

После создания логического сервера базы данных SQL в Azure CLI отображаются примерно такие сведения:

```json
{
  "administratorLogin": "sqladmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/<server_name>",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "<server_name>",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
```

### <a name="configure-a-server-firewall-rule"></a>Настройка правил брандмауэра сервера

Создайте [правило брандмауэра уровня сервера Базы данных SQL Azure](../sql-database/sql-database-firewall-configure.md) с помощью команды [`az sql server firewall create`](/cli/azure/sql/server/firewall-rule?view=azure-cli-latest#az-sql-server-firewall-rule-create). Если для начального и конечного IP-адресов задано значение 0.0.0.0, брандмауэр открыт только для других ресурсов Azure. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server_name> --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP] 
> Вы можете применить еще более строгие ограничения в правиле брандмауэра, [разрешив только исходящие IP-адреса, используемые приложением](overview-inbound-outbound-ips.md#find-outbound-ips).
>

### <a name="create-a-database"></a>Создание базы данных

Создайте на сервере базу данных с [уровнем производительности S0](../sql-database/sql-database-service-tiers-dtu.md) с помощью команды [`az sql db create`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-create).

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server_name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Создание строки подключения

В следующей строке замените *\<server_name>* , *\<db_username>* и *\<db_password>* на использованные ранее значения.

```
Server=tcp:<server_name>.database.windows.net,1433;Database=coreDB;User ID=<db_username>;Password=<db_password>;Encrypt=true;Connection Timeout=30;
```

Это строка подключения для вашего приложения .NET Core. Скопируйте ее для дальнейшего использования.

## <a name="deploy-app-to-azure"></a>Развертывание приложения в Azure

На этом шаге вы развернете приложение .NET Core, подключенное к базе данных SQL, в службе приложений.

### <a name="configure-local-git-deployment"></a>Настройка локального развертывания Git

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Создание плана службы приложений

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a>Создание веб-приложения

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-dotnetcore-win-no-h.md)] 

### <a name="configure-connection-string"></a>Настройка строки подключения

Чтобы задать строки подключения для приложения Azure, используйте команду [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) в Cloud Shell. В следующей команде замените *\<app name>* на собственное значение, а параметр *\<connection_string>*  — на строку подключения, созданную ранее.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection="<connection_string>" --connection-string-type SQLServer
```

В ASP.NET Core можно использовать эту именованную строку подключения (`MyDbConnection`) со стандартным шаблоном, как и любую строку подключения, указанную в файле *appsettings.json*. В этом случае `MyDbConnection` также определяется в файле *appsettings.json*. Если вы работаете в Службе приложений, определенная в ней строка подключения имеет приоритет над строкой подключения, определенной в файле *appsettings.json*. Код использует значение, указанное в файле *appsettings.json*, во время локальной разработки, и тот же код использует значение Службы приложений при развертывании.

Сведения о том, как в коде указывается ссылка на строку подключения, см. в разделе о [подключении к Базе данных SQL в рабочей среде](#connect-to-sql-database-in-production).

### <a name="configure-environment-variable"></a>Настройка переменной среды

Затем задайте для параметра приложения `ASPNETCORE_ENVIRONMENT` значение _Production_. Этот параметр позволяет определить, выполняется ли приложение в Azure, так как SQLite применяется для локальной среды разработки, а База данных SQL — для среды Azure.

В следующем примере настраивается параметр приложения `ASPNETCORE_ENVIRONMENT` в приложении Azure. Замените заполнитель *\<app_name>* собственным значением.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
```

Сведения о том, как в коде указывается ссылка на переменную среды, см. в разделе о [подключении к Базе данных SQL в рабочей среде](#connect-to-sql-database-in-production).

### <a name="connect-to-sql-database-in-production"></a>Подключение к базе данных SQL в рабочей среде

В локальном репозитории откройте файл Startup.cs и найдите такой код:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Замените его следующим кодом, в котором используются переменные среды, заданные ранее.

```csharp
// Use SQL Database if in Azure, otherwise, use SQLite
if(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlite("Data Source=localdatabase.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<MyDatabaseContext>().Database.Migrate();
```

Если этот код определяет, что он выполняется в рабочей среде (что указывает на среду Azure), он использует строку подключения, настроенную для подключения к Базе данных SQL.

Вызов `Database.Migrate()` полезен при выполнении в среде Azure, так как он позволяет автоматически создавать базы данных, необходимые для приложения .NET Core, в зависимости от конфигурации миграции. 

> [!IMPORTANT]
> Для рабочих приложений, которые нужно масштабировать, следуйте рекомендациям из раздела [Применение миграций в рабочей среде](/aspnet/core/data/ef-rp/migrations#applying-migrations-in-production).
> 

Сохраните изменения и зафиксируйте их в репозитории Git. 

```bash
git add .
git commit -m "connect to SQLDB in Azure"
```

### <a name="push-to-azure-from-git"></a>Публикация в Azure из Git

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 98, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (92/92), done.
Writing objects: 100% (98/98), 524.98 KiB | 5.58 MiB/s, done.
Total 98 (delta 8), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: .
remote: Updating submodules.
remote: Preparing deployment for commit id '0c497633b8'.
remote: Generating deployment script.
remote: Project file path: ./DotNetCoreSqlDb.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-azure-app"></a>Переход к приложению Azure

Перейдите к развернутому приложению в веб-браузере.

```bash
http://<app_name>.azurewebsites.net
```

Добавьте несколько элементов списка дел.

![Приложение, работающее в службе приложений](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

**Поздравляем!** Вы запустили приложение .NET Core на основе данных в службе приложений.

## <a name="update-locally-and-redeploy"></a>Обновление на локальном компьютере и повторное развертывание

На этом шаге вы измените схему базы данных и опубликуете ее в Azure.

### <a name="update-your-data-model"></a>Обновление модели данных

Откройте файл _Models\Todo.cs_ в редакторе кода. Добавьте в класс `ToDo` следующее свойство:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Локальный запуск Code First Migrations

Выполните несколько команд, чтобы обновить локальную базу данных.

```bash
dotnet ef migrations add AddProperty
```

Обновите локальную базу данных:

```bash
dotnet ef database update
```

### <a name="use-the-new-property"></a>Использование нового свойства

Внесите некоторые изменения в код, чтобы использовалось свойство `Done`. Для простоты мы изменим только представления `Index` и `Create`, чтобы просмотреть свойство в действии.

Откройте файл _Controllers\TodosController.cs_.

Найдите метод `Create([Bind("ID,Description,CreatedDate")] Todo todo)` и добавьте `Done` в список свойств атрибута `Bind`. Когда все будет готово, сигнатура метода `Create()` должна выглядеть следующим образом:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Откройте файл _Views\Todos\Create.cshtml_.

В коде Razor вы должны увидеть элемент `<div class="form-group">` для `Description` и еще один элемент `<div class="form-group">` для `CreatedDate`. Сразу после этих двух элементов добавьте еще один элемент `<div class="form-group">` для `Done`:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
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

Найдите элемент `<td>`, который содержит вспомогательные функции тегов `asp-action`. Добавьте следующий код Razor над этим элементом:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Это все, что нужно сделать, чтобы увидеть изменения в представлениях `Index` и `Create`.

### <a name="test-your-changes-locally"></a>Проверьте изменения локально.

Запустите приложение на локальном компьютере.

```bash
dotnet run
```

В браузере перейдите по адресу `http://localhost:5000/`. Теперь вы сможете добавить элемент списка дел и установить флажок **Готово**. После этого задание должно появиться на главной странице как выполненное. Помните, что в представлении `Edit` не отображается поле `Done`, так как вы не изменили представление `Edit`.

### <a name="publish-changes-to-azure"></a>Публикация изменений в Azure

```bash
git add .
git commit -m "added done field"
git push azure master
```

Когда команда `git push` будет выполнена, перейдите к приложению Службы приложений, попробуйте добавить элемент задачи и установите флажок **Готово**.

![Приложение Azure после включения Code First Migration](./media/app-service-web-tutorial-dotnetcore-sqldb/this-one-is-done.png)

Все имеющиеся элементы списка дел по-прежнему отображаются. При повторной публикации приложения .NET Core существующие данные в Базе данных SQL не теряются. Кроме того, Entity Framework Core Migrations изменяет только схему данных, оставляя существующие данные нетронутыми.

## <a name="stream-diagnostic-logs"></a>Потоковая передача журналов диагностики

При запуске приложения ASP.NET Core в Службе приложений Azure можно передавать журналы консоли в Cloud Shell. Таким образом, вы будете получать те же диагностические сообщения, которые помогут устранить ошибки приложения.

В примере проекта уже выполнены инструкции из руководства [Ведение журналов в ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging#azure-app-service-provider) и внесены два изменения в настройки:

- в файле *DotNetCoreSqlDb.csproj* содержится ссылка на `Microsoft.Extensions.Logging.AzureAppServices`.
- Вызовы `loggerFactory.AddAzureWebAppDiagnostics()` в *Program.cs*.

Чтобы в Службе приложений для [уровня ведения журнала](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-level) ASP.NET Core задать значение `Information` вместо значения по умолчанию `Error`, используйте команду [`az webapp log config`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-config) в Cloud Shell.

```azurecli-interactive
az webapp log config --name <app_name> --resource-group myResourceGroup --application-logging true --level information
```

> [!NOTE]
> Для уровня ведения журнала проекта уже задано значение `Information` в файле *appsettings.json*.
> 

Чтобы настроить потоки для журналов, выполните команду [`az webapp log tail`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-tail) в Cloud Shell.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
```

После настройки потоков обновите приложение Azure в браузере, чтобы получить немного трафика. Вы должны увидеть, что журналы консоли теперь направляются в терминал. Если журналы консоли не отображаются, проверьте еще раз через 30 секунд.

Чтобы остановить потоковую передачу журналов, нажмите клавиши `Ctrl`+`C`.

Дополнительные сведения о настройке журналов ASP.NET Core см. в статье [Ведение журналов в ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="manage-your-azure-app"></a>Управление приложением Azure

Чтобы просмотреть созданное приложение, на [портале Azure](https://portal.azure.com) найдите в поиске и выберите **Службы приложений**.

![Выберите службы приложений на портале Azure](./media/app-service-web-tutorial-dotnetcore-sqldb/app-services.png)

На странице **Службы приложений** выберите имя приложения Azure.

![Переход к приложению Azure на портале](./media/app-service-web-tutorial-dotnetcore-sqldb/access-portal.png)

По умолчанию на портале отображается страница **Обзор** приложения. Здесь вы можете наблюдать за работой приложения. Вы также можете выполнять базовые задачи управления: обзор, завершение, запуск, перезагрузку и удаление. В левой части страницы отображаются различные страницы конфигурации, которые можно открыть.

![Страница службы приложений на портале Azure](./media/app-service-web-tutorial-dotnetcore-sqldb/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Дальнейшие действия

Вы научились выполнять следующие задачи:

> [!div class="checklist"]
> * Создание базы данных SQL в Azure.
> * Подключение приложения .NET Core к базе данных SQL.
> * Развертывание приложения в Azure
> * Обновление модели данных и повторное развертывание приложения.
> * Потоковая передача журналов из Azure в окно терминала.
> * Управление приложением на портале Azure.

Перейдите к следующему руководству, чтобы научиться сопоставлять пользовательские DNS-имена с приложением.

> [!div class="nextstepaction"]
> [Сопоставление существующего настраиваемого DNS-имени со Службой приложений Azure](app-service-web-tutorial-custom-domain.md)
