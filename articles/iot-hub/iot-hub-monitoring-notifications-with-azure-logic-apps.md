---
title: Удаленный мониторинг и отправка уведомлений в Центре Интернета вещей с помощью Azure Logic Apps | Документация Майкрософт
description: Azure Logic Apps можно использовать для мониторинга температуры в Центре Интернета вещей и автоматической отправки уведомлений на электронную почту в случае обнаружения аномалий.
author: robinsh
keywords: мониторинг Центра Интернета вещей, уведомления Центра Интернета вещей, мониторинг температуры в Центре Интернета вещей
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/19/2019
ms.author: robinsh
ms.openlocfilehash: 26637468f44e12f7ad66f907e0f6be3d907e578f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126281"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>Удаленный мониторинг и отправка уведомлений в Центре Интернета вещей с помощью службы Azure Logic Apps, обеспечивающей подключение между Центром Интернета вещей и почтовым ящиком

![Комплексная схема](media/iot-hub-monitoring-notifications-with-azure-logic-apps/iot-hub-e2e-logic-apps.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Служба Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) помогает координировать процессы в локальных и облачных служб, один или несколько предприятий и по различным протоколам. Приложение логики запускается триггером, который затем следуют одно или несколько действий, которые можно упорядочивать, с помощью встроенных элементов управления, таких как условия и итераторы. Благодаря этой гибкости приложений логики идеальное решение Интернета вещей для мониторинга сценариев Интернета вещей. Например прибытии данных телеметрии с устройства в конечной точке центра Интернета вещей может инициировать рабочих процессов приложения логики для хранилища данных в большом двоичном объекте службы хранилища Azure, отправить по электронной почте, чтобы предупреждать аномальных данных, планировать посещение Технический специалист в том случае, если устройство сообщает о сбое , и т. д.

## <a name="what-you-learn"></a>Что вы узнаете

Вы научитесь создавать приложение логики, обеспечивающее подключение между Центром Интернета вещей и почтовым ящиком для мониторинга температуры и отправки уведомлений.

Код клиента, запущенного на устройстве задает свойства приложения, `temperatureAlert`на каждый телеметрии сообщений, оно отправляет в центр Интернета вещей. Когда клиентский код обнаруживает температуры выше 30 ° C, он устанавливает для данного свойства `true`; в противном случае он устанавливает свойство `false`.

В этом разделе можно настроить маршрутизацию в центре Интернета вещей для отправки сообщений, в котором `temperatureAlert = true` для служебной шины конечной точки и настройте приложение логики, которое запускает на сообщения, поступающие в конечную точку Service Bus и отправит вам уведомление по электронной почте.

## <a name="what-you-do"></a>Что нужно сделать

* Создание пространства имен служебной шины и добавьте его в очередь служебной шины.
* Добавьте настраиваемую конечную точку и правило маршрутизации в центр Интернета вещей для маршрутизации сообщений, содержащих предупреждение о температуре в очередь служебной шины.
* Создание, Настройка и проверка приложения логики для получения сообщений из очереди служебной шины и отправки уведомлений по электронной почте требуемой получателю.

## <a name="what-you-need"></a>Необходимые элементы

