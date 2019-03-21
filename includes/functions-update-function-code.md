---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 03/12/2019
ms.author: glenga
ms.custom: include file, fasttrack-edit
ms.openlocfilehash: 3b0be31afbce78c0de8dc919245e92bee7af04b0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57964275"
---
## <a name="update-the-function"></a>Actualización de la función

De manera predeterminada, la plantilla crea una función que requiere una clave de función al hacer solicitudes. Para facilitar la prueba de la función en Azure, debe actualizar la función para permitir el acceso anónimo. La forma de hacer este cambio depende del lenguaje del proyecto de las funciones.

### <a name="c"></a>C\#

Abra el archivo de código MyHttpTrigger.cs que es su nueva función, actualice el atributo **AuthorizationLevel** de la definición de función a un valor de `Anonymous` y guarde los cambios.

```csharp
[FunctionName("MyHttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req,
    ILogger log)
```

### <a name="javascript"></a>JavaScript

Abra el archivo function.json de la nueva función, ábralo en un editor de texto, actualice la propiedad **authLevel** en **bindings.httpTrigger** a `anonymous` y guarde los cambios.

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

Ahora puede llamar a la función en Azure sin tener que proporcionar la clave de la función. Nunca se requiere la clave de la función cuando se ejecuta de manera local.
