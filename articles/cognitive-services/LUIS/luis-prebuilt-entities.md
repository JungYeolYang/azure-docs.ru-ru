---
title: Предварительно созданные сущности
titleSuffix: Azure Cognitive Services
description: Интеллектуальная служба распознавания речи (LUIS) включает набор предварительно созданных сущностей для распознавания общих типов информации, таких как даты, время, цифры, измерения и валюта. Поддержка предварительно созданной сущности зависит от языка и региональных параметров приложения LUIS.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 0cfc4ff58cfeb65f80f9ac5ce2dd532defde5ef8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60596120"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Предварительно созданные сущности для распознавания распространенных типов данных

Интеллектуальная служба распознавания речи (LUIS) включает набор предварительно созданных сущностей для распознавания общих типов информации, таких как даты, время, цифры, измерения и валюта. 

## <a name="add-a-prebuilt-entity"></a>Добавить предварительно созданную сущность

1. Откройте приложение, щелкнув его имя на странице **Мои приложения**, а затем щелкните **Сущности** в левом углу. 

1. На странице **Сущности** щелкните **Add prebuilt entity** (Добавить предварительно созданную сущность).

1. В диалоговом окне **Добавление предварительно созданных сущностей** выберите предварительно созданную сущность datetimeV2. 

    ![Диалоговое окно добавления предварительно созданной сущности](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. Нажмите кнопку **Готово**.

## <a name="publish-the-app"></a>Публикация приложения

Самый простой способ просмотреть значение предварительно созданной сущности — запросить его из опубликованной конечной точки. 

1. В верхней части панели инструментов щелкните **Опубликовать**. Выполните публикацию в **рабочей среде**. 

1. При появлении уведомления об успешной публикации щелкните ссылку **Перейти к списку конечных точек**, чтобы просмотреть конечные точки.

1. Выберите конечную точку. Откроется новая вкладка браузера для этой конечной точки. Не закрывайте вкладку браузера и перейдите в раздел **Тест**.

## <a name="test"></a>Тест
После добавления сущности не требуется проводить обучение приложения. 

Выполните тест нового намерения в конечной точке, добавив значение для параметра **q**. Используйте следующую таблицу предлагаемых фраз для **q**:

|Тестовая фраза|Значение сущности|
|--|:--|
|Забронировать билет на самолет завтра|2018-10-19|
|отменить встречу 3 марта|Служба LUIS вернет последнюю запись 3 марта в прошлом (2018-03-03) и 3 марта в будущем (2019-03-03), так как во фразе не указан год.|
|Запланировать встречу на 10 утра|10:00:00|

## <a name="marking-entities-containing-a-prebuilt-entity-token"></a>Маркировка сущностей, содержащих предварительно созданный токен объекта
 Если у вас есть текст, такой как `HH-1234`, который нужно маркировать как настраиваемую сущность, _и_ к вашей модели добавлена [предварительно созданная сущность number](luis-reference-prebuilt-number.md), то вы не сможете маркировать эту настраиваемую сущность на портале LUIS. Вы можете маркировать ее с помощью API. 

 Чтобы маркировать такой тип токена, в котором часть уже маркирована настраиваемой сущностью, удалите эту сущность из приложения LUIS. Обучение приложения не требуется. Пометьте токен с помощью вашей собственной настраиваемой сущности. Затем снова добавьте эту сущность в приложение LUIS.

 В качестве еще одного примера рассмотрим это высказывание как список предпочтений. `I want first year spanish, second year calculus, and fourth year english lit.` Если в приложение LUIS добавлена предварительно созданная сущность ordinal, `first`, `second` и `fourth` уже будут маркированы с помощью порядковых номеров. Если необходимо соединить маркер порядкового номера и класса, то можно создать составной объект и поместить в него сущность ordinal и настраиваемую сущность для имени класса.

## <a name="next-steps"></a>Дальнейшие действия
> [!div class="nextstepaction"]
> [Entities per culture](./luis-reference-prebuilt-entities.md) (Язык и региональные параметры сущностей).
