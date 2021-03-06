---
title: Подсчет символов — API перевода текстов
titlesuffix: Azure Cognitive Services
description: Как API перевода текстов считает символы.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 02/01/2019
ms.date: 03/12/2019
ms.author: v-junlch
ms.openlocfilehash: c88eb56288d3a7cf46ce84430a53c12a4ee31c7a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60513762"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Как API перевода текстов считает символы

API перевода текстов считает как знаки каждую кодовую точку Юникода во входном тексте. Каждый перевод текста на язык считается отдельным переводом, даже если запрос был сделан в одном вызове API для перевода на несколько языков. Длина ответа не имеет значения.

Что учитывается при подсчете:

* текст, передаваемый в API перевода текстов в тексте запроса;
   * `Text`, если используются методы Translate, Transliterate и Dictionary Lookup;
   * `Text` и `Translation`, если используется метод Dictionary Examples.
* вся разметка: HTML, теги XML и т. д. в текстовом поле запроса; нотация JSON, используемая для создания запроса (например, "Text:") не учитывается;
* отдельная буква.
* Пунктуация
* Пробел, табуляция, разметка и любой пробел.
* Каждая кодовая точка, определенная в Юникоде.
* Повторяющийся перевод, даже если один и тот же текст был переведен ранее.

Для сценариев с идеограммами, например китайского алфавита и японского алфавита кандзи, API перевода текстов будет по-прежнему считать количество кодовых точек Юникода, по одному знаку на идеограмму. Исключение: Суррогатные символы Юникода подсчитываются как два символа.

Количество запросов, слов, байтов или предложений не учитывается при подсчете количества знаков. 

Вызовы методов Detect и BreakSentence не учитываются при подсчете использованных символов. Тем не менее предполагается, что вызовы методов Detect и BreakSentence логически соотносятся с использованием других функций, которые учитываются. Если число отправленных вызовов Detect или BreakSentence превышает число других сосчитанных методов в 100 раз, корпорация Майкрософт оставляет за собой право ограничить возможность использования методов Detect и BreakSentence.


Дополнительные сведения о подсчете символов см. в разделе [часто задаваемых вопросов о Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).

