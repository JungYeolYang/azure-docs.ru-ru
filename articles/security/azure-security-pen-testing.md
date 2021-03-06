---
title: Тестирование на проникновение | Документация Майкрософт
description: В этой статье представлен обзор тестирования на проникновение и способах его проведения для ваших приложений, которые работают в инфраструктуре Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/13/2018
ms.author: barclayn
ms.openlocfilehash: 8835c4534b6dab1e8dbfb3546257ae4bc3b9d7af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610928"
---
# <a name="penetration-testing"></a>Тест на защиту от несанкционированного доступа
Одно из преимуществ использования Azure для развертывания и тестирования приложений — возможность быстро создавать среды. Вам не нужно беспокоиться о подаче заявок, приобретении и настройке собственного локального оборудования.

Это прекрасно, но вам по-прежнему нужно самостоятельно проводить стандартную экспертизу безопасности. Одна из вещей, скорее всего, вы хотите сделать это на проникновение тестирования приложений, развертываемых в Azure.

Возможно, вы уже знаете, что корпорация Майкрософт выполняет [тестирование среды Azure на проникновение](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Это помогает вносить в Azure усовершенствования.

Мы не тест безопасности вашего приложения за вас, но мы понимаем, что должны и будут должны провести тестирование на проникновение. Это очень важно, поскольку если повысить безопасность приложений, то помочь повысить безопасность всей экосистемы Azure.

С 15 июня 2017 г. Microsoft больше не требует предварительного утверждения, чтобы выполнить тестирование на проникновение для ресурсов Azure. Пользователи, желающие получить официальное документальное подтверждение предстоящего выполнения тестов на проникновение от Microsoft Azure, могут заполнить [форму уведомления о тестировании на проникновение для служб Azure](https://portal.msrc.microsoft.com/en-us/engage/pentest). Этот процесс касается только Microsoft Azure и не применяется к остальным облачным службам Майкрософт.

>[!IMPORTANT]
>Хотя уведомлять Майкрософт о тестировании на проникновение больше не требуется, пользователи по-прежнему должны соответствовать требованиям [общих правил выполнения тестов на проникновение в Microsoft Cloud](https://technet.microsoft.com/mt784683).

Стандартные тесты, которые вы можете проводить:

* тестирование конечных точек для выявления [основных 10 уязвимостей в рамках проекта Open Web Application Security Project (открытый проект обеспечения безопасности веб-приложений, OWASP)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [нечеткое тестирование](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) конечных точек;
* [сканирования портов](https://en.wikipedia.org/wiki/Port_scanner) конечных точек;

К тестам, которые запрещено выполнять, относятся атаки типа ["отказ в обслуживании" (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) . В эту категорию включены как запуск DoS-атаки, так и все сопутствующие тесты, которые могут определять, демонстрировать или имитировать DoS-атаку любого типа.

## <a name="next-steps"></a>Дальнейшие действия

- Если вы хотите, чтобы формально задокументировать предстоящих проникновение от приложений, размещенных в Microsoft Azure, перейдите на [Penetration Testing правила использования](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2) и заполните форму тестирования уведомлений.
