---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 08/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3cbe634d862682a5f6b06c2cfc77a4d3b03954f9
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42809463"
---
El código de todas las funciones de una aplicación de función está ubicado en una carpeta raíz (`wwwroot`) que contiene un archivo de configuración de host y una o varias subcarpetas. Cada subcarpeta contiene el código de una función diferente como se puede ver en el ejemplo siguiente:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
 | - bin
 | | - mycompiledcsharp.dll
```

El archivo host.json contiene alguna configuración específica del tiempo de ejecución y se coloca en la carpeta principal de la aplicación de función. Para más información sobre la configuración disponible, consulte la [referencia de host.json](../articles/azure-functions/functions-host-json.md).

Cada función tiene una carpeta que contiene uno o varios archivos de código, la configuración de function.json y otras dependencias. Para un proyecto de la biblioteca de clases de C#, el archivo (.dll) de la biblioteca de clases compilado se implementa en la subcarpeta `bin`.

