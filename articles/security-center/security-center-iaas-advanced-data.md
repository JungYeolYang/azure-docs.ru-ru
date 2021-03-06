---
title: Дополнительные данные безопасности для IaaS в центре безопасности Azure | Документация Майкрософт
description: " Узнайте, как включить дополнительные данные безопасности для IaaS в центре безопасности Azure. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: monhaber
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2019
ms.author: monhaber
ms.openlocfilehash: e601bbaa0d15078fc2b19b5b7c536e3a1f6d20ad
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442752"
---
# <a name="advanced-data-security-for-sql-servers-on-iaas"></a>Дополнительные данные безопасности для серверов SQL Server в IaaS
Дополнительные данные безопасности для серверов SQL Server в IaaS — это единый пакет, для обеспечения дополнительных возможностей безопасности SQL. В настоящее время, он содержит функции, отображая и Устранение потенциальных уязвимостей базы данных и обнаружение аномальных действий, которые могут указывать угрозу для базы данных.

Предложение IaaS SQL Server для безопасности основан на ту же фундаментальную технологию, которая используется в [Azure безопасности базы данных SQL дополнительные данные пакета](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).


## <a name="overview"></a>Обзор

Дополнительные данные безопасности (ADS) предоставляет набор расширенные возможности безопасности SQL, состоящее из оценки уязвимостей и Advanced Threat Protection.

* [Оценка уязвимостей](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) — это легко настраиваемая служба, которая помогает обнаруживать, отслеживать и устранять потенциальные уязвимости базы данных. Он дает представление о состоянии безопасности и включает в себя действия для устранения проблем безопасности и повышения fortifications вашей базы данных.
* [Advanced Threat Protection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) выявляет аномальные операции, указывающие на нестандартные и потенциально вредоносные попытки получить доступ к или воспользоваться SQL server. Он постоянно отслеживает базу данных для подозрительных действий и предоставляет оповещения системы безопасности, действия, ориентированные на шаблоны доступа к данных с подозрительной активностью. Эти уведомления предоставляют сведения о подозрительной активности и рекомендуемые действия по поиску и устранению угрозы.

## <a name="get-started-with-ads-for-iaas"></a>Начало работы с помощью решения ADS для IaaS

Следующие шаги приступить к работе с помощью решения ADS для IaaS.

### <a name="set-up-ads-for-iaas"></a>Настройка РЕКЛАМЫ для IaaS

