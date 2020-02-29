---
title: Обнаружение и классификация данных
description: Классификация & базы данных и обнаружения данных SQL Azure
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
titleSuffix: Azure SQL Database and Azure Synapse
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 02/05/2020
tags: azure-synapse
ms.openlocfilehash: e22205e81178ac0caff4b71462ece776238900f6
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191951"
---
# <a name="azure-sql-database-and-azure-synapse-analytics-data-discovery--classification"></a>Классификация & базы данных SQL Azure и аналитики Azure синапсе Analytics

Классификация & обнаружения данных предоставляет расширенные возможности, встроенные в базу данных SQL Azure для **обнаружения**, **классификации**и **маркировки** & **создания отчетов о** конфиденциальных данных в базах данных.

Обнаружение и классификация наиболее конфиденциальных данных (деловых или финансовых, медицинских, персональных данных и т. д.) может играть ключевую роль в концепции защиты информации вашей организации. На основе этих процессов может формироваться инфраструктура для решения следующих задач:

- Соблюдение стандартов конфиденциальности данных и нормативных требований.
- Различные сценарии безопасности, такие как мониторинг (аудит) и оповещение об аномальном доступе к конфиденциальным данным.
- Управление доступом к базам данных, содержащим конфиденциальные данные, и усиление их защиты.

Классификация & обнаружения данных является частью предложения [расширенной системы безопасности данных](sql-database-advanced-data-security.md) (ADS), которое является единым пакетом для расширенных возможностей обеспечения безопасности SQL. Доступ к этой службе можно получить на центральном портале SQL ADS. Там же можете управлять ею.

