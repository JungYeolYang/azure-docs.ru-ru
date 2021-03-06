---
title: Определение фильтров в Службах мультимедиа Microsoft Azure
description: В этом разделе описывается создание фильтров, с помощью которых клиент может передавать определенные секции потока. Для достижения такой выборочной потоковой передачи службы мультимедиа создают динамические манифесты.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 05/07/2019
ms.author: juliako
ms.openlocfilehash: 3a562f98635d581aa320fdbd59d05a0382f09606
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465540"
---
# <a name="define-account-filters-and-asset-filters"></a>Определение фильтров учетных записей и ресурсов  

При доставке содержимого клиентам (потоковой трансляции событий или видео по запросу) клиент может потребоваться больше гибкости, чем описано в файле манифеста актива по умолчанию. Службы мультимедиа Azure позволяют определять фильтры учетной записи и фильтры ресурсов для содержимого. 

Фильтры — это серверные правила, с помощью которых пользователи могут выполнять различные действия, в том числе перечисленные ниже. 

- Воспроизведение только части видео (вместо воспроизведения видео целиком). Например:
  - Сокращение манифеста для отображения субклипа текущего события ("фильтрация субклипов") или
  - Обрезка начала видео ("обрезка видео").
- Доставка только указанных представлений и/или языковых дорожек, поддерживаемых устройством, на котором воспроизводится содержимое ("фильтрация представлений"). 
- Настройка окна представления (DVR) для предоставления ограниченной длительности окна DVR в проигрывателе ("настройка окна представления").

Службы мультимедиа предоставляют [динамические манифесты](filters-dynamic-manifest-overview.md) на основе предварительно определенных фильтров. После определения фильтров клиенты могут использовать их в URL-адресах потоковой передачи. Фильтры можно применять к протоколам потоковой передачи с переменной скоростью: Apple HTTP Live Streaming (HLS), MPEG-DASH и Smooth Streaming.

В представленной ниже таблице приводятся некоторые примеры URL-адресов с фильтрами.

