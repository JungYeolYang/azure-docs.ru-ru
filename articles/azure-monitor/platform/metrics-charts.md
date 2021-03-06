---
title: Расширенные возможности обозревателя метрик Azure
description: Дополнительные сведения о расширенных функциях обозреватель метрик Azure Monitor
author: lingliw
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/12/19
ms.author: v-lingwu
ms.subservice: metrics
ms.openlocfilehash: 67e4281b24a7489cf202d82bdddbe99992aac095
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60256786"
---
# <a name="advanced-features-of-azure-metrics-explorer"></a>Расширенные возможности обозревателя метрик Azure

> [!NOTE]
> В этой статье предполагается, что вы знакомы с основными возможностями обозревателя метрик. Если вы новый пользователь и хотите узнать, как создать свою первую диаграмму метрики, см. в разделе [Приступая к работе с обозревателем метрик Azure](metrics-getting-started.md).

## <a name="metrics-in-azure"></a>Метрики в Azure

[Метрики в Azure Monitor](data-platform-metrics.md) — это ряд измеренных значений и счетчиков, которые собираются и сохраняются в течение определенного времени. Существуют стандартные метрики (или "метрики платформы") и настраиваемые метрики. Стандартные метрики предоставляет платформа Azure. Стандартные метрики показывают работоспособность и статистику потребления ресурсов Azure. В то время как пользовательские метрики отправляются в Azure приложения используют [API Application Insights для пользовательских событий и метрик](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics), [расширения Windows Azure Diagnostics (WAD)](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview), или с помощью [Azure Мониторинг REST API](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-store-custom-rest-api).

## <a name="create-views-with-multiple-metrics-and-charts"></a>Создание представления с помощью нескольких метрики и диаграммы

Можно создавать диаграммы, построения несколько строк метрики или отображение нескольких диаграмм метрик за один раз. Эта функция позволяет:

- коррелировать связанные метрики на одной диаграмме, чтобы увидеть как одно значение связан с другим
- Отображение метрик с различными единицами измерения в непосредственной близости
- визуально статистической обработки и сравнения метрики из нескольких ресурсов

Например если у вас есть 5 учетных записей хранения, и вы хотите узнать, сколько всего места потребляется между ними, можно создать диаграмму с областями (с накоплением), показывающий лицом и сумма всех значений в определенные моменты времени.

### <a name="multiple-metrics-on-the-same-chart"></a>Нескольких метрик на одной диаграмме

