---
title: Ценовые категории для базы данных Azure для PostgreSQL — один сервер
description: 'В этой статье описаны ценовые категории для базы данных Azure для PostgreSQL: один сервер.'
author: jan-eng
ms.author: janeng
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: e2580a57f943ad8da16cfbaeda2ee35d0f4bb691
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65073183"
---
# <a name="pricing-tiers-in-azure-database-for-postgresql---single-server"></a>Ценовые категории в базе данных Azure для PostgreSQL — один сервер

Вы можете создать сервер службы "База данных Azure для PostgreSQL" в одной из трех ценовых категорий: "Базовый", "Общего назначения" и "Оптимизированная для операций в памяти". Ценовые категории различаются объемом вычислительных ресурсов в виртуальных ядрах, которые можно подготовить, объемом памяти на виртуальное ядро и технологиями хранения данных. Все ресурсы настраиваются на уровне сервера PostgreSQL. У сервера может быть одна или несколько баз данных.

|    | **базовая;** | **Общего назначения** | **С оптимизацией для операций в памяти** |
|:---|:----------|:--------------------|:---------------------|
| Поколение вычислительных ресурсов | Поколение 4, поколение 5 | Поколение 4, поколение 5 | Поколение 5 |
| Виртуальные ядра | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| Объем памяти на виртуальное ядро | 2 ГБ | 5 ГБ | 10 ГБ |
| Размер хранилища | От 5 ГБ до 1 ТБ | От 5 ГБ до 4 ТБ | От 5 ГБ до 4 ТБ |
| Тип хранилища | Служба хранилища Azure класса Standard | Хранилище Azure класса Premium | Хранилище Azure класса Premium |
| Срок хранения резервной копии | От 7 до 35 дней | От 7 до 35 дней | От 7 до 35 дней |

Чтобы выбрать ценовую категорию, в качестве отправной точки используйте следующую таблицу.

| Ценовой уровень | Целевые рабочие нагрузки |
|:-------------|:-----------------|
| базовая; | Рабочие нагрузки с невысокими вычислительными потребностями и производительностью операций ввода-вывода. В качестве примера можно привести серверы, используемые для разработки или тестирования, или небольшие редко используемые приложения. |
| Общего назначения | Для решения большинства бизнес-задач требуются сбалансированные вычислительные ресурсы и память с масштабируемой пропускной способностью операций ввода-вывода. К примерам относятся серверы для размещения мобильных и веб-приложений, а также других корпоративных приложений.|
| С оптимизацией для операций в памяти | Рабочие нагрузки с высокой производительностью базы данных, требующие эффективной обработки данных в памяти для быстрого выполнения транзакций и повышения уровня параллелизма. К примерам относятся серверы для обработки данных в режиме реального времени, а также аналитические или транзакционные приложения с высоким уровнем производительности.|

