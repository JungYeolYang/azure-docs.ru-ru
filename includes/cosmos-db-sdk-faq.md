---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 11/09/2018
ms.author: sngun
ms.openlocfilehash: 99dddd86c9348c9791d3012b382298bb020e63c9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60326869"
---
**1. Каким образом клиенты будут уведомлены о выводе пакета SDK из эксплуатации?**

Корпорация Майкрософт за 12 месяцев отправит предварительное уведомление об окончании поддержки пакета SDK, чтобы обеспечить более плавный переход на поддерживаемый пакет SDK. Кроме того, клиенты будут проинформированы с помощью различных каналов связи — портал управления Azure, центр разработчиков, записи блога и отправка прямых сообщений назначенным администраторам служб.

**2. Могут ли клиенты создавать приложения, используя выводимый из эксплуатации пакет SDK для Azure Cosmos DB в течение 12-месячного периода?** 

Да, в течение 12-месячного периода пользователи будут иметь полный доступ для разработки, развертывания и изменения приложений с помощью выводимого из эксплуатации пакета SDK для Azure Cosmos DB. В рамках 12-месячного льготного периода клиентам рекомендуется соответствующим образом перейти на новую поддерживаемую версию пакета SDK для Azure Cosmos DB.

**3. Могут ли клиенты создавать и изменять приложения с помощью выведенного из эксплуатации пакета SDK для Azure Cosmos DB по истечении 12-месячного периода уведомления?**

По истечении 12-месячного периода уведомления поддержка пакета SDK будет прекращена. Доступ приложений, которые используют выведенный из эксплуатации пакет SDK, к Azure Cosmos DB будет запрещен платформой Cosmos DB. Кроме того, Майкрософт не будет предоставлять клиентам поддержку выведенного пакета SDK.

**4. Что произойдет с работающими приложениями клиента, которые используют неподдерживаемую версию пакета SDK для Azure Cosmos DB?**

Любые попытки подключения к службе Azure Cosmos DB с помощью выведенной из эксплуатации версии пакета SDK будут отклонены. 

**5. Новые функции и возможности будут применены ко всем оставшимся пакетам SDK?**

Новые функции и возможности будут добавлены только в новые версии. Если вы используете старую, но не выведенную из эксплуатации версию пакета SDK, запросы в Azure Cosmos DB будут выполняться без изменений, но вы не сможете получить доступ к новым функциям.  

**6. Что делать, если не удается обновить приложение до даты вывода из эксплуатации?**

Рекомендуется как можно раньше выполнить обновление до последней версии SDK. После отметки пакета SDK для вывода из эксплуатации у вас будет 12 месяцев на обновление приложения. Если по какой-либо причине вы не сможете обновить приложение в течение этого времени, заранее обратитесь за помощью к [команде разработчиков Cosmos DB](mailto:askcosmosdb@microsoft.com).

