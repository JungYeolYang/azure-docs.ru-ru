---
title: Руководство по устранению неполадок производительности Azure файлы
description: Известные проблемы производительности с помощью общих папок Azure уровня "премиум" (Предварительная версия) и связанные методы обхода.
services: storage
author: gunjanj
ms.service: storage
ms.topic: article
ms.date: 04/25/2019
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: 5ae0bb736a7cc0bbc38df5905abc5d8a71f60eb9
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190055"
---
# <a name="troubleshoot-azure-files-performance-issues"></a>Устранение проблем с производительностью службы файлов Azure

В этой статье перечислены некоторые общие проблемы, связанные с уровня "премиум" файловыми ресурсами Azure (Предварительная версия). Он предоставляет потенциальные причины и способы их устранения при возникновении этих проблем.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Высокая задержка, Низкая пропускная способность и снижение общей производительности

### <a name="cause-1-share-experiencing-throttling"></a>Причина 1. Совместное использование испытывать регулирования

Квота по умолчанию в общей папке — 100 ГБ, который предоставляет базовые 100 операций ввода-ВЫВОДА (с высокой вероятностью для прорыва до 300 на один час). Дополнительные сведения о подготовке и его связь с операций ввода-ВЫВОДА, см. в разделе [подготовки общих папок](storage-files-planning.md#provisioned-shares) разделе руководства по планированию.

Для подтверждения, если общий ресурс регулируется, вы можете использовать метрики Azure на портале.

1. Войдите на [портале Azure](https://portal.azure.com).

1. Выберите **все службы** и выполните поиск **метрики**.

1. Выберите **Метрики**.

1. Выберите свою учетную запись хранения в качестве ресурса.

1. Выберите **файл** как пространство имен метрики.

1. Выберите **транзакций** качестве метрики.

1. Добавьте фильтр для **ResponseType** и посмотрите, если все запросы имеют код ответа **SuccessWithThrottling**.

![Параметры метрик для уровня "премиум" папкам](media/storage-troubleshooting-premium-fileshares/metrics.png)

### <a name="solution"></a>Решение

- Увеличение совместное использование подготовленную емкость, указав более высокую квоту на общем ресурсе.

### <a name="cause-2-metadatanamespace-heavy-workload"></a>Причина 2. Высокая рабочая нагрузка метаданных и пространства имен

Если большинство запросов метаданных, ориентированные на (например, createfile/openfile/closefile/queryinfo/querydirectory), а затем задержка будет хуже по сравнению с операциями чтения и записи.

Чтобы проверить, если метаданные в ориентированных на большинство запросов, можно использовать те же действия, как описано выше. За исключением вместо добавления фильтра для **ResponseType**, добавьте фильтр для **имя API**.

![Фильтр для имени API ваших метрик](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>Возможное решение

- Проверьте, если приложение можно изменить, чтобы уменьшить количество операций с метаданными.

### <a name="cause-3-single-threaded-application"></a>Причина 3. Однопоточного приложения

Если используемого клиентом приложения является однопотоковым, это может привести значительно меньше операций ввода-ВЫВОДА и пропускную способность, чем максимально возможное число на основе вашего размера подготовленного общего файлового ресурса.

### <a name="solution"></a>Решение

- Увеличьте параллелизм приложения, увеличив количество потоков.
- Переключиться на приложения, где возможна параллелизма. Например, для операций копирования клиентам использовать AzCopy или RoboCopy из клиентов Windows или **параллельных** команду на клиентах Linux.

## <a name="very-high-latency-for-requests"></a>Высокая задержка для запросов

### <a name="cause"></a>Причина:

На клиентскую виртуальную Машину может находиться в разных регионах файловый ресурс уровня "премиум".

### <a name="solution"></a>Решение

- Запустите приложение от виртуальной Машины, который находится в том же регионе, что файловый ресурс уровня "премиум".

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>Клиенту не удается обеспечить максимальную пропускную способность, поддерживаемых в сети

Одна из причин этого является отсутствие fo SMB поддержку нескольких каналов. В настоящее время файловые ресурсы Azure поддерживают только один канал, поэтому доступно только одно подключение к серверу из клиентской виртуальной Машине. Это одно подключение привязанное одно ядро на клиентской виртуальной Машине, чтобы максимальная пропускная способность, достигаемую из виртуальной Машины ограничивается одним ядром.

### <a name="workaround"></a>Возможное решение

- Получение виртуальной Машины с помощью больше core может помочь повысить пропускную способность.
- Запустить клиентское приложение из нескольких виртуальных машин будет увеличить пропускную способность.
- Используйте интерфейсы REST API, где это возможно.

## <a name="throughput-on-linux-clients-is-significantly-lower-when-compared-to-windows-clients"></a>Пропускная способность на клиентах Linux значительно меньше по сравнению с клиентами Windows.

### <a name="cause"></a>Причина:

Это известная проблема с реализацией SMB-клиента на платформе Linux.

### <a name="workaround"></a>Возможное решение

- Распределить нагрузку между несколькими виртуальными машинами
- На той же виртуальной Машине, используйте несколько точек подключения с **nosharesock** параметр и распределить нагрузку между такими точки подключения.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Высоких значений задержки для больших рабочих нагрузок метаданных с обширной операций открытия и закрытия.

### <a name="cause"></a>Причина:

Отсутствие поддержки аренды каталогов.

### <a name="workaround"></a>Возможное решение

- По возможности избегайте чрезмерного открывающий и закрывающий маркер на том же каталоге, в течение короткого периода времени.
- Для виртуальных машин Linux, увеличить тайм-аут кэша записи каталога, указав **actimeo =<sec>**  как параметр подключения. По умолчанию это одну секунду, так что может помочь большее значение как три или пять.
- Для виртуальных машин Linux обновите ядра до 4.20 или более поздней версии.

## <a name="low-iops-on-centosrhel"></a>Низкий операций ввода-ВЫВОДА на CentOS/RHEL

### <a name="cause"></a>Причина:

Глубина операций ввода-ВЫВОДА больше 1 не поддерживается на CentOS/RHEL.

### <a name="workaround"></a>Возможное решение

- Обновление до CentOS 8 / RHEL 8.
- Измените на Ubuntu.

## <a name="jitterysaw-tooth-pattern-for-iops"></a>Шаблон перебои управления/saw конино для операций ввода-ВЫВОДА

### <a name="cause"></a>Причина:

Клиентское приложение постоянно превышает базовых показателей операций ввода-ВЫВОДА. Сейчас доступна не на стороне службы сглаживание нагрузки запросов, так что если клиент превышает базовых показателей операций ввода-ВЫВОДА, скорость будет ограничена службой. Что регулирование может привести клиента, где произошли шаблон перебои управления/saw конино операций ввода-ВЫВОДА. В этом случае среднее операций ввода-ВЫВОДА достигается клиента может быть ниже, чем базовые показатели операций ввода-ВЫВОДА.

### <a name="workaround"></a>Возможное решение

- Снизьте нагрузку запроса из клиентского приложения, таким образом, чтобы не выполнять регулирование общего ресурса.
- Увеличьте квоту общего ресурса, чтобы не выполнять регулирование общего ресурса.

## <a name="excessive-directoryopendirectoryclose-calls"></a>Большое число вызовов DirectoryOpen/DirectoryClose

### <a name="cause"></a>Причина:

Если число вызовов DirectoryOpen/DirectoryClose указан среди top вызовы API, а не предполагается, что клиенту осуществлять, что многие вызовы, она может быть проблема с антивирусной программы, установленной на клиенте Azure виртуальной Машины.

### <a name="workaround"></a>Возможное решение

- Исправление этой проблемы доступно в [апреля платформы обновления для Windows](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform).

## <a name="file-creation-is-slower-than-expected"></a>Создание файла не медленнее, чем ожидалось

### <a name="cause"></a>Причина:

Рабочие нагрузки, которые полагаются на создание большое количество файлов не увидит существенное различие между производительность файловых ресурсов уровня "премиум" и стандартные файловые ресурсы.

### <a name="workaround"></a>Возможное решение

- Отсутствует.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Низкая производительность из Windows 8.1 или Server 2012 R2

### <a name="cause"></a>Причина:

Выше, чем ожидаемая задержка, доступ к файлам Azure для рабочих нагрузок с большим объемом операций ввода-ВЫВОДА.

### <a name="workaround"></a>Возможное решение

- Установка доступных [исправление](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1).