После создания сервера количество виртуальных ядер, поколение оборудования и ценовую категорию (кроме переключения на категорию "Базовый" и с нее) можно увеличить и уменьшить в считаные секунды. Кроме того, можно независимо друг от друга увеличивать и уменьшать объем хранилища и срок хранения резервных копий без простоя приложения. После создания сервера невозможно изменить тип хранилища резервных копий. Дополнительные сведения см. в разделе [Масштабирование ресурсов](#scale-resources).


## <a name="compute-generations-and-vcores"></a>Поколения вычислительных ресурсов и виртуальные ядра

Вычислительные ресурсы поставляются в виде виртуальных ядер, которые представляют собой логические ЦП основного оборудования. Сейчас можно выбирать между двумя поколениями вычислительных ресурсов: 4 и 5. Логические ЦП 4-го поколения основаны на процессорах Intel E5-2673 v3 (Haswell) с тактовой частотой 2,4 ГГц. Логические ЦП 5-го поколения основаны на процессорах Intel E5-2673 v4 (Broadwell) с тактовой частотой 2,3 ГГц. ЦП 4-го и 5-го поколений доступны в следующих регионах (X обозначает доступность). 

| **Регион Azure** | **Поколение 4** | **Поколение 5** |
|:---|:----------:|:--------------------:|
| Центральный регион США |  | X |
| Восточная часть США |  | X |
| Восток США 2 |  | X |
| Центрально-северная часть США |  | X |
| Центрально-южная часть США |  | X |
| Запад США |  | X |
| Западный регион США 2 |  | X |
| Южная часть Бразилии |  | X |
| Центральная Канада |  | X |
| Восточная Канада |  | X |
| Северная Европа |  | X |
| Западная Европа |  | X |
| Центральная Франция |  | X |
| Южная часть Великобритании |  | X |
| Западная часть Великобритании |  | X |
| Восточная Азия |  | X |
| Юго-Восточная Азия |  | X |
| Восточная часть Австралии |  | X |
| Центральная Австралия |  | X |
| Центральная Австралия 2 |  | X |
| Юго-Восточная часть Австралии |  | X |
| Центральная Индия |  | X |
| Южная Индия |  | X |
| Западная Индия |  | X |
| Восточная часть Японии |  | X |
| Западная часть Японии |  | X |
| Центральная Корея |  | X |
| Корея, юг |  | X |
| Восточный Китай 1 | X |  |
| Восточный Китай 2 |  | X |
| Северный Китай 1 | X |  |
| Северный Китай 2 |  | X |
| Центральная Германия |  | X |
| Центральная часть США, МО  | X |  |
| Восток США, МО  | X |  |
| Аризона (для обслуживания государственных организаций США) |  | X |
| Техас (для обслуживания государственных организаций США) |  | X |
| Правительство штата Вирджиния |  | X |

## <a name="storage"></a>Хранилище

Хранилище, которое вы подготавливаете, определяет объем доступной емкости хранилища для сервера службы "База данных Azure для PostgreSQL". Хранилище используется для файлов базы данных, временных файлов, журналов транзакций и журналов сервера PostgreSQL. Общий объем хранилища, который вы подготовили, также определяет доступную производительность операций ввода-вывода для сервера.

|    | **базовая;** | **Общего назначения** | **С оптимизацией для операций в памяти** |
|:---|:----------|:--------------------|:---------------------|
| Тип хранилища | Служба хранилища Azure класса Standard | Хранилище Azure класса Premium | Хранилище Azure класса Premium |
| Размер хранилища | От 5 ГБ до 1 ТБ | От 5 ГБ до 4 ТБ | От 5 ГБ до 4 ТБ |
| Шаг приращения хранилища | 1 GB | 1 GB | 1 GB |
| IOPS | Переменная |3 операции ввода-вывода в секунду на ГБ<br/>Минимум 100 операций ввода-вывода в секунду<br/>Макс. 6000 операций ввода-вывода в секунду | 3 операции ввода-вывода в секунду на ГБ<br/>Минимум 100 операций ввода-вывода в секунду<br/>Макс. 6000 операций ввода-вывода в секунду |

Вы можете добавить дополнительную емкость во время и после создания сервера. Для ценовой категории "Базовый" фиксированное число операций ввода-вывода в секунду не гарантируется. В категориях "Общего назначения" и "С оптимизацией для операций в памяти" показатель операций ввода-вывода в секунду соотносится с подготовленным размером хранилища как 3 к 1.

Вы можете отслеживать показатель операций ввода-вывода на портале Azure или с помощью команд Azure CLI. Следует отслеживать такие метрики, как [размер хранилища, процентное соотношение для хранилища, используемое хранилище и процент операций ввода-вывода данных](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Достигнут размер хранилища

Сервер помечается как доступный только для чтения, когда объем свободной емкости хранилища становится меньше 5 ГБ или 5 % от емкости подготовленного хранилища (смотря какое значение меньше). Например, если вы подготовили 100 ГБ хранилища и фактическое использование превысило 95 ГБ, то сервер помечается как доступный только для чтения. Еще пример: если вы подготовили 5 ГБ хранилища, то сервер помечается как доступный только для чтения, когда остается менее 250 МБ свободного хранилища.  

Если сервер помечен как доступный только для чтения, все существующие сеансы отключаются и выполняется откат всех незафиксированных транзакций. Какие-либо последующие операции записи и фиксации транзакций не выполняются. Все запросы на чтение продолжат обрабатываться без перерыва.  

Можно увеличить объем подготовленного хранилища для сервера или запустить новый сеанс в режиме чтения и записи и удалить данные, чтобы освободить хранилище. Команда `SET SESSION CHARACTERISTICS AS TRANSACTION READ WRITE;` задает режим записи для чтения для текущего сеанса. Во избежание повреждения данных не выполняйте какие-либо операции записи, пока сервер находится в состоянии "только для чтения".

Мы рекомендуем настроить оповещение для получения уведомления о приближении к порогу емкости хранилища сервера, чтобы избежать перехода в состояние только для чтения. Дополнительные сведения см. в статье о том, [как настроить оповещение](howto-alert-on-metric.md).

## <a name="backup"></a>Azure Backup

В службе автоматически создаются резервные копии сервера. Минимальный срок хранения резервных копий составляет семь дней. Можно увеличить срок вплоть до 35 дней. Срок хранения можно изменить в любой момент времени существования сервера. Вы можете выбрать локально избыточное или геоизбыточное резервное копирование. Геоизбыточные резервные копии хранятся в [регионе, географически связанном](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) с тем, в котором создан сервер. Эта избыточность обеспечивает защиту сервера в случае аварии. Также вы можете восстановить сервер в любом регионе Azure, в котором доступна служба с геоизбыточным резервным копированием. После создания сервера нельзя изменить один вариант хранилища резервных копий на другой.

## <a name="scale-resources"></a>Масштабирование ресурсов

После создания сервера можно независимо друг от друга изменять количество виртуальных ядер, поколение оборудования, ценовую категорию (кроме переключения на категорию "Базовый" и с нее), объем хранилища и срок хранения резервных копий. После создания сервера невозможно изменить тип хранилища резервных копий. Число виртуальных ядер можно увеличивать или уменьшать. Срок хранения резервных копий можно увеличивать и уменьшать в диапазоне от 7 до 35 дней. Размер хранилища можно только увеличить. Масштабирование ресурсов можно выполнить с помощью портала или Azure CLI. Пример масштабирования с помощью CLI см. в статье [Мониторинг и масштабирование сервера базы данных Azure для PostgreSQL с помощью Azure CLI](scripts/sample-scale-server-up-or-down.md).

При изменении количества виртуальных ядер, поколения оборудования или ценовой категории создается копия сервера-источника с новым распределением вычислительных ресурсов. После того как новый сервер заработает, подключения переключатся на него. Во время перехода системы на новый сервер нельзя будет установить новые подключения, а для всех незафиксированных транзакций будет выполнен откат. Длительность этого промежутка может быть разной, но, как правило, он составляет меньше минуты.

Масштабирование хранилища и изменение срока хранения резервных копий — это полностью интерактивные операции. Они не останавливают работу приложения и не влияют на нее. Так как количество операций ввода-вывода в секунду возрастает по мере увеличения размера подготовленного хранилища, вы можете увеличивать количество доступных операций ввода-вывода на сервере путем масштабирования хранилища.

## <a name="pricing"></a>Цены

Наиболее актуальные сведения о стоимости см. в статье [Цены на Базу данных Azure для PostgreSQL](https://azure.microsoft.com/pricing/details/PostgreSQL/). Расходы на вашу конфигурацию можно посмотреть на [портале Azure](https://portal.azure.com/#create/Microsoft.PostgreSQLServer). На вкладке **Ценовая категория** отображается ежемесячная стоимость выбранных параметров. Если у вас нет подписки Azure, для расчета цены можно воспользоваться калькулятором цен Azure. На сайте с [калькулятором цен Azure](https://azure.microsoft.com/pricing/calculator/) нажмите кнопку **Добавить элементы**, разверните категорию **Базы данных** и выберите **База данных Azure для PostgreSQL**, чтобы настроить параметры.

## <a name="next-steps"></a>Дальнейшие действия

- [Руководство по проектированию службы "База данных Azure для PostgreSQL" с помощью портала Azure](tutorial-design-database-using-azure-portal.md).
- Дополнительные сведения см. в статье [Ограничения в базе данных Azure для PostgreSQL](concepts-limits.md). 
- Дополнительные сведения см. в статье [Создание реплик чтения и управление ими с помощью портала Azure](howto-read-replicas-portal.md).
