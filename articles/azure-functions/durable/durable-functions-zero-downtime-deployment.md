---
title: Implementación sin tiempo de inactividad con Durable Functions
description: Aprenda a habilitar la orquestación de Durable Functions para realizar implementaciones sin tiempo de inactividad.
services: functions
author: tsushi
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: azfuncdf
ms.openlocfilehash: 5e6e51d2a058f89a04a81800b81f3c316be4eab7
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2019
ms.locfileid: "72302069"
---
# <a name="zero-downtime-deployment-for-durable-functions"></a>Implementación sin tiempo de inactividad con Durable Functions
El [modelo de ejecución confiable](durable-functions-checkpointing-and-replay.md) de Durable Functions requiere que las orquestaciones sean deterministas, lo que constituye un desafío adicional que se debe tener en cuenta al implementar actualizaciones. Cuando una implementación contiene cambios en las signaturas de las funciones de actividad o la lógica del orquestador, las instancias de orquestación en curso producen errores. Esta situación supone especialmente un problema para las instancias de orquestaciones de larga ejecución, que pueden llevar horas o días de trabajo.

Para evitar que se produzcan estos errores, debe retrasar la implementación hasta que se hayan completado todas las instancias de orquestación en ejecución, o asegurarse de que las instancias de orquestación en curso usan las versiones existentes de las funciones. Para más información acerca del control de versiones, consulte [Control de versiones en Durable Functions](durable-functions-versioning.md).

En el gráfico siguiente se comparan las tres estrategias principales para lograr una implementación sin tiempo de inactividad con Durable Functions: 

