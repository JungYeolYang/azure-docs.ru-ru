---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/06/2019
ms.author: alkohli
ms.openlocfilehash: 796b6280c2814e6358ce1942a230488cd484415d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61462449"
---
Чтобы устранить неполадки устройства, вам потребуется общения с группой поддержки Майкрософт. Технической поддержки Майкрософт может потребоваться использовать сеанс поддержки для входа в устройство.

1. Запустите Windows PowerShell от имени администратора.
2. Назначьте переменную для IP-адрес устройства. В командной строке выполните следующую команду:

    $ip = "<device_ip>"

    Замените `<device_ip>` IP-адресом вашего устройства.
 
3. Запустите сеанс Windows PowerShell на устройстве и подключение к пространству выполнения minishell.

    ```
    Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Force
    $minishellSession= New-PSSession -ComputerName $ip -ConfigurationName "Minishell" -Credential ~\EdgeUser
    ```
    При запросе укажите пароль, используемый для входа в устройство.

4. Подключиться к пространству выполнения сеанса поддержки.  

    ```
    Invoke-Command -Session $minishellSession -ScriptBlock { Enable-HcsSupportAccess }
    ```  
    Эта команда выводит зашифрованный ключ. Скопируйте эту строку в текстовый редактор, например в Блокнот.

5. Отправьте этот ключ специалисту службы поддержки по электронной почте. Инженер поддержки создаст соответствующий ключ доступа сеанса поддержки.

6. Сотрудник службы поддержки будет использовать ключ доступа в качестве пароля в следующей команде:

    ``` 
    $supportSession = New-PSSession -ComputerName $ip -Credential ~\EdgeSupport -ConfigurationName SupportSession
    Enter-PSSession -Session $supportSession
    ```
7. Теперь вы в сеанса поддержки. Следующий пример выходных данных показано, как подключиться к сеансу поддержки:

    ```
    PS C:\WINDOWS\system32> $ip = "10.100.10.10";
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Force
    PS C:\WINDOWS\system32> $minishellSession= New-PSSession -ComputerName $ip -ConfigurationName "Minishell" -Credential ~\EdgeUser
    WARNING: The Windows PowerShell interface of your device is intended to
    be used only for the initial network configuration. Please
    engage Microsoft Support if you need to access this interface
    to troubleshoot any potential issues you may be experiencing.
    Changes made through this interface without involving Microsoft
    Support could result in an unsupported configuration.
    PS C:\WINDOWS\system32> Invoke-Command -Session $minishellSession -ScriptBlock { Enable-HcsSupportAccess }
    UAAAADcAMQAyAEYAQwBDAEEANwA0ADIARABCADUANgA1ADkANQA0AEIAQgA0ADgAMQBEADEAMQAxADMAMgAyADYAOAA3AEIANwA0ADgAMwBFAEMAiix6gA6zBIELa6vbb73CSO9/Yo/g85QRp2g5ngw773sKgBVcirk5sTHFuSQXWRirggGEip9NI5m54iPcVxdIEcoH+2vlvxCAJVWXOLGOB6WqWDtzR3XWSwJKig95LZfBjtPO1sM5TZLu65iCRCB4AV9nOezhAoy2lGdTuZOpXP2w5FIZPFvmgR4+4m+pfsD0NQSsw+PD3hNBHvUUyIhc4WTVaIJbzoJBzg06uJnc6C0Zo1YnYD6u8SoevejXbt3dgU7m36Vg3K0qPfCPA9WNLd71uiUsV1lMLwCabj60rAuRp/qJlPRXPU5PbGeayKNQJQfUkQYh6afUxtOXohke+A==
    PS C:\WINDOWS\system32> $supportSession = New-PSSession -ComputerName $ip -Credential ~\EdgeSupport -ConfigurationName SupportSession
    PS C:\WINDOWS\system32> Enter-PSSession -Session $supportSession
    [10.100.10.10]: PS C:\Users\EdgeSupport\Documents>
    ```
    Если вам нужно получить тот же ключ зашифрованных доступа, который использовался при включении в сеансе поддержки, используйте `Get-HcsSupportAccessKey` командлета.

    ```
    [10.128.24.33]: PS C:\Users\EdgeSupport\Documents> Get-HcsSupportAccessKey
    UAAAADcAMQAyAEYAQwBDAEEANwA0ADIARABCADUANgA1ADkANQA0AEIAQgA0ADgAMQBEADEAMQAxADMAMgAyADYAOAA3AEIANwA0ADgAMwBFAEMAiix6gA6zBIELa6vbb73CSO9/Yo/g85QRp2g5ngw773sKgBVcirk5sTHFuSQXWRirggGEip9NI5m54iPcVxdIEcoH+2vlvxCAJVWXOLGOB6WqWDtzR3XWSwJKig95LZfBjtPO1sM5TZLu65iCRCB4AV9nOezhAoy2lGdTuZOpXP2w5FIZPFvmgR4+4m+pfsD0NQSsw+PD3hNBHvUUyIhc4WTVaIJbzoJBzg06uJnc6C0Zo1YnYD6u8SoevejXbt3dgU7m36Vg3K0qPfCPA9WNLd71uiUsV1lMLwCabj60rAuRp/qJlPRXPU5PbGeayKNQJQfUkQYh6afUxtOXohke+A==
    [10.128.24.33]: PS C:\Users\EdgeSupport\Documents>
    ```

    В сеансе поддержки остается включенной за 8 часов. Для отключения сеанса поддержки в любое время, используйте `Disable-HcsSupportAccess` командлета.

