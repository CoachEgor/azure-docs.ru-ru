---
title: Настройка правила ограничения скорости WAF для передней дверцы — Azure PowerShell
description: Узнайте, как настроить правило ограничения скорости для существующей конечной точки передней дверцы.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 02/26/2020
ms.author: victorh
ms.openlocfilehash: 29f50b2cf9523b9266de2f73607b0099f32852e1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "87005418"
---
# <a name="configure-a-web-application-firewall-rate-limit-rule-using-azure-powershell"></a>Настройка правила ограничения скорости брандмауэра веб-приложения с помощью Azure PowerShell
Правило предельного числа запросов брандмауэра веб-приложения Azure (WAF) для передней дверцы Azure управляет количеством разрешенных клиентов в течение одной минуты.
В этой статье показано, как настроить правило ограничения скорости WAF, которое управляет количеством запросов, разрешенных клиентами, в веб-приложение, содержащее */промо* в URL-адресе с помощью Azure PowerShell.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования
Прежде чем приступить к настройке политики ограничения скорости, настройте среду PowerShell и создайте профиль передней дверцы.
### <a name="set-up-your-powershell-environment"></a>Настройка среды PowerShell
В Azure PowerShell доступен набор командлетов, которые используют модель [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) для управления ресурсами Azure. 

Вы можете установить [Azure PowerShell](https://docs.microsoft.com/powershell/azure/) на локальном компьютере и использовать его в любом сеансе PowerShell. Выполните инструкции на странице, чтобы войти с помощью учетных данных Azure, и установите команду AZ PowerShell Module.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Подключение к Azure с помощью интерактивного диалогового окна для входа
```
Connect-AzAccount

```
Перед установкой модуля Front дверь убедитесь, что установлена текущая версия PowerShellGet. Выполните следующую команду и повторно откройте PowerShell.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

#### <a name="install-azfrontdoor-module"></a>Установка модуля Az.FrontDoor 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Создание профиля Front Door
Создайте профиль передней дверцы, следуя инструкциям, приведенным в [кратком руководстве по созданию профиля передней дверцы.](../../frontdoor/quickstart-create-front-door.md)

## <a name="define-url-match-conditions"></a>Определение условий соответствия URL-адреса
Определите условие соответствия URL-адреса (URL-адрес содержит/промо) с помощью [New-азфронтдурвафматчкондитионобжект](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject).
Следующий пример соответствует */промо* в качестве значения переменной *RequestUri* :

```powershell-interactive
   $promoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
     -MatchVariable RequestUri `
     -OperatorProperty Contains `
     -MatchValue "/promo"
```
## <a name="create-a-custom-rate-limit-rule"></a>Создание настраиваемого правила ограничения скорости
Установите ограничение скорости с помощью [New-азфронтдурвафкустомрулеобжект](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject). В следующем примере устанавливается ограничение 1000. Запросы от любого клиента к странице акционная, превышающие 1000 в течение минуты, блокируются до начала следующей минуты.

```powershell-interactive
   $promoRateLimitRule = New-AzFrontDoorWafCustomRuleObject `
     -Name "rateLimitRule" `
     -RuleType RateLimitRule `
     -MatchCondition $promoMatchCondition `
     -RateLimitThreshold 1000 `
     -Action Block -Priority 1
```


## <a name="configure-a-security-policy"></a>Настройка политики безопасности

С помощью команды `Get-AzureRmResourceGroup` найдите имя группы ресурсов, содержащей профиль Front Door. Затем настройте политику безопасности с помощью настраиваемого правила ограничения скорости, используя [New-азфронтдурвафполици](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) в указанной группе ресурсов, которая содержит профиль передней дверцы.

В приведенном ниже примере используется имя группы ресурсов *myResourceGroupFD1* с предположением, что вы создали профиль передней дверцы с помощью инструкций, приведенных в статье [Краткое руководство по созданию передней дверцы](../../frontdoor/quickstart-create-front-door.md) .

 с помощью [New-азфронтдурвафполици](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell-interactive
   $ratePolicy = New-AzFrontDoorWafPolicy `
     -Name "RateLimitPolicyExamplePS" `
     -resourceGroupName myResourceGroupFD1 `
     -Customrule $promoRateLimitRule `
     -Mode Prevention `
     -EnabledState Enabled
```
## <a name="link-policy-to-a-front-door-front-end-host"></a>Связывание политики с интерфейсным узлом передней дверцы
Свяжите объект политики безопасности с существующим интерфейсным узлом передней дверцы и обновите свойства передней дверцы. Сначала получите объект Front дверь с помощью команды [Get-азфронтдур](/powershell/module/Az.FrontDoor/Get-AzFrontDoor) .
Затем задайте для свойства *вебаппликатионфиреваллполицилинк* внешнего интерфейса значение *resourceId* из "$ratePolicy", созданного на предыдущем шаге, с помощью команды [Set-азфронтдур](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) . 

В приведенном ниже примере используется имя группы ресурсов *myResourceGroupFD1* с предположением, что вы создали профиль передней дверцы с помощью инструкций, приведенных в статье [Краткое руководство по созданию передней дверцы](../../frontdoor/quickstart-create-front-door.md) . Кроме того, в следующем примере замените $frontDoorName именем профиля передней дверцы. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $ratePolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> Свойство *вебаппликатионфиреваллполицилинк* необходимо установить только один раз, чтобы связать политику безопасности с внешним интерфейсом передней дверцы. Последующие обновления политики автоматически применяются к интерфейсной части.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [передней дверце](../../frontdoor/front-door-overview.md). 


