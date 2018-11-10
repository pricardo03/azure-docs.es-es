---
title: Procesamiento de datos y funciones definidas por el usuario con Azure Digital Twins | Microsoft Docs
description: Información general del procesamiento de datos, buscadores de coincidencias y funciones definidas por el usuario con Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: a45f82b142ee4f4c9c88ea755607b88323feaae5
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210132"
---
# <a name="data-processing-and-user-defined-functions"></a>Procesamiento de datos y funciones definidas por el usuario

Azure Digital Twins ofrece funcionalidades avanzadas de proceso. Los desarrolladores pueden definir y ejecutar funciones personalizadas en los mensajes de telemetría entrantes para enviar eventos a los puntos de conexión predefinidos.

## <a name="data-processing-flow"></a>Flujo de procesamiento de datos

Una vez que los dispositivos envían datos de telemetría a Digital Twins, los desarrolladores pueden procesar los datos en cuatro fases: _validación_, _coincidencia_, _proceso_ y _envío_:

![Flujo de procesamiento de Digital Twins][1]

1. La fase _validación_ transforma el mensaje de telemetría entrante en un formato de [**objeto de transferencia de datos**](https://en.wikipedia.org/wiki/Data_transfer_object) reconocido. Esta fase también ejecuta la validación de dispositivos y sensores.
1. La fase _coincidencia_ busca las funciones definidas por el usuario adecuadas para ejecutarse. Los buscadores de coincidencias predefinidos buscarán las funciones definidas por el usuario según la información del dispositivo, del sensor y del espacio a partir del mensaje entrante de telemetría.
1. La fase _proceso_ ejecuta a las funciones definidas por el usuario coincidentes en la fase anterior. Estas funciones pueden leer y actualizar los valores calculados en los nodos de un grafo espacial y pueden emitir notificaciones personalizadas.
1. La fase _envío_ enruta las notificaciones personalizadas de la fase de proceso a puntos de conexión definidos en el grafo.

## <a name="data-processing-objects"></a>Objetos de procesamiento de datos

El procesamiento de datos en Azure Digital Twins consiste en definir tres objetos: _buscadores de coincidencias_, _funciones definidas por el usuario_ y _asignaciones de roles_:

![Flujo de procesamiento de Digital Twins][2]

### <a name="matchers"></a>Buscadores de coincidencias

Los _buscadores de coincidencias_ definen un conjunto de condiciones que evalúan las acciones que llevarán a cabo según la telemetría entrante del sensor. Estas condiciones para establecer la coincidencia podrían incluir propiedades del sensor, el dispositivo primario del sensor y el espacio primario del sensor. Las condiciones se expresan como comparaciones frente a una [ruta de acceso JSON](http://jsonpath.com/), tal como se describe en el ejemplo siguiente:

- Todos los sensores de tipo de datos **Temperatura**.
- Que tienen `01` en su puerto.
- Que pertenecen a los dispositivos con la clave de propiedad extendida **Fabricante** establecida en el valor `"GoodCorp"`.
- Que pertenecen a los espacios del tipo `"Venue"`.
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
> - Las ruta de acceso JSON distinguen entre mayúsculas y minúsculas.
> - La carga de JSON es la misma que la carga que devolvería:
>   - `/sensors/{id}?includes=properties,types` para el sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` para el dispositivo primario del sensor.
>   - `/spaces/{id}?includes=properties,types,location,timezone` para el espacio primario del sensor.
> - Las comparaciones distinguen entre mayúsculas y minúsculas.

### <a name="user-defined-functions"></a>Funciones definidas por el usuario

Una _función definida por el usuario_, o _UDF_, es una función personalizada que se ejecuta en un entorno aislado en Azure Digital Twins. Las UDF tienen acceso tanto al mensaje de telemetría sin procesar del sensor tal como se ha recibido, como al grafo espacial y al servicio de distribuidor. Una vez que se registra la UDF dentro del grafo, debe crearse un buscador de coincidencias (detallado anteriormente) para especificar cuándo se debe ejecutar la UDF. Cuando Digital Twins recibe nuevos datos de telemetría desde un sensor determinado, la UDF coincidente puede calcular una media móvil de las últimas lecturas del sensor, por ejemplo.

Las UDF se pueden escribir en JavaScript y permiten a los desarrolladores ejecutar fragmentos de código personalizados con mensajes de telemetría del sensor. También son métodos auxiliares para interactuar con el grafo en el entorno de ejecución definido por el usuario. Con una UDF, los desarrolladores pueden:

- Establecer la lectura del sensor directamente en el objeto de sensor dentro del grafo.
- Realizar una acción basada en las lecturas de distintos sensores dentro de un espacio en el grafo.
- Crear una notificación cuando se cumplen ciertas condiciones para un lectura entrante del sensor.
- Asociar metadatos del grafo a la lectura del sensor antes de enviar una notificación.

Consulte [Creación de funciones definidas por el usuario](how-to-user-defined-functions.md) para obtener más detalles.

### <a name="role-assignment"></a>Asignación de roles

Las acciones de una UDF están sujetos al control de acceso basado en roles de Digital Twins para proteger los datos dentro del servicio. Las asignaciones de roles aseguran que una UDF dada tenga los permisos adecuados para interactuar con el grafo espacial. Por ejemplo, una UDF puede intentar crear, leer, actualizar o eliminar datos del grafo en un espacio determinado. El nivel de acceso de una UDF se comprueba cuando la UDF solicita datos al grafo o intenta una acción. Para más información, consulte [Control de acceso basado en roles](security-create-manage-role-assignments.md).

Es posible que un buscador de coincidencias desencadene una UDF que no tiene ninguna asignación de roles. En este caso, la UDF produciría un error al leer datos del grafo.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de cómo enrutar eventos y mensajes de telemetría a otros servicios de Azure, lea [Enrutamiento de eventos y mensajes](concepts-events-routing.md).

Para más información sobre cómo crear buscadores de coincidencias, funciones definidas por el usuario y asignaciones de roles, lea la [guía para la creación de funciones definidas por el usuario](how-to-user-defined-functions.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
