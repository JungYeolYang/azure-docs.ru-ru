---
title: Узел ссылки сопоставления Потока данных Фабрики данных Azure
description: Поток данных Фабрики данных добавит узел ссылки для соединений, подстановок и объединений
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 626943143e8fa193f143e66d856d9b00e3589fb5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61262700"
---
# <a name="mapping-data-flow-reference-node"></a>Узел ссылки сопоставления Потока данных

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Узел ссылки](media/data-flow/referencenode.png "узел ссылки")

Узел ссылки автоматически добавляется на холст, чтобы отметить, что узел, к которому он присоединен, ссылается на другой узел, существующий на холсте. В этом смысле узел ссылки можно рассматривать как указатель или ссылку на другое преобразование потока данных.

Пример. При присоединении или объединении более одного потока данных холст "Поток данных" может добавить узел ссылки, который отражает имя и параметры вторичного входящего потока.

Узел ссылки невозможно переместить или удалить. Тем не менее вы можете щелкнуть в узле, чтобы изменить исходные параметры преобразования.

Правила пользовательского интерфейса, которые определяют, когда Поток данных добавляет узел ссылки. Они основаны на доступном и вертикальном пространстве между строками.
