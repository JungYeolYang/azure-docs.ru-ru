---
title: Создание мобильного приложения для Android в службе мобильных приложений Azure | Документация Майкрософт
description: Изучите этот учебник, чтобы начать работу с серверными частями мобильных приложений Azure для разработки программ на платформе Android.
services: app-service\mobile
documentationcenter: android
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: conceptual
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 805868617fe7161159c72ba53ac0c94247722ac9
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62113455"
---
# <a name="create-an-android-app"></a>Создание приложения Android
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Обзор
В этом учебнике рассказывается, как добавить облачную серверную службу в мобильное приложение для платформы Android с помощью серверной части мобильного приложения Azure.  Мы создадим серверную часть мобильного приложения и простое приложение *списка дел* (на платформе Android), которое хранит свои данные в Azure.

Выполнение инструкций из этого учебника необходимо для работы с другими учебниками по Android, посвященными использованию функции мобильных приложений в службе приложений Azure.

## <a name="prerequisites"></a>Технические условия
Для работы с этим учебником требуется:

* [Средства разработчика Android](https://developer.android.com/sdk/index.html), которые включают интегрированную среду разработки Android Studio и новейшую платформу Android.
* Пакет SDK для мобильных приложений Android в Azure, который автоматически включается как часть скачиваемого вами ознакомительного проекта.
* [Активная учетная запись Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Создание серверной части мобильного приложения Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Настройка серверного проекта
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a>Скачивание и запуск приложения для Android
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
