---
title: Как подготовить данные для пользовательского голосовой связи — службы распознавания речи
titlesuffix: Azure Cognitive Services
description: Создание настраиваемых голосовых для вашего бренда с помощью служб Azure речи. Вы укажете studio записи и связанные сценарии, служба создает модель уникальный голос, настроены так, чтобы записи голоса. Используйте этот голос для синтезирования речи в продуктов, средств и приложений.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: 18e1bb486c47baf7648a74e31451e2db73f72250
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65156863"
---
# <a name="prepare-data-to-create-a-custom-voice"></a>Подготовка данных для создания настраиваемых голосовых

Когда вы будете готовы к созданию пользовательского речь для вашего приложения, первым шагом является сбор аудиозаписи и связанные сценарии, чтобы начать обучение голосовую модель. Служба использует эти данные, чтобы создать уникальный голос, настроена в соответствии с голос в записи. После обучения голос, можно запустить, создав речи в приложениях.

Можно начать с небольшой объем данных для создания подтверждения концепции. Тем не менее чем больше данных, указываемое вами, более естественным, настраиваемые голоса будут звучать. Прежде чем вы можете обучить модель речь, вам потребуется, аудиозаписи и транскрипций связанный текст. На этой странице мы рассмотрим типы данных, способы их использования и управления каждой.

## <a name="data-types"></a>Типы данных

Голосовые обучающий набор данных включает в себя аудиозаписи и текстовый файл с помощью связанных транскрипций. Каждый звуковой файл должен содержать один utterance (предложение или одной включить для диалогового окна системы) и быть не более 15 секунд.

В некоторых случаях может не иметь правого набора данных готов и будет нужно протестировать обучения настраиваемых голосовых доступных звуковые файлы, короткого или длинного, независимо от записи. Мы предоставляем средства (бета-версия), позволяющие разделить звукового фразы, а также подготовки с помощью табель [расшифровка дикторского текста для API пакетной службы](batch-transcription.md).

В этой таблице перечислены типы данных и как каждый используется для создания модели пользовательского речь.

| Тип данных | ОПИСАНИЕ | Сценарии использования | Требуется дополнительная служба | Количество для обучения модели | Языковых стандартов |
| --------- | ----------- | ----------- | --------------------------- | ----------------------------- | --------- |
| **Отдельные фразы + сопоставления расшифровку дикторского текста** | Коллекция (ZIP-файл) звуковые файлы (.wav) как отдельные фразы. Каждый звуковой файл должен быть 15 секунд или длиной, сопровождается форматированный расшифровку дикторского текста (.txt). | Профессиональные записей с совпадающими табель | Все готово для обучения. | Не строгим требованием для en US "и" zh-CN. Более чем 2 000 + distinct фразы для других языков. | Все голосовые пользовательские языковые стандарты |
| **Long аудиозапись и расшифровку дикторского текста (бета-версия)** | Коллекция (ZIP-файл), long, несегментированную звуковых файлов (более 20 секунд), связан с запись разговора (.txt), содержащий все произнесенного слова. | У вас есть звуковые файлы и тексты выступлений сопоставления, но не разбиваются на фразы. | Сегментации (с помощью расшифровка дикторского текста для пакетной службы).<br>Звуковой формат преобразования там, где необходимо. | Не строгим требованием для en US "и" zh-CN. | `en-US` и `zh-CN` |
| **Только звук (бета-версия)** | Коллекция звуковые файлы без запись разговора (ZIP-файл). | Только у вас есть звуковые файлы, доступные без табель. | Сегментация + поколение расшифровку дикторского текста (с помощью расшифровка дикторского текста для пакетной службы).<br>Звуковой формат преобразования там, где необходимо.| Не строгим требованием для `en-US` и `zh-CN`. | `en-US` и `zh-CN` |

Файлы следует сгруппированных по типу в набор данных и загрузить как ZIP-файл. Каждый набор данных может содержать только один тип данных.

> [!NOTE]
> Максимальное число наборов данных, которые разрешено импортировать на одну подписку — 10 ZIP-файл файлы бесплатно пользователи подписки (F0) и 500 для стандартной подписки (S0) пользователей.

## <a name="individual-utterances--matching-transcript"></a>Отдельные фразы + сопоставления расшифровку дикторского текста

Можно подготовить записи отдельные фразы и соответствующие записи двумя способами. Либо написать сценарий, который будет прочитан талант голосовой или использовать общедоступные аудио и переписывать их в текст. В последнем случае отредактируйте в аудиофайлах слова-паразиты, такие как "эм", заикание, нечетко или неправильно произнесенные слова.

