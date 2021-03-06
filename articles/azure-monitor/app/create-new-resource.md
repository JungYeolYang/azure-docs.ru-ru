---
title: Создание ресурса Azure Application Insights | Документация Майкрософт
description: Вручную настройте мониторинг Application Insights для нового работающего приложения.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: mbullwin
ms.openlocfilehash: 5daf0944212dc4b8040a39e6efbf5bb25f7f39f0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60901818"
---
# <a name="create-an-application-insights-resource"></a>Создание ресурса Application Insights
В Azure Application Insights данные о приложении отображаются в *ресурсе* Microsoft Azure. Таким образом, создание ресурса является частью [настройки Application Insights для мониторинга нового приложения][start]. Во многих случаях создать ресурс можно автоматически с помощью IDE. Однако в некоторых случаях создавать ресурс необходимо вручную. Например, чтобы иметь отдельные ресурсы для сборок разработки и производственных сборок приложения.

После создания ресурса можно получить его ключ инструментирования и использовать этот ключ для настройки пакета SDK в приложении. Ключ ресурса позволяет связать данные телеметрии с ресурсом.

## <a name="sign-up-to-microsoft-azure"></a>Регистрация в Microsoft Azure
Если у вас еще нет [учетной записи Майкрософт, получите ее сейчас](https://live.com). (Если вы используете такие службы, как Outlook.com, OneDrive, Windows Phone или XBox Live, значит, у вас уже есть учетная запись Майкрософт.)

Вам также потребуется подписка [Microsoft Azure](https://azure.com). Если у вашей группы или организации есть подписка Azure, владелец может добавить вас в нее с помощью вашей учетной записи Windows Live ID. Плата взимается только за используемый объем, а базовый план по умолчанию предусматривает бесплатное использование определенного объема в экспериментальных целях.

Если у вас есть доступ к подписке, войдите в Application Insights по адресу [https://portal.azure.com](https://portal.azure.com) с помощью Live ID.

## <a name="create-an-application-insights-resource"></a>Создание ресурса Application Insights
Перейдите по адресу [portal.azure.com](https://portal.azure.com)и добавьте новый ресурс Application Insights.

![Нажмите "Создать" и "Application Insights"](./media/create-new-resource/01-new.png)

* **Тип приложения** определяет содержимое колонки "Обзор" и свойства, доступные в [обозревателе метрик][metrics]. Если тип приложения не отображается, выберите пункт "Общие".
* **Подписка** представляет собой учетную запись для оплаты в Azure.
* **Группа ресурсов** – удобный способ для управления свойствами наподобие контроля доступа. Если вы уже создали другие ресурсы Azure, можно поместить новый ресурс в ту же группу.
* **Расположение** – это место, в котором мы храним ваши данные.
* Установив флажок **Закрепить на панели мониторинга**, вы сможете поместить плитку для быстрого доступа к ресурсу на главную страницу Azure. (рекомендуется).

После создания приложения откроется новая колонка. В этой колонке будут представлены данные о производительности и использовании приложения. 

Чтобы перейти сюда после следующего входа в Azure,используйте плитку быстрого доступа на начальной доске (на начальном экране). Ее также можно открыть, щелкнув «Обзор».

## <a name="copy-the-instrumentation-key"></a>Копирование ключа инструментирования
Ключ инструментирования идентифицирует созданный вами ресурс. Он должен указываться в SDK.

![Щелкните Essentials, выделите ключ инструментирования и нажмите клавиши CTRL + C.](./media/create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>Установка пакета SDK в приложении
Установите пакет SDK Application Insights в приложении. Выполнение этого шага зависит от типа приложения. 

Используйте ключ инструментирования для настройки [пакета SDK, который можно установить в приложении][start].

Пакет SDK включает стандартные модули, которые отправляют данные телеметрии, не требуя написания кода. Для более подробного отслеживания действий пользователей или диагностики неполадок отправляйте собственные данные телеметрии, [используя API][api].

## <a name="monitor"></a>Просмотр данных телеметрии
Закройте колонку быстрого доступа, чтобы вернуться к колонке приложения на портале Azure.

Щелкните элемент "Поиск", чтобы открыть [Diagnostic Search][diagnostic] (Поиск по журналу диагностики), где отображаются первые события. 

Нажмите кнопку **Обновить** через несколько секунд, если ожидаете дополнительные данные.

## <a name="creating-a-resource-automatically"></a>Автоматическое создание ресурса
Вы можете написать [Сценарий PowerShell](../../azure-monitor/app/powershell.md) для автоматического создания ресурса.

## <a name="next-steps"></a>Дальнейшие действия
* [Создание панели мониторинга](../../azure-monitor/app/app-insights-dashboards.md)
* [Поиск по журналу диагностики](../../azure-monitor/app/diagnostic-search.md)
* [Изучение метрик](../../azure-monitor/app/metrics-explorer.md)
* [Написание запросов аналитики](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[start]: ../../azure-monitor/app/app-insights-overview.md

