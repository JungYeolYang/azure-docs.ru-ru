---
title: Пример скрипта Azure CLI. Создание и отправка задания | Документация Майкрософт
description: В скрипте Azure CLI в этой статье показано, как отправить задание для простого преобразования кодирования с помощью URL-адреса HTTPs.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/01/2019
ms.author: juliako
ms.openlocfilehash: e4f2d4f0e7dd9380ba708d84dfe7588043fd4a68
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236702"
---
# <a name="cli-example-create-and-submit-a-job"></a>Пример использования CLI. Создание и отправка задания

При отправке заданий из Служб мультимедиа версии 3 необходимо указать расположение входного видео. Это можно сделать, указав URL-адрес HTTPS как входные данные задания (как показано в этой статье). 

## <a name="prerequisites"></a>Предварительные требования 

[Создание учетной записи Служб мультимедиа](create-account-cli-how-to.md).

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Пример сценария

При запуске `az ams job start` можно задать метку для выходных данных задания. Метку можно использовать позже, чтобы определить, для чего этот выходной ресурс. 

- Если вы присваиваете значение метке, задайте для "--output-assets" значение "assetname=label".
- Если вы не присваиваете значение метке, задайте для "--output-assets" значение "assetname=".
  Обратите внимание, что вы добавляете "=" к `output-assets`. 

```azurecli
az ams job start \
  --name testJob001 \
  --transform-name testEncodingTransform \
  --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' \
  --files 'Ignite-short.mp4' \
  --output-assets testOutputAssetName= \
  -a amsaccount \
  -g amsResourceGroup 
```

Вы получите ответ следующего вида.

```
{
  "correlationData": {},
  "created": "2019-02-15T05:08:26.266104+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform/jobs/testJob001",
  "input": {
    "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
    "files": [
      "Ignite-short.mp4"
    ],
    "label": null,
    "odatatype": "#Microsoft.Media.JobInputHttp"
  },
  "lastModified": "2019-02-15T05:08:26.266104+00:00",
  "name": "testJob001",
  "outputs": [
    {
      "assetName": "testOutputAssetName",
      "error": null,
      "label": "",
      "odatatype": "#Microsoft.Media.JobOutputAsset",
      "progress": 0,
      "state": "Queued"
    }
  ],
  "priority": "Normal",
  "resourceGroup": "amsResourceGroup",
  "state": "Queued",
  "type": "Microsoft.Media/mediaservices/transforms/jobs"
}
```

## <a name="next-steps"></a>Дополнительная информация

[Общие сведения о службах мультимедиа](media-services-overview.md)