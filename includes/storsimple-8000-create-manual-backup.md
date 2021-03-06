---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 4fc92931979aa367bdead435c3d6fd758d66a397
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60819166"
---
#### <a name="to-create-a-manual-backup"></a>Создание резервной копии вручную

1. В службе диспетчера устройств StorSimple выберите **Устройства**. Выберите устройство в таблице со списком устройств. Щелкните **Параметры > Управление > Политики архивации**.

2. В колонке **Политики архивации** приведен список всех политик резервного копирования в табличном формате, включая политику для тома, резервную копию которого требуется создать. Выберите политику, связанную с томом, резервную копию которого требуется создать, и щелкните его правой кнопкой мыши, чтобы вызвать контекстное меню. В раскрывающемся списке выберите **Выполнить моментальную архивацию**.

    ![Создание резервной копии вручную](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. В колонке **Выполнить моментальную архивацию** сделайте следующее:

    1. В раскрывающемся меню выберите требуемый **Тип моментального снимка** — **Локальный** или **Облачный**. Выбирайте локальный моментальный снимок для быстрой архивации или восстановления, а облачный — для обеспечения устойчивости данных.

        ![Создание резервной копии вручную](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. Щелкните **ОК** для запуска задания по созданию моментального снимка. После успешного создания задания в верхней части страницы отобразится уведомление.

        ![Создание резервной копии вручную](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. Чтобы отслеживать ход выполнения задания, щелкните это уведомление. Вы перейдете к колонке **заданий**, в которой можно просмотреть ход выполнения задания.


5. После выполнения задания архивации перейдите на вкладку **Каталог резервного копирования** .

6. С помощью фильтров отобразите нужное устройство, политику резервного копирования и диапазон времени. Резервная копия должна появиться в списке резервных наборов данных, которые отображаются в каталоге.

