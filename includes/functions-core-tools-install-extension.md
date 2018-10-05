---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 09/21/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f1b53c53b1e5fb089eb9b8a9b816b11a1eea126d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47044516"
---
Al desarrollar funciones a nivel local, puede instalar las extensiones necesarias con el uso de Azure Functions Core Tools desde el terminal o desde un símbolo del sistema.

Después de haber actualizado el archivo *function.json* para que incluya todos los enlaces que necesita la función, ejecute el siguiente comando en la carpeta del proyecto.

```bash
func extensions install
```

El comando lee el archivo *function.json* para ver qué paquetes necesita, los instala y, luego, recompila el proyecto de extensiones. Agrega los nuevos enlaces en la versión actual, pero no actualiza los enlaces existentes. Use la opción `--force` para actualizar los enlaces existentes a la versión más reciente cuando instale otros nuevos.

Si desea instalar una versión concreta de un paquete o va a instalar paquetes antes de editar el archivo *function.json*, use el comando `func extensions install` con el nombre del paquete, tal como se muestra en el ejemplo siguiente:

```bash
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Reemplace `<target_version>` con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org).
