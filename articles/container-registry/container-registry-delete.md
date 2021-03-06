---
title: Удаление ресурсов образов в Реестре контейнеров Azure
description: Дополнительные сведения об эффективном управлении размером реестра путем удаления данных образа контейнера.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 04/04/2019
ms.author: danlep
ms.openlocfilehash: 1e496002c869c5d2c072773d37ed5fd5d4a5841e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60430816"
---
# <a name="delete-container-images-in-azure-container-registry"></a>Удаление образов контейнеров в службе "Реестр контейнеров Azure"

Чтобы управлять размером Реестра контейнеров Azure, необходимо периодически удалять данные устаревших образов. Хотя некоторые образы контейнеров, развернутые в рабочей среде, могут потребовать долговременного хранения, другие, как правило, могут быть удалены быстрее. Например, в сценарии автоматизированной сборки и тестирования реестр может быть быстро заполнен изображениями, которые никогда не придется развертывать, и после того, как завершится этап построения и тестирования, они незамедлительно могут быть удалены.

Так как данные образов можно удалить несколькими способами, важно понимать, как каждая из операций удаления влияет на использование хранилища. Сначала в этой статье представлены компоненты реестра Docker и образов контейнера, а затем несколько методов удаления образа контейнера. Примеры сценариев предоставляются для автоматизации операций удаления.

## <a name="registry"></a>Реестр

*Реестр* контейнера — служба, которая хранит и распределяет образы контейнеров. Центр Docker является публичным реестром контейнеров Docker в то время, как Реестр контейнеров Azure предоставляет частный реестр контейнеров Docker в Azure.

## <a name="repository"></a>Репозиторий

Реестры контейнеров управляют *репозиториями*, коллекциями образов контейнеров с одинаковыми именами, но разными тегами. Например, следующие три образа находятся в репозитории acr-helloworld.

```
acr-helloworld:latest
acr-helloworld:v1
acr-helloworld:v2
```

