---
title: Azure базы данных SQL и хранилища данных SQL обнаружение и классификация данных | Документация Майкрософт
description: Azure базы данных SQL и данных обнаружения и классификации
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 03/22/2019
ms.openlocfilehash: e451b7837a1cff4bbeaecd1573dc860524caf4d3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65142658"
---
# <a name="azure-sql-database-and-sql-data-warehouse-data-discovery--classification"></a>Azure базы данных SQL и хранилища данных SQL обнаружение и классификация данных

Служба обнаружения и классификации данных (в настоящее время находится на этапе предварительной версии) предоставляет расширенные возможности, встроенные в службу "База данных SQL Azure", для **обнаружения**, **классификации**, **добавления меток** & **защиты** конфиденциальных данных в базах данных.
Обнаружение и классификация наиболее конфиденциальных данных (деловых или финансовых, медицинских, персональных данных и т. д.) может играть ключевую роль в концепции защиты информации вашей организации. Она может использоваться в качестве инфраструктуры для следующего:

- Соблюдение стандартов конфиденциальности данных и нормативных требований.
- Различные сценарии безопасности, такие как мониторинг (аудит) и оповещение об аномальном доступе к конфиденциальным данным.
- Управление доступом к базам данных, содержащим конфиденциальные данные, и усиление их защиты.

Обнаружение и классификация данных является частью [повышенной безопасности данных](sql-database-advanced-data-security.md) (ADS) предложения, которое представляет собой единую пакет для обеспечения дополнительных возможностей безопасности SQL. Доступ к этой службе можно получить на центральном портале SQL ADS. Там же можете управлять ею.

