---
ms.openlocfilehash: 53c683dacbb3b94e34bd8662743673c3a0490d94
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119965"
---
#### <a name="prerequisites"></a>Технические условия
* Учетная запись Azure. Вы можете создать [бесплатную учетную запись](https://azure.microsoft.com/free).
* Учетная запись [Microsoft Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx). 

Прежде чем использовать свою учетную запись Dynamics CRM Online в приложении логики, необходимо авторизовать приложение логики для подключения к этой учетной записи. Это можно легко сделать из приложения логики на портале Azure. 

Авторизуйте приложение логики для подключения к учетной записи CRM Online, выполнив следующее.

1. Создайте приложение логики. В конструкторе приложений логики в раскрывающемся списке выберите параметр **Показать API, управляемые Майкрософт**, а затем введите в поле поиска "dynamics". Выберите один из триггеров или одно из действий.  
   ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Если вы ранее не создавали подключения к Dynamics, то вам будет предложено ввести учетные данные Dynamics.  
   ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Выберите **Войти** и введите имя пользователя и пароль. Выберите **Войти**. 
   
    Эти учетные данные используются для авторизации приложения логики, чтобы оно могло подключиться и получить доступ к данным в вашей учетной записи Dynamics. 
4. Обратите внимание, что было создано подключение. Теперь перейдите к другим действиям в приложении логики.  
   ![](./media/connectors-create-api-crmonline/dynamics-properties.png)

