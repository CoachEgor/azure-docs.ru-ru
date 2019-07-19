---
title: Анализ веб-приложений Java с помощью Azure Application Insights | Документация Майкрософт
description: 'Сведения о мониторинге производительности веб-приложений Java с помощью Application Insights. '
services: application-insights
documentationcenter: java
author: lgayhardt
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: lagayhar
ms.openlocfilehash: d3edfa1ca63560f447d2c9ea3da3588e069b7af1
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68226827"
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Приступая к работе с Application Insights в веб-проекте Java

[Application Insights](https://azure.microsoft.com/services/application-insights/) — это расширяемая служба аналитики для разработчиков веб-ресурсов, позволяющая оценивать производительность и использование работающего приложения. Используйте его для [автоматического инструментирования запросов, мониторинга зависимостей и сбора счетчиков производительности](auto-collect-dependencies.md#java), диагностики проблем производительности и исключений, а также для [написания кода][api] , который позволяет отслеживать действия пользователей с приложением. 

![Снимок экрана: обзор с примером данных](./media/java-get-started/overview-graphs.png)

Надстройка Application Insights поддерживает приложения Java, работающие под управлением Linux, Unix или Windows.

Вам необходимы:

* JRE версии 1.7 или 1.8;
* подписка на [Microsoft Azure](https://azure.microsoft.com/).

Если вы предпочитаете платформу Spring, попробуйте [настроить Application Insights в приложении-инициализаторе Spring Boot](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights).

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Получение ключа инструментирования Application Insights
1. Войдите на [портал Microsoft Azure](https://portal.azure.com).
2. Создайте ресурс Application Insights. Задайте тип приложения: веб-приложение Java.

3. Найдите ключ инструментирования нового ресурса. Далее будет необходимо вставить его в проект кода.

    ![В обзоре нового ресурса щелкните "Свойства" и скопируйте ключ инструментирования](./media/java-get-started/instrumentation-key-001.png)

## <a name="2-add-the-application-insights-sdk-for-java-to-your-project"></a>2. Добавление в проект пакета SDK Application Insights для Java
*Выберите подходящий метод для проекта.*

#### <a name="if-youre-using-maven-a-namemaven-setup-"></a>Если вы используете Maven<a name="maven-setup" />
Если проект уже настроен для сборки с использованием Maven, добавьте следующий код в файл pom.xml.

Затем обновите зависимости проекта, чтобы скачать двоичные файлы.

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[2.0,)</version>
      </dependency>
    </dependencies>
```

* *Ошибки проверки сборки или контрольной суммы?* Попробуйте указать конкретную версию, например `<version>2.0.n</version>`. Сведения о последней версии см. в [заметках о выпуске пакета SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) или в [артефактах репозитория Maven](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Требуется обновить пакет SDK до новой версии?* Обновите зависимости проекта.

#### <a name="if-youre-using-gradle-a-namegradle-setup-"></a>Если вы используете Gradle<a name="gradle-setup" />
Если проект уже настроен для сборки с использованием Gradle, добавьте следующий фрагмент кода в файл build.gradle.

Затем обновите зависимости проекта, чтобы скачать двоичные файлы.

```gradle

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '2.+'
      // or applicationinsights-core for bare API
    }
```

#### <a name="if-youre-using-eclipse-to-create-a-dynamic-web-project-"></a>Если вы используете Eclipse для создания динамического веб-проекта
Используйте [Application Insights SDK для подключаемого модуля Java][eclipse]. Примечание. Хотя с помощью этого подключаемого модуля и Application Insights настройка и запуск выполняются быстрее (при условии, что вы не используете Maven или Gradle), это не система управления зависимостями. Таким образом, при обновлении подключаемого модуля библиотеки Application Insights в проекте не будут обновляться автоматически.

* *Ошибки проверки сборки или контрольной суммы?* Попробуйте указать конкретную версию, например `version:'2.0.n'`. Сведения о последней версии см. в [заметках о выпуске пакета SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) или в [артефактах репозитория Maven](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Для обновления до последней версии пакета SDK* обновите зависимости проекта.

#### <a name="otherwise-if-you-are-manually-managing-dependencies-"></a>Если вы вручную управляете зависимостями
Скачайте [последнюю версию](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest) и скопируйте необходимые файлы в проект, заменив все предыдущие версии.

### <a name="questions"></a>Вопросы
* *Каковы отношения между компонентами `-core` и `-web`?*
  * `applicationinsights-core` предоставляет чистый API. Этот компонент требуется всегда.
  * Компонент `applicationinsights-web` предоставляет метрики для отслеживания количества запросов HTTP и значений времени ответа. Его можно опустить, если автоматический сбор данных телеметрии не требуется, например, если вы хотите написать собственный код сбора данных.
  
* *Как обновить пакет SDK до последней версии?*
  * Если вы используете Gradle или Maven
    * Обновите файл сборки, чтобы указать последнюю версию либо используйте синтаксис подстановочных знаков Gradle или Maven, чтобы автоматически включить последнюю версию. Затем обновите зависимости проекта. Синтаксис подстановочных знаков можно найти в приведенных выше примерах [Gradle](#gradle-setup) или [Maven](#maven-setup).
  * Если вы вручную управляете зависимостями
    * Загрузите последнюю версию [пакета SDK Application Insights для Java](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest) и установите ее вместо более старых версий. Изменения описаны в статье [Заметки о выпуске пакета SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).

## <a name="3-add-an-applicationinsightsxml-file"></a>3. Добавление файла ApplicationInsights.xml
Добавьте ApplicationInsights.xml в папку ресурсов проекта или проверьте, добавлен ли этот файл в путь класса развертывания проекта. Скопируйте в него следующий код XML.

Замените ключ инструментирования на полученный в портале Azure.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->
      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->
      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

При необходимости файл конфигурации может находиться в любом месте, доступном для приложения.  Системное свойство `-Dapplicationinsights.configurationDirectory` указывает на каталог, содержащий файл ApplicationInsights.xml. Например, файл конфигурации, расположенный в каталоге `E:\myconfigs\appinsights\ApplicationInsights.xml`, будет настроен со свойством `-Dapplicationinsights.configurationDirectory="E:\myconfigs\appinsights"`.

* Ключ инструментирования пересылается вместе с каждым элементом телеметрии; служба Application Insights отобразит его в ресурсе.
* Компонент HTTP-запросов является необязательным. Он автоматически передает на портал телеметрию о запросах и значения времени ответа.
* Корреляция события является дополнением к компоненту HTTP-запросов. Это дополнение назначает идентификатор для каждого запроса, полученного сервером, и добавляет его в качестве свойства каждого элемента телеметрии в форме "Операция.ИД". Благодаря этому можно выделить данные телеметрии, связанные с каждым из запросов, путем установки фильтра [Поиск по журналу диагностики][diagnostic].

### <a name="alternative-ways-to-set-the-instrumentation-key"></a>Другие способы задать ключ инструментирования
Пакет SDK Application Insights ищет ключ в следующем порядке:

1. Системное свойство: -DAPPLICATION_INSIGHTS_IKEY=your_ikey
2. Переменная среды. APPLICATION_INSIGHTS_IKEY
3. Файл конфигурации. ApplicationInsights.xml

Вы также можете [задать его в коде](../../azure-monitor/app/api-custom-events-metrics.md#ikey):

```java
    String instrumentationKey = "00000000-0000-0000-0000-000000000000";

    if (instrumentationKey != null)
    {
        TelemetryConfiguration.getActive().setInstrumentationKey(instrumentationKey);
    }
```

Обратите внимание, что [динамические метрики](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) не поддерживают чтение ключа инструментирования из кода.

## <a name="4-add-an-http-filter"></a>4. Добавление фильтра HTTP
Последний шаг настройки позволяет компоненту HTTP-запросов выполнить протоколирование каждого веб-запроса. (Не обязательно, если используется только упрощенный интерфейс API.)

### <a name="spring-boot-applications"></a>Приложения Spring Boot
Зарегистрируйте `WebRequestTrackingFilter` Application Insights в классе конфигурации:

```Java
package <yourpackagename>.configurations;

import javax.servlet.Filter;

import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.core.Ordered;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;
import com.microsoft.applicationinsights.TelemetryConfiguration;
import com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter;

@Configuration
public class AppInsightsConfig {

    @Bean
    public String telemetryConfig() {
        String telemetryKey = System.getenv("<instrumentation key>");
        if (telemetryKey != null) {
            TelemetryConfiguration.getActive().setInstrumentationKey(telemetryKey);
        }
        return telemetryKey;
    }

    /**
     * Programmatically registers a FilterRegistrationBean to register WebRequestTrackingFilter
     * @param webRequestTrackingFilter
     * @return Bean of type {@link FilterRegistrationBean}
     */
    @Bean
    public FilterRegistrationBean webRequestTrackingFilterRegistrationBean(WebRequestTrackingFilter webRequestTrackingFilter) {
        FilterRegistrationBean registration = new FilterRegistrationBean();
        registration.setFilter(webRequestTrackingFilter);
        registration.addUrlPatterns("/*");
        registration.setOrder(Ordered.HIGHEST_PRECEDENCE + 10);
        return registration;
    }


    /**
     * Creates bean of type WebRequestTrackingFilter for request tracking
     * @param applicationName Name of the application to bind filter to
     * @return {@link Bean} of type {@link WebRequestTrackingFilter}
     */
    @Bean
    @ConditionalOnMissingBean

    public WebRequestTrackingFilter webRequestTrackingFilter(@Value("${spring.application.name:application}") String applicationName) {
        return new WebRequestTrackingFilter(applicationName);
    }


}
```

> [!NOTE]
> Если вы используете Spring Boot 1.3.8 или более ранней версии, замените FilterRegistrationBean следующей строкой

```Java
    import org.springframework.boot.context.embedded.FilterRegistrationBean;
```

Этот класс настроит `WebRequestTrackingFilter` в качестве первого фильтра в цепочке фильтров HTTP. Он также извлечет ключ инструментирования из переменной среды операционной системы, если он доступен.

> Мы используем конфигурацию веб-фильтра HTTP, а не конфигурацию Spring MVC, свойственную для приложения Spring Boot. Дополнительные сведения о конфигурации Spring MVC см. в разделах ниже.

### <a name="applications-using-webxml"></a>Приложения, использующие Web.xml
Найдите и откройте файл web.xml в проекте, добавьте следующий фрагмент кода в узел web-app, где настраиваются фильтры вашего приложения.

Для получения наиболее точных результатов этот фильтр должен применяться до всех остальных фильтров.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>

   <!-- This listener handles shutting down the TelemetryClient when an application/servlet is undeployed. -->
    <listener>
      <listener-class>com.microsoft.applicationinsights.web.internal.ApplicationInsightsServletContextListener</listener-class>
    </listener>
```

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a>Если вы используете Spring Web MVC 3.1 или более поздней версии
Для включения пакета Application Insights измените следующие элементы в *-servlet.xml:

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a>Если вы используете Struts 2
Добавьте в файл конфигурации Struts следующий элемент (обычно называется struts.xml или struts-default.xml):

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

При наличии перехватчиков, определенных в стандартном стеке, перехватчик можно добавить в стек.

## <a name="5-run-your-application"></a>5. Запуск приложения
Запустите приложение в режиме отладки на компьютере разработки или опубликуйте его на своем сервере.

## <a name="6-view-your-telemetry-in-application-insights"></a>6. Просмотр данных телеметрии в Application Insights
Вернитесь к ресурсу Application Insights на [портале Microsoft Azure](https://portal.azure.com).

В колонке обзора появятся данные HTTP-запросов. (Если данные отсутствуют, подождите несколько секунд и нажмите кнопку обновления).

![Снимок экрана: обзор с примером данных](./media/java-get-started/overview-graphs.png)

[Дополнительные сведения о метриках.][metrics]

Щелкните любую диаграмму, чтобы увидеть более подробные агрегированные метрики.

![Панель Application Insights сбоев с диаграммами](./media/java-get-started/006-barcharts.png)

> Служба Application Insights предполагает, что HTTP-запросы для приложений MVC имеют следующий формат: `VERB controller/action`. Например, `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` и `GET Home/Product/sdf96vws` сгруппированы в `GET Home/Product`. Это позволяет осмысленно группировать запросы, получая, например, число запросов и среднее время выполнения запросов.
>
>

### <a name="instance-data"></a>Данные экземпляров
Щелкните тип запроса, чтобы просмотреть отдельные экземпляры.

![Детализация конкретного примера представления](./media/java-get-started/007-instance.png)

### <a name="analytics-powerful-query-language"></a>Аналитика. Эффективный язык запросов
По мере увеличения объема накопленных данных вы сможете использовать запросы для объедения данных и поиска отдельных экземпляров.  [Аналитика](../../azure-monitor/app/analytics.md) — это мощный инструмент, который не только позволяет изучать сведения о производительности и использовании, но и диагностировать возможные неполадки.

![Пример аналитики](./media/java-get-started/0025.png)

## <a name="7-install-your-app-on-the-server"></a>7. Установка приложения на сервере
Теперь опубликуйте приложение на сервере, откройте доступ для пользователей и изучайте телеметрию на портале.

* Убедитесь, что брандмауэр позволяет приложению отправлять телеметрию на следующие порты:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Если исходящий трафик должен проходить через брандмауэр, определите системные свойства `http.proxyHost` и `http.proxyPort`.

* На серверах Windows необходимо установить следующее:

  * [распространяемые компоненты Microsoft Visual C++.](https://www.microsoft.com/download/details.aspx?id=40784)

    (Сюда входят счетчики производительности).

## <a name="azure-app-service-config-spring-boot"></a>Конфигурация службы приложений Azure (пружинная загрузка)

Приложения с пружинной загрузкой, запущенные в Windows, нуждаются в дополнительной настройке для запуска в службах приложений Azure. Измените **файл Web. config** и добавьте следующее:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <handlers>
            <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified"/>
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\AzureWebAppExample-0.0.1-SNAPSHOT.jar&quot;">
        </httpPlatform>
    </system.webServer>
</configuration>
```

## <a name="exceptions-and-request-failures"></a>Исключения и ошибки запросов
Необработанные исключения автоматически собираются.

Для сбора данных по другим исключениям доступны два варианта:

* [Вставьте в код вызовы trackException ()][apiexceptions].
* [установить на сервере агент для Java](java-agent.md). Необходимо указать методы, которые требуется отслеживать.

## <a name="monitor-method-calls-and-external-dependencies"></a>Мониторинг вызовов методов и внешних зависимостей.
[установить агент для Java](java-agent.md) .

## <a name="w3c-distributed-tracing"></a>Распределенная трассировка W3C

Пакет средств разработки для Java Application Insights теперь поддерживает [распределенную трассировку W3C](https://w3c.github.io/trace-context/).

Конфигурация входящих параметров пакета средств разработки описана далее в статье о [корреляции](correlation.md#w3c-distributed-tracing).

Конфигурация входящих параметров пакета средств разработки задана в файле [AI-Agent.xml](java-agent.md).

## <a name="performance-counters"></a>Счетчики производительности
Откройте раздел **исследование**, **метрики**, чтобы просмотреть диапазон счетчиков производительности.

![Снимок экрана: панель метрик с выбранным байтовым частным байтом процесса](./media/java-get-started/011-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Настройка сбора данных счетчиками производительности
Чтобы отключить сбор данных стандартным набором счетчиков производительности, добавьте следующий фрагмент кода в корневой узел файла ApplicationInsights.xml:

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a>Сбор данных дополнительными счетчиками производительности
Можно задать необходимость сбора данных дополнительными счетчиками производительности.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>Счетчики JMX (предоставляются виртуальной машиной Java)

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* `displayName` — имя, отображаемое в портале Application Insights.
* `objectName` — имя объекта JMX.
* `attribute` — атрибут имени объекта JMX для выборки
* `type` (необязательно) — тип атрибута объекта JMX:
  * по умолчанию: простой тип, такой как int или long;
  * `composite`— данные счетчика производительности имеют формат "Атрибут.Данные";
  * `tabular`— данные счетчика производительности имеют формат строки таблицы.

#### <a name="windows-performance-counters"></a>Счетчики производительности Windows
Каждый [счетчик производительности Windows](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) входит в состав категории (аналогично поле является членом класса). Категория может быть глобальной либо иметь пронумерованные или именованные экземпляры.

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* displayName — имя, отображаемое в портале Application Insights.
* categoryName — категория счетчика производительности (объект производительности), с которой связан этот счетчик производительности.
* counterName — имя счетчика производительности.
* instanceName — имя экземпляра категории счетчика производительности или пустая строка (""), если категория содержит единственный экземпляр. Если categoryName имеет значение "Process", и источником данных для счетчиков производительности является текущий процесс виртуальной машины Java, на которой выполняется ваше приложение, укажите `"__SELF__"`.

### <a name="unix-performance-counters"></a>Счетчики производительности Unix
* [установите collectd с подключаемым модулем Application Insights](java-collectd.md) .

## <a name="get-user-and-session-data"></a>Получение данных о пользователях и сеансах
Итак, вы отправляете телеметрию с веб-сервера. Теперь для получения полного представления о приложении можно настроить дополнительные функции мониторинга:

* [Добавьте телеметрии на веб-страницы][usage] для мониторинга просмотров страниц и метрик пользователя.
* [Настройте веб-тесты][availability] , которые помогут быть уверенными в том, что приложение остается работоспособным и правильно отвечает на запросы.

## <a name="capture-log-traces"></a>Журнал трассировки
Службу Application Insights можно использовать для журналов продольных и поперечных срезов данных из Log4J, Logback или других платформ ведения журнала. Записи журналов можно соотносить с HTTP-запросами и другими данными телеметрии. [Подробнее][javalogs].

## <a name="send-your-own-telemetry"></a>Отправка собственных данных телеметрии
После установки пакета SDK можно использовать интерфейс API для отправки собственных данных телеметрии.

* [Следите за пользовательскими событиями и метриками][api] , чтобы узнать, какие пользователи выполняются с приложением.
* [Поиск событий и журналов][diagnostic] для диагностики неполадок.

## <a name="availability-web-tests"></a>Доступность веб-тестов
Application Insights может тестировать ваш веб-сайт через равные промежутки времени для проверки, работает ли он и правильно ли отвечает на запросы.

[Дополнительные сведения о настройке веб-тестов доступности см. здесь.][availability]

## <a name="questions-problems"></a>Вопросы? Проблемы?
[Устранение неполадок Java](java-troubleshoot.md)

## <a name="next-steps"></a>Следующие шаги
* [Отслеживайте вызовы зависимостей.](java-agent.md)
* [Отслеживайте счетчики производительности Unix.](java-collectd.md)
* Добавляйте [мониторинг на веб-страницы](javascript.md), чтобы отслеживать время загрузки страниц, вызовы AJAX и исключения браузера.
* Пишите [пользовательскую телеметрию](../../azure-monitor/app/api-custom-events-metrics.md) для отслеживания использования в браузере или на сервере.
* Используйте [аналитику](../../azure-monitor/app/analytics.md), чтобы выполнять эффективные запросы телеметрии из приложения.
* Дополнительные сведения см. в разделе [Azure for Java developers](/java/azure) (Azure для разработчиков Java).

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#trackexception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: ../../azure-monitor/learn/java-quick-start.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[usage]: javascript.md
