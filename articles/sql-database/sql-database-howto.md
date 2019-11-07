---
title: Настройка базы данных SQL Azure
description: Сведения о том, как настроить базу данных SQL Azure и управлять ею.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 01/25/2019
ms.openlocfilehash: ccdeb883dc9cf2cba499e45e25ff4706bb120463
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73689876"
---
# <a name="how-to-use-azure-sql-database"></a>Использование Базы данных SQL Azure

В этой статье можно найти различные руководства, сценарии и пояснения, которые помогут в администрировании и настройке отдельной базы данных SQL Azure. Здесь также можно найти специальные руководства для [отдельной базы данных](sql-database-howto-single-database.md) и [Управляемого экземпляра](sql-database-howto-managed-instance.md).

## <a name="load-data"></a>Загрузка данных

- [Копирование отдельной базы данных или базы данных в пуле в Azure](sql-database-copy.md)
- [Импорт базы данных из BACPAC-файла](sql-database-import.md)
- [Экспорт базы данных в BACPAC-файл](sql-database-export.md)
- [Загрузка данных с помощью BCP](sql-database-load-from-csv-with-bcp.md)
- [Загрузка данных с помощью ADF](../data-factory/connector-azure-sql-database.md?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Синхронизация данных

- [Синхронизация данных SQL](sql-database-sync-data.md)
- [Агент синхронизации данных](sql-database-data-sync-agent.md)
- [Репликация изменений схемы](sql-database-update-sync-schema.md)
- [Отслеживание с помощью OMS](sql-database-sync-monitor-oms.md)
- [Рекомендации по синхронизации данных](sql-database-best-practices-data-sync.md)
- [Устранение ошибок синхронизации данных](sql-database-troubleshoot-data-sync.md)

## <a name="monitoring-and-tuning"></a>Мониторинг и настройка

- [Настройка вручную](sql-database-performance-guidance.md)
- [Использование динамических административных представлений для мониторинга производительности](sql-database-monitoring-with-dmvs.md)
- [Использование хранилища запросов для мониторинга производительности](sql-database-operate-query-store.md)
- [Устранение проблем производительности с помощью Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md)
- [Использование журнала диагностики Intelligent Insights](sql-database-intelligent-insights-use-diagnostics-log.md)
- [Мониторинг используемого пространства в хранилище выполняющейся в памяти OLTP](sql-database-in-memory-oltp-monitoring.md)

### <a name="extended-events"></a>Расширенные события

- [Расширенные события](sql-database-xevent-db-diff-from-svr.md)
- [Хранение расширенных событий в файлах событий](sql-database-xevent-code-event-file.md)
- [Хранение расширенных событий в кольцевом буфере](sql-database-xevent-code-ring-buffer.md)

## <a name="configure-features"></a>Настройка компонентов

- [Настройка аутентификации Azure AD](sql-database-aad-authentication-configure.md)
- [Настройка условного доступа](sql-database-conditional-access.md)
- [Многофакторная проверка подлинности с помощью AAD](sql-database-ssms-mfa-authentication.md)
- [Настройка многофакторной проверки подлинности](sql-database-ssms-mfa-authentication-configure.md)
- [Настройка политики временного хранения](sql-database-temporal-tables-retention-policy.md)
- [Настройка TDE и BYOK](transparent-data-encryption-byok-azure-sql-configure.md)
- [Смена ключей TDE BYOK](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- [Удаление предохранителя TDE](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)
- [Настройка In-Memory OLTP](sql-database-in-memory-oltp-migration.md)
- [Настройка службы автоматизации Azure](sql-database-manage-automation.md)

## <a name="develop-applications"></a>Разработка приложений

- [Соединение](sql-database-libraries.md)
- [Использование соединителя Spark](sql-database-spark-connector.md)
- [Аутентификация приложения](sql-database-client-id-keys.md)
- [Сообщения об ошибках](sql-database-develop-error-messages.md)
- [Использование пакетной обработки для лучшей производительности](sql-database-use-batching-to-improve-performance.md)
- [Руководство по настройке подключений](sql-database-connectivity-issues.md)
- [Псевдонимы DNS](dns-alias-overview.md)
- [Настройка DNS-псевдонимов в PowerShell](dns-alias-powershell.md)
- [Порты: ADO.NET](sql-database-develop-direct-route-ports-adonet-v12.md)
- [C и C++](sql-database-develop-cplusplus-simple.md)
- [Excel](sql-database-connect-excel.md)

## <a name="design-applications"></a>Разработка приложений

- [Разработка для аварийного восстановления](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Разработка для эластичных пулов](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
- [Разработка для обновлений приложений](sql-database-manage-application-rolling-upgrade.md)

### <a name="design-multi-tenant-saas-applications"></a>Разработка мультитенантных приложений SaaS

- [Конструктивные шаблоны SaaS](saas-tenancy-app-design-patterns.md)
- [Индексатор видео SaaS](saas-tenancy-video-index-wingtip-brk3120-20171011.md)
- [Безопасность приложений SaaS](saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)

## <a name="next-steps"></a>Дальнейшие действия

- [Использование Управляемого экземпляра](sql-database-howto-managed-instance.md).
- [Использование отдельной базы данных](sql-database-howto-single-database.md).
