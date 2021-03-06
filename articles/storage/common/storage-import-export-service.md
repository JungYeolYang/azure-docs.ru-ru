---
title: Использование службы "Импорт и экспорт Azure" для передачи данных в службу хранилища Azure и обратно | Документация Майкрософт
description: Сведения о создании заданий импорта и экспорта на портале Azure для передачи данных в службу хранилища Azure и обратно.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/07/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 97a3ac275613b644dfd90144039e4f3127186997
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65603107"
---
# <a name="what-is-azure-importexport-service"></a>Что такое служба "Импорт и экспорт Azure"?

Служба "Импорт и экспорт Azure" используется для безопасного переноса больших объемов данных в хранилище BLOB-объектов Azure и службу файлов Azure посредством отправки дисков в центр обработки данных Azure. Кроме того, эту службу можно использовать, чтобы переносить данные из хранилища BLOB-объектов Azure на диски и передавать на локальные сайты. Данные с одного или нескольких дисков можно импортировать в хранилище BLOB-объектов Azure или службу файлов Azure. 

Подключайте собственные диски и передавайте данные с помощью службы "Импорт и экспорт Azure". Также можно использовать диски, предоставляемые корпорацией Майкрософт. 

Если вам нужно передать данные с помощью дисков, предоставленных корпорацией Майкрософт, для импорта данных в Azure можно использовать [диск Azure Data Box](../../databox/data-box-disk-overview.md). Корпорация Майкрософт поставляет до пяти зашифрованных твердотельных накопителей (SSD) с общей емкостью 40 ТБ для одного заказа в центр обработки данных через регионального поставщика. Вы можете быстро настроить диски, скопировать на них данные с помощью подключения USB 3.0 и отправить их обратно в Azure. Дополнительные сведения см в [обзоре дисков Azure Data Box](../../databox/data-box-disk-overview.md).

## <a name="azure-importexport-usecases"></a>Варианты использования службы "Импорт и экспорт Azure"

Службу "Импорт и экспорт Azure" можно использовать, если отправка или скачивание данных по сети занимают слишком много времени, а увеличение пропускной способности сети нерентабельно. Используйте эту службу в следующих сценариях.

* **Перенос данных в облако**. Быстрое и экономичное перемещение больших объемов данных в Azure.
* **Распространение содержимого**. Быстрая отправка данных на веб-сайты клиентов.
* **Архивация**. Создание резервных копий локальных данных для хранения в службе хранилища Azure.
* **Восстановление данных**. Восстановление большого объема данных в хранилище и его перенос в локальное расположение.

## <a name="importexport-components"></a>Компоненты служба "Импорт и экспорт"

Служба "Импорт и экспорт" использует следующие компоненты.

- **Служба"Импорт и экспорт"**. Эта служба доступна на портале Azure, она помогает пользователю создавать и отслеживать задания импорта (отправки) и экспорта (скачивания).  

- **Средство WAImportExport**. Это программа командной строки, которая выполняет следующие действия: 
    - подготавливает ваши диски, которые доставляются для импорта;
    - упрощает копирование данных на диск;
    - шифрует данные на диске с помощью BitLocker;
    - создает файлы журнала диска, используемые во время создания задания импорта;
    - помогает определить число дисков, необходимых для заданий экспорта.
    
