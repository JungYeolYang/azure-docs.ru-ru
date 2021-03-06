---
title: Руководство по Интеграция Azure Active Directory с Yonyx Interactive Guides | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Yonyx Interactive Guides.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fce7653770d0fb3134f266137e167125b9161865
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60863198"
---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a>Руководство по Интеграция Azure Active Directory с Yonyx Interactive Guides

В этом руководстве описано, как интегрировать Yonyx Interactive Guides с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Yonyx Interactive Guides обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к Yonyx Interactive Guides.
- Вы можете включить автоматический вход пользователей в Yonyx Interactive Guides (единый вход) с помощью учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с Yonyx Interactive Guides, вам потребуется:

- подписка Azure AD;
- подписка Yonyx Interactive Guides с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Yonyx Interactive Guides из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-yonyx-interactive-guides-from-the-gallery"></a>Добавление Yonyx Interactive Guides из коллекции
Чтобы настроить интеграцию Yonyx Interactive Guides с Azure AD, необходимо добавить Yonyx Interactive Guides из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Yonyx Interactive Guides из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"][3]

1. В поле поиска введите **Yonyx Interactive Guides**, выберите **Yonyx Interactive Guides** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Yonyx Interactive Guides в списке результатов](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Yonyx Interactive Guides с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Yonyx Interactive Guides соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователям Azure AD и соответствующим пользователем в Yonyx Interactive Guides.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Yonyx Interactive Guides.

Чтобы настроить и проверить единый вход Azure AD в Yonyx Interactive Guides, вам потребуется выполнить следующие стандартные действия.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Yonyx Interactive Guides](#create-a-yonyx-interactive-guides-test-user)**. Требуется для создания в Yonyx Interactive Guides пользователя Britta Simon, связанного с соответствующим представлением пользователя в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В данном разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Yonyx Interactive Guides.

**Чтобы настроить единый вход Azure AD в Yonyx Interactive Guides, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Yonyx Interactive Guides** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Yonyx Interactive Guides** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа приложения Yonyx Interactive Guides](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<company name>.yonyx.com`

    > [!NOTE] 
    > Эти значения приведены для примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки Yonyx Interactive Guides](mailto:support@yonyx.com). 
 
1. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)**, а затем сохраните файл сертификата на компьютере.

    ![Ссылка для скачивания сертификата](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/yonyx-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация Yonyx Interactive Guides** нажмите **Настроить Yonyx Interactive Guides**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Конфигурация Yonyx Interactive Guides](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_configure.png) 

1. Чтобы настроить единый вход на стороне **Yonyx Interactive Guides**, нужно отправить скачанный **сертификат в кодировке Base64**, **URL-адрес выхода**, **URL-адрес службы единого входа SAML** и **идентификатор сущности SAML** [группе поддержки Yonyx Interactive Guides](mailto:support@yonyx.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).


### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

  ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](./media/yonyx-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/yonyx-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Кнопка "Добавить"](./media/yonyx-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Диалоговое окно "Пользователь"](./media/yonyx-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-a-yonyx-interactive-guides-test-user"></a>Создание тестового пользователя Yonyx Interactive Guides

Цель этого раздела — создать в приложении Yonyx Interactive Guides пользователя с именем Britta Simon. Приложение Yonyx Interactive Guides поддерживает JIT-подготовку. Эта функция включена по умолчанию.

В этом разделе никакие действия с вашей стороны не требуются. При попытке получить доступ к Yonyx Interactive Guides создается пользователь (если он еще не создан).

> [!NOTE]
> Чтобы создать пользователя вручную, необходимо обратиться в службу поддержки Yonyx Interactive Guides по адресу <mailto:support@yonyx.com>. 

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Yonyx Interactive Guides.

![Назначение роли пользователя][200]

**Чтобы назначить пользователя Britta Simon в Yonyx Interactive Guides, выполните следующие действия:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Yonyx Interactive Guides**.

    ![Ссылка на Yonyx Interactive Guides в списке приложений](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Тест единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Yonyx Interactive Guides на панели доступа, вы автоматически войдете в приложение Yonyx Interactive Guides.

См. дополнительные сведения о [панели доступа](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/yonyx-tutorial/tutorial_general_01.png
[2]: ./media/yonyx-tutorial/tutorial_general_02.png
[3]: ./media/yonyx-tutorial/tutorial_general_03.png
[4]: ./media/yonyx-tutorial/tutorial_general_04.png

[100]: ./media/yonyx-tutorial/tutorial_general_100.png

[200]: ./media/yonyx-tutorial/tutorial_general_200.png
[201]: ./media/yonyx-tutorial/tutorial_general_201.png
[202]: ./media/yonyx-tutorial/tutorial_general_202.png
[203]: ./media/yonyx-tutorial/tutorial_general_203.png

