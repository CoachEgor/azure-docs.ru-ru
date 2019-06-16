---
title: Изучение журналов трассировки Java в Azure Application Insights | Документация Майкрософт
description: Поиск данных трассировки Log4J или Logback в Application Insights
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/18/2019
ms.author: mbullwin
ms.openlocfilehash: 2703c97dc78983ef294b3aa50f7ace879c96f66d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061227"
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Просмотр журналов трассировки Java в Application Insights
Если вы используете Logback или Log4J (версия 1.2 или 2.0) для трассировки, можно настроить автоматическую пересылку журналов в Application Insights, где вы сможете их изучить.

## <a name="install-the-java-sdk"></a>Установка пакета SDK для Java

Выполните инструкции по установке [пакета SDK Application Insights для Java][java], если это еще не сделано.

## <a name="add-logging-libraries-to-your-project"></a>Добавление в проект библиотеки ведения журналов
*Выберите подходящий метод для проекта.*

#### <a name="if-youre-using-maven"></a>Если вы используете Maven...
Если проект уже настроен для сборки с использованием Maven, добавьте один из следующих фрагментов кода в файл pom.xml.

Затем обновите зависимости проекта, чтобы загрузить двоичные файлы.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J версии 2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J версии 1.2*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Если вы используете Gradle...
Если проект уже настроен для сборки с использованием Gradle, добавьте одну из следующих строк в группу `dependencies` в файле build.gradle.

Затем обновите зависимости проекта, чтобы загрузить двоичные файлы.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '2.0.+'
```

**Log4J версии 2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '2.0.+'
```

**Log4J версии 1.2**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '2.0.+'
```

#### <a name="otherwise-"></a>В противном случае...
Следуйте указаниям, чтобы вручную установить пакет SDK Application Insights для Java, скачайте JAR-файл (после перехода на страницу центра Maven щелкните ссылку JAR в разделе загрузок) для соответствующего аппендера и добавьте скачанный JAR-файл аппендера в проект.

| Средство ведения журнала | Download (Скачать) | Библиотека |
| --- | --- | --- |
| Logback |[JAR-файл аппендера Logback](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-logback%22) |applicationinsights-logging-logback |
| Log4J версии 2.0 |[JAR-файл аппендера Log4J версии 2](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j2%22) |applicationinsights-logging-log4j2 |
| Log4J версии 1.2 |[JAR-файл аппендера Log4J версии 1.2](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j1_2%22) |applicationinsights-logging-log4j1_2 |


## <a name="add-the-appender-to-your-logging-framework"></a>Добавление аппендера в платформу ведения журнала
Чтобы начать трассировку, добавьте соответствующий фрагмент кода в файл конфигурации Log4J или Logback: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
        <instrumentationKey>[APPLICATION_INSIGHTS_KEY]</instrumentationKey>
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J версии 2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.log4j.v2">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" instrumentationKey="[APPLICATION_INSIGHTS_KEY]" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J версии 1.2*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
        <param name="instrumentationKey" value="[APPLICATION_INSIGHTS_KEY]" />
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

К аппендерам Application Insights может обращаться любое сконфигурированное средство ведения журнала, а не только корневое средство ведения журнала (как показано в примерах выше).

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Просмотр данных трассировки на портале Application Insights
После настройки проекта для передачи данных трассировки в Application Insights можно выполнять поиск и просмотр этих данных на портале Application Insights в колонке [Поиск][diagnostic].

Исключения, отправленные с помощью средств ведения журнала, будут отображаться на портале в разделе телеметрии исключений.

![На портале Application Insights откройте колонку "Поиск".](./media/java-trace-logs/01-diagnostics.png)

## <a name="next-steps"></a>Дальнейшие действия
[Поиск по журналу диагностики][diagnostic]

<!--Link references-->

[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[java]: java-get-started.md


