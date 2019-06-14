---
title: База данных Azure для MySQL драйверы и совместимость средств управления
description: В этой статье описываются драйверы и инструменты управления MySQL, совместимые со службой "База данных Azure для MySQL".
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: 05f48145973777052590f8d10e1a2ce1fd22ec7a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60525389"
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>Совместимость драйверов и инструментов управления MySQL с базой данных Azure для MySQL.
В этой статье описываются драйверы и инструменты управления, совместимые с Базой данных Azure для MySQL.

## <a name="mysql-drivers"></a>Драйверы MySQL
База данных Azure для MySQL использует самый популярный выпуск Community Edition базы данных MySQL. Таким образом, она совместима с самыми разнообразными языками программирования и драйверами. Цель этого руководства — обеспечить поддержку трех последних версий драйверов MySQL и взаимодействие с сообществом разработчиков открытого кода, чтобы постоянно улучшать функциональность и удобство использования драйверов MySQL. Следующая таблица содержит список драйверов, которые были протестированы и которые совместимы с базой данных Azure для MySQL 5.6 и 5.7.

| **Драйвер** | **Ссылки** | **Совместимые версии** | **Несовместимые версии** | **Примечания** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5,3 | Для подключения PHP 7.0 с SSL MySQLi добавьте MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT в строке подключения. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> Набор PDO: параметр ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` имеет значение False.|
| .NET | [MySqlConnector на GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Пакет установки из Nuget](https://www.nuget.org/packages/MySqlConnector/) | Версия 0.27 и более поздние версии | Версия 0.26.5 и предыдущие версии | |
| MySQL Connector/NET | [MySQL Connector/NET](https://github.com/mysql/mysql-connector-net) | 8.0, 7.0, 6.10 |  | Ошибка кодирования может привести к сбою подключения в некоторых системах Windows, не поддерживающих UTF-8. |
| NodeJS |  [MySQLjs на GitHub](https://github.com/mysqljs/mysql/) <br> Пакет установки из NPM:<br> Запустите команду `npm install mysql` из NPM | 2.15 | Версия 2.14.1 и предыдущие версии | |
| GO | https://github.com/go-sql-driver/mysql/releases | 1.3, 1.4 | Версия 1.2 и предыдущие версии | Используйте `allowNativePasswords=true` в строке подключения для версии 1.3. Версия 1.4 содержит исправления и `allowNativePasswords=true` больше не требуется. |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | Версия 1.2.2 и предыдущие версии | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1, 2.0, 1.6 | Версия 1.5.5 и предыдущие версии | |

## <a name="management-tools"></a>Инструменты управления
Преимущества совместимости также распространяются на инструменты управления базой данных. Имеющиеся инструменты должны продолжать работать с базой данных Azure для MySQL, пока обработка базы данных выполняется в пределах разрешений пользователя. Следующая таблица содержит три распространенных инструмента управления базой данных, которые были протестированы и которые совместимы с базой данных Azure для MySQL 5.6 и 5.7.

|                                     | **MySQL Workbench 6.x и более поздней версии** | **Navicat 12** | **PHPMyAdmin 4.x и более поздней версии** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Создание, обновление, чтение, запись и удаление | X | X | X |
| SSL-соединение | X | X | X |
| Автозавершение запроса SQL | X | X |  |
| Импорт и экспорт данных | X | X | X |
| Экспорт в несколько форматов | X | X | X |
| Архивация и восстановление |  | X |  |
| Отображение параметров сервера | X | X | X |
| Отображение клиентских подключений | X | X | X |

## <a name="next-steps"></a>Дальнейшие действия

- [Узнайте, как устранить проблемы с подключением к Базе данных Azure для MySQL](howto-troubleshoot-common-connection-issues.md)