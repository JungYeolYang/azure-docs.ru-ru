---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: fe1b4699a300831294c26b103d322fb83ad87d3b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65151095"
---
Чтобы настроить прокси-сервер HTTP для исходящих запросов, используйте следующие два аргумента.

| ИМЯ | Тип данных | ОПИСАНИЕ |
|--|--|--|
|HTTP_PROXY|строка|Используемый прокси-сервер, например `http://proxy:8888`.<br><proxy-url>|
|HTTP_PROXY_CREDS|строка|Любые учетные данные, необходимые для выполнения аутентификации на прокси-сервере, например username:password.|
|`<proxy-user>`|строка|Пользователь прокси-сервера.|
|`proxy-password`|строка|Пароль, связанный с параметром `<proxy-user>` прокси-сервера.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```