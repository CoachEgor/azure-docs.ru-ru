---
title: Руководство. Миграция MongoDB в Интернет в интерфейс API Azure Cosmos DB для MongoDB
titleSuffix: Azure Database Migration Service
description: Узнайте, как выполнить миграцию из локальной MongoDB в Azure Cosmos DB API для MongoDB Online с помощью Azure Database Migration Service.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 66375d83dca4edef17919e3b493d5e45be37cc40
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "78255616"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-online-using-dms"></a>Руководство. Миграция MongoDB в API Azure Cosmos DB для MongoDB Online с помощью DMS

С помощью Azure Database Migration Service можно оперативно переносить базы данных (с минимальным простоем) из локального или облачного экземпляра MongoDB в API Azure Cosmos DB для MongoDB.

В этом руководстве вы узнаете, как:
> [!div class="checklist"]
>
> * создание экземпляра Azure Database Migration Service;
> * создание проекта миграции с использованием Azure Database Migration Service.
> * выполнение миграции.
> * мониторинг миграции.
> * Когда будете готовы, выполните миграцию.

В этом руководстве выполняется перенос набора данных из базы данных MongoDB, размещенной на виртуальной машине Azure, в API Azure Cosmos DB для MongoDB с минимальным простоем с помощью Azure Database Migration Service. Если у вас еще не настроен источник MongoDB, обратитесь к статье [Установка и настройка базы данных MongoDB на виртуальной машине Windows в Azure](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb).

> [!NOTE]
> Чтобы выполнить сетевую миграцию с помощью Azure Database Migration Service, требуется создать экземпляр ценовой категории "Премиум".

> [!IMPORTANT]
> Чтобы процесс миграции был выполнен без проблем, Майкрософт рекомендует создать экземпляр Azure Database Migration Service в том же регионе Azure, в котором размещена целевая база данных. Перемещение данных между регионами и географическими областями может замедлить миграцию.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

В этой статье описан перенос баз данных по сети из MongoDB в API Azure Cosmos DB для MongoDB. Чтобы узнать, как выполнить миграцию в автономном режиме, см. [руководство по миграции MongoDB в API Azure Cosmos DB для MongoDB в автономном режиме с помощью DMS](tutorial-mongodb-cosmos-db.md).

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством вам потребуется следующее:

