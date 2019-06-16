---
title: Источники данных, поддерживаемые в службах Azure Analysis Services | Документы Майкрософт
description: Описание источников данных, поддерживаемых для моделей данных в службах Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 7abd0ac3d95825594dffe385bccc1672d0f71c5f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66142560"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Источники данных, поддерживаемые в службах Azure Analysis Services

Источники данных и соединители, представленные в мастере получения или импорта данных в Visual Studio, отображаются как для служб Azure Analysis Services, так и для служб SQL Server Analysis Services. Однако в Azure Analysis Services поддерживаются не все показанные источники данных и соединители. Типы источников данных, к которым можно подключиться, зависят от многих факторов, таких как уровень совместимости модели, доступные соединители данных, тип аутентификации, поставщики и поддержка локального шлюза данных. 

## <a name="azure-data-sources"></a>Источники данных Azure

|Источник данных  |В памяти  |DirectQuery  |
|---------|---------|---------|
|База данных SQL Azure<sup>[2](#azsqlmanaged)</sup>     |   Да      |    Да      |
|Хранилище данных SQL Azure     |   Да      |   Да       |
|Хранилище BLOB-объектов Azure<sup>[1](#tab1400a)</sup>     |   Да       |    Нет      |
|Табличное хранилище Azure<sup>[1](#tab1400a)</sup>    |   Да       |    Нет      |
|Azure Cosmos DB<sup>[1](#tab1400a)</sup>     |  Да        |  Нет        |
|Azure Data Lake Store (1-го поколения)<sup>[1](#tab1400a)</sup>, <sup>[4](#gen2)</sup>      |   Да       |    Нет      |
|Azure HDInsight (HDFS)<sup>[1](#tab1400a)</sup>     |     Да     |   Нет       |
|Azure HDInsight Spark<sup>[1](#tab1400a)</sup>, <sup>[3](#databricks)</sup>     |   Да       |   Нет       |
||||

<a name="tab1400a">1</a> Только табличная модель 1400 и более поздние.   
<a name="azsqlmanaged">2</a> Поддерживается Управляемый экземпляр Базы данных SQL Azure. Так как управляемый экземпляр работает в виртуальной сети Azure с частным IP-адресом, требуется локальный шлюз данных. В настоящее время Azure SQL управляемый экземпляр базы данных с общедоступной конечной точки не поддерживается.   
<a name="databricks">3</a> Экземпляр Azure Databricks, использующий соединитель Spark, в настоящее время не поддерживается.   
<a name="gen2">4</a> ADLS 2-го поколения в настоящее время не поддерживается.


**Поставщик**   
Для размещенных в памяти моделей и моделей DirectQuery, подключающихся к источникам данных Azure, используется поставщик данных .NET Framework для SQL Server.

## <a name="on-premises-data-sources"></a>Локальные источники данных

Для подключения к локальным источникам данных с сервера автономной системы Azure требуется локальный шлюз. При использовании шлюза требуются 64-разрядные поставщики.

### <a name="in-memory-and-directquery"></a>Размещение в памяти и DirectQuery

|Источник данных | Поставщик с размещением в памяти | Поставщик DirectQuery |
|  --- | --- | --- |
| SQL Server; |SQL Server Native Client 11.0, поставщик Microsoft OLE DB для SQL Server, поставщик данных .NET Framework для SQL Server | Поставщик данных .NET Framework для SQL Server |
| хранилище данных SQL Server. |SQL Server Native Client 11.0, поставщик Microsoft OLE DB для SQL Server, поставщик данных .NET Framework для SQL Server | Поставщик данных .NET Framework для SQL Server |
| Oracle | Поставщик OLE DB для Oracle, поставщик данных Oracle для .NET |Поставщик данных Oracle для .NET |
| Teradata |Поставщик OLE DB для Teradata, поставщик данных Teradata для .NET |Поставщик данных Teradata для .NET |
| | | |

### <a name="in-memory-only"></a>Только в памяти

|Источник данных  |  
|---------|
|База данных Access     |  
|Active Directory<sup>[1](#tab1400b)</sup>     |  
|Analysis Services     |  
|Система платформы аналитики     |  
|CSV-файл  |
|Dynamics CRM<sup>[1](#tab1400b)</sup>     |  
|Книга Excel     |  
|Exchange<sup>[1](#tab1400b)</sup>     |  
|Папка<sup>[1](#tab1400b)</sup>     |
|IBM Informix<sup>[1](#tab1400b)</sup> (бета-версия) |
|Документ JSON<sup>[1](#tab1400b)</sup>     |  
|Строки из двоичного файла<sup>[1](#tab1400b)</sup>     | 
|База данных MySQL     | 
|Канал OData<sup>[1](#tab1400b)</sup>     |  
|Запрос ODBC     | 
|OLE DB     |   
|База данных SQL Postgre<sup>[1](#tab1400b)</sup>    | 
|Объекты Salesforce<sup>[1](#tab1400b)</sup> |  
|Отчеты Salesforce<sup>[1](#tab1400b)</sup> |
|SAP HANA<sup>[1](#tab1400b)</sup>    |  
|SAP Business Warehouse<sup>[1](#tab1400b)</sup>    |  
|Список SharePoint<sup>[1](#tab1400b)</sup>, <sup> [2](#filesSP)</sup>     |   
|База данных Sybase     |  
|TXT-файла  |
|Таблица XML<sup>[1](#tab1400b)</sup>    |  
||
 
<a name="tab1400b">1</a> Только табличная модель 1400 и более поздние.   
<a name="filesSP">2</a> -файлы в локальному сайту SharePoint не поддерживаются.

## <a name="specifying-a-different-provider"></a>Указание другого поставщика

Моделям данных в службах Azure Analysis Services могут требоваться разные поставщики данных при подключении к определенным источникам данных. В некоторых случаях табличные модели, которые подключаются к источникам данных с помощью собственных поставщиков, таких как собственный клиент SQL Server (SQLNCLI11), могут возвращать ошибку. При использовании собственных поставщиков, отличных от SQLOLEDB, может появиться сообщение об ошибке: **The provider 'SQLNCLI11.1' is not registered** (Поставщик SQLNCLI11.1 не зарегистрирован). Если же при наличии модели DirectQuery, подключающейся к локальным источникам данных, используются собственные поставщики, может появиться сообщение об ошибке: **Error creating OLE DB row set. Incorrect syntax near 'LIMIT'** (Ошибка при создании набора строк OLE DB. Неверный синтаксис рядом с "LIMIT").

При переносе локальной табличной модели служб SQL Server Analysis Services в службы Azure Analysis Services может потребоваться изменить поставщика.

**Указание поставщика**

1. В разделе SSDT > **Tabular Model Explorer (Обозреватель табличных моделей)**  > **Data Sources (Источники данных)** щелкните правой кнопкой подключение к источнику данных и выберите **Edit Data Source (Изменить источник данных)** .
2. В окне **Edit Connection** (Изменение подключения) щелкните **Advanced** (Дополнительно), чтобы открыть окно дополнительных свойств.
3. В разделе **Set Advanced Properties (Настройка дополнительных свойств)**  > **Providers (Поставщики)** выберите соответствующего поставщика.

## <a name="impersonation"></a>Олицетворение
В некоторых случаях может быть необходимо указать другую учетную запись олицетворения. Учетную запись олицетворения можно указать в Visual Studio (SSDT) или SSMS.

Для локальных источников данных:

* При использовании проверки подлинности SQL олицетворением должна быть учетная запись службы.
* При использовании проверки подлинности Windows задайте пользователя и пароль Windows. Для SQL Server проверка подлинности Windows с использованием конкретной учетной записи олицетворения поддерживается только для моделей данных в памяти.

Для облачных источников данных

* При использовании проверки подлинности SQL олицетворением должна быть учетная запись службы.

## <a name="next-steps"></a>Дальнейшие действия
[Локальный шлюз](analysis-services-gateway.md)   
[Управление службами Analysis Services](analysis-services-manage.md)   