|Протокол|Пример|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`<br/>HLS v3, используйте: `format=m3u8-aapl-v3`.|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="define-filters"></a>Определение фильтров

Существуют два типа фильтров активов. 

* [Фильтры учетных записей](https://docs.microsoft.com/rest/api/media/accountfilters) (глобальные) — применимы к любому ресурсу в учетной записи Служб мультимедиа Azure и действуют в течение всего времени ее существования.
* [Фильтры ресурсов](https://docs.microsoft.com/rest/api/media/assetfilters) (локальные) — применимы только к тому ресурсу, с которым фильтр был связан при создании, и действуют в течение времени существования ресурса. 

[Фильтры учетных записей](https://docs.microsoft.com/rest/api/media/accountfilters) и [ресурсов](https://docs.microsoft.com/rest/api/media/assetfilters) содержат одинаковые свойства для определения и описания фильтра. Во всех случаях, кроме создания **фильтра ресурса**, необходимо указать имя ресурса, с которым требуется связать фильтр.

В зависимости от сценария, вы выбираете наиболее подходящий тип фильтра (фильтр ресурса или учетной записи). Фильтры учетных записей подходят для профилей устройств (фильтрация представлений), в то время как фильтры ресурсов можно использовать для обрезки определенного ресурса.

Для описания фильтров можно использовать указанные ниже свойства. 

|ИМЯ|Описание|
|---|---|
|firstQuality|Первая качественная скорость фильтра.|
|presentationTimeRange|Диапазон времени презентации. Это свойство используется для фильтрации начальной и конечной точек манифеста, длины окна презентации и начального положения прямой трансляции. <br/>Дополнительные сведения см. в разделе [PresentationTimeRange](#presentationtimerange).|
|tracks|Условия выбора дорожек. Дополнительные сведения см. в разделе [tracks](#tracks).|

### <a name="presentationtimerange"></a>presentationTimeRange

Это свойство используется с **фильтрами ресурсов**. Не рекомендуем задавать это свойство для **фильтров учетных записей**.

|ИМЯ|Описание|
|---|---|
|**endTimestamp**|Применяется для видео по запросу (VoD).<br/>Презентации потоковой трансляции, он автоматически пропускается и применения вступления завершения презентации и потока видео по запросу.<br/>Это значение long, представляющее абсолютное конечную точку презентации, округленное до ближайшего следующего запуска GOP. Единица измерения — шкалы времени, так что endTimestamp 1800000000 бы в течение 3 минут.<br/>Используйте startTimestamp и endTimestamp для обрезки фрагментов, которые будут добавлены в список воспроизведения (манифест).<br/>Например, startTimestamp = 40000000 и endTimestamp = 100000000 шкалы времени по умолчанию формировать список воспроизведения, содержащий фрагменты в диапазоне от 4 секунды и 10 секунд презентации видео по запросу. Если фрагмент выходит за границы, он будет включен в манифест целиком.|
|**forceEndTimestamp**|Применяется к динамической потоковой передачи только.<br/>Указывает, должно ли свойство endTimestamp присутствовать. Значение true, если необходимо указать endTimestamp или возвращается код недопустимый запрос.<br/>Допустимые значения: false, true.|
|**liveBackoffDuration**|Применяется к динамической потоковой передачи только.<br/> Это значение определяет последней позиции в передаваемом, клиент может выполнять поиск в.<br/>С помощью этого свойства, можно отложить позиции воспроизведения в реальном времени и создать буфер на стороне сервера для игроков.<br/>Единица измерения для этого свойства — шкалы времени (см. ниже).<br/>Максимум динамической откладывание длительность составляет 300 секунд (3000000000).<br/>Например значение 2000000000 означает, что, в которые новейшее содержимое составляет 20 секунд отложить с реальных динамической границы.|
|**presentationWindowDuration**|Применяется к динамической потоковой передачи только.<br/>Используйте presentationWindowDuration для применения скользящего окна фрагментов для включения в список воспроизведения.<br/>Единица измерения для этого свойства — шкалы времени (см. ниже).<br/>Например, задайте значение presentationWindowDuration = 1200000000, чтобы применить двухминутное скользящее окно. В список воспроизведения будут включены данные мультимедиа за две минуты до крайней позиции прямой трансляции. Если фрагмент выходит за границы, он будет включен в список воспроизведения целиком. Минимальная продолжительность окна презентации составляет 60 секунд.|
|**startTimestamp**|Применяется к видео по запросу (VoD) или динамической потоковой передачи.<br/>Это значение long, представляющее точку абсолютное время начала потока. Оно округляется до начала ближайшей из следующих групп изображений. Единица измерения — шкалы времени, поэтому startTimestamp 150000000 будет 15 секунд.<br/>Используйте startTimestamp и endTimestampp для обрезки фрагментов, которые будут добавлены в список воспроизведения (манифест).<br/>Например, startTimestamp = 40000000 и endTimestamp = 100000000 шкалы времени по умолчанию формировать список воспроизведения, содержащий фрагменты в диапазоне от 4 секунды и 10 секунд презентации видео по запросу. Если фрагмент выходит за границы, он будет включен в манифест целиком.|
|**timescale**|Применяется ко всем отметки времени и длительности в диапазоне времени презентации, указанный как количество шагов приращения за одну секунду.<br/>Значение по умолчанию — 10000000 - десять миллионов приращения за одну секунду, где каждое приращение бы много 100 наносекунд.<br/>Например если вы хотите задать startTimestamp на 30 секунд, используется значение 300000000 при использовании шкалы времени по умолчанию.|

### <a name="tracks"></a>Дорожки

Укажите список условий свойство отслеживания фильтра (FilterTrackPropertyConditions) зависимости, на котором дорожки потока (потоковой трансляции или видео по запросу) должны быть включены в динамически созданный манифест. Фильтры объединяются с помощью логических операций **AND** и **OR**.

Условия фильтрации свойств дорожек описывают типы дорожек, значения (приведенные в таблице ниже) и операции (Equal, NotEqual). 

|ИМЯ|Описание|
|---|---|
|**Bitrate**|Фильтрация по скорости дорожки.<br/><br/>Рекомендуемое значение — диапазон скоростей (в битах в секунду). Пример: "0-2427000".<br/><br/>Примечание: можно использовать конкретное значение скорости, например 250000 (бит в секунду), но такой подход не рекомендуется, так как точная скорость может меняться от ресурса к ресурсу.|
|**FourCC**|Фильтрация по значению FourCC дорожки.<br/><br/>Это значение является первым элементом формата кодеков согласно стандарту [RFC 6381](https://tools.ietf.org/html/rfc6381). Сейчас поддерживаются перечисленные ниже кодеки. <br/>Для видео: "avc1", "hev1", "hvc1"<br/>Для звука: "mp4a", "ec-3"<br/><br/>Для определения значений FourCC для дорожек в ресурсе, получить и просмотрите файл манифеста.|
|**Язык**|Фильтрация по языку дорожки.<br/><br/>Это значение представляет собой тег языка, который требуется включить, как указано в стандарте RFC 5646. Например, en.|
|**Имя**|Фильтрация по имени дорожки.|
|**Тип**|Фильтрация по типу дорожки.<br/><br/>Допускаются следующие значения: "video", "audio" и "text".|

## <a name="associate-filters-with-streaming-locator"></a>Связать фильтры с указатель потоковой передачи

Можно указать список фильтров активов или учетной записи, которые относятся к вашей указатель потоковой передачи. [Для работы динамического упаковщика](dynamic-packaging-overview.md) применяет этот список фильтров с соответствующими клиент указывает в URL-адрес. Создает это сочетание [динамический манифест](filters-dynamic-manifest-overview.md), основанная на фильтры в URL-адрес + фильтры, укажите на указатель потоковой передачи. Мы рекомендуем использовать эту функцию, если вы хотите применить фильтры, но не требуется предоставлять имена фильтров в URL-адрес.

## <a name="definition-example"></a>Пример определения

```json
{
  "properties": {
    "presentationTimeRange": {
      "startTimestamp": 0,
      "endTimestamp": 170000000,
      "presentationWindowDuration": 9223372036854776000,
      "liveBackoffDuration": 0,
      "timescale": 10000000,
      "forceEndTimestamp": false
    },
    "firstQuality": {
      "bitrate": 128000
    },
    "tracks": [
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Audio"
          },
          {
            "property": "Language",
            "operation": "NotEqual",
            "value": "en"
          },
          {
            "property": "FourCC",
            "operation": "NotEqual",
            "value": "EC-3"
          }
        ]
      },
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Video"
          },
          {
            "property": "Bitrate",
            "operation": "Equal",
            "value": "3000000-5000000"
          }
        ]
      }
    ]
  }
}
```

## <a name="next-steps"></a>Дальнейшие действия

В указанных ниже статьях показано, как создавать фильтры программным способом.  

- [Create filters with REST APIs](filters-dynamic-manifest-rest-howto.md) (Создание фильтров с помощью интерфейсов REST API)
- [Создание фильтров с помощью .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Create filters with CLI](filters-dynamic-manifest-cli-howto.md) (Создание фильтров с помощью .NET)

