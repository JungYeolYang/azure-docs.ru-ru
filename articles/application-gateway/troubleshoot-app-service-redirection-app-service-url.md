---
title: Устранение неполадок шлюза приложений Azure с помощью службы приложений — перенаправление на URL-адрес службы приложений
description: Статья содержит сведения о том, как решить эту проблему перенаправления при использовании шлюза приложений Azure с помощью службы приложений Azure
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: f456cfec82a315a2be877a52e4f3f1850b992736
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60715203"
---
# <a name="troubleshoot-application-gateway-with-app-service"></a>Устранение неполадок шлюза приложений с помощью службы приложений

Узнайте, как диагностировать и устранить проблемы, возникающие с помощью шлюза приложений и службы приложений в качестве сервера базы данных.

## <a name="overview"></a>Обзор

В этой статье вы узнаете способы устранения следующих проблем:

> [!div class="checklist"]
> * URL-адрес службы приложений, получение доступ в браузере, при отсутствии перенаправление
> * Домен службы приложений ARRAffinity Cookie присвоено имя узла службы приложений (example.azurewebsites.net) вместо исходного узла

При настройке общедоступные службы приложений в серверном пуле шлюза приложений и при наличии перенаправления, заданные в коде приложения, может появиться, при доступе к шлюзу приложений, вы будете перенаправлены с помощью браузера напрямую к приложению URL-адрес службы.

Эта проблема может происходить из-за следующих причин:

- У вас есть перенаправления, настроены в вашей службе приложений. Перенаправления может быть простым добавлением к запросу конечной косой чертой.
- У вас есть проверка подлинности Azure AD, которая вызывает перенаправление.
- Вы включили параметр «Выбрать адрес имени узла из серверной части» в параметрах HTTP шлюза приложений.
- У вас нет личного домена, зарегистрированного в службе приложений.

Кроме того при использовании службы приложений за шлюзом приложений и при использовании личного домена для доступа к шлюзу приложений, может появиться, значение домена файла cookie ARRAffinity установлено службой приложений будет содержать имя домена «example.azurewebsites.net». Исходное имя вашего узла, использовать в качестве домена файла cookie, выполните решение в этой статье.

## <a name="sample-configuration"></a>Пример конфигурации

- Прослушиватель HTTP: Основные или нескольких сайтов
- Пул адресов серверной части: Служба приложений
- Параметры HTTP: «Выберите имя узла из адресов серверной части» включен
- Пробы: «Выберите имя узла из параметры HTTP» включен

## <a name="cause"></a>Причина:

Служба приложений может осуществляться только с помощью настроенных именах узлов в настройках личного домена, по умолчанию, это «example.azurewebsites.net», и если вы хотите получить доступ к вашей службе приложений, с помощью шлюза приложений с именем узла, не зарегистрирован в службе приложений или с помощью Шлюз приложений полное доменное имя, необходимо переопределить имя узла в исходном запросе имя узла службы приложений.

Чтобы добиться этого с помощью шлюза приложений, мы используем параметр «Выбрать адрес имени узла из серверной части» в параметрах HTTP и для проверки работы, мы используем «Выберите имя узла из параметры HTTP серверной части» в конфигурации пробы.

![appservice-1](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Из-за этого при служба приложений выполняет перенаправление, используется имя узла «example.azurewebsites.net» в заголовке Location вместо исходного имени узла, если не настроено. Вы можете проверить пример заголовки запросов и ответов ниже.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://example.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=example.azurewebsites.net

X-Powered-By: ASP.NET
```
В приведенном выше примере можно заметить, что заголовок ответа содержит код состояния 301 для перенаправления и заголовок location имеет имя узла в службе приложений вместо исходного имени узла «www.contoso.com».

## <a name="solution"></a>Решение

Эту проблему можно устранить, не выполняя перенаправление на стороне приложения, тем не менее, если это невозможно, необходимо передать один и тот же заголовок узла, получающий шлюза приложений в службе приложений также вместо выполнения переопределение узла.

После этого, служба приложений выполнит перенаправление (если таковые имеются) на том же исходный заголовок узла, который указывает на шлюз приложений и не свой собственный.

Для этого необходимо владельцем и выполните действия, описанные ниже.

- Зарегистрируйте домен в список личного домена службы приложений. Для этого необходимо иметь запись CNAME в доменном имени указывает на полное доменное имя службы приложений. Дополнительные сведения см. в разделе [сопоставление существующего настраиваемого DNS-имени в службе приложений Azure](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

![appservice-2](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- После этого, службы приложений — Готово к приему имя узла «www.contoso.com». Теперь измените запись CNAME в DNS должен указывать обратно на полное доменное имя шлюза приложений. Например «appgw.eastus.cloudapp.azure.com».

- Убедитесь, что при выполнении запроса DNS домена «www.contoso.com» разрешается в полное доменное имя шлюза приложений.

- Задайте вашей пользовательской пробы для отключения «Выберите имя узла из параметры HTTP серверной части». Это можно сделать на портале, снимите флажок в параметры проверки и в PowerShell, не используя - PickHostNameFromBackendHttpSettings, включите в команду Set-AzApplicationGatewayProbeConfig. В поле имя узла проверки введите вашей полное доменное имя службы приложений «example.azurewebsites.net», как проверка запросов, отправленных из шлюза приложений это будет нести в заголовке узла.

  > [!NOTE]
  > Во время выполнения к следующему шагу, убедитесь, что "вашей пользовательской пробы" не связана в параметрах HTTP серверной части, так как ваши параметры для HTTP по-прежнему имеет параметр «Выбрать имя узла из серверного адреса» включена на этом этапе.

- Задайте параметры HTTP шлюза приложений чтобы отключить «Выберите имя узла из серверной части Address». Это можно сделать на портале, сняв флажок, и в PowerShell, не используя - PickHostNameFromBackendAddress переключаться в команде Set-AzApplicationGatewayBackendHttpSettings.

- Связать пользовательский зонд обратно на параметры HTTP серверной части и убедитесь в работоспособности серверной части в том случае, если он находится в работоспособном состоянии.

- После этого, шлюз приложений теперь пересылать же именем узла «www.contoso.com» в службе приложений и перенаправление произойдет на том же имени узла. Вы можете проверить пример заголовки запросов и ответов ниже.

Чтобы реализовать описанные выше с помощью PowerShell для настройки существующего действия, выполните приведенный ниже пример скрипта PowerShell. Обратите внимание на то, как мы не использовали - PickHostname переключатели в конфигурации пробы и параметры HTTP.

```azurepowershell-interactive
$gw=Get-AzApplicationGateway -Name AppGw1 -ResourceGroupName AppGwRG
Set-AzApplicationGatewayProbeConfig -ApplicationGateway $gw -Name AppServiceProbe -Protocol Http -HostName "example.azurewebsites.net" -Path "/" -Interval 30 -Timeout 30 -UnhealthyThreshold 3
$probe=Get-AzApplicationGatewayProbeConfig -Name AppServiceProbe -ApplicationGateway $gw
Set-AzApplicationGatewayBackendHttpSettings -Name appgwhttpsettings -ApplicationGateway $gw -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 30
Set-AzApplicationGateway -ApplicationGateway $gw
```
  ```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
  ```
  ## <a name="next-steps"></a>Дальнейшие действия

Если описанные выше шаги не устранят проблему, отправьте [запрос в службу поддержки](https://azure.microsoft.com/support/options/).