* Завершить [онлайн-симулятор Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md) руководства или одним из руководств устройства; например, [Raspberry Pi с помощью node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Они охватывают следующие требования:

  * Активная подписка Azure.
  * Центр Интернета вещей Azure в подписке;
  * Клиентское приложение, работающее на устройстве, которое отправляет сообщения телеметрии в центр Интернета вещей Azure.

## <a name="create-service-bus-namespace-and-queue"></a>Создание пространства имен служебной шины и очереди

Создайте пространство имен и очередь служебной шины. Далее в этом разделе создайте правило маршрутизации в центр Интернета вещей для прямого сообщения, содержащие служит для очереди служебной шины, где они будут использоваться приложением логики и активировать его для отправки уведомления по электронной почте уведомления о температуре.

### <a name="create-a-service-bus-namespace"></a>Создание пространства имен служебной шины

1. На [портала Azure](https://portal.azure.com/)выберите **+ создать ресурс** > **интеграции** > **служебной шины**.

1. На **создание пространства имен** панели укажите следующие сведения:

   **Имя.** Имя пространства имен служебной шины. Пространство имен должно быть уникальным в Azure.

   **Ценовая категория**. Выберите **основные** из раскрывающегося списка. Базового уровня для данного учебника достаточно.

   **Группа ресурсов.** Выберите ту же группу ресурсов, которую использует центр Интернета вещей.

   **Расположение.** Используйте то же расположение, которое использует Центр Интернета вещей.

   ![Создание пространства имен служебной шины с помощью портала Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1-create-service-bus-namespace-azure-portal.png)

1. Нажмите кнопку **Создать**. Дождитесь завершения, прежде чем перейти к следующему этапу развертывания.

### <a name="add-a-service-bus-queue-to-the-namespace"></a>Добавление очереди служебной шины к пространству имен

1. Откройте пространство имен служебной шины. Самый простой способ получить пространству имен служебной шины, — для выбора **групп ресурсов** из области ресурсов выберите свою группу ресурсов, а затем выберите пространство имен служебной шины в списке ресурсов.

1. На **пространство имен служебной шины** области выберите **+ очередь**.

1. Введите имя для очереди, а затем выберите **создать**. После успешного создания очереди **Создание очереди** область закроется.

   ![Добавление очереди служебной шины на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-queue.png)

1. Вернитесь на **пространство имен служебной шины** панели в разделе **сущностей**выберите **очереди**. Откройте очередь служебной шины из списка, а затем выберите **политики общего доступа** > **+ добавить**.

1. Введите имя для политики, установите флажок **управление**, а затем выберите **создать**.

   ![Добавление политики очереди шины службы на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2-add-service-bus-queue-azure-portal.png)

## <a name="add-a-custom-endpoint-and-routing-rule-to-your-iot-hub"></a>Добавление пользовательской конечной точки и правила маршрутизации в центр Интернета вещей

Добавление пользовательской конечной точки для очереди служебной шины в центр Интернета вещей и создайте правило маршрутизации сообщений для направления сообщений, содержащих предупреждение о температуре, к этой конечной точке, где они будут использоваться при приложения логики. Правило маршрутизации используется запрос маршрутизации `temperatureAlert = "true"`, для пересылки сообщений на основе значений из `temperatureAlert` свойства приложения, задайте в клиентском коде, запущенной на устройстве. Дополнительные сведения см. в разделе [сообщения маршрутизации запросов на основе свойств сообщения](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#message-routing-query-based-on-message-properties).

### <a name="add-a-custom-endpoint"></a>Добавление пользовательской конечной точки

1. Откройте Центр Интернета вещей. Самый простой способ получить в центр Интернета вещей, — для выбора **групп ресурсов** из области ресурсов выберите свою группу ресурсов, а затем выберите центр Интернета вещей в списке ресурсов.

1. В разделе **Messaging**выберите **маршрутизации сообщений**. На **маршрутизации сообщений** области выберите **настраиваемых конечных точек** , а затем — **+ добавить**. В раскрывающемся списке выберите **очереди служебной шины**.

   ![Добавление конечной точки в Центр Интернета вещей на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-iot-hub-custom-endpoint.png)

1. На **добавьте конечную точку службы шины** области введите следующие сведения:

   **Имя конечной точки**: Имя конечной точки.

   **Пространство имен служебной шины**. Выберите созданное пространство имен.

   **Очередь служебной шины**: Выберите созданную очередь.

   ![Добавление конечной точки в Центр Интернета вещей на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3-add-iot-hub-endpoint-azure-portal.png)

1. Нажмите кнопку **Создать**. После успешного создания конечной точки, перейдите к следующему шагу.

### <a name="add-a-routing-rule"></a>Добавление правила маршрутизации

1. Вернитесь на **маршрутизации сообщений** области выберите **маршруты** , а затем — **+ добавить**.

1. На **добавления маршрута** области введите следующие сведения:

   **Имя.** Имя правила маршрутизации.

   **Конечная точка**. Выберите конечную точку, в которой вы создали.

   **Источник данных**: Выберите **сообщений телеметрии устройства**.

   **Маршрутизация запроса.** Укажите `temperatureAlert = "true"`.

   ![Добавление правила маршрутизации на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4-add-routing-rule-azure-portal.png)

1. Щелкните **Сохранить**. Вы можете закрыть **маршрутизации сообщений** области.

## <a name="create-and-configure-a-logic-app"></a>Создание и настройка приложения логики

В предыдущем разделе можно настроить центр Интернета вещей для маршрутизации сообщений, содержащий предупреждение о температуре в очередь служебной шины. Теперь настройкой приложения логики для отслеживания очереди служебной шины и отправлять уведомление по электронной почте при каждом добавлении сообщения в очередь.

### <a name="create-a-logic-app"></a>Создайте приложение логики

1. Выберите **создать ресурс** > **интеграции** > **приложение логики**.

1. Введите следующие сведения:

   **Имя.** Имя приложения логики.

   **Группа ресурсов.** Выберите ту же группу ресурсов, которую использует центр Интернета вещей.

   **Расположение.** Используйте то же расположение, которое использует Центр Интернета вещей.

   ![Создание приложения логики на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-a-logic-app.png)

1. Нажмите кнопку **Создать**.

### <a name="configure-the-logic-app-trigger"></a>Настройка триггера приложения логики

1. Откройте приложение логики. Самый простой способ получить приложение логики, — для выбора **групп ресурсов** из области ресурсов выберите свою группу ресурсов, а затем выберите приложение логики из списка ресурсов. При выборе приложения логики открывается конструктор Logic Apps.

1. В конструкторе приложений логики, прокрутите вниз до раздела **шаблоны** и выберите **пустое приложение логики**.

   ![Начало работы с пустым приложением логики на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5-start-with-blank-logic-app-azure-portal.png)

1. Выберите **все** , а затем — **служебной шины**.

   ![Выбор служебной шины для начала создания приложения логики на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6-select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. В разделе **триггеры**выберите **когда одно или несколько сообщений поступает в очередь (автозавершение)**.

   ![Выберите триггер для приложения логики на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-service-bus-trigger.png)

1. Создайте подключение к служебной шине.
   1. Введите имя подключения и выберите в списке пространство имен Service Bus. Откроется следующий экран.

      ![Создание подключения к служебной шине для приложения логики на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-connection-1.png)

   1. Выберите политику служебной шины (RootManageSharedAccessKey). Затем выберите **создать**.

      ![Создание подключения к служебной шине для приложения логики на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7-create-service-bus-connection-in-logic-app-azure-portal.png)

   1. На последнем экране для **имя очереди**, выберите очереди, которую вы создали, из раскрывающегося списка. Введите `175` для **максимальное число сообщений**.

      ![Установка максимального числа сообщений для подключения к служебной шине в приложении логики](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8-specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)

   1. Выберите **Сохранить** в меню в верхней части конструктора Logic Apps, чтобы сохранить изменения.

### <a name="configure-the-logic-app-action"></a>Настройте действие приложения логики

1. Создайте подключение к службе SMTP.

   1. Выберите **Новый шаг**. В **выбрать действие**выберите **все** вкладки.

   1. Тип `smtp` в поле поиска, выберите **SMTP** службы в результатах поиска, а затем выберите **отправить электронное письмо**.

      ![Создание подключения к службе SMTP в приложении логики на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9-create-smtp-connection-logic-app-azure-portal.png)

   1. Указать параметры SMTP почтового ящика, а затем выберите **создать**.

      ![Ввод сведений о подключении к службе SMTP в приложении логики на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10-enter-smtp-connection-info-logic-app-azure-portal.png)

      Получение сведений об SMTP для [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en) и [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).

      > [!NOTE]
      > Может потребоваться отключить протокол SSL для установления соединения. Если это так, и вы хотите включить SSL после установления соединения, см. в разделе необязательный шаг в конце этого раздела.

   1. Из **добавьте параметр** раскрывающееся меню **отправить электронное письмо** выберите **из**, **для**, **субъекта**и **текст**. Щелкните или нажмите в любом месте на экране, чтобы закрыть окно выбора.

      ![Выберите поля сообщения электронной почты SMTP-соединений](media/iot-hub-monitoring-notifications-with-azure-logic-apps/smtp-connection-choose-fields.png)

   1. Введите адрес электронной почты в полях **От** и **Кому** и укажите `High temperature detected` в **теме** и **тексте сообщения**. Если **Добавление динамического содержимого из приложений и соединителей, используемых в этом потоке** откроется диалоговое окно, выберите **скрыть** закрыть его. Не используйте динамическое содержимое в этом руководстве.

      ![Поля сообщения электронной почты пользователям добавлять SMTP подключения](media/iot-hub-monitoring-notifications-with-azure-logic-apps/fill-in-smtp-connection-fields.png)

   1. Выберите **Сохранить** сохранить соединения SMTP.

1. (Необязательно) Если вам необходимо отключить SSL, чтобы подключиться к поставщику услуг электронной почты и хотите снова включить ее, выполните следующие действия.

   1. На **приложение логики** панели в разделе **средства разработки**выберите **подключения API**.

   1. Из списка подключений API выберите соединения SMTP.

   1. На **smtp API подключения** панели в разделе **Общие**выберите **Правка подключения API**.

   1. На **Правка подключения API** области выберите **включить SSL?**, повторно введите пароль для учетной записи электронной почты и выберите **Сохранить**.

      ![Правка подключения SMTP API в приложении логики на портале Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/re-enable-smtp-connection-ssl.png)

Приложение логики теперь готов для обработки оповещения о температуре из очереди служебной шины и отправки уведомлений в учетную запись электронной почты.

## <a name="test-the-logic-app"></a>Тестирование приложения логики

1. Запустите клиентское приложение на устройстве.

1. При использовании физического устройства, источник тепловой рядом с датчиком тепловой тщательно перевести пока температура превышает 30 градусов с. Если вы используете онлайн-симулятор, клиентский код случайным образом будет выводить сообщения телеметрии, которые должна превышать 30 с.

1. Следует начинать уведомления по электронной почте, отправленное приложением логики.

   > [!NOTE]
   > Возможно, поставщику услуг электронной почты потребуется проверить подлинность отправителя и убедиться, что именно вы отправили это сообщение.

## <a name="next-steps"></a>Дальнейшие действия

Вы успешно создали приложение логики, обеспечивающее подключение между Центром Интернета вещей и почтовым ящиком для мониторинга температуры и отправки уведомлений.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
