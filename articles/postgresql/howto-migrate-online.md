---
title: Перенос минимального времени простоя в базу данных Azure для PostgreSQL — один сервер
description: В этой статье описывается, как выполнить простой перенос базы данных PostgreSQL в базу данных Azure для PostgreSQL-Single Server с помощью Azure Database Migration Service.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 93cd390889c023adf1c30a8470e1c2298598439e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "65067512"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql---single-server"></a>Перенос минимального времени простоя в базу данных Azure для PostgreSQL — один сервер
Благодаря новой возможности **непрерывной синхронизации**, реализованной в службе [Azure Database Migration Service](https://aka.ms/get-dms) (DMS), вы можете переносить базы данных PostgreSQL в Базу данных Azure для PostgreSQL с минимальным временем простоя. Эта функция позволяет сократить время простоя при работе приложения.

## <a name="overview"></a>Обзор
Azure Database Migration Service выполняет начальную загрузку локальных данных в Базу данных Azure для PostgreSQL и непрерывно синхронизирует все новые транзакции с Azure. Все это время приложение продолжает работать. Когда данные поступят в целевую службу Azure, остановите приложение на короткое время (минимальное время простоя), подождите, пока последний пакет данных (с момента остановки приложения до его фактической недоступности для принятия нового трафика) будет передан в целевую службу, а затем обновите строку подключения, чтобы она указывала на Azure. По завершении приложение станет доступно в Azure.

![Непрерывная синхронизация с Azure Database Migration Service](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Дальнейшие действия
- Посмотрите видео [App Modernization with Microsoft Azure](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102) (Усовершенствование приложений с помощью Microsoft Azure), в котором показано, как переносить приложения PostgreSQL в Базу данных Azure для PostgreSQL.
- Ознакомьтесь с руководством [Миграция PostgreSQL в Базу данных Azure для PostgreSQL через Интернет с помощью Azure Database Migration Service](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online).
