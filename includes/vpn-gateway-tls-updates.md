---
title: включение файла
description: включение файла
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e4d20cd39d2a843ee1ab57a412ac668b3495fdb1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60320227"
---
>[!NOTE]
>Начиная с 1 июля 2018 года прекращается поддержка TLS 1.0 и TLS 1.1 в VPN-шлюзе Azure. VPN-шлюз будет поддерживать только TLS 1.2. Дополнительные сведения см. в разделе [об обновлении для поддержки TLS 1.2](#tls1).

Кроме того, с 1 июля 2018 года объявляются нерекомендуемыми для TLS приведенные ниже устаревшие алгоритмы:

* RC4 (Rivest Cipher 4);
* DES (Data Encryption Algorithm);
* 3DES (Triple Data Encryption Algorithm);
* MD5 (Message Digest 5);

### <a name="tls1"></a>Как включить поддержку TLS 1.2 в Windows 7 и Windows 8.1?

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]
