---
title: Автоматический сбор зависимостей в Azure Application Insights | Документация Майкрософт
description: Автоматический сбор и визуализация зависимостей Application Insights
services: application-insights
documentationcenter: .net
author: nikmd23
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 04/29/2019
ms.reviewer: mbullwin
ms.author: nimolnar
ms.openlocfilehash: 832f927f81b57fd16c202b855d8f1dbe0617ad56
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65149896"
---
# <a name="dependency-auto-collection"></a>Автоматический сбор зависимостей

Ниже приведен список поддерживаемых в настоящее время вызовов зависимостей, которые автоматически определяются как зависимости без каких-либо дополнительных изменений в код приложения. Сюда входят исходящие вызовы к библиотекам связи, клиентам хранилища, библиотекам ведения журнала и метрик, а также входящие вызовы к серверам и платформам приложений. Эти зависимости отображаются в представлениях [схемы приложений](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) и [диагностики транзакций](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics) Application Insights. Если зависимость отсутствует в списке ниже, можно по-прежнему отслеживать ее самостоятельно с помощью [отслеживания вызова зависимости](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency).

## <a name="net"></a>.NET

| Платформы приложений| Версии |
| ------------------------|----------|
| Веб-формы ASP.NET | 4.5+ |
| ASP.NET MVC 3 | 4+ |
| ASP.NET WebAPI | 4.5+ |
| ASP.NET Core | 1.1+ |
| <b>Библиотеки обмена данными</b> |
| [HTTPClient](https://www.microsoft.com/net/) | 4.5+, .NET Core 1.1+ |
| [SqlClient](https://www.nuget.org/packages/System.Data.SqlClient) | .NET Core 1.0+, NuGet 4.3.0 |
| [Пакет SDK для клиента концентраторов событий](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1.1.0 |
| [Пакет SDK для клиента служебной шины](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) | 3.0.0 |
| <b>Клиенты хранилища</b>|  |
| ADO.NET | 4.5+ |
| <b>Библиотеки ведения журналов</b> |  |
| ILogger | 1.1+ |
| System.Diagnostics.Trace | 4.5+ |
| [nLog](https://www.nuget.org/packages/NLog/) | 4.4.12+ |
| [log4net](https://www.nuget.org/packages/log4net/) | 2.0.8+ на NetStandard 1.3, 2.0.6+ на платформе .NET 4.5+ |

## <a name="java"></a>Java
| Серверы приложений | Версии |
|-------------|----------|
| [Tomcat](https://tomcat.apache.org/) | 7, 8 | 
| [JBoss EAP](https://developers.redhat.com/products/eap/download/) | 6, 7 |
| [Jetty](https://www.eclipse.org/jetty/) | 9 |
| <b>Платформы приложений</b> |  |
| [Spring](https://spring.io/) | 3.0 |
| [Spring Boot](https://spring.io/projects/spring-boot) | 1.5.9+<sup>*</sup> |
| Сервлет Java | 3.1+ |
| <b>Библиотеки обмена данными</b> |  |
| [HTTP-клиент Apache](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) | 4.3+<sup>†</sup> |
| <b>Клиенты хранилища</b> | |
| [SQL Server]( https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc) | 1+<sup>†</sup> |
| [PostgreSQL (поддержка бета-версия)](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/CHANGELOG.md#version-240-beta) | |
| [Oracle]( https://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html) | 1+<sup>†</sup> |
| [MySql]( https://mvnrepository.com/artifact/mysql/mysql-connector-java) | 1+<sup>†</sup> |
| <b>Библиотеки ведения журналов</b> | |
| [Logback](https://logback.qos.ch/) | 1+ |
| [Log4j](https://logging.apache.org/log4j/) | 1.2+ |
| <b>Метрики библиотек</b> |  |
| JMX | 1.0+ |

> [!NOTE]
> *За исключением поддержки программирования.
> <br>†Требуется установка [агента виртуальной машины Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-agent#install-the-application-insights-agent-for-java).

## <a name="nodejs"></a>Node.js

| Библиотеки обмена данными | Версии |
| ------------------------|----------|
| [HTTP](https://nodejs.org/api/http.html), [HTTPS](https://nodejs.org/api/https.html) | 0.10+ |
| <b>Клиенты хранилища</b> | |
| [Redis](https://www.npmjs.com/package/redis) | 2.x |
| [MongoDb](https://www.npmjs.com/package/mongodb); [MongoDb Core](https://www.npmjs.com/package/mongodb-core) | 2.x - 3.x |
| [MySQL](https://www.npmjs.com/package/mysql) | 2.0.0 - 2.16.x |
| [PostgreSql](https://www.npmjs.com/package/pg); | 6.x - 7.x |
| [PG-пул](https://www.npmjs.com/package/pg-pool) | 1.x - 2.x |
| <b>Библиотеки ведения журналов</b> | |
| [консоль](https://nodejs.org/api/console.html) | 0.10+ |
| [Bunyan](https://www.npmjs.com/package/bunyan) | 1.x |
| [Winston](https://www.npmjs.com/package/winston) | 2.x - 3.x |

## <a name="javascript"></a>JavaScript

| Библиотеки обмена данными | Версии |
| ------------------------|----------|
| [XMLHttpRequest](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) | Все |

## <a name="next-steps"></a>Дальнейшие действия

- Настройка отслеживания зависимостей для платформы [.NET](../../azure-monitor/app/asp-net-dependencies.md).
- Настройка отслеживания зависимостей для [Java](../../azure-monitor/app/java-agent.md).
- [Создание телеметрии настраиваемых зависимостей](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- В [этой статье](../../azure-monitor/app/data-model.md) представлены типы данных и модель данных для Application Insights.
- Ознакомление с [платформами](../../azure-monitor/app/platforms.md), поддерживаемыми Application Insights.