| Estrategia |  Cuándo se deben usar | Ventajas | Desventajas |
| -------- | ------------ | ---- | ---- |
| **[Control de versiones](#versioning)** |  En aplicaciones que no experimentan [cambios importantes](durable-functions-versioning.md) frecuentes. | Fácil de implementar. |  Mayor tamaño de la aplicación de funciones en la memoria y mayor número de funciones.<br/>Duplicación de código. |
| **[Comprobación de estado con espacio](#status-check-with-slot)** | Un sistema que no tenga orquestaciones de larga ejecución que duren más de 24 horas u orquestaciones que se superpongan con frecuencia. | Base de código sencilla.<br/>No requiere una administración adicional de la aplicación de funciones. | Requiere administración adicional de la cuenta de almacenamiento o de la central de tareas.<br/>Requiere períodos de tiempo en los que no se ejecuten orquestaciones. |
| **[Enrutamiento de aplicaciones](#application-routing)** | Un sistema que no tenga períodos de tiempo en los que no se ejecuten orquestaciones como, por ejemplo, aquellos con orquestaciones que duran más de 24 horas u orquestaciones que se superponen con frecuencia. | Controla las nuevas versiones de los sistemas con orquestaciones que se ejecutan continuamente y que tienen cambios importantes. | Requiere un enrutador de aplicación inteligente.<br/>Podría maximizar el número de aplicaciones de funciones que permite la suscripción (el valor predeterminado es 100). |

## <a name="versioning"></a>Control de versiones
Defina nuevas versiones de las funciones y deje las versiones anteriores en la aplicación de funciones. Como puede ver en el diagrama, la versión de una función forma parte de su nombre. Dado que las versiones anteriores de las funciones se conservan, las instancias de orquestación en curso pueden seguir haciendo referencia a ellas. En cambio, las solicitudes de nuevas instancias de orquestación llaman a la versión más reciente, a la que la función de cliente de orquestación puede hacer referencia desde una configuración de aplicación.

![Estrategia de control de versiones](media/durable-functions-zero-downtime-deployment/versioning-strategy.png)

En esta estrategia, se deben copiar todas las funciones y sus referencias a otras funciones actualizadas. Puede simplificar esto mediante la escritura de un script. Este es un [ejemplo de proyecto](https://github.com/TsuyoshiUshio/DurableVersioning) con un script de migración.

>[!NOTE]
>Esta estrategia usa ranuras de implementación para evitar tiempos de inactividad durante la implementación. Para obtener información más detallada sobre cómo crear y usar nuevas ranuras de implementación, consulte [Ranuras de implementación de Azure Functions](../functions-deployment-slots.md).

## <a name="status-check-with-slot"></a>Comprobación de estado con espacio

Mientras se ejecuta la versión actual de la aplicación de funciones en el espacio de producción, implemente la nueva versión de esa aplicación en el espacio de ensayo. Antes de cambiar entre los espacios de producción y de ensayo, compruebe si hay instancias de orquestación en ejecución. Una vez completadas todas las instancias de orquestación, puede realizar el intercambio. Esta estrategia funciona cuando hay períodos predecibles en los que no hay instancias de orquestación en curso. Este es el mejor enfoque cuando las orquestaciones no son de ejecución prolongada y cuando las ejecuciones de la orquestación no se superponen con frecuencia.

### <a name="function-app-configuration"></a>Configuración de la aplicación de funciones

Puede usar el siguiente procedimiento para configurar este escenario:

1. [Agregue ranuras de implementación](../functions-deployment-slots.md#add-a-slot) a la aplicación de funciones en los espacios de ensayo y de producción.

1. En cada espacio, establezca la [configuración de la aplicación AzureWebJobsStorage](../functions-app-settings.md#azurewebjobsstorage) en la cadena de conexión de una cuenta de almacenamiento compartida. El entorno en tiempo de ejecución de Azure Functions usará esta cadena. El entorno en tiempo de ejecución de Azure Functions usará esta cuenta y administrará las claves de la función.

1. Para cada espacio, cree un nuevo valor para la aplicación (por ejemplo, DurableManagementStorage) y establezca su valor en la cadena de conexión de distintas cuentas de almacenamiento. La extensión de Durable Functions usará estas cuentas de almacenamiento para una [ejecución confiable](durable-functions-checkpointing-and-replay.md). Use una cuenta de almacenamiento independiente para cada espacio. No marque este valor como un valor de ranura de implementación.

1. En la [sección durableTask del archivo host.json](durable-functions-bindings.md#hostjson-settings) de la aplicación de funciones, especifique azureStorageConnectionStringName como el nombre de la configuración de la aplicación que creó en el paso 3.

En el diagrama siguiente se muestra la configuración descrita de las ranuras de implementación y las cuentas de almacenamiento. En este posible escenario anterior a la implementación, la versión 2 de una aplicación de funciones se ejecuta en el espacio de producción, mientras que la versión 1 permanece en el espacio de ensayo.

![Ranura de implementación](media/durable-functions-zero-downtime-deployment/deployment-slot.png)

### <a name="hostjson-examples"></a>Ejemplos de host.json

Los siguientes fragmentos JSON son ejemplos de la configuración de la cadena de conexión en el archivo host.json.

#### <a name="functions-2x"></a>Functions 2.x

```json
{
  "version": 2.0,
  "extensions": {
    "durableTask": {
      "azureStorageConnectionStringName": "DurableManagementStorage"
    }
  }
}
```

#### <a name="functions-1x"></a>Functions 1.x

```json
{
  "durableTask": {
    "azureStorageConnectionStringName": "DurableManagementStorage"
  }
}
```

### <a name="cicd-pipeline-configuration"></a>Configuración de la canalización de CI/CD

Configure la canalización de CI/CD para que se implemente solo cuando la aplicación de funciones no tenga instancias de orquestación pendientes o en ejecución. Cuando use Azure Pipelines, puede crear una función que compruebe estas condiciones, como en el ejemplo siguiente:

```csharp
[FunctionName("StatusCheck")]
public static async Task<IActionResult> StatusCheck(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    var runtimeStatus = new List<OrchestrationRuntimeStatus>();

    runtimeStatus.Add(OrchestrationRuntimeStatus.Pending);
    runtimeStatus.Add(OrchestrationRuntimeStatus.Running);

    var status = await client.GetStatusAsync(new DateTime(2015,10,10), null, runtimeStatus);
    return (ActionResult) new OkObjectResult(new Status() {HasRunning = (status.Count != 0)});
}
```

A continuación, configure la puerta de ensayo para que espere hasta que no haya orquestaciones en ejecución. Para más información, consulte [Release deployment control using gates](/azure/devops/pipelines/release/approvals/gates?view=azure-devops) (Liberar control de implementaciones mediante puertas).

![Puerta de implementación](media/durable-functions-zero-downtime-deployment/deployment-gate.png)

Azure Pipelines comprueba la ejecución de instancias de orquestación de la aplicación de funciones antes de que se inicie la implementación.

![Puerta de implementación (en ejecución)](media/durable-functions-zero-downtime-deployment/deployment-gate-2.png)

Ahora la nueva versión de la aplicación de funciones se debería implementar en el espacio de ensayo.

![Ranura de implementación](media/durable-functions-zero-downtime-deployment/deployment-slot-2.png)

Por último, intercambie los espacios. 

Los valores de configuración de la aplicación que no están marcados como valores de configuración de la ranura de implementación también se intercambian, por lo que la aplicación de la versión 2 conserva su referencia a la cuenta de almacenamiento A. Dado que se realiza un seguimiento del estado de la orquestación en la cuenta de almacenamiento, las orquestaciones que se ejecutan en la aplicación de la versión 2 continúan ejecutándose en la nueva ranura sin interrupción.

![Ranura de implementación](media/durable-functions-zero-downtime-deployment/deployment-slot-3.png)

Para usar la misma cuenta de almacenamiento en ambos espacios, puede cambiar los nombres de las centrales de tareas. En este caso, debe administrar el estado de los espacios y la configuración de HubName de las aplicaciones. Para más información, consulte [Centrales de tareas en Durable Functions](durable-functions-task-hubs.md).

## <a name="application-routing"></a>Enrutamiento de aplicaciones

Esta estrategia es la más compleja. Sin embargo, se puede usar para las aplicaciones de funciones que no tengan tiempo entre las orquestaciones en ejecución.

Para esta estrategia, debe crear un *enrutador de aplicación* delante de Durable Functions. Este enrutador se puede implementar con Durable Functions y tiene las siguientes responsabilidades:

* Implementar la aplicación de funciones.
* Administrar la versión de Durable Functions. 
* Enrutar las solicitudes de orquestación a las aplicaciones de funciones.

La primera vez que se recibe una solicitud de orquestación, el enrutador realiza las tareas siguientes:

1. Crea una aplicación de funciones en Azure.
2. Implementa el código de la aplicación de funciones en la nueva aplicación de funciones en Azure.
3. Reenvía la solicitud de orquestación a la nueva aplicación.

El enrutador administra la instrucción sobre qué versión del código de la aplicación se implementa en qué aplicación de funciones en Azure.

![Enrutamiento de aplicaciones (por primera vez)](media/durable-functions-zero-downtime-deployment/application-routing.png)

El enrutador dirige las solicitudes de implementación y orquestación a la aplicación de funciones adecuada según el valor de `version` que se envíe con la solicitud, ignorando la versión de revisión.

Si implementa una nueva versión de la aplicación *sin* ningún cambio importante, puede aumentar la versión de revisión. El enrutador se implementa en la aplicación de funciones existente y las solicitudes de la versión anterior y de la nueva del código se enrutan hacia la misma aplicación de funciones.

![Enrutamiento de aplicaciones (sin cambios importantes)](media/durable-functions-zero-downtime-deployment/application-routing-2.png)

Si implementa una nueva versión de la aplicación con algún cambio importante, puede aumentar la versión principal o la secundaria. Posteriormente, el enrutador de la aplicación crea una nueva aplicación de funciones en Azure, se implementa en ella y enruta las solicitudes de la nueva versión de la aplicación en ella. En el siguiente diagrama, las orquestaciones en ejecución en la versión 1.0.1 de la aplicación continúan ejecutándose, pero las solicitudes de la versión 1.1.0 se enrutan a la nueva aplicación de funciones.

![Enrutamiento de aplicaciones (con cambios importantes)](media/durable-functions-zero-downtime-deployment/application-routing-3.png)

El enrutador supervisa el estado de las orquestaciones en la versión 1.0.1 y quita las aplicaciones una vez finalizadas todas las orquestaciones.  

### <a name="tracking-store-settings"></a>Seguimiento de la configuración de almacén

Cada aplicación de funciones debe usar colas de programación independientes, posiblemente en cuentas de almacenamiento independientes. Sin embargo, si desea consultar todas las instancias de orquestaciones en todas las versiones de la aplicación, puede compartir las tablas de instancia y de historial entre las aplicaciones de funciones. Puede compartir tablas configurando `trackingStoreConnectionStringName` y `trackingStoreNamePrefix` en el archivo de [configuración host.json](durable-functions-bindings.md#host-json) para que todas usen los mismos valores.

Para más información, consulte [Administración de instancias con Durable Functions en Azure](durable-functions-instance-management.md).

![Seguimiento de la configuración de almacén](media/durable-functions-zero-downtime-deployment/tracking-store-settings.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Control de versiones en Durable Functions](durable-functions-versioning.md)

