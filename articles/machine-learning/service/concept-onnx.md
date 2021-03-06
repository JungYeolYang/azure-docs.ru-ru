---
title: Высокая производительность, кросс-вывод платформы с ONNX
titleSuffix: Azure Machine Learning service
description: Дополнительные сведения о ONNX и ONNX среды выполнения для убыстрения моделей
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 04/24/2019
ms.custom: seodec18
ms.openlocfilehash: a8bc46011b00a0c63eddd2799ac1309b5754472e
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442416"
---
# <a name="onnx-and-azure-machine-learning-create-and-accelerate-ml-models"></a>ONNX и Машинное обучение Azure: Создание и ускорения моделей машинного Обучения

Узнайте, как с помощью [откройте нейронной сети Exchange](https://onnx.ai) (ONNX) может способствовать оптимизации моделей машинного обучения.

Оптимизации моделей машинного обучения для вывода сложно, так как требуется для настройки модели и определение библиотеки по максимуму возможности оборудования. Проблема становится предельно сложна, если вы хотите получить оптимальную производительность на различные платформы (облака и Microsoft edge, CPU и GPU, т. д.), поскольку каждое одна имеет различные возможности и характеристики. Сложность возрастает, если у вас есть моделей из различных платформ, которые должны выполняться на различных платформах. Это очень много времени для оптимизации все различные комбинации платформы и оборудования. Требуется решение для обучения один раз в вашей основной платформы и запуск из любого места в облаке или edge. Это, когда вступает в дело ONNX.

Корпорация Майкрософт и сообщества партнеров создан ONNX как открытый стандарт для представления моделей машинного обучения. Моделирует из [множество платформ](https://onnx.ai/supported-tools) включая TensorFlow, PyTorch, SciKit-Learn, Keras, Chainer, MXNet и MATLAB возможность экспорта или преобразуются в стандартный формат ONNX. После определения модели в формат ONNX, они могут выполняться на различных платформах и устройствах.

[Среда выполнения ONNX](https://github.com/Microsoft/onnxruntime) — это механизм определения высокой производительности для развертывание моделями ONNX в рабочей среде. Он оптимизирован для облачные и пограничные и работает в Linux, Windows и Mac. На языке C++, он также имеет C, Python, и C# API-интерфейсы. Среда выполнения ONNX обеспечивает поддержку для всех спецификации ONNX-ML и также интегрируется с ускорители на другом оборудовании, например TensorRT на графических процессоров NVidia.

Среда выполнения ONNX используется в большом масштабе служб Майкрософт, таких как Bing, Office и Cognitive Services. Выигрыш в производительности зависят от ряда факторов, но уже видели эти службы Microsoft __среднее двукратный прирост производительности на ЦП__. Среда выполнения ONNX также используется как часть Windows ML на сотни миллионов устройств. Можно использовать среду выполнения с помощью служб машинного обучения Azure. С помощью среды выполнения ONNX, преимущества можно получить обширные производственного уровня оптимизации, тестирования и текущих улучшения.

[![ONNX блок-схема обучения, преобразователи типов и развертывания](media/concept-onnx/onnx.png)](./media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>Получение моделей ONNX

Модели ONNX можно получить различными способами:
+ Обучения новой модели ONNX в службе машинного обучения Azure (см. Примеры в нижней части этой статьи.)
+ Преобразовать существующую модель из другого формата в ONNX (см. в разделе [учебники](https://github.com/onnx/tutorials)) 
+ Получение предварительно обученной модели ONNX из [Zoo модели ONNX](https://github.com/onnx/models) (см. Примеры в нижней части этой статьи.)
+ Создание настраиваемых моделей ONNX в [пользовательской службе визуального распознавания Azure](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/). 

Множество моделей, включая классификации изображений, обнаружение и обработку текста может быть представлено как моделями ONNX. Тем не менее некоторые модели не может иметь возможность успешного преобразования. При возникновении такой ситуации, сообщите о проблеме в GitHub соответствующего преобразователя, который вы использовали. Вы можете использовать существующий формат модели, пока проблема была устранена.

## <a name="deploy-onnx-models-in-azure"></a>Развертывание моделей ONNX в Azure

С помощью службы машинного обучения Azure можно развертывать модели ONNX, управлять ими и осуществлять мониторинг. Используя стандартный [рабочий процесс развертывания](concept-model-management-and-deployment.md) и среду выполнения ONNX, можно создать конечную точку REST, размещенную в облаке. См. в разделе примеров записных книжек Jupyter в конце этой статьи, чтобы попробовать это на себя. 

### <a name="install-and-use-onnx-runtime-with-python"></a>Установка и использование среды выполнения ONNX с помощью Python

Пакеты Python для среды выполнения ONNX будут доступны на [PyPi.org](https://pypi.org) ([ЦП](https://pypi.org/project/onnxruntime), [GPU](https://pypi.org/project/onnxruntime-gpu)). См. в статье [требования к системе](https://github.com/Microsoft/onnxruntime#system-requirements) перед установкой. 

 Чтобы установить среду выполнения ONNX для Python, используйте один из следующих команд: 
```python   
pip install onnxruntime       # CPU build
pip install onnxruntime-gpu   # GPU build
``` 

Для вызова среды выполнения ONNX в сценарии Python используйте следующую команду.    
```python   
import onnxruntime  
session = onnxruntime.InferenceSession("path to model") 
``` 

Входные и выходные данные для использования модели см. в документации к соответствующей модели. Для просмотра модели можно также использовать такое средство визуализации, как [Netron](https://github.com/lutzroeder/Netron). Среда выполнения ONNX также позволяет запрашивать метаданные модели и ее входные и выходные данные:    
```python   
session.get_modelmeta() 
first_input_name = session.get_inputs()[0].name 
first_output_name = session.get_outputs()[0].name   
``` 

Для вывода модели добавьте параметр `run` и передайте список выходных данных, которые хотите получить (не заполняйте, если вам нужны все), а также карту входных значений. Результатом будет список выходных данных.  
```python   
results = session.run(["output1", "output2"], {"input1": indata1, "input2": indata2})   
results = session.run([], {"input1": indata1, "input2": indata2})   
``` 

Полный справочник по API Python см. в [документации по среде выполнения ONNX](https://aka.ms/onnxruntime-python).    

## <a name="examples"></a>Примеры

[how-to-use-azureml/deployment/onnx](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx): примеры записных книжек, создающих и развертывающих модели ONNX.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Дополнительная информация

Узнайте больше об ONNX или поддержите проект:
+ [Веб-сайт проекта ONNX](https://onnx.ai)
+ [Код ONNX в GitHub](https://github.com/onnx/onnx)

Узнайте больше о среде выполнения ONNX или поддержите проект:
+ [Репозиторий GitHub для среды выполнения ONNX](https://github.com/Microsoft/onnxruntime)


