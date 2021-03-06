---
title: Понимать данные подробные сведения об использовании и затратах | Документация Майкрософт
description: Узнайте, как читать и понимать данные подробные сведения об использовании и затратах
author: bandersmsft
manager: micflan
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/24/2019
ms.author: banders
ms.openlocfilehash: 9ff9b6b5313026d2102b98659183fa97c6a5ef84
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64683984"
---
# <a name="understand-the-terms-in-your-azure-usage-and-charges-file"></a>Значение терминов в файле Azure об использовании и затратах

Подробный файл об использовании и затратах содержит ежедневные об оплате за использование, на основе согласованного тарифы, покупки (например, резервирования, плата Marketplace) и возмещения за указанный период.
Плата не будут содержать деньги на счете, налоги, или другие расходы и скидки.
Следующие таблицы помогут какие входит плата для каждого типа счета.

Тип учетной записи | Использование Azure | Использование Marketplace | Покупки | Возвраты платежей
--- | --- | --- | --- | ---
Соглашение Enterprise (EA) | Yes | Да | Да | Нет 
Клиентское соглашение Майкрософт (MCA) | Yes | Да | Да | Yes
Оплата по мере использования | Yes | Нет  | Нет  | Нет 

Дополнительные сведения о заказах на Marketplace (также известных как внешние службы), см. в разделе [понять службы расходах на внешние Azure](billing-understand-your-azure-marketplace-charges.md).

См. в разделе [способы получения счета Azure счета и данных о ежедневном](billing-download-azure-invoice-daily-usage-date.md) инструкции по загрузке.
Об использовании и затратах файл доступен в формате файла значений с разделителями запятыми (.csv), который можно открыть в приложение электронных таблиц.

## <a name="list-of-terms-and-descriptions"></a>Список терминов и описания

В следующей таблице описаны важные термины, используемые в последнюю версию Azure файл об использовании и затратах.
В списке содержится оплаты по мере использования (PAYG), Enterprise Agreement (EA) и учетные записи клиентов соглашения Microsoft (MCA).

