---
title: Лучшие методики для Службы Azure Kubernetes (AKS)
description: Коллекция лучших методик по созданию приложений и управлению ими в Службе Azure Kubernetes (AKS) для оператора и разработчика кластера
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 12/07/2018
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: c73f2fea808944847fab6a675914ee57e537560b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60467159"
---
# <a name="cluster-operator-and-developer-best-practices-to-build-and-manage-applications-on-azure-kubernetes-service-aks"></a>Лучшие методики по созданию приложений и управлению ими в Службе Azure Kubernetes (AKS) для оператора и разработчика кластера

Существуют некоторые ключевые рекомендации, которые необходимо изучить и реализовать, чтобы успешно создать и запустить приложения в Службе Azure Kubernetes (AKS). К этим областям относится мультитенантность и возможности планировщика, безопасность кластера и pod или непрерывность бизнес-процессов и аварийное восстановление. Следующие лучшие методики сгруппированы, чтобы операторы и разработчики кластеров могли узнать рекомендации по каждой из этих областей и реализовать соответствующие возможности.

Эти лучшие методики и тематические статьи приведены в сочетании с соответствующей группой продуктов AKS, командами разработчиков и промысловыми группами, включая глобальные черные пояса.

## <a name="cluster-operator-best-practices"></a>Лучшие методики для оператора кластера

Операторы кластера сотрудничают с владельцами и разработчиками приложений, чтобы узнать об их потребностях. Затем при необходимости можно настроить кластеры AKS с помощью следующих рекомендаций.

**Мультитенантность**

* [Рекомендации по изоляции кластера в Службе Azure Kubernetes (AKS)](operator-best-practices-cluster-isolation.md)
    * Содержит сведения о базовых компонентах мультитенантности и логической изоляции с пространствами имен.
* [Рекомендации по основным функциям планировщика в "Службе Azure Kubernetes" (AKS)](operator-best-practices-scheduler.md)
    * Содержит сведения об использовании квот ресурсов и о бюджетах неработоспособности pod.
* [Рекомендации по расширенным возможностям планировщика в Службе Azure Kubernetes (AKS)](operator-best-practices-advanced-scheduler.md)
    * Содержит сведения об использовании отметок и толерантностей, о селекторах узлов и сходстве, сходстве pod и антисходстве между модулями.
* [Рекомендации по аутентификации и авторизации в Службе Azure Kubernetes (AKS)](operator-best-practices-identity.md)
    * Содержит сведения об интеграции с Azure Active Directory с использованием элементов управления доступом на основе ролей и удостоверений pod.

**Безопасность**

* [Best practices for cluster security and upgrades in Azure Kubernetes Service (AKS)](operator-best-practices-cluster-security.md) (Рекомендации по безопасности кластера и обновлениях в Службе Azure Kubernetes (AKS))
    * Содержит сведения о защите доступа к серверу API, ограничении доступа к контейнеру и управлении обновлениями и перезагрузками узла.
* [Best practices for container image management and security in Azure Kubernetes Service (AKS)](operator-best-practices-container-image-management.md) (Рекомендации по управлению образами контейнеров и обеспечении их безопасности в Службе Azure Kubernetes (AKS))
    * Содержит сведения об обеспечении безопасности образа и сред выполнения с использованием доверенных реестров и автоматизированных сборок при обновлении базового образа...
* [Best practices for pod security in Azure Kubernetes Service (AKS)](developer-best-practices-pod-security.md) (Рекомендации по обеспечению безопасности pod в Службе Azure Kubernetes (AKS))
    * Содержит сведения о защите доступа к ресурсам, ограничении доступности учетных данных и использовании удостоверений pod и цифровых хранилищ ключей.

**Сеть и хранилище**

* [Рекомендации по подключению сетей и безопасности в Службе Azure Kubernetes (AKS)](operator-best-practices-network.md)
    * Содержит сведения о разных моделях сетей, использовании входящего трафика и брандмауэров веб-приложений и обеспечении безопасности доступа к узлам по протоколу SSH.
* [Best practices for storage and backups in Azure Kubernetes Service (AKS)](operator-best-practices-storage.md) (Рекомендации по хранилищу и резервному копированию в Службе Azure Kubernetes (AKS))
    * Содержит сведения о выборе соответствующего типа хранилища и размера узла, динамической подготовке томов и резервном копировании данных.

**Запуск рабочих нагрузок, готовых к использованию на предприятии**

* [Best practices for business continuity and disaster recovery in Azure Kubernetes Service (AKS)](operator-best-practices-multi-region.md) (Лучшие методики по обеспечению непрерывности бизнес-процессов и аварийного восстановления в Службе Azure Kubernetes (AKS))
    * Содержит сведения об использовании пар регионов, нескольких кластеров с помощью диспетчера трафика Azure и географической репликации образов контейнеров.

## <a name="developer-best-practices"></a>Рекомендации разработчикам

Разработчики или владельцы приложений могут упростить процесс разработки и определить требования к производительности приложения.

* [Рекомендации для разработчиков приложений по управлению ресурсами в Службе Azure Kubernetes (AKS)](developer-best-practices-resource-management.md)
    * Содержит сведения об определении запросов и ограничений ресурсов pod, настройке средств разработки и проверке наличия проблем приложений.
* [Best practices for pod security in Azure Kubernetes Service (AKS)](developer-best-practices-pod-security.md) (Рекомендации по обеспечению безопасности pod в Службе Azure Kubernetes (AKS))
    * Содержит сведения о защите доступа к ресурсам, ограничении доступности учетных данных и использовании удостоверений pod и цифровых хранилищ ключей.

## <a name="kubernetes--aks-concepts"></a>Концепции Kubernetes и AKS

Для понимания некоторых возможностей и компонентов этих лучших методик также можно обратиться к следующим тематическим статьям о кластерах в Службе Azure Kubernetes (AKS):

* [Ключевые концепции Kubernetes для Службы Azure Kubernetes (AKS)](concepts-clusters-workloads.md)
* [Возможности контроля доступа и идентификации в Службе Azure Kubernetes (AKS)](concepts-identity.md)
* [Основные понятия безопасности приложений и кластеров в Службе Azure Kubernetes (AKS)](concepts-security.md)
* [Основные понятия сети в Службе Azure Kubernetes (AKS)](concepts-network.md)
* [Варианты хранилищ](concepts-storage.md)
* [Возможности масштабирования приложений в Службе Azure Kubernetes (AKS)](concepts-scale.md)

## <a name="next-steps"></a>Дальнейшие действия

Если вам необходимо приступить к работе с AKS, ознакомьтесь с одним из кратких руководств по развертыванию кластера Службы Azure Kubernetes (AKS) с помощью [Azure CLI](kubernetes-walkthrough.md) или [портала Azure](kubernetes-walkthrough-portal.md).