> [!NOTE]
> Этот документ относится к базе данных SQL Azure и Azure синапсе. Для простоты база данных SQL используется при обращении к базе данных SQL и Azure синапсе. Сведения о SQL Server (в локальной среде) см. в разделе [Обнаружение и классификация данных SQL](https://go.microsoft.com/fwlink/?linkid=866999).

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

В следующем разделе описываются действия по обнаружению в базе данных столбцов, содержащих конфиденциальные данные, их классификации и назначению им меток, а также просмотру текущего состояния классификации базы данных и экспорту отчетов.

Классификация включает в себя два типа атрибутов метаданных:

- метки — основные атрибуты классификации, которые служат для определения уровня конфиденциальности данных, хранящихся в столбце;  
- типы сведений — предоставляют более подробные сведения о типе данных, хранящихся в столбце.

## <a name="define-and-customize-your-classification-taxonomy"></a>Определение и настройка таксономии классификации

Обнаружение и классификация данных SQL поставляется со встроенным набором меток конфиденциальности, встроенным набором типов сведений и логикой обнаружения. Теперь вы можете настраивать таксономию и задавать набор и ранжирование конструкций классификации для своей среды.

Определение и настройка таксономии классификации выполняется в одном центральном расположении для всего клиента Azure. Это расположение находится в [центре безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-intro), что является частью политики безопасности. Только пользователь с правами администратора в корневой группе управления клиента может выполнить эту задачу.

В процессе управления политикой Information Protection можно определить пользовательские метки, ранжировать их и связать их с выбранным набором типов сведений. Вы также можете добавить собственные настраиваемые типы сведений и настроить их с использованием шаблонов строки, которые добавляются в логику обнаружения для идентификации этого типа данных в базах данных.
Дополнительные сведения о настройке политики и управлении ею см. в статье [Customize the SQL information protection policy in Azure Security Center (Preview)](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409) (Настройка политики защиты информации SQL в центре безопасности Azure (предварительная версия)).

После определения политики на уровне клиента можно приступить к классификации отдельных баз данных с помощью настроенной политики.

## <a name="classify-your-sql-database"></a>Классификация базы данных SQL

1. Перейдите на [портал Azure](https://portal.azure.com).

2. Перейдите к разделу **Расширенная защита данных** под заголовком "Безопасность" в области "База данных SQL Azure". Щелкните, чтобы включить расширенную защиту данных, а затем щелкните карту **& классификации данных** .

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

   - Просмотрите список рекомендаций. Чтобы принять рекомендацию для определенного столбца, установите флажок в левом столбце соответствующей строки. Вы также можете принять *все рекомендации*, установив флажок в заголовке таблицы рекомендаций.

       ![Просмотр списка рекомендаций](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

   - Чтобы применить выбранные рекомендации, нажмите синюю кнопку **Принять выбранные рекомендации**.

      ![Применение рекомендаций](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png)

7. Вы также можете **классифицировать столбцы вручную** вместо использования рекомендованных классификаций или в дополнение к ним.

   - В меню в верхней части страницы щелкните **Добавить классификацию**.

      ![Добавление классификации вручную](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png)

   - В открывшемся контекстном окне выберите схему, таблицу и столбец, которые нужно классифицировать, а затем выберите тип сведений и метку классификации. После этого нажмите синюю кнопку **Добавить классификацию** в нижней части контекстного окна.

      ![Выбор столбца для классификации](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Чтобы завершить классификацию и назначить столбцам базы данных новые метаданные (метки) классификации, щелкните элемент **Сохранить** в меню в верхней части окна.

   ![Сохранять](./media/sql-data-discovery-and-classification/10_data_classification_save.png)

## <a id="subheading-3"></a>Аудит доступа к конфиденциальным данным

Важным аспектом в концепции защиты информации является возможность контролировать доступ к конфиденциальным данным. [Аудит базы данных SQL Azure](sql-database-auditing.md) был расширен — в журнал аудита добавлено новое поле *data_sensitivity_information*, в которое записываются классификации конфиденциальности (метки) фактических данных, возвращенных запросом.

![Журнал аудита](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a id="subheading-4"></a>Permissions

Следующие встроенные роли могут считывать классификацию данных базы данных SQL Azure: `Owner`, `Reader`, `Contributor`, `SQL Security Manager` и `User Access Administrator`.

Следующие встроенные роли могут изменять классификацию данных базы данных SQL Azure: `Owner`, `Contributor``SQL Security Manager`.

Дополнительные сведения о [RBAC для ресурсов Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)

## <a id="subheading-5"></a>Управление классификациями

# <a name="t-sql"></a>[T-SQL](#tab/azure-t-sql)
Используйте T-SQL, чтобы добавить или удалить классификацию столбца или извлечь все классификации для целой базы данных.

> [!NOTE]
> При использовании T-SQL для управления метками добавляемые в столбец метки не проверяются на предмет присутствия в политике защиты информации организации (набор меток, которые отображаются в рекомендациях портала). Поэтому проверку нужно выполнять вам.

- Добавление и обновление классификации одного или нескольких столбцов: [ADD SENSITIVITY CLASSIFICATION](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- Удаление классификации одного или нескольких столбцов: [DROP SENSITIVITY CLASSIFICATION](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Просмотр всех классификаций в базе данных: [sys.sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

# <a name="rest-apis"></a>[Интерфейсы API-интерфейсов RESTful](#tab/azure-rest-api)
Для программного управления классификациями и рекомендациями можно использовать интерфейсы API. Опубликованные интерфейсы REST API поддерживают следующие операции:

- [CREATE или Update](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate) — создает или обновляет метку чувствительности данного столбца.
- [Delete](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete). Удаление метки конфиденциальности данного столбца.
- [Disable рекомендация](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/disablerecommendation) — отключает рекомендации по конфиденциальности для данного столбца
- [Включить рекомендацию](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/enablerecommendation) — включает рекомендации по конфиденциальности для данного столбца (рекомендации включены по умолчанию для всех столбцов).
- [Get](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get). Получение метки конфиденциальности данного столбца.
- [List Current By Database](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase). Получение списка текущих меток конфиденциальности для конкретной базы данных.
- [List, рекомендованный базой данных](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase) — получение рекомендуемых меток чувствительности для заданной базы данных.

# <a name="powershell-cmdlet"></a>[Командлет PowerShell](#tab/azure-powelshell)
PowerShell можно использовать для управления классификациями и рекомендациями для базы данных SQL Azure и Управляемый экземпляр.

### <a name="powershell-cmdlet-for-azure-sql-database"></a>Командлет PowerShell для базы данных SQL Azure
- [Get-Азсклдатабасесенситивитиклассификатион](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-Азсклдатабасесенситивитиклассификатион](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-Азсклдатабасесенситивитиклассификатион](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-Азсклдатабасесенситивитирекоммендатион](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)
- [Enable-Азсклдатабасесенситивитирекоммендатион](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqldatabasesensitivityrecommendation)
- [Disable-Азсклдатабасесенситивитирекоммендатион](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqldatabasesensitivityrecommendation)

### <a name="powershell-cmdlets-for-managed-instance"></a>Командлеты PowerShell для Управляемый экземпляр
- [Get-Азсклинстанцедатабасесенситивитиклассификатион](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-Азсклинстанцедатабасесенситивитиклассификатион](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-Азсклинстанцедатабасесенситивитиклассификатион](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-Азсклинстанцедатабасесенситивитирекоммендатион](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)
- [Enable-Азсклинстанцедатабасесенситивитирекоммендатион](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqlinstancedatabasesensitivityrecommendation)
- [Disable-Азсклинстанцедатабасесенситивитирекоммендатион](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqlinstancedatabasesensitivityrecommendation)

---

## <a id="subheading-6"></a>Следующие шаги

- Дополнительные сведения о [расширенной защите данных](sql-database-advanced-data-security.md).
- Рекомендуется настроить [аудит базы данных SQL Azure](sql-database-auditing.md) для мониторинга и аудита доступа к секретным конфиденциальным данным.
- Для презентаций YouTube, включающих & классификацию данных, см. статью обнаружение [, классификация, маркировка & Защита данных SQL | Предоставленные данные](https://www.youtube.com/watch?v=itVi9bkJUNc).

<!--Anchors-->
[What is data discovery & classification]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Permissions]: #subheading-4
[Manage classifications]: #subheading-5
[Next Steps]: #subheading-6
