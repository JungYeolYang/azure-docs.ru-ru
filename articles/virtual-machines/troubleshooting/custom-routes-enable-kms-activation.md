---
title: Использование настраиваемых маршрутов Azure для активации сервера управления ключами с помощью принудительного туннелирования | Документация Майкрософт
description: Узнайте, как использовать настраиваемые маршруты Azure для активации сервера управления ключами с использованием принудительного туннелирования в Azure.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: b121996530ea0618fc757f1ae12dfafde10ed7bb
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2019
ms.locfileid: "55979383"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Активация Windows завершается ошибкой в случае принудительного туннелирования

В этой статье описывается, как разрешить проблемы активации сервера управления ключами, которые могут возникнуть при включении принудительного туннелирования в подключении VPN типа "сеть — сеть" или использовании ExpressRoute.

## <a name="symptom"></a>Симптом

Вы включаете [принудительное туннелирование](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) в подсетях виртуальной сети Azure, чтобы направить весь интернет-трафик обратно в локальную сеть. В этом случае виртуальные машины Azure под управлением Windows Server 2012 R2 или более поздних версий могут успешно активировать Windows. Но виртуальные машины под управлением более ранней версии Windows не могут активировать Windows.

## <a name="cause"></a>Причина:

Виртуальные машины Windows Azure должны подключиться к серверу управления ключами Azure для активации Windows. Для активации требуется, чтобы запрос на активацию поступал с общедоступного IP-адреса Azure. В случае принудительного туннелирования активация будет завершаться ошибкой, так как запрос на активацию поступает из вашей локальной сети вместо общедоступного IP-адреса Azure.

## <a name="solution"></a>Решение

Чтобы устранить эту проблему, используйте настраиваемый маршрут Azure для направления трафика активации на сервер управления ключами Azure.

IP-адрес сервера управления ключами для глобального облака Azure — 23.102.135.246. Имя DNS — kms.core.windows.net. Если вы используете другие платформы Azure, например Azure для Германии, необходимо использовать IP-адрес соответствующего сервера управления ключами. Дополнительные сведения приведены в таблице ниже.

|платформа| DNS СЕРВЕРА УПРАВЛЕНИЯ КЛЮЧАМИ|IP-АДРЕС СЕРВЕРА УПРАВЛЕНИЯ КЛЮЧАМИ|
|------|-------|-------|
|Azure (глобальный)|kms.core.windows.net|23.102.135.246|
|Azure для Германии|kms.core.cloudapi.de|51.4.143.248|
|Azure для государственных организаций США|kms.core.usgovcloudapi.net|23.97.0.13|
|Azure China 21Vianet|kms.core.chinacloudapi.cn|42.159.7.249|


Чтобы добавить настраиваемый маршрут, выполните следующие действия.

### <a name="for-resource-manager-vms"></a>Виртуальные машины Resource Manager

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

1. Откройте Azure PowerShell и [войдите в свою подписку Azure](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Выполните следующие команды:

    ```powershell
    # First, get the virtual network that hosts the VMs that have activation problems. In this case, we get virtual network ArmVNet-DM in Resource Group ArmVNet-DM:

    $vnet = Get-AzVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out:

    $RouteTable = New-AzRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzRouteTable -RouteTable $RouteTable
    ```
3. Перейдите на виртуальную машину, на которой возникла проблема с активацией. Используйте команду [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping), чтобы проверить возможность доступа к серверу управления ключами.

        psping kms.core.windows.net:1688

4. Попробуйте активировать Windows, чтобы увидеть, устранена ли проблема.

### <a name="for-classic-vms"></a>Классические виртуальные машины

1. Откройте Azure PowerShell и [войдите в свою подписку Azure](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Выполните следующие команды:

    ```powershell
    # First, create a new route table:
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the route table that was created:
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route:
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply the KMS route table to the subnet that hosts the problem VMs (in this case, we apply it to the subnet that's named Subnet-1):
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Перейдите на виртуальную машину, на которой возникла проблема с активацией. Используйте команду [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping), чтобы проверить возможность доступа к серверу управления ключами.

        psping kms.core.windows.net:1688

4. Попробуйте активировать Windows, чтобы увидеть, устранена ли проблема.

## <a name="next-steps"></a>Дополнительная информация

- [Ключи установки клиента сервера управления ключами](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Просмотр и выбор методов активации](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)
