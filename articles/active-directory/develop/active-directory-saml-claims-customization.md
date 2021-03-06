---
title: Настройка утверждений, выпущенных в токене SAML для корпоративных приложений в Azure AD | Документация Майкрософт
description: Узнайте, как настраивать утверждения, выпущенные в токене SAML для корпоративных приложений в Azure AD.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2019
ms.author: ryanwi
ms.reviewer: luleon, paulgarn, jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c1f8640918d433956935e9428e23aac59e36334
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/16/2019
ms.locfileid: "65764666"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Практическое руководство: Настройка утверждений, выпущенных в токене SAML для корпоративных приложений

В настоящее время Azure Active Directory (Azure AD) поддерживает единый вход (SSO) в большинстве корпоративных приложений, включая предварительно интегрированные в коллекции приложений Azure AD, а также пользовательские приложения приложения. Когда пользователь проходит аутентификацию для приложения в Azure AD с помощью протокола SAML 2.0, Azure AD отправляет токен в приложение (через запрос HTTP POST). Затем приложение проверяет и использует маркер для входа пользователя вместо запроса имени пользователя и пароля. Эти маркеры SAML содержат элементы информации о пользователе, известный как *утверждений*.

*Утверждение* представляет собой информацию, предложенную поставщиком удостоверений, о пользователе в составе токена, выпущенного для этого пользователя. В [токене SAML](https://en.wikipedia.org/wiki/SAML_2.0)эти данные обычно содержит оператор атрибута SAML. А уникальный идентификатор пользователя, как правило, представлен в субъекте SAML, который также называют идентификатором имени.

По умолчанию, Azure AD выпускает маркер SAML для приложения, которое содержит `NameIdentifier` утверждения с указанием имени пользователя (также известный как имя участника-пользователя) в Azure AD, который может идентифицировать пользователя. Маркер SAML включает также дополнительные утверждения, содержащие адрес электронной почты, имя и фамилию пользователя.

Чтобы просмотреть или изменить утверждения, выданные приложению в токене SAML, откройте приложение на портале Azure. Затем откройте **атрибуты пользователя и утверждения** раздел.

![В разделе атрибутов пользователя & утверждений](./media/active-directory-saml-claims-customization/sso-saml-user-attributes-claims.png)

Изменение утверждений, выданных в маркере SAML, может потребоваться по двум основным причинам:

* Приложению требуется `NameIdentifier` или NameID выдает что-то отличное от имени пользователя (или имя участника-пользователя), хранящиеся в Azure AD.
* Приложение требует другого набора URI утверждений или значений утверждений.

## <a name="editing-nameid"></a>Изменение идентификатора имени

Изменение идентификатора имени (значение идентификатора имени):

1. Откройте **имя значение идентификатора** страницы.
1. Выберите атрибут или преобразования, которые необходимо применить к атрибуту. При необходимости можно указать формат, в который он утверждение NameID иметь.

   ![Изменить значение идентификатора имени (идентификатора имени)](./media/active-directory-saml-claims-customization/saml-sso-manage-user-claims.png)

### <a name="nameid-format"></a>Формат идентификатора имени

Если запрос SAML содержит элемент политики идентификатора имени определенного формата, Azure AD будет поддерживать формата, в запросе.

Если в запросе SAML не содержит элемент, для политики идентификатора имени, Azure AD будет выдавать NameID в формате, указанную вами. Если формат не задан в Azure AD будет использовать исходный формат по умолчанию, связанные с выбранным источником утверждения.

Из **формат идентификатора имени Выбор** раскрывающемся списке можно выбрать один из следующих вариантов.

| Формат идентификатора имени | Описание |
|---------------|-------------|
| **По умолчанию** | Azure AD будет использовать исходный формат по умолчанию. |
| **Постоянные** | Azure AD будет использовать постоянный формат идентификатора имени. |
| **EmailAddress** | Azure AD будет использовать EmailAddress в качестве формата NameID. |
| **Этот атрибут не задан** | Azure AD будет использовать не указано в качестве формата идентификатора имени. |
| **Несохраняемым** | Несохраняемым будет использоваться в Azure AD как формат идентификатора имени. |

Дополнительные сведения об атрибуте политики идентификатора имени см. в разделе [протокол SAML](single-sign-on-saml-protocol.md).

### <a name="attributes"></a>Атрибуты

Выберите нужный источник для утверждения `NameIdentifier` (или NameID). Вы можете выбирать из следующих параметров.

| ИМЯ | Описание |
|------|-------------|
| Адрес эл. почты | Адрес электронной почты пользователя |
| userprincipalName | Имя участника-пользователя (UPN) пользователя |
| onpremisessamaccount | Имя учетной записи SAM, синхронизированное из локального Azure AD |
| objectid | Идентификатор объекта пользователя в Azure AD |
| employeeid | EmployeeID пользователя |
| Расширения каталога | Расширения каталогов, [синхронизированные из локальной Active Directory с помощью синхронизации Azure AD Connect](../hybrid/how-to-connect-sync-feature-directory-extensions.md) |
| Атрибуты расширения 1–15 | Локальные атрибуты расширения, используемые для расширения схемы Azure AD |

Дополнительные сведения см. в разделе [3 таблицы: Допустимые значения Идентификаторов для одного источника](active-directory-claims-mapping.md#table-3-valid-id-values-per-source).

### <a name="special-claims---transformations"></a>Специальные утверждения - преобразования

Можно также использовать функции преобразования утверждений.

| Функция | Описание |
|----------|-------------|
| **ExtractMailPrefix()** | Удаляет суффикса домена из адреса электронной почты или имя участника-пользователя. В таком случае будет извлекаться только первая часть передаваемого имени пользователя (например, joe_smith вместо joe_smith@contoso.com). |
| **функция JOIN()** | Присоединяет атрибут к проверенному домену. Если выбранное значение идентификатора пользователя имеет домен, он извлечет имя пользователя для добавления выбранного проверенного домена. Например, если в качестве значения идентификатора пользователя вы выберите адрес электронной почты (joe_smith@contoso.com), а в качестве проверенного домена выберите contoso.onmicrosoft.com, то в результате получите joe_smith@contoso.onmicrosoft.com. |
| **ToLower()** | Преобразует символы выбранного атрибута в символы нижнего регистра. |
| **ToUpper()** | Преобразует символы выбранного атрибута в символы верхнего регистра. |

## <a name="adding-application-specific-claims"></a>Добавление утверждения для конкретного приложения

Чтобы добавить утверждения для конкретного приложения:

1. В **атрибуты пользователя и утверждения**выберите **добавить новое утверждение** открыть **управляет утверждениями пользователя** страницы.
1. Введите **имя** утверждений. Значение не должно строго соответствовать шаблону URL, в спецификации SAML. Если вам требуется шаблон URI, который можно поместить в **пространства имен** поля.
1. Выберите **источника** где утверждение будет извлечь его значение. Можно выбрать атрибут пользователя из раскрывающегося списка атрибут источника или применить преобразование к атрибуту пользователя до его введения в качестве утверждения.

### <a name="application-specific-claims---transformations"></a>Утверждения для конкретного приложения - преобразования

Можно также использовать функции преобразования утверждений.

| Функция | Описание |
|----------|-------------|
| **ExtractMailPrefix()** | Удаляет суффикса домена из адреса электронной почты или имя участника-пользователя. В таком случае будет извлекаться только первая часть передаваемого имени пользователя (например, joe_smith вместо joe_smith@contoso.com). |
| **функция JOIN()** | Создает новое значение путем объединения двух атрибутов. При необходимости можно использовать разделитель между двумя атрибутами. |
| **ToLower()** | Преобразует символы выбранного атрибута в символы нижнего регистра. |
| **ToUpper()** | Преобразует символы выбранного атрибута в символы верхнего регистра. |
| **CONTAINS()** | Выводит атрибут или константу, если входные данные соответствуют указанному значению. В противном случае можно указать другой выход, если совпадения нет.<br/>Например, если вы хотите выдавать утверждения, где значение — адрес электронной почты пользователя, если он содержит домен "@contoso.com«, в противном случае нужно вывести имя участника-пользователя. Для этого следует настроить следующие значения:<br/>*Параметр 1(input)*: user.email<br/>*Значение*: "@contoso.com"<br/>Параметр 2 (вывод): user.email<br/>Параметр 3 (выходные данные, если нет совпадений): user.userprincipalname |
| **EndWith()** | Выводит атрибут или константа, если входные данные с указанным значением. В противном случае можно указать другой выход, если совпадения нет.<br/>Например если вы хотите выдавать утверждения, где значение — employeeid пользователя, если employeeid заканчивается «000», в противном случае нужно вывести атрибут расширения. Для этого следует настроить следующие значения:<br/>*Параметр 1(input)*: параметром user.employeeid<br/>*Value* (Значение): "000"<br/>Параметр 2 (вывод): параметром user.employeeid<br/>Параметр 3 (выходные данные, если нет совпадений): атрибут user.extensionattribute1 |
| **StartWith()** | Выводит атрибут или константа, если входные данные начинается с указанного значения. В противном случае можно указать другой выход, если совпадения нет.<br/>Например если вы хотите выдавать утверждения, где значение — employeeid пользователя, Если страны или региона начинается с «США», в противном случае нужно вывести атрибут расширения. Для этого следует настроить следующие значения:<br/>*Параметр 1(input)*: user.country<br/>*Value* (Значение): «США»<br/>Параметр 2 (вывод): параметром user.employeeid<br/>Параметр 3 (выходные данные, если нет совпадений): атрибут user.extensionattribute1 |
| **Extract() - после сопоставления** | Возвращает подстроку после его совпадает с указанным значением.<br/>Например если значение элемента input «Finance_BSimon», соответствующее значение — «Finance_», а затем выходные данные утверждения — «BSimon». |
| **Extract() - до соответствия** | Возвращает подстроку, пока не будет соответствовать указанному значению.<br/>Например если значение элемента input «BSimon_US», соответствующее значение — «_US», а затем выходные данные утверждения — «BSimon». |
| **Extract() - между совпадающими** | Возвращает подстроку, пока не будет соответствовать указанному значению.<br/>Например если значение элемента input «Finance_BSimon_US», первое совпадающее значение — «Finance_», второе значение сопоставления — «_US», а затем выходные данные утверждения — «BSimon». |
| **ExtractAlpha() - префикс** | Возвращает префикс алфавитном часть строки.<br/>Например если значение элемента input «BSimon_123», он возвращает «BSimon». |
| **ExtractAlpha() - суффикс** | Возвращает суффикс алфавитном часть строки.<br/>Например если значение элемента input «123_Simon», он возвращает «Молчунов». |
| **ExtractNumeric() - префикс** | Возвращает префикс числовой части строки.<br/>Например если значение элемента input «123_BSimon», он возвращает «123». |
| **ExtractNumeric() - суффикс** | Возвращает числовой суффикс часть строки.<br/>Например если значение элемента input «BSimon_123», он возвращает «123». |
| **IfEmpty()** | Выводит атрибут или константа, если входные данные — null или пустым.<br/>Например, если требуется направить вывод атрибут, хранящиеся в extensionattribute, если для данного пользователя столбец employeeid является пустым. Для этого следует настроить следующие значения:<br/>Параметр 1(input): параметром user.employeeid<br/>Параметр 2 (вывод): атрибут user.extensionattribute1<br/>Параметр 3 (выходные данные, если нет совпадений): параметром user.employeeid |
| **IfNotEmpty()** | Выводит атрибут или константа, если входные данные не равен null или пуст.<br/>Например, если требуется направить вывод атрибут хранится в extensionattribute Если employeeid для данного пользователя не является пустым. Для этого следует настроить следующие значения:<br/>Параметр 1(input): параметром user.employeeid<br/>Параметр 2 (вывод): атрибут user.extensionattribute1 |

Если вам нужны дополнительные преобразования, отправить свои идеи в [форуме обратной связи в Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=160599) под *приложения SaaS* категории.

## <a name="next-steps"></a>Дальнейшие действия

* [Управление приложениями с помощью Azure Active Directory](../manage-apps/what-is-application-management.md)
* [Настройка федеративного единого входа для приложения не из коллекции](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [Устранение неполадок единого входа на основе SAML](howto-v1-debug-saml-sso-issues.md)
