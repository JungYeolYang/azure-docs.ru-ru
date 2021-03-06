---
title: Проверка качества данных речи Custom - службы распознавания речи
titlesuffix: Azure Cognitive Services
description: Настраиваемое преобразование речи предоставляет средства, которые дают возможность визуально оценить качество распознавания модели путем сравнения звуковых данных с соответствующий результат распознавания. На портале Custom речи можно воспроизвести отправленного аудио и определить, является ли правильный результат предоставленного распознавания.  Это средство позволяет быстро проверять качество модели речи в текст базовых показателей корпорации Майкрософт или пользовательских обученной модели без необходимости переписывать никаких аудиоданных.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 682f3df362a7fbb0e95a07aa8a8f3a068367eef2
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025822"
---
# <a name="inspect-custom-speech-data"></a>Проверьте данные Custom речи

> [!NOTE]
> Здесь предполагается, что вы прочитали [подготовки тестовых данных для пользовательских речи](how-to-custom-speech-test-data.md) и отправили набор данных для проверки.

Настраиваемое преобразование речи предоставляет средства, которые дают возможность визуально оценить качество распознавания модели путем сравнения звуковых данных с соответствующий результат распознавания. На портале Custom речи можно воспроизвести отправленного аудио и определить, является ли правильный результат предоставленного распознавания. Это средство позволяет быстро проверять качество модели речи в текст базовых показателей корпорации Майкрософт или пользовательских обученной модели без необходимости переписывать никаких аудиоданных.

В этом документе вы узнаете, как проверить качество модели с помощью обучающих данных, загруженный ранее.

На этой странице вы узнаете, как проверить качество модели речи в текст базовых показателей корпорации Майкрософт и/или пользовательскую модель, обученную. Вы используете данных, передаваемых в **данных** вкладку для тестирования.

## <a name="create-a-test"></a>Создание теста

Выполните эти инструкции для создания теста.

1. Перейдите к **речи в текст > настраиваемое преобразование речи > тестирование**.
2. Нажмите кнопку **добавьте тест**.
3. Выберите **проверки качества (только для звука данных)**. Присвойте имя, описание, теста и выберите свой звуковой набор данных.
4. Выберите до двух моделей, которые вы хотите проверить.
5. Нажмите кнопку **Создать**.

После успешного создания теста можно сравнить модели рядом друг с другом.

## <a name="side-by-side-model-comparisons"></a>Сравнение модели Side-by-side

Если находится в состоянии теста *Succeeded*, щелкните имя элемента теста, чтобы просмотреть сведения о тесте. На этой странице подробно перечислены все фразы в наборе данных, показывающее результат распознавания этих двух моделях, наряду с транскрипции из отправленного набора данных.

Чтобы проверить сравнения side-by-side, можно переключать различные типы ошибок, включая вставки, удаления и замены. Прослушивание аудио и сравнение результатов распознавания, в каждом столбце (отображается с меткой human расшифровка дикторского текста и результаты двух моделей речи в текст), можно решить, какую модель соответствует вашим потребностям, а где требуются улучшения.

Проверка качества тестирования полезно проверить качество конечную точку распознавания речи вполне достаточно для работы приложения.  Для объективное измерение точности, требуя расшифрованного аудио следуйте инструкциям, приведенным в тестирования: Оцените точность.

## <a name="next-steps"></a>Дальнейшие действия

* [Оценка данных](how-to-custom-speech-evaluate-data.md)
* [Обучение модели](how-to-custom-speech-train-model.md)
* [Развертывание модели](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Подготовка тестовых данных для пользовательских речи](how-to-custom-speech-test-data.md)
