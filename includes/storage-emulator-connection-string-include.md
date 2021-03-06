---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 10/26/2018
ms.date: 01/14/2019
ms.author: v-jay
ms.openlocfilehash: 8c577db3e9f2bff9e86c3a7c37274630f90dd680
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125521"
---
Эмулятор хранения поддерживает одну предопределенную учетную запись и известный ключ аутентификации для аутентификации с помощью общего ключа. Эти учетная запись и ключ — единственные разрешенные учетные данные общего ключа для использования с эмулятором хранения. К ним относятся:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Ключ аутентификации, поддерживаемый эмулятором хранения, предназначен только для тестирования работы клиентского кода. Его использование не гарантирует защиту. Нельзя использовать рабочую учетную запись хранения и ключ для эмулятора хранения. Не следует использовать учетную запись для разработки с рабочими данными.
> 
> Эмулятор хранения поддерживает подключения только по протоколу HTTP. Тем не менее для доступа к ресурсам в рабочей учетной записи хранения Azure рекомендуется использовать протокол HTTPS.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Подключение к учетной записи эмулятора с помощью ярлыка
Самый простой способ подключиться к эмулятору хранения из приложения — настроить строку подключения в файле конфигурации приложения, на которое ссылается ярлык: `UseDevelopmentStorage=true`. Ниже приведен пример строки подключения к эмулятору хранения в файле *app.config*: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Подключение к учетной записи эмулятора с помощью известного имени и ключа учетной записи
Для создания строки подключения, которая содержит ссылку на имя учетной записи и ключ эмулятора, в строке подключения следует указать конечные точки для каждой службы, которую нужно использовать из эмулятора. Это необходимо, чтобы строка подключения содержала ссылки на конечные точки эмулятора, которые отличаются от конечных точек рабочей учетной записи хранения. Например, значение строки подключения будет выглядеть следующим образом:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Это значение идентично показанному выше ярлыку ( `UseDevelopmentStorage=true`).

#### <a name="specify-an-http-proxy"></a>Указание прокси-сервера HTTP
Если вы тестируете службу на эмуляторе хранения, можно также указать прокси-сервер HTTP. Это может быть полезно для отслеживания HTTP-запросов и ответов при отладке операций со службами хранилища. Чтобы указать прокси-сервер, добавьте параметр `DevelopmentStorageProxyUri` в строку подключения и присвойте ему значение URI прокси-сервера. В качестве примера приведена строка подключения, которая указывает эмулятор хранения и задает прокси-сервер HTTP:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

