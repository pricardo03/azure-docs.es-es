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
ms.openlocfilehash: aad66a91f7de8380ac7e87f0ce8e35ed43cac4a6
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594524"
---
El código de todas las funciones de una aplicación de función está ubicado en una carpeta de proyecto raíz que contiene un archivo de configuración de host y una o varias subcarpetas. Cada subcarpeta contiene el código de una función diferente. La estructura de carpetas se muestra en la siguiente representación:

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

En la versión 2.x del runtime de Functions, todas las funciones de la aplicación de función deben compartir la misma pila de idioma.  

El archivo [host.json](../articles/azure-functions/functions-host-json.md) contiene configuraciones específicas del entorno de ejecución y está en la carpeta raíz de la aplicación de funciones. Un carpeta *bin* contiene paquetes y otros archivos de biblioteca que requiere la aplicación de funciones. Consulte los requisitos específicos de idioma para un proyecto de aplicación de función:

* [Biblioteca de clases C# (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [Script de C# (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [Script de F#](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)



