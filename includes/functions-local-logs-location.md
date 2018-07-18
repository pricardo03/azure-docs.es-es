---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/01/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 88c01e8e57d4a92478b8b1ca0689ff0f8e499b39
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38739634"
---
Cuando el host de funciones se ejecuta localmente, escribe registros en la ruta de acceso siguiente:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

En Windows, `<DefaultTempDirectory>` es el primer valor que se encuentra de las variables de entorno TMP, TEMP, USERPROFILE o del directorio de Windows.
En Mac OS o Linux, `<DefaultTempDirectory>` es la variable de entorno TMPDIR.

> [!NOTE]
> Cuando se inicia el host de funciones, este sobrescribirá la estructura de archivos existentes en el directorio.