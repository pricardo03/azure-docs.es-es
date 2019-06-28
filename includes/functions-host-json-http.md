---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 35b087d13099b975a1c9c6d2dbd449935f5f0d1d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66249002"
---
```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 200,
        "maxConcurrentRequests": 100,
        "dynamicThrottlesEnabled": true
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------| 
|routePrefix|api|Prefijo de ruta que se aplica a todas las rutas. Use una cadena vacía para quitar el prefijo predeterminado. |
|maxOutstandingRequests|200<sup>*</sup>|Número máximo de solicitudes pendientes que se mantienen en un momento dado. Este límite incluye las solicitudes que están en cola pero no han empezado a ejecutarse, así como todas las ejecuciones en curso. Se rechazan todas las solicitudes entrantes que superen este límite con una respuesta 429 "Too Busy" (demasiado ocupado). Esto permite que los llamadores empleen estrategias de reintento basadas en tiempo y también le ayuda a controlar las latencias de solicitud máximas. Únicamente se controlan los movimientos de la cola que se producen dentro de la ruta de ejecución del host del script. Otras colas, como la cola de solicitudes de ASP.NET, siguen en efecto y no se ven alteradas por esta opción de configuración. <sup>*</sup>El valor predeterminado para la versión 1.x es ilimitado (`-1`). El valor predeterminado para la versión 2.x en un plan de consumo es 200. El valor predeterminado para la versión 2.x en un plan dedicado es ilimitado (`-1`).|
|maxConcurrentRequests|100<sup>*</sup>|Número máximo de funciones HTTP que se ejecutarán en paralelo. Esto permite controlar la simultaneidad, que a su vez puede ayudar a administrar el uso de recursos. Por ejemplo, podría tener una función HTTP que utiliza una gran cantidad de recursos del sistema (memoria/cpu/sockets) y causa problemas cuando la simultaneidad es demasiado alta. O bien podría tener una función que realiza solicitudes de salida a un servicio de terceros y puede que haya que limitar la velocidad de dichas llamadas. En estos casos puede ayudar aplicar una limitación. <sup>*</sup>El valor predeterminado para la versión 1.x es ilimitado (`-1`). El valor predeterminado para la versión 2.x en un plan de consumo es 100. El valor predeterminado para la versión 2.x en un plan dedicado es ilimitado (`-1`).|
|dynamicThrottlesEnabled|true<sup>*</sup>|Cuando se habilita, esta configuración hace que la canalización de procesamiento de la solicitud compruebe periódicamente contadores de rendimiento del sistema como conexiones, subprocesos, procesos, memoria o cpu y, si cualquiera de esos contadores superan un umbral alto integrado (80 %), las solicitudes se rechazarán con una respuesta 429 "Ocupado" hasta que los contadores vuelvan a niveles normales. <sup>*</sup>El valor predeterminado para la versión 1.x es false. El valor predeterminado para la versión 2.x en un plan de consumo es true. El valor predeterminado para la versión 2.x en un plan dedicado es false.|