> [!NOTE]
> Этот документ относится к базе данных SQL Azure и хранилище данных SQL Azure. Для простоты база данных SQL используется как для базы данных SQL, так и для хранилища данных SQL. SQL Server (на локальном компьютере), см. в разделе [SQL данных обнаружения и классификации](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Что такое обнаружение и классификация данных?

Служба обнаружения и классификации данных предоставляет набор расширенных служб и новые возможности SQL, формирующие новую концепцию защиты информации SQL, направленную на защиту данных, а не только базы данных.

- **Обнаружение и рекомендации**

  Подсистема классификации проверяет базы данных и выявляет столбцы, содержащие потенциально конфиденциальные данные. Затем она позволяет легко просмотреть их и применить соответствующие рекомендации по классификации на портале Azure.

- **Добавление меток**

  К столбцам можно добавить метки классификации конфиденциальности, используя новые атрибуты метаданных классификации, появившиеся в ядре SQL. Эти метаданные затем могут использоваться в более сложных сценариях аудита и защиты на основе конфиденциальности.

- **Конфиденциальность результирующего набора запроса**

  Конфиденциальность результирующего набора запроса вычисляется в режиме реального времени для аудита.

- **Видимость**

  Состояние классификации базы данных можно просмотреть в подробной панели мониторинга на портале. Кроме того, можно скачать отчет (в формате Excel) и использовать его для соблюдения соответствия и аудита, а также для других нужд.

## <a id="subheading-2"></a>Обнаружение, классификация и добавление тегов для конфиденциальных столбцов

В следующем разделе описывается процедура обнаружения, классификации и добавления меток для столбцов с конфиденциальными данными в базе данных, а также просмотр текущего состояния классификации базы данных и экспорт отчетов.

Для классификации используются два атрибута метаданных.

- Метки — это основные атрибуты классификации, которые используется для определения уровня конфиденциальности данных, хранящихся в столбце.  
- Типы информации обеспечивают дополнительную степень детализации для типа данных, хранящихся в столбце.

## <a name="define-and-customize-your-classification-taxonomy"></a>Определение и настройка таксономии классификации

Обнаружение и классификация данных SQL поставляется со встроенным набором меток конфиденциальности, встроенным набором типов сведений и логикой обнаружения. Теперь вы можете настраивать таксономию и задавать набор и ранжирование конструкций классификации для своей среды.

Определение и настройка таксономии классификации выполняется в одном центральном расположении для всего клиента Azure. Это расположение находится в [центре безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-intro), что является частью политики безопасности. Только пользователь с правами администратора в корневой группе управления клиента может выполнить эту задачу.

В процессе управления политикой Information Protection можно определить пользовательские метки, ранжировать их и связать их с выбранным набором типов сведений. Вы также можете добавить собственные настраиваемые типы сведений и настроить их с использованием шаблонов строки, которые добавляются в логику обнаружения для идентификации этого типа данных в базах данных.
Дополнительные сведения о настройке политики и управлении ею см. в статье [Customize the SQL information protection policy in Azure Security Center (Preview)](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409) (Настройка политики защиты информации SQL в центре безопасности Azure (предварительная версия)).

После определения политики на уровне клиента можно приступить к классификации отдельных баз данных с помощью настроенной политики.

## <a name="classify-your-sql-database"></a>Классификация базы данных SQL

1. Перейдите на [портал Azure](https://portal.azure.com).

2. Перейдите к разделу **Расширенная защита данных** под заголовком "Безопасность" в области "База данных SQL Azure". Щелкните, чтобы включить службу "Расширенная защита данных", и выберите карту **Обнаружение и классификация данных (предварительная версия)** .

   ![Проверка базы данных](./media/sql-data-discovery-and-classification/data_classification.png)

3. Вкладка **Обзор** содержит сводку по текущему состоянию базы данных, включая подробный список всех классифицированных столбцов, который можно отфильтровать, чтобы просмотреть определенные части схемы, типы информации и метки. Если столбцы еще не классифицированы, [перейдите к шагу 5](#step-5).

   ![Сводка по текущему состоянию классификации](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Чтобы скачать отчет в формате Excel, щелкните параметр **Экспорт** в верхнем меню окна.

   ![Экспорт в Excel](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png)

5. <a id="step-5"></a>Чтобы начать классификацию данных, щелкните вкладку **Классификация** в верхней части окна.

    ![Классификация данных](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png)

6. Подсистема классификации проверяет базу данных на наличие столбцов, содержащих конфиденциальные данные, и выводит список **рекомендуемых классификаций столбцов**. Вот как можно просмотреть и применить рекомендации по классификации.

   - Чтобы просмотреть список рекомендуемых классификаций столбцов, щелкните панель рекомендаций в нижней части окна.

      ![Классификация данных](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png)

   - Просмотрите список рекомендаций. Чтобы принять рекомендацию для определенного столбца, установите флажок в левом столбце соответствующей строки. Можно также пометить *все рекомендации* как принятые, установив флажок в заголовке таблицы рекомендаций.

       ![Просмотр списка рекомендаций](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

   - Чтобы применить выбранные рекомендации, нажмите синюю кнопку **Accept selected recommendations** (Принять выбранные рекомендации).

      ![Применение рекомендаций](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png)

7. Вы можете также **вручную классифицировать** столбцы в качестве альтернативы или в дополнение к классификации на основе рекомендаций.

   - Щелкните **Добавить классификацию** в верхнем меню окна.

      ![Добавление классификации вручную](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png)

   - В открывшемся контекстном окне выберите схему, таблицу и столбец, предназначенный для классификации, а также тип информации и метку конфиденциальности. Затем нажмите синюю кнопку **Добавить классификацию**, расположенную в нижней части контекстного окна.

      ![Выбор столбца для классификации](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Чтобы завершить классификацию и сохранить метки столбцов базы данных с новыми метаданными классификации, щелкните **Сохранить** в верхнем меню окна.

   ![Сохранить](./media/sql-data-discovery-and-classification/10_data_classification_save.png)

## <a id="subheading-3"></a>Аудит доступа к конфиденциальным данным

Важным аспектом в концепции защиты информации является возможность контролировать доступ к конфиденциальным данным. [Аудит базы данных SQL Azure](sql-database-auditing.md) был расширен — в журнал аудита добавлено новое поле *data_sensitivity_information*, в которое записываются классификации конфиденциальности (метки) фактических данных, возвращенных запросом.

![Журнал аудита](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a id="subheading-4"></a>Управлять классификацией данных, с помощью T-SQL

Используйте T-SQL, чтобы добавить или удалить классификацию столбца или извлечь все классификации для целой базы данных.

> [!NOTE]
> При использовании T-SQL для управления метками добавляемые в столбец метки не проверяются на предмет присутствия в политике защиты информации организации (набор меток, которые отображаются в рекомендациях портала). Поэтому проверку нужно выполнять вам.

- Добавление и обновление классификации одного или нескольких столбцов: [ADD SENSITIVITY CLASSIFICATION](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- Удаление классификации одного или нескольких столбцов: [DROP SENSITIVITY CLASSIFICATION](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Просмотр всех классификаций в базе данных: [sys.sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

Также интерфейсы REST API можно использовать для программного управления классификациями. Опубликованные интерфейсы REST API поддерживают следующие операции:

- [Create или Update](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate). Создание или обновление метки конфиденциальности данного столбца.
- [Delete](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete). Удаление метки конфиденциальности данного столбца.
- [Get](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get). Получение метки конфиденциальности данного столбца.
- [List Current By Database](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase). Получение списка текущих меток конфиденциальности для конкретной базы данных.
- [Список рекомендуется по базе данных](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase) -возвращает метки рекомендуемые чувствительности заданной базы данных.

## <a name="manage-data-discovery-and-classification-using-azure-powershell"></a>Управление данных обнаружения и классификации с помощью Azure PowerShell

Чтобы получить все столбцы, рекомендуемые в базе данных Azure SQL и управляемый экземпляр можно использовать PowerShell.

### <a name="powershell-cmdlets-for-azure-sql-database"></a>Командлеты PowerShell для базы данных Azure SQL

- [Get-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)

### <a name="powershell-cmdlets-for-managed-instance"></a>Командлеты PowerShell для управляемого экземпляра

- [Get-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)

## <a name="permissions"></a>Разрешения

Следующие встроенные роли можно прочитать классификацию данных к базе данных Azure SQL: `Owner`, `Reader`, `Contributor`, `SQL Security Manager` и `User Access Administrator`.

Следующие встроенные роли можно изменить классификацию данных к базе данных Azure SQL: `Owner`, `Contributor`, `SQL Security Manager`.

Дополнительные сведения о [RBAC для ресурсов Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)

## <a id="subheading-5"></a>Дальнейшие действия

- Дополнительные сведения о [расширенной защите данных](sql-database-advanced-data-security.md).
- Рекомендуется настроить [аудит базы данных SQL Azure](sql-database-auditing.md) для мониторинга и аудита доступа к секретным конфиденциальным данным.

<!--Anchors-->
[SQL data discovery & classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Automated/Programmatic classification]: #subheading-4
[Next Steps]: #subheading-5
