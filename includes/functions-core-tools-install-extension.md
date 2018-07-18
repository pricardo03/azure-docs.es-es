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
ms.openlocfilehash: d166a77a0636efea3b63660fde2187e3f2ec15c0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38944726"
---
Al desarrollar funciones a nivel local, puede instalar las extensiones necesarias con el uso de Azure Functions Core Tools desde el terminal o desde un símbolo del sistema. 

Después de haber actualizado el archivo *function.json* para que incluya todos los enlaces que necesita la función, ejecute el comando `func extensions install` en la carpeta del proyecto. El comando lee el archivo *function.json* para ver qué paquetes necesita y, a continuación, los instala.

Si desea instalar una versión concreta de un paquete o va a instalar paquetes antes de editar el archivo *function.json*, use el comando `func extensions install` con el nombre del paquete, tal como se muestra en el ejemplo siguiente:

```
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Reemplace `<target_version>` con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org).
