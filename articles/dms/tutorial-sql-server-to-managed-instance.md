---
title: Руководство по Перенос в управляемый экземпляр Базы данных SQL Azure с помощью DMS | Документация Майкрософт
description: Узнайте, как выполнять миграцию из локального экземпляра SQL Server в управляемый экземпляр Базы данных SQL Azure с помощью Azure Database Migration Service.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: c768b7548b9759e85ebfb050f0ead2dfd3c1a6a6
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415112"
---
# <a name="tutorial-migrate-sql-server-to-an-azure-sql-database-managed-instance-offline-using-dms"></a>Руководство по Перенос баз данных SQL Server в управляемый экземпляр Базы данных SQL Azure с помощью DMS в автономном режиме

Azure Database Migration Service можно использовать для переноса баз данных из локального экземпляра SQL Server в [управляемый экземпляр Базы данных SQL Azure](../sql-database/sql-database-managed-instance.md). Сведения о дополнительных методах, которые могут потребовать некоторых действий вручную, см. в статье [Перенос экземпляра SQL Server в управляемый экземпляр Базы данных SQL Azure](../sql-database/sql-database-managed-instance-migrate.md).

В этом руководстве выполняется перенос базы данных **Adventureworks2012** из локального экземпляра SQL Server в управляемый экземпляр Базы данных SQL Azure с помощью Azure Database Migration Service.

Из этого руководства вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> - создание экземпляра Azure Database Migration Service;
> - создание проекта миграции с помощью Azure Database Migration Service;
> - выполнение миграции.
> - мониторинг миграции.
> - скачивание отчета о миграции.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

В этой статье описан перенос данных из SQL Server в управляемый экземпляр Базы данных SQL Azure в автономном режиме. Чтобы получить сведения о миграции в подключенном режиме, ознакомьтесь с [руководством по переносу баз данных из SQL Server в Управляемый экземпляр Базы данных SQL Azure по сети с помощью службы DMS](tutorial-sql-server-managed-instance-online.md).

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством вам потребуется следующее:

