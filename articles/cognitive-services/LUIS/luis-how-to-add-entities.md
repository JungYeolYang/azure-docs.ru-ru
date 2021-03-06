---
title: Добавление сущностей
titleSuffix: Language Understanding - Azure Cognitive Services
description: Создайте сущности для извлечения данных ключа из фразы пользователя в приложениях Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 241e89ac7fa78184e7c55f9e8065e1534cea9143
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148729"
---
# <a name="create-entities-without-utterances"></a>Создание сущностей без высказываний

Сущность представляет подлежащее извлечению слово или фразу в высказывании. Сущность представляет класс, включая коллекцию схожие объекты (места, вещи, людьми, события или концепции). Сущности описывают информацию, относящуюся к намерению, и иногда они необходимы приложению для выполнения поставленной задачи. Вы можете создавать сущности, при добавлении utterance к объекту intent или друг от друга (до или после) Добавление utterance к объекту intent.

Добавлять, изменять или удалять сущности в приложении LUIS можно с помощью **Списка сущностей** на странице **Сущности**. LUIS предоставляет два основных типа сущностей: [предварительно созданные сущности](luis-reference-prebuilt-entities.md) и [пользовательские сущности](luis-concept-entity-types.md#types-of-entities).

После создания сущности, результаты обучения компьютера, необходимо пометить все utterance примере всех способов, которые он находится в этой сущности.

<a name="add-prebuilt-entity"></a>

## <a name="add-a-prebuilt-entity-to-your-app"></a>Добавление предварительно созданных сущностей в приложение

Общие предварительно созданные сущности, добавляемые в приложение: *number* и *datetimeV2*. 

1. Войдите в раздел **Сборка** и выберите на левой панели **Сущности**.
 
1. На странице **Сущности** выберите **Добавление предварительно созданной сущности**.

1. В диалоговом окне **Добавление предварительно созданной сущности** выберите предварительно созданные сущности **number** и **datetimeV2**. Затем выберите **Готово**.

    ![Снимок экрана диалогового окна "Добавление предварительно созданной сущности"](./media/add-entities/list-of-prebuilt-entities.png)

<a name="add-simple-entities"></a>

## <a name="add-simple-entities-for-single-concepts"></a>Добавление простой сущностей для одного основные понятия

Простая сущность описывает одно понятие. Создайте сущность, чтобы извлекать названия отделов компаний, например *Отдел кадров* или *Производственный отдел*, с помощью следующей процедуры.   

1. В приложении выберите раздел **Сборка**, затем на левой панели нажмите **Сущности** и выберите **Создать сущность**.

1. Во всплывающем диалоговом окне введите `Location` в поле **Имя сущности**, выберите **Простое** из списка **Тип сущности**, а затем выберите **Готово**.

    Когда сущность будет создана, перейдите ко всем намерениям с примерами высказываний, содержащих сущность. В примере высказываний выделите текст и пометьте его как сущность. 

    Обычно [список фраз](luis-concept-feature.md) используют для усиления сигнала простой сущности.

<a name="add-regular-expression-entities"></a>

## <a name="add-regular-expression-entities-for-highly-structured-concepts"></a>Добавление сущностей регулярное выражение для структурированного основные понятия

Сущность регулярного выражения используется для извлечения данных из выражения на основе указанного регулярного выражения. 

1. В левой области навигации приложения выберите **Сущности**, а затем выберите **Создать сущность**.

1. Во всплывающем диалоговом окне введите `Human resources form name` в поле **Имя сущности**, выберите **Регулярное выражение** в списке **Тип сущности**, введите регулярное выражение `hrf-[0-9]{6}`, а затем выберите **Готово**. 

    Это регулярное выражение соответствует литеральными символами `hrf-`, затем 6 цифр для представления формы номеров для формы отдела кадров.

<a name="add-composite-entities"></a>

## <a name="add-composite-entities-to-group-into-a-parent-child-relationship"></a>Добавьте составной сущности для группировки в родительско дочернее отношение

Создавая составные сущности, можно определить связь между различными видами сущностей. В приведенном ниже примере сущность содержит регулярные выражения, а также предварительно созданные сущности имени.  

В высказывании `Send hrf-123456 to John Smith` текст `hrf-123456` связан с [регулярным выражением](#add-regular-expression-entities) "Отдел кадров", а `John Smith` извлечено предварительно созданной сущностью personName. Каждая сущность является частью большей родительской сущности. 

1. В приложении выберите **Сборка** и в левой области навигации раздела выберите **Сущности**, а затем — **Добавить предварительно созданную сущность**.

1. Добавьте предварительно созданную сущность **PersonName**. Инструкции см. в разделе [Добавление предварительно созданных сущностей](#add-prebuilt-entity). 

1. В левой области навигации выберите **Сущности**, а затем выберите **Создать сущность**.

1. Во всплывающем диалоговом окне введите `SendHrForm` в поле **Имя сущности**, затем выберите **Составная сущность** из списка **Тип сущности**.

1. Выберите **Добавить дочерний элемент** для добавления нового дочернего элемента.

1. В **Дочерний №1** выберите сущность **number** из списка.

1. В **Дочерний №2** выберите из списка сущность **Имя формы "Отдел кадров"**. 

1. Нажмите кнопку **Готово**.

<a name="add-pattern-any-entities"></a>

## <a name="add-patternany-entities-to-capture-free-form-entities"></a>Добавление сущностей Pattern.any для записи сущности в свободной форме

Сущности [Pattern.any](luis-concept-entity-types.md) допустимы только в [шаблонах](luis-how-to-model-intent-pattern.md), но не в намерениях. Этот вид сущности помогает службе LUIS найти конец сущностей различной длины и выбора машинного слова. Так как эта сущность используется в шаблоне, служба LUIS знает, где конец сущности в шаблоне высказывания.

Если приложение имеет намерение `FindHumanResourcesForm`, название извлеченной формы может конфликтовать с намерением прогнозирования. Чтобы уточнить, какие машинные слова находятся в названии формы, используйте Pattern.any в шаблоне. Прогнозирование LUIS начинается с высказывания. Сначала высказывание проверяется и сопоставляется сущностям при их обнаружении, а затем шаблон проверяется и сопоставляется. 

В высказывании `Where is Request relocation from employee new to the company on the server?` название формы оказывается каверзным, поскольку оно не является контекстно-зависимым и непонятно, когда заканчивается заголовок и где начинается остальная часть высказывания. В названиях порядок слов может быть любым, они могут состоять из одного слова, сложных фраз со знаками препинания и иметь бессмысленный порядок слов. Шаблон позволяет создать сущность, в которой можно извлечь полную и точную сущность. После обнаружения названия намерение `FindHumanResourcesForm` прогнозированное, потому что это намерение для шаблона.

1. В разделе **Сборка** на левой панели выберите **Сущности**, а затем — **Создать сущность**.

1. В диалоговом окне **Добавление сущности** введите `HumanResourcesFormTitle` в поле **Имя сущности** и выберите **Pattern.any** как **Тип сущности**.

    Чтобы использовать сущность pattern.any, добавьте шаблон на странице **Шаблоны** в разделе **Повышение производительности приложения** с помощью правильных фигурных скобок, например `Where is **{HumanResourcesFormTitle}** on the server?`.

    Если ваш шаблон, который включает сущность Pattern.any, извлекает сущности неправильно, используйте [явный список](luis-concept-patterns.md#explicit-lists), чтобы решить эту проблему. 

<a name="add-a-role-to-pattern-based-entity"></a>

## <a name="add-a-role-to-distinguish-different-contexts"></a>Добавление роли для различения разных контекстах

Роль — именованный подтип, исходя из контекста. Он доступен всем сущностям, включая сущности, предварительно созданные и узнали без машин. 

Синтаксис для роли **`{Entityname:Rolename}`** где имя сущности следует двоеточие, а затем имя роли. Например, `Move {personName} from {LocationUsingRoles:Origin} to {LocationUsingRoles:Destination}`.

1. В разделе **Сборка** на левой панели выберите **Сущности**.

1. Нажмите кнопку **Создать сущность**. Укажите имя `LocationUsingRoles`. Выберите тип **Простой** и выберите **Готово**. 

1. На левой панели выберите **Сущности**, а затем выберите новую сущность **LocationUsingRoles**, созданную на предыдущем шаге.

1. В текстовом поле **Имя роли** введите имя роли `Origin` и нажмите клавишу ВВОД. Добавьте имя второй роли `Destination`. 

    ![Снимок экрана добавления роли отправления сущности Location](./media/add-entities/roles-enter-role-name-text.png)

<a name="add-list-entities"></a>

## <a name="add-list-entities-for-exact-matches"></a>Добавление списка сущностей для точных совпадений

Сущности списка — это фиксированный закрытый набор связанных машинных слов. 

В приложении "Отдел кадров" может существовать список названий всех отделов вместе с синонимами к ним. При создании сущности не нужно знать все значения. Вы можете добавить больше значений после просмотра высказываний реального пользователя с синонимами.

1. В разделе **Сборка** на левой панели выберите **Сущности**, а затем — **Создать сущность**.

1. В диалоговом окне**Добавление сущности** введите `Department` в поле **Имя сущности** и выберите **Список** как **Тип сущности**. Нажмите кнопку **Готово**.
  
1. На странице "Сущность списка" можно добавлять нормализованные имена. В текстовом поле **Значения** выберите название отдела из списка, например `HumanResources`, а затем нажмите клавишу ВВОД на клавиатуре. 

1. Справа от нормализованного значения введите синонимы, нажав на клавиатуре клавишу ВВОД после каждого элемента.

1. Если необходимо добавить нормализованные элементы в список, выберите **Рекомендуемые**, чтобы просмотреть параметры в [семантическом словаре](luis-glossary.md#semantic-dictionary).

    ![Снимок экрана просмотра параметров при выборе рекомендуемых элементов](./media/add-entities/hr-list-2.png)


1. Выберите элемент в списке "Рекомендуемые", чтобы добавить его в качестве нормализованного значения или выберите **Добавить все**, чтобы добавить все элементы. 
    Можно импортировать значения в существующую сущность списка, используя существующий формат JSON.

    ```JSON
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```

<a name="change-entity-type"></a>

## <a name="do-not-change-entity-type"></a>Не изменяйте тип сущности

Интеллектуальная служба распознавания речи не позволяет изменить тип сущности, поскольку неизвестно, что добавлять или удалять для создания этой сущности. Чтобы изменить тип, лучше создать новую сущность правильного типа с незначительно отличающимся именем. После создания сущности в каждом высказывании удалите старое отмеченное имя сущности и добавьте новое. После того как все высказывания будут повторно отмечены, удалите старую сущность. 

<a name="create-a-pattern-from-an-utterance"></a>

## <a name="create-a-pattern-from-an-example-utterance"></a>Создать шаблон из utterance пример

Дополнительные сведения см. в разделе [Добавление шаблона на основе существующего фрагмента речи на странице сущности или намерения](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="train-your-app-after-changing-model-with-entities"></a>Обучение приложения после изменения модели с сущностями

После добавления, изменения и удаления сущностей выполните [обучение](luis-how-to-train.md) и [публикацию](luis-how-to-publish-app.md) приложения, чтобы применить изменения к запросам конечной точки. 

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о предварительно созданных сущностях см. в проекте [Распознаватели текста](https://github.com/Microsoft/Recognizers-Text). 

Сведения о том, как сущность отображается в ответе на запрос конечной точки JSON, см. в разделе [Извлечение данных](luis-concept-data-extraction.md).

Теперь, когда добавлены намерения, выражения и сущности, вы получаете базовое приложение LUIS. Узнайте, как [обучать](luis-how-to-train.md), [тестировать](luis-interactive-test.md) и [опубликовать](luis-how-to-publish-app.md) приложение.
 
