---
title: Подключение к центру безопасности Azure с помощью PowerShell
description: В этом документе рассматривается процесс подключения центра безопасности Azure с помощью командлетов PowerShell.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/02/2018
ms.author: memildin
ms.openlocfilehash: b20b3c1e4216fe8065fbc8ac24c7d8097903fc5a
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686369"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>Автоматизированное подключение центра безопасности Azure с помощью PowerShell

Вы можете защитить рабочие нагрузки Azure программным способом с помощью модуля PowerShell центра безопасности Azure.
PowerShell позволяет автоматизировать задачи и избежать человеческих ошибок, возникающих в результате выполнения задач вручную. Это особенно полезно в крупномасштабных развертываниях, включающих десятки подписок с сотнями и тысячами ресурсов, которые должны быть защищены с самого начала.

Подключение центра безопасности Azure с помощью PowerShell позволяет программным образом автоматизировать подключение и управление вашими ресурсами Azure, а также добавить необходимые средства управления безопасностью.

В этой статье содержится пример сценария PowerShell, который можно изменить и использовать в вашей среде, чтобы развернуть центр безопасности для подписок. 

В этом примере мы включим центр безопасности для подписки с идентификатором d07c0080-170c-4c24-861d-9c817742786c и применим рекомендуемые параметры, которые обеспечивают высокий уровень защиты. Для этого мы используем уровень "Стандартный" центра безопасности, который предоставляет дополнительные возможности защиты и обнаружения угроз:

1. Задайте [уровень защиты "Стандартный" центра безопасности](https://azure.microsoft.com/pricing/details/security-center/). 
 
2. Задайте рабочую область Log Analytics, в которую Microsoft Monitoring Agent будет отправлять данные, собранные на виртуальных машинах в данной подписке. В этом примере мы используем существующую определенную пользователем рабочую область myWorkspace.

3. Активируйте автоматическую подготовку агента центра безопасности, во время которой [развертывается Microsoft Monitoring Agent](security-center-enable-data-collection.md#auto-provision-mma).

5. Задайте перспективы Организации в [качестве контактного лица по безопасности для оповещений центра безопасности и важных событий](security-center-provide-security-contact-details.md).

6. Присвойте [политики безопасности по умолчанию](tutorial-security-policy.md) центра безопасности.

## <a name="prerequisites"></a>Предварительные требования

Эти действия следует выполнять перед запуском командлетов центра безопасности:

1.  Запустите PowerShell от имени администратора.
2.  В PowerShell выполните следующие команды:
      
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Install-Module -Name Az.Security -Force

## <a name="onboard-security-center-using-powershell"></a>Подключение центра безопасности с помощью PowerShell

1.  Зарегистрируйте свои подписки в поставщике ресурсов центра безопасности:

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Register-AzResourceProvider -ProviderNamespace 'Microsoft.Security' 

2.  (Необязательно.) Установите уровень покрытия (ценовую категорию) подписок (если не определена, используется ценовая категория "Бесплатный"):

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Set-AzSecurityPricing -Name "default" -PricingTier "Standard"

3.  Настройте рабочую область Log Analytics, куда агенты будут отправлять отчеты. У вас должна быть уже созданная рабочая область Log Analytics, куда виртуальные машины подписки будут отправлять отчеты. Вы можете определить несколько подписок для отправки отчетов в ту же рабочую область. Если не определить рабочую область, будет использоваться установленная по умолчанию.

        Set-AzSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  Запустите автоматическую подготовку установки Microsoft Monitoring Agent на виртуальные машины Azure:
    
        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
    
        Set-AzSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > Рекомендуется включить автоматическую подготовку, чтобы убедиться, что виртуальные машины Azure автоматически защищены с помощью центра безопасности Azure.
    >

5.  (Необязательно.) Настоятельно рекомендуется указать сведения о контактном лице по вопросам безопасности для подключаемых подписок. Эта информация будет использоваться для отправки оповещений и уведомлений, созданных центром безопасности:

        Set-AzSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertAdmin -NotifyOnAlert 

6.  Назначьте инициативу политики центра безопасности по умолчанию:

        Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzPolicySetDefinition | where {$_.Properties.displayName -EQ '[Preview]: Enable Monitoring in Azure Security Center'}
        New-AzPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'

Вы успешно подключили центр безопасности Azure с помощью PowerShell.

Теперь вы можете использовать эти командлеты PowerShell со сценариями автоматизации, чтобы программно повторить эти операции для подписок и ресурсов. Это экономит время и уменьшает вероятность человеческой ошибки. Вы можете использовать этот [пример сценария](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) в качестве ориентира.






## <a name="see-also"></a>Дополнительные материалы
Дополнительные сведения об использовании PowerShell для автоматизации подключения к центру безопасности см. в следующей статье:

* [Az.Security](https://docs.microsoft.com/powershell/module/az.security).

Дополнительные сведения о центре безопасности см. в следующих статьях:

* [Настройка политик безопасности в Центре безопасности Azure](tutorial-security-policy.md) — узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — часто задаваемые вопросы об использовании этой службы.
