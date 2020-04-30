---
title: Создание общей локальной среды выполнения интеграции с помощью PowerShell
description: Узнайте, как создать общую локальную среду выполнения интеграции в Фабрике данных Azure, обеспечивающую нескольким фабрикам данных доступ к среде выполнения интеграции.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: abnarain
author: nabhishek
manager: anansub
ms.custom: seo-lt-2019
ms.date: 10/31/2018
ms.openlocfilehash: 0f018d6b94d1c5b9d9002a767b3ebceb6c9c746c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82106633"
---
# <a name="create-a-shared-self-hosted-integration-runtime-in-azure-data-factory"></a>Создание общей локальной среды выполнения интеграции в фабрике данных Azure

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

В этом руководство показано, как создать общую локальную среду выполнения интеграции в фабрике данных Azure. Общую локальную среду выполнения интеграции затем можно использовать в другой фабрике данных.

## <a name="create-a-shared-self-hosted-ir-using-azure-data-factory-ui"></a>Создание общей локальной среды IR с помощью пользовательского интерфейса фабрики данных Azure

Чтобы создать общий собственный IR-объект с помощью пользовательского интерфейса фабрики данных Azure, можно выполнить следующие действия.

1. В локальной среде выполнения интеграции для совместного использования предоставьте разрешения фабрике данных, в которой нужно создать связанную среду IR.
      
    ![Кнопка предоставления разрешений на вкладке "Общий доступ"](media/create-self-hosted-integration-runtime/grant-permissions-IR-sharing.png)
      
    ![Выбранные элементы для назначения разрешений](media/create-self-hosted-integration-runtime/3_rbac_permissions.png)     
    
2. Запишите идентификатор ресурса локальной среды IR, к которой нужно предоставить общий доступ.
      
   ![Расположение идентификатора ресурса](media/create-self-hosted-integration-runtime/4_ResourceID_self-hostedIR.png)
    
3. В фабрике данных, которой были предоставлены разрешения, создайте новую локальную среду IR (связанную) и введите идентификатор ресурса.
      
   ![Кнопка создания связанной локальной среды выполнения интеграции](media/create-self-hosted-integration-runtime/6_create-linkedIR_2.png)
      
    ![Поля для имени и идентификатора ресурса](media/create-self-hosted-integration-runtime/6_create-linkedIR_3.png)

## <a name="create-a-shared-self-hosted-ir-using-azure-powershell"></a>Создание общей локальной среды IR с помощью Azure PowerShell

Чтобы создать общедоступную локальную среду IR с помощью Azure PowerShell, можно выполнить следующие действия. 
1. Создали фабрику данных. 
1. Создайте локальную среду выполнения интеграции.
1. Предоставьте общий доступ к локальной среде выполнения интеграции другим фабриками данных.
1. Создайте связанную среду выполнения интеграции.
1. Отмените общий доступ.

### <a name="prerequisites"></a>Предварительные требования 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Подписка Azure**. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу. 