Имена репозиториев также могут включать [пространство имен](container-registry-best-practices.md#repository-namespaces). Пространства имен, чтобы разрешить для группирования образов с использованием имен прямой разделением косыми чертами репозитория, например:

```
marketing/campaign10-18/web:v2
marketing/campaign10-18/api:v3
marketing/campaign10-18/email-sender:v2
product-returns/web-submission:20180604
product-returns/legacy-integrator:20180715
```

## <a name="components-of-an-image"></a>Компоненты образа

Образ контейнера вместе с реестром связан с одним или несколькими тегами, обладает одним или несколькими слоями и определяется с помощью манифеста. Общие сведения об этих компонентах, связанных друг с другом, могут помочь определить лучшие методы для освобождения места в реестре.

### <a name="tag"></a>Тег

*Тег* образа указывает его версию. Один образ без репозитория может быть назначен одному или нескольким тегам, а также может оставаться "без тега". То есть можно удалить все теги из образа, пока данных изображения (его слоев) остаются в реестре.

Репозиторий (или репозиторий и пространство имен) вместе с тегом определяют имя образа. Образ можно отправлять и получать. Для этого в операции передачи или получения необходимо указать его имя.

В частном реестре, например в Реестре контейнеров Azure, имя образа также включает полное имя узла реестра. Сервер реестра для образов в ACR имеет формат *acrname.azurecr.io* (прописными буквами). Например будет полное имя первое изображение в пространство имен «marketing» в предыдущем разделе:

```
myregistry.azurecr.io/marketing/campaign10-18/web:v2
```

Дополнительные сведения по добавлению тегов к образам см. в записи блога MSDN [Docker Tagging: Best practices for tagging and versioning docker images][tagging-best-practices] (Добавление тегов в Docker. Рекомендации по добавлению тегов и версий в образы Docker).

### <a name="layer"></a>Слой

Образы создаются для одного или нескольких *слоев*, каждый из которых соответствует строке в Dockerfile, которая определяет образ. Образы в реестре используют слои совместно, что позволяет увеличить эффективность хранилища. Например, несколько образов, которые находятся в разных репозиториях, могут совместно использовать базовый слой Alpine Linux, но только одна копия из этого слоя может хранится в реестре.

Совместное использование слоя также оптимизирует распределение слоев для узлов с несколькими образами, совместно использующих общие слои. Например, если образ уже находится в узле, который включает в себя уровень Alpine Linux в качестве основы, последующее притяжение другого образа, ссылающегося на один и тот же слой, не переносит его слой на узел. Вместо этого он ссылается на слой, который уже существует на узле.

### <a name="manifest"></a>Manifest

Каждый образ контейнера, который отправляется в реестр контейнеров, связан с *манифестом*. Манифест создается реестром во время отправки образа, уникально идентифицирует образ и указывает его слой. С помощью команды Azure CLI [az acr repository show-manifests][az-acr-repository-show-manifests] можно получить список манифестов репозитория.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>
```

Пример перечисления дайджестов манифестов для репозитория acr-helloworld.

```console
$ az acr repository show-manifests --name myregistry --repository acr-helloworld
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  },
  {
    "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
    "tags": [
      "v1"
    ],
    "timestamp": "2018-07-11T21:38:35.9170967Z"
  }
]
```

Манифест, рассматриваемый в этой статье, отличается от манифеста образа, который можно просмотреть на портале Azure или с помощью команды [docker manifest inspect][docker-manifest-inspect]. В следующем разделе "дайджест манифеста" ссылается на дайджест, созданный операцией отправки, а не *config.digest* в манифесте образа. Извлекать и удалять образы можно с помощью **дайджеста манифеста**, а не с помощью config.digest. На следующем изображении приведены два типа дайджестов.

![Дайджест манифеста и config.digest на портале Azure][manifest-digest]

### <a name="manifest-digest"></a>Дайджест манифеста

Манифесты идентифицируются с помощью уникального хэша SHA-256 или *дайджеста манифеста*. Каждый образ, к которому был или не был применен тег, идентифицируется по дайджесту. Значение дайджеста уникально, даже если данные слоя образа идентичны данным другого образа. Данный механизм позволяет многократно отправлять образы с идентичными тегами в реестр. Например, можно многократно отправлять `myimage:latest` в реестр, при этом не получая сообщений об ошибке. Это происходит из за того, что каждый образ идентифицируется по уникальному дайджесту.

Чтобы извлечь образ из реестра, необходимо указать его дайджест в операции передачи. Некоторые системы можно настроить на извлечения с помощью дайджеста, так как он гарантирует, что будет извлечена запрашиваемая версия образа. Операция будет выполнена, даже если образы, которые были идентично помечены, впоследствии будут перенесены в реестр.

Пример получения образа из репозитория acr-helloworld с помощью дайджеста манифеста.

```console
$ docker pull myregistry.azurecr.io/acr-helloworld@sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108
```

> [!IMPORTANT]
> Если многократно отправлять измененные образы с идентичными тегами, можно создать потерянные образы, которые остались без тегов, но все еще потребляют пространство реестра. Образы без тегов не отображаются в Azure CLI или на портале Azure при просмотре списка или образов по тегу. Тем не менее их слои по-прежнему существуют и занимают место в реестре. В разделе [Удаление образов без тегов](#delete-untagged-images) этой статьи рассматривается освобождение пространства используемого образами без тегов.

## <a name="delete-image-data"></a>Удаление данных образа

Удалить данные образа из реестра контейнеров можно несколькими способами.

* Удаление [репозитория](#delete-repository). Это действие удаляет из репозитория все образы и все уникальные слои.
* Удаление [тегов](#delete-by-tag). Это действие удаляет образ, тег и все уникальные слои, связанные с образом, и все другие теги, связанные с образом.
* Удаление [дайджеста манифеста](#delete-by-manifest-digest). Удаление образа, всех уникальных слоев в образе и всех тегов, связанных с образом.

## <a name="delete-repository"></a>Удаление репозитория

При удалении репозитория удаляются все образы хранилища, включая все теги, уникальные слои и манифесты. При удалении репозитория восстанавливается дисковое пространство, используемое образами, которые в нем находились.

Следующая команда Azure CLI удаляет репозиторий acr-helloworld и все теги и манифесты репозитория. Если слои, на которые ссылаются удаляемые манифесты, никак не связаны с другими образами реестра, их данные слоя также удаляются.

```azurecli
 az acr repository delete --name myregistry --repository acr-helloworld
