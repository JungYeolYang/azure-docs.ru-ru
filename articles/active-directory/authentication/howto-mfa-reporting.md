---
title: Отчеты о доступе и использовании для многофакторной Идентификации Azure — Azure Active Directory
description: Эта статья содержит информацию о том, как использовать функцию отчетов службы Многофакторной идентификации Azure.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64d4c48697d38cfa5942e09cb672af37c27eede2
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64688674"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Отчеты в службе Многофакторной идентификации Azure

Служба "Многофакторная идентификация Azure" предоставляет несколько типов отчетов, которые могут быть полезны для вас и вашей организации. Просмотреть их можно на портале Azure. В следующей таблице перечислены доступные отчеты:

| Отчет | Location | ОПИСАНИЕ |
|:--- |:--- |:--- |
| Журнал заблокированных пользователей | "Azure AD > Сервер MFA > Блокировать или разблокировать пользователей" | История запросов на блокировку или разблокирование пользователей. |
| Предупреждения об использовании и о мошенничестве | "Azure AD > События входа" | Сведения об общем использовании, сводка пользователей, сведения о пользователях, а также история предупреждений о мошенничестве, отправленных в течение указанного диапазона дат. |
| Использование локальных компонентов | "Azure AD > Сервер MFA > Отчет о действиях" | Сведения об общем использовании MFA через расширение NPS, ADFS и сервер MFA. |
| Журнал обойденных пользователей | "Azure AD > Сервер MFA > Одноразовый обход проверки" | История запросов на обход многофакторной проверки подлинности для пользователя. |
| Состояние сервера | "Azure AD > Сервер MFA > Состояние сервера" | Сведения о состоянии серверов Многофакторной идентификации, с которыми связана ваша учетная запись. |

## <a name="view-mfa-reports"></a>Просмотр отчетов MFA

1. Войдите на [портале Azure](https://portal.azure.com).
2. В левой части экрана выберите **Azure Active Directory** > **Сервер MFA**.
3. Выберите отчет, который нужно просмотреть.

   ![Отчет о состоянии сервера многофакторной проверки Подлинности сервера на портале Azure](./media/howto-mfa-reporting/report.png)

## <a name="azure-ad-sign-ins-report"></a>Отчет по действиям входа Azure AD

**Отчеты по действиям входа** на [портале Azure](https://portal.azure.com) позволяют получать всю необходимую информацию, чтобы определить, как работает среда.

Благодаря этим отчетам вы получаете информацию об использовании управляемых приложений и действиях входа пользователей, которые включают сведения об использовании многофакторной проверки подлинности (MFA). Эти данные позволяют понять, как работает многофакторная проверка подлинности в вашей организации. Они позволяют ответить на такие вопросы:

- Выполнена ли многофакторная проверка подлинности при входе?
- Как пользователь выполнил ее?
- Почему пользователю не удалось это сделать?
- Сколько пользователей проходят многофакторную проверку подлинности?
- Скольким пользователям не удалось ее пройти?
- С какими общими проблемами сталкиваются пользователи, проходя многофакторную проверку подлинности?

Доступ к этим данным можно получить через [портал Azure](https://portal.azure.com) и [API отчетов](../reports-monitoring/concept-reporting-api.md).

![Входы в систему отчета Azure AD на портале Azure](./media/howto-mfa-reporting/sign-in-report.png)

### <a name="sign-ins-report-structure"></a>Структура отчета по действиям входа

Отчеты по действиям входа для многофакторной проверки подлинности позволяют получить доступ к следующим данным:

**Необходимость многофакторной проверки подлинности.** Требуется ли многофакторная проверка подлинности при входе в систему. MFA может потребоваться из-за соответствующих параметров для каждого пользователя, условного доступа или по другим причинам. Возможные значения: **Да** или **нет**.

**Результат многофакторной проверки подлинности.** Дополнительные сведения о выполнении или сбое многофакторной проверки подлинности:

- Если многофакторная проверка подлинности выполнена, в этом столбце можно просмотреть дополнительные сведения о ее выполнении.
   - Многофакторная идентификация Azure
      - Завершено в облаке.
      - Истек срок действия из-за политик, настроенных для клиента.
      - Запрос регистрации.
      - Выполнено путем утверждения в токене.
      - Выполнено путем утверждения, предоставленного внешним поставщиком.
      - Выполнено за счет строгой проверки подлинности.
      - Пропущено, так как был выполнен поток входа в брокер Windows.
      - Пропущено из-за пароля приложения.
      - Пропущено из-за расположения.
      - Пропущено из-за зарегистрированного устройства.
      - Пропущено из-за устройства.
      - Успешно завершено.
   - Перенаправлено к внешнему поставщику многофакторной проверки подлинности.

- Если произошел сбой многофакторной проверки подлинности, в этом столбце будет указана причина сбоя.
   - Отказ службы Azure Multi-Factor Authentication
      - Выполняется проверка подлинности.
      - Попытка повторного выполнения проверки подлинности.
      - Неверный код указан слишком много раз.
      - Недействительная проверка подлинности.
      - Недействительный код проверки в мобильном приложении.
      - Неправильные настройки.
      - Звонок был направлен на голосовую почту.
      - Недопустимый формат номера телефона.
      - Ошибка службы.
      - Не удается подключиться к телефону пользователя.
      - Не удается отправить уведомление от мобильного приложения на устройстве.
      - Не удается отправить уведомление от мобильного приложения.
      - Пользователь отклонил проверку подлинности.
      - Пользователь не отвечает на уведомление от мобильного приложения.
      - Пользователь не зарегистрировал методы проверки.
      - Пользователь ввел неправильный код.
      - Пользователь ввел неправильный ПИН-код.
      - Пользователь отклонил телефонный звонок без успешного выполнения проверки подлинности.
      - Пользователь заблокирован.
      - Пользователь не ввел код проверки.
      - Не удалось найти пользователя.
      - Код проверки уже использован один раз.

**Метод многофакторной проверки подлинности.** Метод многофакторной проверки подлинности, примененный пользователем. Ниже перечислены возможные значения.

- Текстовое сообщение
- Уведомление от мобильного приложения
- Телефонный звонок (телефон для проверки подлинности).
- Код проверки в мобильном приложении
- Телефонный звонок (рабочий телефон).
- Телефонный звонок (альтернативный для проверки подлинности).

**Сведения о многофакторной проверке подлинности.** Очищенная версия номера телефона, например +X XXXXXXXX64.

**Условный доступ** Узнайте о политиках условного доступа, которые затрагиваются попытками входа, в том числе:

- Имя политики
- Элементы управления предоставлением прав
- Элементы управления сеансов
- Результат

## <a name="powershell-reporting-on-users-registered-for-mfa"></a>PowerShell, отчеты о пользователей, зарегистрированных для многофакторной проверки Подлинности

Во-первых, убедитесь, что у вас есть [модуля MSOnline PowerShell версии 1](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0) установлен.

Определите пользователей, зарегистрированных для многофакторной проверки подлинности с помощью Powershell:

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Определите пользователей, не зарегистрированных для многофакторной проверки подлинности с помощью Powershell:

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Дальнейшие действия

* [Для пользователей](../user-help/multi-factor-authentication-end-user.md)
* [Выберите для себя решение "Многофакторная идентификация Microsoft Azure"](concept-mfa-whichversion.md)
