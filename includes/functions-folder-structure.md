---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 27dc1b1315a8e33b8ac13b34d4a86ad0343388b4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131506"
---
El código de todas las funciones de una aplicación de función está ubicado en una carpeta de proyecto raíz que contiene un archivo de configuración de host y una o varias subcarpetas. Cada subcarpeta contiene el código de una función diferente como se puede ver en la representación siguiente:

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

En la versión 2.x del sistema en ejecución de Functions, todas las funciones de la aplicación de función deben compartirse en el mismo proceso de trabajo de idioma.  

El archivo [host.json](../articles/azure-functions/functions-host-json.md), que contiene alguna configuración específica del tiempo de ejecución, se coloca en la carpeta principal de la aplicación de función. Un carpeta `bin` contiene paquetes y otros archivos de biblioteca requeridos por la aplicación de función. Consulte los requisitos específicos de idioma para un proyecto de aplicación de función:

* [Biblioteca de clases C# (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [Script de C# (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [Script de F#](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)