- Создайте виртуальную сеть Azure для Azure Database Migration Service с помощью модели развертывания Azure Resource Manager, которая обеспечивает подключение типа "сеть — сеть" к локальным исходным серверам с помощью [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) или [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Изучите сетевые топологии для переноса в управляемый экземпляр Базы данных Azure SQL с помощью Azure Database Migration Service](https://aka.ms/dmsnetworkformi). Дополнительные сведения о создании виртуальной сети приведены в [документации по виртуальным сетям](https://docs.microsoft.com/azure/virtual-network/). В частности, уделите внимание кратким руководствам с пошаговыми инструкциями.

    > [!NOTE]
    > Если вы используете ExpressRoute с пиринговой связью с сетью корпорации Майкрософт, во время настройки виртуальной сети добавьте в подсеть, в которой будет подготовлена служба, следующие [конечные точки службы](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview):
    > - целевую конечную точку базы данных (например, конечная точка SQL, конечная точка Cosmos DB и т. д.);
    > - конечную точку службы хранилища;
    > - конечную точку служебной шины.
    >
    > Эта настройка необходима, потому что у Azure Database Migration Service нет подключения к Интернету.

- Убедитесь, что правила группы безопасности сети для виртуальной сети не блокируют следующие входящие порты для Azure Database Migration Service: 443, 53, 9354, 445, 12000. Дополнительные сведения о фильтрации трафика, предназначенного для виртуальной сети Azure, с помощью NSG см. в статье [Plan virtual networks](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (Планирование виртуальных сетей).
- Настройте [брандмауэр Windows для доступа к ядру исходной СУБД](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Откройте брандмауэр Windows, чтобы предоставить Azure Database Migration Service доступ к исходному серверу SQL Server. По умолчанию это TCP-порт 1433.
- Если вы запустили несколько именованных экземпляров SQL Server, использующих динамические порты, вы можете включить службу обозревателя SQL и разрешить доступ к UDP-порту 1434 через брандмауэры. Это позволит службе Azure Database Migration Service подключиться к именованному экземпляру на исходном сервере.
- Если перед исходными базами данных используется брандмауэр, его правила должны разрешать службе Azure Database Migration Service доступ к исходным базам данных для миграции и файлам SMB через порт 445.
- Создайте управляемый экземпляр Базы данных SQL Azure на портале Azure, следуя инструкциям из [этой статьи](https://aka.ms/sqldbmi).
- Убедитесь, что для подключения к исходному серверу SQL Server и целевому управляемому экземпляру используются имена, входящие в серверную роль sysadmin.
- Создайте общую сетевую папку, которую Azure Database Migration Service сможет использовать для резервного копирования базы данных-источника.
- Предоставьте учетной записи службы, от имени которой выполняется исходный экземпляр SQL Server, права на запись в эту созданную сетевую папку, а учетной записи компьютера для исходного сервера — права на чтение и запись для этой папки.
- Запишите имя пользователя и пароль учетной записи Windows, которой предоставлены полные права доступа к этой сетевой папке. Служба Azure Database Migration Service олицетворяет пользователя с этими учетными данными, чтобы отправить файлы резервных копий в контейнер службы хранилища Azure для операции восстановления.
- Создайте контейнер больших двоичных объектов и получите его URI SAS, следуя инструкциям, приведенным в разделе [Получение SAS для контейнера больших двоичных объектов](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container), и не забудьте установить все разрешения (чтение, запись, удаление, перечисление) в окне политик при создании URI SAS. Это позволяет Azure Database Migration Service с помощью контейнера учетной записи хранения выполнять отправку резервных копий, которые используются для переноса баз данных в управляемый экземпляр Базы данных SQL Azure.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Регистрация поставщика ресурсов Microsoft.DataMigration

1. Войдите на портал Azure, щелкните **Все службы** и выберите **Подписки**.

    ![Отображение подписок на портале](media/tutorial-sql-server-to-managed-instance/portal-select-subscriptions.png)        

2. Выберите подписку, в которой нужно создать экземпляр Azure Database Migration Service, а затем щелкните **Поставщики ресурсов**.

    ![Отображение поставщиков ресурсов](media/tutorial-sql-server-to-managed-instance/portal-select-resource-provider.png)

3. В поле поиска введите migration, а затем справа от **Microsoft.DataMigration** щелкните **Зарегистрировать**.

    ![Регистрация поставщика ресурсов](media/tutorial-sql-server-to-managed-instance/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Создание экземпляра Azure Database Migration Service

1. На портале Azure выберите + **Создать ресурс**, введите в поле поиска **Azure Database Migration Service**, а затем в раскрывающемся списке выберите **Azure Database Migration Service**.

     ![Azure Marketplace](media/tutorial-sql-server-to-managed-instance/portal-marketplace.png)

2. На экране **Azure Database Migration Service** выберите **Создать**.

    ![Создание экземпляра Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance/dms-create1.png)

3. На экране **Создание службы миграции** укажите имя службы, подписку и новую или существующую группу ресурсов.

4. Выберите расположение, в котором хотите создать экземпляр DMS.

5. Создайте виртуальную сеть или выберите имеющуюся.

    Виртуальная сеть предоставляет Azure Database Migration Service доступ к исходному экземпляру SQL Server и целевому управляемому экземпляру Базы данных SQL Azure.

    Дополнительные сведения о создании виртуальной сети на портале Azure см. в статье [Создание виртуальной сети с помощью портала Azure](https://aka.ms/DMSVnet).

    Подробные сведения см. в статье [Сетевые топологии для переноса Управляемого экземпляра Базы данных Azure SQL с помощью Azure Database Migration Service](https://aka.ms/dmsnetworkformi).

6. Выберите ценовую категорию.

    Дополнительные сведения о ценовых категориях и затратах см. на [странице с описанием цен](https://aka.ms/dms-pricing).

    ![Создание службы DMS](media/tutorial-sql-server-to-managed-instance/dms-create-service2.png)

7. Выберите **Создать**, чтобы создать службу.

## <a name="create-a-migration-project"></a>Создание проекта миграции

После создания экземпляра службы найдите его на портале Azure, откройте и создайте проект миграции.

1. На портале Azure щелкните **Все службы**, выполните поиск по запросу "Azure Database Migration Service" и выберите **Azure Database Migration Services** (Службы Azure Database Migration Service).

    ![Поиск всех экземпляров Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance/dms-search.png)

2. На экране **Служба миграции баз данных Azure** найдите имя созданного экземпляра и выберите его.

3. Выберите **+ Новый проект миграции**.

4. На экране **New migration project** (Новый проект миграции) задайте имя для проекта, в текстовом поле **Source server type** (Тип исходного сервера) выберите **SQL Server**, в текстовом поле **Target server type** (Тип целевого сервера) выберите **Управляемый экземпляр Базы данных SQL Azure**, а затем в разделе **Выберите тип действия** нажмите кнопку **Автономная миграция данных**.

   ![Создание проекта DMS](media/tutorial-sql-server-to-managed-instance/dms-create-project2.png)

5. Выберите **Создать**, чтобы создать проект.

## <a name="specify-source-details"></a>Указание сведений об источнике

1. На экране **Migration source detail** (Сведения об источнике миграции) задайте сведения о подключении для исходного SQL Server.

2. Если на сервере не установлен доверенный сертификат, установите флажок **Доверять сертификату сервера**.

    Если доверенный сертификат не установлен, SQL Server создаст самозаверяющий сертификат при запуске экземпляра. Этот сертификат используется с целью шифрования учетных данных для клиентских подключений.

    > [!CAUTION]
    > SSL-соединения, шифруемые с помощью самозаверяющего сертификата, не обеспечивают надежной защиты. Они уязвимы для атак "злоумышленник в середине". В рабочей среде или на серверах, подключенных к Интернету, не следует применять самозаверяющие сертификаты для SSL.

   ![Сведения об источнике](media/tutorial-sql-server-to-managed-instance/dms-source-details1.png)

3. Щелкните **Сохранить**.

4. На экране **Выбор баз данных-источников** выберите базу данных **Adventureworks2012** для миграции.

   ![Выбор баз данных-источников](media/tutorial-sql-server-to-managed-instance/dms-source-database1.png)

    > [!IMPORTANT]
    > При использовании MSSQL Integration Services (SSIS), DMS не поддерживает перенос базы данных каталога для проектов или пакетов служб SSIS (SSISDB) из SQL Server в управляемый экземпляр Базы данных SQL Azure. Но можно подготовить SSIS в Фабрике данных Azure (ADF) и повторно развернуть проекты или пакеты служб SQL Server Integration Services в целевой базе данных SQL Server Integration Services, размещенной в управляемом экземпляре Базы данных SQL Azure. Дополнительные сведения о миграции пакетов SSIS см. в статье [Перенос пакетов SQL Server Integration Services в Azure](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

5. Щелкните **Сохранить**.

## <a name="specify-target-details"></a>Указание сведений о цели

1. На экране **Migration target details** (Сведения о целевом объекте переноса) задайте сведения о подключении для целевого объекта. Целевым объектом выступает предварительно подготовленный управляемый экземпляр Базы данных SQL Azure, в который нужно перенести базу данных **AdventureWorks2012**.

    Если управляемый экземпляр Базы данных SQL Azure еще не подготовлен, щелкните [эту ссылку](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started), чтобы подготовить его. Вы можете продолжить создание проекта, а затем, когда управляемый экземпляр Базы данных SQL Azure будет готов, вернуться к этому проекту, чтобы выполнить миграцию.

     ![Выбор цели](media/tutorial-sql-server-to-managed-instance/dms-target-details2.png)

2. Щелкните **Сохранить**.

## <a name="select-source-databases"></a>Выбор баз данных-источников

1. На экране **Выбор баз данных-источников** выберите базу данных-источник для миграции.

    ![Выбор баз данных-источников](media/tutorial-sql-server-to-managed-instance/select-source-databases.png)

2. Щелкните **Сохранить**.

## <a name="select-logins"></a>Выберите имена для входа

1. Выберите имена для входа, которые хотите перенести, на экране **Select logins** (Выбор имени входа).

    >[!NOTE]
    >В этом выпуске поддерживается только перенос имен для входа SQL.

    ![Выберите имена для входа](media/tutorial-sql-server-to-managed-instance/select-logins.png)

2. Щелкните **Сохранить**.

## <a name="configure-migration-settings"></a>Настройка параметров миграции

1. На экране **Configure migration settings** (Настройка параметров миграции) укажите следующие сведения:

    | | |
    |--------|---------|
    |**Выбор исходного архива** | Выберите вариант **I will provide latest backup files** (Я предоставлю файлы последних резервных копий), если у вас есть файлы полных резервных копий, которые DMS может использовать для переноса базы данных. Выберите вариант **I will let Azure Database Migration Service create backup files** (Я разрешу Azure Database Migration Service создавать файлы резервных копий), если нужно, чтобы служба DMS сначала создала полную резервную копию базы данных-источника и использовала ее для миграции. |
    |**Общая сетевая папка** | Локальная сетевая папка SMB, в которую Azure Database Migration Service может поместить резервные копии базы данных-источника. Учетная запись службы, в которой выполняется исходный экземпляр SQL Server, должна иметь права записи для этой сетевой папки. Укажите полное доменное имя или IP-адрес сервера и сетевую папку, например "\\\servername.domainname.com\backupfolder" или "\\\IP address\backupfolder".|
    |**Имя пользователя** | Убедитесь, что пользователь Windows имеет полное право доступа к указанной ранее сетевой папке. Служба Azure Database Migration Service будет олицетворять пользователя с этими учетными данными, чтобы отправить файлы резервных копий в контейнер службы хранилища Azure для операции восстановления. В случае выбора для миграции баз данных с поддержкой TDE указанный ранее пользователь Windows должен иметь встроенную учетную запись администратора, а режим [Контроль учетных записей](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/user-account-control-overview) должен быть отключен для Azure Database Migration Service, чтобы отправлять и удалять файлы сертификатов. |
    |**Пароль** | Пароль для пользователя |
    |**Параметры учетной записи хранения** | Код URI SAS, который позволяет Azure Database Migration Service обращаться к контейнеру учетной записи хранения для отправки файлов резервных копий, используемых для переноса базы данных в управляемый экземпляр Базы данных SQL Azure. [Получение SAS для контейнера больших двоичных объектов](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    |**Параметры TDE** | Чтобы перенести базы данных-источники с активированным прозрачным шифрованием данных (TDE), необходимо иметь права на запись в целевой управляемый экземпляр Базы данных SQL Azure.  В раскрывающемся меню выберите подписку, в которой подготовлен управляемый экземпляр Базы данных SQL Azure.  В раскрывающемся меню выберите целевой **Управляемый экземпляр Базы данных SQL Azure**. |

    ![Настройка параметров миграции](media/tutorial-sql-server-to-managed-instance/dms-configure-migration-settings3.png)

2. Щелкните **Сохранить**.

## <a name="review-the-migration-summary"></a>Просмотр сводки по миграции

1. На экране **Сводка по миграции** в текстовом поле **Имя активности** задайте имя действия миграции.

2. Разверните раздел **Вариант проверки**, чтобы открыть экран **Выбор варианта проверки**, укажите, нужно ли проверять переносимую базу данных на правильность запроса, а затем щелкните **Сохранить**.

3. Проверьте и подтвердите сведения, связанные с проектом миграции.

    ![Сводка по проекту миграции](media/tutorial-sql-server-to-managed-instance/dms-project-summary2.png)

4. Щелкните **Сохранить**.

## <a name="run-the-migration"></a>Выполнение миграции

- Выберите **Запустить миграцию**.

  Появится окно действия миграции, в котором будет указано состояние действия **Ожидание**.

## <a name="monitor-the-migration"></a>Мониторинг миграции

1. Для обновления отображающихся данных нажмите кнопку **Обновить** на экране действия миграции.

   ![Выполняется действие миграции](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration1.png)

    Далее можно развернуть категории баз данных и имен для входа, чтобы отслеживать состояние переноса соответствующих объектов сервера.

   ![Выполняется действие миграции](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration-extend.png)

2. После завершения миграции нажмите **Скачать отчет**, чтобы получить отчет с подробными сведениями о процессе миграции.

3. Проверьте конечную базу данных в конечной среде управляемого экземпляра Базы данных SQL Azure.

## <a name="next-steps"></a>Дополнительная информация

- Дополнительные сведения о переносе базы данных в управляемый экземпляр с помощью команды T-SQL RESTORE см. в [этой статье](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Сведения об управляемом экземпляре см. в статье [Использование расширенной защиты данных Базы данных SQL с виртуальными сетями и почти полной совместимостью](../sql-database/sql-database-managed-instance.md).
- Сведения о подключении приложений к управляемому экземпляру см. в статье [Подключение приложения к Управляемому экземпляру Базы данных SQL](../sql-database/sql-database-managed-instance-connect-app.md).
