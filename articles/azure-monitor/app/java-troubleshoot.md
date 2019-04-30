---
title: Устранение неполадок Application Insights в веб-проекте Java
description: Руководство по устранению неполадок — мониторинг динамических приложений Java с помощью Application Insights.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/14/2018
ms.author: mbullwin
ms.openlocfilehash: eaade5f9ec9db7e8d224305147dafc264916d9c5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60899505"
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Устранение неполадок, а также вопросы и ответы по Application Insights для Java
Возникли вопросы или проблемы при использовании [Azure Application Insights в Java][java]? Ниже приведен ряд советов.

## <a name="build-errors"></a>Ошибки сборки
**При добавлении пакета SDK для Application Insights посредством Maven или Gradle в Eclipse или Intellij Idea возникают ошибки проверки сборки или контрольной суммы.**

* Если в элементе `<version>` зависимости используется шаблон с подстановочными знаками (например, (Maven) `<version>[2.0,)</version>` или (Gradle) `version:'2.0.+'`), попробуйте указать конкретную версию, например `2.0.1`. Сведения о последней версии см. в [заметках о выпуске](https://github.com/Microsoft/ApplicationInsights-Java/releases).

## <a name="no-data"></a>Нет данных
**Мне удалось успешно добавить Application Insights и запустить приложение, но данные не появились на портале.**

* Подождите минуту и нажмите «Обновить». Диаграммы автоматически обновляются через определенные интервалы, однако их можно обновлять и вручную. Интервал обновления зависит от временного диапазона, выбранного для диаграммы.
* Убедитесь, что ключ инструментирования указан в файле ApplicationInsights.xml (в папке ресурсов проекта) или настроен в переменной среды.
* Убедитесь в том, что в XML-файле нет узла `<DisableTelemetry>true</DisableTelemetry>`.
* В вашем брандмауэре вам может потребоваться открыть TCP-порты 80 и 443 для исходящего трафика, идущего на сайт dc.services.visualstudio.com. Ознакомьтесь с [полным списком исключений брандмауэра](../../azure-monitor/app/ip-addresses.md).
* На начальном экране Microsoft Azure посмотрите на карту состояния службы. Если есть какие-либо предупреждения, дождитесь возвращения всех модулей в состояние ОК, затем закройте и заново откройте модуль приложения Application Insights.
* Включите ведение журнала в окне консоли IDE, добавив элемент `<SDKLogger />` в корневой узел в файле ApplicationInsights.xml (в папке ресурсов проекта), и проверьте наличие записей, начинающихся с AI: INFO/WARN/ERROR во всех подозрительных журналах.
* Убедитесь, что правильный файл ApplicationInsights.xml успешно загружен с помощью пакета SDK для Java. В случае успешной загрузки на консоли в разделе сообщений вывода отображается: «Файл конфигурации успешно найден».
* Если файл конфигурации не найден, просмотрите сообщения вывода, чтобы узнать, где происходил поиск файла конфигурации, и убедитесь, что файл ApplicationInsights.xml находится в одном из этих расположений. Как правило, файл конфигурации следует размещать вместе с JAR-файлами пакета SDK для Application Insights. Например, в Tomcat это папка WEB-INF/classes. На период разработки можно поместить ApplicationInsights.xml в папку resources веб-проекта.
* Также проверьте, нет ли на [странице проблем на GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) известных проблем с пакетом SDK.
* Убедитесь, что используются одинаковые версии ядра Application Insights и аппендеров web, агента и ведения журнала, чтобы исключить проблемы с конфликтом версий.

#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Ранее видимые данные перестали отображаться
* Проверьте [блог состояний](https://blogs.msdn.com/b/applicationinsights-status/).
* Вы достигли месячной квоты точек данных? Чтобы выяснить это, см. разделы «Параметры», «Квота» и «Расценки». Если вы достигли квоты, вы можете изменить свой тарифный план или заплатить за дополнительную емкость. См. [таблицу цен](https://azure.microsoft.com/pricing/details/application-insights/).
* Возможно, вы недавно обновляли SDK? Убедитесь, что в каталоге проекта присутствуют только уникальные JAR-файлы пакета SDK. Не должно существовать двух разных версий одного пакета SDK.
* Правильный ли ресурс AI вы изучаете? Проверьте параметр iKey в приложении и в ресурсе, в который вы передаете телеметрию. Эти значения должны совпадать.

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Не отображаются все данные, которые ожидалось увидеть
* Откройте страницу данных об использовании и предполагаемых расходов и проверьте, не выполняется ли операция [выборки](../../azure-monitor/app/sampling.md). (100 % передачи означает, что выборка не выполняется.) Службу Application Insights можно настроить на прием только части данных телеметрии, поступающих из приложения. Это помогает не выходить за пределы месячной квоты.
* Включена ли выборка SDK? Если включена,то для выборки данных будет применяться частота, настроенная для всех применимых типов.
* Возможно, выполняется старая версия пакета SDK для Java? Начиная с версии 2.0.1 мы внедрили механизм отказоустойчивости, который обрабатывает временные ошибки в сети и на серверной части, а также сохраняет данные на локальных дисках.
* Возможно, сработало регулирование из-за слишком высокой нагрузки на телеметрию? Включив ведение журнала INFO, вы увидите сообщение "Приложение регулируется". В настоящее время действует ограничение 32 000 элементов телеметрии в секунду.

### <a name="java-agent-cannot-capture-dependency-data"></a>Агент Java не может собрать данные зависимостей
* При настройке агента Java вы соблюдали все инструкции [из этой статьи](java-agent.md)?
* Убедитесь, что JAR-файл агента и файл AI-Agent.xml размещаются в одной папке.
* Убедитесь, что поддерживается автоматический сбор для зависимости, которую вы намерены собирать. Сейчас поддерживаются только такие коллекции зависимостей: MySQL, MsSQL, Oracle DB и кэш Redis для Azure.
* Ваш пакет JDK имеет версию 1.7 и 1.8? В настоящее время мы не поддерживаем сбор зависимостей в JDK 9.

## <a name="no-usage-data"></a>Нет данных по использованию
**Приводятся данные по запросам и времени ответа, но нет данных по просмотру страниц, использованию браузеров и пользователям.**

Вы успешно настроили приложение для отправки данных телеметрии с сервера. Теперь вам нужно [настроить веб-страницы для отправки данных телеметрии из веб-браузера][usage].

Если клиент является приложением на [телефоне или другом устройстве][platforms], из него также можно отправлять данные телеметрии.

Для настройки телеметрии в клиенте и на сервере используйте один и тот же ключ инструментирования. Данные будут приводиться в одном и том же ресурсе Application Insights, и вы сможете сопоставлять события, полученные от клиента и с сервера.


## <a name="disabling-telemetry"></a>Отключение телеметрии
**Как отключить сбор данных телеметрии?**

В коде:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Или**

Измените файл ApplicationInsights.xml (в папке ресурсов проекта). Добавьте следующий элемент в корневой узел:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

При изменении значения в файле XML необходимо перезапустить приложение.

## <a name="changing-the-target"></a>Изменение целевого ресурса
**Как изменить ресурс Azure, в который проект отправляет данные?**

* [Получите ключ инструментирования нового ресурса.][java]
* Если вы добавили Application Insights в проект с помощью набора средств Azure для Eclipse, щелкните веб-проект правой кнопкой мыши, выберите **Azure**, а затем выберите **Настроить Application Insights** и измените ключ.
* Если ключ инструментирования настроен в переменной среды, внесите в эту переменную новое значение iKey.
* В противном случае измените ключ в файле ApplicationInsights.xml в папке ресурсов проекта.

## <a name="debug-data-from-the-sdk"></a>Отладка данных из пакета SDK

**Как определить, что делает пакет SDK?**

Чтобы получить дополнительные сведения о работе API, добавьте `<SDKLogger/>` в корневой узел файла конфигурации ApplicationInsights.xml.

### <a name="applicationinsightsxml"></a>ApplicationInsights.xml

Можно также указать средству ведения журнала выводить данные в файл.

```XML
  <SDKLogger type="FILE">
    <Level>TRACE</Level>
    <UniquePrefix>AI</UniquePrefix>
    <BaseFolderPath>C:/agent/AISDK</BaseFolderPath>
</SDKLogger>
```

### <a name="spring-boot-starter"></a>Начальное приложение Spring Boot

Чтобы включить ведение журнала пакета SDK с приложениями Spring Boot, с помощью начального приложения Spring Boot Application Insights, добавьте следующий код в `application.properties` файла.:

```yaml
azure.application-insights.logger.type=file
azure.application-insights.logger.base-folder-path=C:/agent/AISDK
azure.application-insights.logger.level=trace
```

### <a name="java-agent"></a>Агент для Java

Чтобы включить ведение журнала агента виртуальной машины Java обновление [файл AI-Agent.xml](java-agent.md).

```xml
<AgentLogger type="FILE">
    <Level>TRACE</Level>
    <UniquePrefix>AI</UniquePrefix>
    <BaseFolderPath>C:/agent/AIAGENT</BaseFolderPath>
</AgentLogger>
```

## <a name="the-azure-start-screen"></a>Начальный экран Azure
**Я просматриваю [портал Azure](https://portal.azure.com). Скажет ли карта что-нибудь о приложении?**

Нет, она показывает работоспособность серверов Azure по всему миру.

*Как найти данные о приложении на начальной доске (начальном экране) Azure?*

Если вы [настроили приложение для использования Application Insights][java], нажмите кнопку "Обзор", выберите Application Insights, а затем выберите ресурс приложения, который вы создали для приложения. Чтобы упростить эту операцию в будущем, можно закрепить приложение на начальном экране.

## <a name="intranet-servers"></a>Серверы в интрасети
**Можно ли наблюдать за сервером в интрасети?**

Да, если ваш сервер может отправлять данные телеметрии на портал Application Insights через общедоступную сеть Интернет.

В вашем брандмауэре вам может потребоваться открыть TCP-порты 80 и 443 для исходящего трафика, идущего на сайты dc.services.visualstudio.com и f5.services.visualstudio.com.

## <a name="data-retention"></a>Хранение данных
**Как долго данные хранятся на портале? Защищен ли он?**

Ознакомьтесь с разделом [Хранение данных и конфиденциальность][data].

## <a name="debug-logging"></a>Ведение журнала отладки
Application Insights использует модуль `org.apache.http`. Он перемещен в JAR-файлы ядра Application Insights в пространство имен `com.microsoft.applicationinsights.core.dependencies.http`. Это позволяет Application Insights обрабатывать ситуации, в которых разные версии `org.apache.http` существуют в одной базе кода.

>[!NOTE]
>Если включить уровень ведения журнала "Отладка" для всех пространств имен в приложении, он будет действовать для всех выполняемых модулей, включая `org.apache.http`, который был переименован в `com.microsoft.applicationinsights.core.dependencies.http`. Application Insights не сможет применять фильтрацию этих вызовов, так как вызов журнала выполняется библиотекой Apache. При уровне ведения журнала "Отладка" создается значительный объем данных журнала, и он не рекомендуется для экземпляров, выполняемых в рабочей среде.


## <a name="next-steps"></a>Дальнейшие действия
**Служба Application Insights настроена для серверного приложения Java. Что еще можно сделать?**

* [Отслеживать доступность веб-страниц][availability]
* [Отслеживать использование веб-страниц][usage]
* [Отслеживать использование и диагностировать проблемы в приложениях для устройств][platforms]
* [Написать код для отслеживания использования приложения][track]
* [Записать журналы диагностики][javalogs]

## <a name="get-help"></a>Получение справки
* [Stack Overflow](https://stackoverflow.com/questions/tagged/ms-application-insights)
* [Сообщите о проблеме на GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues)

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[data]: ../../azure-monitor/app/data-retention-privacy.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[platforms]: ../../azure-monitor/app/platforms.md
[track]: ../../azure-monitor/app/api-custom-events-metrics.md
[usage]: javascript.md

