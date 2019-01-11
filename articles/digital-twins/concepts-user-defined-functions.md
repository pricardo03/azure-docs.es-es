---
title: Procesamiento de datos y funciones definidas por el usuario con Azure Digital Twins | Microsoft Docs
description: Introducción al procesamiento de datos, los buscadores de coincidencias y las funciones definidas por el usuario con Azure Digital Twins.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: alinast
ms.openlocfilehash: 915c57033209ff982946163c408cf8557515e2f5
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999210"
---
# <a name="data-processing-and-user-defined-functions"></a>Procesamiento de datos y funciones definidas por el usuario

Azure Digital Twins ofrece funcionalidades avanzadas de proceso. Los desarrolladores pueden definir y ejecutar funciones personalizadas en los mensajes de telemetría entrantes para enviar eventos a puntos de conexión predefinidos.

## <a name="data-processing-flow"></a>Flujo de procesamiento de datos

Una vez que los dispositivos envían datos de telemetría a Azure Digital Twins, los desarrolladores pueden procesar los datos en cuatro fases: *validación*, *coincidencia*, *proceso* y *distribución*.

![Flujo de procesamiento de datos de Azure Digital Twins][1]

1. La fase de validación transforma el mensaje de telemetría entrante en un formato de [objeto de transferencia de datos](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/part-5) reconocido. Esta fase también ejecuta la validación de dispositivos y sensores.
1. La fase coincidencia busca las funciones definidas por el usuario adecuadas para ejecutarse. Los buscadores de coincidencias predefinidos buscan las funciones definidas por el usuario según la información del dispositivo, del sensor y del espacio a partir del mensaje entrante de telemetría.
1. La fase proceso ejecuta a las funciones definidas por el usuario coincidentes en la fase anterior. Estas funciones pueden leer y actualizar los valores calculados en los nodos de un grafo espacial y pueden emitir notificaciones personalizadas.
1. La fase de envío enruta las notificaciones personalizadas desde la fase de proceso a los puntos de conexión definidos en el grafo.

## <a name="data-processing-objects"></a>Objetos de procesamiento de datos

El procesamiento de datos en Azure Digital Twins consiste en definir tres objetos: *buscadores de coincidencias*, *funciones definidas por el usuario* y *asignaciones de roles*.

![Objetos de procesamiento de datos de Azure Digital Twins][2]

<div id="matcher"></div>

### <a name="matchers"></a>Buscadores de coincidencias

Los buscadores de coincidencias definen un conjunto de condiciones que evalúan las acciones que se llevarán a cabo según la telemetría entrante del sensor. Las condiciones para determinar la coincidencia podrían incluir propiedades del sensor, el dispositivo primario del sensor y el espacio primario del sensor. Las condiciones se expresan como comparaciones frente a una [ruta de acceso JSON](http://jsonpath.com/), tal como se describe en este ejemplo:

- Todos los sensores de tipo de datos **Temperatura** representados por el valor de cadena de escape `\"Temperature\"`.
- Que tienen `01` en su puerto.
- Que pertenecen a dispositivos con la clave de propiedad extendida **Manufacturer** establecida en el valor de cadena de escape `\"GoodCorp\"`.
- Que pertenecen a los espacios del tipo especificado por la cadena de escape `\"Venue\"`.
- Que son descendientes del elemento primario **SpaceId** `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`.

```JSON
{
  "SpaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Name": "My custom matcher",
  "Description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value GoodCorp and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"GoodCorp\"",
      "comparison": "Equals"
    },
    {
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - Las rutas de acceso JSON distinguen entre mayúsculas y minúsculas.
> - La carga de JSON es la misma que la carga que devolvería:
>   - `/sensors/{id}?includes=properties,types` para el sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` para el dispositivo primario del sensor.
>   - `/spaces/{id}?includes=properties,types,location,timezone` para el espacio primario del sensor.
> - Las comparaciones distinguen entre mayúsculas y minúsculas.

### <a name="user-defined-functions"></a>Funciones definidas por el usuario

Una función definida por el usuario es una función personalizada que se ejecuta en un entorno aislado en Azure Digital Twins. Las funciones definidas por el usuario tienen acceso al mensaje de telemetría sin formato del sensor, que se ha recibido. También tienen acceso al grafo espacial y al servicio de distribuidor. Una vez que se registra la función definida por el usuario dentro del grafo, debe crearse un buscador de coincidencias (detallado [anteriormente](#matcher)) para especificar cuándo se debe ejecutar dicha función. Por ejemplo, cuando Azure Digital Twins recibe nuevos datos de telemetría de un sensor determinado, la función definida por el usuario coincidente puede calcular una media móvil de las últimas lecturas del sensor.

Las funciones definidas por el usuario se pueden escribir en JavaScript. Los métodos auxiliares interactúan con el grafo en el entorno de ejecución definido por el usuario. Los desarrolladores pueden ejecutar fragmentos de código personalizados en relación con los mensajes de telemetría de los sensores. Algunos ejemplos son:

- Establecer la lectura del sensor directamente en el objeto de sensor dentro del grafo.
- Realizar una acción basada en las lecturas de distintos sensores dentro de un espacio en el grafo.
- Crear una notificación cuando se cumplen ciertas condiciones para un lectura entrante del sensor.
- Asociar metadatos del grafo a la lectura del sensor antes de enviar una notificación.

Para más información, consulte [Uso de funciones definidas por el usuario](./how-to-user-defined-functions.md).

### <a name="role-assignment"></a>Asignación de roles

Las acciones de una función definida por el usuario están sujetas al [control de acceso basado en rol](./security-role-based-access-control.md) de Azure Digital Twins para proteger los datos dentro del servicio. Las asignaciones de roles definen las funciones definidas por el usuario que tienen los permisos adecuados para interactuar con el grafo espacial y sus entidades. Por ejemplo, una función definida por el usuario podría tener la capacidad y el permiso para *CREAR*, *LEER*, *ACTUALIZAR* o *ELIMINAR* datos del grafo en un espacio determinado. El nivel de acceso de una función definida por el usuario se comprueba cuando esta solicita datos al grafo o intenta una acción. Para más información, consulte [Control de acceso basado en rol](./security-create-manage-role-assignments.md).

Es posible que un buscador de coincidencias desencadene una función definida por el usuario que no tiene ninguna asignación de roles. En este caso, la función definida por el usuario no podría leer datos del grafo.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo enrutar eventos y mensajes de telemetría a otros servicios de Azure, lea [Enrutamiento de eventos y mensajes](./concepts-events-routing.md).

- Para más información sobre cómo crear buscadores de coincidencias, funciones definidas por el usuario y asignaciones de roles, lea la [guía para la creación de funciones definidas por el usuario](./how-to-user-defined-functions.md).

- Revise la [documentación de referencia de biblioteca de cliente de funciones definidas por el usuario](./reference-user-defined-functions-client-library.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
