---
title: Защита на уровне столбцов
description: С помощью функции безопасности на уровне столбцов (CLS) можно управлять доступом к столбцам таблицы базы данных на основе контекста выполнения пользователя или членства в группе. Безопасность на уровне столбцов (CLS) упрощает проектирование и программирование безопасности в приложении. CLS позволяет реализовать ограничения на доступ к столбцам.
services: sql-data-warehouse
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/02/2019
ms.author: jrasnick
ms.reviewer: igorstan, carlrab
ms.custom: seo-lt-2019
ms.openlocfilehash: 34172e313b537d59287a424683ea1555cbba9b99
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2019
ms.locfileid: "74049159"
---
# <a name="column-level-security"></a>Безопасность на уровне столбцов
С помощью функции безопасности на уровне столбцов (CLS) можно управлять доступом к столбцам таблицы базы данных на основе контекста выполнения пользователя или членства в группе.
Обновление на видео ниже. так как это видео было опубликовано как [безопасность на уровне строк](https://docs.microsoft.com/en-us/sql/relational-databases/security/row-level-security?toc=%2Fazure%2Fsql-data-warehouse%2Ftoc&view=sql-server-2017) , также доступно в хранилище данных SQL. 
> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]

Безопасность на уровне столбцов (CLS) упрощает проектирование и программирование безопасности в приложении. CLS позволяет реализовать ограничения на доступ к столбцам для защиты конфиденциальных данных. Например, можно предоставить конкретным пользователям доступ только к определенным столбцам таблицы, имеющей отношение к их отделу. Логика ограничения доступа находится на уровне базы данных, а не на другом уровне приложения отдельно от данных. Базы данных применяет ограничения доступа при каждой попытке получения доступа к данным независимо от уровня. Это ограничение делает систему безопасности более надежной и устойчивой за счет уменьшения ее контактной зоны. Кроме того, безопасность на уровне столбцов также позволяет отказаться от ввода представлений для фильтрации столбцов с целью наложения на пользователей ограничений доступа.

Реализовать CLS можно с помощью инструкции T-SQL [GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql). При использовании этого механизма поддерживаются проверки подлинности SQL и Azure Active Directory (AAD).

![cls](./media/column-level-security/cls.png)

## <a name="syntax"></a>Синтаксис

```sql
GRANT <permission> [ ,...n ] ON
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]
    TO <database_principal> [ ,...n ]
    [ WITH GRANT OPTION ]
    [ AS <database_principal> ]
<permission> ::=
    SELECT
  | UPDATE
<database_principal> ::=
      Database_user
    | Database_role
    | Database_user_mapped_to_Windows_User
    | Database_user_mapped_to_Windows_Group
```

## <a name="example"></a>Пример
В следующем примере показано, как ограничить доступ пользователя "TestUser" к столбцу "SSN" таблицы "Membership".

Создайте таблицу "Membership" со столбцом "SSN" для хранения номеров социального страхования.

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

Предоставьте пользователю "TestUser" доступ ко всем столбцам, за исключением столбца "SSN", который содержит конфиденциальные данные.

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

Запросы, выполняемые от имени "TestUser", завершатся ошибкой, если они содержат столбец "SSN".

```sql
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>Варианты использования
Ниже приведены некоторые примеры использования CLS в современном мире.
- В компании, предоставляющей финансовые услуги, обращаться к номерам социального страхования (SSN), номерам телефонов и другим персональным данным клиентов могут только менеджеры по работе с клиентами.
- В медицинской организации доступ к конфиденциальным медицинским записям имеют только врачи и медсестры. Сотрудники отдела выставления счетов не могут просматривать эти данные.
