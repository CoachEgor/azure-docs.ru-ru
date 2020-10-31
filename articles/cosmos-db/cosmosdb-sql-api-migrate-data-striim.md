---
title: Перенос данных в учетную запись Azure Cosmos DB API SQL с помощью Стриим
description: Узнайте, как использовать Стриим для переноса данных из базы данных Oracle в учетную запись Azure Cosmos DB API SQL.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: how-to
ms.date: 07/22/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: a77f039ea266e10130b6460855f989ab317a20ba
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93097674"
---
# <a name="migrate-data-to-azure-cosmos-db-sql-api-account-using-striim"></a>Перенос данных в учетную запись Azure Cosmos DB API SQL с помощью Стриим
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]
 
Образ Стриим в Azure Marketplace обеспечивает непрерывное перемещение данных из хранилищ и баз данных в Azure в режиме реального времени. При перемещении данных можно выполнить встроенную денормализацию, преобразование данных, включить аналитику в реальном времени и сценарии создания отчетов с данными. Вы можете легко приступить к работе с Стриим, чтобы постоянно перемещать корпоративные данные в Azure Cosmos DB API SQL. Azure предоставляет предложение Marketplace, которое упрощает развертывание Стриим и перенос данных в Azure Cosmos DB. 

В этой статье показано, как использовать Стриим для переноса данных из **базы данных Oracle** в **учетную запись Azure Cosmos DB API SQL** .

## <a name="prerequisites"></a>Предварительные требования

* Если у вас еще нет [подписки Azure](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), прежде чем начать работу.

* База данных Oracle, работающая в локальной среде, с некоторыми данными.

## <a name="deploy-the-striim-marketplace-solution"></a>Развертывание решения Стриим Marketplace

