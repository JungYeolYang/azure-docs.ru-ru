---
title: Потоки проверки подлинности (библиотека проверки подлинности Майкрософт) | Azure
description: Дополнительные сведения о проверки подлинности потоков и предоставление разрешений используются библиотеки аутентификации Майкрософт (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb9a6f162a10408469669cf40b29efc6d2903944
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/11/2019
ms.locfileid: "65546048"
---
# <a name="authentication-flows"></a>Потоки проверки подлинности

В этой статье описывается потоков проверки подлинности, предоставленные библиотеки проверки подлинности Майкрософт (MSAL).  Эти потоки можно использовать в разнообразных различных сценариях приложений.

| Поток | Описание | Используется в|  
| ---- | ----------- | ------- | 
| [Интерактивный](#interactive) | Получает токен через интерактивный процесс, который запрашивает у пользователя учетные данные с помощью браузера или pop окно. | [Классические приложения](scenario-desktop-overview.md), [мобильных приложений](scenario-mobile-overview.md) |
| [Неявное разрешение](#implicit-grant) | Позволяет приложению получать маркеры, не выполняя обмен учетными данными для внутреннего сервера. Так, приложение может авторизовать пользователей, поддерживать сеансы и получать маркеры для других веб-API — и все это из клиентского кода JavaScript.| [Одностраничных приложениях (SPA)](scenario-spa-overview.md) |
| [Код авторизации](#authorization-code) | Используется в приложениях, установленных на устройстве, чтобы получить доступ к защищенным ресурсам, таким как веб-API. Это позволяет добавить вход и доступ к API в мобильные и настольные приложения. | [Классические приложения](scenario-desktop-overview.md), [мобильных приложений](scenario-mobile-overview.md), [веб-приложения](scenario-web-app-call-api-overview.md) | 
| [On-behalf-of](#on-behalf-of) | Приложение вызывает службу или веб-API, который в свою очередь требуется вызывать другой службы или веб-API. Идея состоит в том, чтобы распространить делегированное удостоверение пользователя и разрешения с помощью цепочки запросов. | [Интерфейсы веб-API](scenario-web-api-call-api-overview.md) |
| [Учетные данные клиента](#client-credentials) | Обеспечивает доступ к Интернет-ресурсам, используя удостоверение приложения. Обычно используется для взаимодействия между серверами, которые должны выполняться в фоновом режиме без немедленного вмешательства пользователя. | [управляющая программа приложений](scenario-daemon-overview.md) |
| [Код устройства](#device-code) | Пользователи должны входить ограниченное ввода устройств, таких как смарт-ТВ, устройства Интернета вещей или принтер. | [Рабочий стол или мобильные приложения](scenario-desktop-acquire-token.md#command-line-tool-without-web-browser) |
| [Встроенная аутентификация Windows](scenario-desktop-acquire-token.md#integrated-windows-authentication) | Позволяет приложениям в домене или Azure AD присоединенных к компьютерам получать маркер в автоматическом режиме (без вмешательства пользовательского интерфейса от пользователя).| [Рабочий стол или мобильные приложения](scenario-desktop-acquire-token.md#integrated-windows-authentication) |
| [Имя пользователя и пароль](scenario-desktop-acquire-token.md#username--password) | Позволяет приложению вход пользователя непосредственно обрабатывая свой пароль. Этот поток не рекомендуется. | [Рабочий стол или мобильные приложения](scenario-desktop-acquire-token.md#username--password) | 

## <a name="interactive"></a>Интерактивный
MSAL поддерживает возможность интерактивно приглашение ввести свои учетные данные для входа и получения маркера с использованием этих учетных данных.

![Интерактивный поток](media/msal-authentication-flows/interactive.png)

Дополнительные сведения об использовании MSAL.NET интерактивного получения маркеров на определенных платформах см. ниже:
- [Xamarin Android](msal-net-xamarin-android-considerations.md)
- [Xamarin iOS](msal-net-xamarin-ios-considerations.md)
- [Универсальная платформа Windows](msal-net-uwp-considerations.md)

Дополнительные сведения о интерактивные вызовы в MSAL.js см. в статье [поведение в интерактивных запросов MSAL.js подсказки](msal-js-prompt-behavior.md)

## <a name="implicit-grant"></a>Неявное предоставление

MSAL поддерживает [потоке предоставления OAuth 2 неявное](v2-oauth2-implicit-grant-flow.md), который позволяет приложению для получения маркеров из платформы удостоверений Microsoft без выполнения внутреннего сервера exchange учетных данных. Так, приложение может авторизовать пользователей, поддерживать сеансы и получать маркеры для других веб-API — и все это из клиентского кода JavaScript.

![Неявный поток предоставления](media/msal-authentication-flows/implicit-grant.svg)

Многие современные веб-приложения создаются в виде одной страницы на стороне клиента приложения, написанные с использованием JavaScript или платформы SPA, например Angular, Vue.js и React.js. Эти приложения выполняются в веб-браузер и имеют характеристики проверки подлинности, отличный от традиционных серверных веб-приложений. Платформы удостоверений позволяет приложениям одной страницы входа в систему и получить маркеры для доступа к внутренним службам или веб-API с помощью неявного потока предоставления. Неявный поток позволяет приложению получить маркеры Идентификации представляют пользователя, прошедшего проверку подлинности и маркеры, требовалось вызывать защищенный API также доступа.

Этот поток проверки подлинности не поддерживает сценарии приложений, с помощью кросс платформенных инфраструктур JavaScript, например Electron и React-Native, так как они требуются дополнительные возможности для взаимодействия с платформ.

## <a name="authorization-code"></a>Код авторизации
MSAL поддерживает [предоставление кода авторизации OAuth 2](v2-oauth2-auth-code-flow.md), который может использоваться в приложениях, установленных на устройстве, чтобы получить доступ к защищенным ресурсам, таким как веб-API. Это позволяет добавить вход и доступ к API в мобильные и настольные приложения. 

При входе пользователей веб-приложений (веб-сайты), веб-приложение получает код авторизации.  Чтобы получить маркер для вызова веб-API является обменять код авторизации. В ASP.NET и ASP.NET core, веб-приложений, единственное предназначение этого `AcquireTokenByAuthorizationCode` — добавить маркер в кэш маркера, чтобы он может затем использоваться приложения (обычно в контроллерах), который просто получить маркер для API с помощью `AcquireTokenSilent`.

![Поток кода авторизации](media/msal-authentication-flows/authorization-code.png)

1. Запрашивает код авторизации, который используется для маркера доступа.
2. Использует маркер доступа для вызова веб-API.

### <a name="considerations"></a>Рекомендации
- Код авторизации можно использовать только один раз в обмене маркера. Не пытайтесь получить маркер несколько раз с тем же кодом авторизации (он запрещен явным образом в стандартной спецификации протокола). Если его активировать код несколько раз намеренно или не учитывать, что платформа также делает это автоматически, вы получите ошибку: `AADSTS70002: Error validating credentials. AADSTS54005: OAuth2 Authorization code was already redeemed, please retry with a new valid code or use an existing refresh token.`

- Если вы создаете приложение ASP.NET/ASP.NET Core, это может произойти, если не говорите ASP.NET/Core framework, вы уже погасили код авторизации. Для этого необходимо вызвать `context.HandleCodeRedemption()` метод `AuthorizationCodeReceived` обработчик событий.

- Избегайте совместного использования маркера доступа с помощью ASP.NET, что может помешать добавочное согласие, происходит правильно.  Дополнительные сведения см. в разделе [выдавать #693](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/693).

## <a name="on-behalf-of"></a>On-behalf-of

MSAL поддерживает [поток on-behalf-of аутентификации OAuth 2](v2-oauth2-on-behalf-of-flow.md).  Этот поток используется, когда приложение вызывает службу или веб-API, который в свою очередь требуется вызывать другой службы или веб-API. Идея состоит в том, чтобы распространить делегированное удостоверение пользователя и разрешения с помощью цепочки запросов. Служба среднего уровня для прошедшего проверку подлинности запросов к подчиненной службе она должна для защиты маркера доступа с платформой Microsoft identity, от имени пользователя.

![Поток On-Behalf-Of](media/msal-authentication-flows/on-behalf-of.png)

1. Получает маркер доступа для веб-API
2. (Web, рабочего стола, мобильных устройств, одностраничного приложения) клиент вызывает защищенный веб-API, добавление маркер доступа в качестве маркера носителя в заголовке проверки подлинности HTTP-запроса. Веб-API проверяет подлинность пользователя.
3. Когда клиент вызывает веб-API, веб-API запрашивает другой токен on-behalf-of пользователя.  
4. Защищенный веб-API использует этот маркер для вызова веб-API, подчиненных on-behalf-of пользователя.  Веб-API в дальнейшем может запрашивать маркеры для других подчиненных API (но по-прежнему от имени одного пользователя).

## <a name="client-credentials"></a>Учетные данные клиента

MSAL поддерживает [поток учетных данных клиента OAuth 2](v2-oauth2-client-creds-grant-flow.md). Этот поток позволяет получить доступ к Интернет-ресурсам, используя удостоверение приложения. Этот тип предоставления разрешения часто используется для взаимодействия между серверами, которое должно выполняться в фоновом режиме без непосредственного взаимодействия с пользователем. Приложения такого типа часто называются управляющих программ или учетные записи служб. 

Разрешает поток предоставления учетных данных клиента веб-службы (конфиденциальный клиент) использовать свои собственные учетные данные вместо олицетворения пользователя, для проверки подлинности при вызове другой веб-службы. В этом сценарии клиент обычно является веб-службой среднего уровня, службой управляющей программы или веб-сайтом. Для большей надежности платформа удостоверений Майкрософт также позволяет вызывающей службе использовать в качестве учетных данных сертификат (вместо общего секрета).

> [!NOTE]
> Поток конфиденциального клиента на мобильных платформах (UWP, Xamarin.iOS и Xamarin.Android), недоступен, так как они поддерживают только общедоступных клиентских приложений.  Открытый клиентские приложения не знаю, как для подтверждения удостоверения приложения для поставщика удостоверений. Безопасное подключение может осуществляться на веб-приложение или веб-API серверные решения путем развертывания сертификата.

MSAL.NET поддерживает два типа учетных данных клиента. Эти учетные данные клиента должны быть зарегистрированы с помощью Azure AD. Учетные данные передаются в конструкторы конфиденциального клиентского приложения в коде.

### <a name="application-secrets"></a>Секреты приложений 

![Конфиденциальный клиент с паролем](media/msal-authentication-flows/confidential-client-password.png)

1. Получает маркер, используя учетные данные секрета и пароль приложения.
2. Использует маркер для выполнения запросов ресурса.

### <a name="certificates"></a>Сертификаты 

![Конфиденциальный клиент с сертификатом](media/msal-authentication-flows/confidential-client-certificate.png)

1. Получает маркер с помощью учетных данных сертификата.
2. Использует маркер для выполнения запросов ресурса.

Эти учетные данные клиента должны быть:
- Зарегистрировано в Azure AD.
- Переданный во время создания объекта конфиденциального клиентского приложения в коде.


## <a name="device-code"></a>Код устройства
MSAL поддерживает [потока кода OAuth 2 устройства](v2-oauth2-device-code.md), который позволяет пользователям входить ограниченное ввода устройств, таких как смарт-ТВ, устройства Интернета вещей или принтера. Интерактивная проверка подлинности с помощью Azure AD требует веб-браузер. Поток кода устройства пользователь может использовать другое устройство (например другого компьютера или мобильного телефона) для входа в интерактивном режиме где устройство или операционная система не предоставляет веб-браузер.

С помощью потока кода устройства, приложение получает маркеры через двухэтапный процесс, специально предназначенный для этих устройств/OS. Примерами таких приложений являются приложения, работающие на устройствах Интернета вещей или средства командной строки (CLI). 

![Поток кода устройства](media/msal-authentication-flows/device-code.png)

1. Каждый раз, когда необходима проверка подлинности пользователя, приложение предоставляет код и предлагает пользователю, используйте другое устройство (например смартфонов с подключением к Интернету), чтобы перейти на URL-адрес (например, https://microsoft.com/devicelogin), где пользователю будет предложено ввести код. Что сделать, веб-страницу приводит пользователя посредством взаимодействия с обычной проверки подлинности, включая запросы согласия и многофакторной проверки подлинности, при необходимости.

2. После успешной проверки подлинности приложение командной строки получит необходимые маркеры по каналу обратной и будет использовать его для выполнения вызовов API web, которые необходимы.

### <a name="constraints"></a>Ограничения

- Поток кода устройства доступна только в общедоступных клиентских приложений.
- Полномочия, передаваемый при создании общедоступного клиентского приложения должен быть:
  - клиентский (в формате `https://login.microsoftonline.com/{tenant}/` где `{tenant}` — это GUID, представляющий идентификатор клиента или домен, связанный с клиентом.
  - или, все рабочие и учебные учетные записи (`https://login.microsoftonline.com/organizations/`).
- Личные учетные записи Майкрософт еще не поддерживаются конечной точкой версии 2.0 Azure AD (нельзя использовать `/common` или `/consumers` клиентов).

## <a name="integrated-windows-authentication"></a>Интегрированная проверка подлинности Windows
MSAL поддерживает встроенную проверку подлинности Windows (IWA) для рабочего стола или мобильных приложений, работающих на присоединенных к домену или Azure AD присоединенных к компьютеру Windows. С помощью IWA, эти приложения можно получить токен автоматически (без пользовательского интерфейса взаимодействия от пользователя). 

![Интегрированная проверка подлинности Windows](media/msal-authentication-flows/integrated-windows-authentication.png)

1. Получает маркер, используя встроенную проверку подлинности Windows.
2. Использует маркер для выполнения запросов ресурса.

### <a name="constraints"></a>Ограничения

Поддерживает IWA **федеративные** только для пользователей.  Пользователи создаются в Active Directory и на базе Azure Active Directory. Пользователи, созданные непосредственно в Azure AD, без резервного AD (**управляемых** пользователей) не может использовать этот поток проверки подлинности. Это ограничение не влияет на [имя пользователя и пароль потока](#usernamepassword).

Для приложений, написанных для платформ .NET Framework, .NET Core и универсальной платформы Windows — IWA.

IWA не обходить MFA (многофакторной проверки подлинности). Если настройки многофакторной проверки Подлинности IWA может завершиться ошибкой, если запрос MFA является обязательным, поскольку многофакторной проверки Подлинности требуется взаимодействие с пользователем. Такой – непростая задача. Неинтерактивный режим IWA, но авторизации двухфакторная (2FA) требует интерактивное взаимодействие с пользователем. Не требуется контролировать, когда поставщик удостоверений запрашивает 2FA должна выполнять, выполняет администратора клиента. Из наших наблюдений 2FA является обязательным при входе из другой страны, если не подключено через VPN-Подключение к корпоративной сети, а иногда даже в том случае, при подключении через VPN. Не ожидайте, что детерминированным набор правил, Azure Active Directory использует AI постоянно дополнительные Если 2FA не требуется. Должен воспользоваться пользователю запрос (https://aka.ms/msal-net-interactive) при сбое IWA.

Полномочия, передаваемый при создании общедоступного клиентского приложения должен быть:
- клиентский (в формате `https://login.microsoftonline.com/{tenant}/` где `tenant` — это guid, представляющий идентификатор клиента или домен, связанный с клиентом.
- для любой рабочих и учебных учетных записей (`https://login.microsoftonline.com/organizations/`). Личные учетные записи Майкрософт не поддерживаются (вы не можете использовать `/common` или `/consumers` клиентов).

Поскольку IWA приведена silent.
- пользователь приложения должен ранее согласились использовать приложение. 
- или администратор клиента должен ранее согласились всех пользователей в клиенте, чтобы использовать приложение.
- Это означает следующее.
    - либо вы как разработчик быть нажаты **Grant** кнопку на портале Azure для себя 
    - или администратор клиента нажал **согласия администратора Grant/revoke для {домен}** кнопку в **разрешения API** вкладка регистрации для приложения (см. в разделе [добавьте разрешения для веб-доступа к API](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis))
    - или вы предоставили пользователям предоставить согласие для приложения (см. в разделе [запрос на получение согласия пользователя](v2-permissions-and-consent.md#requesting-individual-user-consent))
    - или вы предоставили позволяет администратору клиентов предоставить согласие для приложения (см. в разделе [согласия администратора](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant))

Классические приложения .NET, .NET Core и приложениях универсальной платформы Windows включения потока IWA. Только перегрузка, принимающая имя пользователя на платформе .NET Core доступна как платформа .NET Core не может задавать имя пользователя для операционной системы.
  
Дополнительные сведения о согласие, см. в разделе [версии 2.0 разрешения и согласие](v2-permissions-and-consent.md).

## <a name="usernamepassword"></a>Имя пользователя или пароль 
MSAL поддерживает [предоставление пароля владельца ресурса OAuth 2](v2-oauth-ropc.md), который позволяет приложению вход пользователя непосредственно обрабатывая свой пароль. В приложения для настольных компьютеров можно использовать потоку имя пользователя и пароль, чтобы получить маркер без вмешательства пользователя. Пользовательский Интерфейс не является обязательным при использовании приложения.

![Имя пользователя и пароль потока](media/msal-authentication-flows/username-password.png)

1. Получает маркер, отправив имя пользователя и пароль на поставщика удостоверений.
2. Вызывает веб-API с помощью токена.

> [!WARNING]
> Этот поток является **не рекомендуется** так, как он необходим высокий уровень доверия и пользователем раскрытия.  Этот поток следует использовать только в том случае, когда потоки других, более безопасные, нельзя использовать. Дополнительные сведения об этой проблеме см. в разделе [в этой статье](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

Основной поток для получения маркера без уведомления на компьютерах, присоединенных к домену Windows [встроенную проверку подлинности Windows](#integrated-windows-authentication). В противном случае можно также использовать [потока кода устройства](#device-code)

Несмотря на то, что это полезно в некоторых случаях (сценарии DevOps), если вы хотите использовать имя пользователя и пароль в интерактивных сценариях, где можно ввести собственный пользовательский Интерфейс, как следует по думать о том, как отойти от его. С помощью имени пользователя и пароля, вы предоставляя доступ к ряд действий:
- основные концепции современного удостоверения: пароль получает fished, воспроизведения. Поскольку у нас есть эта концепция секрет общей папки, которые могут быть перехвачены.
Это несовместимо с без пароля.
- пользователей, которым необходим для выполнения многофакторной проверки Подлинности не будет входить в систему (как никак не взаимодействует)
- Пользователи не будут иметь возможность единого входа

### <a name="constraints"></a>Ограничения

Помимо [ограничения встроенной проверки подлинности Windows](#integrated-windows-authentication), также применяются следующие ограничения:

- Потоку имя пользователя и пароль не совместим с условным доступом и многофакторной проверки подлинности: Как следствие Если приложение выполняется в клиенте Azure AD, где администратор клиента требует многофакторной проверки подлинности, нельзя использовать этот поток. Во многих организациях для этого.
- Он работает только для рабочих и учебных учетных записей (не MSA)
- Поток можно найти в Классические приложения .NET и .NET core, но не в универсальной платформы Windows.

### <a name="azure-ad-b2c-specifics"></a>Особенности Azure AD B2C

Дополнительные сведения об использовании MSAL.NET и Azure AD B2C [с помощью ROPC с B2C Azure AD (MSAL.NET)](msal-net-aad-b2c-considerations.md#resource-owner-password-credentials-ropc-with-azure-ad-b2c).
