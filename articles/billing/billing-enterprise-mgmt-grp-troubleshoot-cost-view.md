---
title: Устранение неполадок в представлении корпоративных затрат в Azure | Документация Майкрософт
description: Узнайте, как устранить все проблемы, связанные с представлением организационных затрат в пределах портала Azure.
author: rthorn17
manager: adpick
editor: ''
ms.assetid: ''
ms.service: billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2017
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: d35996b16d615a198b9a6039386f6b295172f388
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60615775"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Устранение неполадок в представлении затрат на условиях соглашения Enterprise

В рамках регистрации на предприятии с помощью некоторых настроек можно ограничить возможность пользователей просматривать затраты.  Этими параметрами управляет администратор регистрации. Если же регистрацию не приобрели напрямую через корпорацию Майкрософт, параметрами управляет партнер.  В этой статье содержится обзор параметров, а также их взаимосвязи с регистрацией. Эти параметры не зависят от ролей управления доступом на основе ролей (RBAC) Azure.

## <a name="enabling-access-to-costs"></a>Включение доступа к затратам

При поиске сведений о затратах вы можете получить сообщение об ошибке авторизации или об *отключении представлений затрат в регистрации*. С вами такое случилось?
![Снимок экрана, показывающий "не авторизовано" в поле текущей стоимости для подписки.](media/billing-enterprise-mgmt-groups/unauthorized.png)

Это может быть вызвано одной из следующих причин:

1. Вы приобрели Azure через корпоративного участника, который еще не предоставил вам сведения о ценах. Свяжитесь с партнером, чтобы обновить параметр цен на портале [Enterprise Portal](https://ea.azure.com).
2. Если вы являетесь непосредственным клиентом EA, доступны несколько возможностей:
    * Вы являетесь владельцем учетной записи, и администратор регистрации отключил параметр **просмотра затрат для владельца учетной записи**.  
    * Вы являетесь администратором отдела, и администратор регистрации отключил **возможность просмотра затрат для администратора отдела**.
    * Обратитесь к администратору регистрации для получения доступа. Администратор регистрации может обновить параметры на портале [Enterprise Portal](https://ea.azure.com/manage/enrollment).

      ![Снимок экрана, показывающий параметры для просмотра затрат на портале Enterprise Portal.](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)

## <a name="asset-is-unavailable"></a>Ресурс недоступен

Если при попытке получить доступ к подписке или группе управления вы получили сообщение об ошибке недоступности ресурса, значит вам не назначена определенная роль, чтобы просмотреть этот элемент.  

![Снимок экрана: сообщение "ресурс недоступен".](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Чтобы получить доступ, попросите администратора подписки Azure или группы управления. Дополнительные сведения см. в статье [Управление доступом с помощью RBAC и портала Azure](../role-based-access-control/role-assignments-portal.md).
