---
title: Защита клиентских данных в Azure
description: В этой статье рассматривается, как в Azure обеспечивается защита клиентских данных.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 04163d1fa2a46a2de877702d479f439a5e8711d7
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65603134"
---
# <a name="azure-customer-data-protection"></a>Защита данных клиентов в Azure   
По умолчанию у операционного и технического персонала службы поддержки корпорации Майкрософт нет доступа к пользовательским данным. После предоставления доступа к клиентским данным требуется утверждение руководства, а затем осуществляется контроль и журналирование доступа. Требования к управлению доступом устанавливаются следующей политикой безопасности Azure:

- отсутствие доступа к клиентским данным по умолчанию;
- на виртуальных машинах клиентов отсутствуют учетные записи пользователей или администраторов;
- предоставление минимальных привилегий, необходимых для выполнения задачи; аудит и запросы доступа к журналу.

Корпорация Майкрософт присвоила сотрудникам службы поддержки Microsoft Azure уникальные корпоративные учетные записи Active Directory. Azure использует Microsoft Corporate Active Directory, которым управляет отдел информационных технологий Майкрософт (MSIT), для контроля доступа к ключевым информационным системам. Многофакторная проверка подлинности обязательная, а доступ предоставляется только из безопасной консоли.

Все попытки доступа отслеживаются и могут отображаться с помощью базового набора отчетов.

## <a name="data-protection"></a>Защита данных
Azure предоставляет клиентам надежную защиту данных — как по умолчанию, так и в параметрах пользователя.

**Разделение данных.** Azure является многопользовательской службой, а это означает, что развертывания и виртуальные машины многих клиентов хранятся на одном физическом оборудовании. Azure также использует логическую изоляцию для разделения данных каждого клиента. Разделение предоставляет экономическую выгоду и выгоду масштабирования от многопользовательских услуг и в то же время строго предохраняет клиентов от доступа к данным других клиентов.

**Защита неактивных данных**. Клиенты несут ответственность за обеспечение шифрования данных, хранящихся в Azure, в соответствии со своими стандартами. Azure предлагает широкий спектр возможностей шифрования, предоставляя клиентам достаточную гибкость при выборе решений, наилучшим образом удовлетворяющих их потребности. Azure Key Vault позволяет удобно контролировать ключи, используемые облачными приложениями и службами для шифрования данных. Шифрование дисков Azure позволяет клиентам шифровать виртуальные машины. Шифрование службы хранилища Azure позволяет шифровать все данные, помещенные в клиентскую учетную запись хранения.

**Защита данных при передаче**. Клиенты могут включить шифрование для трафика между собственными виртуальными машинами и пользователями. Azure защищает данные при передаче к или от внешних компонентов и при внутренней передаче, например, между двумя виртуальными сетями. В Azure используется стандартный протокол TLS версии 1.2 или более поздней с 2048-разрядными ключами шифрования RSA/SHA256, как рекомендовано в документации CESG/NCSC, для шифрования сообщений между:

- клиентом и облаком;
- между системами и центрами обработки данных Azure.

**Шифрование**. Клиенты могут развертывать шифрование данных в хранилище и при передаче как лучшую методику для обеспечения конфиденциальности и целостности данных. Клиентам легко настроить свои облачные сервисы Azure на использование SSL для защиты сообщений из Интернета и даже от виртуальных машин Azure, которые их принимали.

**Data redundancy** (Избыточность данных). Корпорация Майкрософт помогает защитить данные в случае кибератаки или физических повреждений центра обработки данных. Клиенты могут выбрать:

- В в страны хранилище для соответствия требованиям или задержки рекомендации.
- Хранилище out из Страна/out региона для целей безопасности или аварийного восстановления.

Данные можно реплицировать в пределах выбранной географической области для избыточности, но они не будут передаваться за ее пределы. У клиентов есть несколько опций репликации данных, включая количество копий, количество и расположение центров данных репликации.

