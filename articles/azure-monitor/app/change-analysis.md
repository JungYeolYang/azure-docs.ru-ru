---
title: Приложения Azure Monitor изменить анализ — обнаруживать изменения, которые могут повлиять на проблемах активного сайта/простоях приложения Azure Monitor изменить analysis | Документация Майкрософт
description: Устранение неполадок активного сайта приложения в службах приложений Azure с помощью функций анализа изменений приложения Azure Monitor
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: b04bd57c66b52e9a2c14d43c9e89d7c54fc48ae2
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415840"
---
# <a name="azure-monitor-application-change-analysis-public-preview"></a>Анализ изменений приложения Azure Monitor (Предварительная версия)

В случае динамической/проблема площадки важно быстро определить основную причину. Standard, решения для мониторинга может помочь быстро выявить, что возникла проблема, и часто даже компонент, который не удается. Но это не всегда приведет к объяснение интерпретации для Почему происходит сбой. Сайт работал пяти минут назад, теперь он нарушена. Что изменилось за последние пять минут? Это вопрос, нового компонента Azure Monitor, анализ изменений приложения предназначен отвечать. Основываясь на мощь [график ресурсов Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview) анализ изменений приложения позволяет отслеживать изменения приложения службы приложений Azure для повышения наблюдаемость и снижения MTTR (среднее время для восстановления).

> [!IMPORTANT]
> Анализ изменений приложения Azure Monitor в настоящее время находится в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-do-i-use-application-change-analysis"></a>Как использовать анализ изменений приложения?

Анализ изменений приложения Azure Monitor в настоящее время встроена в самостоятельная **Диагностика и решение проблем** возникли, который может быть организован **Обзор** части службы приложений Azure приложение:

![Снимок экрана: служба приложений Azure Общие сведения о странице с красные поля вокруг кнопки "Обзор" и диагностировать и решить проблемы кнопки](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis"></a>Включить анализ изменений

1. Выберите **доступность и производительность**

    ![Снимок экрана доступности и производительности, устранение неполадок](./media/change-analysis/availability-and-performance.png)

2. Нажмите кнопку **сбой приложения происходит** плитку.

   ![Снимок экрана с плиткой сбоев приложения](./media/change-analysis/application-crashes-tile.png)

3. Чтобы включить **анализа изменений** выберите **включить сейчас**.

   ![Снимок экрана доступности и производительности, устранение неполадок](./media/change-analysis/application-crashes.png)

4. Вступили преимуществами полной изменить набор функций для анализа **изменить Analysis**, **поиск изменений кода**, и **Always on** для **на** и выберите **Сохранить**.

    ![Снимок экрана: Включение службы приложений Azure изменить анализа пользовательского интерфейса](./media/change-analysis/change-analysis-on.png)

    Если **анализа изменений** — включена, можно для обнаружения изменений на уровне ресурсов. Если **поиск изменений кода** будет включен, будет также см. в разделе файлов развертывания и изменения конфигурации сайта. Включение **Always on** оптимизирует производительность просмотра изменений, но может повлечь дополнительные затраты с точки зрения выставления счетов.

5.  После того как все, что включен, выбрав **Диагностика и решение проблем** > **доступности и производительности** > **сбой приложения происходит** позволит вам получить доступ к процедуры анализа изменений. Граф будет summerize тип изменений, произошедших со временем вместе со сведениями об этих изменениях:

     ![Снимок экрана: представление различий для изменений](./media/change-analysis/change-view.png)

## <a name="troubleshooting"></a>Устранение неполадок

### <a name="unable-to-fetch-change-analysis-information"></a>Не удалось получить сведения об анализе изменений.

Это временная проблема с текущей предварительной версии адаптации. Обходной путь состоит из установки скрытый тег для веб-приложения, а затем обновите страницу.

   ![Снимок экрана скрытые изменение тега](./media/change-analysis/hidden-tag.png)

Чтобы задать скрытый тег, с помощью PowerShell:

1. Откройте Azure Cloud Shell:

    ![Снимок экрана: изменение Azure Cloud Shell](./media/change-analysis/cloud-shell.png)

2. Измените тип оболочки PowerShell:

   ![Снимок экрана: изменение Azure Cloud Shell](./media/change-analysis/choose-powershell.png)

3. Выполните следующую команду:

```powershell
$webapp=Get-AzWebApp -Name <name_of_your_webapp>
$tags = $webapp.Tags
$tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
```

> [!NOTE]
> После добавления скрытых тега может по-прежнему необходимо изначально ожидания на 4 часа, чтобы иметь возможность сначала просмотреть изменения. Это вызвано freqeuncy 4 часов, используемых службой анализа изменений для проверки веб-приложения, при этом ограничивая влияние на производительность сканирования.

## <a name="next-steps"></a>Дальнейшие действия

- Улучшения мониторинга служб приложений Azure [включить функции Application Insights](azure-web-apps.md) из Azure Monitor.
- Углубление изучения [график ресурсов Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview) удовлетворение приложения Azure Monitor power изменить анализа. 
