---
title: Правила брандмауэра на уровне сервера
description: Правила брандмауэра на уровне сервера
keywords: Подключения SQL, строка подключения
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: 8d0f9899dbb7599340b8d15ca010a0157011fb9e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "67185366"
---
1. Войдите на [портал Azure](https://portal.azure.com/).

2. В меню слева выберите пункт **Все службы**.

3. Найдите и выберите **Серверы SQL Server**.

    ![Поиск своего сервера Базы данных SQL Azure на портале][b21-FindServerInPortal]
5. В текстовом поле фильтра начните вводить имя своего сервера. Отобразится строка.

6. Выберите строку со своим сервером. Отобразится колонка для вашего сервера.

7. В колонке вашего сервера выберите **Параметры**.

8. Выберите **Брандмауэр**.

    ![Пункты "Параметры" > "Брандмауэр"][b31-SettingsFirewallNavig]
9. Выберите **Добавить IP-адрес клиента**. В первое текстовое поле введите имя нового правила.

10. Введите нижнее и верхнее значения IP-адресов для требуемого диапазона.

    * Удобными могут оказаться нижнее значение, оканчивающееся на **.0**, и верхнее, оканчивающееся на **.255**.

11. Щелкните **Сохранить**.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
