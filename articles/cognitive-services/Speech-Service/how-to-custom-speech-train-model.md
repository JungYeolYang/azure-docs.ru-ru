---
title: Обучение модели речи Custom - службы распознавания речи
titlesuffix: Azure Cognitive Services
description: Обучение речи в текст необходим для повышения точности распознавания для базовой модели корпорации Майкрософт или пользовательской модели, который вы планируете создать. Модель обучена с использованием с меткой human транскрипций, а также связанный текст. Эти наборы данных вместе с ранее отправленную звуковых данных, используются для уточнения и обучения модели речи в текст для распознавания слов, фраз, акронимов, имена и другие условия конкретного продукта.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 194ae477bb3cba4ac7e3350da6b793c6fea6ecdb
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025861"
---
# <a name="train-a-model-for-custom-speech"></a>Обучение модели для пользовательских речи

Обучение речи в текст необходим для повышения точности распознавания для базовой модели корпорации Майкрософт или пользовательской модели, который вы планируете создать. Модель обучена с использованием с меткой human транскрипций, а также связанный текст. Эти наборы данных вместе с ранее отправленную звуковых данных, используются для уточнения и обучения модели речи в текст для распознавания слов, фраз, акронимов, имена и другие условия конкретного продукта. Более предметно-наборы данных, вы предоставите (данные, связанные с пользователями, которые будет иметь и что нужно для распознавания), более точные модели будет, что приводит к улучшенное распознавание. Имейте в виду, что, передав несвязанных данных в обучение, можно сократить или снизить точность модели.

## <a name="use-training-to-resolve-accuracy-issues"></a>Использовать для устранения проблем точность обучения

Если возникать распознавания проблемы с использованием модели с помощью с меткой human детализированных записей и связанные данные дополнительное обучение может помочь повысить точность. Используйте эту таблицу, чтобы определить, какой набор данных используется для решения вашей проблемы:

| Вариант использования  | Тип данных | Количество данных |
|----------|-----------|---------------|
| Правильные имена являются неверно распознал слово | Связать текст (предложения или фразы) | 10 МБ до 500 МБ |
| Слова неверно распознал слово из-за акцента | Связанный текст (фонетическая) | Укажите misrecognized слова |
| Распространенные слова удален или неверно распознал слово | Табель аудио + с меткой отдела | Расшифровка дикторского текста для 10 до 1 000 часов |

> [!IMPORTANT]
> Если вы еще не отправили набор данных, см. раздел [Подготовка и проверка данных](how-to-custom-speech-test-data.md). Этот документ содержит инструкции по загрузке данных, а также рекомендации по созданию наборов данных высокого качества.

## <a name="train-and-evaluate-a-model"></a>Обучение и анализ модели

Первым шагом для обучения модели заключается в отправке обучающих данных. Используйте [Подготовка и проверка данных,](how-to-custom-speech-test-data.md) пошаговые инструкции для подготовки с меткой human транскрипций и соответствующем текстовом (фразы и произношение). После отправки данных для обучения, выполните следующие действия, чтобы начать, обучения модели:

1. Перейдите к **речи в текст > настраиваемое преобразование речи > обучения**.
2. Нажмите кнопку **Обучение модели**.
3. Затем предоставьте обучения **имя** и **описание**.
4. Из **сценарий и базовой модели** раскрывающееся меню, выберите сценарий, который оптимально подходит для вашего домена. Если вы знаете, какой сценарий, чтобы выбрать, выберите **Общие**. Базовой модели является начальной точкой для обучения. Если нет иных предпочтений, можно использовать последнюю версию.
5. Из **выберите обучающие данные** выберите один или несколько наборов данных транскрибирования аудио + с меткой отдела, вы бы хотели использовать для обучения.
6. После завершения обучения вы можете выполнить точности, тестирование на обученной модели. Этот шаг не является обязательным.
7. Выберите **создать** создавать пользовательские модели.

В таблице обучающих отображает новую запись, которая соответствует только что созданной модели. Она также отображает состояние:  Обработки, выполнена успешно, сбой.

## <a name="evaluate-the-accuracy-of-a-trained-model"></a>Оценить точность обученной модели

Можно проверить данные и оценить точность модели, с помощью этих документов:

* [Проверить свои данные](how-to-custom-speech-inspect-data.md)
* [Оценка данных](how-to-custom-speech-evaluate-data.md)


Если вы выбрали позволяет проверить точность, важно выбрать акустических набора данных, которая отличается от той, которая используется с моделью реалистичные понять смысл эффективности модели.

## <a name="next-steps"></a>Дальнейшие действия

* [Развертывание модели](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Подготовка и тестирование данных](how-to-custom-speech-test-data.md)
* [Проверить свои данные](how-to-custom-speech-inspect-data.md)
* [Оценка данных](how-to-custom-speech-evaluate-data.md)
* [Обучение модели](how-to-custom-speech-train-model.md)
