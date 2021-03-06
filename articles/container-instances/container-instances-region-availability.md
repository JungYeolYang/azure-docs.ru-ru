---
title: Доступность ресурсов службы "Экземпляры контейнеров Azure"
description: Доступность вычислительных ресурсов и памяти для службы "Экземпляры контейнеров Azure" в разных регионах Azure.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 03/01/2019
ms.author: danlep
ms.openlocfilehash: 1ca23a95c746139963aa70ed20bb888152fd5cd8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554902"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Доступность ресурсов службы "Экземпляры контейнеров Azure" в регионах Azure

В этой статье подробно описывается доступность вычислительных ресурсов и ресурсов памяти экземпляров Azure в регионах Azure. 

Представленные значения — это максимальные ресурсы, доступные для развертывания [группы контейнеров](container-instances-container-groups.md). Значения актуальны на момент публикации. Чтобы получить последние сведения, воспользуйтесь [API List Capabilities](/rest/api/container-instances/listcapabilities/listcapabilities). 

> [!NOTE]
> Группы контейнеров, созданные в пределах границ этих ресурсов, зависят от доступности в регионе развертывания. Если в регионе высокая нагрузка, при развертывании экземпляров могут возникать сбои. Чтобы устранить такой сбой развертывания, попробуйте развернуть экземпляры с более низкими параметрами ресурсов или выполнить развертывание позднее.

Сведения о квотах и ​​других ограничениях в развертываниях см. в разделе [Квоты и доступность по регионам для службы "Экземпляры контейнеров Azure"](container-instances-quotas.md).

## <a name="availability---general"></a>Доступность: общая

| Расположение | ОС | ЦП | Память (ГБ) |
| -------- | -- | :---: | :-----------: |
| Центральная Канада, центральная часть США, восточная часть США 2, центрально-южная часть США | Linux | 4. | 16 |
| Восточная часть США, Северная Европа, Западная Европа, западная часть США, западная часть США 2 | Linux | 4. | 14 |
| Восточная часть Японии | Linux | 2 | 8 |
| Восточная Австралия, Юго-Восточная Азия | Linux | 2 | 7 |
| Центральная Индия, Восточная Азия, центрально-северная часть США, Южная Индия | Linux | 2 | 3,5 |
| Восточная часть США, Западная Европа, восточная часть США |  Windows | 4. | 14 |
| Восточная Австралия, восточная часть США 2, Восточная Япония, западная часть США 2, Северная Европа, Центральная Индия, Центральная Канада, центральная часть США, центрально-северная часть США, центрально-южная часть США, Юго-Восточная Азия, Южная Индия |  Windows | 2 | 3,5 |

## <a name="availability---virtual-network-deployment-preview"></a>Доступность: развертывание виртуальной сети (предварительный просмотр)

Следующие регионы и ресурсы доступны для группы контейнеров, развернутых в [виртуальной сети Azure](container-instances-vnet.md) (предварительный просмотр).

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Доступность: ресурсы GPU (предварительная версия)

Следующие регионы и ресурсы доступны для группы контейнеров, развернутых с помощью [ресурсов GPU](container-instances-gpu.md) (предварительная версия).

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Дополнительная информация

Если вы хотите увидеть дополнительные регионы или увеличить доступность ресурсов, сообщите об этом команде, перейдя по адресу [​​aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Сведения об устранении неполадок развертывания экземпляра контейнера см. в статье [Устранение распространенных неполадок с помощью службы "Экземпляры контейнеров Azure"](container-instances-troubleshooting.md).
