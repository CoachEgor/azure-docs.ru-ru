---
title: Мониторинг производительности служб приложений Azure | Документация Майкрософт
description: Мониторинг производительности приложений для служб приложений Azure. Загрузка диаграммы и время отклика, сведения о зависимостях и Настройка оповещений о производительности.
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 03d332af182f8f40ede634fbd563f7b064751f32
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2020
ms.locfileid: "78367709"
---
# <a name="monitor-azure-app-service-performance"></a>Мониторинг производительности Службы приложений Azure

Включение мониторинга для веб-приложений на основе ASP.NET и ASP.NET Core, работающих в [службах приложений Azure](https://docs.microsoft.com/azure/app-service/) , стало еще проще, чем когда бы то ни было. В то время как раньше требовалось вручную установить расширение сайта, в образ службы приложений теперь по умолчанию встроено Последнее расширение или агент. В этой статье описано, как включить мониторинг Application Insights, а также приводятся предварительные рекомендации по автоматизации процесса для крупномасштабных развертываний.

> [!NOTE]
> Добавление расширения Application Insights сайта вручную с помощью **средств разработки** > **расширений** является устаревшим. Этот метод установки расширения был зависеть от ручных обновлений для каждой новой версии. Последний стабильный выпуск расширения теперь [предварительно установлен](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) в составе образа службы приложений. Файлы находятся в `d:\Program Files (x86)\SiteExtensions\ApplicationInsightsAgent` и автоматически обновляются в каждом стабильном выпуске. Если вы выполните инструкции на основе агента, чтобы включить наблюдение ниже, оно автоматически удалит устаревшее расширение.

## <a name="enable-application-insights"></a>Включение Application Insights

Включить мониторинг приложений для приложений, размещенных в службах приложений Azure, можно двумя способами:

* **Мониторинг приложений на основе агента** (аппликатионинсигхтсажент).  
    * Этот метод является самым простым для включения и не требует дополнительной настройки. Часто это называется мониторингом среды выполнения. Для служб приложений Azure рекомендуется как минимум включить этот уровень мониторинга, а затем в зависимости от конкретного сценария можно оценить, требуется ли более сложный мониторинг с помощью инструментирования вручную.

* **Ручное инструментирование приложения с помощью кода** путем установки пакета SDK для Application Insights.

    * Этот подход гораздо более настраиваемый, но требует [добавления зависимости от Application Insights пакетов NUGET SDK](https://docs.microsoft.com/azure/azure-monitor/app/asp-net). Этот метод также означает, что вам нужно самостоятельно управлять обновлениями для последних версий пакетов.

    * Если необходимо выполнить пользовательские вызовы API для отслеживания событий или зависимостей, не отслеживаемых по умолчанию с помощью мониторинга на основе агентов, необходимо использовать этот метод. Дополнительные сведения см. в [статье об API для получения пользовательских событий и метрик](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) .

> [!NOTE]
> Если обнаруживается и отслеживание на основе агентов, и инструментирование вручную на основе пакета SDK, будут учитываться только параметры инструментирования вручную. Это позволяет предотвратить отправку повторяющихся данных. Чтобы получить дополнительные сведения об этом, ознакомьтесь с [разделом устранение неполадок](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting) ниже.

## <a name="enable-agent-based-monitoring"></a>Включить мониторинг на основе агентов

# <a name="net"></a>[.NET](#tab/net)

> [!NOTE]
> Сочетание APPINSIGHTS_JAVASCRIPT_ENABLED и urlCompression не поддерживается. Дополнительные сведения см. в описании [раздела Устранение неполадок](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting).


1. На панели управления Azure **выберите Application Insights** для своей службы приложений.

    ![В разделе "Параметры" выберите пункт Application Insights](./media/azure-web-apps/settings-app-insights-01.png)

   * Нажмите переключатель "Создать новый ресурс", если вы еще не настроили ресурс Application Insights для этого приложения. 

     > [!NOTE]
     > Нажав кнопку **OK** для создания нового ресурса, вам будет предложено выбрать параметр **Применение параметров мониторинга**. Нажав кнопку **Продолжить**, вы свяжете новый ресурс Application Insights со службой приложений, а также **активируете перезапуск самой службы приложений**. 

     ![Инструментирование веб-приложения](./media/azure-web-apps/create-resource-01.png)

2. Указав ресурс для использования, вы можете выбрать способ сбора данных в Application Insights для каждой платформы для приложения. Мониторинг приложений ASP.NET по умолчанию имеет два разных уровня коллекции.

    ![Выбор параметров для каждой платформы](./media/azure-web-apps/choose-options-new.png)

   * Уровень **базовой коллекции** .NET предоставляет основные возможности APM с одним экземпляром.

   * Уровень **рекомендуемой коллекции** .NET:
       * Добавляет тенденции использования ЦП, памяти и операций ввода-вывода.
       * Сопоставляет микрослужбы в пределах границ запросов или зависимостей.
       * Собирает тенденции использования и обеспечивает сопоставление от результатов доступности до транзакций.
       * Собирает исключения, необработанные хост-процессом.
       * Улучшает точность метрик APM под нагрузкой при использовании выборки.

3. Чтобы настроить такие параметры, как выборка, которые можно было бы контролировать с помощью файла applicationinsights. config, теперь можно взаимодействовать с теми же параметрами через параметры приложения с соответствующим префиксом. 

    * Например, чтобы изменить начальный процент выборки, можно создать параметр приложения: `MicrosoftAppInsights_AdaptiveSamplingTelemetryProcessor_InitialSamplingPercentage` и значение `100`.

    * Список поддерживаемых параметров обработчика данных телеметрии адаптивной выборки можно найти в [коде](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/master/src/ServerTelemetryChannel/AdaptiveSamplingTelemetryProcessor.cs) и [связанной документации](https://docs.microsoft.com/azure/azure-monitor/app/sampling).

# <a name="net-core"></a>[.NET Core](#tab/netcore)

Поддерживаются следующие версии .NET Core: ASP.NET Core 2,0, ASP.NET Core 2,1, ASP.NET Core 2,2, ASP.NET Core 3,0

Для мониторинга на основе .NET Core, автономного развертывания и приложений на основе Linux в настоящее время **не поддерживаются** мониторинг с помощью агентов и расширений. ([Инструментирование вручную](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) с помощью кода будет работать во всех предыдущих сценариях.)

1. На панели управления Azure **выберите Application Insights** для своей службы приложений.

    ![В разделе "Параметры" выберите пункт Application Insights](./media/azure-web-apps/settings-app-insights-01.png)

   * Нажмите переключатель "Создать новый ресурс", если вы еще не настроили ресурс Application Insights для этого приложения. 

     > [!NOTE]
     > Нажав кнопку **OK** для создания нового ресурса, вам будет предложено выбрать параметр **Применение параметров мониторинга**. Нажав кнопку **Продолжить**, вы свяжете новый ресурс Application Insights со службой приложений, а также **активируете перезапуск самой службы приложений**. 

     ![Инструментирование веб-приложения](./media/azure-web-apps/create-resource-01.png)

2. Указав, какой ресурс следует использовать, можно выбрать способ, с помощью которого Application Insights собирает данные для каждой платформы приложения. .NET Core предлагает **рекомендуемую коллекцию** или **Отключить** для .net Core 2,0, 2,1, 2,2 и 3,0.

    ![Выбор параметров для каждой платформы](./media/azure-web-apps/choose-options-new-net-core.png)

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

В веб-приложении службы приложений в разделе **параметры** > **выберите Application Insights** > **включить**. Мониторинг на основе агента Node. js в настоящее время находится на этапе предварительной версии.

# <a name="java"></a>[Java](#tab/java)

Веб-приложения на основе службы приложений Java в настоящее время не поддерживают автоматическое отслеживание на основе агентов и расширений. Чтобы включить мониторинг для приложения Java, необходимо [вручную выполнить инструментирование приложения](https://docs.microsoft.com/azure/azure-monitor/app/java-get-started).

# <a name="python"></a>[Python](#tab/python)

Веб-приложения на основе службы приложений Python в настоящее время не поддерживают автоматическое отслеживание на основе агентов и расширений. Чтобы включить мониторинг для приложения Python, необходимо [вручную выполнить инструментирование приложения](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python).

---

## <a name="enable-client-side-monitoring"></a>Включение наблюдения на стороне клиента

# <a name="net"></a>[.NET](#tab/net)

Наблюдение на стороне клиента — это согласие на ASP.NET. Чтобы включить наблюдение на стороне клиента:

* Выберите **параметры** > * * * Параметры приложения * * * *
   * В разделе Параметры приложения добавьте новое имя и **значение** **параметра приложения** :

     Имя: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Значение: `true`

   * **Сохраните** параметры и **перезапустите** приложение.

![Снимок экрана: Пользовательский интерфейс параметров приложения](./media/azure-web-apps/appinsights-javascript-enabled.png)

Чтобы отключить наблюдение на стороне клиента, удалите связанную пару "ключ-значение" из параметров приложения или установите значение false.

# <a name="net-core"></a>[.NET Core](#tab/netcore)

Наблюдение на стороне клиента **включено по умолчанию** для приложений .NET Core с **рекомендуемой коллекцией**, независимо от наличия параметра приложения "APPINSIGHTS_JAVASCRIPT_ENABLED".

Если по какой-либо причине вы хотели бы отключить наблюдение на стороне клиента:

* Выберите **параметры** > **Параметры приложения** .
   * В разделе Параметры приложения добавьте новое имя и **значение** **параметра приложения** :

     Имя: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Значение: `false`

   * **Сохраните** параметры и **перезапустите** приложение.

![Снимок экрана: Пользовательский интерфейс параметров приложения](./media/azure-web-apps/appinsights-javascript-disabled.png)

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Чтобы включить наблюдение на стороне клиента для приложения Node. js, необходимо [вручную добавить пакет SDK JavaScript на стороне клиента в приложение](https://docs.microsoft.com/azure/azure-monitor/app/javascript).

# <a name="java"></a>[Java](#tab/java)

Чтобы включить мониторинг на стороне клиента для приложения Java, необходимо [вручную добавить пакет SDK JavaScript на стороне клиента в приложение](https://docs.microsoft.com/azure/azure-monitor/app/javascript).

# <a name="python"></a>[Python](#tab/python)

Чтобы включить мониторинг на стороне клиента для приложения Python, необходимо [вручную добавить пакет SDK JavaScript на стороне клиента в приложение](https://docs.microsoft.com/azure/azure-monitor/app/javascript).

---

## <a name="automate-monitoring"></a>Автоматизация мониторинга

Чтобы включить сбор данных телеметрии с Application Insights, необходимо задать только параметры приложения:

   ![Параметры приложения службы приложений с доступными параметрами Application Insights](./media/azure-web-apps/application-settings.png)

### <a name="application-settings-definitions"></a>Определения параметров приложения

|Имя параметра приложения |  Определение | Значение |
|-----------------|:------------|-------------:|
|ApplicationInsightsAgent_EXTENSION_VERSION | Главное расширение, которое управляет мониторингом среды выполнения. | `~2` |
|XDT_MicrosoftApplicationInsights_Mode |  В режиме по умолчанию для обеспечения оптимальной производительности включены только функции, обеспечивающие их работу. | `default` или `recommended`. |
|InstrumentationEngine_EXTENSION_VERSION | Определяет, будет ли включен модуль двоичной перезаписи `InstrumentationEngine`. Этот параметр оказывает влияние на производительность и влияет на время холодного запуска и запуска. | `~1` |
|XDT_MicrosoftApplicationInsights_BaseExtensions | Определяет, будет ли записываться текст таблицы SQL & Azure вместе с вызовами зависимостей. Предупреждение о производительности: время холодного запуска приложения будет затронуто. Для этого параметра требуется `InstrumentationEngine`. | `~1` |

### <a name="app-service-application-settings-with-azure-resource-manager"></a>Параметры приложения службы приложений с Azure Resource Manager

Параметры приложения для служб приложений можно управлять и настраивать с помощью [шаблонов Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). Этот метод можно использовать при развертывании новых ресурсов службы приложений с помощью Azure Resource Manager Automation или изменении параметров существующих ресурсов.

Ниже показана базовая структура JSON параметров приложения для службы приложений.

```JSON
      "resources": [
        {
          "name": "appsettings",
          "type": "config",
          "apiVersion": "2015-08-01",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites', variables('webSiteName'))]"
          ],
          "tags": {
            "displayName": "Application Insights Settings"
          },
          "properties": {
            "key1": "value1",
            "key2": "value2"
          }
        }
      ]
```

Чтобы получить пример шаблона Azure Resource Manager с параметрами приложения, настроенными для Application Insights, этот [шаблон](https://github.com/Andrew-MSFT/BasicImageGallery) может быть полезен, в частности раздел, начинающийся в [строке 238](https://github.com/Andrew-MSFT/BasicImageGallery/blob/c55ada54519e13ce2559823c16ca4f97ddc5c7a4/CoreImageGallery/Deploy/CoreImageGalleryARM/azuredeploy.json#L238).

### <a name="automate-the-creation-of-an-application-insights-resource-and-link-to-your-newly-created-app-service"></a>Автоматизируйте создание Application Insights ресурса и ссылку на созданную службу приложений.

Чтобы создать шаблон Azure Resource Manager со всеми настроенными параметрами Application Insights по умолчанию, начните процесс, как если бы вы создавали новое веб-приложение с включенным Application Insights.

Выбор **параметров автоматизации**

   ![Меню создания веб-приложения службы приложений](./media/azure-web-apps/create-web-app.png)

Этот параметр создает последний шаблон Azure Resource Manager со всеми настройками обязательных параметров.

  ![Шаблон веб-приложения службы приложений](./media/azure-web-apps/arm-template.png)

Ниже приведен пример, замените все экземпляры `AppMonitoredSite` именем вашего сайта:

```json
{
    "resources": [
        {
            "name": "[parameters('name')]",
            "type": "Microsoft.Web/sites",
            "properties": {
                "siteConfig": {
                    "appSettings": [
                        {
                            "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').InstrumentationKey]"
                        },
                        {
                            "name": "APPLICATIONINSIGHTS_CONNECTION_STRING",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').ConnectionString]"
                        },
                        {
                            "name": "ApplicationInsightsAgent_EXTENSION_VERSION",
                            "value": "~2"
                        }
                    ]
                },
                "name": "[parameters('name')]",
                "serverFarmId": "[concat('/subscriptions/', parameters('subscriptionId'),'/resourcegroups/', parameters('serverFarmResourceGroup'), '/providers/Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "microsoft.insights/components/AppMonitoredSite"
            ],
            "apiVersion": "2016-03-01",
            "location": "[parameters('location')]"
        },
        {
            "apiVersion": "2016-09-01",
            "name": "[parameters('hostingPlanName')]",
            "type": "Microsoft.Web/serverfarms",
            "location": "[parameters('location')]",
            "properties": {
                "name": "[parameters('hostingPlanName')]",
                "workerSizeId": "[parameters('workerSize')]",
                "numberOfWorkers": "1",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "sku": {
                "Tier": "[parameters('sku')]",
                "Name": "[parameters('skuCode')]"
            }
        },
        {
            "apiVersion": "2015-05-01",
            "name": "AppMonitoredSite",
            "type": "microsoft.insights/components",
            "location": "West US 2",
            "properties": {
                "ApplicationId": "[parameters('name')]",
                "Request_Source": "IbizaWebAppExtensionCreate"
            }
        }
    ],
    "parameters": {
        "name": {
            "type": "string"
        },
        "hostingPlanName": {
            "type": "string"
        },
        "hostingEnvironment": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "sku": {
            "type": "string"
        },
        "skuCode": {
            "type": "string"
        },
        "workerSize": {
            "type": "string"
        },
        "serverFarmResourceGroup": {
            "type": "string"
        },
        "subscriptionId": {
            "type": "string"
        }
    },
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0"
}
```

### <a name="enabling-through-powershell"></a>Включение с помощью PowerShell

Чтобы включить мониторинг приложений с помощью PowerShell, необходимо изменить только базовые параметры приложения. Ниже приведен пример, который позволяет отслеживать приложения для веб-сайта с именем "Аппмониторедсите" в группе ресурсов "Аппмониторедрг" и настраивает данные для отправки в ключ инструментирования "012345678-ABCD-ef01-2345-6789abcd".

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
$app = Get-AzWebApp -ResourceGroupName "AppMonitoredRG" -Name "AppMonitoredSite" -ErrorAction Stop
$newAppSettings = @{} # case-insensitive hash map
$app.SiteConfig.AppSettings | %{$newAppSettings[$_.Name] = $_.Value} # preserve non Application Insights application settings.
$newAppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"] = "012345678-abcd-ef01-2345-6789abcd"; # set the Application Insights instrumentation key
$newAppSettings["APPLICATIONINSIGHTS_CONNECTION_STRING"] = "InstrumentationKey=012345678-abcd-ef01-2345-6789abcd"; # set the Application Insights connection string
$newAppSettings["ApplicationInsightsAgent_EXTENSION_VERSION"] = "~2"; # enable the ApplicationInsightsAgent
$app = Set-AzWebApp -AppSettings $newAppSettings -ResourceGroupName $app.ResourceGroup -Name $app.Name -ErrorAction Stop
```

## <a name="upgrade-monitoring-extensionagent"></a>Обновить расширение или агент мониторинга

### <a name="upgrading-from-versions-289-and-up"></a>Обновление с версий 2.8.9 и выше

Обновление с версии 2.8.9 происходит автоматически без каких бы то ни было дополнительных действий. Новые биты мониторинга доставляются в фоновом режиме в целевую службу приложений, и при перезапуске приложения они будут отобраны.

Чтобы проверить, какую версию расширения вы используете, перейдите `http://yoursitename.scm.azurewebsites.net/ApplicationInsights`

![Снимок экрана: http://yoursitename.scm.azurewebsites.net/ApplicationInsights URL-пути](./media/azure-web-apps/extension-version.png)

### <a name="upgrade-from-versions-100---265"></a>Обновление с версий 1.0.0 – 2.6.5

Начиная с версии 2.8.9 используется предварительно установленное расширение сайта. Если вы используете более раннюю версию, можно выполнить обновление одним из двух способов:

* [Обновление путем включения через портал](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights). (Даже если у вас установлен модуль Application Insights для службы приложений Azure, в пользовательском интерфейсе отображается только кнопка **включить** . В фоновом режиме старое расширение частного сайта будет удалено.)

* [Обновление с помощью PowerShell](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell):

    1. Задайте параметры приложения, чтобы включить предварительно установленное расширение сайта Аппликатионинсигхтсажент. См. раздел [Включение с помощью PowerShell](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell).
    2. Вручную удалите расширение частного сайта с именем Application Insights расширение для службы приложений Azure.

Если обновление выполняется из версии до 2.5.1, убедитесь, что библиотеки DLL Аппликатионинсигсс удалены из папки Bin приложения см. в [разделе действия по устранению неполадок](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting).

## <a name="troubleshooting"></a>Устранение неполадок

Ниже приведено пошаговое руководство по устранению неполадок для мониторинга на основе расширений и агентов для приложений на платформе .NET и .NET Core, работающих в службах приложений Azure.

> [!NOTE]
> Приложения Java поддерживаются только в службах приложений Azure с помощью инструментирования вручную на основе пакета SDK, поэтому приведенные ниже действия не применимы к этим сценариям.

1. Убедитесь, что приложение отслеживается с помощью `ApplicationInsightsAgent`.
    * Убедитесь, что для параметра `ApplicationInsightsAgent_EXTENSION_VERSION` приложения задано значение "~ 2".
2. Убедитесь, что приложение соответствует требованиям, которые необходимо отслеживать.
    * Перейти к `https://yoursitename.scm.azurewebsites.net/ApplicationInsights`

    ![Снимок экрана со страницей результатов https://yoursitename.scm.azurewebsites/applicationinsights](./media/azure-web-apps/app-insights-sdk-status.png)

    * Убедитесь, что `Application Insights Extension Status` `Pre-Installed Site Extension, version 2.8.12.1527, is running.`
        * Если она не запущена, следуйте инструкциям по [включению мониторинга Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights)

    * Убедитесь, что источник состояния существует и выглядит следующим образом: `Status source D:\home\LogFiles\ApplicationInsights\status\status_RD0003FF0317B6_4248_1.json`
        * Если такое же значение отсутствует, это означает, что приложение в данный момент не выполняется или не поддерживается. Чтобы убедиться, что приложение выполняется, попробуйте вручную посетить конечные точки URL-адреса приложения или приложения, что позволит получить доступ к сведениям о среде выполнения.

    * Убедитесь, что `IKeyExists` `true`
        * Если это `false`, добавьте `APPINSIGHTS_INSTRUMENTATIONKEY` и `APPLICATIONINSIGHTS_CONNECTION_STRING` с идентификатором GUID iKey в параметры приложения.

    * Убедитесь, что отсутствуют записи для `AppAlreadyInstrumented`, `AppContainsDiagnosticSourceAssembly`и `AppContainsAspNetTelemetryCorrelationAssembly`.
        * Если любая из этих записей существует, удалите из приложения следующие пакеты: `Microsoft.ApplicationInsights`, `System.Diagnostics.DiagnosticSource`и `Microsoft.AspNet.TelemetryCorrelation`.

В таблице ниже приведено более подробное описание этих значений, их основных причин и Рекомендуемые исправления.

|Значение проблемы|Объяснение|Fix
|---- |----|---|
| `AppAlreadyInstrumented:true` | Это значение указывает, что расширение обнаружило, что некоторые аспекты пакета SDK уже имеются в приложении и будут отключаться. Это может быть вызвано ссылками на `System.Diagnostics.DiagnosticSource`, `Microsoft.AspNet.TelemetryCorrelation`или `Microsoft.ApplicationInsights`  | Удалите ссылки. Некоторые из этих ссылок добавляются по умолчанию из определенных шаблонов Visual Studio, а более старые версии Visual Studio могут добавлять ссылки на `Microsoft.ApplicationInsights`.
|`AppAlreadyInstrumented:true` | Если приложение предназначено для .NET Core 2,1 или 2,2, а ссылается на [Microsoft. AspNetCore. ALL](https://www.nuget.org/packages/Microsoft.AspNetCore.All) meta-Package, то оно переносится в Application Insights, а расширение будет отключаться. | Клиентам в .NET Core 2.1, 2.2 [рекомендуется](https://github.com/aspnet/Announcements/issues/287) использовать вместо него мета-пакет Microsoft. AspNetCore. app.|
|`AppAlreadyInstrumented:true` | Это значение также может быть вызвано наличием указанных выше библиотек DLL в папке приложения из предыдущего развертывания. | Очистите папку приложения, чтобы убедиться, что эти библиотеки DLL удалены. Проверьте каталог Bin локального приложения и каталог wwwroot в службе приложений. (Чтобы проверить каталог wwwroot веб-приложения службы приложений: Advanced Tools (KUDU) > консоль отладки > CMD > хоме\сите\ввврут).
|`AppContainsAspNetTelemetryCorrelationAssembly: true` | Это значение указывает, что расширение обнаружило ссылки на `Microsoft.AspNet.TelemetryCorrelation` в приложении и будет отключаться. | Удаление ссылки.
|`AppContainsDiagnosticSourceAssembly**:true`|Это значение указывает, что расширение обнаружило ссылки на `System.Diagnostics.DiagnosticSource` в приложении и будет отключаться.| Удаление ссылки.
|`IKeyExists:false`|Это значение указывает, что ключ инструментирования отсутствует в AppSetting, `APPINSIGHTS_INSTRUMENTATIONKEY`. Возможные причины. возможно, значения были случайно удалены, забыли установить значения в скрипте автоматизации и т. д. | Убедитесь, что параметр есть в параметрах приложения службы приложений.

### <a name="appinsights_javascript_enabled-and-urlcompression-is-not-supported"></a>APPINSIGHTS_JAVASCRIPT_ENABLED и urlCompression не поддерживаются

При использовании APPINSIGHTS_JAVASCRIPT_ENABLED = true в случаях, когда содержимое кодируется, могут возникать такие ошибки: 

- 500 ошибка переопределения URL-адреса
- Ошибка модуля переопределения URL-адресов 500,53. правила перезаписи исходящих сообщений не могут быть применены, если содержимое HTTP-ответа закодировано (gzip). 

Это происходит из-за того, что для параметра приложения APPINSIGHTS_JAVASCRIPT_ENABLED задано значение true и кодировка содержимого представлена одновременно. Этот сценарий пока не поддерживается. Обходной путь состоит в том, чтобы удалить APPINSIGHTS_JAVASCRIPT_ENABLED из параметров приложения. К сожалению, это означает, что если на стороне клиента и браузера по-прежнему требуется инструментирование JavaScript, для веб-страниц требуются ручные ссылки на пакеты SDK. Следуйте [инструкциям](https://github.com/Microsoft/ApplicationInsights-JS#snippet-setup-ignore-if-using-npm-setup) по ручному инструментированию с помощью пакета SDK для JavaScript.

Последние сведения об агенте или расширении Application Insights см. в [заметках о выпуске](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/app-insights-web-app-extensions-releasenotes.md).

## <a name="next-steps"></a>Дальнейшие действия
* [Запуск профилировщика в живом приложении](../app/profiler.md).
* [Функции Azure.](https://github.com/christopheranderson/azure-functions-app-insights-sample) Отслеживайте функции Azure с помощью Application Insights.
* [Включите отправку данных диагностики Azure](../platform/diagnostics-extension-to-application-insights.md) в Application Insights.
* [Отслеживайте метрики состояния службы](../platform/data-platform.md), чтобы убедиться, что служба доступна и отвечает на запросы.
* [Получайте уведомления](../platform/alerts-overview.md) при возникновении операционных событий или превышении пороговых значений метрик.
* Используйте [расширение Application Insights для приложений JavaScript и веб-страниц](javascript.md), чтобы получать данные телеметрии клиентов из браузеров, которые используются для открытия веб-страницы.
* [Настройте веб-тесты доступности](monitor-web-app-availability.md), чтобы получать уведомления о сбоях в работе сайта.
