---
title: включение файла
description: включение файла
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
origin.date: 09/12/2018
ms.date: 10/19/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 27dc1b1315a8e33b8ac13b34d4a86ad0343388b4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60731249"
---
Код всех функций конкретного приложения-функции хранится в корневой папке проекта, содержащей файл конфигурации главного узла и одну или несколько вложенных папок. Каждая вложенная папка содержит код для отдельной функции, как показано в следующем примере:

```
FunctionApp
 | - host.json
 | - Myfirstfunction
 | | - function.json
 | | - ...  
 | - mysecondfunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

В версии 2.x среды выполнения Функций Azure все функции в приложении-функции следует создавать на одном языке.  

Файл [host.json](../articles/azure-functions/functions-host-json.md), который содержит конфигурацию среды выполнения, находится в корневой папке приложения-функции. Папка `bin` содержит пакеты и другие файлы библиотеки, необходимые для приложения-функции. См. требования конкретного языка для проекта приложения-функции:

- [Библиотека классов C# (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
- [Скрипт C# (CSX)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
- [Скрипт F#](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
- [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
- [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)


<!-- ms.date: 10/19/2018 -->

