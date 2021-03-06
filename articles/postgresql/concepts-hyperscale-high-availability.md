---
title: Высокая доступность — масштабирование (Цитус) — база данных Azure для PostgreSQL
description: Основные понятия высокого уровня доступности и аварийного восстановления
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 406b4f2ada665d65ee0452579e24744d1f7cfc63
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91314859"
---
# <a name="high-availability-in-azure-database-for-postgresql--hyperscale-citus"></a>Высокий уровень доступности в базе данных Azure для PostgreSQL — масштабирование (Цитус)

Высокая доступность (HA) позволяет избежать простоя базы данных за счет поддержания резервных реплик каждого узла в группе серверов. Если узел выходит из строя, то Цитус переключается на входящие подключения с сбойного узла на резервный. Отработка отказа происходит в течение нескольких минут, а повышенные узлы всегда имеют актуальные данные через синхронную потоковую репликацию PostgreSQL.

Чтобы воспользоваться преимуществами высокой доступности на узле координатора, приложения базы данных должны обнаружить и повторить удаленные соединения и неудачные транзакции. Вновь повышенный координатор будет доступен с той же строкой подключения.

Восстановление можно разделить на три этапа: обнаружение, отработка отказа и полное восстановление.  Цитус выполняет периодические проверки работоспособности на каждом узле, а после четырех неудачных проверок он определяет, что узел не работает. Затем масштабирование (Цитус) переводит состояние ожидания в первичный узел (отработка отказа) и подготавливает новую резервную копию.
Начнется репликация потоковой передачи с новым узлом в актуальном состоянии.  После репликации всех данных узел достиг полного восстановления.

### <a name="next-steps"></a>Дальнейшие шаги

- Узнайте, как [включить высокий уровень доступности](howto-hyperscale-high-availability.md) в группе серверов с горизонтальным масштабированием (Цитус).
