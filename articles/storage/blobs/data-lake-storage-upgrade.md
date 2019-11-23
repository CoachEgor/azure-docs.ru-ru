---
title: Upgrade Azure Data Lake Storage from Gen1 to Gen2
description: Upgrade Azure Data Lake Storage from Gen1 to Gen2.
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 11/19/2019
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.reviewer: rugopala
ms.openlocfilehash: 41074561b4805fef1889bd889b625e1a59d57d91
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74327867"
---
# <a name="upgrade-azure-data-lake-storage-from-gen1-to-gen2"></a>Upgrade Azure Data Lake Storage from Gen1 to Gen2

Если вы используете Azure Data Lake Storage 1-го поколения в решениях для аналитики больших данных, это руководство поможет вам обновить эти решения, чтобы использовать Azure Data Lake Storage 2-го поколения. С помощью этого документа можно оценить зависимости вашего решения от Azure Data Lake Storage 1-го поколения. Также в этом руководстве показано, как запланировать и выполнить обновление.

Мы поможем вам выполнить следующие задачи:

:heavy_check_mark: Assess your upgrade readiness

:heavy_check_mark: Plan for an upgrade

:heavy_check_mark: Perform the upgrade

## <a name="assess-your-upgrade-readiness"></a>Оценка готовности к обновлению

Наша цель заключается в том, чтобы все возможности Data Lake Storage 1-го поколения стали доступны в Data Lake Storage 2-го поколения. Способ предоставления этих возможностей, например с помощью SDK, CLI и т. д., в 1-м и 2-м поколении Data Lake Storage может различаться. Приложениям и службам, которые работают с Data Lake Storage 1-го поколения, требуется возможность работать аналогичным образом с Data Lake Storage 2-го поколения. Наконец, некоторые возможности не будут доступны в Data Lake Storage 2-го поколения прямо сейчас. Мы будем объявлять об их добавлении в этом документе.

Следующие разделы помогут вам выбрать оптимальный способ обновления до Data Lake Storage 2-го поколения и наиболее подходящий момент для этого.

### <a name="data-lake-storage-gen1-solution-components"></a>Компоненты решения Data Lake Storage 1-го поколения

Скорее всего, при использовании Data Lake Storage 1-го поколения в конвейерах или решениях для аналитики, для достижения общей комплексной функциональности вы задействуете множество дополнительных технологий. В этой [статье](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-scenarios) описаны различные компоненты потока данных, к которым относятся прием, обработка и использование данных.

Кроме того, существуют сквозные компоненты для подготовки, контроля и отслеживания названных выше компонентов. Каждый из компонентов работает с Data Lake Storage 1-го поколения через интерфейс, который лучше всего приспособлен для этого. Планируя обновление решения до Data Lake Storage 2-го поколения, необходимо помнить об интерфейсах, которые используются. Вам потребуется обновить как интерфейсы управления, так и интерфейсы данных, так как у каждого интерфейса отдельные требования.

![Компоненты решения Data Lake Storage](./media/data-lake-storage-upgrade/solution-components.png)

На **рис. 1** выше показаны функциональные компоненты, которые встречаются в большинстве решений для аналитики.

На **рис. 2** показан пример реализации этих компонентов с использованием определенных технологий.

Функциональные возможности хранения на **рис. 1** обеспечивает Data Lake Storage 1-го поколения (**рис. 2**). Обратите внимание на то, как различные компоненты потока данных взаимодействуют с Data Lake Storage 1-го поколения с помощью REST API или пакета SDK для Java. Также обратите внимание, как сквозные функциональные компоненты взаимодействуют с Data Lake Storage 1-го поколения. The Provisioning component uses Azure Resource templates, whereas the Monitoring component which uses Azure Monitor logs utilizes operational data that comes from Data Lake Storage Gen1.

Чтобы обновить решение с Data Lake Storage 1-го поколения до Data Lake Storage 2-го поколения, потребуется скопировать данные и метаданные, повторно подключить потоки данных, после чего все компоненты должны успешно работать с Data Lake Storage 2-го поколения.

В следующих разделах содержатся сведения, которые помогут вам принимать оптимальные решения:

:heavy_check_mark: Platform capabilities

:heavy_check_mark: Programming interfaces

:heavy_check_mark: Azure ecosystem

:heavy_check_mark: Partner ecosystem

:heavy_check_mark: Operational information

