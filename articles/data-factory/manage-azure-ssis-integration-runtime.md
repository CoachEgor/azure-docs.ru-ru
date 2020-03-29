---
title: Перенастройка среды выполнения интеграции SQL Server Integration Services Azure
description: Узнайте, как перенастроить среду выполнения интеграции Azure-SSIS в фабрике данных Azure после ее подготовки.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: anandsub
ms.openlocfilehash: fbac52d65433f2137d565ca60fcf754e49199640
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931392"
---
# <a name="reconfigure-the-azure-ssis-integration-runtime"></a>Перенастройка среды выполнения интеграции SQL Server Integration Services Azure
Из этой статьи вы узнаете, как перенастроить существующую среду выполнения интеграции SQL Server Integration Services Azure. Сведения о создании среды выполнения интеграции SQL Server Integration Services Azure в Фабрике данных Azure см. в [этой статье](create-azure-ssis-integration-runtime.md).  

## <a name="data-factory-ui"></a>Пользовательский интерфейс Фабрики данных 
Пользовательский интерфейс фабрики данных можно использовать для остановки, изменения, повторной настройки и удаления среды выполнения интеграции Azure SSIS. 

1. В **uI Data Factory**переключитесь на вкладку **Edit.** Чтобы запустить uI Data Factory, щелкните **Автор & монитор** на главной странице фабрики данных.
2. В области слева щелкните **Подключения**.
3. На панели справа перейдите на вкладку **Integration Runtimes** (Среды выполнения интеграции). 
4. С помощью кнопок в столбце действий можно **остановить**, **изменить** или **удалить** среду выполнения интеграции. Кнопка **Код** в столбце **Действия** позволяет просмотреть определение JSON, связанное со средой выполнения интеграции.  
    
    ![Действия для среды выполнения интеграции Azure SSIS](./media/manage-azure-ssis-integration-runtime/actions-for-azure-ssis-ir.png)

### <a name="to-reconfigure-an-azure-ssis-ir"></a>Перенастройка среды выполнения интеграции Azure SSIS
1. Чтобы остановить среду выполнения интеграции, нажмите кнопку **Остановить** в столбце **Действия**. Чтобы обновить представление списка, нажмите кнопку **Обновить** на панели инструментов. Когда среда выполнения интеграции будет остановлена, вы увидите, что первое действие позволяет запустить среду. 

    ![Действия для среды выполнения интеграции Azure SSIS после ее остановки](./media/manage-azure-ssis-integration-runtime/actions-after-ssis-ir-stopped.png)
2. Чтобы изменить или повторно настроить среду выполнения интеграции, нажмите кнопку **Изменить** в столбце **Действия**. В окне **Integration Runtime Setup** (Настройка среды выполнения интеграции) измените параметры (например, размер узла, количество узлов или максимальное число параллельных выполнений на одном узле). 
3. Чтобы повторно запустить среду выполнения интеграции, нажмите кнопку **Запустить** в столбце **Действия**.     

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

После подготовки и запуска экземпляра среды выполнения Azure SSIS его можно перенастроить, выполнив последовательность командлетов PowerShell `Stop` - `Set` - `Start`. Например, следующий сценарий PowerShell изменяет количество узлов, выделенных для экземпляра среды выполнения интеграции Azure-SSIS, до пяти.

### <a name="reconfigure-an-azure-ssis-ir"></a>Перенастройка Azure-SSIS IR

1. Во-первых, остановить время выполнения интеграции Azure-SSIS с помощью [cmdlet Stop-AzDataFactoryV2IntegrationRuntime.](/powershell/module/az.datafactory/stop-Azdatafactoryv2integrationruntime) Эта команда освобождает все узлы и прекращает выставление счетов.

    ```powershell
    Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. Затем переназначите ИК Azure-SSIS с помощью [cmdlet Set-AzDataFactoryV2IntegrationRuntime.](/powershell/module/az.datafactory/set-Azdatafactoryv2integrationruntime) В следующем примере команды развертывается среда выполнения интеграции Azure-SSIS на пять узлов.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. Затем запустите время выполнения интеграции Azure-SSIS с помощью cmdlet [Start-AzDataFactoryV2IntegrationRuntime.](/powershell/module/az.datafactory/start-Azdatafactoryv2integrationruntime) Эта команда выделяет все ее узлы для запуска пакетов SSIS.   

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

### <a name="delete-an-azure-ssis-ir"></a>Удаление среды выполнения интеграции Azure-SSIS
1. Сначала перечислите все имеющиеся среды выполнения интеграции Azure-SSIS в фабрике данных.

    ```powershell
    Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName -Status
    ```
2. Затем остановите все имеющиеся среды выполнения интеграции Azure-SSIS в фабрике данных.

    ```powershell
    Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
3. После чего удалите все имеющиеся среды выполнения интеграции Azure-SSIS в фабрике данных одну за другой.

    ```powershell
    Remove-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
4. Наконец, удалите фабрику данных.

    ```powershell
    Remove-AzDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
    ```
5. Если вы создавали группу ресурсов, удалите ее.

    ```powershell
    Remove-AzResourceGroup -Name $ResourceGroupName -Force 
    ```

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о среде выполнения Azure SSIS см. в следующих разделах: 

- [Запуск интеграции Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). В этой статье содержатся общие сведения о средах выполнения интеграции в целом, включая Azure SSIS IR. 
- [Развертывание пакетов служб интеграции SQL Server (SSIS) в Azure](tutorial-create-azure-ssis-runtime-portal.md). Эта статья содержит пошаговые инструкции для создания Azure SSIS IR и использует базу данных SQL Azure для размещения каталога SSIS. 
- [Создание среды выполнения интеграции Azure SSIS](create-azure-ssis-integration-runtime.md). Эта статья дополняет соответствующее руководство, а также предоставляет инструкции по использованию Управляемого экземпляра Базы данных SQL Azure и присоединению среды выполнения интеграции к виртуальной сети. 
- [Присоединение среды выполнения интеграции Azure SSIS к виртуальной сети](join-azure-ssis-integration-runtime-virtual-network.md) В этой статье содержатся общие сведения о присоединении среды выполнения интеграции Azure–SSIS к виртуальной сети Azure. В ней также показано, как настроить виртуальную сеть, чтобы присоединить среду выполнения интеграции Azure–SSIS к виртуальной сети с помощью портала Azure. 
- [Мониторинг среды выполнения интеграции в фабрике данных Azure](monitor-integration-runtime.md#azure-ssis-integration-runtime). В этом статье показано, как извлечь сведения о среде выполнения интеграции Azure SSIS и описания состояний из возвращаемых данных. 
 
