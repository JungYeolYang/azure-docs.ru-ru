---
title: включение файла
description: включение файла
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: e87a82e985ed1d1794f9da00546f167ef01e1779
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/16/2019
ms.locfileid: "65815612"
---
## <a name="business-disaster-recovery"></a>Непрерывность бизнес-процессов и аварийное восстановление

В этом разделе описываются возможности Azure Time Series Insights, которая обеспечит приложений и служб, даже если произойдет авария (**аварийного восстановления для бизнеса**).

### <a name="high-availability"></a>Высокая надежность

Как службу Azure Time Series Insights предоставляет определенные **высокий уровень доступности** функции за счет использования избыточности на уровне региона Azure. Например, Microsoft Azure поддерживает возможности по аварийному восстановлению посредством Azure **межрегиональной доступностью** функции.

Возможности дополнительных высокий уровень доступности обеспечивается за счет Azure (и также доступна в любой экземпляр Time Series Insights):

1. **Отработка отказа**. Azure предоставляет [георепликации и загрузке балансировки](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
1. **Восстановление данных** и **восстановления хранилища**: Azure предоставляет [несколько вариантов для сохранения и восстановления данных](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
1. **Site Recovery** функции через [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Убедитесь в том включить эти функции Azure для обеспечения глобальной, между регионами, высокий уровень доступности, устройствами и пользователями.

> [!NOTE]
> Если Azure настроен для включения **межрегиональной доступностью**, никаких дополнительных межрегиональной доступностью настроек не требуется в Azure Time Series Insights.

### <a name="iot-and-event-hubs"></a>Интернета вещей и концентраторы событий

Некоторые службы Azure IoT также включать встроенные бизнеса функции аварийного восстановления:

1. [Высокого уровня доступности аварийное восстановление центра Интернета вещей](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr) включая избыточности внутри регионов.
1. [Политики концентраторов событий](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).
1. [Избыточность хранилища Azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

Интеграция Time Series Insights с этими службами обеспечивает аварийного дополнительные возможности восстановления. Например данные телеметрии, отправляемые в концентратор событий может быть сохранена в резервной копии базы данных хранилища больших двоичных объектов.

### <a name="time-series-insights"></a>Аналитика временных рядов

Существует несколько способов хранить свои данные, приложения и службы, даже если они при сбое Time Series Insights. Также можно предположить, что полный, повторяющиеся, резервного копирования копию среды аналитика временных рядов Azure является обязательным:

1. Как конкретный аналитики временных рядов **экземпляра отработки отказа** для перенаправления данных и трафик.
1. В целях аудита и данных хранения.

Как правило лучшим способом для дублирования среды Time Series Insights является создание второй среде Time Series Insights в резервной копии регион Azure. События также отправляются в этой вторичной среды из вашего источника событий первичной. Убедитесь, что для использования группы второе, выделенной потребителей и рекомендациям этого источника бизнеса аварийного восстановления (приведенный выше).

В частности для создания повторяющихся среды:

1. Создание среды во втором регионе ([создания новой среды Time Series Insights на портале Azure](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started)).
1. Создайте для вашего источника событий вторую выделенную группу потребителей.
1. Источник событий подключения к новой среде, убедитесь, что группа потребителей, во-вторых, выделенный.
1. Просмотрите Time Series Insights [центра Интернета вещей](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) и [концентратора событий](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) документации.

Последний этап:

* Если во время инцидента ваш основной регион дал сбой, переключите операции на резервную среду службы "Аналитика временных рядов".
* Используйте ваши во второй регион для резервного копирования и восстановления любых данных телеметрии и запроса аналитики временных рядов.

> [!IMPORTANT]
> * Обратите внимание, что задержка может возникнуть в случае сбоя.
> * Отработка отказа также может привести к небольшой пик в процессе обработки сообщения, так как операции, пересылаются.
> * Дополнительные сведения см. в статье [Отслеживание и уменьшение регулирования для сокращения задержек в службе "Аналитика временных рядов Azure"](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).