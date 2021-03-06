---
title: включение файла
description: включение файла
services: cdn
author: SyntaxC4
ms.service: cdn
ms.topic: include
ms.date: 05/24/2018
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: c58b226c0f3bd63cb2a54bd3d8c91eb750a26f0a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60684663"
---
## <a name="create-a-new-cdn-profile"></a>Создание нового профиля сети CDN

Профиль CDN — это контейнер для конечных точек сети CDN, который определяет ценовую категорию.

1. На портале Azure слева вверху выберите **Создать ресурс**. 
    
    Появится панель **Создание**.
   
2. Выберите **Интернет и мобильные устройства**, а затем — **CDN**.
   
    ![Выбор ресурса CDN](./media/cdn-create-profile/cdn-new-resource.png)

    Появится панель **Профили CDN**.

3. Для параметров профиля CDN используйте значения, указанные в следующей таблице:
   
    | Параметр  | Значение |
    | -------- | ----- |
    | **Имя** | Введите *my-cdn-profile-123* в качестве имени профиля. Это имя должно быть глобально уникальным. Если оно уже используется, введите другое имя. |
    | **Подписка** | В раскрывающемся списке выберите подписку Azure. |
    | **Группа ресурсов** | Щелкните **Создать** и введите *my-resource-group-123* в качестве имени группы ресурсов. Оно должно быть глобально уникальным. Если это имя уже используется, вы можете ввести другое имя или в разделе **Use existing** (Использовать существующее) выбрать в раскрывающемся списке **my-resource-group-123**. | 
    | **Расположение группы ресурсов** | В раскрывающемся списке выберите **Central US**. |
    | **Ценовая категория** | В раскрывающемся списке выберите **Verizon уровня "Стандартный"**. |
    | **Создать конечную точку CDN** | Не устанавливайте этот флажок. |  
   
    ![Новый профиль CDN](./media/cdn-create-profile/cdn-new-profile.png)

4. Чтобы сохранить созданный профиль на панели мониторинга установите флажок **Закрепить на панели мониторинга**.
    
5. Нажмите кнопку **Создать**, чтобы создать профиль. 

    Для профилей **Azure CDN уровня "Стандартный" от Майкрософт** (исключительно) заполнение обычно завершается в течение двух часов. 