При создании учетной записи хранения необходимо выбрать один из следующих вариантов репликации:

- **Локально избыточное хранилище (LRS)**.  Локально избыточное хранилище обслуживает три копии ваших данных. LRS реплицируется три раза в одном здании одного региона. LRS защищает ваши данные от стандартных сбоев оборудования, но не от сбоев одного помещения.
- **Хранилище, избыточное между зонами (ZRS)**. Хранилище, избыточное между зонами, обслуживает три копии ваших данных. ZRS трижды реплицируется через два-три устройства, чтобы обеспечить устойчивость, большую чем в LRS. Репликация происходит в одном или двух регионах. ZRS помогает обеспечить устойчивость данных в одном регионе.
- **Геоизбыточное хранилище (GRS).** Геоизбыточное хранилище включается для учетной записи хранения при создании по умолчанию. GRS хранит шесть копий ваших данных. При использовании GRS данные в основном регионе реплицируются трижды. Ваши данные также трижды реплицируются во вторичном регионе, который находится в сотнях километров от первичного, для самого высокого уровня устойчивости. В случае сбоя в основном регионе служба хранилища Azure выполнит отработку отказа в дополнительном регионе. GRS помогает обеспечить устойчивость данных в двух отдельных регионах.

**Уничтожение данных**. Когда клиенты удаляют данные или покидают Azure, корпорация Майкрософт использует строгие стандарты для перезаписи ресурсов хранилища перед повторным использованием, а также для физического уничтожения оборудования, выведенного из эксплуатации. Корпорация Майкрософт выполняет полное удаление данных по запросу клиента и по окончании контракта.

## <a name="customer-data-ownership"></a>Владение клиентскими данными
Корпорация Майкрософт не проверяет, не утверждает или не отслеживает работу приложений, развертываемых клиентами в Azure. Более того, Корпорация Майкрософт не знает, какой тип данных клиент решит хранить в Azure. Корпорация Майкрософт не предъявляет право на владение данными клиентов, которые введены в Azure.

## <a name="records-management"></a>Управление записями
Azure установила внутренние требования к сохранению серверных данных. Клиенты несут ответственность за определение собственных требований к хранению записей. Что касается записей, хранящихся в Azure, то клиент несет ответственность за извлечение своих данных и сохранение содержимого за пределами Azure в течение указанного периода хранения.

Azure позволяет клиентам экспортировать данные и контролировать отчеты из продукта. Экспортированные файлы сохраняют локально, чтобы сохранить информацию на определенный пользователем период хранения.

## <a name="electronic-discovery-e-discovery"></a>Электронное обнаружение (e-discovery)
Клиенты Azure несут ответственность за соблюдение требований электронного обнаружения при использовании служб Azure. Если клиенту Azure нужно сохранить свои клиентские данные, то он может экспортировать и сохранять их локально. Кроме того, клиенты могут запрашивать экспорт своих данных из отдела поддержки пользователей Azure. Помимо того что клиенты могут экспортировать свои данные, Azure проводит внутреннее обширное ведение журнала и мониторинг.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о действиях корпорации Майкрософт в сфере защиты инфраструктуры Azure приведены в следующих статьях:

- [Объекты Azure, локальная среда и физическая безопасность](azure-physical-security.md)
- [Доступность инфраструктуры Azure](azure-infrastructure-availability.md)
- [Компоненты и границы информационной системы Azure](azure-infrastructure-components.md)
- [Сетевая архитектура Azure](azure-infrastructure-network.md)
- [Рабочая сеть Azure](azure-production-network.md)
- [Возможности безопасности Базы данных SQL Azure](azure-infrastructure-sql.md)
- [Операции и управление в рабочей среде Azure](azure-infrastructure-operations.md)
- [Мониторинг инфраструктуры Azure](azure-infrastructure-monitoring.md)
- [Целостность инфраструктуры Azure](azure-infrastructure-integrity.md)