> [!NOTE]
> Доступны две версии инструмента WAImportExport: версия 1 и версия 2. Мы рекомендуем использовать:
> - версию 1 для операций импорта и экспорта с хранилищем BLOB-объектов Azure; 
> - версию 2 для операций импорта данных в службу файлов Azure.
>
> Средство WAImportExport совместимо только с 64-разрядной операционной системой Windows. Поддерживаемые версии ОС приведены в [требованиях для службы "Импорт и экспорт Azure"](storage-import-export-requirements.md#supported-operating-systems).

- **Диски**. В центр обработки данных Azure можно переслать твердотельные накопители (SSD) или жесткие диски (HDD). При создании задания импорта отправляются диски, содержащие ваши данные. При создании задания экспорта в центр обработки данных Azure отправляются пустые диски. Типы дисков описываются в разделе [Supported hardware](storage-import-export-requirements.md#supported-hardware) (Поддерживаемое оборудование).

## <a name="how-does-importexport-work"></a>Как работает служба "Импорт и экспорт"?

Служба "Импорт и экспорт Azure" позволяет передавать данные в большие двоичные объекты Azure и службу файлов Azure путем создания заданий. Используйте портал Azure или REST API Azure Resource Manager для создания заданий. а каждое задание — с отдельной учетной записью хранения. 

Можно создавать задания импорта или экспорта. Задание импорта позволяет импортировать данные в большие двоичные объекты Azure или службу файлов Azure, а задание экспорта позволяет экспортировать данные из больших двоичных объектов Azure. Для выполнения задания импорта вы отправляете диски, содержащие ваши данные. При создании задания экспорта в центр обработки данных Azure вы отправляете пустые диски. В каждом случае можно отправить до десяти дисков на задание.

### <a name="inside-an-import-job"></a>Задание импорта

На высоком уровне задание импорта включает следующие действия:

1. Определение данных, которые будут импортироваться, необходимого количества дисков и расположения целевого большого двоичного объекта для данных в службе хранилища Azure.
2. Использование инструмента WAImportExport для копирования данных на диски. Шифрование дисков с помощью BitLocker.
3. Создание задания импорта в целевой учетной записи хранения на портале Azure. Передача файлов журналов для дисков.
4. Указание обратного адреса и номера учетной записи перевозчика для возврата дисков.
5. Отправка дисков по адресу, указанному при создании задания.
6. Обновите номер отслеживания доставки в сведениях о задании импорта и отправьте задание импорта.
7. Диски принимаются и обрабатываются центром обработки данных Azure.
8. Диски возвращаются с использованием учетной записи транспортного агента по обратному адресу, указанному в задании импорта.

> [!NOTE]
> Для поставки локальной (в пределах Страна/регион центра обработки данных) Поделитесь ими внутренних перевозчика 
>
> Для поставки за рубежом (за пределами Страна/регион центра обработки данных) Поделитесь ими международных перевозчика

 ![Рисунок 1. Процесс выполнения задания импорта](./media/storage-import-export-service/importjob.png)

Пошаговые инструкции по импорту данных:

- [Импорт данных в большие двоичные объекты Azure](storage-import-export-data-to-blobs.md)
- [Импорт данных в службу файлов Azure](storage-import-export-data-to-files.md)


### <a name="inside-an-export-job"></a>Задание экспорта

> [!IMPORTANT]
> Служба поддерживает экспорт данных только из больших двоичных объектов Azure. Экспорт данных из службы файлов Azure не поддерживается.

На высоком уровне задание экспорта включает следующие действия:

1. Определение данных для экспорта, необходимого числа дисков, исходных больших двоичных объектов или путей к контейнерам данных в хранилище BLOB-объектов.
3. Создание задания экспорта в исходной учетной записи хранения на портале Azure.
4. Указание исходных больших двоичных объектов или путей к контейнерам данных для экспорта.
5. Указание обратного адреса и номера учетной записи перевозчика для возврата дисков.
6. Отправка дисков по адресу, указанному при создании задания.
7. Обновите номер отслеживания доставки в сведениях о задании экспорта и отправьте задание экспорта.
8. Диски принимаются и обрабатываются центром обработки данных Azure.
9. Шифрование дисков с помощью BitLocker с использованием ключей с портала Azure.  
10. Диски возвращаются с использованием учетной записи транспортного агента по обратному адресу, указанному в задании импорта.

> [!NOTE]
> Для поставки локальной (в пределах Страна/регион центра обработки данных) Поделитесь ими внутренних перевозчика 
>
> Для поставки за рубежом (за пределами Страна/регион центра обработки данных) Поделитесь ими международных перевозчика
  
 ![Рисунок 2. Процесс выполнения задания экспорта](./media/storage-import-export-service/exportjob.png)

Пошаговые инструкции по экспорту данных см. в разделе [Использование службы "Импорт и экспорт Azure" для экспорта данных из хранилища BLOB-объектов Azure](storage-import-export-data-from-blobs.md).

## <a name="region-availability"></a>Доступность в регионах 

Служба "Импорт и экспорт Azure" поддерживает копирование данных в любые учетные записи хранения Azure и из них. Диски можно отправлять в одно из указанных ниже расположений. Если ваша учетная запись хранения Azure находится в расположении Azure, которое здесь не указано, то при создании задания будет указано альтернативное расположение для отправки.

### <a name="supported-shipping-locations"></a>Диски можно отправлять в следующие расположения:


|Страна (регион)  |Страна (регион)  |Страна (регион)  |Страна (регион)  |
|---------|---------|---------|---------|
|Восточная часть США    | Северная Европа        | Центральная Индия        |US Gov (Айова)         |
|Запад США     |Западная Европа         | Южная Индия        | Восток США, МО        |
|Восток США 2    | Восточная Азия        |  Западная Индия        | Центральная часть США, МО        |
|Западный регион США 2     | Юго-Восточная Азия        | Центральная Канада        | Восточный Китай         |
|Центральный регион США     | Восточная часть Австралии        | Восточная Канада        | Северный Китай        |
|Центрально-северная часть США     |  Юго-Восточная часть Австралии       | Южная часть Бразилии        | Южная часть Великобритании        |
|Центрально-южная часть США     | Западная часть Японии        |Центральная Корея         | Центральная Германия        |
|Западно-центральная часть США     |  Восточная часть Японии       | Правительство штата Вирджиния        | Северо-восточная Германия        |


## <a name="security-considerations"></a>Вопросы безопасности

Данные на диске должны быть зашифрованы с помощью технологии шифрования диска BitLocker. Это шифрование защитит данные во время пересылки.

Для заданий импорта диски шифруются двумя способами.  


- Можно задать параметр при использовании файла *dataset.csv* во время запуска инструмента WAImportExport в процессе подготовки диска. 

- Можно включить шифрование диска BitLocker вручную. Для этого следует указать ключ шифрования в файле *driveset.csv* во время запуска окна командной строки инструмента WAImportExport в процессе подготовки диска.


В случае экспорта после копирования данных на диски и перед их отправкой вам служба выполняет шифрование с помощью технологии BitLocker. Ключ шифрования предоставляется пользователю на портале Azure.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]


### <a name="pricing"></a>Цены

**Стоимости обработки дисков**

За обработку каждого диска в рамках задания импорта или экспорта взимается плата. Дополнительные сведения см. на странице [цен на службу "Импорт и экспорт Azure"](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Стоимость отправки**

При отправке дисков в Azure вы оплачиваете транспортные расходы перевозчика. Когда Майкрософт возвращает вам диск, стоимость обратной пересылки списывается с вашей учетной записи транспортного агента, указанной при создании задания.

**Плата за транзакции**

[Плата за транзакции хранилища класса Standard](https://azure.microsoft.com/pricing/details/storage/) применяются во время импорта, а также экспорт данных. Стандартный исходящий трафик взимается плата за также а также плата за транзакции хранилища при экспорте данных из хранилища Azure. Дополнительные сведения о расходы на исходящие данные см. в разделе [цены на передачу данных.](https://azure.microsoft.com/pricing/details/data-transfers/).



## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как использовать службу "Импорт и экспорт Azure" для выполнения следующих задач:
* [Импорт данных в большие двоичные объекты Azure](storage-import-export-data-to-blobs.md)
* [Экспорт данных из больших двоичных объектов Azure](storage-import-export-data-from-blobs.md)
* [Импорт данных в службу файлов Azure](storage-import-export-data-to-files.md)

