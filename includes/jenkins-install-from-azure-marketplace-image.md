---
description: включение файла
author: tomarcher
manager: rloutlaw
ms.service: multiple
ms.workload: web
ms.devlang: na
ms.topic: include
ms.date: 03/12/2018
ms.author: tarcher
ms.custom: Jenkins
ms.openlocfilehash: 5439de30b02b0ce05853c8112f9e29239743ef98
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58124633"
---
1. В браузере откройте [образ Azure Marketplace для Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Выберите **Загрузить сейчас**.

    ![Выберите "Загрузить сейчас", чтобы начать процесс установки образа Jenkins Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Просмотрев информацию о ценах и условиях, выберите **Продолжить**.

    ![Сведения о ценах и условиях образов Jenkins Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Чтобы настроить сервер Jenkins на портале Azure, выберите **Создать**. 

    ![Установите образ Jenkins Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. На вкладке **Основные сведения** укажите следующие значения:

   - **Name** (Имя). Введите `Jenkins`.
   - **Имя пользователя.** Введите имя пользователя для входа на виртуальную машину, на которой запущен сервер Jenkins. Имя пользователя должно соответствовать [особым требованиям](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
   - Для параметра **Тип проверки подлинности** выберите значение **Открытый ключ SSH**.
   - **Открытый ключ SSH.** Скопируйте и вставьте в это поле открытый ключ RSA в однострочном формате (начиная с `ssh-rsa`) или в формате многострочного текста PEM. Ключи SSH можно создать с помощью ssh-keygen (в Linux и macOS) или PuTTYGen (в Windows). Дополнительные сведения см. в статье об [использовании ключей SSH с Windows в Azure](/azure/virtual-machines/linux/ssh-from-windows).
   - **Подписка**. Выберите подписку Azure, в которой необходимо установить Jenkins.
   - **Группа ресурсов**. Выберите **Создать новый** и введите имя для группы ресурсов, которая используется как логический контейнер для коллекции ресурсов, которые составляют установку Jenkins.
   - **Расположение**. Выберите **Восточная часть США**.

     ![Введите сведения о проверке подлинности и группе ресурсов для Jenkins на вкладке "Основные сведения".](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Щелкните **ОК**, чтобы открыть вкладку **Дополнительные параметры**. 

1. На вкладке **Дополнительные параметры** укажите следующие значения:

   - **Размер**. Выберите соответствующий вариант для виртуальной машины Jenkins.
   - **Тип диска виртуальной машины**. Выберите тип диска, HDD (жесткий диск) или SSD (твердотельный накопитель), чтобы указать тип диска хранилища, разрешенный для виртуальных машин Jenkins.
   - **Виртуальная сеть** (необязательно) — выберите **виртуальную сеть**, чтобы применить ее вместо настроек по умолчанию.
   - **Подсети** — выберите **подсети**, проверьте информацию о них и щелкните **ОК**.
   - **Общедоступный IP-адрес**. Для имени IP-адреса по умолчанию присваивается имя Jenkins, указанное на предыдущей странице с суффиксом IP. Вы можете изменить это значение по умолчанию.
   - **Метка доменного имени**. Укажите значение полного URL-адреса для виртуальной машины Jenkins.
   - **Тип выпуска Jenkins** — выберите нужный тип выпуска (доступны следующие варианты: `LTS`, `Weekly build` или `Azure Verified`). Варианты `LTS` и `Weekly build` описаны в статье [о строке выпуска Jenkins LTS](https://jenkins.io/download/lts/). Вариант `Azure Verified` указывает на [версию Jenkins LTS](https://jenkins.io/download/lts/), которая проверена на совместимость с Azure. 
   - **Тип JDK**. Требуемый тип JDK для установки. По умолчанию используются протестированные и сертифицированные сборки Zulu OpenJDK.

     ![Введите параметры виртуальной машины Jenkins на вкладке "Параметры".](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Щелкните **ОК**, чтобы открыть вкладку **Параметры интеграции**.

1. На вкладке **Параметры интеграции** укажите следующие значения:

    - **Субъект-служба** — добавленный в Jenkins субъект-служба применяется как учетные данные для аутентификации в Azure. Вариант `Auto` означает, что этот субъект будет создан службой MSI (управляемые удостоверения службы). Вариант `Manual` означает, что вы сами создадите этот субъект. 
        - **Идентификатор приложения** и **Секрет** — если для параметра **Субъект-служба** выбран вариант `Manual`, то для этого субъекта-службы нужно указать `Application ID` и `Secret`. Когда вы [создаете субъект-службу](/cli/azure/create-an-azure-service-principal-azure-cli), учитывайте, что по умолчанию назначается роль **участника**, которой вполне достаточно для работы с ресурсами Azure.
    - **Включить агенты облака** — укажите облачный шаблон по умолчанию для агентов, где `ACI` обозначает экземпляр контейнера Azure, а `VM` относится к виртуальным машинам. Также здесь можно указать `No`, если вы не хотите включать агент облака.

1. Нажмите кнопку **ОК**, чтобы перейти на вкладку **Сводка**.

1. Когда отобразится вкладка **Сводка**, проверьте введенные данные. Когда в верхней части вкладки появится сообщение **Проверка пройдена**, нажмите кнопку **ОК**. 

     ![Отобразится вкладка "Сводка", и будет выполнена проверка выбранных параметров.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Когда отобразится вкладка **Создание**, выберите **Создать**, чтобы создать виртуальную машину Jenkins. Когда сервер будет готов, отобразится уведомление на портале Azure.

     ![Уведомление о готовности Jenkins.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)