- **Azure PowerShell**. Выполните инструкции из руководства [Install Azure PowerShell on Windows with PowerShellGet](https://docs.microsoft.com/powershell/azure/install-az-ps) (Установка Azure PowerShell в Windows c помощью PowerShellGet). С помощью PowerShell выполните скрипт, чтобы создать локальную среду выполнения интеграции, которую можно использовать совместно с другими фабриками данных. 

> [!NOTE]  
> Чтобы получить список регионов Azure, в которых сейчас доступна Фабрика данных, выберите интересующие вас регионы на странице [Доступность продуктов по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory).

### <a name="create-a-data-factory"></a>Создание фабрики данных

1. Откройте интегрированную среду сценариев (ISE) Windows PowerShell.

1. Создайте переменные. Скопируйте и вставьте следующий скрипт. Замените переменные, такие как **SubscriptionName** и **ResourceGroupName**, фактическими значениями: 

    ```powershell
    # If input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$". 
    $SubscriptionName = "[Azure subscription name]" 
    $ResourceGroupName = "[Azure resource group name]" 
    $DataFactoryLocation = "EastUS" 

    # Shared Self-hosted integration runtime information. This is a Data Factory compute resource for running any activities 
    # Data factory name. Must be globally unique 
    $SharedDataFactoryName = "[Shared Data factory name]" 
    $SharedIntegrationRuntimeName = "[Shared Integration Runtime Name]" 
    $SharedIntegrationRuntimeDescription = "[Description for Shared Integration Runtime]"

    # Linked integration runtime information. This is a Data Factory compute resource for running any activities
    # Data factory name. Must be globally unique
    $LinkedDataFactoryName = "[Linked Data factory name]"
    $LinkedIntegrationRuntimeName = "[Linked Integration Runtime Name]"
    $LinkedIntegrationRuntimeDescription = "[Description for Linked Integration Runtime]"
    ```

1. Войдите в систему и выберите подписку. Добавьте следующий код в скрипт, чтобы войти и выбрать подписку Azure.

    ```powershell
    Connect-AzAccount
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

1. Создайте группу ресурсов и фабрику данных.

    > [!NOTE]  
    > Этот шаг является необязательным. Если у вас уже есть фабрика данных, пропустите этот шаг. 

    Создайте [группу ресурсов Azure](../azure-resource-manager/management/overview.md) с помощью команды [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). Группа ресурсов — это логический контейнер, в котором ресурсы Azure развертываются и администрируются как группа. В следующем примере создается группа ресурсов с именем `myResourceGroup` в расположении WestEurope. 

    ```powershell
    New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
    ```

    Чтобы создать фабрику данных, выполните следующие команды: 

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                             -Location $DataFactoryLocation `
                             -Name $SharedDataFactoryName
    ```

### <a name="create-a-self-hosted-integration-runtime"></a>Создание локальной среды выполнения интеграции

> [!NOTE]  
> Этот шаг является необязательным. Если у вас уже есть локальная среда выполнения интеграции, которую вы хотите использовать совместно с другими фабриками данных, пропустите этот шаг.

Выполните следующую команду, чтобы создать локальную среду выполнения интеграции.

```powershell
$SharedIR = Set-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Type SelfHosted `
    -Description $SharedIntegrationRuntimeDescription
```

#### <a name="get-the-integration-runtime-authentication-key-and-register-a-node"></a>Получение ключа проверки подлинности для среды выполнения интеграции и регистрация узла

Выполните следующую команду, чтобы получить ключ проверки подлинности для локальной среды выполнения интеграции.

```powershell
Get-AzDataFactoryV2IntegrationRuntimeKey `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName
```

Ответ содержит ключ для локальной среды выполнения интеграции. Используйте этот ключ при регистрации узла среды выполнения интеграции.

#### <a name="install-and-register-the-self-hosted-integration-runtime"></a>Установка и регистрация локальной среды выполнения интеграции

1. Скачайте установщик локальной среды выполнения интеграции из [Integration Runtime Фабрики данных Azure](https://aka.ms/dmg).

2. Запустите установщик, чтобы установить локальную среду выполнения интеграции на локальном компьютере.

3. Зарегистрируйте новую локальную среду выполнения интеграции с помощью ключа проверки подлинности, полученного на предыдущем этапе.

### <a name="share-the-self-hosted-integration-runtime-with-another-data-factory"></a>Предоставление общего доступа к локальной среде выполнения интеграции другой фабрике данных

#### <a name="create-another-data-factory"></a>Создание другой фабрикой данных

> [!NOTE]  
> Этот шаг является необязательным. Если у вас уже есть фабрика данных, которой вы хотите предоставить общий доступ, пропустите этот шаг.

```powershell
$factory = Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
    -Location $DataFactoryLocation `
    -Name $LinkedDataFactoryName
```
#### <a name="grant-permission"></a>Предоставление разрешения

Предоставьте разрешение фабрике данных, которой требуется доступ к созданной и зарегистрированной локальной среде выполнения интеграции.

> [!IMPORTANT]  
> Не пропускайте этот шаг!

```powershell
New-AzRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId ` #MSI of the Data Factory with which it needs to be shared
    -RoleDefinitionName 'Contributor' `
    -Scope $SharedIR.Id
```

### <a name="create-a-linked-self-hosted-integration-runtime"></a>Создание связанной локальной среды выполнения интеграции

Выполните следующую команду, чтобы создать связанную локальную среду выполнения интеграции.

```powershell
Set-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $LinkedDataFactoryName `
    -Name $LinkedIntegrationRuntimeName `
    -Type SelfHosted `
    -SharedIntegrationRuntimeResourceId $SharedIR.Id `
    -Description $LinkedIntegrationRuntimeDescription
```

Теперь можно использовать эту связанную среду выполнения интеграции в любой связанной службе. Связанная среда выполнения интеграции использует общую среду выполнения интеграции для выполнения действий.

### <a name="revoke-integration-runtime-sharing-from-a-data-factory"></a>Отмена для фабрики данных общего доступа к среде выполнения интеграции

Выполните следующую команду, чтобы отменить для фабрики данных доступ к общей среде выполнения интеграции:

```powershell
Remove-AzRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId `
    -RoleDefinitionName 'Contributor' `
    -Scope $SharedIR.Id
```

Выполните следующую команду для общей среды выполнения интеграции, чтобы удалить имеющуюся связанную среду выполнения интеграции:

```powershell
Remove-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Links `
    -LinkedDataFactoryName $LinkedDataFactoryName
```

### <a name="next-steps"></a>Дальнейшие шаги

- Ознакомьтесь с [основными понятиями среды выполнения интеграции в Фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/concepts-integration-runtime).

- Узнайте, как [создать локальную среду выполнения интеграции на портале Azure](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime).
