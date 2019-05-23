---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 26789a12053fa6275b09836e706c391e181c8efd
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132141"
---
## <a name="create-a-function"></a>Creación de una función

El siguiente comando crea una función desencadenada por HTTP denominada `MyHttpTrigger`.

```bash
func new --name MyHttpTrigger --template "HttpTrigger"
```

Cuando se ejecute el comando, verá una salida similar a la siguiente:

```output
The function "MyHttpTrigger" was created successfully from the "HttpTrigger" template.
```
