---
title: включение файла
description: включение файла
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: deabef0c2c3540e515fe72a161710c95a20fa86f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60320242"
---
Откройте консоль PowerShell с повышенными привилегиями.



Если вы используете Azure PowerShell локально, подключитесь к своей учетной записи Azure. Командлет *Connect-AzAccount* запросит учетные данные. После проверки подлинности он загрузит параметры учетной записи, чтобы они были доступны в Azure PowerShell. Если вы не запускаете PowerShell локально, а вместо этого используете пробную версию Azure Cloud Shell в браузере, можете пропустить этот шаг. Вы автоматически подключитесь к учетной записи Azure.

```azurepowershell
Connect-AzAccount
```

Если у вас есть несколько подписок Azure, запросите их список.

```azurepowershell-interactive
Get-AzSubscription
```

Укажите подписку, которую нужно использовать.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```