Чтобы получить хороший голоса, создайте записи в тихом помещении с высококачественный микрофон. Скорость, говоря, прецессии и собственного выразительный mannerisms речи озвучивания важны согласованные тома.

> [!TIP]
> Чтобы создать голос для использования в рабочей среде, рекомендуем использовать профессиональную студию звукозаписи и актера озвучивания. Дополнительные сведения см. в разделе [Как записывать образцы голоса для создания пользовательских голосовых моделей](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/record-custom-voice-samples).

### <a name="audio-files"></a>Аудиофайлы

Каждый звуковой файл должен содержать один utterance, (предложение или одной включить диалоговое окно системы), не более 15 секунд. Все файлы должны находиться в том же устной. Пользовательские преобразования текста в речь голоса Многоязычная версия не поддерживаются, за исключением китайский английский bi языков. Каждый звуковой файл должен иметь уникальное числовое имя файла с расширением имени файла WAV.

Соблюдайте эти рекомендации при подготовке аудио.

| Свойство | Value |
| -------- | ----- |
| Формат файла | RIFF (.wav), сгруппированных в ZIP-файл |
| Частота выборки | по крайней мере 16 000 Гц |
| Формат выборки | PCM, 16-разрядные |
| Имя файла | Числовые, с расширением .wav. Не разрешенные имена файлов-дубликатов. |
| Длина аудио | Короче, чем 15 секунд |
| Формат архива | .zip |
| Максимальный размер архива | 200 МБ |

> [!NOTE]
> WAV-файлов с дискретизации ниже, чем 16 000 Гц, будут отклонены. Если ZIP-файл содержит WAV-файлов с различной частотой выборки, будут импортированы только те равно или выше, чем 16 000 Гц. Портал в настоящее время импортирует ZIP-архивы размером до 200 МБ. Однако можно отправить несколько архивов.

### <a name="transcripts"></a>Расшифровка

Расшифровка дикторского текста для файла — это обычный текстовый файл. Используйте эти инструкции для подготовки вашей транскрипций.

| Свойство | Value |
| -------- | ----- |
| Формат файла | Обычный текст (TXT) |
| Формат кодировки | ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE или UTF-16-BE. Zh-CN кодировки ANSI/ASCII и UTF-8 не поддерживаются. |
| Количество фраз в строке | **Один** -каждой строки файла расшифровка дикторского текста должен содержать имя одного из звуковые файлы, следуют соответствующие транскрипции. Для разделения имени файла и транскрипции необходимо использовать символ табуляции (\t). |
| Максимальный размер файла | 50 MB |

Ниже приведен пример записи организация utterance по utterance в одном файле .txt:

```
0000000001[tab] This is the waistline, and it's falling.
0000000002[tab] We have trouble scoring.
0000000003[tab] It was Janet Maslin.
```
Очень важно, что записи являются 100% точные транскрипций соответствующий звука. Ошибки в записи представит потери качества в процессе обучения.

> [!TIP]
> При создании преобразования текста в речь голоса, выберите фразы (или написать сценарии), которые принимают в рабочей учетной записи фонетического покрытия и эффективность. Возникли проблемы при получении требуемых результатов? [Обратитесь в службу настраиваемый голос](mailto:speechsupport@microsoft.com) team для выяснения того, см. Дополнительные сведения о необходимости нам.

## <a name="long-audio--transcript-beta"></a>Long аудиозапись и расшифровку дикторского текста (бета-версия)

В некоторых случаях вам может иметь сегментируются аудио доступна. Мы предоставляем службы (бета-версия) на портале настраиваемых голосовых для сегментирования долго звуковые файлы и создать транскрипций. Сохранить в виду, эта служба будет взиматься плата по направлению к использование вашей подписки речи в текст.

> [!NOTE]
> Службы сегментации аудио долго будет использовать функцию пакетной службы транскрибирование речи в текст, который поддерживает только подписка standard (S0) пользователей. Во время обработки разбить, звуковых файлов и записи будут также отправляться к службе Custom речи для уточнения модели распознавания, поэтому можно повысить точность данных. Данные не будут храниться во время этого процесса. После завершения сегментацию за загрузку и обучения будут храниться только сегментированных фразы и тексты выступлений их сопоставление.

### <a name="audio-files"></a>Аудиофайлы

Соблюдайте эти рекомендации при подготовке аудио для сегментации.

