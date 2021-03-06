---
title: Интерфейсы API для выставления счетов Azure корпоративным клиентам | Документация Майкрософт
description: Узнайте, как с помощью интерфейсов API отчетов для корпоративных клиентов Azure извлекать данные о потреблении программным способом.
services: ''
documentationcenter: ''
author: mumami
manager: mumami
editor: ''
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: erikre
ms.openlocfilehash: 52612419599ef69e7476c660b52f9e6e36946825
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60615573"
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Обзор API- интерфейсов отчетов для корпоративных клиентов
Интерфейсы API отчетов позволяют корпоративным клиентам Azure извлекать данные о потреблении и выставлении счетов программным способом и передавать их в предпочитаемые средства анализа данных. Клиенты, которые принадлежат к типу Enterprise, подписали [Соглашение Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) с Azure и тем самым приняли согласованные денежные обязательства, и получили доступ к пользовательским тарифам на ресурсы Azure.

## <a name="enabling-data-access-to-the-api"></a>Включение доступа данных к API
* **Создание или получение ключа API.** Войдите на корпоративный портал и последовательно выберите "Отчеты" > "Скачать сведения об использовании" > API Access Key (Ключ доступа к API), чтобы создать или получить ключ API.
* **Передача ключей в API**. Для аутентификации и авторизации необходимо передать ключ API для каждого вызова. В заголовки HTTP необходимо добавить следующее свойство:

|Ключ заголовка запроса | Value|
|-|-|
|Авторизация| Укажите значение в следующем формате: **bearer {API_KEY}** <br/> Пример: bearer eyr....09| 

## <a name="consumption-apis"></a>Интерфейсы API потребления
[Здесь](https://consumption.azure.com/swagger/ui/index) вы можете найти конечную точку Swagger для интерфейсов API, описанных ниже. С ее помощью можно упростить самоанализ API и создать клиентские пакеты SDK, используя [AutoRest](https://github.com/Azure/AutoRest) или [Swagger CodeGen](https://swagger.io/swagger-codegen/). С 1 мая 2014 г. данные доступны через этот API. 

* **Баланс и сводка**. [Интерфейс API для управления балансом и просмотра сводки](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) отображает ежемесячную сводку о состоянии баланса, новых покупках, расходах на службу Azure Marketplace, корректировках и оплате за избыток.

* **Сведения об использовании**. [Интерфейс API сведений об использовании](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) предоставляет сводку об израсходованных объемах и предполагаемых расходах для каждой регистрации с разбивкой по дням. Результаты также содержат сведения об экземплярах, метриках и отделах. Запрашивать данные в API можно по расчетному периоду или по дате начала и окончания. 

* **Платежи в Marketplace**. [Интерфейс API платежей в Marketplace](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) отображает сводку о расходах в Marketplace с разбивкой по дням. Данные основаны на фактическом использовании и отображаются для указанного расчетного периода или дат начала и окончания (однократные сборы не включаются).

* **Прейскурант**. [API прейскуранта](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) предоставляет соответствующий тариф для каждой метрики в отдельной регистрации и за определенный расчетный период. 

## <a name="data-freshness"></a>Актуальность данных
Теги ETag возвращаются в ответе API, упомянутого выше. Изменение ETag означает, что данные обновлены.  В последующих вызовах этого же API с теми же параметрами передайте записанный ETag с ключом If-None-Match в заголовке HTTP-запроса. Код состояния будет содержать ответ NotModified, если данные больше не обновлялись, и данные не будут возвращены. API будет возвращать полный набор данных за требуемый период при каждом изменении ETag.

## <a name="helper-apis"></a>Интерфейсы API вспомогательного приложения
 **Список расчетных периодов**. [API расчетных периодов](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) возвращает список расчетных периодов, которые содержат данные о потреблении для определенной регистрации, приведенные в обратном хронологическом порядке. Каждый период содержит свойство, указывающее на маршрут API к четырем наборам данных: BalanceSummary, UsageDetails, Marketplace Charges и Price Sheet.


## <a name="api-response-codes"></a>Коды ответов API   
|Код состояния отклика|Сообщение|ОПИСАНИЕ|
|-|-|-|
|200| OК|Без ошибок|
|401| Не авторизовано| Ключ API не найден, недопустимый формат, срок действия истек и т. д.|
|404| Рекомендации недоступны| Не найдена конечная точка отчетов|
|400| Ошибка запроса| Недопустимые параметры — диапазоны дат, числа EA и т. д.|
|500| Ошибка сервера| Непредвиденная ошибка при обработке запроса| 









