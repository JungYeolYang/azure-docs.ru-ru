---
title: Общие сведения о пользовательских конечных точках Центра Интернета вещей Azure | Документы Майкрософт
description: Руководство разработчика по использованию запросов маршрутизации для маршрутизации сообщений, передаваемых с устройства в облако, в пользовательские конечные точки.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: e5e92c40cef15e99431dc9652820c71e87935f67
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61244350"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Использование правил маршрутизации и пользовательских конечных точек для сообщений, отправляемых с устройства в облако

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Функция [маршрутизации сообщений](iot-hub-devguide-routing-query-syntax.md) Центра Интернета вещей позволяет маршрутизировать сообщения с устройства в облако в обращенные к службе конечные точки. Маршрутизация также обеспечивает возможность выполнения запросов для фильтрации данных перед их отправкой на конечные точки. Каждый настраиваемый запрос маршрутизации имеет следующие свойства:

| Свойство      | ОПИСАНИЕ |
| ------------- | ----------- |
| **Имя**      | Уникальное имя, определяющее запрос. |
| **Source**    | Источник потока данных, в отношении которого выполняются действия. Например, телеметрия устройства. |
| **Condition** | Выражение для запроса маршрутизации, вычисляется для свойств приложения сообщений, свойств системы, текста сообщения, тегов двойника устройства и свойств двойника устройства, чтобы определить, соответствует ли сообщение конечной точке. Дополнительные сведения о построении запроса см. в статье [Синтаксис запросов маршрутизации сообщений центра Интернета вещей](iot-hub-devguide-routing-query-syntax.md). |
| **Конечная точка**  | Имя конечной точки, в которую Центр Интернета вещей отправляет сообщения, соответствующие запросу. Рекомендуется выбрать конечную точку в том же регионе, в котором находится Центр Интернета вещей. |

Одно сообщение может соответствовать условиям в нескольких запросах маршрутизации. В таких случаях Центр Интернета вещей доставляет сообщение в конечную точку, связанную с каждым из этих запросов. Центр Интернета вещей также выполняет автоматическую дедупликацию доставки сообщений. То есть если сообщение соответствует нескольким запросам, имеющим общее назначение, то оно записывается в это назначение только один раз.

## <a name="endpoints-and-routing"></a>Конечные точки и маршрутизация

Центр Интернета вещей по умолчанию обладает [встроенной конечной точкой](iot-hub-devguide-messages-read-builtin.md). Кроме того, можно создать пользовательские конечные точки для маршрутизации сообщений, связав с Центром другие службы в подписке. Сейчас Центр Интернета вещей поддерживает использование следующих служб в качестве пользовательских конечных точек: контейнеры службы хранилища Azure, Центры событий, очереди служебной шины и разделы служебной шины.

При использовании маршрутизации и пользовательских конечных точек сообщения доставляются во встроенную конечную точку, только если они не соответствуют запросам. Чтобы доставить сообщения во встроенную конечную точку, а также в пользовательскую конечную точку, добавьте маршрут, который отправляет сообщения во встроенную конечную точку **событий**.

> [!NOTE]
> * Центр Интернета вещей поддерживает только запись данных в контейнеры службы хранилища Azure в виде больших двоичных объектов.
> * Очереди и разделы служебной шины, для которых включены **сеансы** или **поиск повторяющихся данных**, не поддерживаются в качестве пользовательских конечных точек.

Дополнительные сведения о создании пользовательских конечных точек в Центре Интернета вещей см. в статье [Руководство. Конечные точки Центра Интернета вещей](iot-hub-devguide-endpoints.md).

Дополнительные сведения о чтении из пользовательских конечных точек см. в статьях, посвященных:

* Считыванию данных из [контейнеров службы хранилища Azure](../storage/blobs/storage-blobs-introduction.md).

* Считыванию данных из [Центров событий](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* Считыванию данных из [очередей служебной шины](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

* Считыванию данных из [разделов служебной шины](../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md).

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о конечных точках Центра Интернета вещей см. в статье [Руководство. Конечные точки Центра Интернета вещей](iot-hub-devguide-endpoints.md).

* Дополнительные сведения о языке запросов, используемом для определения запросов маршрутизации, см. в статье [Синтаксис запросов маршрутизации сообщений центра Интернета вещей](iot-hub-devguide-routing-query-syntax.md).

* В статье [Руководство. Настройка маршрутизации сообщений с Центром Интернета вещей](tutorial-routing.md) демонстрируется использование запросов маршрутизации и пользовательских конечных точек.