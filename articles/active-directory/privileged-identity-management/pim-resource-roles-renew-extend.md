---
title: Продление или обновление назначений ролей ресурсов Azure в PIM — Azure Active Directory | Документация Майкрософт
description: Узнайте, как продлить или возобновить назначения ролей ресурсов в Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: a064fc67bf94ba6aa443e429fe83179d84cada84
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602651"
---
# <a name="extend-or-renew-azure-resource-role-assignments-in-pim"></a>Расширение или возобновление назначений ролей ресурсов Azure в PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) представляет новые элементы для управления доступом и жизненным циклом назначения ресурсов Azure. Администраторы могут назначать членство, используя свойства даты и времени начала и окончания. Перед завершением срока назначения PIM отправляет уведомления по электронной почте всем пользователям или группам, которых оно касается. Также уведомления по электронной почте отправляются администраторам ресурса, чтобы они приняли меры для сохранения требуемого доступа. Назначения нужно возобновить. Для удобства они остаются видимыми в течение 30 дней после истечения срока действия, даже если доступ не продлевается.

## <a name="who-can-extend-and-renew"></a>Кто может продлить и возобновить назначение?

Только администраторы ресурса могут продлевать или возобновлять назначения ролей. Участник может запросить продление для ролей с истекающим сроком действия или возобновление для ролей с уже истекшим сроком действия.

## <a name="when-are-notifications-sent"></a>Когда оправляются уведомления?

PIM отправляет уведомления по электронной почте администраторам и участникам тех ролей, срок действия которых истекает в течение 14 дней, и повторные уведомления за день до истечения срока действия. Еще одно сообщение отправляется по электронной почте в момент официального завершения срока назначения. 

Администраторы получают уведомления, когда участник с ролью с истекающим (или истекшим) сроком действия запрашивает продление или возобновление. Когда конкретный администратор отвечает на запрос, его решение (утверждение или отклонение запроса) пересылается всем остальным администраторам. Также об этом решении оповещается участник, направивший запрос. 

## <a name="extend-role-assignments"></a>Продление назначений ролей

Ниже описаны шаги, входящие в процессы запроса, ответа на запрос или продления или возобновления назначения роли. 

### <a name="member-extend"></a>Расширение участника

Участники с назначенной ролью могут запросить продление назначения, срок действия которого истекает, прямо на вкладке **Разрешенные** или **Активные** на странице **Мои роли** для конкретного ресурса, или из верхнего уровня страницы **Мои роли** на портале PIM. Участники могут запросить продление разрешенных и активных (назначенных) ролей, срок действия которых истекает в течение следующих 14 дней.

![Продление ролей](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-ui.png)

Когда до окончания назначения остается 14 дней, кнопка **Продлить** в пользовательском интерфейсе становится активной ссылкой. В приведенном ниже примере текущей датой является 27 марта.

![Расширить кнопки](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-within-14.png)

Чтобы запросить продление такого назначения роли, нажмите кнопку **Продлить** после чего откроется форма запроса.

![Открытие формы запроса](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-role-assignment-request.png)

Чтобы просмотреть информацию об исходном назначении, разверните пункт **Сведения о назначении**. Введите причину для запроса на продление и щелкните **Продлить**.

>[!Note]
>Мы рекомендуем подробно описать, для чего требуется продление и на какой срок (если у вас есть такая информация).

![Продление назначения роли](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-form-complete.png)

Уже через несколько секунд администраторы ресурсов получат по электронной почте уведомления с просьбой рассмотреть запрос на продление. Если запрос на продление уже был отправлен ранее, в верхней части портала Azure появится всплывающее уведомление с объяснением ошибки.

![Уведомление о том, ошибка](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-failed-existing-request.png)

Откройте вкладку **Ожидающие запросы** на панели слева, чтобы просмотреть состояние запроса или отменить его.

![Ожидающие запросы](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-cancel-request.png)

### <a name="admin-approve"></a>Утверждение администратором

Когда участник отправляет запрос на продление назначения роли, администраторы ресурсов получают по электронной почте уведомление с информацией об исходном назначении и причине, указанной в запросе. Уведомление содержит прямую ссылку на утверждение или отклонение запроса для администратора. 