1. Войдите на [портал Azure](https://portal.azure.com/).

1. Выберите **создать ресурс** и выполните поиск по запросу **Стриим** в Azure Marketplace. Выберите первый вариант и **Создайте** .

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/striim-azure-marketplace.png" alt-text="Найти элемент Стриим Marketplace":::

1. Затем введите свойства конфигурации для экземпляра Стриим. Среда Стриим развертывается на виртуальной машине. В области **Основные сведения** введите **имя пользователя виртуальной машины** , **пароль виртуальной** машины (этот пароль используется для подключения к виртуальной машине по протоколу SSH). Выберите **подписку** , **группу ресурсов** и **сведения о расположении** , где вы хотите развернуть Стриим. После завершения нажмите кнопку **ОК** .

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/striim-configure-basic-settings.png" alt-text="Найти элемент Стриим Marketplace" по умолчанию.  | 
   | Имя кластера Стриим|    <Striim_cluster_Name>|  Имя кластера Стриим.|
   | Пароль кластера Стриим|   <Striim_cluster_password>|  Пароль для кластера.|

   После заполнения формы нажмите кнопку **ОК** , чтобы продолжить.

1. В области **параметры доступа Стриим** настройте **общедоступный IP-адрес** (выберите значения по умолчанию), **доменное имя для Стриим** , **пароль администратора** , который вы хотите использовать для входа в пользовательский интерфейс Стриим. Настройте ВИРТУАЛЬную сеть и подсети (выберите значения по умолчанию). После заполнения сведений нажмите кнопку **ОК** , чтобы продолжить.

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/striim-access-settings.png" alt-text="Найти элемент Стриим Marketplace":::

1. Azure будет проверять развертывание и убедиться в том, что все выглядит хорошо. выполнение проверки занимает несколько минут. После завершения проверки нажмите кнопку **ОК** .
  
1. Наконец, ознакомьтесь с условиями использования и выберите **создать** , чтобы создать экземпляр Стриим. 

## <a name="configure-the-source-database"></a>Настройка базы данных источника 

В этом разделе вы настроите базу данных Oracle в качестве источника для перемещения данных.  Для подключения к Oracle требуется [драйвер Oracle JDBC](https://www.oracle.com/technetwork/database/features/jdbc/jdbc-ucp-122-3110062.html) . Чтобы считать изменения из исходной базы данных Oracle, можно использовать API [поступающие](https://www.oracle.com/technetwork/database/features/availability/logmineroverview-088844.html) или [ксстреам](https://docs.oracle.com/cd/E11882_01/server.112/e16545/xstrm_intro.htm#XSTRM72647). Для чтения, записи или сохранения данных из базы данных Oracle драйвер для Oracle JDBC должен присутствовать в подкаталогах Java Стриим.

Скачайте драйвер [ojdbc8. jar](https://www.oracle.com/technetwork/database/features/jdbc/jdbc-ucp-122-3110062.html) на локальный компьютер. Он будет установлен в кластере Стриим позже.

## <a name="configure-the-target-database"></a>Настройка целевой базы данных

В этом разделе вы настроите учетную запись Azure Cosmos DB SQL API в качестве целевой для перемещения данных.

1. Создайте [учетную запись Azure Cosmos DB API SQL](create-cosmosdb-resources-portal.md) с помощью портал Azure.

1. Перейдите в область **Обозреватель данных** в учетной записи Azure Cosmos. Выберите **создать контейнер** , чтобы создать новый контейнер. Предположим, что выполняется перенос *продуктов* и данных *заказов* из базы данных Oracle в Azure Cosmos DB. Создайте новую базу данных с именем **стриимдемо** и контейнером с именем **Orders** . Подготавливаете контейнер с помощью **1000 RUS** (в этом примере используется 1000 RUS, но для рабочей нагрузки следует использовать оценку пропускной способности) и **/ORDER_ID** в качестве ключа секции. Эти значения будут различаться в зависимости от исходных данных. 

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/create-sql-api-account.png" alt-text="Найти элемент Стриим Marketplace":::

## <a name="configure-oracle-to-azure-cosmos-db-data-flow"></a>Настройка Oracle для Azure Cosmos DB потока данных

1. Теперь вернемся к Стриим. Прежде чем взаимодействовать с Стриим, установите драйвер Oracle JDBC, скачанный ранее.

1. Перейдите к экземпляру Стриим, развернутому в портал Azure. Нажмите кнопку **подключить** в верхней строке меню и на вкладке **SSH** скопируйте URL-адрес в поле **Вход с помощью локальной учетной записи виртуальной машины** .

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/get-ssh-url.png" alt-text="Найти элемент Стриим Marketplace":::

1. Откройте новое окно терминала и выполните команду SSH, скопированную из портал Azure. В этой статье используется терминал в MacOS. Вы можете выполнить аналогичные инструкции с помощью выводимых инструкций или другого клиента SSH на компьютере Windows. При появлении запроса введите **Да** , чтобы продолжить, и введите **пароль** , заданный для виртуальной машины на предыдущем шаге.

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/striim-vm-connect.png" alt-text="Найти элемент Стриим Marketplace":::

1. Теперь откройте новую вкладку терминала, чтобы скопировать ранее загруженный файл **ojdbc8. jar** . Используйте следующую команду SCP для копирования JAR-файла с локального компьютера в папку tmp экземпляра Стриим, работающего в Azure:

   ```bash
   cd <Directory_path_where_the_Jar_file_exists> 
   scp ojdbc8.jar striimdemo@striimdemo.westus.cloudapp.azure.com:/tmp
   ```

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/copy-jar-file.png" alt-text="Найти элемент Стриим Marketplace":::

1. Затем вернитесь к окну, где вы выполнили подключение SSH к экземпляру Стриим и выполните вход как sudo. Переместите файл **ojdbc8. jar** из каталога **/tmp** в каталог **lib** экземпляра Стриим с помощью следующих команд:

   ```bash
   sudo su
   cd /tmp
   mv ojdbc8.jar /opt/striim/lib
   chmod +x ojdbc8.jar
   ```

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/move-jar-file.png" alt-text="Найти элемент Стриим Marketplace":::


1. В том же окне терминала перезапустите сервер Стриим, выполнив следующие команды:

   ```bash
   Systemctl stop striim-node
   Systemctl stop striim-dbms
   Systemctl start striim-dbms
   Systemctl start striim-node
   ```
 
1. Запуск Стриим займет около минуты. Если вы хотите просмотреть состояние, выполните следующую команду: 

   ```bash
   tail -f /opt/striim/logs/striim-node.log
   ```

1. Теперь вернитесь в Azure и скопируйте общедоступный IP-адрес виртуальной машины Стриим. 

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/copy-public-ip-address.png" alt-text="Найти элемент Стриим Marketplace":::

1. Чтобы перейти к веб-ИНТЕРФЕЙСу Стриим, откройте новую вкладку в браузере и скопируйте общедоступный IP-адрес, за которым следует: 9080. Войдите, используя имя **администратора** , а также пароль администратора, указанный в портал Azure.

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/striim-login-ui.png" alt-text="Найти элемент Стриим Marketplace":::

1. Теперь вы приступили к домашней странице Стриим. Существует три различные панели: **панели мониторинга** , **приложения** и **саурцепревиев** . Панель панели мониторинга позволяет перемещать данные в режиме реального времени и визуализировать их. Панель приложения содержит конвейеры потоковых данных или потоки данных. В правой части страницы находится Саурцепревиев, где можно просмотреть данные перед их перемещением.

1. Выберите область **приложения** . Сейчас мы рассмотрим эту панель. Существует множество примеров приложений, которые можно использовать для изучения Стриим, однако в этой статье вы создадите свой собственный. Нажмите кнопку **Добавить приложение** в правом верхнем углу.

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/add-striim-app.png" alt-text="Найти элемент Стриим Marketplace":::

1. Существует несколько различных способов создания Стриим приложений. Выберите **начать с шаблона** , чтобы начать с существующего шаблона.

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/start-with-template.png" alt-text="Найти элемент Стриим Marketplace" и выберите **target: Azure Cosmos DB** а затем выберите **Oracle CDC для Azure Cosmos DB** .

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/oracle-cdc-cosmosdb.png" alt-text="Найти элемент Стриим Marketplace":::

1. На следующей странице Назовите свое приложение. Можно указать имя, например **оратокосмосдб** , а затем нажать кнопку **сохранить** .

1. Затем введите конфигурацию источника исходного экземпляра Oracle. Введите значение для **имени источника** . Имя источника — это просто соглашение об именовании для приложения Стриим. можно использовать нечто вроде **src_onPremOracle** . Введите значения в полях **URL-адрес** исходного параметра, **имя пользователя** , **пароль** и выберите **поступающие** в качестве читателя для чтения данных из Oracle. Нажмите кнопку **Далее** , чтобы продолжить.

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/configure-source-parameters.png" alt-text="Найти элемент Стриим Marketplace":::

1. Стриим проверит вашу среду и убедитесь, что она может подключиться к исходному экземпляру Oracle, иметь правильные права доступа и этот CDC настроен правильно. После проверки всех значений нажмите кнопку **Далее** .

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/validate-source-parameters.png" alt-text="Найти элемент Стриим Marketplace":::

1. Выберите таблицы из базы данных Oracle, которые вы хотите перенести. Например, выберите таблицу Заказы и нажмите кнопку **Далее** . 

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/select-source-tables.png" alt-text="Найти элемент Стриим Marketplace" портал Azure. 

   Нажмите кнопку **сохранить** и **Далее** .

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/configure-target-parameters.png" alt-text="Найти элемент Стриим Marketplace":::


1. Затем вы приступите к конструктору Flow, где можно перетаскивать соединители Box для создания приложений потоковой передачи. В этот момент изменения последовательности не будут внесены. Итак, разверните приложение, нажав кнопку **развернуть приложение** .
 
   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/deploy-app.png" alt-text="Найти элемент Стриим Marketplace":::

1. В окне Развертывание можно указать, следует ли запускать определенные части приложения в определенных частях топологии развертывания. Так как мы выполняем в простой топологии развертывания с помощью Azure, мы будем использовать параметр по умолчанию.

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/deploy-using-default-option.png" alt-text="Найти элемент Стриим Marketplace":::

1. После развертывания можно просмотреть поток для просмотра данных, передаваемых по. Щелкните значок **волна** и проводим рядом с ним. Нажмите кнопку **Развернутая** в верхней строке меню и выберите **запустить приложение** .

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/start-app.png" alt-text="Найти элемент Стриим Marketplace":::

1. С помощью средства чтения **CDC (отслеживания измененных данных)** Стриим выберет только новые изменения в базе данных. При наличии данных, передаваемых через исходные таблицы, вы увидите их. Однако, поскольку это демонстрационная таблица, источник не подключен ни к одному приложению. При использовании образца генератора данных можно вставить цепочку событий в базу данных Oracle.

1. Вы увидите данные, передаваемые через платформу Стриим. Стриим также берет все метаданные, связанные с таблицей, что полезно для отслеживания данных и обеспечения того, что распространители данных должны быть в правильном целевом объекте.

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/configure-cdc-pipeline.png" alt-text="Найти элемент Стриим Marketplace":::

1. Наконец, давайте выполним вход в Azure и перейду к своей учетной записи Azure Cosmos. Обновите обозреватель данных, и вы увидите, что данные получены.  

   :::image type="content" source="./media/cosmosdb-sql-api-migrate-data-striim/portal-validate-results.png" alt-text="Найти элемент Стриим Marketplace":::

С помощью решения Стриим в Azure можно непрерывно переносить данные в Azure Cosmos DB из различных источников, таких как Oracle, Cassandra, MongoDB и других, для Azure Cosmos DB. Дополнительные сведения см. на [веб-сайте Стриим](https://www.striim.com/), [Загрузите бесплатную 30-дневную пробную версию Стриим](https://go2.striim.com/download-free-trial)и в случае возникновения проблем при настройке пути миграции с помощью Стриим отправьте [запрос в службу поддержки.](https://go2.striim.com/request-support-striim)

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о переносе данных в Azure Cosmos DB API SQL см. в статье [Перенос данных в учетную запись API Cassandra с помощью Стриим](cosmosdb-cassandra-api-migrate-data-striim.md) .

* [Мониторинг и отладка данных с помощью метрик Azure Cosmos DB](use-metrics.md)