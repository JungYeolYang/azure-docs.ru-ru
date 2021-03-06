---
title: 'Azure AD Connect выполняет следующие функции: обновление до последней версии | Документация Майкрософт'
description: В этой статье описаны различные способы обновления Azure Active Directory Connect до последней версии, включая обновление на месте и обновление со сменой сервера.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 04/08/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2a3e7373a8b0354a3d08debf944f2f77f1609382
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60347755"
---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect выполняет следующие функции: обновление до последней версии
В этой статье описываются различные варианты обновления установленного экземпляра Azure Active Directory (Azure AD) Connect до последней версии. Мы рекомендуем устанавливать все новые выпуски Azure AD Connect. Действия, описанные в разделе [Обновление со сменой сервера](#swing-migration), можно также использовать при значительных изменениях конфигурации.

>[!NOTE]
> В настоящее время поддерживается обновление любой версии Azure AD Connect до текущей версии. Обновление на месте DirSync или ADSync не поддерживаются и сменой является обязательным.  Если вы хотите обновить DirSync, см. в разделе [обновление средства синхронизации Azure AD (DirSync)](how-to-dirsync-upgrade-get-started.md) или [обновление со сменой](#swing-migration) раздел.  </br>На практике клиенты, использующие старые версии могут возникать проблемы, не связанные непосредственно с Azure AD Connect. Серверы, которые были в рабочей среде в течение нескольких лет, обычно имели несколько обновления, примененные к ним и не все из них мог быть учтен.  Как правило, пользователям, которые не обновились в 12-18 месяцев следует обновление со сменой вместо этого, так как это параметр наиболее Консервативная и бы рискованным.

Инструкции по обновлению DirSync см. в статье [Azure AD Connect: обновление DirSync](how-to-dirsync-upgrade-get-started.md).

Существует несколько различных стратегий обновления Azure AD Connect.

| Метод | ОПИСАНИЕ |
| --- | --- |
| [Автоматическое обновление](how-to-connect-install-automatic-upgrade.md) |Самый простой вариант — для клиентов с экспресс-установкой. |
| [Обновление «на месте»](#in-place-upgrade) |Если у вас один сервер, то установку можно обновить "на месте". |
| [Обновление со сменой сервера](#swing-migration) |Если серверов два, то можно установить новый выпуск или конфигурацию на один из них, а затем сменить активный сервер. |

Сведения о разрешениях см. в разделе [Azure AD Connect: учетные записи и разрешения](reference-connect-accounts-permissions.md#upgrade).

> [!NOTE]
> После включения нового сервера Azure AD Connect для запуска синхронизации изменений в Azure AD откат к использованию DirSync или Azure AD Sync уже невозможен. Обратный переход с Azure AD Connect к устаревшим клиентам, включая DirSync и Azure AD Sync, не поддерживается и может привести к таким проблемам, как потеря данных в Azure AD.

## <a name="in-place-upgrade"></a>Обновление «на месте»
Обновление на месте подходит для обновления Azure AD Sync или Azure AD Connect. Оно не подходит для перехода с DirSync или для решения на основе Forefront Identity Manager (FIM) и соединителя Azure AD.

Мы советуем использовать этот вариант, если у вас есть один сервер и меньше 100 000 объектов. Если в стандартных правилах синхронизации есть какие-либо изменения, после обновления выполняется полный импорт и полная синхронизация. Этот способ гарантирует, что новая конфигурация будет применена ко всем существующим в системе объектам. Этот процесс может занять несколько часов в зависимости от числа объектов в области действия модуля синхронизации. Обычная синхронизация изменений, по умолчанию выполняемая каждые 30 минут, приостанавливается, но синхронизация паролей продолжает выполняться. Обновление "на месте" лучше всего проводить в выходные дни. Если в новом выпуске Azure AD Connect не изменены стандартные правила синхронизации, то начнется обычная процедура импорта или синхронизации изменений.  
![Обновление «на месте»](./media/how-to-upgrade-previous-version/inplaceupgrade.png)

Если вы внесли изменения в стандартные правила синхронизации, при обновлении они возвращаются к конфигурации по умолчанию. Чтобы после обновления конфигурация сохранялась, внесите изменения, как указано в статье [Службы синхронизации Azure AD Connect: рекомендации по изменению конфигурации по умолчанию](how-to-connect-sync-best-practices-changing-default-configuration.md).

Во время обновления на месте могут быть внесены изменения, требующие выполнения определенных действий по синхронизации (включая шаг полного импорта и шаг полной синхронизации) после завершения обновления. Сведения о том, как отложить эти действия, см. в разделе [Как отложить полную синхронизацию после обновления](#how-to-defer-full-synchronization-after-upgrade).

При использовании Azure AD Connect с нестандартным соединителем (например, универсальным соединителем LDAP и универсальным соединителем SQL) необходимо обновить соответствующую конфигурацию соединителя в [Synchronization Service Manager](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-connectors) после обновления на месте. Дополнительные сведения о том, как обновить конфигурацию соединителя, см. в статье [История выпусков версий соединителей](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history#troubleshooting). Если не обновить конфигурацию, действия импорта и экспорта для соединителя не будут выполняться правильно. В журнале событий приложений появится ошибка *Assembly version in AAD Connector configuration ("X.X.XXX.X") is earlier than the actual version ("X.X.XXX.X") of "C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll* (Версия сборки в конфигурации соединителя AAD ("X.X.XXX.X") ниже текущей версии ("X.X.XXX.X") файла "C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll").

## <a name="swing-migration"></a>Обновление со сменой сервера
При наличии сложного развертывания или большого числа объектов обновление системы "на месте" будет нецелесообразно. У некоторых клиентов процесс может занять несколько дней, в течение которых никакие изменения синхронизироваться не будут. Этот метод также можно использовать, если планируется значительно изменить конфигурацию и нужно проверить ее перед отправкой в облако.

В подобных случаях мы рекомендуем обновление со сменой сервера. Вам потребуется по крайней мере два сервера — активный и промежуточный. Активный сервер (обозначен сплошными синими линиями на приведенном ниже рисунке) отвечает за активные рабочие нагрузки. Промежуточный сервер (обозначен пунктирными фиолетовыми линиями) подготавливается с использованием нового выпуска или конфигурации. После завершения подготовки к использованию он становится активным сервером. Сервер, который раньше был активным и на котором теперь установлена старая версия или конфигурация, становится промежуточным и обновляется.

На двух серверах могут использоваться разные версии. Например, активный сервер, который вы планируете перестать использовать, может использовать Azure AD Sync, а новый промежуточный сервер — Azure AD Connect. Если для развертывания новой конфигурации используется обновление со сменой сервера, мы рекомендуем устанавливать одинаковые версии на двух серверах.  
![промежуточного сервера](./media/how-to-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Некоторые клиенты предпочитают использовать для этого сценария три или четыре сервера. Когда промежуточный сервер обновляется, он не может служить резервным сервером для [аварийного восстановления](how-to-connect-sync-staging-server.md#disaster-recovery). При наличии трех или четырех серверов можно установить новую версию на одной паре основного и резервного серверов. Это обеспечит постоянную доступность промежуточного сервера.

Эти действия подходят также для обновления Azure AD Sync или решения на основе FIM и соединителя Azure AD. Они не подходят для DirSync. Метод обновления со сменой сервера (также называемый параллельным развертыванием) для DirSync описывается в статье [Azure AD Connect: обновление DirSync](how-to-dirsync-upgrade-get-started.md).

### <a name="use-a-swing-migration-to-upgrade"></a>Использование обновления со сменой сервера
1. Если вы используете Azure AD Connect на обоих серверах и хотите только изменить конфигурацию, убедитесь, что на активном и промежуточном серверах установлена одна и та же версия. Это упростит сравнение различий позже. При обновлении Azure AD Sync эти серверы имеют разные версии. При обновлении более ранней версии Azure AD Connect удобно использовать одинаковую версию на обоих серверах, но это необязательно.
2. Если вы внесли изменения в конфигурацию на активном сервере, а на промежуточном они отсутствуют, выполните инструкции в разделе [Перенос пользовательской конфигурации с активного на промежуточный сервер](#move-a-custom-configuration-from-the-active-server-to-the-staging-server).
3. При обновлении более ранней версии Azure AD Connect обновите промежуточный сервер до последней версии. При перемещении из Azure AD Sync установите Azure AD Connect на промежуточном сервере.
4. Разрешите модулю синхронизации выполнить полный импорт и полную синхронизацию на промежуточном сервере.
5. Убедитесь, что новая конфигурация не вызывает непредвиденных изменений, выполнив инструкции из раздела "Проверка" в статье [Проверка конфигурации сервера](how-to-connect-sync-staging-server.md#verify-the-configuration-of-a-server). Если что-то пошло не так, как ожидалось, внесите необходимые исправления, выполните импорт и синхронизацию, после чего проверьте, исправлены ли данные, выполнив приведенные ниже действия.
6. Сделайте промежуточный сервер активным. Это действие соответствует последнему шагу, "Переключение активного сервера", в статье [Проверка конфигурации сервера](how-to-connect-sync-staging-server.md#verify-the-configuration-of-a-server).
7. При обновлении Azure AD Connect обновите сервер, находящийся в промежуточном режиме, до последней версии. Обновите данные и конфигурацию, как описано выше. При обновлении Azure AD Sync на этом этапе можно отключить и списать старый сервер.

### <a name="move-a-custom-configuration-from-the-active-server-to-the-staging-server"></a>Перемещение пользовательской конфигурации с активного сервера на промежуточный сервер
Если вы внесли изменения в конфигурацию на активном сервере, их нужно применить и к промежуточному серверу. Чтобы упростить этот переход, можно воспользоваться [средством документирования конфигурации Azure AD Connect](https://github.com/Microsoft/AADConnectConfigDocumenter).

Можно переместить созданные вами пользовательские правила синхронизации с помощью PowerShell. Другие изменения необходимо применить одинаковым способом в обеих системах, и их нельзя перенести. [Средство документирования конфигураций](https://github.com/Microsoft/AADConnectConfigDocumenter) поможет сравнить две системы на идентичность. Его также можно использовать при автоматизации шагов, описанных в этом разделе.

На обоих серверах необходимо одинаково настроить следующее:

* Подключение к одним и тем же лесам.
* Фильтрацию доменов и подразделений.
* Одни и те же дополнительные компоненты, например синхронизацию и обратную запись паролей.

**Перемещение пользовательских правил синхронизации**  
Для перемещения пользовательских правил синхронизации выполните следующее.

1. Откройте **редактор правил синхронизации** на активном сервере.
2. Выберите пользовательское правило. Щелкните **Экспорт**. Откроется окно Блокнота. Сохраните временный файл с расширением PS1 — он станет скриптом PowerShell. Скопируйте PS1-файл на промежуточный сервер.  
   ![Экспорт правил синхронизации](./media/how-to-upgrade-previous-version/exportrule.png)
3. На промежуточном сервере используется другой GUID соединителя, и его нужно изменить. Чтобы получить GUID, запустите **редактор правил синхронизации**, выберите одно из стандартных правил, представляющих ту же подключенную систему, и щелкните **Экспорт**. Замените GUID в файл PS1 на GUID для промежуточного сервера.
4. Запустите файл PS1 из командной строки PowerShell. На промежуточном сервере будет создано пользовательское правило синхронизации.
5. Повторите эти действия для всех пользовательских правил.

## <a name="how-to-defer-full-synchronization-after-upgrade"></a>Как отложить полную синхронизацию после обновления
Во время обновления на месте могут быть внесены изменения, требующие выполнения определенных действий по синхронизации (включая шаг полного импорта и шаг полной синхронизации). Например, при изменении схемы соединителя на затронутых соединителях необходимо выполнить **полный импорт**, а при изменении стандартного правила синхронизации — **полную синхронизацию**. Во время обновления Azure AD Connect определяет действия синхронизации, которые необходимо выполнить, и записывает их в виде *переопределений*. В следующем цикле синхронизации планировщик синхронизации выбирает и выполняет эти переопределения. После успешного выполнения переопределения оно удаляется.

Возможны ситуации, при которых не нужно выполнять эти переопределения сразу же после обновления. Например, у вас есть много синхронизируемых объектов и вы хотите выполнить эти шаги синхронизации после рабочих часов. Чтобы удалить эти переопределения, сделайте следующее:

1. Во время обновления **снимите** флажок **Запустить синхронизацию сразу после завершения настройки**. Это отключит планировщик синхронизации и предотвратит автоматическое выполнение цикла синхронизации, прежде чем переопределения будут удалены.

   ![DisableFullSyncAfterUpgrade](./media/how-to-upgrade-previous-version/disablefullsync01.png)

2. После завершения обновления выполните следующий командлет, чтобы узнать, какие переопределения были добавлены: `Get-ADSyncSchedulerConnectorOverride | fl`

   >[!NOTE]
   > Переопределения зависят от соединителя. В указанном ниже примере в локальный соединитель AD и соединитель Azure AD был добавлен шаг для полного импорта и полной синхронизации.

   ![DisableFullSyncAfterUpgrade](./media/how-to-upgrade-previous-version/disablefullsync02.png)

3. Запишите добавленные переопределения.
   
4. Чтобы удалить переопределения для полного импорта и полной синхронизации на произвольном соединителе, выполните следующий командлет: `Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid-of-ConnectorIdentifier> -FullImportRequired $false -FullSyncRequired $false`

   Чтобы удалить переопределения во всех соединителях, выполните следующий сценарий PowerShell:

   ```
   foreach ($connectorOverride in Get-ADSyncSchedulerConnectorOverride)
   {
       Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier $connectorOverride.ConnectorIdentifier.Guid -FullSyncRequired $false -FullImportRequired $false
   }
   ```

5. Чтобы возобновить работу планировщика, выполните следующий командлет: `Set-ADSyncScheduler -SyncCycleEnabled $true`

   >[!IMPORTANT]
   > Не забудьте выполнить необходимые шаги синхронизации при первой возможности. Вы можете выполнить эти действия вручную с помощью Synchronization Service Manager или вернуть переопределения с помощью командлета Set-ADSyncSchedulerConnectorOverride.

Чтобы добавить переопределения для полного импорта и полной синхронизации на произвольном соединителе, выполните следующий командлет: `Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid> -FullImportRequired $true -FullSyncRequired $true`

## <a name="troubleshooting"></a>Устранение неполадок
В следующем разделе приведены сведения об устранении неполадок, которые можно использовать, если у вас возникнет проблема с обновлением Azure AD Connect.

### <a name="azure-active-directory-connector-missing-error-during-azure-ad-connect-upgrade"></a>Ошибка из-за отсутствия соединителя Azure Active Directory при обновлении Azure AD Connect

При обновлении предыдущей версии Azure AD Connect в начале этого процесса может произойти приведенная ниже ошибка. 

![Ошибка](./media/how-to-upgrade-previous-version/error1.png)

Эта ошибка происходит потому, что соединитель Azure Active Directory с идентификатором b891884f-051e-4a83-95af - 2544101c 9083 не существует в текущей конфигурации Azure AD Connect. Чтобы убедиться в этом, откройте окно PowerShell и выполните командлет `Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083`.

```
PS C:\> Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083
Get-ADSyncConnector : Operation failed because the specified MA could not be found.
At line:1 char:1
+ Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ReadError: (Microsoft.Ident...ConnectorCmdlet:GetADSyncConnectorCmdlet) [Get-ADSyncConne
   ctor], ConnectorNotFoundException
    + FullyQualifiedErrorId : Operation failed because the specified MA could not be found.,Microsoft.IdentityManageme
   nt.PowerShell.Cmdlet.GetADSyncConnectorCmdlet

```

Командлет PowerShell выдаст сообщение об ошибке **the specified MA could not be found** (Не удалось найти указанный MA).

Причина заключается в том, что обновление текущей конфигурации Azure AD Connect не поддерживается. 

Если требуется установить более новую версию Azure AD Connect, закройте мастер Azure AD Connect. Удалите существующий мастер Azure AD Connect. Выполните чистую установку более новой версии Azure AD Connect.



## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения об интеграции локальных удостоверений см. в статье [Подключение Active Directory к Azure Active Directory](whatis-hybrid-identity.md).
