---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: cf39baf34096691144181332566cf567ebc02310
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925616"
---
1. Установите удаленное подключение рабочего стола на компьютере, где выполняется сервер обработки. 
2. Запустите cspsconfigtool.exe, чтобы запустить средство настройки сервера Azure Site Recovery процесса.
    - Средство запускается автоматически при первом входе на сервер обработки.
    - Если мастер не запустился, щелкните соответствующий ярлык на рабочем столе.

3. В **сервер конфигурации, полное доменное имя или IP-адрес**, укажите имя или IP-адрес сервера конфигурации, с которым регистрируется на сервере обработки.
4. В **порт сервера конфигурации**, убедитесь, что 443. Это порт, прослушиваемый сервером конфигурации для запросов.
5. В **парольную фразу для подключения**, укажите парольную фразу, созданную при настройке сервера конфигурации. Чтобы найти парольная фраза:
    -  На сервере конфигурации, перейдите к папке установки Site Recovery **\home\svssystems\bin\**. 
    - Выполните следующую команду: **genpassphrase.exe.n**. Это показывает расположение файла с парольной фразой, который затем можно обратить внимание.

6. В **порт передачи данных**, оставьте значение по умолчанию, если вы не указали другой порт.

7. Нажмите кнопку **Сохранить** сохранить настройки и регистрация сервера обработки.

    
    ![Регистрация сервера обработки](./media/site-recovery-vmware-register-process-server/register-ps.png)
