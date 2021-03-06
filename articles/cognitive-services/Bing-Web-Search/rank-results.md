---
title: Как использовать ранжирование для отображения результатов поиска — API Bing для поиска в Интернете
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать ранжирование для отображения результатов поиска в API Bing для поиска в Интернете.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: BBF87972-B6C3-4910-BB52-DE90893F6C71
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 03/17/2019
ms.author: scottwhi
ms.openlocfilehash: 9a49c4af474d7f0618bf0cff1a093e5cbb62fe2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60648828"
---
# <a name="how-to-use-ranking-to-display-bing-web-search-api-results"></a>Как использовать ранжирование для отображения результатов в API Bing для поиска в Интернете  

Каждый ответ поиска содержит объект [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse), который описывает требуемый формат отображения результатов поиска. Ответ ранжирования объединяет результаты по содержимому основного поля и боковой панели традиционной страницы результатов поиска. Если вы не используете для отображения результатов стандартный формат основного поля и боковой панели, нужно по меньшей мере обеспечить более высокую заметность для содержимого основного поля.  

В каждой группе (основное поле или боковая панель) есть массив [Items](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items), который определяет порядок отображения содержимого. Каждый элемент предоставляет следующие два метода идентификации результатов в ответе.  

-   `answerType` и `resultIndex`. Поле `answerType` идентифицирует тип ответа (например, Webpage или News), а `resultIndex` идентифицирует результат в этом ответе (например, новостную статью). Нумерация индекса начинается с нуля.  

-   `value`. Поле `value` содержит идентификатор, который обозначает некоторый ответ или результат в этом ответе. Идентификатор может встречаться только в ответе или только в результате.  

Использовать идентификатор проще, так как требуется только согласовать идентификатор ранжирования и идентификатор определенного ответа или одного из его результатов. Если объект ответа содержит поле `id`, при отображении объедините все результаты из этого ответа. Например, если объект `News` содержит поле `id`, отобразите вместе все содержащиеся в нем новостные статьи. Если объект `News` не содержит поле `id`, тогда поле `id` должно включаться во все новостные статьи, и ответ ранжирования будет содержать новостные статьи наряду с результатами из других ответов.  

Использовать `answerType` и `resultIndex` немного сложнее. С помощью `answerType` можно найти ответ, который содержит результаты для отображения. Затем с помощью `resultIndex` следует определить порядок результатов в этом ответе и создать отображаемые результаты. (Значение `answerType` содержит имя поля в объекте [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse).) Если вам не нужно отображать вместе все результаты из ответа, элемент ранжирования в ответе не будет содержать поле `resultIndex`.  

## <a name="ranking-response-example"></a>Пример ответа ранжирования

Ниже приведен пример элемента [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse). Так как ответ при поиске в Интернете не включает в себя поле `id`, будут отображены все веб-страницы по отдельности в соответствии с ранжированием (каждая веб-страница содержит поле `id`). И так как найденные изображения, видео и связанные результаты поиска содержат поле `id`, будут отображены результаты для каждого из этих типов ответов вместе на основании ранжирования.

```json
{  
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214...",
        "totalEstimatedMatches" : 835000,
        "value" : [
            {
                "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
                "name" : "Motor Sports - Live at the race track ...",
                "url" : "http:\/\/www.bing.com\/cr?IG=96C4CF214A0A43...",
                "displayUrl" : "www.contoso.com\/usa\/eventsandracing\/motorsport",
                "snippet" : "Here you will find detailed information about racing...",
                "deepLinks" : [{
                    "name" : "Customer Racing",
                    "url" : "http:\/\/www.bing.com\/cr?IG=96C4CF214A0A43...",
                    "snippet" : "Customer racing news; General news..."
            },
            . . .  
        ]  
    }],  
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214A...",
        "isFamilyFriendly" : true,
        "value" : [
            {
                "name" : "2016 Supercar Wallpapers",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP...",
                "datePublished" : "2017-03-25T11:14:00",
                "contentUrl" : "http:\/\/www.contoso.com\/wall...",
                "hostPageUrl" : "http:\/\/www.bing.com\/cr?IG=96C4CF214...",
                "contentSize" : "373283 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "http:\/\/www.contoso.com\/lmp-...",
                "width" : 1920,
                "height" : 1080,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 168
                },
                "insightsSourcesSummary" : {
                    "shoppingSourcesCount" : 0,
                    "recipeSourcesCount" : 0
                }
            },
            . . .  
        ]  
    },  
    "relatedSearches" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches",
        "value" : [
            {
                "text" : "vintage racing teams",
                "displayText" : "vintage racing teams",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF2..."
            },
            . . .  
        ]  
    },  
    "videos" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Videos",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/videos...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214A...",
        "isFamilyFriendly" : true,
        "value" : [
            {
                "name" : "Why We Race",
                "description" : "A new era begins in motorsports this weekend...",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF2...",
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.Vo1...",
                "datePublished" : "2014-01-25T16:31:48",
                "publisher" : [
                    {
                        "name" : "Fabrikam"
                    }
                ],
                "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=oL...",
                "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214...",
                "encodingFormat" : "mp4",
                "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=oLAZgD...",
                "width" : 480,
                "height" : 360,
                "duration" : "PT2M42S",
                "motionThumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OM...",
                "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"http:\/\/www.you...<\/iframe>",
                "allowHttpsEmbed" : true,
                "viewCount" : 47325,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 168
                },
                "allowMobileEmbed" : true,
                "isSuperfresh" : false
            },
            . . .  
        ]  
    },  
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "WebPages",
                "resultIndex" : 0,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0"
                }
            },
            {
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 1,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 2,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.2"
                }
            },
            {
                "answerType" : "Videos",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Videos"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 3,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.3"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 4,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.4"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 5,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.5"
                }
            }]
        },
        "sidebar" : {
            "items" : [{
                "answerType" : "RelatedSearches",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches"
                }
            }]
        }
    }
}  
```  

На основании этого ответа ранжирования в основном поле отобразятся следующие результаты:  

-   первая найденная веб-страница;
-   все изображения;  
-   вторая и третья найденные веб-страницы;  
-   все видео;  
-   четвертая, пятая и шестая найденные веб-страницы.  

На боковой панели отобразятся следующие результаты поиска:  

-   все связанные результаты поиска.  


## <a name="next-steps"></a>Дальнейшие действия

Сведения о повышении уровня неранжированных результатов см. в разделе [Повышение уровня результатов, которые не ранжированы](./filter-answers.md#promoting-answers-that-are-not-ranked).

Сведения об ограничении числа ранжированных результатов в ответе см. в разделе [Ограничение числа результатов в ответе](./filter-answers.md#limiting-the-number-of-answers-in-the-response).

Пример на C#, использующий ранжирование для отображения результатов, доступен в [руководстве по использованию ранжирования на языке C#](./csharp-ranking-tutorial.md).
