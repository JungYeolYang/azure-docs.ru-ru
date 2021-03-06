---
title: Настройка правила ограничение скорости веб приложения брандмауэра для входной двери — Azure PowerShell
description: Узнайте, как настроить правило ограничение скорости для существующей конечной точки двери.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/16/2019
ms.author: kumud;tyao
ms.openlocfilehash: 3701a69ab72abf20a4f1608a1cee56c9cea38aca
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523644"
---
# <a name="configure-a-web-application-firewall-rate-limit-rule-using-azure-powershell"></a>Настройка web application скорость ограничение правила брандмауэра с помощью Azure PowerShell
Azure web приложения (WAF) скорость ограничение правило для входной двери Azure управляет количеством запросов, разрешенных с одного клиентского IP-адреса за период до минуты.
В этой статье показано, как настроить правило WAF скорость ограничение, которое управляет количеством запросов, разрешенных из одного клиента веб-приложению, которое содержит */promo* в URL-адрес, с помощью Azure PowerShell.

> [!IMPORTANT]
> Функция правила ограничения частоты WAF передовой линии для Azure в настоящее время находится в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Технические условия
Прежде чем начать настроить политику ограничения скорости, настройте среду PowerShell и создание передовой профиля.
### <a name="set-up-your-powershell-environment"></a>Настройка среды PowerShell
В Azure PowerShell доступен набор командлетов, которые используют модель [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) для управления ресурсами Azure. 

Вы можете установить [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) на локальном компьютере и использовать его в любом сеансе PowerShell. Следуйте инструкциям на странице, чтобы войти с использованием учетных данных Azure и установите модуль Az PowerShell.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Подключение к Azure с помощью интерактивного диалогового окна для входа
```
Connect-AzAccount

```
Перед установкой модуля Front Door убедитесь, что у вас установлена текущая версия PowerShellGet. Выполните следующую команду и снова откройте PowerShell.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

#### <a name="install-azfrontdoor-module"></a>Установка модуля Az.FrontDoor 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Создание профиля Front Door
Создайте профиль Front Door, следуя инструкциям, описанным в статье [Краткое руководство. Создание профиля передовой линии](quickstart-create-front-door.md)

## <a name="define-url-match-conditions"></a>Определить условия сопоставления URL-адрес
Определить условие соответствия URL-адрес (URL-адрес содержит /promo) с помощью [New AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject).
Следующий пример сопоставляет */promo* для параметра *RequestUri* переменной:

```powershell-interactive
   $promoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
     -MatchVariable RequestUri `
     -OperatorProperty Contains `
     -MatchValue "/promo"
```
## <a name="create-a-custom-rate-limit-rule"></a>Создайте правило ограничение уровня пользовательской скорости
Задать ограничение скорости с помощью [New AzFrontDoorCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject). В следующем примере ограничение присваивается значение 1000. Запросы из любого клиента рекламная акция на экземпляр страницы, превышает 1000 за одну минуту, блокируются до начала следующей минуты.

```powershell-interactive
   $promoRateLimitRule = New-AzFrontDoorCustomRuleObject `
     -Name "rateLimitRule" `
     -RuleType RateLimitRule `
     -MatchCondition $promoMatchCondition `
     -RateLimitThreshold 1000 `
     -Action Block -Priority 1
```


## <a name="configure-a-security-policy"></a>Настройка политики безопасности

С помощью команды `Get-AzureRmResourceGroup` найдите имя группы ресурсов, содержащей профиль Front Door. Настройте политику безопасности с правило ограничение уровня пользовательской скорости с помощью [New AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) в указанной группе ресурсов, содержащий профиль двери.

В приведенном ниже примере используется имя группы ресурсов *myResourceGroupFD1* при условии, что профиль Front Door создан с помощью инструкций, приведенных в статье [Краткое руководство. Создание профиля Front Door для высокодоступного глобального веб-приложения](quickstart-create-front-door.md).

 с помощью [New AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell-interactive
   $ratePolicy = New-AzFrontDoorWafPolicy `
     -Name "RateLimitPolicyExamplePS" `
     -resourceGroupName myResourceGroupFD1 `
     -Customrule $promoRateLimitRule `
     -Mode Prevention `
     -EnabledState Enabled
```
## <a name="link-policy-to-a-front-door-front-end-host"></a>Политики ссылки на входной двери переднего плана узел
Свяжите объект политики безопасности на существующий узел двери переднего плана и обновляет свойства двери. Сначала нужно получить объект двери с помощью [Get AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor) команды.
Затем задайте интерфейсной части *WebApplicationFirewallPolicyLink* свойства *resourceId* из «$ratePolicy» создан в предыдущем шаге, с помощью [AzFrontDoor набора](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) команда. 

В приведенном ниже примере используется имя группы ресурсов *myResourceGroupFD1* при условии, что профиль Front Door создан с помощью инструкций, приведенных в статье [Краткое руководство. Создание профиля Front Door для высокодоступного глобального веб-приложения](quickstart-create-front-door.md). Кроме того, в следующем примере замените $frontDoorName на имя профиля двери. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $ratePolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> Необходимо задать *WebApplicationFirewallPolicyLink* свойство один раз, чтобы связать политику безопасности двери переднего плана. Последующие политики обновления автоматически применяются к внешней сети.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [двери](front-door-overview.md) 


