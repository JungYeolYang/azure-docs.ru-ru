---
title: Защита Azure, архитектура
description: Это эталонную архитектуру для архитектура демилитаризованной ЗОНЫ корпоративного уровня, виртуальные сетевые устройства и другие средства. Эта архитектура была разработана для соответствия требованиям министерства обороны защита облачных вычислений архитектуры функциональной. Тем не менее он будет доступен для любой организации. Этот справочник содержит две автоматические параметры, с помощью Citrix или F5 устройств.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: f2e3d72db3f29dbc6d03b3259acb18daf684fb12
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917588"
---
# <a name="secure-azure-computing-architecture"></a>Защита Azure, архитектура

Руководство по настройке защищенных виртуальных сетей и настройка средств обеспечения безопасности и служб, предусмотрено стандартов министерства обороны США и практических просили наши учитывать быстро увеличивающий количество клиентов министерства обороны, развертывании рабочих нагрузок в Azure. Опубликованные DISA [функциональные требования документа Secure Cloud Computing архитектуры (SCCA)](https://iasecontent.disa.mil/stigs/pdf/SCCA_FRD_v2-9.pdf) в 2017 г. SCCA описаны функциональной цели для защиты Defense сведения о системе сети (DISN) и подключения поставщика коммерческих облачных служб, указывает и как критически владельцев безопасные облачные приложения, расположенным на границе подключения. Он является обязательной, что каждая сущность министерства обороны США, который подключается к коммерческой облачной следует правилам, изложенных в SCCA FRD.
 
Есть четыре компонента SCCA. Точка доступа к облаку границ (BCAP), стека безопасности виртуального центра обработки данных (VDSS), виртуальный центр обработки данных служб (процессы VDM) под управлением и доверенные диспетчер учетных данных облака (TCCM). Корпорация Майкрософт разработала решения, которая будет соответствовать требованиям SCCA IL4 и IL5 рабочих нагрузок, запущенных в Azure. Это решение Azure конкретных называется Secure Azure вычислений архитектуры (SACA). Клиенты, которые развертывают SACA будет соответствовали SCCA FRD и позволит перемещать рабочие нагрузки в Azure после того, как клиентам министерства обороны США. 

Хотя архитектуры и рекомендации SCCA характерные для клиентов министерства обороны, последних ревизий для SACA также поможет сотрудниках гражданских клиентам обеспечить соответствие законам доверенного подключения (КРЕСТИКИ) руководство по internet, а также коммерческими клиентами, которые желают реализовать безопасной сети Периметра для Защитите свои среды azure.


## <a name="secure-cloud-computing-architecture-components"></a>Защита облачных вычислений архитектура компонентов

**BCAP**

BCAP предназначена для защиты от атак, выполняемых в облачной среде DISN. BCAP будет выполнять обнаружение вторжений и предотвращения, а также отфильтровать несанкционированного трафика. Этот компонент может размещаться вместе с другими компонентами SCCA. Настоятельно рекомендуется, что этот компонент развертывается с помощью физического оборудования. Ниже Вы найдете список BCAP требования к безопасности.

***Требования к безопасности BCAP***

![Матрица требований к BCAP](media/bcapreqs.png)


**VDSS**

VDSS предназначена для защиты приложений владелец критически министерства обороны США, размещенных в Azure. VDSS выполняет ряд операций безопасности в SCCA. Он будет проводить проверку трафика для обеспечения безопасности приложений, работающих в Azure. Этот компонент можно задавать в пределах среды Azure.

***Требования к безопасности VDSS***

![Матрица требований к VDSS](media/vdssreqs.png)

**ПРОЦЕССЫ VDM**

Цель процессы VDM заключается в предоставлении безопасности узла, а также общие службы центра данных. Функции процессы VDM можно запустить в концентраторе вашей SCCA или владелец критически развернуть отдельные его части в свои собственные определенной подписке Azure. Этот компонент можно задавать в пределах среды Azure.

***Требования к безопасности процессы VDM***

![Матрица требований к процессы VDM](media/vdmsreqs.png)


**TCCM**

TCCM является бизнес-роли. Этот пользователь будет отвечать за управление SCCA. В его обязанности входит установка планы и политики для учетной записи доступа к облачной среде, обеспечивая удостоверений и управления доступом функционирует правильно и для поддержания план управления учетных данных облака. Этот пользователь является назначенными корпорацией официальный авторизации. BCAP, VDSS и процессы VDM предоставит возможности, которые требуются для TCCM для выполнения должностных функций.

***Требования к безопасности TCCM***

![Матрица требований к TCCM](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>SACA компоненты и рекомендации по планированию 

Эталонная архитектура SACA предназначен для развертывания компонентов VDSS и процессы VDM в azure, а также включить TCCM. Эта архитектура является модульной, который означает, что все компоненты, VDSS и процессы VDM могут существовать в информационный центр, или некоторые элементы управления могут выполняться в критически владельца пространства или даже локально. Наша команда Майкрософт рекомендуется для совместного размещения компонентов VDSS и процессы VDM в центральной виртуальной Net, все владельцы критически может подключиться при помощи. На схеме ниже показан наш рекомендуемой конфигурации. 


![Схема эталонной архитектуры SACA](media/sacav2generic.png)

При планировании стратегии соответствия SCCA и технической архитектуры, которые следует учитывать множество моментов. Очень важно, что следующие разделы учитывается с самого начала, как каждого клиента необходимо охватить эти. Перечисленные ниже разделы были проблемы, которые получили реальных клиентов министерства обороны США и замедляют планирования и выполнения. 

- Какие BCAP будет использовать в вашей организации?
    - DISA BCAP
        - DISA имеет два оперативной BCAPs уровне пятиугольник и Camp Иванов ЦС, с третьей скоро online. 
        - Все BCAPs DISA и использовать каналы ExpressRoute в Azure, который будет доступен для подключения клиентов министерства обороны США. 
        - DISA уже сеанс Пиринга Майкрософт корпоративного уровня для клиентов министерства обороны США, которые требуется подписаться средства SaaS корпорации Майкрософт, такие как Office 365. С помощью DISA BCAP, вы можете включить подключения и пиринг для своего экземпляра SACA. 
    - Создание собственных BCAP
        - Это необходимо предоставлять в аренду пространство в центре совместно размещенных данных и настройки канала ExpressRoute в Azure. 
        - Этот параметр будет требовать дополнительные утверждения 
        - Из-за дополнительного утверждения и физическое построение out этот вариант требует больше всего времени. 
    - Корпорация Майкрософт рекомендует — использование DISA BCAP. Этот параметр доступен, содержит встроенную избыточность и уже клиентами беспрецедентные на нем уже сегодня в рабочей среде.
- Маршрутизируемый IP-адрес пространства министерства обороны США
    - Необходимо будет использовать маршрутизируемый IP-адрес пространства министерства обороны на границе. Параметр для преобразования сетевых адресов их в пространство частных IP-адрес в Azure доступен.  
    - Свяжитесь с сетевым Адаптером министерства обороны США, чтобы получить пространство IP-адрес, оно потребуется в рамках вашего отправления ОСНАСТКИ с DISA. 
    - Если вы планируете пространство частных адресов в Azure для преобразования сетевых адресов, вам потребуется как минимум/24 подсети адресного пространства, назначенные из сетевой КАРТЫ для каждого региона, необходимо выполнить развертывание SACA. 
- Избыточность 
    - Корпорация Майкрософт предлагает развернуть экземпляр SACA по крайней мере два региона. В облаке министерства обороны США это означает, что развертывается в обоих регионах министерства обороны США. 
    - Кроме того, рекомендуем соединиться по крайней мере два BCAPs через отдельные каналы ExpressRoute. Оба каналы Expressroute затем могут быть связаны с экземпляром SACA каждого региона. 
- Требования к специфических для компонентов DoD
    - У организации есть специальных требований за пределами SCCA требования? (В некоторых организациях нет особых требований IP-адреса)
- SACA — это модульная архитектура  
    - Используйте только компоненты, необходимые для вашей среды. 
        - Развернуть виртуальные сетевые устройства в одном уровне или несколькими уровнями
        - Интегрированные IP-адресов и возможность использования собственных IP-адресов
- Уровень влияния министерства обороны США, приложений и данных
    - Если есть вероятность того, приложений, работающих в других регионах IL5, рекомендуется создать экземпляр SACA в IL5. Экземпляр может использоваться перед IL4 приложений, а также IL5. Однако экземпляр IL4 SACA перед IL5 приложения скорее всего не получит аккредитации. 
- Какой поставщик виртуальное сетевое устройство будет использовать для VDSS?
    - Как упоминалось ранее, эта ссылка SACA может быть построен с использованием широкого спектра устройств и служб Azure. Тем не менее у нас есть шаблоны автоматизированное решение для развертывания SACA архитектуру с помощью F5 и Citrix. Эти решения будут рассматриваться более подробно ниже. 
- Какие службы Azure вы будете использовать?
    - Существуют службы Azure, которые требованиям решения log analytics, защиты на основе узла и Идентификаторы функциональные возможности. Тем не менее существует возможность, что некоторые службы недоступны обычно в других регионах IL5. Это может привести к необходимости использования некоторых сторонних средств, если эти службы Azure не может обеспечить соблюдение требований. Необходимо будет рассмотреть, какие средства являются слишком сложными и возможности с помощью Azure собственными средствами. 
    - Это, корпорация Майкрософт рекомендует использовать столько собственного средства Azure по возможности, как они создаются с учетом безопасности облака и простая интеграция с остальной частью платформы Azure. Ниже приведен список Azure собственных средств, которые могут использоваться различные требованиям SCCA. 
        - [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview )
        - [Центр безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) 
        - [Наблюдатель за сетями](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
        - [Хранилище ключей Azure](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) 
        - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
        - [Шлюз приложений](https://docs.microsoft.com/azure/application-gateway/overview)
        - [Брандмауэр Azure](https://docs.microsoft.com/azure/firewall/overview) 
        - [Azure двери](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
        - [Группы безопасности](https://docs.microsoft.com/azure/virtual-network/security-overview)
        - [Защита от атак DDoS Azure](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
        - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Определение размера
    - Определения размера будет необходимо выполнить. Необходимо будет просмотреть число одновременных подключений, возможно, через экземпляр SACA, а также требования к пропускной способности сети. 
    - Это важный этап, поскольку он будет помогают размер виртуальных машин, а также помочь определить лицензии, которые будут необходимы от различных поставщиков, которые будут использоваться в вашем экземпляре SACA. 
    - Не удается выполнить анализ затрат хороший без этого оценку размеров, важно также убедиться, все, что имеет правильный размер для обеспечения оптимальной производительности. 


## <a name="most-common-deployment-scenario"></a>Наиболее распространенным сценарием развертывания

Корпорация Майкрософт имеет несколько клиентов, которые уже прошли через полного развертывания или по крайней мере этапы своих сред SACA планирования. Это позволило нам получить представление о наиболее распространенным сценарием развертывания. На схеме ниже показан наиболее распространенные архитектуры. 


![Схема эталонной архитектуры SACA](media/sacav2commonscenario.png) 


Как видно из диаграммы, клиенты министерства обороны США обычно подписаться на два BCAPs DISA, один из этих находятся на западном побережье и других жизнь на восточном побережье. Однорангового объекта ExpressRoute Private включена в Azure во всех расположениях DISA BCAP. Такие одноранговые узлы ExpressRoute затем связываются с шлюза виртуальной сети, предназначенные для министерства обороны Востока и министерства обороны США в центральных регионах Azure. Экземпляр SACA развертывается в регионе Восток министерства обороны США и министерства обороны США центра Azure и все входящий и исходящий трафик проходит через него через подключения Express Route DISA BCAP. 

Миссия владельца приложения выберите каких регионах Azure будет развертывать свои приложения в и с помощью пиринга между виртуальными сетями подключение свои приложения в виртуальной сети к виртуальной сети SACA. Принудительное туннелирование всего трафика через экземпляр VDSS. 

Эта архитектура настоятельно рекомендуется корпорацией Майкрософт, так как он будет соответствовать требованиям SCCA, высокой доступности, легко масштабируются и упрощает развертывание и управление.

## <a name="automated-saca-deployment-options"></a>Автоматические параметры развертывания SACA

 Ранее мы упоминали, что корпорация Майкрософт сотрудничает с двух поставщиков для создания автоматизированных шаблонов инфраструктуры SACA. Оба шаблона развертываются следующие компоненты Azure. 

- SACA виртуальной сети
    - Подсеть управления
        - Где развернуты виртуальные машины управления и служб (поля перехода)
        - Процессы VDM подсети
            - Где развернуты виртуальные машины и службы, используемые для процессы VDM
        - Недоверенных и доверенных подсетей 
            - Где развернуты виртуальные устройства
        - Подсеть шлюза
            - Где будет развернут шлюз ExpressRoute
- Виртуальных машин управления перехода поле
    - Используется для внешнего управления среды.
- Виртуальные сетевые модули
    - Citrix или F5, в зависимости от шаблона, который можно развернуть.
- Общедоступные IP-адреса.
    - Используется для внешнего интерфейса, пока не ExpressRoute будет подключен к сети. Эти IP-адреса будет преобразовано в серверную часть Azure частного адресного пространства
- таблицы маршрутов; 
    - Применяемый во время автоматизации этих маршрутов таблицы принудительного туннелирования весь трафик через виртуальное устройство
- Подсистемы балансировки нагрузки Azure - SKU "стандартный"
    - Они используются для балансировки нагрузки трафика между устройствами
- группы сетевой безопасности;
    - Используется для управления, типы трафика могут перемещаться на определенные конечные точки


**Развертывание Citrix SACA**

Citrix создал шаблон развертывания, который развертывает два уровня высокой доступности устройств Citrix ADC. Эта архитектура соответствует требованиям к VDSS. 

![Схема Citrix SACA](media/citrixsaca.png)


Документации Citrix и скрипта развертывания можно найти [здесь.](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca)


 **F5 SACA развертывания**

F5 создал два отдельных развертывания шаблонов, охватывающих две различные архитектуры. Первый содержит только один слой F5 устройств в активный-активный высокой доступности. Эта архитектура соответствует требованиям для VDSS. Второй добавляет второй уровень высокой доступности F5s активный — активный. Этот второй уровень предназначена для того, чтобы разрешить клиентам добавлять свои собственные IP-адреса, отдельно от F5 между слоями F5. Не все компоненты министерства обороны США имеют указано для использования определенных IP-адресов. Если это так, один слой устройств F5 будет работать для наиболее с момента что архитектура содержит IP-адресов на устройствах F5.  

![Схема Citrix SACA](media/f5saca.png)

Документация по F5 и скрипта развертывания можно найти [здесь.](https://github.com/f5devcentral/f5-azure-saca) 