```

## <a name="delete-by-tag"></a>Удаление по тегу

С помощью операции удаления можно удалить отдельные образы из репозитория по указанным именам репозиториев и тегов. При удалении по тегу восстанавливается дисковое пространство, используемое любыми уникальными слоями в образе (слои, которые не используются совместно с другими образами в реестре).

Чтобы удалить тег, используйте команду [az acr repository delete][az-acr-repository-delete] и укажите имя образа в параметре `--image`. Удаляются все уникальные слои образа и все другие теги, связанные с удаляемым образом.

Пример удаления образа acr-helloworld:latest из реестра myregistry.

```azurecli
$ az acr repository delete --name myregistry --image acr-helloworld:latest
This operation will delete the manifest 'sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108' and all the following images: 'acr-helloworld:latest', 'acr-helloworld:v3'.
Are you sure you want to continue? (y/n): y
```

> [!TIP]
> Удаление *по тегу* не следует путать с удалением тега (обестегивание). Удалить тег можно с помощью команды Azure CLI [az acr repository untag][az-acr-repository-untag]. Отсутствует пространство освобождается в том случае, когда тег изображения, так как его [манифеста](#manifest) и уровень данных остаются в реестре. Удаляется только ссылка на тег.

## <a name="delete-by-manifest-digest"></a>Удаление с помощью дайджеста манифеста

[Дайджест манифеста](#manifest-digest) может быть связан с одним, несколькими тегами или не иметь связей с тегами вообще. При удалении с помощью дайджеста все теги, связанные манифестом, удаляются, как и данные слоя для любых слоев, уникальных для образа. Данные общего слоя не удаляются.

Для удаления с помощью дайджеста сначала перечислите дайджесты манифеста для репозитория, содержащего изображения, которые необходимо удалить. Например: 

```console
$ az acr repository show-manifests --name myregistry --repository acr-helloworld
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  }
]
```

Затем в команде [az acr repository delete][az-acr-repository-delete] необходимо указать дайджест, который требуется удалить. Команда имеет следующий формат.

```azurecli
az acr repository delete --name <acrName> --image <repositoryName>@<digest>
```

Пример удаления последнего манифеста из списка выходных данных (с тегом "v2").

```console
$ az acr repository delete --name myregistry --image acr-helloworld@sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57
This operation will delete the manifest 'sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57' and all the following images: 'acr-helloworld:v2'.
Are you sure you want to continue? (y/n): y
```

`acr-helloworld:v2` Образ будет удален из реестра, так как все данные слоя, уникальные для этого образа. Если манифест связан с несколькими тегами, все связанные теги также удаляются.

### <a name="list-digests-by-timestamp"></a>Список выборки по метке времени

Чтобы поддерживался размер репозитория или реестра, может потребоваться периодически удалять манифеста дайджестов старше определенной даты.

Следующая команда Azure CLI выводит все манифеста хэш-кода в репозитории, старше указанной отметке времени, в порядке возрастания. Замените `<acrName>` и `<repositoryName>` значениями, уместными для вашей среды. Метка времени может быть выражение с даты времени или даты, как в следующем примере.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> \
--orderby time_asc -o tsv --query "[?timestamp < '2019-04-05'].[digest, timestamp]"
```

### <a name="delete-digests-by-timestamp"></a>Удалить дайджестов по метке времени

После определения устаревших манифеста дайджестов, можно запустить следующий сценарий Bash, чтобы удалить манифеста дайджестов старше указанной отметке времени. Для работы этого сценария требуется Azure CLI и **xargs**. Сценарий не выполняет удаление по умолчанию. Чтобы включить удаление образа, измените значение `ENABLE_DELETE` на `true`.

> [!WARNING]
> Используйте приведенный ниже пример сценария с осторожностью--данные удаленных изображения — неисправимая. Если у вас есть системы, которые извлекают образы с манифеста хэш-кода (в отличие от имени образа), не следует запускать эти скрипты. Если удалить манифеста дайджестов не этих систем извлекать образы из реестра. Вместо получения с помощью манифеста попробуйте внедрить схему *уникальных тегов*, которая [рекомендуется в качестве лучшей методики][tagging-best-practices]. 

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
# TIMESTAMP can be a date-time string such as 2019-03-15T17:55:00.
REGISTRY=myregistry
REPOSITORY=myrepository
TIMESTAMP=2019-04-05  