| Свойство | Value |
| -------- | ----- |
| Формат файла | RIFF (.wav) с частотой выборки минимум 16 кГц-16-разрядное в PCM или MP3 со скоростью по крайней мере 256 Кбит/с, сгруппированных в ZIP-файл |
| Имя файла | Только символы ASCII. Символы Юникода в имени завершится ошибкой (например, иероглифы, то есть такие символы, как «—»). Повторяющиеся имена не допускается. |
| Длина аудио | Больше, чем 20 секунд |
| Формат архива | .zip |
| Максимальный размер архива | 200 МБ |

Все звуковые файлы должны быть сгруппированы в ZIP-файл. Это нормально для помещения файлов .wav и .mp3 файлов в один zip аудио, но может не вложенную папку в ZIP-файле. Например можно отправить файл zip, содержащий звуковой файл с именем «kingstory.wav», 45 длительностью в секунду и другой один именованный «queenstory.mp3», 200-секунду long, без всех вложенных папках. После обработки преобразуются все MP3-файлы в формате WAV.

### <a name="transcripts"></a>Расшифровка

Записи должны быть готовы спецификации, приведенные в этой таблице. Записи должны быть соответствующие каждый звуковой файл.

| Свойство | Value |
| -------- | ----- |
| Формат файла | Обычный текст (txt), сгруппированных в ZIP-файл |
| Имя файла | Используйте имя, совпадающее с именем сопоставления звуковой файл |
| Формат кодировки | UTF-8-BOM только |
| Количество фраз в строке | Без ограничений |
| Максимальный размер файла | 50 МЛН |

Все файлы табель в этот тип данных должны быть сгруппированы в ZIP-файл. В ZIP-файл может быть не вложенную папку. Например загруженный ZIP-файл, содержащий звуковой файл с именем «kingstory.wav», 45 секунд, и другой один именованный «queenstory.mp3», 200 секунды долго. Необходимо будет передать другой файл zip, содержащий две записи, один именованный «kingstory.txt», другие один «queenstory.txt». В пределах каждого обычный текстовый файл будет указано полное правильный расшифровка дикторского текста для сопоставления аудио.

После успешной передачи набора данных мы поможем вам разделить фразы, основанные на записи предоставляются звуковой файл. Сегментированный фразы и соответствующие записи можно проверить, загрузив набор данных. Уникальные идентификаторы будут назначаться сегментированных фразы автоматически. Очень важно убедиться в том, что абсолютной вами записи. Ошибки в записи можно снизить точность во время аудио сегментации и дальнейшего ознакомления потери качества на этапе обучения, входящий в более поздней версии.

## <a name="audio-only-beta"></a>Только звук (бета-версия)

Если у вас транскрипций для вашей аудиозаписи, используйте **только звук** параметр, чтобы отправить данные. Наша система поможет вам сегментировать и транскрипция звуковых файлов. Имейте в виду, эта служба будет учитываются использование вашей подписки речи в текст.

Соблюдайте эти рекомендации при подготовке аудио.

> [!NOTE]
> Службы сегментации аудио долго будет использовать функцию пакетной службы транскрибирование речи в текст, который поддерживает только подписка standard (S0) пользователей.

| Свойство | Value |
| -------- | ----- |
| Формат файла | RIFF (.wav) с частотой выборки минимум 16 кГц-16-разрядное в PCM или MP3 со скоростью по крайней мере 256 Кбит/с, сгруппированных в ZIP-файл |
| Имя файла | Только символы ASCII. Символы Юникода в имени завершится ошибкой (например, иероглифы, то есть такие символы, как «—»). Повторяющееся имя не допускается. |
| Длина аудио | Больше, чем 20 секунд |
| Формат архива | .zip |
| Максимальный размер архива | 200 МБ |

Все звуковые файлы должны быть сгруппированы в ZIP-файл. В ZIP-файл может быть не вложенную папку. После успешной передачи набора данных мы поможем вам разделить звуковой файл высказываний на основе нашей службы расшифровка дикторского текста для пакетной службы распознавания речи. Уникальные идентификаторы будут назначаться сегментированных фразы автоматически. Сопоставление записей создается через распознавания речи. После обработки преобразуются все MP3-файлы в формате WAV. Сегментированный фразы и соответствующие записи можно проверить, загрузив набор данных.

## <a name="next-steps"></a>Дальнейшие действия

- [Создание настраиваемых голосовых](how-to-custom-voice-create-voice.md)
- [Руководство по: Запишите ваших примеров голоса](record-custom-voice-samples.md)
