---
title: Пример скрипта Azure CLI. Маршрутизация трафика через виртуальный сетевой модуль | Документация Майкрософт
description: Пример скрипта Azure CLI. Маршрутизация трафика через виртуальный сетевой модуль брандмауэра.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: f077080793ef595019667028188a2a98dcbd2c0c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60624468"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Маршрутизация трафика через виртуальный сетевой модуль

В этом примере скрипта создается виртуальная сеть с интерфейсной и внутренней подсетями. Скрипт также создает виртуальную машину с IP-пересылкой, которая позволяет маршрутизировать трафик между двумя подсетями. После выполнения скрипта вы можете развернуть на виртуальной машине программное обеспечение сети, например брандмауэр.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Пример скрипта


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной сети и групп безопасности сети этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [az group create](/cli/azure/group) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az network vnet create](/cli/azure/network/vnet) | Создает виртуальную сеть Azure и интерфейсную подсеть. |
| [az network subnet create](/cli/azure/network/vnet/subnet) | Создает внутреннюю подсеть и подсеть DMZ. |
| [az network public-ip create](/cli/azure/network/public-ip) | Создает общедоступный IP-адрес для доступа к виртуальной машине из Интернета. |
| [az network nic create](/cli/azure/network/nic) | Создает виртуальный сетевой интерфейс и включает IP-пересылку для него. |
| [az network nsg create](/cli/azure/network/nsg) | Создает группу безопасности сети (NSG). |
| [az network nsg rule create](/cli/azure/network/nsg/rule) | Создает правила группы безопасности сети, которые разрешают входящий трафик по портам HTTP и HTTPS к виртуальной машине. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet)| Связывает группы безопасности сети и таблицы маршрутов с подсетями. |
| [az network route-table create](/cli/azure/network/route-table#az-network-route-table-create)| Создает таблицу маршрутов для всех маршрутов. |
| [az network route-table route create](/cli/azure/network/route-table/route#az-network-route-table-route-create)| Создает маршруты для маршрутизации трафика между подсетями и Интернетом через виртуальную машину. |
| [az vm create](/cli/azure/vm) | Создает виртуальную машину и присоединяет к ней сетевой адаптер. Эта команда также указывает образ виртуальной машины и учетные данные администратора. |
| [az group delete](/cli/azure/group) | Удаляет группу ресурсов и все содержащиеся в ней ресурсы. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).

Дополнительные примеры сценариев Azure CLI для сетей Azure см. в [обзоре документации по сетям Azure](../cli-samples.md).
