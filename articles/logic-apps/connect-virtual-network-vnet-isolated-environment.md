---
title: Подключение к виртуальным сетям Azure из Azure Logic Apps через среду службы интеграции (ISE)
description: Создайте среду службы интеграции (ISE), чтобы приложения логики и учетные записи интеграции могли получать доступ к виртуальным сетям Azure, оставаясь при этом частными и изолированными от общедоступной ("глобальной") среды Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: b452485ccf235d1f245989e40840f2f0b3b2ae45
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544546"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Подключение к виртуальным сетям Azure из Azure Logic Apps с помощью среды службы интеграции (ISE)

Для сценариев, в которых приложениям логики и учетным записям интеграции требуется доступ к [виртуальной сети Azure](../virtual-network/virtual-networks-overview.md), создайте [*среду службы интеграции*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Интегрированную среду Сценариев — это закрытый и изолированной среда, которая использует выделенное хранилище и другие ресурсы, которые будут храниться отдельно в службе Logic Apps public или «глобальные». Такое разделение помогает уменьшить любое влияние других клиентов Azure на производительность вашего приложения. Среда службы интеграции *внедряется* в виртуальную сеть Azure и развертывает службу Logic Apps в вашей виртуальной сети. При создании приложений логики и учетных записей интеграции выберите эту среду службы интеграции в качестве их расположения. После этого приложение логики или учетная запись интеграции смогут напрямую получать доступ к ресурсам в виртуальной сети, таким как виртуальные машины, серверы, системы и службы.

![Выбор среды службы интеграции](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

В этой статье показано, как выполнять следующие задачи:

* настраивать порты в виртуальной сети Azure для передачи трафика через среду службы интеграции (ISE) в подсетях в виртуальной сети;

* создавать среду службы интеграции;

* создавать приложение логики, которое может выполняться в среде службы интеграции;

* создавать учетную запись интеграции для приложений логики в среде службы интеграции.

Дополнительные сведения о средах службы интеграции см. в статье [Доступ к ресурсам виртуальных сетей Azure из Azure Logic Apps с использованием сред службы интеграции (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Технические условия

* Подписка Azure. Если у вас еще нет подписки Azure, <a href="https://azure.microsoft.com/free/" target="_blank">зарегистрируйтесь для получения бесплатной учетной записи Azure</a>.

  > [!IMPORTANT]
  > Приложения логики, триггеры, встроенные, встроенных действий и соединителей, которые выполняются в интегрированной среде Сценариев использования ценовой план отличается от тарифный план с оплатой. Дополнительные сведения см. на странице с [ценами на Logic Apps](../logic-apps/logic-apps-pricing.md).

* [Виртуальная сеть Azure](../virtual-network/virtual-networks-overview.md). Если у вас нет виртуальной сети, узнайте, [как ее создать](../virtual-network/quick-create-portal.md). 

  * Виртуальной сети должен иметь четыре *пустой* подсетей для развертывания и создания ресурсов в вашей интегрированной среде Сценариев. Эти подсети можно создать заранее, или вы можете ожидать, пока вы не создадите вашей интегрированной среды Сценариев, где можно создавать подсети, в то же время. Дополнительные сведения о [требования к подсети](#create-subnet). 
  
    > [!NOTE]
    > При использовании [ExpressRoute](../expressroute/expressroute-introduction.md), который предоставляет частное подключение к облачным службам Майкрософт, вам необходимо [создать таблицу маршрутов](../virtual-network/manage-route-table.md) , имеет следующее значение маршрута и связать эту таблицу с каждой подсетью, используемых в интегрированной среде Сценариев:
    > 
    > **Имя**: <*имя маршрута*><br>
    > **Префикс адреса**: 0.0.0.0/0<br>
    > **Определение следующего прыжка**. Интернет

  * Убедитесь, что виртуальная сеть [делает доступными эти порты](#ports) вашей интегрированной среды Сценариев работает правильно, и остается доступным.

* Если вы хотите использовать пользовательские DNS-серверы для виртуальной сети Azure, [настройки этих серверов, выполнив следующие действия](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) перед развертыванием вашей интегрированной среды Сценариев к виртуальной сети. Иначе при каждом изменении DNS-сервера вам придется перезагружать среду службы интеграции. Такая возможность доступна в общедоступной предварительной версии среды службы интеграции.

* Базовые знания [создания приложений логики](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="ports"></a>

## <a name="set-up-network-ports"></a>Настройка сетевых портов

Чтобы среда службы интеграции работала правильно и оставалась доступной, ей должны быть доступны определенные порты в виртуальной сети. Если любой из этих портов будет недоступен, вы можете потерять доступ к среде службы интеграции и она может перестать работать. Когда вы используете среду службы интеграции в виртуальной сети, часто возникает проблема с блокировкой одного или нескольких нужных портов. Кроме того, дополнительные требования к портам могут быть у соединителя, который используется для подключений между средой службы интеграции и целевой системой. Например, если для связи с системой FTP вы используете соединитель FTP, убедитесь в доступности порта, который нужен для этой системы FTP (обычно это порт 21).

Для управления трафиком между подсетями виртуальной сети, где развертывается вашей интегрированной среды Сценариев, можно настроить [группы безопасности сети](../virtual-network/security-overview.md) для этих подсетей с [фильтрации сетевого трафика между подсетями](../virtual-network/tutorial-filter-network-traffic.md). В этих таблицах указаны порты, которые среда службы интеграции использует в виртуальной сети, и описано их назначение. [Теги служб Resource Manager](../virtual-network/security-overview.md#service-tags) представляет группу префиксов IP-адресов, которые помогут свести к минимуму сложности при создании правил безопасности.

> [!IMPORTANT]
> Для внутреннего взаимодействия внутри подсети в интегрированной среде Сценариев требуется открыть все порты в пределах этих подсетей.

| Назначение | Направление | порты; | Тег службы источника | Тег службы назначения | Примечания |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Поток данных из Azure Logic Apps | Исходящие | 80 и 443 | Виртуальная сеть | Интернет | Порт зависит от внешней службы, с которым взаимодействует служба Logic Apps |
| Azure Active Directory | Исходящие | 80 и 443 | Виртуальная сеть | AzureActiveDirectory | |
| Зависимость от службы хранилища Azure | Исходящие | 80 и 443 | Виртуальная сеть | Хранилище | |
| Intersubnet связи | Входящий и исходящий | 80 и 443 | Виртуальная сеть | Виртуальная сеть | Для обмена данными между подсетями |
| Поток данных в Azure Logic Apps | Входящий трафик | 443 | Интернет  | Виртуальная сеть | IP-адрес для компьютера или службы, которая вызывает любой триггер запроса или веб-перехватчика, который существует в приложении логики. Закрытие или блокирование этого порта предотвращает вызовы HTTP для приложений логики с триггерами запроса.  |
| Журнал выполнения приложения логики | Входящий трафик | 443 | Интернет  | Виртуальная сеть | IP-адрес компьютера, с помощью которой можно просмотреть приложение логики запуска журнала. Несмотря на то, что закрытие или блокирует этот порт не запрещает просмотр журнала выполнения, нельзя просмотреть входные и выходные данные для каждого шага, журнал выполнения. |
| Управление соединениями | Исходящие | 443 | Виртуальная сеть  | Интернет | |
| Публикация журналов диагностики и метрик | Исходящие | 443 | Виртуальная сеть  | AzureMonitor | |
| Обмен данными из диспетчера трафика Azure | Входящий трафик | 443 | AzureTrafficManager | Виртуальная сеть | |
| Конструктор Logic Apps — динамические свойства | Входящий трафик | 454 | Интернет  | Виртуальная сеть | Чтобы запросы исходили из приложений логики [доступ к конечной точке входящий IP-адресов в соответствующем регионе](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| Зависимость от управления службой приложений | Входящий трафик | 454 и 455 | AppServiceManagement | Виртуальная сеть | |
| Развертывание соединителя | Входящий трафик | 454 & 3443 | Интернет  | Виртуальная сеть | Необходимые для развертывания и выполняется обновление соединителей. Закрытие или блокирование этого порта приводит к интегрированной среде Сценариев развертывания, переход на другой и позволяет избежать обновлений соединителя и исправления. |
| Зависимость SQL Azure | Исходящие | 1433 | Виртуальная сеть | SQL |
| Работоспособность ресурсов Azure | Исходящие | 1886 | Виртуальная сеть | Интернет | Для публикации состояния работоспособности служба работоспособности ресурсов |
| Управление API — конечная точка управления | Входящий трафик | 3443 | APIManagement  | Виртуальная сеть | |
| Зависимость от политики передачи журнала в концентратор событий и от агента мониторинга | Исходящие | 5672 | Виртуальная сеть  | EventHub | |
| Доступ к экземплярам кэша Azure для Redis между экземплярами ролей | Входящий трафик <br>Исходящие | 6379-6383 | Виртуальная сеть  | Виртуальная сеть | Кроме того, для интегрированной среды Сценариев для работы с кэшем Azure для Redis, необходимо открыть эти [входящие и исходящие порты, описанные в кэше Azure Redis часто задаваемые вопросы о](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Балансировщик нагрузки Azure | Входящий трафик | * | AzureLoadBalancer | Виртуальная сеть |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Создание среды службы интеграции

Чтобы создать среду службы интеграции, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com) в главном меню Azure выберите **Создать ресурс**.
В поле поиска введите "среда службы интеграции" в качестве фильтра.

   ![Создание ресурса](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. На панели создания среды службы интеграции, выберите **создать**.

   ![Нажатие кнопки "Создать"](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Укажите следующие сведения для своей среды, а затем выберите **Просмотр и создание**, как показано здесь:

   ![Предоставление сведений о среде](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Свойство | Обязательно для заполнения | Value | Описание |
   |----------|----------|-------|-------------|
   | **Подписка** | Да | <*Azure-subscription-name*> | Подписка Azure, используемая для среды. |
   | **Группа ресурсов** | Да | <*имя_группы_ресурсов_Azure*> | Группа ресурсов Azure, в которой необходимо создать среду. |
   | **Имя среды службы интеграции** | Да | <*имя_среды*> | Имя, которое будет присвоено среде. |
   | **Местоположение.** | Да | <*регион_центра_обработки_данных_Azure*> | Регион центра обработки данных Azure, в котором нужно развернуть среду. |
   | **Дополнительная емкость** | Да | 0 до 10 | Число единиц дополнительной обработки, используемых для этого ресурса интегрированной среды Сценариев. Чтобы добавить емкость после ее создания, см. в разделе [ISE добавить емкость](#add-capacity). |
   | **Виртуальная сеть** | Да | <*имя_виртуальной_сети_Azure*> | Виртуальная сеть Azure, в которую нужно внедрить среду, чтобы приложения логики в этой среде могли получить доступ к виртуальной сети. Если у вас нет подключения к сети, [сначала создать виртуальную сеть Azure](../virtual-network/quick-create-portal.md). <p>**Важно!** Внедрение среды службы интеграции можно выполнить *только* при ее создании. |
   | **Подсети** | Да | <*subnet-resource-list*> | Среде службы интеграции требуется четыре *пустых* подсети для создания ресурсов в среде. Для создания каждой подсети [выполните действия, описанные в этой таблице](#create-subnet).  |
   |||||

   <a name="create-subnet"></a>

   **Создание подсети**

   Чтобы создать ресурсы в вашей среде, вашей интегрированной среды Сценариев требуется четыре *пустой* подсетей, которые не являются делегировать к любой службе. 
   Вы *нельзя* после создания среды изменить эти адреса подсети. Каждая подсеть должна соответствовать следующим критериям:

   * Имеет имя, которое начинается с буквы или символа подчеркивания, а не имеет следующие символы: `<`, `>`, `%`, `&`, `\\`, `?`, `/`

   * Использует [бесклассовой междоменной маршрутизации (CIDR) формат](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) и пространства адресов класса B.

   * Использует по крайней мере `/27` в адресе пространство, поскольку каждая подсеть должна иметь 32 адреса как *минимальное*. Например:

     * `10.0.0.0/27` содержит 32 адресов, так как 2<sup>(32-27)</sup> 2<sup>5</sup> или 32.

     * `10.0.0.0/24` содержит 256 адресов, так как 2<sup>(32 – 24)</sup> 2<sup>8</sup> или 256.

     * `10.0.0.0/28` содержит только 16 адресов и слишком мал поскольку 2<sup>(32-28)</sup> 2<sup>4</sup> или 16.

     Дополнительные сведения о вычислении адреса, см. в разделе [блоки IPv4 CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Если вы используете [ExpressRoute](../expressroute/expressroute-introduction.md), не забудьте [создать таблицу маршрутов](../virtual-network/manage-route-table.md) , имеет следующее значение маршрута и связать эту таблицу с каждой подсетью, используемых в интегрированной среде Сценариев:

     **Имя**: <*имя маршрута*><br>
     **Префикс адреса**: 0.0.0.0/0<br>
     **Определение следующего прыжка**. Интернет

   1. В списке **Подсети** выберите **Управление конфигурацией подсети**.

      ![Управление конфигурацией подсети](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. На панели **Подсети** выберите **Подсеть**.

      ![Добавление подсети](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. На панели **Добавить подсеть** введите следующие сведения.

      * **Имя**. Имя для этой подсети.
      * **Диапазон адресов (блок CIDR)**. Диапазон адресов для подсети в виртуальной сети, выраженный в формате CIDR.

      ![Добавление сведений о подсети](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Когда все будет готово, нажмите кнопку **ОК**.

   1. Повторите эти шаги еще для трех подсетей.

      > [!NOTE]
      > Если подсети, которые вы попытаетесь создать недействительны, портала Azure отображается сообщение, но не блокирует ход.

1. После того как Azure успешно проверит данные для среды службы интеграции, щелкните **Создать**, как показано здесь:

   ![После успешной проверки щелкните "Создать"](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure начнет развертывание среды. Выполнение этого процесса *может* занять до двух часов. 
   Чтобы проверить состояние развертывания, на панели инструментов Azure выберите значок уведомлений, который открывает область уведомлений.

   ![Проверка состояния развертывания](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Если развертывание будет успешно завершено, в Azure отобразится следующее уведомление:

   ![Развертывание прошло успешно](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   В противном случае следуйте инструкциям Azure портала для устранения неполадок развертывания.

   > [!NOTE]
   > Если развертывание завершается ошибкой, или вы удалите вашей интегрированной среды Сценариев Azure может занять до часа перед тем как освободить подсети. Эта задержка означает, что означает, что может потребоваться ожидания перед повторным использованием этих подсетей в другой интегрированной среде Сценариев. 
   >
   > При удалении виртуальной сети Azure обычно занимает до двух часов перед освобождением вверх в подсетях, но эта операция может занять больше времени. 
   > При удалении виртуальных сетей, убедитесь, что ресурсы не будут по-прежнему подключены. См. в разделе [удалить виртуальную сеть](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Если Azure не перейдет автоматически к вашей среде после завершения развертывания, выберите **Перейти к ресурсу**, чтобы просмотреть среду.  

Дополнительные сведения о создании подсетей см. в разделе [добавить подсеть виртуальной сети](../virtual-network/virtual-network-manage-subnet.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Создание приложения логики, использующего среду службы интеграции

Для создания приложений логики, которые выполняются в среде службы интеграции (ISE), [создание приложений логики в обычном режиме](../logic-apps/quickstart-create-first-logic-app-workflow.md) за исключением при задании **расположение** свойства, выберите вашей интегрированной среды Сценариев из  **Среды службы интеграции** раздела, например:

  ![Выбор среды службы интеграции](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

Различия в том, как триггеры и действия рабочего и как они выполняется с меткой при использовании интегрированную среду Сценариев, по сравнению с глобальной службы Logic Apps, см. в разделе [изолированной и глобального в обзоре интегрированной среды Сценариев](connect-virtual-network-vnet-isolated-environment-overview.md#difference).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Создание учетной записи интеграции для среды службы интеграции

Если вы хотите использовать учетную запись интеграции с приложениями логики в среде службы интеграции (ISE), необходимо использовать эту учетную запись интеграции *одной среде* как logic apps. Приложения логики в среде ISE могут ссылаться только на учетные записи интеграции, которые находятся в одной с ними среде ISE.

Чтобы создать учетную запись интеграции, которую использует интегрированную среду Сценариев [создать учетную запись интеграции обычным способом](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) за исключением при задании **расположение** свойства, выберите вашей интегрированной среды Сценариев из **интеграции службы среды** раздела, например:

![Выбор среды службы интеграции](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>Увеличив производительность интегрированной среды Сценариев

Базовый модуль в интегрированной среде Сценариев имеет фиксированную емкости, поэтому если вам требуется более высокая пропускная способность, можно добавить дополнительные единицы масштабирования. Вы можете автомасштабирования на основе метрик производительности или на основе количества единиц дополнительной обработки. Если выбрано автоматическое масштабирование на основе метрик, можно выбрать из различных критериев и указать пороговое значение условия для собрания такому условию.

1. На портале Azure найдите вашей интегрированной среды Сценариев.

1. Чтобы просмотреть метрики использования и производительности для интегрированной среды Сценариев, в главном меню в интегрированной среде Сценариев выберите **Обзор**.

   ![Просмотр использования для интегрированной среды Сценариев](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-usage.png)

1. Для настройки автоматического масштабирования, в разделе **параметры**выберите **горизонтальное масштабирование**. На **Настройка** вкладке, выберите **включить Автомасштабирование**.

   ![Включить автоматическое масштабирование](./media/connect-virtual-network-vnet-isolated-environment/scale-out.png)

1. Для **имя параметра автомасштабирования**, укажите имя параметра.

1. В **по умолчанию** выберите либо **масштабирования на основе метрики** или **Масштабирование до указанного числа экземпляров**.

   * Если основываться на экземпляре, введите число единиц обработки от 0 до 10 включительно.

   * Если с метриками, выполните следующие действия.

     1. В **правила** выберите **добавьте правило**.

     1. На **правила масштабирования** области, настроить условия и действия, подлежащие выполнению при активации правила.

     1. Когда все будет готово, выберите **добавить**.

1. После завершения работы с параметрами автомасштабирования, сохраните изменения.

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте больше о [виртуальной сети Azure](../virtual-network/virtual-networks-overview.md).
* Узнайте об [интеграции виртуальной сети для служб Azure](../virtual-network/virtual-network-for-azure-services.md).
