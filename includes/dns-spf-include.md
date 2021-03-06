---
author: WenJason
ms.service: dns
ms.topic: include
origin.date: 11/25/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.openlocfilehash: 9cc650cea17acb8d89933c819c4ca60e2c459d1c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60563370"
---
Записи инфраструктуры политики отправителей (SPF) используются для указания почтовых серверов, которые могут отправлять электронную почту от имени доменного имени. Правильная конфигурация записей SPF очень важна, чтобы получатели не помечали ваши сообщения электронной почты как "Нежелательная почта".

В RFC DNS новый тип записи SPF был изначально представлен для поддержки этого сценария. Для поддержки старых серверов доменных имен в них также может использоваться тип записи TXT, чтобы указывать записи SPF. Эта неоднозначность привела к путанице, которую удалось устранить с помощью документа [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1). В нем сказано, что записи SPF должны создаваться с помощью записей типа TXT. Также там сказано, что записи типа SPF являются устаревшими.

**Записи SPF поддерживаются в Azure DNS, и их необходимо создавать с помощью записи типа TXT.** Устаревший тип записи SPF не поддерживается. Во время [импорта файла зоны DNS](../articles/dns/dns-import-export.md) все записи SPF, которые используют тип SPF, преобразуются в записи типа TXT.
