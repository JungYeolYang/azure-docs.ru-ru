---
title: Скрипт Azure CLI. Восстановление сервера службы "База данных Azure для MySQL" до предыдущей точки во времени
description: Этот пример скрипта CLI позволяет восстановить сервер службы "База данных Azure для MySQL" до предыдущей точки во времени.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 38825f7815713678946e86252d4c8a1bee0abbc6
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/06/2019
ms.locfileid: "55752693"
---
# <a name="restore-an-azure-database-for-mysql-server-using-azure-cli"></a>Восстановление сервера службы "База данных Azure для MySQL" с помощью Azure CLI
Этот пример скрипта CLI позволяет восстановить один сервер службы "База данных Azure для MySQL" до предыдущей точки во времени.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этим примером вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Пример скрипта
В этом примере скрипта измените выделенные строки, чтобы настроить имя и пароль администратора. Замените идентификатор подписки, используемый в командах мониторинга az, собственным.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/backup-restore.sh?highlight=15-16 "Restore Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Очистка развертывания
После выполнения примера сценария можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Описание скрипта
Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| **Команда** | **Примечания** |
|---|---|
| [az group create](/cli/azure/group) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az mysql server create](/cli/azure/mysql/server) | Создает сервер MySQL, на котором размещены базы данных. |
| [az mysql server restore](/cli/azure/mysql/server) | Восстановление сервера из резервной копии. |
| [az group delete](/cli/azure/group) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дополнительная информация
- Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).
- Дополнительные скрипты — [Примеры Azure CLI для базы данных Azure для MySQL](../sample-scripts-azure-cli.md)
