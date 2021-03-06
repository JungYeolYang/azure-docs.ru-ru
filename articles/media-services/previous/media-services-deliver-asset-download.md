---
title: Скачивание ресурсов служб мультимедиа на компьютер в Azure | Документация Майкрософт
description: Инструкции по загрузке ресурсов на компьютер. Примеры кода написаны на языке C# и используют пакет SDK служб мультимедиа для .NET.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 8908a1dd-3ffb-4f18-955d-4c8e2d82fc5d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 21fcc6ae09718ffbb22e1d438926586dd3cde71d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465666"
---
# <a name="how-to-deliver-an-asset-by-download"></a>Практическое руководство: доставка актива путем скачивания  
В этой статье рассматриваются возможности доставки файлов мультимедиа, переданных в Службы мультимедиа. Для доставки контента служб мультимедиа можно использовать различные сценарии. После кодирования скачайте созданные файлы мультимедиа или получите к ним доступ с помощью указателя потоковой передачи. Для повышения производительности и масштабируемости можно также доставлять контент с помощью сети доставки содержимого (CDN).

В этом примере показано, как загрузить мультимедийные активы из служб мультимедиа на локальный компьютер. Код запрашивает задания, связанные с учетной записью служб мультимедиа, по идентификатору задания и обращается к соответствующей коллекции **OutputMediaAssets** (которая представляет собой набор из одного или нескольких выходных мультимедийных активов, являющихся результатом выполнения задания). В этом примере показано, как загрузить выходные мультимедийные активы из задания, но вы можете применить тот же подход для загрузки других активов.

>[!NOTE]
>Действует ограничение в 1 000 000 записей для разных политик AMS (например, для политики Locator или ContentKeyAuthorizationPolicy). Указывайте один и тот же идентификатор политики, если вы используете те же дни, разрешения доступа и т. д., например политики для указателей, которые должны оставаться на месте в течение длительного времени (политики запрета передачи). Дополнительные сведения см. в [этой статье](media-services-dotnet-manage-entities.md#limit-access-policies).

```csharp
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference to the job. 
        IJob job = GetJob(jobId);

        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);

        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };

        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;

            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);

            Console.WriteLine("File download path:  " + localDownloadPath);

            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));

            outputFile.DownloadProgressChanged -= DownloadProgress;
        }

        Task.WaitAll(downloadTasks.ToArray());

        return outputAsset;
    }

    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }
```


## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>См. также
[Доставка потокового контента](media-services-deliver-streaming-content.md)

