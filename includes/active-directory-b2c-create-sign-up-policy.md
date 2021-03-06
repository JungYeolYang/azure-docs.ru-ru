---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
origin.date: 11/30/2018
ms.date: 04/04/2019
ms.author: v-junlch
ms.openlocfilehash: 17c0213d63879687e9c6d5f8dca06b9113c44af8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60456906"
---
Если вам нужно добавить в приложение только функцию регистрации, воспользуйтесь потоком пользователя **регистрации**. Этот поток описывает действия, которые пользователь должен выполнить во время регистрации, и содержимое токенов, которые приложение получает при успешной регистрации.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

В разделе **Управление** выберите **Потоки пользователей**.

Щелкните +**Новый поток пользователя** в верхней части колонки.

В разделе **Выбор типа потока пользователя** щелкните **Все**, а затем выберите нужную версию **регистрации**.

**Имя** определяет имя потока пользователя регистрации, используемое приложением. Например, введите **SiUp**.

В разделе **Поставщики удостоверений** выберите **Регистрация по электронной почте**. При необходимости можно также выбрать поставщиков удостоверений в социальных сетях, если они уже настроены.

В разделе **Атрибуты пользователя и утверждения** щелкните **Показать еще**.

В столбце **Собрать атрибут** выберите атрибуты, которые пользователь должен предоставить во время регистрации. Например, выберите **Страна или регион**, **Отображаемое имя** и **Почтовый индекс**.

В столбце **Вернуть утверждение** вы можете выбрать утверждения, которые необходимо возвращать в маркерах, отправляемых приложению после успешной регистрации. Например, выберите **Отображаемое имя**, **Поставщик удостоверений**, **Почтовый индекс**, **Новый пользователь** и **Идентификатор объекта пользователя**.

Последовательно выберите **ОК**.

Нажмите кнопку **Создать**. Созданный поток пользователя отображается как **B2C_1_SiUp** (фрагмент **B2C\_1\_** добавляется автоматически).

Щелкните **Выполнить поток пользователя**.

В раскрывающемся списке **Приложения** выберите пункт **Приложение B2C Contoso**, а в списке **URL-адрес ответа** — `https://localhost:44321/`.

Щелкните **Выполнить поток пользователя**. Откроется новая вкладка браузера, и вы сможете пройти процесс регистрации в своем приложении.

> [!NOTE]
> Создание, обновление и выполнение потока пользователя занимает около минуты.
>