* [Завершите шаги, которые необходимо выполнить перед миграцией](../cosmos-db/mongodb-pre-migration.md), включая оценку пропускной способности, а также выбор ключа секции и политики индексирования.
* [Создайте приложение API Azure Cosmos DB для учетной записи MongoDB](https://ms.portal.azure.com/#create/Microsoft.DocumentDB).
* Создайте виртуальная сеть Microsoft Azure для Azure Database Migration Service с помощью модели развертывания Azure Resource Manager, которая обеспечивает подключение типа "сеть — сеть" к локальным исходным серверам с помощью [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) или [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

    > [!NOTE]
    > При установке виртуальной сети с помощью ExpressRoute с пирингом сети в корпорацию Майкрософт добавьте следующие [конечные точки](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) службы в подсеть, в которой будет подготовлена служба:
    >
    > * целевую конечную точку базы данных (например, конечная точка SQL, конечная точка Cosmos DB и т. д.);
    > * конечную точку службы хранилища;
    > * конечную точку служебной шины.
    >
    > Такая конфигурация вызвана тем, что у Azure Database Migration Service нет подключения к Интернету.

* Убедитесь, что правила группы безопасности сети виртуальной сети (NSG) не блокируют следующие порты связи: 53, 443, 445, 9354 и 10000-20000. Дополнительные сведения о фильтрации трафика NSG в виртуальной сети см. в статье [Фильтрация сетевого трафика с помощью групп безопасности сети](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Откройте брандмауэр Windows, чтобы предоставить Azure Database Migration Service доступ к исходному серверу MongoDB. По умолчанию это TCP-порт 27017.
* Если перед исходными базами данных развернуто устройство брандмауэра, вам может понадобиться добавить правила брандмауэра, чтобы позволить службе Azure Database Migration Service обращаться к исходным базам данных для выполнения миграции.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Регистрация поставщика ресурсов Microsoft.DataMigration

1. Войдите на портал Azure, щелкните **Все службы** и выберите **Подписки**.

   ![Отображение подписок на портале](media/tutorial-mongodb-to-cosmosdb-online/portal-select-subscription1.png)

2. Выберите подписку, в которой нужно создать экземпляр Azure Database Migration Service, а затем щелкните **Поставщики ресурсов**.

    ![Отображение поставщиков ресурсов](media/tutorial-mongodb-to-cosmosdb-online/portal-select-resource-provider.png)

3. В поле поиска введите migration, а затем справа от **Microsoft.DataMigration** щелкните **Зарегистрировать**.

    ![Регистрация поставщика ресурсов](media/tutorial-mongodb-to-cosmosdb-online/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Создание экземпляра

1. На портале Azure выберите **+Создать ресурс**, введите в поле поиска "Azure Database Migration Service", а затем в раскрывающемся списке выберите **Azure Database Migration Service**.

    ![Azure Marketplace](media/tutorial-mongodb-to-cosmosdb-online/portal-marketplace.png)

2. На экране **Azure Database Migration Service** выберите **Создать**.

    ![Создание экземпляра Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb-online/dms-create1.png)
  
3. На экране **Создание службы миграции** укажите имя службы, подписку и новую или существующую группу ресурсов.

4. Выберите расположение, в котором нужно создать экземпляр Azure Database Migration Service.

5. Выберите существующую виртуальную сеть или создайте новую.

   Виртуальная сеть предоставляет Azure Database Migration Service с доступом к исходному экземпляру MongoDB и учетной записи целевой Azure Cosmos DB.

   Дополнительные сведения о создании виртуальной сети в портал Azure см. в статье [Создание виртуальной сети с помощью портал Azure](https://aka.ms/DMSVnet).

6. Выберите номер SKU ценовой категории "Премиум".

    > [!NOTE]
    > Миграция по сети поддерживается, только если используется уровень "Премиум". Дополнительные сведения о затратах и ценовых категориях см. на [странице с ценами](https://aka.ms/dms-pricing).

    ![Настройка параметров экземпляра Database Migration Service](media/tutorial-mongodb-to-cosmosdb-online/dms-settings3.png)

7. Выберите **Создать**, чтобы создать службу.

## <a name="create-a-migration-project"></a>Создание проекта миграции

После создания службы найдите ее на портале Azure, откройте и создайте проект миграции.

1. На портале Azure щелкните **Все службы**, выполните поиск по запросу "Azure Database Migration Service" и выберите **Azure Database Migration Services** (Службы Azure Database Migration Service).

    ![Поиск всех экземпляров Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb-online/dms-search.png)

2. На экране **Службы миграции баз данных Azure** найдите и выберите имя созданного экземпляра Azure Database Migration Service.

    Вы можете также обнаружить экземпляр службы Azure Database Migration Service в области поиска на портале Azure.

    ![Использование панели поиска на портале Azure](media/tutorial-mongodb-to-cosmosdb-online/dms-search-portal.png)

3. Выберите **+ Новый проект миграции**.

4. На экране **New migration project** (Создать проект миграции) задайте имя проекта, в текстовом поле **Source server type** (Тип исходного сервера) выберите **MongoDB**, в текстовом поле **Target server type** (Тип целевого сервера) выберите **Cosmos DB (API MongoDB)**, а затем для параметра **Выберите тип действия** выберите значение **Online data migration [preview]** (Перенос данных по сети [предварительная версия]).

    ![Создание проекта Database Migration Service](media/tutorial-mongodb-to-cosmosdb-online/dms-create-project1.png)

5. Выберите **Сохранить** и **Создать и выполнить действие**, чтобы создать проект и выполнить действие миграции.

## <a name="specify-source-details"></a>Указание сведений об источнике

1. На экране **Сведения об источнике** задайте сведения о подключении для исходного сервера MongoDB.

   > [!IMPORTANT]
   > Azure Database Migration Service не поддерживает Azure Cosmos DB в качестве источника.

    Доступно три режима для подключения к источнику:
   * **Стандартный режим**, в котором принимается полное доменное имя или IP-адрес, номер порта и учетные данные для подключения.
   * **Режим строки подключения**, в котором принимается строка подключения MongoDB, как описано в статье о [формате URI строки подключения](https://docs.mongodb.com/manual/reference/connection-string/).
   * **Данные из службы хранилища Azure**, в котором принимается URL-адрес SAS контейнера BLOB-объектов. Выберите **BLOB-объект содержит дампы BSON**, если в контейнере BLOB-объектов есть дампы BSON, созданные [средством bsondump](https://docs.mongodb.com/manual/reference/program/bsondump/) MongoDB, и отмените выбор, если контейнер содержит файлы JSON.

     Если вы выбрали этот параметр, убедитесь, что строка подключения к учетной записи хранения выглядит следующим образом:

     ```
     https://blobnameurl/container?SASKEY
     ```

     Кроме того, на основе данных дампа типа в службе хранилища Azure учитывайте следующие сведения.

     * Для дампов BSON данные в контейнере больших двоичных объектов должны быть в формате bsondump так, чтобы файлы данных помещались в папках с именами содержащих баз данных в формате "коллекция.bson". Файлы метаданных (если таковые имеются) должны быть названы в формате *коллекция*.metadata.json.

     * Для дампов JSON файлы в контейнере больших двоичных объектов должны размещаться в папках с именами содержащих баз данных. В каждой папке баз данных файлы данных должны быть помещены в подпапку с именем "data" и названы в формате *коллекция*.json. Файлы метаданных (если таковые имеются) должны быть помещены в подпапку с именем "metadata" и названы в том же формате: *коллекция*.json. Файлы метаданных должны быть в том же формате, что и файлы, созданные инструментом bsondump MongoDB.

    > [!IMPORTANT]
    > Не рекомендуется использовать самозаверяющий сертификат на сервере Mongo. Но если он все же применяется, подключитесь к серверу в **режиме строки подключения** и убедитесь, что в строке подключения используются двойные кавычки.
    >
    >```
    >&sslVerifyCertificate=false
    >```

    Если разрешение DNS-имен невозможно, можно использовать IP-адрес.

   ![Указание сведений об источнике](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-source1.png)

2. Нажмите кнопку **Сохранить**.

   > [!NOTE]
   > Адрес исходного сервера должен быть адресом основного сервера, если источником является набор реплик, и маршрутизатором, если источником является сегментированный кластер MongoDB. Если используется сегментированный кластер MongoDB, служба Azure Database Migration Service также должна иметь возможность подключения к отдельным сегментам в кластере. Для этого, возможно, потребуется открыть брандмауэр на нескольких компьютерах.

## <a name="specify-target-details"></a>Указание сведений о цели

1. На экране **Migration target details** (Сведения о целевом объекте переноса) укажите сведения о подключении для целевой учетной записи Azure Cosmos DB. Это предварительно подготовленная учетная запись API Azure Cosmos DB для MongoDB, в которую нужно перенести данные MongoDB.

    ![Указание сведений о цели](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-target1.png)

2. Нажмите кнопку **Сохранить**.

## <a name="map-to-target-databases"></a>Сопоставление с целевыми базами данных

1. На экране **Map to target databases** (Сопоставить с целевыми базами данных) сопоставьте исходную и целевую базы данных для миграции.

   Если в целевой базе данных содержится база данных с тем же именем, что у исходной базы данных, Azure Database Migration Service по умолчанию выберет целевую базу данных.

   Если строка **Создать** отображается рядом с именем базы данных, это значит, что Azure Database Migration Service не удалось найти целевую базу данных, и служба автоматически создаст ее.

   На этом этапе переноса укажите пропускную способность в ЕЗ, если для базы данных необходимо использовать общую пропускную способность. В Cosmos DB пропускную способность можно подготовить на уровне базы данных или индивидуально для каждой коллекции. Пропускная способность измеряется в [единицах запроса](https://docs.microsoft.com/azure/cosmos-db/request-units) (ЕЗ). Дополнительные сведения о ценах Azure Cosmos DB см. [здесь](https://azure.microsoft.com/pricing/details/cosmos-db/).

   ![Сопоставление с целевыми базами данных](media/tutorial-mongodb-to-cosmosdb-online/dms-map-target-databases1.png)

2. Нажмите кнопку **Сохранить**.

3. На экране **Параметр коллекции** разверните список коллекций и просмотрите коллекции, которые будут перенесены.

   Azure Database Migration Service автоматически выбирает все коллекции, которые существуют в исходном экземпляре MongoDB и не существуют в целевой учетной записи Azure Cosmos DB. Чтобы повторно перенести коллекции, уже содержащие данные, необходимо явным образом выбрать их на этом экране.

   Вы можете указать количество ЕЗ, которое необходимо использовать коллекциям. Обычно достаточно использовать значение в диапазоне от 500 (не менее 1000 для сегментированных коллекций) до 4000. Azure Database Migration Service предлагает интеллектуальные значения по умолчанию с учетом размера коллекции.

    > [!NOTE]
    > При необходимости выполните перенос базы данных и коллекции в параллельном режиме с помощью нескольких экземпляров Azure Database Migration Service, чтобы ускорить процесс.

   Вы можете также указать ключ сегмента, чтобы воспользоваться преимуществами [секционирования в Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) для обеспечения наилучшей масштабируемости. Обязательно просмотрите [рекомендации по выбору ключа сегмента и секции](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey). Если у вас нет ключа секции, в качестве ключа сегмента вы всегда можете использовать **_id**, чтобы улучшить пропускную способность.

   ![Выбор таблиц коллекций](media/tutorial-mongodb-to-cosmosdb-online/dms-collection-setting1.png)

4. Нажмите кнопку **Сохранить**.

5. На экране **Сводка по миграции** в текстовом поле **Имя активности** задайте имя действия миграции.

    ![Сводка по миграции](media/tutorial-mongodb-to-cosmosdb-online/dms-migration-summary1.png)

## <a name="run-the-migration"></a>Выполнение миграции

* Выберите **Запустить миграцию**.

   Появится окно действия миграции, в котором будет указано **состояние** действия.

   ![Состояние действия](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Мониторинг миграции

* На экране действия миграции нажимайте **Обновить**, чтобы периодически обновлять содержимое экрана, пока в поле **Состояние** не будет указано значение **Идет повторное воспроизведение**.

   > [!NOTE]
   > Вы можете выбрать действие, чтобы получить сведения о метриках миграции на уровне базы данных и на уровне коллекции.

   ![Повторное воспроизведение состояния действия](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-replaying.png)

## <a name="verify-data-in-cosmos-db"></a>Проверка данных в Cosmos DB

1. Внесите изменения в исходную базу данных MongoDB.
2. Подключитесь к COSMOS DB, чтобы убедиться, что данные реплицированы с исходного сервера MongoDB.

    ![Повторное воспроизведение состояния действия](media/tutorial-mongodb-to-cosmosdb-online/dms-verify-data.png)

## <a name="complete-the-migration"></a>Выполнение миграции

* Когда все документы с исходного сервера станут доступны на целевом сервере COSMOS DB, выберите **Готово** в контекстном меню действия миграции, чтобы завершить ее.

    Это действие приведет к окончанию повторного воспроизведения всех ожидающих изменений и завершению миграции.

    ![Повторное воспроизведение состояния действия](media/tutorial-mongodb-to-cosmosdb-online/dms-finish-migration.png)

## <a name="post-migration-optimization"></a>Оптимизация после переноса

Для управления данными, перенесенными из базы данных MongoDB в API Azure Cosmos DB для MongoDB, можно подключиться к Azure Cosmos DB. После переноса можно также выполнить другие действия, включая оптимизацию политики индексирования, обновление уровня согласованности по умолчанию и настройку глобального распределения для своей учетной записи Azure Cosmos DB. См. подробнее об [оптимизации после переноса](../cosmos-db/mongodb-post-migration.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Сведения о службе Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Дальнейшие шаги

* Просмотрите другие сценарии в [руководстве по миграции базы данных Майкрософт](https://datamigration.microsoft.com/).
