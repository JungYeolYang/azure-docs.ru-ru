---
title: Наблюдение за доступностью и скоростью реагирования веб-сайта | Документация Майкрософт
description: Настройка веб-тестов в Application Insights. Получение оповещений, когда веб-сайт становится недоступным или медленно реагирует на запросы.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/22/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: 6cd5413d64be2117cc5f64202ecdaaf40f35db4b
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205378"
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Наблюдение за доступностью и скоростью реагирования веб-сайта
Развернув веб-приложение или веб-сайт на любом сервере, вы можете настроить тесты для наблюдения за его доступностью и скоростью реагирования. [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) отправляет веб-запросы через одинаковые промежутки времени из разных точек по всему миру. Эта надстройка предупреждает вас, если приложение реагирует медленно или не реагирует вообще.

Вы можете настроить тесты доступности для любой конечной точки HTTP или HTTPS, доступной из Интернета. На тестируемый веб-сайт не нужно ничего добавлять. Этот сайт даже может принадлежать кому-то другому. Вы можете протестировать службу REST API, от которой зависит ваш сайт.

Существует два вида тестов доступности:

* [Тест проверки связи с URL-адресом](#create)– простой тест, который вы можете создать на портале Azure.
* [Многошаговый веб-тест](#multi-step-web-tests) — тест, который вы создаете в Visual Studio Enterprise и загружаете на портал.

Для одного ресурса приложения можно создать не более 100 тестов доступности.


## <a name="create"></a>Открытие ресурса для отчетов по тестам доступности

**Если вы уже настроили Application Insights** для веб-приложения, откройте ресурс Application Insights на [портале Azure](https://portal.azure.com).

**Чтобы увидеть отчеты в новом ресурсе**, перейдите на [портал Azure](https://portal.azure.com) и создайте ресурс Application Insights.

!["Создать ресурс" > "Инструменты разработчика" > Application Insights](./media/monitor-web-app-availability/1create-resource-appinsights.png)

Щелкните **Все ресурсы** , чтобы открыть колонку обзора для нового ресурса.

## <a name="setup"></a>Создание теста проверки связи с URL-адресом
Откройте колонку "Доступность" и добавьте тест.

![Укажите хотя бы URL-адрес своего веб-сайта](./media/monitor-web-app-availability/2addtest-url.png)

* Вы можете указать **URL-адрес** любой веб-страницы, которую требуется протестировать, но он должен быть видимым из общедоступного Интернета. URL-адрес может содержать строку запроса, поэтому вы, например, сможете немного поупражняться в работе с базой данных. Если URL-адрес указывает на перенаправление, мы будем переходить по нему до 10 раз.
* **Анализировать зависимые запросы**. Если этот флажок установлен, изображения, сценарии, файлы стилей и другие файлы, являющиеся частью тестируемых веб-страниц, запрашиваются для теста. Записанное время ответа включает время, затраченное на получение этих файлов. Тест завершается ошибкой, если эти ресурсы не удается загрузить в течение времени ожидания, актуального для всего теста. Если этот флажок не установлен, то тест запросит только файл по указанному URL-адресу.

* **Enable retries** (Разрешить повторные попытки).  Если этот флажок установлен, то при неудачном завершении тест будет повторяться через короткие интервалы. Сообщение об ошибке отобразится только после трех неудачных попыток подряд. Последующие тесты будут выполняться с обычной частотой. Повторные попытки будут временно приостановлены до следующей успешной попытки. Это правило действует в любом расположении тестирования. Этот вариант является рекомендуемым. В среднем около 80 % неудачных попыток решаются при повторной попытке.

* **Периодичность проведения тестирования.** Задает частоту выполнения теста для всех тестовых расположений. При стандартной частоте в пять минут и с пятью тестовыми расположениями ваш сайт будет проверяться в среднем каждую минуту.

* **Расположения тестирования** – это места, из которых наши серверы отправляют веб-запросы на ваш URL-адрес. Чтобы убедиться, что вы можете различать проблемы веб-сайта и сетевые проблемы, минимальное количество рекомендуемых расположений тестирования равно пяти. Вы можете выбрать до 16 таких расположений.

> [!NOTE]
> * Мы настоятельно рекомендуем тестирования из нескольких расположений, как минимум пяти. Это позволяет предотвратить ложные предупреждения, которые могут возникнуть в результате временных проблем с определенным местоположением. Помимо этого мы обнаружили, что оптимальная конфигурация — это количество тестовых местоположений, равное пороговому значению расположения оповещения + 2. 
> * Включение параметра "Разбирать зависимые запросы" делает проверку более строгой. Тест может завершиться ошибкой в тех случаях, которые могут быть незаметны, если вручную просматривать сайт.

* **Критерии успешного завершения**:

    **Время ожидания тестирования**. Уменьшите значение этого параметра, чтобы получать оповещения о медленных откликах. Тест считается неудачной попыткой, если ответы от сайта не были получены в течение заданного периода. Если выбрать параметр **Анализировать зависимые запросы**, все изображения, файлы стилей, скрипты и другие зависимые ресурсы будут получены в течение этого периода.

    **HTTP-ответ.** Возвращаемый код состояния, который считается успешным результатом. Код 200 указывает на возврат нормальной веб-страницы.

    **Совпадение содержимого**: строка, например «Добро пожаловать!». Проверим наличие точного совпадения (с учетом регистра) в каждом ответе. Это должна быть строка обычного текста без подстановочных знаков. Не забывайте, что если контент страницы изменяется, необходимо обновить эту строку. **В настоящее время поддерживаются только символы латинского алфавита совпадения содержимого.** 

* **Пороговое значение для расположения оповещения.** Мы рекомендуем как минимум 3 из 5 расположений. Оптимальное отношение между пороговым значением предупреждения расположения и числом тестовых местоположений: **пороговое значение для предупреждения расположения** = **количество расположений тестирования** —2 с как минимум пятью расположениями тестирования.

## <a name="multi-step-web-tests"></a>Многошаговые веб-тесты
Вы можете отслеживать сценарий, который содержит последовательность URL-адресов. Например, в случае наблюдения за интернет-магазином вы можете проверить, что добавление товаров в корзину работает исправно.

> [!NOTE]
> За многошаговые веб-тесты взимается плата. См. [таблицу расценок](https://azure.microsoft.com/pricing/details/application-insights/).
> 

Для создания многошагового теста вам сначала нужно записать сценарий с помощью Visual Studio Enterprise, а затем отправить запись в Application Insights. Application Insights периодически воспроизводит сценарий и проверяет ответы.

> [!NOTE]
> * В тестах нельзя использовать запрограммированные функции или циклы. Тест должен полностью находиться в WEBTEST-файле скрипта. Однако вы можете использовать стандартные подключаемые модули.
> * В многошаговых веб-тестах поддерживаются только символы английского алфавита. Если вы используете Visual Studio на других языках, обновите файл определения веб-теста, чтобы перевести или исключить символы языков, отличных от английского.
>

#### <a name="1-record-a-scenario"></a>1. Запись сценария
Для записи веб-сеанса используйте Visual Studio Enterprise.

1. Создайте проект веб-теста производительности.

    ![В Visual Studio Enterprise создайте новый проект на основе шаблона с веб-тестами производительности и нагрузочными тестами.](./media/monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Отсутствует шаблон с веб-тестами производительности и нагрузочными тестами?* Закройте Visual Studio Enterprise. Откройте **установщик Visual Studio**, чтобы изменить установленные компоненты Visual Studio Enterprise. В разделе **Отдельные компоненты** выберите **Средства для тестирования производительности веб-сайтов и нагрузочного тестирования**.

2. Откройте WEBTEST-файл и начните запись.

    ![Откройте WEBTEST-файл и щелкните "Запись".](./media/monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. Выполните действия пользователя, которые нужно смоделировать в тесте: откройте веб-сайт, добавьте продукт в корзину и т. д. Затем остановите тест.

    ![Запись веб-теста выполняется в Internet Explorer.](./media/monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Не делайте слишком длинный сценарий. Ограничение — 100 шагов и 2 минуты.
4. Отредактируйте тест, чтобы:

   * Добавить проверки полученного текста и кодов ответов.
   * Удалить все лишние взаимодействия. Вы также можете удалить связанные запросы изображений, запросы к рекламным сайтам или сайтам отслеживания.

     Помните, что редактировать можно только тестовый сценарий — добавлять собственный код и вызывать другие веб-тесты нельзя. Не вставляйте в тест циклы. Вы можете использовать стандартные подключаемые модули для веб-тестов.
5. Запустите тест в Visual Studio и убедитесь, что он работает.

    Средство выполнения веб-тестов откроет веб-браузер и повторит записанные действия. Убедитесь, что тест работает правильно.

    ![В Visual Studio откройте WEBTEST-файл и щелкните "Выполнить".](./media/monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-the-web-test-to-application-insights"></a>2. Загрузка веб-теста в Application Insights
1. На портале Application Insights в колонке "Доступность" щелкните "Добавить тест".

    ![В колонке "Доступность" выберите "Добавить тест".](./media/monitor-web-app-availability/3addtest-web.png)
2. Выберите многошаговый тест и загрузите WEBTEST-файл.

    Установите для тестовых местоположений, частоты и параметров оповещения те же значения, что и для проверок связи.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Вставка времени и случайных чисел в многошаговый тест
Предположим, что вы тестируете инструмент, который получает зависящие от времени данные (например, запасы) от внешнего источника. При записи веб-теста необходимо использовать определенные значения времени, но они должны быть заданы как параметры теста StartTime и EndTime.

![Веб-тест с параметрами.](./media/monitor-web-app-availability/appinsights-72webtest-parameters.png)

При запуске теста параметру EndTime следует всегда присваивать текущее время, а параметру StartTime – время за 15 минут до текущего.

Подключаемые модули веб-теста позволяют параметризовать время.

1. Добавьте подключаемые модули веб-теста для всех необходимых значений переменных параметров. На панели инструментов веб-теста выберите **Добавить подключаемый модуль веб-теста**.

    ![Щелкните "Добавить подключаемый модуль веб-теста" и выберите тип.](./media/monitor-web-app-availability/appinsights-72webtest-plugins.png)

    В этом примере используется два экземпляра подключаемого модуля даты и времени. Для первого экземпляра будет задано время за 15 минут до текущего, а для второго — текущее время.
2. Откройте свойства каждого подключаемого модуля. Присвойте ему имя и настройте его для использования текущего времени. Для одного из экземпляров задайте для параметра "Добавление минут" значение "-15".

    ![Задайте имя, выберите "Использовать текущее время" и "Добавить минуты".](./media/monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. В параметрах веб-теста используйте {{имя подключаемого модуля}} для ссылки на имя подключаемого модуля.

    ![В параметрах теста используйте {{имя подключаемого модуля}}.](./media/monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Теперь можно передать тест на портал. Он использует динамические значения при каждом тестировании.


## <a name="monitor"></a>Просмотр результатов теста доступности

На вкладке "Обзор" отображается доля успешно выполненных тестов, а на вкладке "Сведения" — точечная диаграмма и сетка с подробными сведениями.

Через несколько минут щелкните **Обновить**, чтобы просмотреть результаты теста.

![Точечная диаграмма на вкладке "Сведения"](./media/monitor-web-app-availability/4refresh.png)

На точечной диаграмме отобразятся примеры результатов теста со сведениями о этапах диагностического теста. Обработчик тестов хранит сведения о диагностике тестов, которые завершились сбоем. Диагностические сведения об успешно выполненных тестах хранятся для подмножества выполнений. Наведите указатель мыши на любые красные или зеленые точки, чтобы просмотреть метку времени теста, длительность теста, местоположение и его имя. Щелкните любую точку на точечной диаграмме, чтобы просмотреть сведения о результатах теста.  

Выберите конкретный тест, расположение или сократите период времени, чтобы просмотреть дополнительные результаты для нужного периода времени. Используйте обозреватель поиска, чтобы просмотреть результаты всех тестов или воспользуйтесь запросами аналитики, чтобы запустить пользовательские отчеты для этих данных.

Помимо необработанных результатов в обозревателе метрик имеются две метрики доступности: 

1. "Доступность": количество успешно выполненных тестов (в процентах). 
2. "Продолжительность теста": средняя длительность выполнения тестов.

Вы можете применить фильтры по имени теста или расположению, чтобы проанализировать тенденции конкретного теста и/или местоположения.

## <a name="edit"></a> Изменение и проверка тестов

На вкладке "Сведения" для конкретного теста щелкните многоточие справа, чтобы изменить, временно отключить, удалить или скачать веб-тест. Может занять до 20 минут распространить изменения конфигурации.

Выберите **Просмотр сведений о тесте** для конкретного теста, чтобы просмотреть точечную диаграмму и сведения о расположении для этого теста.

![Просмотр сведений о тесте, изменение и отключение веб-теста](./media/monitor-web-app-availability/5viewdetails.png)

При обслуживании службы может потребоваться отключить тесты доступности или правила оповещения, связанные с ними.

![Удаление веб-теста](./media/monitor-web-app-availability/6disable.png)
![Изменение теста](./media/monitor-web-app-availability/8edittest.png)

## <a name="failures"></a>При возникновении сбоев
Щелкните красную точку.

![Щелкните красную точку.](./media/monitor-web-app-availability/open-instance-3.png)

С помощью результатов тестов доступности вы увидите сведения о транзакциях для всех компонентов. Здесь можно:

* изучить ответ, полученный от сервера;
* диагностировать сбой на основе коррелированной телеметрии на стороне сервера, собранной во время обработки теста доступности, завершившегося сбоем;
* добавить в журнал проблему или рабочий элемент в Git или Azure Boards для отслеживания проблемы; ошибка будет содержать ссылку на это событие;
* открыть результат веб-теста в Visual Studio.

См. дополнительные сведения об [интерфейсе сквозной диагностики транзакций](../../azure-monitor/app/transaction-diagnostics.md).

Щелкните строку исключения, чтобы просмотреть подробности исключения на стороне сервера, вызвавшего сбой искусственного теста доступности. Можно также получить [отладочный моментальный снимок](../../azure-monitor/app/snapshot-debugger.md) для более широкой диагностики на уровне кода.

![Диагностика на стороне сервера](./media/monitor-web-app-availability/open-instance-4.png)

## <a name="alerts"></a> Оповещения доступности
Существует следующие типы правил генерации оповещений для данных о доступности с помощью классического интерфейса оповещений:
1. X из Y расположений сообщают о сбоях за период времени;
2. агрегированный процент доступности опускается ниже порогового значения;
3. средняя длительность теста превышает пороговое значение.

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Оповещение о том, что X из Y расположений сообщают о сбоях
Правило генерации оповещений о том, что X из Y расположений сообщают о сбоях, включено по умолчанию в [новом унифицированном интерфейсе оповещений](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) при создании нового теста доступности. Вы можете отказаться, выбрав "классический" вариант или отключив правило генерации оповещений.

![Интерфейс создания](./media/monitor-web-app-availability/appinsights-71webtestUpload.png)

> [!NOTE]
>  В [новых унифицированных оповещениях](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) настройки серьезности и уведомлений для правила генерации оповещений с помощью [группы действий](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) **должны** настраиваться в интерфейсе оповещений. Без следующих действий вы будете получать уведомления только на портале.

1. Сохранив тест доступности, щелкните многоточие на вкладке "Сведения" для созданного теста. Выберите "Изменить оповещение".
![Изменить после сохранения](./media/monitor-web-app-availability/9editalert.png)

2. Задайте требуемый уровень серьезности, описание правила и самое важное — группу действий, настройки уведомлений которой вы хотите использовать для этого правила генерации оповещений.
![Изменить после сохранения](./media/monitor-web-app-availability/10editalert.png)


> [!NOTE]
> * Настройте группы действий, чтобы получать уведомления при активации оповещения, выполнив описанные выше действия. Без этого шага вы будете получать при активации правила только уведомления на портале.
>

### <a name="alert-on-availability-metrics"></a>Оповещения на основе метрик доступности
С помощью [новых унифицированных оповещений](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) можно создавать оповещения о доступности сегментированных агрегированных метрик доступности и длительности теста:

1. Выберите ресурс Application Insights в интерфейсе метрик и укажите метрику доступности:  ![Выбор метрики доступности](./media/monitor-web-app-availability/selectmetric.png)

2. Команда настройки оповещений в меню перенаправит вас на новый интерфейс, где можно выбрать определенные тесты или расположения, чтобы настроить правило генерации оповещений. Здесь можно также настроить группы действий для этого правила.
    ![Настройка оповещений доступности](./media/monitor-web-app-availability/availabilitymetricalert.png)

### <a name="alert-on-custom-analytics-queries"></a>Оповещение о настраиваемых аналитических запросах
С помощью [новых унифицированных оповещений](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) вы можете создать оповещение [о настраиваемых запросах к журналу](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log). С помощью настраиваемых запросов вы можете создать оповещение с любым произвольным условием, которое поможет вам получить самый надежный сигнал о проблемах с доступностью. Это особенно применимо при отправке настраиваемых результатов доступности с помощью пакета TrackAvailability SDK. 

> [!Tip]
> * К метрикам доступности данных относятся все настраиваемые результаты доступности, которые вы может отправлять путем вызова TrackAvailability SDK. Поддержку оповещений о метриках можно использовать для оповещения о доступности результатов.
>

## <a name="dealing-with-sign-in"></a>Работа с входом
Если пользователи входят в приложение, вы можете протестировать страницы входа, используя несколько способов имитации входа. Выбор подхода зависит от типа безопасности в приложении.

Во всех случаях учетную запись в приложении следует создавать только для целей тестирования. По возможности ограничьте разрешения для этой тестовой учетной записи, чтобы веб-тесты не доставляли неудобств реальным пользователям.

### <a name="simple-username-and-password"></a>Простое имя пользователя и пароль
Запишите веб-тест обычным образом. Сначала удалите файлы cookie.

### <a name="saml-authentication"></a>Проверка подлинности SAML
Используйте подключаемый модуль SAML, доступный для веб-тестов.

### <a name="client-secret"></a>Секрет клиента
Если в приложении предусмотрен маршрут входа с использованием секрета клиента, используйте этот маршрут. Azure Active Directory (AAD) — это пример службы, в которой доступен вход в систему с использованием секрета клиента. В AAD секрет клиента — это ключ приложения.

Ниже приведен пример веб-теста веб-приложения Azure с использованием ключа приложения.

![Пример секрета клиента](./media/monitor-web-app-availability/110.png)

1. Получите токен из Azure AD с помощью секрета клиента (AppKey).
2. Извлеките токен носителя из ответа.
3. Вызовите интерфейс API, используя токен носителя в заголовке авторизации.

Убедитесь, что веб-тест является фактическим клиентом, т. е. у него есть собственное приложение в AAD, и используйте его значения clientId и appkey. У тестируемой службы также есть собственное приложение в AAD: универсальный код ресурса URI appID этого приложения содержится в веб-тесте в поле resource.

### <a name="open-authentication"></a>Открытая проверка подлинности
Пример открытой проверки подлинности — это вход с помощью учетной записи Майкрософт или Google. Многие приложения, использующие OAuth, предоставляют альтернативный вариант с использованием секрета клиента, поэтому сначала стоит попробовать эту возможность.

Если в вашем тесте должен выполняться вход с использованием OAuth, общий порядок действий таков:

* Проверьте трафик между веб-браузером, сайтом проверки подлинности и приложением при помощи Fiddler или аналогичного средства.
* Выполните вход несколько раз с разных компьютеров или из разных браузеров либо через большие промежутки времени (чтобы истек срок действия маркеров).
* Сравнивая различные сеансы, определите маркер, возвращаемый с сайта проверки подлинности, который затем передается на сервер приложения после входа.
* Запишите веб-тест с помощью Visual Studio.
* Параметризуйте маркеры, задав параметр при возврате маркера из структуры проверки подлинности и использовав его в запросе к сайту.
  (Visual Studio попытается параметризовать тест, но не сможет правильно параметризовать маркеры).

## <a name="performance-tests"></a>Тесты производительности
> [!NOTE]  
> Облачные службы нагрузочного тестирования является устаревшим. Дополнительные сведения об устаревании, доступность службы и альтернативных служб можно найти [здесь](https://docs.microsoft.com/azure/devops/test/load-test/overview?view=azure-devops).

Вы можете выполнить на своем веб-сайте нагрузочный тест. Как и при проверке доступности, вы можете отправлять простые или многошаговые запросы из точек по всему миру. В отличие от теста доступности, отправляется много запросов, что имитирует несколько одновременно работающих пользователей.

В разделе **Настройка** перейдите в раздел **Тестирование производительности** и щелкните "Создать", чтобы создать тест.

![Создание теста производительности](./media/monitor-web-app-availability/11new-performance-test.png)

По завершении теста на экране отобразится время ответа и число успешных попыток.

![Результаты теста производительности](./media/monitor-web-app-availability/12performance-test.png)

> [!TIP]
> Чтобы увидеть результаты теста производительности, используйте [Live Stream](../../azure-monitor/app/live-stream.md) и [профилировщик](../../azure-monitor/app/profiler.md).
>

## <a name="automation"></a>Служба автоматизации
* [Используйте сценарии PowerShell, чтобы настройка теста доступности](../../azure-monitor/app/powershell.md#add-an-availability-test) выполнялась автоматически.
* Настройте вызов [webhook](../../azure-monitor/platform/alerts-webhooks.md), активируемый оповещением.

## <a name="qna"></a> Часто задаваемые вопросы

* *Сайт выглядит правильно, но возникают сбои тестирования. Почему Application Insights оповещает меня?*

    * Включен ли у вашего теста параметр "Анализировать зависимые запросы"? Он приводит к строгой проверке таких ресурсов, как скрипты, изображения и т. д. Эти типы сбоев могут быть незаметны в браузере. Проверьте все изображения, скрипты, таблицы стилей и любые другие файлы, загружаемые на страницу. Если не удается загрузить какой-либо компонент, отчет о тесте выдаст ошибку, даже если главная HTML-страница загружается правильно. Чтобы тестирование не зависело от таких сбоев ресурсов, просто снимите флажок "Разбирать зависимые запросы" в конфигурации тестирования. 

    * Чтобы уменьшить вероятность возникновения помех из-за нестабильной работы сети и т. д., установите параметр "Enable retries for test failures" (Выполнять повторные попытки при сбое тестирования). Вы также можете выполнять тестирование из любого расположения и соответствующим образом управлять пороговым значением правила генерации оповещений для предотвращения проблем с конкретными расположениями, вызывающих ненужные оповещения.

    * Щелкните любую из красных точек в интерфейсе доступности или любой сбой доступности в проводнике поиска для просмотра сведений о том, почему мы сообщили о сбое. Результат теста, а также коррелированные данные телеметрии на стороне сервера (если они включены) должны помочь понять, почему тест завершился ошибкой. Распространенные причины временных неполадок — проблемы сети или подключения. 

    * Истекло ли время ожидания теста? Мы прерываем тесты через две минуты. Если ваша проверка связи или многошаговый тест занимает более двух минут, мы воспринимаем это как сбой. Разбейте тест на несколько сеансов, которые можно выполнить последовательно.

    * Все ли расположения сообщили о сбоях? Если только некоторые их них прислали отчет о сбое, возможно, причина в проблемах с сетью или CDN. Щелкая красные точки, вы можете понять, почему расположение сообщило о сбое.

* *Мне не пришло сообщение электронной почты при активации или разрешении оповещения.*

    Проверьте конфигурацию классических оповещений, чтобы убедиться, что ваш адрес электронной почты явно указан, или список рассылки, в который вы входите, настроен для получения уведомлений. Если это так, проверьте конфигурацию списка рассылки, чтобы убедиться, что он может получать внешние сообщения электронной почты. Проверьте также, не настроил ли администратор электронной почты политики, которые могут вызвать эту проблему.

* *Я не получаю уведомления веб-перехватчика.*

    Убедитесь, что приложение, получающее уведомления от веб-перехватчика, доступно и успешно обрабатывает запросы веб-перехватчика. Дополнительную информацию см. [здесь](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook).

* *Периодические сбои тестирования и ошибка с уведомлением о нарушении протокола.*

    Ошибка ("нарушение протокола...За возвратом каретки должен следовать перевод строки") свидетельствует о возможной неполадке с сервером (или зависимостями). Такое происходит, когда в ответе задаются неправильно сформированные заголовки. Это может быть вызвано подсистемами балансировки нагрузки или сетями доставки содержимого (CDN). В частности, некоторые заголовки могут не использовать CRLF для указания конца строки, что нарушает спецификацию протокола HTTP и, следовательно, приводит к сбою во время проверки на уровне WebRequest .NET. Проверьте, есть ли в ответе заголовки, содержащие нарушение.
    
    Примечание. При переходе по URL-адресу в браузерах, для которых установлена упрощенная проверка заголовков HTTP, может не произойти сбой. См. подробное описание проблемы в этой записи блога: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  
    
* *Я не вижу данные телеметрии на стороне связанного сервера и не могу диагностировать сбои тестирований.*
    
    Если служба Application Insights настроена для серверного приложения, возможно, это произошло из-за [выборки](../../azure-monitor/app/sampling.md) в операции. Выберите другой результат доступности.

* *Можно ли вызывать код из моего веб-теста?*

    № Действия теста должны находиться в WEBTEST-файле. Кроме того, нельзя вызывать другие веб-тесты и использовать циклы. Но есть несколько подключаемых модулей, которые могут оказаться полезными.

* *Поддерживается ли протокол HTTPS?*

    Мы поддерживаем TLS 1.1 и TLS 1.2. Мы сейчас не проверять ошибки сертификата HTTPS.  

* *Есть ли разница между понятиями "веб-тесты" и "тесты доступности"?*

    Эти два термина могут быть взаимозаменяемыми. Тест доступности является более универсальным термином, так как кроме многошаговых веб-тестов вам доступны проверка связи с отдельным URL-адресом.
    
* *Мне нужно использовать тесты доступности на нашем внутреннем сервере, который работает за брандмауэром.*

    Есть два возможных решения:
    
    * Разрешите в брандмауэре входящие запросы с [IP-адресов агентов веб-тестирования](../../azure-monitor/app/ip-addresses.md).
    * Напишите собственный код для периодической проверки внутреннего сервера. Запустите код в виде фонового процесса на тестовом сервере за брандмауэром. Процесс тестирования может отправлять результаты в Application Insights с помощью API [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) в основном пакете SDK. Для этого тестовый сервер должен иметь исходящий доступ к конечной точке приема в Application Insights, что представляет гораздо меньшую угрозу безопасности, чем разрешение входящих запросов. Результаты не будут отображаться в колонках веб-тестов доступности, но будут отображаться как результаты в обозревателе метрик, поиска и анализа.

* *Сбой отправки многошагового веб-теста.*

    Некоторые причины, по которым он может произойти:
    * Максимальный размер — 300 000.
    * Циклы не поддерживаются.
    * Ссылки на другие веб-тесты не поддерживаются.
    * Источники данных не поддерживаются.

* *Мой многошаговый тест не выполняется.*

    Максимальное количество запросов в тесте — 100. Кроме того, тест будет остановлен, если он выполняется дольше двух минут.

* *Как запустить тест с клиентскими сертификатами?*

    К сожалению, эта возможность не поддерживается.

## <a name="who-receives-the-classic-alert-notifications"></a>Кто получает (классические) оповещения?

Этот раздел относится только к классическим оповещениям, он поможет вам оптимизировать уведомления так, чтобы они приходили только желаемым получателям. Чтобы узнать больше о различиях между [классическими оповещениями](../platform/alerts-classic.overview.md) и новыми возможностями оповещений, ознакомьтесь с [обзорной статьей об оповещениях](../platform/alerts-overview.md). Чтобы управлять оповещениями в новом интерфейсе оповещений, используйте [группы действий](../platform/action-groups.md).

* Мы рекомендуем использовать определенных получателей для классических оповещений.

* Если флажок **bulk/group** (массовый/групповой) включен, оповещения о сбоях из X из Y расположений будут оправляться пользователям с ролями администратора или соадминистратора.  По сути, _все_ администраторы _подписки_ будут получать уведомления.

* Если флажок **bulk/group** (массовый/групповой) включен, оповещения на основе метрик доступности (или всех соответствующих метрик Application Insights) будут отправляться пользователям с ролями владельца, участника или читателя в подписке. Фактически, _все_ пользователи, имеющие доступ к подписке Application Insights, находятся в области действия и будут получать уведомления. 

> [!NOTE]
> При отключении используемого флажка **bulk/group** (массовый/груповой) вы не сможете отменить изменение.

Используйте новые возможности оповещений и оповещения почти в реальном времени, если вам нужно уведомлять пользователей в зависимости от их ролей. С помощью [групп действий](../platform/action-groups.md) вы можете настроить уведомления по электронной почте для пользователей с любой из ролей — участника, владельца или читателя (не объединяемых вместе как один параметр).



## <a name="next"></a>Дальнейшие действия
[Поиск по журналам диагностики][diagnostic]

[Устранение неполадок][qna]

[IP-адреса веб-агентов тестирования](../../azure-monitor/app/ip-addresses.md)

<!--Link references-->

[azure-availability]: ../../insights-create-web-tests.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[start]: ../../azure-monitor/app/app-insights-overview.md
