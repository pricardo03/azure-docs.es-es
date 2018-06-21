---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 04/06/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 6fb497a5b6da00dece43c7f41ea3c411f385a2ba
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726894"
---
Al desarrollar funciones a nivel local, puede instalar las extensiones necesarias con el uso de Azure Functions Core Tools desde el terminal o desde un símbolo del sistema. 

Después de haber actualizado el archivo *function.json* para que incluya todos los enlaces que necesita la función, ejecute el comando `func extensions install` en la carpeta del proyecto. El comando lee el archivo *function.json* para ver qué paquetes necesita y, a continuación, los instala.

Si desea instalar una versión concreta de un paquete o va a instalar paquetes antes de editar el archivo *function.json*, use el comando `func extensions install` con el nombre del paquete, tal como se muestra en el ejemplo siguiente:

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

Reemplace `<target_version>` con una versión específica del paquete. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org).
