---
title: Использование на уровне приложения с высокой плотностью размещения масштабирование — службы приложений Azure | Документация Майкрософт
description: Высокая плотность размещения в службе приложений Azure
author: btardif
manager: erikre
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/13/2019
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: 824abbdfd1b3980b419e6d6c46814bb0318adf13
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602327"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>Высокая плотность размещения в службе приложений Azure, с помощью масштабирования на уровне приложения

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

При использовании службы приложений, вы можете масштабировать свои приложения путем масштабирования [план службы приложений](overview-hosting-plans.md) они выполняются. Если в одном плане службы приложений выполняется несколько приложений, в каждом масштабируемом экземпляре выполняются все приложения в плане.

*Масштабирование на уровне приложения* включается на уровне плана службы приложений для позволяют масштабировать приложение независимо от плана службы приложений, на котором она размещена. Таким образом, план службы приложений можно увеличить до 10 экземпляров, но приложение максимально может использовать только пять.

> [!NOTE]
> Независимое масштабирование приложений доступно только для ценовых категорий **Стандартный**, **Премиум**, **Премиум V2** и **Изолированный**.
>

Приложения, выделяются в доступных план службы приложений, с помощью наилучший подход усилий для равномерного распределения по экземплярам. Хотя равномерное распределение не гарантируется, платформа будет убедитесь, что два экземпляра одного приложения не будут размещаться на один и тот же экземпляр плана службы приложений.

Платформа не зависит от метрики, недоступные для распределения рабочей роли. Приложения Реорганизация только в том случае, если экземпляры добавляются или удаляются из плана службы приложений.

## <a name="per-app-scaling-using-powershell"></a>Независимое масштабирование приложений с помощью PowerShell

Чтобы создать план с независимым масштабированием приложений, передайте параметр ```-PerSiteScaling $true``` в командлет ```New-AzAppServicePlan```.

```powershell
New-AzAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Чтобы включить масштабирование с помощью существующего плана службы приложений, передайте параметр `-PerSiteScaling $true` в командлет ```Set-AzAppServicePlan```.

```powershell
# Enable per-app scaling for the App Service Plan using the "PerSiteScaling" parameter.
Set-AzAppServicePlan -ResourceGroupName $ResourceGroup `
   -Name $AppServicePlan -PerSiteScaling $true
```

На уровне приложения настройте несколько экземпляров, которые приложение сможет использовать в плане службы приложений.

В следующем примере для приложения установлено ограничение на два экземпляра независимо от того, сколько экземпляров можно развернуть в пределах базового плана службы приложений.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzWebApp -ResourceGroupName $ResourceGroup -Name $webapp

# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2

# Post updated app back to azure
Set-AzWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` отличается от `$newapp.MaxNumberOfWorkers`. Функция "Независимое масштабирование приложений" определяет с параметры масштабирования приложения с помощью свойства `$newapp.SiteConfig.NumberOfWorkers`.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Независимое масштабирование приложений с помощью Azure Resource Manager

Следующий шаблон Azure Resource Manager создает:

- план службы приложений, в пределах которого можно развернуть до 10 экземпляров;
- приложение, для которого настроено масштабирование до пяти экземпляров максимум.

План службы приложений задает для свойства **PerSiteScaling** значение true (`"perSiteScaling": true`). Приложение задает **количество рабочих ролей**, равное 5 (`"properties": { "numberOfWorkers": "5" }`).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a>Рекомендуемая конфигурация для высокой плотности размещения

Независимое масштабирование приложений — это функция, которая включается как в глобальных регионах Azure, так и в [средах службы приложений](environment/app-service-app-service-environment-intro.md). Тем не менее рекомендуется использовать среды службы приложений, чтобы воспользоваться преимуществами они предоставляют дополнительные возможности и увеличить емкость плана службы приложений.  

Чтобы настроить высокую плотность размещения для приложений, выполните следующие действия.

1. Назначить план службы приложений с высокой плотностью плана и масштабировать его с требуемой производительностью.
1. Задайте для флага `PerSiteScaling` плана службы приложений значение "true".
1. Новые приложения будут созданы и назначены этому плану службы приложений, при этом для свойства **numberOfWorkers** будет задано значение **1**.
   - Такая конфигурация позволит достичь максимальной плотности.
1. Число рабочих ролей можно настроить отдельно для каждого приложения, чтобы предоставить дополнительные ресурсы согласно требованиям. Например:
   - Для приложения с высоким уровнем использования свойству **numberOfWorkers** можно присвоить значение **3**, чтобы обеспечить большую вычислительную мощность для этого приложения.
   - Для приложений с низким уровнем использования свойству **numberOfWorkers** можно присвоить значение **1**.

## <a name="next-steps"></a>Дальнейшие действия

- [Подробный обзор планов службы приложений Azure](overview-hosting-plans.md)
- [Введение в среду службы приложения](environment/app-service-app-service-environment-intro.md)