Термин | Тип учетной записи | ОПИСАНИЕ
--- | --- | ---
AccountName | EA | Отображаемое имя учетной записи регистрации.
ИД владельца учетной записи | EA | Уникальный идентификатор для регистрации учетной записи.
Дополнительные сведения | Все | Метаданные определенных служб. Например, тип образа для виртуальной машины.
billingAccountId | СОГЛАШЕНИЕ EA, MCA | Уникальный идентификатор для корня платежный счет.
billingAccountName | СОГЛАШЕНИЕ EA, MCA | Имя учетной записи выставления счетов.
billingCurrency | СОГЛАШЕНИЕ EA, MCA | Валюта, связанный с учетной записью выставления счетов.
BillingPeriod | EA | Расчетный период издержек.
billingPeriodEndDate | СОГЛАШЕНИЕ EA, MCA | Дата окончания расчетного периода.
billingPeriodStartDate | СОГЛАШЕНИЕ EA, MCA | Дата начала расчетного периода.
billingProfileId | СОГЛАШЕНИЕ EA, MCA | Уникальный идентификатор регистрации EA или MCA, профиль выставления счетов.
billingProfileName | СОГЛАШЕНИЕ EA, MCA | Имя регистрации EA или MCA, профиль выставления счетов.
ChargeType | СОГЛАШЕНИЕ EA, MCA | Указывает, представляет ли плата за использование (**использования**), покупки (**приобрести**), или возврат (**возмещение**).
ConsumedQuantity | (PAYG) | См. в разделе Quantity.
Использованная служба | Все | Имя службы накладные расходы, связанные с.
Стоимость | EA | См. в разделе CostInBillingCurrency.
CostCenter | СОГЛАШЕНИЕ EA, MCA | Центр затрат, определенные для подписки для отслеживания затрат (доступно только в открытых периодов выставления счетов для учетных записей MCA).
costInBillingCurrency | MCA | Стоимость расходов в Валюта выставления счетов до кредиты или налоги.
CostInPricingCurrency | MCA | Стоимость плата в ценовой категории валюте перед кредиты или налоги.
Валюта | (PAYG) | См. в разделе BillingCurrency.
Дата | СОГЛАШЕНИЕ EA, MCA | Дата использования или покупки издержек.
ExchangeRateDate | MCA | Дата, когда было установлено курс.
ExchangeRatePricingToBilling | MCA | Курс, используется для преобразования затрат в валюте ценообразования Валюта выставления счетов.
Frequency | СОГЛАШЕНИЕ EA, MCA | Указывает, ожидается ли плата для повторения. Расходов можно либо происходит один раз (**OneTime**), повторения на основе ежемесячно или ежегодно (**повторяющееся задание**), или в зависимости от использования (**UsageBased**).
includedQuantity | (PAYG) | Объем ресурсов, предоставляемый бесплатно в рамках текущего расчетного периода.
InstanceId | PAGY | См. в разделе ResourceId.
invoiceId | СОГЛАШЕНИЕ EA, MCA | Уникальный идентификатор в списке в счете PDF.
invoiceSection | MCA | См. в разделе InvoiceSectionName.
invoiceSectionId | СОГЛАШЕНИЕ EA, MCA | Уникальный идентификатор отдела EA или MCA счета раздела.
invoiceSectionName | СОГЛАШЕНИЕ EA, MCA | Имя отдела EA или разделе MCA счета.
isAzureCreditEligible | СОГЛАШЕНИЕ EA, MCA | Указывает, является ли плата за право принимать оплату за использование кредиты Azure (значения: Значение true, False).
Location | СОГЛАШЕНИЕ EA, MCA | Расположение центра обработки данных, где выполняется ресурс.
Категория единицы измерения | Все | Имя категории, классификации для единицы измерения. Например *облачные службы* и *сети*.
Значение MeterId | Все | Уникальный идентификатор для единицы измерения.
Имя единицы измерения | Все | Имя счетчика.
Регион единицы измерения | Все | Имя расположения центра обработки данных для служб ценам на основе расположения. См. в разделе Расположение.
Подкатегория единицы измерения | Все | Имя категории подклассификация индикатора.
OfferId | СОГЛАШЕНИЕ EA, MCA | Название предложения, которые приобрели.
PartNumber | EA | Идентификатор, используемый для получения цены для определенного счетчика.
Имя плана | EA | Имя плана Marketplace.
previousInvoiceId | MCA | Ссылка на исходный счет, если этот элемент имеет возмещение.
pricingCurrency | MCA | Валюта, используемая при оценку на основе согласованного цен.
Продукт | MCA | См. в разделе ProductName.
ProductId | СОГЛАШЕНИЕ EA, MCA | Уникальный идентификатор продукта.
Название продукта | EA | Имя продукта.
productOrderId | СОГЛАШЕНИЕ EA, MCA | Уникальный идентификатор для заказа продукта.
productOrderName | СОГЛАШЕНИЕ EA, MCA | Уникальное имя для продукта заказа.
PublisherName | СОГЛАШЕНИЕ EA, MCA | Издатель для служб Marketplace.
значение свойства publisherType | СОГЛАШЕНИЕ EA, MCA | Тип добавляемого издателя (значения: firstParty, thirdPartyReseller, thirdPartyAgency).
Количество | СОГЛАШЕНИЕ EA, MCA | Число единиц, приобретенных или обрабатываются.
Тариф | (PAYG) | См. в разделе «цена».
Идентификатор резервирования | СОГЛАШЕНИЕ EA, MCA | Уникальный идентификатор для экземпляра приобретенное резервирование.
reservationName | СОГЛАШЕНИЕ EA, MCA | Имя экземпляра приобретенное резервирование.
resourceGroupId | СОГЛАШЕНИЕ EA, MCA | Уникальный идентификатор для [группы ресурсов](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) ресурс является членом коллекции.
ResourceGroupName | СОГЛАШЕНИЕ EA, MCA | Имя [группы ресурсов](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) ресурс является членом коллекции.
resourceId | СОГЛАШЕНИЕ EA, MCA | Уникальный идентификатор [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources) ресурсов.
Расположение ресурса | СОГЛАШЕНИЕ EA, MCA | Расположение центра обработки данных, где выполняется ресурс. См. в разделе Расположение.
ResourceName | EA | Имя ресурса.
ResourceType | MCA | Тип экземпляра ресурса.
serviceFamily | СОГЛАШЕНИЕ EA, MCA | Семейство службы, к которой принадлежит служба.
Служебная информация 1 | Все | Метаданные определенных служб.
Служебная информация 2 | Все | Устаревшее поле с необязательные дополнительные метаданные службы.
ServicePeriodEndDate | MCA | Дата окончания периода оценки, определенных и заблокирован, цены для использованных или приобретенной службы.
servicePeriodStartDate | MCA | Дата начала периода оценки, определенных и заблокирован цен на службу потребляемого или приобретенных.
SubscriptionId | Все | Уникальный идентификатор для подписки.
Параметр SubscriptionName | Все | Имя подписки.
Теги | Все | Теги, назначенный ресурсу. Не содержит теги группы ресурсов. Можно использовать для группировки или распределения затрат для внутреннего взимания средств за использование. Дополнительные сведения см. в статье [Использование тегов для организации ресурсов в Azure](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/).
Единица измерения | (PAYG) | См. в разделе UnitOfMeasure.
Единица измерения | Все | Единица измерения для выставления счетов для службы. Например службы вычислений действует почасовая оплата.
Цена | EA | Цена за единицу для расходов.
UsageDate | (PAYG) | См. в разделе даты.

Обратите внимание, что некоторые поля могут отличаться в регистр и пробелы между типами счетов.
Более старые версии файлов с оплатой по мере использования использования имеют отдельными разделами для инструкции и о ежедневном использовании.

## <a name="ensure-that-your-charges-are-correct"></a>Проверьте правильность ваших расходов

Дополнительные сведения о подробные сведения об использовании и расходы, прочитать о том, как понять ваш [оплаты по мере использования](./billing-understand-your-bill.md) или [соглашении клиента Microsoft](billing-mca-understand-your-bill.md) счета.

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или вам нужна помощь, [создайте запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Дальнейшие действия

- [Просмотр и загрузка счете Microsoft Azure](billing-download-azure-invoice.md)
- [Просматривать и скачивать данные об использовании Microsoft Azure и затратах](billing-download-azure-daily-usage.md)