Администраторы могут утвердить или отклонить запрос по ссылкам прямо из электронного письма или через портал администрирования PIM, выбрав раздел **Утверждение запросов** на панели слева.

![Снимок экрана с ошибкой](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-grid.png)

Когда администратор выбирает действие **Утвердить** или **Отклонить**, сведения о запросе отображаются вместе с полем, чтобы предоставить обоснование для журналов аудита.

![Утвердить запрос на назначение роли](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-blade.png)

Утверждая запрос на продление назначения роли, администраторы ресурсов могут выбрать новые даты начала и окончания, а также изменить тип назначения. Изменение типа назначения применяется в тех случаях, когда администратор предоставляет ограниченный доступ для выполнения конкретной задачи (например, на один день). В нашем примере администратор может изменить статус назначения с **Доступно** на **Активно**. Это означает, что запрашивающая сторона сразу получит указанный доступ, без необходимости активации.

### <a name="admin-extend"></a>Расширение администратора

Если участник роли забывает или не может запросить продление членства в роли, администратор может самостоятельно продлить назначение от имени участника. Когда администратор продлевает членства в ролях, дополнительное утверждение не требуется, но всем другим администраторам отправляется уведомление о продлении роли.

Чтобы продлить членство в роли, перейдите в PIM к представлению роли ресурса или участников роли. Найдите участника, для которого нужно продлить назначение. Выберите **Продлить** в столбце действий.

![Продление членства в роли](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-extend.png)

## <a name="renew-role-assignments"></a>Возобновление назначения ролей

В принципе, возобновление назначения роли похоже на продление назначения, но процесс для уже истекшего назначения существенно отличается. Описанные ниже шаги позволяют участникам и администраторам возобновить доступ к ролям с истекшим сроком действия, если это потребуется.

### <a name="member-renew"></a>Продление участника

Участники, которым уже не предоставляется доступ к ресурсам, еще в течение 30 дней могут получить информацию об истечении срока действия назначения. Для этого им нужно открыть раздел **Мои роли** в области слева и выбрать вкладку **Роли с истекшим сроком действия** в разделе ролей для ресурсов Azure.

![Вкладка "роли с истекшим сроком действия"](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-from-myroles.png)

По умолчанию в этом списке ролей отображаются **доступные роли**. Откройте раскрывающийся список, чтобы переключить режим отображения ("Доступные" или "Активные" роли).

Чтобы запросить возобновление для любого из назначений ролей в списке, выберите действие **Возобновить**. Укажите также причину запроса. Мы рекомендуем указать длительность и контекст продления. Это поможет администратору ресурсов сделать выбор: утвердить или отклонить запрос.

![Возобновление назначения роли](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-request-form.png)

После отправки запроса администраторы ресурсов получают уведомления об ожидающем запросе на возобновление назначения роли.

### <a name="admin-approves"></a>Утверждение администратором

Администраторы ресурсов могут получить доступ к запросу на возобновление, перейдя по ссылке в уведомлении по электронной почте или перейдя к PIM на портале Azure и выбрав **Утверждение запросов** на панели слева.

![Утверждение запросов](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-grid.png)

Когда администратор выбирает **Утвердить** или **Отклонить**, сведения о запросе отображаются вместе с полем, чтобы предоставить обоснование для журналов аудита.

![Утверждение назначения роли](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-blade.png)

При утверждении запроса на возобновление назначения роли администраторы ресурсов должны указать новые даты начала и окончания и выбрать тип назначения. 

### <a name="admin-renew"></a>Обновление администратора

Администраторы ресурсов могут возобновлять назначения ролей с истекшим сроком действия на вкладке **Участники** в меню навигации слева для конкретного ресурса. Также они могут возобновить истекшие назначения роли на вкладке **Истек срок действия** для роли ресурса.

Чтобы просмотреть список всех назначений ролей, на экране **Участники** выберите **Роли с истекшим сроком действия**.

![Роли с истекшим сроком действия](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-from-member-blade.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Подтверждение или отклонение запросов для ролей ресурсов Azure в PIM](pim-resource-roles-approval-workflow.md)
- [Настройка параметров роли ресурсов Azure в PIM](pim-resource-roles-configure-role-settings.md)
