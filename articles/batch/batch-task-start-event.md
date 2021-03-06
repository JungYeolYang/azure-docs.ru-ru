---
title: Событие начала выполнения задачи пакетной службы Azure | Документы Майкрософт
description: Справочник по событию начала выполнения задачи пакетной службы.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
origin.date: 04/20/2017
ms.date: 05/15/2018
ms.author: v-junlch
ms.openlocfilehash: d50a0a7082e409084fd966370934a638ca9bb013
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60549875"
---
# <a name="task-start-event"></a>Событие начала выполнения задачи

 Это событие возникает, когда планировщик планирует задачу для запуска на вычислительном узле. Обратите внимание, что если задача повторно выполняется или помещается в очередь, то для нее снова возникает данное событие, при этом число повторов и версия системной задачи изменяются соответствующим образом.


 Ниже приведен пример текста для события начала выполнения задачи.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "retryCount": 0
    }
}
```

|Имя элемента|type|Примечания|
|------------------|----------|-----------|
|jobId|String|Идентификатор задания, содержащего задачу.|
|id|String|Идентификатор задачи.|
|taskType|String|Тип задачи. Может быть установлено значение "JobManager", указывающее, что это задача диспетчера заданий, или значение "User", указывающее, что задача не относится к диспетчеру заданий.|
|systemTaskVersion|Int32|Это внутренний счетчик повторных попыток для задачи. Пакетная служба может повторить попытку выполнения задачи для преодоления временных неполадок. В таким неполадкам относятся внутренние ошибки планирования и попытки восстановления из вычислительных узлов в неисправном состоянии.|
|[nodeInfo](#nodeInfo)|Сложный тип|Содержит сведения о вычислительном узле, где выполнялась задача.|
|[multiInstanceSettings](#multiInstanceSettings)|Сложный тип|Указывает, что задача включает в себя несколько экземпляров и требует несколько вычислительных узлов.  Дополнительные сведения см. в статье [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task).|
|[constraints](#constraints)|Сложный тип|Ограничения выполнения, применяемые к этой задаче.|
|[executionInfo](#executionInfo)|Сложный тип|Содержит сведения о выполнении задачи.|

###  <a name="nodeInfo"></a> nodeInfo

|Имя элемента|type|Примечания|
|------------------|----------|-----------|
|poolId|String|Идентификатор пула, где выполнялась задача.|
|nodeId|String|Идентификатор узла, где выполнялась задача.|

###  <a name="multiInstanceSettings"></a> multiInstanceSettings

|Имя элемента|type|Примечания|
|------------------|----------|-----------|
|numberOfInstances|Int|Число вычислительных узлов, необходимых задаче.|

###  <a name="constraints"></a> constraints

|Имя элемента|type|Примечания|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Максимальное количество повторных попыток для задачи. Пакетная служба пытается выполнить задачу повторно, если ее код выхода имеет ненулевое значение.<br /><br /> Обратите внимание, что это значение определяет количество повторных попыток. Пакетная служба попытается выполнить задачу один раз, а затем будет предпринимать повторные попытки вплоть до этого предела. Например, если максимальное число повторных попыток равно 3, пакетная служба пытается выполнить задачу до 4 раз (одна начальная попытка и 3 повторных).<br /><br /> Если максимальное число повторных попыток равно 0, пакетная служба не пытается выполнить задачи повторно.<br /><br /> Если максимальное число повторных попыток равно –1, пакетная служба может повторять попытки сколько угодно.<br /><br /> Значение по умолчанию — 0 (без повторных попыток).|

###  <a name="executionInfo"></a> executionInfo

|Имя элемента|type|Примечания|
|------------------|----------|-----------|
|retryCount|Int32|Число повторных попыток выполнения задачи пакетной службой. Повторная попытка выполнения задачи предпринимается в случае ненулевого кода выхода, вплоть до указанного предела MaxTaskRetryCount|

<!-- Update_Description: update metedata properties -->