В каждом из разделов вы сможете определить необходимые компоненты для обновления. Когда вы будете уверены, что все возможности доступны или существуют приемлемые обходные пути, перейдите к разделу [Планирование обновления](#planning-for-an-upgrade) этого руководства.

### <a name="platform-capabilities"></a>Возможности платформы

В этом разделе описано, какие возможности платформы Data Lake Storage 1-го поколения сейчас доступны в Data Lake Storage 2-го поколения.

| | Data Lake Storage Gen1 | Data Lake Storage 2-го поколения — цель | Data Lake Storage 2-го поколения — состояние доступности  |
|-|------------------------|-------------------------------|-----------------------------------------------|
| Упорядочение данных| Поддерживает данные, хранящиеся в виде папок и файлов | Поддерживает данные, хранящиеся в виде объектов и больших двоичных объектов, а также папок и файлов — [ссылка](https://docs.microsoft.com/azure/storage/data-lake-storage/namespace) | Поддерживает данные, хранящиеся в виде папок и файлов. *Доступно сейчас*. <br><br> Поддерживает данные, хранящиеся в виде объектов и больших двоичных объектов. *Пока недоступно*. |
| Пространство имен| Иерархическая сущность | Иерархическая сущность |  *Доступно сейчас*  |
| API  | REST API по протоколу HTTPS | REST API по протоколу HTTP/HTTPS| *Доступно сейчас* |
| API серверной части| [REST API, совместимый с WebHDFS](https://msdn.microsoft.com/library/azure/mt693424.aspx) | REST API службы BLOB-объектов Azure. [REST API Data Lake Storage 2-го поколения](https://docs.microsoft.com/rest/api/storageservices/data-lake-storage-gen2) | REST API Data Lake Storage 2-го поколения. *Доступно сейчас*. <br><br> Azure Blob Service REST API – *Available now*       |
| Клиент файловой системы Hadoop | Да ([Azure Data Lake Storage](https://hadoop.apache.org/docs/current/hadoop-azure-datalake/index.html)) | Да ([ABFS](https://jira.apache.org/jira/browse/HADOOP-15407))  | *Доступно сейчас*  |  
| Операции с данными — авторизация  | Списки управления доступом (ACL) POSIX на уровне файлов и папок на основе удостоверений Azure Active Directory  | Списки ACL POSIX на уровне файлов и папок на основе удостоверений Azure Active Directory. [Общий ключ](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key) для авторизации на уровне учетных записей. Управление доступом на основе ролей ([RBAC](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac)) для доступа к контейнерам | *Доступно сейчас* |
| Операции с данными — журналы  | ДА | ДА | *Available now* (Preview). See [known issues](data-lake-storage-known-issues.md).<br><br> Интеграция со службой мониторинга Azure. *Пока недоступно*. |
| Шифрование неактивных данных | Прозрачное, на стороне сервера с использованием управляемых службой ключей и управляемых пользователем ключей в хранилище ключей Azure | Прозрачное, на стороне сервера с использованием управляемых службой ключей и управляемых пользователем ключей в хранилище ключей Azure | Ключи, управляемые службой. *Доступно сейчас*.<br><br> Ключи, управляемые клиентом. *Доступно сейчас*.  |
| Операции управления (например, создание учетной записи) | [Контроль доступа на основе ролей](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC), предоставляемый Azure для управления учетными записями | [Контроль доступа на основе ролей](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC), предоставляемый Azure для управления учетными записями | *Доступно сейчас*|
| Пакеты SDK для разработчиков | .NET, Java, Python, Node.js  | .NET, Java, Python, Node.js, C++, Ruby, PHP, Go, Android, iOS| Blob SDK - *Available now*. Azure Data Lake Storage Gen2 SDK - *Not yet available*  |
| |Оптимизированная производительность для параллельных рабочих нагрузок аналитики. Высокие показатели пропускной способности и операций ввода-вывода в секунду. | Оптимизированная производительность для параллельных рабочих нагрузок аналитики. Высокие показатели пропускной способности и операций ввода-вывода в секунду. | *Доступно сейчас* |
| Поддержка виртуальной сети  | [Интеграция с виртуальной сетью](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-network-security)  | [Использование конечной точки службы для службы хранилища Azure](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) | *Доступно сейчас* |
| Ограничения размера | Нет ограничений на размер учетных записей, размер и количество файлов | Нет ограничений на размер учетных записей и количество файлов. Максимальный размер файла ограничен 5 ТБ. | *Доступно сейчас*|
| Геоизбыточность| Локально избыточное хранилище (LRS) | Locally redundant (LRS) Zone redundant (ZRS) geo-redundant (GRS) Read-access geo-redundant (RA-GRS) See [here](https://docs.microsoft.com/azure/storage/common/storage-redundancy) for more information| *Доступно сейчас* |
| Доступность по регионам | Дополнительные сведения см. [здесь](https://azure.microsoft.com/regions/). | Все [регионы Azure](https://azure.microsoft.com/global-infrastructure/regions/)                                                                                                                                                                                                                                                                                                                                       | *Доступно сейчас*                                                                                                                           |
| Цена                                       | Дополнительные сведения см. на [этой странице](https://azure.microsoft.com/pricing/details/data-lake-store/).                                                                            | Дополнительные сведения см. на [этой странице](https://azure.microsoft.com/pricing/details/storage/data-lake/).                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                           |
| Соглашение об уровне обслуживания по доступности                            | [См. соглашение об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/data-lake-store/v1_0/)                                                                   | [См. соглашение об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/storage/v1_3/)                                                                                                                                                                                                                                                                                                                                                | *Доступно сейчас*                                                                                                                           |
| Управление данными                             | Срок действия файлов                                                                                                                                        | Политики жизненного цикла                                                                                                                                                                                                                                                                                                                                                                                                          | Lifecycle policies *Available now* (Preview). See [known issues](data-lake-storage-known-issues.md).                                                                                                                     |

### <a name="programming-interfaces"></a>Программные интерфейсы

В этой таблице описаны наборы API, доступные для пользовательских приложений. SDK support for ACL and directory-level operations are not yet supported.

|  Набор API                           |  Data Lake Storage Gen1                                                                                                                                                                                                                                                                                                   | Доступность для Data Lake Storage 2-го поколения — с проверкой подлинности на основе общего ключа | Доступность для Data Lake Storage 2-го поколения — с проверкой подлинности на основе OAuth                                                                                                  |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK .NET                  | [Ссылка](https://docs.microsoft.com/dotnet/api/overview/azure/datalakestore/management?view=azure-dotnet) | *Доступно сейчас* | *Доступно сейчас* |
| Пакет Java SDK                | [Ссылка](https://docs.microsoft.com/java/api/overview/azure/datalakestore/management)                                                                                                                                                                                                                                     | *Доступно сейчас*                                                      | *Доступно сейчас*                                     |
| Node.js                | [Ссылка](https://www.npmjs.com/package/azure-arm-datalake-store)                                                                                                                                                                                                                                                                | *Доступно сейчас*                                                     | *Доступно сейчас*                                                                                           |
| Пакет Python SDK                   | [Ссылка](https://docs.microsoft.com/python/api/overview/azure/datalakestore/management?view=azure-python)                                                                                                                                                                                                                 | *Доступно сейчас*                                                      | *Доступно сейчас*                                        |
| REST API                 | [Ссылка](https://docs.microsoft.com/rest/api/datalakestore/accounts)                                                                                                                                                                                                                                                      | *Доступно сейчас*                                                      | *Доступно сейчас*                                                                                                                                               |
| PowerShell | [Ссылка](https://docs.microsoft.com/powershell/module/az.datalakestore)                                                                                                                                                                                                                        | *Доступно сейчас* |
| CLI                 | [Ссылка](https://docs.microsoft.com/cli/azure/dls/account?view=azure-cli-latest)                                                                                                                                                                                                                                          | *Доступно сейчас*                                                      | *Доступно сейчас*                                                               |
| Шаблоны Azure Resource Manager — управление             | [Шаблон1](https://azure.microsoft.com/resources/templates/101-data-lake-store-no-encryption/)  [Шаблон2](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/)  [Шаблон3](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/)  | *Доступно сейчас*                                                      | *Доступно сейчас*                                          |

### <a name="azure-ecosystem"></a>"Экосистема Azure"

При использовании Data Lake Storage 1-го поколения можно задействовать широкий набор продуктов и служб корпорации Майкрософт в сквозных конвейерах. Эти продукты и службы работают с Data Lake Storage 1-го поколения прямо или опосредованно. В этой таблице показан список служб, которые мы изменили для работы с Data Lake Storage 1-го поколения, а также указано, какие из них сейчас совместимы с Data Lake Storage 2-го поколения.

| **Область**             | **Доступность для Data Lake Storage 1-го поколения**                                                                                                                                    | **Доступность для Data Lake Storage 2-го поколения — с проверкой подлинности на основе общего ключа**                                                                                                           | **Доступность для Data Lake Storage 2-го поколения — с проверкой подлинности на основе OAuth**                                                                                        |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Платформа аналитики  | [Apache Hadoop](https://hadoop.apache.org/docs/current/hadoop-azure-datalake/index.html)                                                                                       | *Доступно сейчас*                                                                                                                                                              | *Доступно сейчас*                                                                                                                                 |
|                      | [HDInsight](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal)                                                               | [HDInsight](https://docs.microsoft.com/azure/storage/data-lake-storage/quickstart-create-connect-hdi-cluster) 3.6 — *доступно сейчас*. HDInsight 4.0 — *пока недоступно*      | HDInsight 3.6 ESP. *Доступно сейчас*. <br><br>  HDInsight 4.0 ESP. *Пока недоступно*.                                                                 |
|                      | Databricks Runtime 3.1 и выше                                                                                                                                               | [Databricks Runtime 5.1 и более поздней версии](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html#azure-data-lake-storage-gen2) — *доступно сейчас* | [Databricks Runtime 5.1 и более поздней версии](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html#azure-data-lake-storage-gen2) — *доступно сейчас*                                                                                              |
|                      | [Хранилище данных SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store)                                            | *Не поддерживаются*                                                                                                                                                              | *Доступно сейчас*: [ссылка](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql?view=sql-server-2017) |
| Интеграция данных     | [Фабрика данных](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-store)                                                                                | [Версия 2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage) — *доступно сейчас*. Версия 1 — *не поддерживается*                               | [Версия 2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage). *Доступно сейчас*. <br><br> Версия 1. *Не поддерживается*.  |
|                      | [AdlCopy](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)                                                                 | *Не поддерживаются*                                                                                                                                                              | *Не поддерживаются*                                                                                                                                 |
|                      | [SQL Server Integration Services](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager?view=sql-server-2017) | *Доступно сейчас*                                                                                                                                                          | *Доступно сейчас*                                                                                                                             |
|                      | [Каталог данных](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-with-data-catalog)                                                                       | *Пока недоступно*                                                                                                                                                          | *Пока недоступно*                                                                                                                             |
|                      | [Logic Apps](https://docs.microsoft.com/connectors/azuredatalake/)                                                                                                      | *Доступно сейчас*                                                                                                                                                          | *Доступно сейчас*                                                                                                                             |
| Центр Интернета вещей                  | [Центры событий — функция "Сбор"](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-archive-eventhub-capture)                                                       | *Доступно сейчас*                                                                                                                                                          | *Доступно сейчас*                                                                                                                             |
|                      | [Анализ потока](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-stream-analytics)                                                                   | *Доступно сейчас*                                                                                                                                                          | *Доступно сейчас*                                                                                                                             |
| Потребление          | [Power BI Desktop](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-power-bi)                                                                           | *Доступно сейчас*                                                                                                                                                          | *Доступно сейчас*                                                                                                                             |
|                      | [Excel](https://techcommunity.microsoft.com/t5/Excel-Blog/Announcing-the-Azure-Data-Lake-Store-Connector-in-Excel/ba-p/91677)                                                 | *Пока недоступно*                                                                                                                                                          | *Пока недоступно*                                                                                                                             |
|                      | [Службы Analysis Services](https://blogs.msdn.microsoft.com/analysisservices/2017/09/05/using-azure-analysis-services-on-top-of-azure-data-lake-storage/)                            | *Пока недоступно*                                                                                                                                                          | *Пока недоступно*                                                                                                                             |
| Производительность         | [Портал Azure](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal)                                                                      | *Не поддерживаются*                                                                                                                                                              | Управление учетными записями. *Доступно сейчас*. <br><br>Операции с данными *–* . *Пока недоступно*.                                                                   |
|                      | [средства Data Lake для Visual Studio](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-data-lake-tools-install);                                   | *Пока недоступно*                                                                                                                                                          | *Пока недоступно*                                                                                                                             |
|                      | [Azure Storage Explorer;](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-in-storage-explorer)                                                          | *Доступно сейчас*                                                                                                                                                              | *Доступно сейчас*                                                                                                                                 |
|                      | [Код Visual Studio](https://marketplace.visualstudio.com/items?itemName=usqlextpublisher.usql-vscode-ext)                                                                     | *Пока недоступно*                                                                                                                                                          | *Пока недоступно*                                                                                                                             |

### <a name="partner-ecosystem"></a>"Партнерская экосистема"

В этой таблице показан список сторонних служб и продуктов, которые были изменены для работы с Data Lake Storage 1-го поколения. Также в нем показано, какие из них сейчас совместимы с Data Lake Storage 2-го поколения.

| Область            | Partner  | Продукт или служба  | Доступность для Data Lake Storage 1-го поколения                                                                                                                                               | Доступность для Data Lake Storage 2-го поколения — с проверкой подлинности на основе общего ключа                                                   | Доступность для Data Lake Storage 2-го поколения — с проверкой подлинности на основе OAuth                                                             |
|---------------------|--------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Платформа аналитики | Cloudera     | CDH                  | [Ссылка](https://www.cloudera.com/documentation/enterprise/5-11-x/topics/admin_adls_config.html)                                                                                            | *Пока недоступно*                                                                                                  | [Ссылка](https://www.cloudera.com/documentation/enterprise/latest/topics/admin_adls2_config.html)                                                                                                  |
|                     | Cloudera     | Altus                | [Ссылка](https://www.cloudera.com/more/news-and-blogs/press-releases/2017-09-27-cloudera-introduces-altus-data-engineering-microsoft-azure-hybrid-multi-cloud-data-analytic-workflows.html) | *Не поддерживаются*                                                                                                                  | *Пока недоступно*                                                                                                  |
|                     | HortonWorks  | HDP 3.0              | [Ссылка](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.3/bk_cloud-data-access/content/adls-get-started.html)                                                                       | *Пока недоступно*                                                                                                  | *Пока недоступно*                                                                                                  |
|                     | Qubole       |                      | [Ссылка](https://www.qubole.com/blog/big-data-analytics-microsoft-azure-data-lake-store-qubole/)                                                                                            | *Пока недоступно*                                                                                                  | *Пока недоступно*                                                                                                  |
| Извлечение, преобразование и загрузка                 | Наборы потоков   |                      | [Ссылка](https://streamsets.com/blog/ingest-data-azure)                                                                                                                                     | *Пока недоступно*                                                                                                  | *Пока недоступно*                                                                                                  |
|                     | Informatica  |                      | [Ссылка](https://kb.informatica.com/proddocs/Product%20Documentation/6/IC_Spring2017_MicrosoftAzureDataLakeStoreV2ConnectorGuide_en.pdf)                                                    | *Пока недоступно*                                                                                                  | *Пока недоступно*                                                                                                  |
|                     | Attunity     |                      | [Ссылка](https://www.attunity.com/company/press-releases/microsoft-and-attunity-announce-strategic-partnership-for-data-migration/)                                                         | *Пока недоступно*                                                                                                  | *Пока недоступно*                                                                                                  |
|                     | Alteryx      |                      | [Ссылка](https://help.alteryx.com/2018.2/DataSources/AzureDataLake.htm)                                                                                                                     | *Пока недоступно*                                                                                                  | *Пока недоступно*                                                                                                  |
|                     | ImanisData   |                      | [Ссылка](https://www.imanisdata.com/expansion-azure-support-azure-data-lake-store-integration/)                                                                                             | *Пока недоступно*                                                                                                  | *Пока недоступно*                                                                                                  |
|                     | WANdisco     |                      | [Ссылка](https://azure.microsoft.com/blog/globally-replicated-data-lakes-with-livedata-using-wandisco-on-azure/)                                                                      | [Ссылка](https://azure.microsoft.com/blog/globally-replicated-data-lakes-with-livedata-using-wandisco-on-azure/) | [Ссылка](https://azure.microsoft.com/blog/globally-replicated-data-lakes-with-livedata-using-wandisco-on-azure/) |

### <a name="operational-information"></a>"Эксплуатационные данные"

Data Lake Storage 1-го поколения передает определенную информацию и данные в другие службы, что помогает вводить в эксплуатацию конвейеры. В этой таблице показана доступность соответствующей поддержки в Data Lake Storage 2-го поколения.

| Тип данных                                                                                       | Доступность для Data Lake Storage 1-го поколения                                                                 | Доступность для Data Lake Storage 2-го поколения                                                                                               |
|--------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| Данные о выставлении счетов — единицы измерения, которые отправляются в команду по вопросам коммерции для выставления счетов, а затем становятся доступными для клиентов  | *Доступно сейчас*                                                                                             | *Доступно сейчас*                                                                                                                           |
| Журналы действий                                                                                          | [Ссылка](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs#audit-logs)   | Одноразовые запросы на получение журналов с заданной длительностью с использованием запросов в службу поддержки — *доступно сейчас*. Интеграция со службой мониторинга Azure — *пока недоступно* |
| Журналы диагностики                                                                                        | [Ссылка](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs#request-logs) | *Available now* (Preview). See [known issues](data-lake-storage-known-issues.md). <br>Azure Monitoring integration - *Not yet available* |
| Метрики                                                                                                | *Не поддерживаются*                                                                                               | *Доступно сейчас —* [ссылка](https://docs.microsoft.com/azure/storage/common/storage-metrics-in-azure-monitor)                          |

## <a name="planning-for-an-upgrade"></a>Планирование обновления

В этом разделе предполагается, что вы ознакомились с разделом [Оценка готовности к обновлению](#assess-your-upgrade-readiness) этого руководства и что все необходимые зависимости соблюдены. Если есть возможности, которые все еще недоступны в Data Lake Storage 2-го поколения, продолжайте только в том случае, если вы знаете соответствующие обходные пути. В следующих разделах содержатся рекомендации о том, как запланировать обновление конвейеров. Выполнение фактического обновления будет описано в разделе [Выполнение обновления](#performing-the-upgrade) этого руководства.

### <a name="upgrade-strategy"></a>Стратегия обновления

Важнейшим элементом обновления является выбор стратегии. Это решение определяет доступные для выбора варианты.

This table lists some well-known strategies that have been used to migrate databases, Hadoop clusters, etc. We'll adopt similar strategies in our guidance, and adapt them to our context.

| Стратегия                   | Преимущества                                                                                  | Недостатки                                                           | Сценарии использования                                                                                                                                                                                             |
|--------------------------------|-------------------------------------------------------------------------------------------|--------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Перенос по методу lift-and-shift                 | Максимальна простота.                                                                                 | Приводит к простою при копировании данных, переносе заданий и перемещении входящего и исходящего трафика                                             | Для простых решений, когда доступ к одной и той же учетной записи 1-го поколения есть у небольшого количества решений, для которых можно одновременно выполнить быстрый контролируемый перенос.                                                  |
| Однократное и добавочное копирование | Сокращение времени простоя за счет копирования в фоновом режиме, пока исходная система остается активной. | Приводит к простою при перемещении входящего и исходящего трафика.                   | Копируется большой объем данных, и простои, с которыми сопряжено использование метода lift-and-shift, недопустимы. Может потребоваться тестирование со значительным объемом рабочих данных в целевой системе до выполнения переноса. |
| Параллельное внедрение              | Минимальное время простоя. Предоставляет время для переноса приложений по собственному усмотрению.           | Наиболее сложный, так как требуется двунаправленная синхронизация между двумя системами. | Для сложных сценариев, когда приложения, созданные на основе Data Lake Storage 1-го поколения, нельзя перенести все за один раз и их необходимо переносить в пошаговом режиме.                                                      |

Ниже приведены дополнительные сведения о порядке действий для каждой стратегии. Эти действия указывают, что следует делать с компонентами, задействованными в обновлении. К ним относятся вся исходная система, вся целевая система, источники входящего трафика для исходной системы, пункты назначения исходящего трафика для исходной системы и задания, выполняемые в исходной системе.

Эти действия не являются предписанием. Они приводятся для того, чтобы задать общий план для каждой стратегии. Мы предоставим примеры внедрения каждой из стратегий по мере их реализации.

#### <a name="lift-and-shift"></a>Перенос по методу lift-and-shift

1. Приостановите исходную систему — источники входящего трафика, задания, пункты назначения исходящего трафика.
2. Скопируйте все данные из исходной системы в целевую.
3. Укажите для всех источников входящего трафика целевую систему. Укажите пункт назначения исходящего трафика в целевой системе.
4. Переместите все задания в целевую систему, измените и запустите их в ней.
5. Отключите исходную систему.

#### <a name="copy-once-and-copy-incremental"></a>Однократное и добавочное копирование

1. Скопируйте данные из исходной системы в целевую.
2. Копируйте добавочные данные из исходной системы в целевую с регулярными интервалами.
3. Укажите пункт назначения исходящего трафика в целевой системе.
4. Переместите все задания в целевую систему, измените и запустите их в ней.
5. Пошагово укажите для источников входящего трафика целевую систему удобным для вас способом.
6. Когда все источники входящего трафика будут указывать на целевую систему, сделайте следующее:
    1. Отключите добавочное копирование.
    2. Отключите исходную систему.

#### <a name="parallel-adoption"></a>Параллельное внедрение

1. Настройте целевую систему.
2. Настройте двунаправленную репликацию между исходной и целевой системами.
3. Пошагово укажите для источников входящего трафика целевую систему.
4. Пошагово переместите все задания в целевую систему, измените и запустите их в ней.
5. Пошагово укажите пункты назначения исходящего трафика для целевой системы.
6. Когда все исходные источники входящего трафика, задания и пункты назначения исходящего трафика будут работать с целевой системой, отключите исходную систему.

### <a name="data-upgrade"></a>Обновление данных

Общая стратегия, применяемая для выполнения обновления (описанного в разделе [Стратегия обновления](#upgrade-strategy) этого руководства), определяет средства, с помощью которых можно выполнить обновление данных. Перечисленные ниже инструменты основаны на текущих сведениях и являются рекомендуемыми. 

#### <a name="tools-guidance"></a>Рекомендации по инструментам

| Стратегия                       | Средства                                                                                                             | Преимущества                                                                                                                             | Рекомендации                                                                                                                                                                                                                                                                                                                |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Перенос по методу lift-and-shift**                 | [Фабрика данных Azure](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2-from-gen1) | Управляемая облачная служба                                                                                                                | Both data and ACLs can be copied over currently.                                                                                                                                                                                                                                                                      |
|                                    | [Distcp](https://hadoop.apache.org/docs/r1.2.1/distcp.html)                                                           | Хорошо известное средство от Hadoop. С его помощью можно копировать разрешения, то есть списки ACL                                                   | Требуется кластер с возможностью одновременного подключения к Data Lake Storage как 1-го, так и 2-го поколения.                                                                                                                                                                                   |
| **Однократное и добавочное копирование** | Фабрика данных Azure                                                                                                    | Управляемая облачная служба                                                                                                                | Both data and ACLs can be copied over currently. Для поддержки добавочного копирования в ADF данные должны быть упорядочены в виде временных рядов. Кратчайший интервал между добавочными копиями составляет [15 минут](https://docs.microsoft.com/azure/data-factory/how-to-create-tumbling-window-trigger). |
| **Параллельное внедрение**              | [WANdisco](https://docs.wandisco.com/bigdata/wdfusion/adls/)                                                           | Поддержка согласованной репликации. При использовании чистой среды Hadoop, подключенной к Azure Data Lake Storage, — поддержка двухсторонней репликации | Если чистая среда Hadoop не используется, при репликации может возникнуть задержка.                                                                                                                                                                                                                                                  |

Note that there are third-parties that can handle the Data Lake Storage Gen1 to Data Lake Storage Gen2 upgrade without involving the above data/meta-data copying tools (For example: [Cloudera](https://blog.cloudera.com/blog/2017/08/use-amazon-s3-with-cloudera-bdr/)). Они предоставляют единый интерфейс для переноса данных и рабочих нагрузок. Возможно вам потребуется выполнить отдельное обновление любых средств, находящихся за пределами их экосистемы.

#### <a name="considerations"></a>Рекомендации

* Прежде чем начинать любой этап обновления, необходимо вручную создать учетную запись Data Lake Storage 2-го поколения, так как текущие рекомендации не включают в себя автоматический процесс обработки учетной записи 2-го поколения, основанный на данных учетной записи 1-го поколения. Убедитесь, что сравниваются процессы создания учетных записей [1-го](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) и [2-го](https://docs.microsoft.com/azure/storage/data-lake-storage/quickstart-create-account) поколения.

* Data Lake Storage 2-го поколения поддерживает файлы размером до 5 ТБ. Для обновления до Data Lake Storage 2-го поколения может потребоваться изменить размер файлов в Data Lake Storage 1-го поколения, чтобы их размер не превышал 5 ТБ.

* Если вы используете средство, которое не копирует списки ACL, или не намерены их копировать, вам придется настроить списки ACL в системе назначения вручную на соответствующем верхнем уровне. Это можно сделать с помощью Обозревателя службы хранилища. Убедитесь, что эти списки ACL являются списками, установленными по умолчанию, чтобы копируемые файлы и папки наследовали их.

* В Data Lake Storage 1-го поколения наиболее высокий уровень, на котором можно задать списки ACL, — корень учетной записи. In Data Lake Storage Gen2, however, the highest level you can set ACLs is at the root folder in a container, not the whole account. Таким образом, если нужно задать списки ACL по умолчанию на уровне учетной записи, необходимо продублировать их по всем файловым системам в учетной записи Data Lake Storage 2-го поколения.

* Ограничения на именование файлов в двух системах хранения различаются. Эти различия особенно важны при копировании из Data Lake Storage 2-го поколения в Data Lake Storage 1-го поколения, так как в этом случае в целевой системе ограничения более жесткие.

### <a name="application-upgrade"></a>Обновление приложения

Если вам необходимо создавать приложения в Data Lake Storage 1-го поколения или Data Lake Storage 2-го поколения, следует сначала выбрать соответствующий программный интерфейс. При вызове API в этом интерфейсе необходимо будет указать соответствующий URI и учетные данные. Представление этих трех элементов — API, URI и способа предоставления учетных данных — различаются в Data Lake Storage 1-го поколения и Data Lake Storage 2-го поколения. Поэтому в процессе обновления приложений необходимо будет соответствующим образом сопоставить эти три компонента.

#### <a name="uri-changes"></a>Изменения URI

The main task here is to translate URI's that have a prefix of `adl://` into URI's that have an `abfss://` prefix.

Схема URI для Data Lake Storage 1-го поколения подробно описана [здесь](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-store), но в целом она имеет вид *adl://mydatalakestore.azuredatalakestore.net/\<путь_к_файлу\>.*

The URI scheme for accessing Data Lake Storage Gen2 files is explained [here](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md) in detail, but broadly speaking, it is `abfss://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`.

Вам потребуется просмотреть существующие приложения и убедиться, что URI соответствующим образом изменены и ориентированы на Data Lake Storage 2-го поколения. Кроме того, потребуется добавить соответствующие учетные данные. Наконец, способ снятия с учета исходных приложений и замены их новыми должен быть согласован с общей стратегией обновления.

#### <a name="custom-applications"></a>Пользовательские приложения

В зависимости от интерфейса, используемого приложением с Data Lake Storage 1-го поколения, его потребуется изменить для адаптации к Data Lake Storage 2-го поколения.

##### <a name="rest-apis"></a>API-интерфейсы REST

Если в приложении используются интерфейсы REST API Data Lake Storage, необходимо изменить приложение для использования REST API Data Lake Storage 2-го поколения. Ссылки содержатся в разделе *Программные интерфейсы*.

##### <a name="sdks"></a>Пакеты SDK

Как указано в разделе *Оценка готовности к обновлению*, пакеты SDK сейчас недоступны. Если нужно перенести приложения в Data Lake Storage 2-го поколения, рекомендуем дождаться выпуска поддерживаемых пакетов SDK.

##### <a name="powershell"></a>PowerShell

Как указано в разделе "Оценка готовности к обновлению", поддержка PowerShell сейчас недоступна для плоскости данных.

Командлеты плоскости управления можно заменить соответствующими командлетами в Data Lake Storage 2-го поколения. Ссылки содержатся в разделе *Программные интерфейсы*.

##### <a name="cli"></a>CLI

Как указано в разделе *Оценка готовности к обновлению*, поддержка интерфейса командной строки (CLI) сейчас недоступна для плоскости данных.

Команды плоскости управления можно заменить соответствующими командами в Data Lake Storage 2-го поколения. Ссылки содержатся в разделе *Программные интерфейсы*.

### <a name="analytics-frameworks-upgrade"></a>Обновление платформ аналитики

Если ваше приложение создает метаданные на основе сведений, содержащихся в хранилище, например явные пути к файлам и папкам, после обновления данных и метаданных хранилища вам потребуется выполнить дополнительные действия. Это особенно актуально для платформ аналитики, таких как Azure HDInsight, Databricks и т. д., которые обычно создают данные каталога на основе данных хранилища.

Платформы аналитики работают с данными и метаданными, содержащимися в удаленных хранилищах, таких как Data Lake Storage 1-го и 2-го поколения. Таким образом, теоретически подсистемы могут быть временными и вызываться только тогда, когда необходимо выполнить задания с хранимыми данными.

Однако в целях оптимизации производительности платформы могут создавать явные ссылки на файлы и папки, содержащиеся в удаленном хранилище, а затем создать кэш для их размещения. В случае изменения URI удаленных данных, например, если данные кластера хранились в Data Lake Storage 1-го поколения, а теперь их нужно хранить в Data Lake Storage 2-го поколения, URI для одного и того же скопированного содержимого будет отличаться. Таким образом, после обновления данных и метаданных также потребуется обновить или повторно инициализировать кэши для этих подсистем.

В рамках планирования необходимо будет идентифицировать приложение и выяснить, как сведения о метаданных можно повторно инициализировать, ориентировав их на данные, которые теперь хранятся в Data Lake Storage 2-го поколения. Ниже приведено руководство по распространенным платформам аналитики, которое поможет выполнить шаги по их обновлению.

#### <a name="azure-databricks"></a>Azure Databricks

В зависимости от выбранной стратегии обновления шаги будут отличаться. В текущем разделе предполагается, что вы выбрали стратегию lift-and-shift. Кроме того, от существующей рабочей области Databricks, которая раньше получала доступ к данным в учетной записи Data Lake Storage 1-го поколения, теперь ожидается работа с данными, скопированными в учетную запись Data Lake Storage 2-го поколения.

Во-первых, убедитесь, что вы создали учетную запись 2-го поколения, а затем скопировали данные и метаданные из системы 1-го поколения в систему 2-го поколения с помощью соответствующего инструмента. Эти инструменты указаны в разделе [Обновление данных](#data-upgrade) этого руководства.

Затем [обновите](https://docs.azuredatabricks.net/user-guide/clusters/index.html) существующий кластер Databricks, чтобы начать использовать среду выполнения Databricks 5.1 или более поздней версии, которая должна поддерживать Data Lake Storage 2-го поколения.

Последующие шаги зависят от того, как существующая рабочая область Databricks получает доступ к данным в учетной записи Data Lake Storage 1-го поколения. Это можно сделать либо путем вызова URI с префиксом adl:// [непосредственно](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html#access-azure-data-lake-store-directly) из записных книжек, либо через [точки подключения](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html#mount-azure-data-lake-store-with-dbfs).

При непосредственном доступе из записных книжек путем указания полных URI с префиксом adl:// потребуется просмотреть все записные книжки и изменить конфигурацию так, чтобы доступ осуществлялся к соответствующим URI Data Lake Storage 2-го поколения.

Забегая наперед, необходимо будет перенастроить их так, чтобы они указывали на учетную запись Data Lake Storage 2-го поколения. Дополнительные изменения не требуются, и записные книжки должны работать так же, как раньше.

При использовании любой другой стратегии обновления можно создать разновидность описанных выше действий в соответствии с вашими требованиями.

### <a name="azure-ecosystem-upgrade"></a>Обновление экосистемы Azure

Все инструменты и службы, указанные в разделе [Экосистема Azure](#azure-ecosystem) этого руководства, необходимо будет настроить для работы с Data Lake Storage 2-го поколения.

Во-первых, убедитесь в доступности интеграции с Data Lake Storage 2-го поколения.

Then, the elements called out above (For example: URI and credentials), will have to be changed. Можно изменить существующий экземпляр, который работает с Data Lake Storage 1-го поколения, или создать новый экземпляр, который будет работать с Data Lake Storage 2-го поколения.

### <a name="partner-ecosystem-upgrade"></a>Обновление экосистем партнеров

Обратитесь к партнеру, предоставляющему тот или иной компонент или инструмент, чтобы убедиться в том, что они могут работать с Data Lake Storage 2-го поколения. 

## <a name="performing-the-upgrade"></a>Выполнение обновления

### <a name="pre-upgrade"></a>Перед обновлением

В рамках этого процесса вы изучили разделы *Оценка готовности к обновлению* и [Планирование обновления](#planning-for-an-upgrade) этого руководства, получили все необходимые сведения и создали план, соответствующий вашим потребностям. Вероятно, на этом этапе у вас будет предусмотрена задача тестирования.

### <a name="in-upgrade"></a>В ходе обновления

В зависимости от выбранной стратегии и сложности решения этот этап может быть коротким или расширенным при наличии множества рабочих нагрузок, ожидающих пошагового переноса в Data Lake Storage 2-го поколения. Это будет наиболее важной частью процесса обновления.

### <a name="post-upgrade"></a>После обновления

После завершения переноса заключительные действия включают в себя тщательную проверку. This would include but not be limited to verifying data has been copied over reliably, verifying ACLs have been set correctly, verifying end-to-end pipelines are functioning correctly etc. After the verifications have been completed, you can now turn off your old pipelines, delete your source Data Lake Storage Gen1 accounts and go full speed on your Data Lake Storage Gen2-based solutions.

## <a name="conclusion"></a>Заключение

Рекомендации, приведенные в этом документе, должны помочь вам обновить свое решение для использования Data Lake Storage 2-го поколения. 

Если у вас есть дополнительные вопросы или вы хотите оставить отзыв, напишите комментарий ниже или добавьте отзыв на [форуме отзывов и предложений Azure](https://feedback.azure.com/forums/327234-data-lake).