# Delete all images older than specified timestamp.

if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
    --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
   --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].[digest, timestamp]" -o tsv
fi
```

## <a name="delete-untagged-images"></a>Удаление образов без тегов

Как было упомянуто в разделе [Дайджест манифеста](#manifest-digest), в результате отправки измененных образов с помощью существующего тега **untags** ранее отправленный образ стал потерянным (или "несвязанным"). Манифест образа, который был отправлен ранее, и его данные слоя остаются в реестре. Рассмотрим следующую последовательность событий.

1. Отправка образа *acr-helloworld* с тегом **latest**. `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Проверка манифеста для репозитория *acr-helloworld*.

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

1. Изменение Dockerfile репозитория *acr-helloworld*.
1. Отправка образа *acr-helloworld* с тегом **latest**. `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Проверка манифеста для репозитория *acr-helloworld*.

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:38:35.9170967Z"
     },
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

Как показано в выходных данных последнего шага в последовательности, теперь установлена потерянной манифеста, `"tags"` свойство является пустым списком. Этот манифест существует в реестре вместе с уникальными данными слоя, на которые он ссылается. **Чтобы удалить потерянные образы и их данные слоя, необходимо удалить дайджест манифеста**.

### <a name="list-untagged-images"></a>Список образов без тегов

С помощью следующей команды Azure CLI можно вывести список всех образов репозитория без тегов. Замените `<acrName>` и `<repositoryName>` значениями, уместными для вашей среды.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> --query "[?tags[0]==null].digest"
```

### <a name="delete-all-untagged-images"></a>Удаление всех образов без тегов

> [!WARNING]
> С осторожностью используйте следующие примеры сценариев — данные удаленных изображений восстановлению не подлежат. Если у вас есть системы, которые извлекают образы с манифеста хэш-кода (в отличие от имени образа), не следует запускать эти скрипты. Удаление образов без тегов не позволит этим системам получать образы из реестра. Вместо получения с помощью манифеста попробуйте внедрить схему *уникальных тегов*, которая [рекомендуется в качестве лучшей методики][tagging-best-practices].

**Azure CLI в Bash**

Следующий сценарий Bash удаляет из репозитория все образы, которые не были помечены тегом. Для работы этого сценария требуется Azure CLI и **xargs**. Сценарий не выполняет удаление по умолчанию. Чтобы включить удаление образа, измените значение `ENABLE_DELETE` на `true`.

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
REGISTRY=myregistry
REPOSITORY=myrepository

# Delete all untagged (orphaned) images
if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY  --query "[?tags[0]==null].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted. Set ENABLE_DELETE=true to enable image deletion."
fi
```

**Azure CLI в PowerShell**

Следующий сценарий PowerShell удаляет из репозитория все образы, которые не были помечены тегом. Для него требуется PowerShell и Azure CLI. Сценарий не выполняет удаление по умолчанию. Чтобы включить удаление образа, измените значение `$enableDelete` на `$TRUE`.

```powershell
# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to '$TRUE' to enable image delete
$enableDelete = $FALSE

# Modify for your environment
$registry = "myregistry"
$repository = "myrepository"

if ($enableDelete) {
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null].digest" -o tsv `
    | %{ az acr repository delete --name $registry --image $repository@$_ --yes }
} else {
    Write-Host "No data deleted. Set `$enableDelete = `$TRUE to enable image deletion."
}
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об образе хранилища в реестре контейнеров Azure см. в статье [Хранение образа контейнера в реестре контейнеров Azure](container-registry-storage.md).

<!-- IMAGES -->
[manifest-digest]: ./media/container-registry-delete/01-manifest-digest.png

<!-- LINKS - External -->
[docker-manifest-inspect]: https://docs.docker.com/edge/engine/reference/commandline/manifest/#manifest-inspect
[portal]: https://portal.azure.com
[tagging-best-practices]: https://blogs.msdn.microsoft.com/stevelasker/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-repository-untag]: /cli/azure/acr/repository#az-acr-repository-untag
