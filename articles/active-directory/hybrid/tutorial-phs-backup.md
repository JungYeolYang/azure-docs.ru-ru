---
title: Руководство.  Настройка PHS в качестве резервной копии для AD FS в Azure AD Connect | Документация Майкрософт
description: Показывает, как включить синхронизацию хэшей паролей в качестве резервной копии, а также для AD FS.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 01/30/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2281bc451a5acf9e4e634a124161a3e8b0734deb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58090514"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Руководство.  Настройка PHS в качестве резервной копии для AD FS в Azure AD Connect

В следующем руководстве вы узнаете, как настроить синхронизацию хэша пароля в качестве резервной копии и аварийного переключения для AD FS.  В этом документе также будет продемонстрировано, как включить синхронизацию хэша пароля в качестве основного метода проверки подлинности, если в AD FS произошел сбой или служба стала недоступной.

## <a name="prerequisites"></a>Предварительные требования
Перед изучением этого руководства ознакомьтесь со статьей[Руководство. Создание федерации среды одного леса AD в облаке с использованием PHS](tutorial-federation.md).  Завершите это руководство, прежде чем предпринимать шаги, описанные в этом документе.

## <a name="enable-phs-in-azure-ad-connect"></a>Включение PHS для Azure AD Connect
Теперь, когда у нас есть среда Azure AD Connect, в которой используется федерация, нужно включить синхронизацию хэшей паролей и разрешить Azure AD Connect синхронизировать хэши.

Выполните следующее:

1.  Дважды щелкните значок Azure AD Connect, созданный на рабочем столе.
2.  Нажмите **Настроить**.
3.  На странице "Дополнительные задачи" выберите **Настроить параметры синхронизации** и нажмите **Далее**.
4.  Введите имя пользователя и пароль для глобального администратора.  Эта учетная запись была создана [здесь](tutorial-federation.md#create-a-global-administrator-in-azure-ad) в предыдущем руководстве.
5.  На экране **Подключить каталоги** щелкните **Далее**.
6.  На экране **Фильтрация доменов и подразделений** нажмите кнопку **Далее**.
7.  На экране **Дополнительные возможности** выберите **Синхронизация хэшей паролей** и щелкните **Далее**.
![Select](media/tutorial-phs-backup/backup1.png)</br>
8.  На экране **Все готово к настройке** щелкните **Настроить**.
9.  После завершения конфигурации щелкните **Выйти**.
10. Вот и все!  Готово.  Синхронизация хэшей паролей теперь будет происходить и может использоваться в качестве резервной копии, если AD FS станет недоступным.

## <a name="switch-to-password-hash-synchronization"></a>Переключение на синхронизацию хэшей паролей
Теперь мы покажем, как переключиться на синхронизацию хэшей паролей. Прежде чем начать, рассмотрим, при каких условиях следует использовать переключение. Не используйте переключение для решения временных проблем, например сбоя сети, небольших проблем с AD FS или проблемы, которая влияет на группу пользователей. Если вы решили использовать переключение, потому что устранение проблем займет слишком много времени, сделайте следующее:

1. Дважды щелкните значок Azure AD Connect, созданный на рабочем столе.
2.  Нажмите **Настроить**.
3.  Выберите **Смена имени пользователя для входа** и щелкните **Далее**.
![Изменение](media/tutorial-phs-backup/backup2.png)</br>
4.  Введите имя пользователя и пароль для глобального администратора.  Эта учетная запись была создана [здесь](tutorial-federation.md#create-a-global-administrator-in-azure-ad) в предыдущем руководстве.
5.  На экране **Вход пользователя** выберите **Синхронизация хэшей паролей** и установите флажок в поле **Не преобразовывать учетные записи пользователей**.  
6.  Оставьте значение **Включить единый вход** по умолчанию и щелкните **Далее**.
7.  На экране **Включить единый вход** нажмите **Далее**.
8.  На экране **Готово к настройке** щелкните **Настроить**.
9.  Завершив настройку, нажмите кнопку **Выйти**.
10. Теперь пользователи могут использовать пароли для входа в Azure и служб Azure.

## <a name="test-signing-in-with-one-of-our-users"></a>Проверка входа с помощью одной из учетных записей

1. Перейдите на сайт [https://myapps.microsoft.com](https://myapps.microsoft.com).
2. Выполните вход с помощью учетной записи пользователя, которая была создана в новом клиенте.  Для этого следует использовать следующий формат: (user@domain.onmicrosoft.com). Используйте тот же пароль, что и для входа в локальную среду.</br>
   ![Проверка](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="next-steps"></a>Дальнейшие действия


- [Оборудование и предварительные требования](how-to-connect-install-prerequisites.md) 
- [Стандартные параметры](how-to-connect-install-express.md)
- [Синхронизация хэша паролей](how-to-connect-password-hash-synchronization.md)|
