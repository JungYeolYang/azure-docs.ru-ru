---
title: Вручную останавливать или запускать контейнеры в экземплярах контейнеров Azure
description: Узнайте, как вручную остановить или запустить группу контейнеров в экземплярах контейнеров Azure.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: 8e62d106a42dfbec897e5e14cf68fd3d7fd823c4
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65070816"
---
# <a name="manually-stop-or-start-containers-in-azure-container-instances"></a>Вручную останавливать или запускать контейнеры в экземплярах контейнеров Azure

[Политика перезапуска](container-instances-restart-policy.md) параметр группы контейнеров определяет, как экземпляры контейнеров запустить или остановить по умолчанию. Можно переопределить значение по умолчанию, вручную остановить или запустить группу контейнеров.

## <a name="stop"></a>Остановить

Вручную остановите выполняющейся группы контейнеров — например, с помощью [az контейнера stop] [ az-container-stop] команды или портала Azure. Для некоторых рабочих нагрузок контейнера может потребоваться остановить долго выполняющейся группы контейнеров после определенного периода для экономии затрат. 

*Когда группа контейнеров переходит в состояние Stopped, он завершается и перезапускает все контейнеры в группе. Не сохраняет состояние контейнера.*

Когда контейнеров перезапускаются, [ресурсы](container-instances-container-groups.md#resource-allocation) освобождаются и выставление счетов останавливается для группы контейнеров.

Действие остановки не оказывает влияния группы контейнеров уже завершен (находится в состоянии Succeeded или Failed). Например группа контейнеров с контейнера однократного запуска задачи, которые были выполнены успешно завершается в состоянии Succeeded. Пытается остановить группе, в том, что состояние не изменяют состояние. 

## <a name="start"></a>Начало

Группа контейнеров остановлен - либо потому, что контейнеры завершилась сами по себе или остановлена вручную группа — вы можете запустить контейнеры. Например, использовать [запуска контейнера az] [ az-container-start] команды или портала Azure, чтобы вручную запустить контейнеры в группе. Если образ какого-либо контейнера обновлен, извлекается новый образ. 

Запуск группы контейнеров начинает новое развертывание с той же конфигурацией контейнера. Это действие поможет вам повторно использовать известную конфигурацию группы контейнеров, которая работает необходимым образом. Вам не нужно будет создавать новую группу контейнеров для выполнения той же рабочей нагрузки.

Все контейнеры в группе контейнеров запускаются при выполнении этого действия. Не удается запустить конкретному контейнеру в группе.

После того как вы вручную запустите или перезапустите группу контейнеров, она будет работать в соответствии с настроенной политикой перезапуска.
  
## <a name="restart"></a>Перезагрузить

Вы можете перезапустить группу контейнеров, пока оно выполняется — например, с помощью [перезапуск контейнера az] [ az-container-restart] команды. Это действие перезагружает все контейнеры в группе контейнеров. Если образ какого-либо контейнера обновлен, извлекается новый образ. 

Перезапуск группы контейнеров полезен при устранении проблемы с развертыванием. Например, если временное ограничение ресурса препятствует успешной работе ваших контейнеров, перезапуск группы может решить проблему.

Все контейнеры в группе контейнеров перезапускаются этим действием. Не удается перезапустить конкретному контейнеру в группе.

После перезагрузки вручную группы контейнеров, запуски контейнера группы в соответствии с настроенной политику перезапуска.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [параметрах политики перезапуска](container-instances-restart-policy.md) в экземплярах контейнеров Azure.

В дополнение к остановки и запуска группу контейнеров с существующей конфигурацией вручную, вы можете [обновить параметры](container-instances-update.md) из выполняющейся группы контейнеров.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-restart]: /cli/azure/container?view=azure-cli-latest#az-container-restart
[az-container-start]: /cli/azure/container?view=azure-cli-latest#az-container-start
[az-container-stop]: /cli/azure/container?view=azure-cli-latest#az-container-stop
