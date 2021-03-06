---
title: Защищенное хранилище клиента для Microsoft Azure
description: Техническое описание защищенного хранилища клиента для Microsoft Azure, это позволяет контролировать доступ к облаку поставщика, когда корпорации Майкрософт может потребоваться доступ к данным клиентов.
author: cabailey
ms.service: security
ms.topic: article
ms.author: cabailey
manager: barbkess
ms.date: 05/07/2019
ms.openlocfilehash: 468e392cd2c45d79cbb24f8d737a6e83fbcd2725
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079275"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Защищенное хранилище клиента для Microsoft Azure

> [!NOTE]
> Чтобы использовать эту функцию, у организации должен быть [план поддержки Azure](https://azure.microsoft.com/support/plans/) с минимальным уровнем **разработчика**.

Защищенное хранилище клиента для Microsoft Azure предоставляет интерфейс для клиентов просмотреть и утвердить или отклонить запросы на доступ данных клиента. Он используется в случаях, когда требуется доступ к данным клиентов во время запроса на поддержку специалист Майкрософт.

В этой статье рассматриваются как защищенное хранилище клиента запросы инициированный, отслеживается и хранится для более поздних обзоров и аудита.

Защищенное хранилище клиента выпущена общедоступная и в настоящее время доступна для удаленного рабочего стола доступ к виртуальным машинам.

## <a name="workflow"></a>Рабочий процесс

Ниже описан типичный рабочий процесс для запроса клиента защищенного хранилища.

1. Кто-то в организации есть проблемы с их рабочую нагрузку Azure.

2. После этого человека Диагностика проблемы, но не может устранить ее, они откройте запрос в службу поддержки [портала Azure](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac). Билет будет назначен специалисту службы поддержки клиентов Azure.

3. Инженер поддержки Azure просматривает запрос на обслуживание и определяет дальнейшие действия для устранения проблемы.

4. Если сотрудник службы поддержки не может устранить эту проблему с помощью стандартных средств и данных телеметрии, необходимо будет запрашивать разрешения с повышенными правами с помощью службы доступа, эту задачу выполняет JIT. Этот запрос можно из исходного специалисту службы поддержки. Или, может быть от инженера разных поскольку укрупнения проблемы команде DevOps в Azure.

5. После доступа отправляется запрос по Just-In-Time, инженер Azure оценивает запрос, учитывая факторов учетной записи, такие как:
    - Область видимости ресурса
    - Инициатор запроса, является ли удостоверение изолированного или с помощью многофакторной проверки подлинности
    - Уровни разрешений
    
    На основании правила JIT-компилятора, этот запрос может также включать утверждение из внутренних утверждающих лиц Майкрософт. Например утверждающее лицо может быть интереса поддержки клиента или диспетчера DevOps.

6. Если запрос требует прямого доступа к данным клиентов, инициируется запрос клиента защищенного хранилища. Например удаленному рабочему столу для виртуальной машины клиента.
    
    Запрос находится в **уведомление клиента** состояния, ожидая утверждения клиента перед предоставлением доступа.

7. В организации клиента, пользователь, имеющий [роли "Владелец"](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-rbac-roles) для Azure подписки получает сообщение электронной почты от корпорации Майкрософт, с уведомлением о запросе доступа. Для запросов клиента защищенного хранилища получатель этого письма является назначенный утверждающий.
    
    Пример сообщения электронной почты:
    
    ![Azure защищенное хранилище клиента - уведомление по электронной почте](./media/azure-customer-lockbox/customer-lockbox-email-notification.png)

8. Уведомление по электронной почте содержится ссылка **защищенного хранилища клиента** колонка на портале Azure. С помощью этой ссылки, назначенный утверждающий выполняет вход на портал Azure для просмотра всех ожидающих запросов, которые организации имеет для защищенного хранилища клиента:
    
    ![Azure защищенное хранилище клиента - целевая страница](./media/azure-customer-lockbox/customer-lockbox-landing-page.png)
    
   Запрос остается в очереди клиента в течение четырех дней. По истечении этого времени запрос на доступ автоматически истекает, и нет доступа к инженерами корпорации Майкрософт.

9. Чтобы получить сведения о ожидающий запрос, назначенный утверждающий можно выбрать запрос защищенного хранилища из **запросы в ожидании**:
    
    ![Защищенное хранилище Azure клиента - Просмотр ожидающий запрос](./media/azure-customer-lockbox/customer-lockbox-pending-requests.png)

10. Можно также выбрать назначенный утверждающий **идентификатор ЗАПРОСА службы** просмотреть запрос запрос в службу поддержки, который был создан исходный пользователь. Эти сведения позволят контекст для почему улучшением поддержки Майкрософт, а также журнал обнаруженной проблемы. Например: 
    
    ![Защищенное хранилище Azure клиента - просмотреть запрос билета](./media/azure-customer-lockbox/customer-lockbox-support-ticket.png)

11. После рассмотрения запроса, назначенный утверждающий выбирает **утвердить** или **Deny**:
    
    ![Azure клиента защищенное хранилище - выберите утвердить или отклонить](./media/azure-customer-lockbox/customer-lockbox-approval.png)
    
    В результате выбора:
    - **Утвердить**:  Доступ предоставляется инженер корпорации Microsoft. Доступ предоставляется по умолчанию в течение восьми часов.
    - **Запретить**: Отклонения запроса повышенных прав доступа, инженер корпорации Microsoft, и никакие дальнейшие действия не выполняются.

Для задач аудита записываются действия, выполняемые в этом рабочем процессе [журналы запросов защищенного хранилища клиента](#auditing-logs).

## <a name="auditing-logs"></a>Журналы аудита

Журналы клиента защищенного хранилища хранятся в журналах действий. На портале Azure выберите **журналы действий** Чтобы просмотреть данные аудита, связанные с запросами клиентов защищенного хранилища. Можно установить фильтр для определенных действий, таких как:
- **Отклонить запрос защищенного хранилища**
- **Создание запроса на защищенного хранилища**
- **Утвердить запрос защищенного хранилища**
- **Срок действия запроса защищенного хранилища**

Например:

![Azure защищенное хранилище клиента - журналы действий](./media/azure-customer-lockbox/customer-lockbox-activitylogs.png)

## <a name="supported-services-and-scenarios"></a>Поддерживаемые службы и сценариев

Следующие службы и сценарии предоставляемые в данный момент в режиме общедоступной для клиента защищенного хранилища.

### <a name="remote-desktop-access-to-virtual-machines"></a>Удаленному рабочему столу для виртуальных машин

Защищенное хранилище клиента включена для запросов удаленного доступа к рабочему столу для виртуальных машин. Поддерживаются следующие рабочие нагрузки:
- Платформа как услуга (PaaS) — версии 1
- Инфраструктура как услуга (IaaS) — Windows и Linux (только для Azure Resource Manager)
- Масштабируемый набор виртуальных машин — Windows и Linux

> [!NOTE]
> Экземпляры классической службы IaaS, защищенного хранилища клиента не поддерживается. При наличии рабочих нагрузок, выполняющихся в экземплярах классической службы IaaS, мы рекомендуем перенести их из классической модели развертывания в модель развертывания Resource Manager. Инструкции см. в разделе [поддерживаемый платформой перенос ресурсов IaaS из классической модели развертывания с помощью Azure Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

#### <a name="detailed-audit-logs"></a>Подробные журналы аудита

Для сценариев, включающих удаленному рабочему столу Чтобы просмотреть действия, предпринимаемые инженер корпорации Microsoft можно использовать журналы событий Windows. Рассмотрите возможность использования центра безопасности Azure для сбора журналов событий и скопировать данные в рабочую область для анализа. Дополнительные сведения см. в разделе [сбора данных в центре безопасности Azure](../security-center/security-center-enable-data-collection.md).

## <a name="exclusions"></a>Исключения

Запросы от клиентов защищенного хранилища не активируется в следующих сценариях инженеров поддержки:

- Специалист Майкрософт необходимо выполнить действие, которое выходит за пределы стандартных эксплуатационных процедур. Например, для восстановления службы в сценариях непредвиденное или непредсказуемыми.

- Специалист Майкрософт обращается к платформе Azure как часть устранения неполадок и случайно имеет доступ к данным клиентов. Например команда сети Azure выполняет, устранение неполадок, которые приводят к записи пакетов на сетевом устройстве. Тем не менее если клиент зашифрованные данные, когда он был при передаче, инженер исключается возможность считывать данные.

## <a name="next-steps"></a>Дальнейшие действия

Защищенное хранилище клиента автоматически доступна для всех клиентов, имеющих [план поддержки Azure](https://azure.microsoft.com/support/plans/) с минимальным уровнем **разработчика**.

При наличии плана поддержки право, никаких действий от вас не требуется для включения клиента защищенного хранилища. Запросы от клиентов защищенного хранилища инициируются специалист Майкрософт в том случае, если это действие, необходимое для выполнения запроса в службу поддержки, отправивший от кого-либо в вашей организации.
