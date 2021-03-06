---
title: Общие сведения об Azure Blockchain Workbench
description: Общие сведения об Azure Blockchain Workbench и его функциональных возможностях.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/14/2019
ms.topic: overview
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 58fd09726f05ba442c66387ecbd6cfad37f598e1
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332563"
---
# <a name="what-is-azure-blockchain-workbench"></a>Что такое Azure Blockchain Workbench?

Azure Blockchain Workbench — это совокупность служб и возможностей Azure, которые позволяют создавать и развертывать приложения блокчейн для совместного использования бизнес-процессов и данных с другими организациями. Azure Blockchain Workbench предоставляет формирование шаблонов инфраструктуры для создания приложений блокчейн, позволяя разработчикам сосредоточиться на создании бизнес-логики и смарт-контрактов. Эта служба также упрощает процесс создания приложений блокчейн путем интеграции нескольких служб и возможностей Azure для автоматизации типичных задач разработки.

## <a name="create-blockchain-applications"></a>Создание блокчейн-приложений

С помощью Blockchain Workbench можно определять приложения блокчейн, используя конфигурации, а также путем написания кода смарт-контракта. Вы можете начать процесс разработки приложения блокчейн и сосредоточиться на определении контракта и написании бизнес-логики вместо создания системы формирования шаблонов и настройки вспомогательных служб.

## <a name="manage-applications-and-users"></a>Управление приложениями и пользователями

Azure Blockchain Workbench предоставляет веб-приложение и REST API для управления приложениями и пользователями блокчейн. Администраторы Blockchain Workbench могут управлять доступом к приложениям и назначать пользователям роли приложения. Пользователи Azure AD автоматически сопоставляются с членами в приложении.

## <a name="integrate-blockchain-with-applications"></a>Интеграция с приложениями блокчейн

Вы можете использовать REST API Workbench Blockchain и API на основе сообщений для интеграции с имеющимися системами. API-интерфейсы предоставляют интерфейс для замены или для использования нескольких технологий распределенного реестра, хранилища и предложений базы данных.

Blockchain Workbench может преобразовать сообщения, отправляемые в API на основе сообщений, для создания транзакций в формате, ожидаемом собственным API блокчейна.  Workbench может подписывать и перенаправлять транзакции в соответствующий блокчейн. 

Workbench автоматически передает события в служебную шину и службу "Сетка событий" для отправки сообщений подчиненным потребителям. Разработчики могут выполнять интеграцию с любой из этих систем обмена сообщениями, чтобы запустить транзакции и просмотреть результаты.

## <a name="deploy-a-blockchain-network"></a>Развертывание сети блокчейн

Azure Blockchain Workbench упрощает настройку сети блокчейн консорциума в качестве предварительно настроенных решений с помощью шаблона решения Azure Resource Manager. Шаблон предоставляет упрощенное развертывание всех компонентов, необходимых для запуска консорциума. Сейчас в Blockchain Workbench поддерживается Ethereum.

## <a name="use-active-directory-login"></a>Использование входа в систему Active Directory

С помощью имеющихся протоколов блокчейна его удостоверения представлены в качестве адреса в сети. Azure Blockchain Workbench абстрагируется от удостоверения блокчейна, связав его с удостоверением Active Directory, что упрощает создание корпоративных приложений с помощью удостоверений Active Directory.

## <a name="synchronize-on-chain-data-with-off-chain-storage"></a>Синхронизация данных по цепочке с помощью хранилища вне сети

Azure Blockchain Workbench упрощает анализ событий и данных блокчейна, автоматически синхронизируя эти данные в блокчейне с хранилищем вне сети. Вместо извлечения данных непосредственно из блокчейна вы можете запрашивать системы баз данных вне сети (например, SQL Server). Особый опыт работы с блокчейном не требуется для пользователей, выполняющих задачи анализа данных. 

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Архитектура Azure Blockchain Workbench](architecture.md)