Во-первых, [создать диаграмму](metrics-getting-started.md#create-your-first-metric-chart). Нажмите кнопку **добавить метрику** и повторите шаги, чтобы добавить другую метрику на одной диаграмме.

   > [!NOTE]
   > Как правило, метрики с различными единицами измерения (например, миллисекундами и килобайтами) или значительно отличающимися масштабами на одной диаграмме не требуются. Вместо этого следует использовать несколько диаграмм. Нажмите кнопку "Добавить диаграмму", чтобы создать несколько диаграмм в обозревателе метрик.

### <a name="multiple-charts"></a>Несколько диаграмм

Нажмите кнопку **добавить диаграмму** и создайте другую диаграмму с другую метрику.

### <a name="order-or-delete-multiple-charts"></a>Закажите или удалить несколько диаграмм

Чтобы упорядочить или удалить несколько диаграмм, щелкните многоточие ( **...**  ) символ, чтобы открыть меню диаграммы и выберите нужный пункт меню из **вверх**, **вниз**, или **удалить**.

## <a name="apply-filters-to-charts"></a>Применение фильтров к диаграммам

К диаграммам, на которых показаны метрики с измерениями, можно применить фильтры. Например, если метрика "Счетчик транзакций" имеет измерение "Тип ответа", которое указывает, успешен ли ответ от транзакции, тогда при фильтрации по этому измерению будет создана диаграмма только для успешных (или только неудачных) транзакций. 

### <a name="to-add-a-filter"></a>Добавление фильтра

1. Выберите **Добавить фильтр** над диаграммой.

2. Выберите измерение (свойство), которое необходимо отфильтровать.

   ![изображение метрики](./media/metrics-charts/00006.png)

3. Выберите значения измерения, которые нужно включить при создании диаграммы (в этом примере показаны отфильтрованные успешные транзакции с хранилищем):

   ![изображение метрики](./media/metrics-charts/00007.png)

4. Выбрав значения фильтра, щелкните за пределами средства выбора фильтра, чтобы закрыть его. Теперь на диаграмме показано, сколько транзакций с хранилищем не удалось выполнить:

   ![изображение метрики](./media/metrics-charts/00008.png)

5. Шаги 1–4 можно повторить, чтобы применить несколько фильтров к одной диаграмме.



## <a name="apply-splitting-to-a-chart"></a>Применить разделение в диаграмму

Метрики можно разделить по измерениям, чтобы визуализировать сравнения между различными сегментами метрики и идентифицировать внешние сегменты измерения.

### <a name="apply-splitting"></a>Применение разделения

1. Щелкните **Применить разделение** над диаграммой.
 
   > [!NOTE]
   > Разделение не может использоваться с типами диаграмм, которые имеют несколько метрик. Кроме того может иметь несколько фильтров, но только одно измерение разбиения, применяется к одной диаграмме.

2. Выберите измерения, по которому необходимо сегментировать диаграммы:

   ![изображение метрики](./media/metrics-charts/00010.png)

   Теперь на диаграмме отображаются несколько строк, по одной для каждого сегмента измерения:

   ![изображение метрики](./media/metrics-charts/00012.png)

3. Щелкните за пределами **селектора группирования**, чтобы закрыть его.

   > [!NOTE]
   > С помощью фильтрации и разделения одного измерения можно скрыть сегменты, которые не имеют значения для сценария, и облегчить анализ диаграмм.

## <a name="lock-boundaries-of-chart-y-axis"></a>Фиксирование границ диаграммы по оси Y

Фиксирование диапазона оси Y приобретает значение, если в диаграмме отображаются небольшие колебания больших значений. 

Например, когда объем успешных запросов снижается с 99,99 % до 99,5 %, это может представлять собой значительное снижение качества обслуживания. Тем не менее будет трудно или даже невозможно заметить маленькое колебание числового значения, если заданы параметры диаграммы по умолчанию. В этом случае можно зафиксировать самую нижнюю границу диаграммы на уровне 99 %, в результате чего небольшой спад будет более очевидным. 

Другой пример — колебание объема доступной памяти, когда значение технически никогда не достигнет значения 0. Благодаря фиксированию диапазона на уровне большего значения легче обнаружить уменьшение объема доступной памяти. 

Для управления диапазоном оси Y используйте меню диаграммы "..." и выберите **Изменить диаграмму**, чтобы получить доступ к дополнительным параметрам диаграммы. Измените значения в разделе "Диапазон оси Y" или нажмите кнопку **Автоматически**, чтобы вернуть значения по умолчанию.

![изображение метрики](./media/metrics-charts/00014-manually-set-granularity.png)

> [!WARNING]
> При фиксировании границ оси Y диаграмм, отслеживающих различные количества или суммы за период времени (и таким образом использующих количество, сумму, минимальные или максимальные агрегирования) обычно требуется указать степень детализации фиксированного времени, а не полагаться на автоматические значения по умолчанию. Это необходимо, так как значения на диаграммах изменяются из-за автоматического изменения степени детализации, когда пользователь изменяет размер окна браузера или переходит от одного разрешения экрана к другому. Полученное изменение степени детализации времени влияет на внешний вид диаграммы таким образом, что текущее выделение диапазона оси Y становится недопустимым.

## <a name="pin-charts-to-dashboards"></a>Закрепление диаграмм на панелях мониторинга

После настройки диаграмм их можно добавить на панели мониторинга, чтобы их можно было просмотреть повторно, возможно, в контексте другой телеметрии мониторинга или совместно с другими участниками команды.

Чтобы закрепить настроенную диаграмму на панели мониторинга, сделайте следующее:

После настройки диаграммы щелкните меню **Chart Actions** (Действия на диаграмме) в правом верхнем углу диаграммы и выберите **Закрепить на панели мониторинга**.

![изображение метрики](./media/metrics-charts/00013.png)

## <a name="create-alert-rules"></a>Создание правил генерации оповещений

Условия, которые вы задали для визуализации метрик, можно использовать как основу для правила генерации оповещений на основе метрик. В новом правиле генерации оповещений будет указан целевой ресурс, метрика, разделение и измерения фильтров из диаграммы. Вы сможете изменить эти параметры позднее в области создания правила генерации оповещений.

### <a name="to-create-a-new-alert-rule-click-new-alert-rule"></a>Чтобы создать правило генерации оповещений, щелкните **Новое правило генерации оповещений**.

![Кнопка "Создать правило генерации оповещений", выделенная красным цветом](./media/metrics-charts/015.png)

Откроется панель создания правила генерации оповещений с измерениями базовой метрики из предварительно заполненной диаграммы, чтобы вам было проще создавать настраиваемые правила генерации оповещений.

![Создать правило генерации оповещений](./media/metrics-charts/016.png)

Ознакомьтесь с этой [статьей](alerts-metric.md), чтобы узнать больше о настройке оповещений на основе метрик.

## <a name="troubleshooting"></a>Устранение неполадок

*Данные не отображаются на диаграмме.*

* Фильтры применяются ко всем диаграммам на панели. Убедитесь, что, рассматривая какую-либо диаграмму, вы не задали фильтр, исключающий все данные на другой диаграмме.

* Если вы хотите задать разные фильтры для различных диаграмм, создайте их на разных колонках и по отдельности сохраните в избранное. Если необходимо, вы можете закрепить их на панели мониторинга, чтобы их можно было видеть вместе.

* Если сегментировать диаграмму по свойству, которое не определено в метрике, то на диаграмме ничего не отобразится. Попробуйте очистить сегментацию (разбиение) или выберите другое свойство.

## <a name="next-steps"></a>Дальнейшие действия

  Дополнительные сведения о рекомендациях по созданию готовых к работе панелей мониторинга с метриками см. в статье [Create custom KPI dashboards using Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) (Создание пользовательских панелей мониторинга KPI с помощью Azure Application Insights).
