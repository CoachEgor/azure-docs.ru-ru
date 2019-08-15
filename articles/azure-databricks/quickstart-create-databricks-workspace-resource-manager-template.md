---
title: 'Краткое руководство. Запуск задания Spark в Azure Databricks с использованием шаблона Resource Manager '
description: В этом кратком руководстве объясняется, как с помощью шаблона Azure Resource Manager создать рабочую область Azure Databricks и кластер Apache Spark, а затем запустить задание Spark.
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.custom: mvc
ms.date: 07/12/2019
ms.openlocfilehash: 8d4cfb2be687f258255379d93fd60028dcd2eb2c
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932597"
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-resource-manager-template"></a>Краткое руководство. Запуск задания Spark в Azure Databricks с помощью шаблона Azure Resource Manager

Из этого краткого руководства вы узнаете, как с помощью шаблона Azure Resource Manager создать рабочую область Azure Databricks с кластером Apache Spark. Вы запустите задание в кластере и воспользуетесь пользовательскими диаграммами для получения отчетов в режиме реального времени на основе демографических данных по бесплатному и платному использованию.

## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure — [создайте бесплатную учетную запись](https://azure.microsoft.com/free/).

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портале Azure](https://portal.azure.com). 

> [!Note]
> Инструкции из этого руководство нельзя выполнять с **бесплатной пробной версией подписки**.
> Если у вас есть бесплатная учетная запись, перейдите к профилю и измените подписку на подписку с **оплатой по мере использования**. Дополнительные сведения см. на странице [создания бесплатной учетной записи Azure](https://azure.microsoft.com/free/). Затем [удалите предельную сумму расходов](https://docs.microsoft.com/azure/billing/billing-spending-limit#remove-the-spending-limit-in-account-center) и [запросите увеличение квоты](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) на ЦП в своем регионе. При создании рабочей области Azure Databricks можно выбрать ценовую категорию **Пробная версия ("Премиум" — 14 дней бесплатно (DBU))** для предоставления рабочей области доступа к бесплатным DBU Azure Databricks уровня "Премиум" на 14 дней.

## <a name="create-an-azure-databricks-workspace"></a>Создание рабочей области Azure Databricks

В этом разделе вы создадите рабочую область Azure Databricks с помощью шаблона Azure Resource Manager.

1. Щелкните следующее изображение, чтобы открыть шаблон на портале Azure.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-databricks-workspace%2Fazuredeploy.json" target="_blank"><img src="./media/quickstart-create-databricks-workspace-resource-manager-template/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Укажите необходимые значения, чтобы создать рабочую область Azure Databricks.

   ![Создание рабочей области Azure Databricks с помощью шаблона Azure Resource Manager](./media/quickstart-create-databricks-workspace-resource-manager-template/create-databricks-workspace-using-resource-manager-template.png "Create Azure Databricks workspace using an Azure Resource Manager template")

   Укажите следующие значения.

   |Свойство  |Описание  |
   |---------|---------|
   |**Подписка**     | Выберите подписку Azure в раскрывающемся списке.        |
   |**Группа ресурсов**     | Укажите, следует ли создать новую группу ресурсов или использовать имеющуюся. Группа ресурсов — это контейнер, содержащий связанные ресурсы для решения Azure. Дополнительные сведения см. в [обзоре группы ресурсов Azure](../azure-resource-manager/resource-group-overview.md). |
   |**Местоположение.**     | Выберите регион **Восточная часть США 2**. Другие доступные регионы см. в статье о [доступности служб Azure по регионам](https://azure.microsoft.com/regions/services/).        |
   |**Имя рабочей области**     | Укажите имя рабочей области Databricks.        |
   |**Ценовая категория**     |  Вы можете выбрать уровень **Стандартный** или **Премиум**. Дополнительные сведения об этих ценовых категориях см. на [странице цен на Databricks](https://azure.microsoft.com/pricing/details/databricks/).       |

3. Установите флажок **Я принимаю указанные выше условия** и **Закрепить на панели мониторинга**, а затем нажмите кнопку **Приобрести**.

4. Создание рабочей области займет несколько минут. Во время создания рабочей области на портале с правой стороны отображается плитка **Идет отправка развертывания для Azure Databricks**. Возможно, вам потребуется прокрутить панель мониторинга, чтобы увидеть эту плитку. В верхней части экрана также будет отображаться индикатор хода выполнения. Следить за выполнением можно с помощью любого из этих элементов.

   ![Плитка развертывания Databricks](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-deployment-tile.png "Databricks deployment tile")

## <a name="create-a-spark-cluster-in-databricks"></a>Создание кластера Spark в Databricks

1. На портале Azure перейдите к созданной рабочей области Databricks, а затем выберите **Launch Workspace** (Запуск рабочей области).

2. Вы будете перенаправлены на портал Azure Databricks. На портале выберите **Кластер**.

   ![Databricks в Azure](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-on-azure.png "Databricks on Azure")

3. На странице **создания кластера** укажите значения для создания кластера.

   ![Создание кластера Databricks Spark в Azure](./media/quickstart-create-databricks-workspace-resource-manager-template/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

   Для всех остальных параметров, кроме следующих, примите значения по умолчанию:

   * Введите имя кластера.
   * В рамках этой статьи создайте кластер со средой выполнения **4.0**.
   * Убедитесь, что установлен флажок **Terminate after \_\_ minutes of activity** (Завершить через \_\_ минут бездействия). Укажите длительность (в минутах) для завершения работы кластера, если тот не используется.

   Выберите **Create cluster** (Создать кластер). После запуска кластера можно вложить записные книжки в кластер и запустить задания Spark.

Дополнительные сведения о создании кластеров см. в статье [о создании кластера Spark в Azure Databricks](https://docs.azuredatabricks.net/user-guide/clusters/create.html).

## <a name="run-a-spark-sql-job"></a>Выполнение задания Spark SQL

Прежде чем приступить к работе с этим разделом, выполните следующие предварительные требования.

* [Создайте учетную запись хранения BLOB-объектов Azure](../storage/common/storage-quickstart-create-account.md).
* Скачайте образец файла JSON на [веб-сайте GitHub](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json).
* Отправьте пример JSON-файла в созданную учетную запись хранения BLOB-объектов Azure. Для передачи файлов можно использовать [обозреватель службы хранилища Microsoft Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md).

Выполните указанные ниже задачи, чтобы создать записную книжку в Databricks, настроить ее для считывания данных из учетной записи хранения больших двоичных объектов Azure, а затем запустить задание Spark SQL в данных.

1. В левой области щелкните **рабочую область**. В раскрывающемся списке **рабочей области** выберите **Создать**, а затем выберите **записную книжку**.

   ![Создание записной книжки в Databricks](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-create-notebook.png "Create notebook in Databricks")

2. В диалоговом окне **создания записной книжки** введите имя, выберите **Scala** в качестве языка, а затем выберите созданный ранее кластер Spark.

   ![Создание записной книжки в Databricks](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-notebook-details.png "Create notebook in Databricks")

   Нажмите кнопку **Создать**.

3. На этом шаге свяжите учетную запись службы хранения Azure с кластером Spark Databricks. Есть два способа выполнения этой ассоциации. Вы можете подключить учетную запись хранения Azure к файловой системе Databricks (DBFS) или получить прямой доступ к учетной записи хранения Azure из создаваемого приложения.

   > [!IMPORTANT]
   >В этой статье используется **подход с подключением хранилища к DBFS**. Он гарантирует, что подключенное хранилище самостоятельно связывается с файловой системой кластера. Таким образом, любое приложение, обращающееся к кластеру, может также использовать связанное хранилище. Подход с получением прямого доступа ограничен использованием приложения, с помощью которого настраивается доступ.
   >
   > Для применения подхода с подключением необходимо создать кластер Spark с версией среды выполнения Databricks **4.0**, выбранной в этой статье.

   В следующем фрагменте кода замените `{YOUR CONTAINER NAME}`, `{YOUR STORAGE ACCOUNT NAME}` и `{YOUR STORAGE ACCOUNT ACCESS KEY}` соответствующими значениями для вашей учетной записи хранения Azure. Вставьте фрагмент кода в пустую ячейку в записной книжке и нажмите сочетание клавиш SHIFT + ВВОД, чтобы запустить ячейку кода.

   * **Подключите учетную запись хранения к DBFS (рекомендуется)** . В этом фрагменте кода путь к учетной записи хранения Azure подключается к `/mnt/mypath`. Таким образом, при следующих попытках получить доступ к учетной записи хранения Azure вам не нужно будет предоставлять полный путь. Вы можете просто указать `/mnt/mypath`.

          dbutils.fs.mount(
            source = "wasbs://{YOUR CONTAINER NAME}@{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net/",
            mountPoint = "/mnt/mypath",
            extraConfigs = Map("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net" -> "{YOUR STORAGE ACCOUNT ACCESS KEY}"))

   * **Получите прямой доступ к учетной записи хранения.**

          spark.conf.set("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net", "{YOUR STORAGE ACCOUNT ACCESS KEY}")

     Сведения о том, как получить ключ учетной записи хранения, см. в разделе [Управление учетной записью хранения](../storage/common/storage-account-manage.md#access-keys).

   > [!NOTE]
   > Вы также можете воспользоваться Azure Data Lake Store с кластером Spark в Azure Databricks. Дополнительные сведения см. в разделе [Azure Data Lake Store](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html).

4. Выполните инструкцию SQL для создания временной таблицы, используя данные образца файла данных JSON, **small_radio_json.json**. В указанном ниже фрагменте кода замените значения заполнителя именем контейнера и учетной записи хранения. Вставьте фрагмент кода в ячейку кода в записной книжке, а затем нажмите сочетание клавиш SHIFT + ВВОД. Во фрагменте кода `path` обозначает расположение примера файла JSON, который вы передали в учетную запись хранения Azure.

   ```sql
   %sql
   DROP TABLE IF EXISTS radio_sample_data;
   CREATE TABLE radio_sample_data
   USING json
   OPTIONS (
    path "/mnt/mypath/small_radio_json.json"
   )
   ```

   После успешного выполнения команды все данные из файла JSON будут отображаться в виде таблицы в кластере Databricks.

   Волшебная команда языка `%sql` позволяет выполнить код SQL из записной книжки, даже если записная книжка другого типа. Дополнительные сведения см. в статье об [объединении языков программирования в записной книжке](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook).

5. Рассмотрим моментальный снимок примера данных JSON, чтобы лучше ознакомиться с запросом, который вы выполняете. Вставьте следующий фрагмент кода в ячейку кода и нажмите клавиши **SHIFT + ВВОД**.

   ```sql
   %sql
   SELECT * from radio_sample_data
   ```

6. Отобразятся табличные данные, как показано на следующем снимке экрана (показаны только некоторые столбцы).

   ![Пример данных JSON](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-sample-csv-data.png "Sample JSON data")

   Среди прочего пример данных демонстрирует пол аудитории радиоканала (имя столбца **gender**), а также категорию подписки (бесплатная или платная) (имя столбца **level**).

7. Теперь можно создать визуальное представление данных по каждому полу, а также по числу пользователей с бесплатными и платными учетными записями. В нижней части выходных табличных данных щелкните значок **линейчатой диаграммы**, а затем — **параметры построения**.

   ![Создание линейчатой диаграммы](./media/quickstart-create-databricks-workspace-resource-manager-template/create-plots-databricks-notebook.png "Create bar chart")

8. В разделе **настроек построения** перетащите значения, как показано на снимке экрана.

   ![Настройка линейчатой диаграммы](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-notebook-customize-plot.png "Customize bar chart")

   * Для параметра **Ключи** задайте значение **gender**.
   * Для параметра **Series groupings** (Группирование ряда) задайте значение **level**.
   * Для параметра **Значения** задайте значение **level**.
   * Для параметра **Агрегирование** задайте значение **COUNT**.

   Нажмите кнопку **Применить**.

9. Выходные данные отображают визуальное представление, как показано на следующем снимке экрана:

   ![Настройка линейчатой диаграммы](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-sql-query-output-bar-chart.png "Customize bar chart")

## <a name="clean-up-resources"></a>Очистка ресурсов

Когда вы выполните задачи в статье, можно будет завершить работу кластера. Для этого в рабочей области Azure Databricks на панели слева выберите **Кластеры**. Для кластера, работу которого необходимо завершить, переместите указатель мыши на многоточие в столбце **Actions** (Действия) и выберите значок **Завершить**.

![Завершение работы кластера Databricks](./media/quickstart-create-databricks-workspace-resource-manager-template/terminate-databricks-cluster.png "Stop a Databricks cluster")

Если не завершить работу кластера вручную, она завершится автоматически, если во время создания кластера вы установили флажок **Terminate after \_\_ minutes of inactivity** (Завершать работу после \_\_ мин бездействия). В этом случае работа кластера автоматически завершается, если он был неактивным в течение определенного времени.

## <a name="next-steps"></a>Дополнительная информация

В этой статье вы создали кластер Spark в Azure Databricks и запустили задание Spark, используя данные в хранилище Azure. Вы также можете просмотреть [источники данных Spark](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html), чтобы узнать, как импортировать данные из источников данных в Azure Databricks. Можно также ознакомиться с шаблоном Resource Manager для [создания рабочей области Azure Databricks с настраиваемым адресом виртуальной сети](https://github.com/Azure/azure-quickstart-templates/tree/master/101-databricks-workspace-with-custom-vnet-address). Синтаксис и свойства JSON, используемые в шаблоне, см. в справочнике по шаблону [Microsoft.Databricks/workspaces](/azure/templates/microsoft.databricks/workspaces).

Перейдите к следующей статье, чтобы узнать, как выполнять операции ETL (извлечения, преобразование и загрузка данных) с помощью Azure Databricks.

> [!div class="nextstepaction"]
> [Извлечение, преобразование и загрузка данных с помощью Azure Databricks](databricks-extract-load-sql-data-warehouse.md)
