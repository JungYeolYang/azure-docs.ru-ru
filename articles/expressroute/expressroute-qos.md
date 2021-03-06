---
title: 'Требования к качеству обслуживания ExpressRoute: Azure | Документация Майкрософт'
description: На этой странице подробно описаны требования по настройке качества обслуживания и управлению им. В этой статье также рассматривается Skype для бизнеса и голосовые службы.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 9bdeb91b145f8c7f31be8c1dcd5c5158d50ff2f6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64712311"
---
# <a name="expressroute-qos-requirements"></a>Требования к качеству обслуживания для ExpressRoute
В Skype для бизнеса имеются различные рабочие нагрузки, требующие дифференцированного подхода к качеству обслуживания. Если вы планируете использовать голосовые службы с помощью ExpressRoute, то необходимо соблюдать указанные ниже требования.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> Требования к качеству обслуживания применяются только к пирингу Майкрософт. Значения DSCP в сетевом трафике, полученном через общедоступный или частный пиринг Azure, будут сброшены до "0". 
> 
> 

Следующая таблица предоставляет список пометок DSCP, используемые Microsoft Teams и Скайп для бизнеса. Дополнительные сведения см. в статье [Управление качеством обслуживания для Skype для бизнеса](https://docs.microsoft.com/SkypeForBusiness/manage/network-management/qos/managing-quality-of-service-QoS).

| **Класс трафика** | **Обработка (пометки DSCP)** | **Microsoft Teams и Скайп рабочих нагрузок** |
| --- | --- | --- |
| **Голосовая связь** |EF (46) |Голосовая связь Skype и Lync |
| **Интерактивный** |AF41 (34) |Видео, VBSS |
| |AF21 (18) |Совместный доступ к приложениям | 
| **По умолчанию** |AF11 (10) |Передача файлов |
| |CS0 (0) |Другие варианты |

* Вам необходимо классифицировать рабочие нагрузки и пометить правильные значения DSCP. Следуйте указанным [здесь](https://docs.microsoft.com/SkypeForBusiness/manage/network-management/qos/configuring-port-ranges-for-your-skype-clients#configure-quality-of-service-policies-for-clients-running-on-windows-10) инструкциям по настройке пометок DSCP в сети.
* Вам необходимо настроить и поддерживать несколько очередей качества обслуживания в сети. Голосовая связь должна быть изолированным классом и использовать обработку EF согласно документу [RFC 3246](https://www.ietf.org/rfc/rfc3246.txt). 
* Вы можете решить, какие механизм очередей, политику обнаружения перегрузки и пропускную способность использовать для каждого класса трафика. Тем не менее необходимо сохранить пометки DSCP для рабочих нагрузок Skype для бизнеса. Если вы используете пометки DSCP, которых нет в списке выше, например AF31 (26), то перед отправкой пакета в корпорацию Майкрософт необходимо перезаписать это значение DSCP и сделать его равным 0. Корпорация Майкрософт отправляет только те пакеты, которые помечены значениями DSCP, указанными в таблице выше. 

## <a name="next-steps"></a>Дальнейшие действия
* См. сведения о требованиях для [маршрутизации](expressroute-routing.md) и [преобразования сетевых адресов (NAT)](expressroute-nat.md).
* Чтобы настроить подключение ExpressRoute, см. следующие статьи:
  
  * [Создание канала ExpressRoute](expressroute-howto-circuit-classic.md)
  * [Настройка маршрутизации](expressroute-howto-routing-classic.md)
  * [Связывание виртуальной сети с каналом ExpressRoute](expressroute-howto-linkvnet-classic.md)