**Перед началом**: Требуется рабочая область Log Analytics для хранения журналов безопасности выполняется анализ. Если у вас один, то ее можно создать легко, как описано в [создать рабочую область Log Analytics на портале Azure](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

1. Подключите виртуальную Машину, где размещен SQL server в рабочую область Log Analytics. Инструкции см. в разделе [компьютеров Windows для подключения к Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).

1. Из Azure Marketplace, перейдите к [решение для обеспечения безопасности данных Advanced SQL](https://ms.portal.azure.com/#create/Microsoft.SQLAdvancedDataSecurity).
(Можно найти с помощью параметра поиска marketplace в качестве см. на следующем рисунке.) **SQL повышенной безопасности данных** откроется страница.

    ![Дополнительные данные безопасности для IaaS](./media/security-center-advanced-iaas-data/sql-advanced-data-security.png)

1. Нажмите кнопку **Создать**. Отображаются рабочие места.

    ![Создание расширенных данных безопасности](./media/security-center-advanced-iaas-data/sql-advanced-data-create.png)

1. Выберите рабочую область и нажмите кнопку **создать**.

   ![Выберите рабочую область](./media/security-center-advanced-iaas-data/sql-workspace.png)

1. Перезапустите [Виртуальной машины SQL server](https://docs.microsoft.com/sql/database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services?view=sql-server-2017).


## <a name="explore-and-investigate-security-alerts"></a>Изучение и анализ предупреждений безопасности

Можно просматривать и управлять оповещениями системы безопасности текущей.

1. Нажмите кнопку **центр безопасности** > **оповещения системы безопасности**и нажмите на оповещение.

    ![Найти оповещение](./media/security-center-advanced-iaas-data/find-alert.png)

1. Из **атаке ресурсов** столбец, щелкните ресурс, который подверглись атаке.

1. Чтобы просмотреть сведения о предупреждении и действия для изучения текущей угрозы и адресации будущих угроз, прокрутите вниз **Общие сведения** страницы и в **действия по исправлению** щелкните  **ДЕЙСТВИЯ для ИССЛЕДОВАНИЯ** ссылку.

    ![Действия по исправлению](./media/security-center-advanced-iaas-data/remediation-steps.png)

1. Чтобы просмотреть журналы, которые связаны с вызывающей его срабатывание предупреждения, перейдите к **рабочие области Log analytics** и выполните следующие действия:

     > [!NOTE]
     > Если **рабочие области Log analytics** не отображается в меню слева, щелкните **все службы**и выполните поиск **рабочие области Log analytics**.

    1. Убедитесь, что столбцы при отображении **Ценовая** и **WorkspaceID** столбцы. (**Рабочие области log analytics** > **изменить столбцы**, добавьте **Ценовая** и **WorkspaceID**.)

     ![Правка столбцов](./media/security-center-advanced-iaas-data/edit-columns.png)

    1. Щелкните рабочую область, которая имеет журналов оповещений.

    1. В разделе **Общие** меню, щелкните **журналы**

    1. Щелкните рядом с полем глаза **SQLAdvancedThreatProtection** таблицы. Журналы перечисляются.

     ![Просмотр журналов](./media/security-center-advanced-iaas-data/view-logs.png)

## <a name="set-up-email-notification-for-atp-alerts"></a>Настройка уведомлений по электронной почте для оповещения ATP 

Можно задать список получателей, чтобы получать уведомление по электронной почте при создании оповещения ASC. Сообщение содержит прямую ссылку на оповещение в центре безопасности Azure можно с помощью все необходимые сведения. 

1. Перейдите к **центр безопасности** > **политики безопасности** и в строке щелкните соответствующую подписку **изменение параметров >**.

    ![Параметры подписки](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. Из **параметры** меню **уведомления по электронной почте**. 
1. В **адрес электронной почты** текста введите адреса электронной почты для получения уведомлений. Можно ввести несколько адресов электронной почты, разделяя точкой с запятой (,) адреса электронной почты.  Например admin1@mycompany.com,admin2@mycompany.com,admin3@mycompany.com

      ![Параметры электронной почты](./media/security-center-advanced-iaas-data/email-settings.png)

1. В **уведомление по электронной почте** параметры, задать следующие параметры:
  
    * **Отправить уведомление по электронной почте для оповещений высокого уровня серьезности**: Вместо отправки сообщений электронной почты для всех предупреждений, отправьте только для оповещений высокого уровня серьезности.
    * **Также отправлять уведомления по электронной почте владельцам подписок**:  Отправляйте уведомления слишком владельцам подписок.

1. В верхней части **уведомления по электронной почте** щелкните **Сохранить**.

  > [!NOTE]
  > Необходимо нажать кнопку **Сохранить** перед закрытием окна, или на новую **уведомление по электронной почте** параметры не будут сохранены.

## <a name="explore-vulnerability-assessment-reports"></a>Просмотр отчетов с оценкой уязвимостей

Результаты оценки Обзор панели мониторинга оценки уязвимостей во всех базах данных. Можно просмотреть распределение баз данных в соответствии с версии SQL Server, а также сводку сбоя и передача баз данных и общую сводку непройденных проверок в соответствии с распределением риска.

Можно просмотреть свои результаты оценки уязвимостей и отчеты непосредственно из Log Analytics.

1. Перейдите в рабочую область Log Analytics с помощью решения ADS.
1. Перейдите к **решения** и выберите **оценка уязвимостей SQL** решения.
1. В **Сводка** панели щелкните **Просмотр сводки** и выберите ваш **отчет об оценке уязвимостей SQL**.

    ![Отчет об оценке SQL](./media/security-center-advanced-iaas-data/ads-sql-server-1.png)

    Загружает панели мониторинга отчетов. Убедитесь, что временное окно равно по крайней **последние 7 дней** с момента выполнения сканирования для оценки уязвимостей на базу данных по фиксированному расписанию один раз в 7 дней.

    ![Установите последние 7 дней](./media/security-center-advanced-iaas-data/ads-sql-server-2.png)

1. Чтобы детализацию для получения дополнительных сведений, щелкните любой из элементов панели мониторинга. Например:

   1. Щелкните флажок уязвимость **Failed проверяет Сводка** раздел, чтобы просмотреть таблицу Log Analytics с результатами для такой проверки по всем базам данных. Которые имеют результаты отображаются первыми.

   1. Затем щелкните для просмотра сведений о каждой уязвимости, включая описание уязвимости и влияние, состояние, связанные риски и фактические результаты в этой базе данных. Также вы увидите фактического запроса, выполненного для выполнения этой проверки, а также сведения об исправлении для устранения этой уязвимости.

    ![Выберите рабочую область](./media/security-center-advanced-iaas-data/ads-sql-server-3.png)

    ![Выберите рабочую область](./media/security-center-advanced-iaas-data/ads-sql-server-4.png)

1. Любые запросы Log Analytics можно выполнять к данным результаты оценки уязвимостей, чтобы продольные и поперечные срезы данных в соответствии с потребностями.

## <a name="advanced-threat-protection-for-sql-servers-on-iaas-alerts"></a>Advanced Threat Protection для серверов SQL Server на IaaS оповещения
Оповещения создаются на нестандартные и потенциально вредоносные попытки получить доступ к или воспользоваться серверы SQL Server. Эти события могут активировать создание следующих оповещений.

### <a name="anomalous-access-pattern-alerts"></a>Оповещения об аномальных доступа шаблон

* **Доступ из незнакомого расположения:** Это предупреждение появляется при изменении способа доступа к серверу SQL, если пользователь вошел на сервер SQL из необычного географического расположения. Возможные причины:
     * Злоумышленник или бывшая вредоносных используют получил доступ к серверу SQL Server.
     * Пользователь доступ к серверу SQL из нового расположения.
* **Доступ из потенциально опасного приложения**: его предупреждение активируется при потенциально опасного приложения используется для доступа к базе данных. Возможные причины:
     * Злоумышленники попытаются нарушения безопасности с помощью обычных средств SQL.
     * Допустимые на проникновение.
* **Доступ из незнакомого субъекта**. Это предупреждение появляется при изменении способа доступа к серверу SQL, если пользователь вошел на сервер SQL с помощью необычного субъекта (пользователя SQL). Возможные причины:
     * Злоумышленник или бывшая вредоносных используют получил доступ к серверу SQL Server. 
     * Пользователь для доступа к SQL Server из нового участника.
* **Подбор учетных данных SQL.** Это предупреждение появляется при большом количестве неудачных попыток входа с разными учетными данными. Возможные причины:
     * Злоумышленники попытаются уведомлений о нарушении SQL методом подбора.
     * Допустимые на проникновение.

### <a name="potential-sql-injection-attacks-coming"></a>Потенциальных атак путем внедрения кода SQL (ожидается)

* **Уязвимость к атакам путем внедрения кода SQL.** Это предупреждение появляется, если приложение создает в базе данных инструкцию SQL с ошибкой. Оно может указывать на возможную уязвимость к атакам путем внедрения кода SQL. Возможные причины:
     * Дефект в коде приложения, который создает инструкцию SQL с ошибкой.
     * Код приложения или хранимые процедуры не очищают ввод данных пользователя при создании ошибочной инструкции SQL, которая может быть использована для внедрения кода SQL.
* **Потенциальная атака путем внедрения кода SQL.** Это предупреждение появляется при обнаружении активного эксплойта определенной уязвимости приложения к внедрению кода SQL. Это означает, что злоумышленник пытается внедрить вредоносные инструкции SQL с помощью кода уязвимого приложения или хранимых процедур.
