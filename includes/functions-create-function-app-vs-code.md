---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: 79dbee33928fbc7560d0ea27be3af25cc510e996
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2018
ms.locfileid: "52641897"
---
## <a name="create-an-azure-functions-project"></a>Создание проекта Функций Azure

С помощью шаблона проекта Функции Azure в Visual Studio Code можно создать проект и затем опубликовать его в виде приложения-функции в Azure. Приложение-функция позволяет группировать функции в логические единицы для развертывания и совместного использования ресурсов, а также управления ими.

1. В Visual Studio Code выберите логотип Azure, чтобы отобразить область **Azure: Functions** (Azure: Функции), после чего щелкните значок Create New Project (Создать проект).

    ![Создание проекта приложения-функции](./media/functions-create-function-app-vs-code/create-function-app-project.png)

1. Выберите расположение для рабочей области проекта и нажмите **Select** (Выбрать).

    > [!NOTE]
    > Рассматриваемый в этой статье пример выполняется вне рабочей области. В этом случае не нужно указывать папку проекта, которая является частью рабочей области.

1. Выберите язык для проекта приложения-функции. В этом примере используется JavaScript.
    ![Выбор языка проекта](./media/functions-create-function-app-vs-code/create-function-app-project-language.png)

1. Когда появится запрос, выберите **Add to workspace** (Добавить в рабочую область).

Редактор Visual Studio Code создаст проект приложения-функции в новой рабочей области. Проект будет содержать файлы конфигурации [host.json](../articles/azure-functions/functions-host-json.md) и [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file), а также файлы проекта для указанного языка. В папке проекта также появится новый репозиторий Git.