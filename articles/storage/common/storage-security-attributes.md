---
title: Общие атрибуты безопасности для службы хранилища Azure
description: Контрольный список Общие атрибуты безопасности для оценки службы хранилища Azure
services: storage
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: storage
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 7868b52fee991d4b9323fa0b7969aeca4dc83cdb
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64711951"
---
# <a name="common-security-attributes-for-azure-storage"></a>Общие атрибуты безопасности для службы хранилища Azure

Средства безопасности интегрированы в каждом аспекте службы Azure. В этой статье описываются наиболее распространенные атрибуты безопасности, встроенных в службу хранилища Azure. 

[!INCLUDE [Security Attributes Header](../../../includes/security-attributes-header.md)]

## <a name="preventative"></a>профилактическая;

| Атрибут безопасности | Да/нет | Примечания |
|---|---|--|
| Шифрование при хранении:<ul><li>Шифрование на стороне сервера</li><li>Шифрование на стороне сервера с использованием управляемых клиентом ключей</li><li>Другие возможности шифрования (например, на стороне клиента, постоянное шифрование и т. д.)</ul>| Yes |  |
| Шифрование при передаче:<ul><li>Шифрование ExpressRoute</li><li>При шифровании виртуальной сети</li><li>Шифрование между виртуальными сетями</ul>| Yes | Поддерживают стандартные механизмы HTTPS/TLS.  Пользователи также могут шифровать данные перед их передачей в службу. |
| Управление ключами шифрования (CMK, BYOK, и т.д.)| Yes | См. в разделе [шифрование службы хранилища Azure с помощью управляемых пользователем ключей в Azure Key Vault](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Шифрование на уровне столбцов (службы данных Azure)| Н/Д |  |
| Вызовы API в зашифрованном виде| Yes |  |

## <a name="network-segmentation"></a>Сегментация сети

| Атрибут безопасности | Да/нет | Примечания |
|---|---|--|
| Поддержка конечной точки службы| Yes |  |
| Внедрение поддержки виртуальной сети| Н/Д |  |
| Сетевая изоляция и множество поддержки| Yes | |
| Принудительного туннелирования поддержки| Н/Д |  |

## <a name="detection"></a>Обнаружение

| Атрибут безопасности | Да/нет | Примечания|
|---|---|--|
| Azure Monitor поддержки (Log analytics, Application insights, и т.д.)| Yes | Доступные метрики Azure Monitor теперь регистрирует начальной предварительной версии |

## <a name="identity-and-access-management"></a>Управление удостоверениями и доступом

| Атрибут безопасности | Да/нет | Примечания|
|---|---|--|
| Authentication| Yes | Azure Active Directory, общий ключ, токен общего доступа. |
| Авторизация| Yes | Поддержка авторизации с помощью RBAC, списки управления доступом POSIX и маркеров SAS |


## <a name="audit-trail"></a>Журнал аудита

| Атрибут безопасности | Да/нет | Примечания|
|---|---|--|
| Управление и ведение журнала и аудит | Yes | Журнал действий Azure Resource Manager |
| Ведение журнала данных и аудита| Yes | Журналы диагностики службы и ведение журнала Azure Monitor начальной предварительной версии  |

## <a name="configuration-management"></a>Управление конфигурацией

| Атрибут безопасности | Да/нет | Примечания|
|---|---|--|
| Поддержка конфигурации management (Управление версиями конфигурации и т. д.)| Yes | Поддерживает управление версиями поставщика ресурсов через API-интерфейсы Azure Resource Manager |