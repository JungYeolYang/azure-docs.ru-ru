---
title: Тестирование базы знаний — QnA Maker
titlesuffix: Azure Cognitive Services
description: Тестирование базы знаний QnA Maker является важной частью итерационного процесса для повышения точности возвращаемых ответов. Вы можете протестировать базу знаний через расширенный интерфейс чата, который также позволяет вносить правки.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/08/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 2c596b49d5587b07fe75cefde72e897478dc3dc8
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472087"
---
# <a name="test-your-knowledge-base-interactively-in-qna-maker"></a>Интерактивное тестирование базы знаний в QnA Maker

Тестирование базы знаний QnA Maker является важной частью итерационного процесса для повышения точности возвращаемых ответов. Вы можете протестировать базу знаний через расширенный интерфейс чата, который также позволяет вносить правки.

## <a name="test-answer-matching"></a>Соответствие тестового ответа

1. Получите доступ к базе знаний, выбрав ее имя на странице **Мои базы знаний**.
1. Чтобы открыть выдвигающуюся панель тестирования, щелкните **Тестировать** на верхней панели приложения.
1. Введите запрос в текстовом поле и нажмите клавишу ВВОД.
1. Ответ наилучшего соответствия из базы знаний возвращается как отклик.

## <a name="clear-test-panel"></a>Очистка панели тестирования

Чтобы очистить все введенные тестовые запросы и их результаты из консоли тестирования, выберите **Начать сначала** в левом верхнем углу панели тестирования.

## <a name="close-test-panel"></a>Закрытие панели тестирования

Чтобы закрыть панель тестирования, еще раз нажмите кнопку **Тестировать**. При открытой панели тестирования нельзя изменить содержимое базы знаний.

## <a name="inspect-score"></a>Проверка оценки

Проверка сведений в результате тестирования выполняется на панели проверки.

1.  На открытой выдвигающейся панели тестирования нажмите кнопку **Проверка** для получения более подробной информации об этом ответе.

    ![Проверка ответов](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Появится панель проверки. На панели находится намерение с высокой оценкой, а также любые идентифицированные сущности. На панели отображается результат выбранного высказывания.

## <a name="correct-the-top-scoring-answer"></a>Исправьте ответы с высокой оценкой

Если ответы с высокой оценкой неверны, выберите правильный ответ из списка и нажмите **Сохранить и Обучать**.

![Исправьте ответы с высокой оценкой](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Добавление альтернативных вопросов

Можно добавить альтернативные формы вопроса для данного ответа. Чтобы добавить альтернативные ответы, введите их в текстовое поле и нажмите "Ввод". Для сохранения обновлений выберите **Сохранить и Обучать**.

![Добавление альтернативных вопросов](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Добавление нового ответа

Можно добавить новый ответ, если любой из существующих ответов, которые были сопоставлены, неверный или не существует ответа в базе знаний (в КБ нет хорошего совпадения). 

В нижней части списка ответы использовать текстовое поле, чтобы ввести новый ответ, и нажмите клавишу ВВОД, чтобы добавить его. 

Для сохранения этого ответа выберите **Сохранить и Обучать**. Новая пара вопросов и ответов теперь будет добавлена в базу знаний. 

> [!NOTE]
> Все изменения в базе знаний сохраняются только при нажатии кнопки **Сохранить и Обучать**.

## <a name="test-the-published-knowledge-base"></a>Проверить опубликованные базы знаний

Можно проверить опубликованную версию базы знаний в области тестирования. После публикации базы Знаний, выберите **опубликованные базы Знаний** поле и отправьте запрос для получения результатов из опубликованной базы Знаний.

![Проверить опубликованные КБ](../media/qnamaker-how-to-test-kb/test-against-published-kb.png)

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Публикация базы знаний](./publish-knowledge-base.md)
