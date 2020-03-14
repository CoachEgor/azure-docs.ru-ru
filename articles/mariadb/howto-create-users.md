---
title: Создание пользователей с базой данных Azure для MariaDB
description: В этой статье описывается создание учетных записей пользователей для взаимодействия с сервером базы данных Azure для MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: cbfcb097b4fda30bdeed940a5acb609b02f5d788
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79283359"
---
# <a name="create-users-in-azure-database-for-mariadb"></a>Создание пользователей в базе данных Azure для MariaDB 
В этой статье описывается создание пользователей в базе данных Azure для MariaDB.

При создании первой базы данных Azure для MariaDB вы указали имя пользователя и пароль администратора сервера, используемые для входа. Дополнительные сведения см. в [этом кратком руководстве](quickstart-create-mariadb-server-database-using-azure-portal.md). Имя пользователя администратора сервера можно найти на портале Azure.

Администратор сервера получает определенные привилегии для выполнения следующих операций на сервере: SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER.

После создания сервера базы данных Azure для MariaDB с помощью первой учетной записи администратора сервера можно создать дополнительных пользователей и предоставить им права администратора. Кроме того, учетная запись администратора сервера может использоваться для создания менее привилегированных пользователей, имеющих доступ к отдельным схемам базы данных.

## <a name="create-additional-admin-users"></a>Создание дополнительных администраторов
1. Получите сведения о подключении и имя пользователя администратора.
   Чтобы подключиться к серверу базы данных, вам потребуются учетные данные администратора для входа и полное имя сервера. Вы можете легко найти данные для входа на странице **Обзор** сервера или на странице **Свойства** на портале Azure. 

2. Используйте учетную запись и пароль администратора для подключения к серверу базы данных. Используйте предпочитаемый клиентский инструмент, например MySQL Workbench, mysql.exe, HeidiSQL и т. д. 
   Если вы не знаете, как подключиться, прочитайте раздел [База данных Azure для MySQL: подключение и запрос данных с помощью MySQL Workbench](./connect-workbench.md).

3. Измените и выполните следующий код SQL. Замените новым именем пользователя значение заполнителя `new_master_user`. Эта синтаксическая конструкция предоставляет перечисленные привилегии для всех схем базы данных ( *.* ) имени пользователя (new_master_user в этом примере). 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. Проверка разрешений 
   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="create-database-users"></a>Создание пользователей базы данных

1. Получите сведения о подключении и имя пользователя администратора.
   Чтобы подключиться к серверу базы данных, вам потребуются учетные данные администратора для входа и полное имя сервера. Вы можете легко найти данные для входа на странице **Обзор** сервера или на странице **Свойства** на портале Azure. 

2. Используйте учетную запись и пароль администратора для подключения к серверу базы данных. Используйте предпочитаемый клиентский инструмент, например MySQL Workbench, mysql.exe, HeidiSQL и т. д. 
   Если вы не знаете, как подключиться, прочитайте раздел [База данных Azure для MySQL: подключение и запрос данных с помощью MySQL Workbench](./connect-workbench.md).

3. Измените и выполните следующий код SQL. Замените значение заполнителя `db_user` новым именем пользователя, а значение заполнителя `testdb` — именем базы данных.

   Этот код SQL создает базу данных testdb для примера. Затем он создает пользователя в службе базы данных Azure для MariaDB и предоставляет ему все привилегии для новой схемы базы данных (testdb.\*). 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Проверьте предоставление привилегий в базе данных.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Войдите на сервер, указав эту базу данных и введя новое имя пользователя и пароль. В этом примере показана командная строка MySQL. После ввода этой команды вам будет предложено ввести пароль для имени пользователя. Укажите собственные имя сервера, имя базы данных и имя пользователя.

   ```bash
   mysql --host mydemoserver.mariadb.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```
   Чтобы получить дополнительные сведения об управлении учетными записями пользователей, ознакомьтесь с [управлением учетными записями пользователей](https://mariadb.com/kb/en/library/user-account-management/), [синтаксисом GRANT](https://mariadb.com/kb/en/library/grant/) и [привилегиями](https://mariadb.com/kb/en/library/grant/#privilege-levels) в документации по MariaDB.

## <a name="next-steps"></a>Дальнейшие действия
Откройте брандмауэр для IP-адресов компьютеров новых пользователей, чтобы обеспечить их подключение. Для этого ознакомьтесь с разделом [Создание правил брандмауэра базы данных Azure для MariaDB и управление ими с помощью портала Azure](howto-manage-firewall-portal.md).  

<!--or [Azure CLI](howto-manage-firewall-using-cli.md).-->
