---
title: Uso de funciones definidas por el usuario en Azure Digital Twins | Microsoft Docs
description: Instrucciones sobre cómo crear funciones definidas por el usuario, buscadores de coincidencias y asignaciones de roles con Azure Digital Twins.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: alinast
ms.openlocfilehash: 6a757dca48dc3ff41adfe6f8802fad40e7a4ca81
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636839"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Uso de funciones definidas por el usuario en Azure Digital Twins

[Las funciones definidas por el usuario](./concepts-user-defined-functions.md) (UDF) le permiten ejecutar una lógica personalizada en los mensajes de telemetría entrantes y los metadatos de grafos espaciales. A continuación, el usuario puede enviar eventos a puntos de conexión predefinidos. En esta guía se muestra un ejemplo de reacción ante eventos de temperatura para detectar y notificar sobre cualquier lectura que supere una temperatura determinada.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>Referencia de la biblioteca cliente

Las funciones que están disponibles como métodos auxiliares en el tiempo de ejecución de las funciones definidas por el usuario se enumeran en la sección [referencia de clientes](#Client-Reference).

## <a name="create-a-matcher"></a>Creación de un buscador de coincidencias

Los buscadores de coincidencias son objetos gráficos que determinan qué funciones definidas por el usuario se ejecutan para un mensaje de telemetría determinado.

- Comparaciones de condiciones válidas para el buscador de coincidencias:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Destinos de condiciones válidas para el buscador de coincidencias:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

El buscador de coincidencias del ejemplo siguiente evalúa como true a cualquier evento de telemetría de sensores con `"Temperature"` como su valor de tipo de datos. Puede crear varios buscadores de coincidencias en una función definida por el usuario:

```plaintext
POST YOUR_MANAGEMENT_API_URL/matchers
{
  "Name": "Temperature Matcher",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ],
  "SpaceId": "YOUR_SPACE_IDENTIFIER"
}
```

| Valor | Reemplazar por |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | La región de servidor en la que está hospedada la instancia |

## <a name="create-a-user-defined-function-udf"></a>Creación de una función definida por el usuario

Tras la creación de los buscadores de coincidencias, cargue el fragmento de código de la función con la siguiente llamada **POST**:

> [!IMPORTANT]
> - En los encabezados, configure el siguiente valor `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - El cuerpo está formado por varias partes:
>   - La primera parte contiene los metadatos necesarios para la función definida por el usuario.
>   - La segunda es la lógica de ejecución de JavaScript.
> - En la sección **USER_DEFINED_BOUNDARY**, reemplace los valores de **SpaceId** y **Machers**.

```plaintext
POST YOUR_MANAGEMENT_API_URL/userdefinedfunctions with Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"
```

| Valor del parámetro | Reemplazar por |
| --- | --- |
| *USER_DEFINED_BOUNDARY* | Nombre de un límite de contenido de varias partes |

### <a name="body"></a>Cuerpo

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "YOUR_SPACE_IDENTIFIER",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Valor | Reemplazar por |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | El identificador de espacio  |
| YOUR_MATCHER_IDENTIFIER | El identificador del buscador de coincidencias que quiere usar |

### <a name="example-functions"></a>Funciones de ejemplo

Se establece la lectura de telemetría del sensor directamente con el tipo de datos **Temperature**, que es `sensor.DataType`:

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

El parámetro **telemetry** expone los atributos **SensorId** y **Message**, que corresponden a un mensaje enviado por un sensor. El parámetro **executionContext** expone los siguientes atributos:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

En el ejemplo siguiente, registramos un mensaje si la lectura de telemetría del sensor supera un umbral definido previamente. Si la configuración de diagnóstico está habilitada en la instancia de Azure Digital Twins, también se reenvían los registros de las funciones definidas por el usuario:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

El código siguiente desencadena una notificación si el nivel de temperatura supera la constante predefinida:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Para obtener un ejemplo de código más complejo para una función definida por el usuario, [compruebe los espacios disponibles con una función definida por el usuario para aire fresco](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Crear una asignación de rol

Es necesario crear una asignación de roles para que se ejecute la función definida por el usuario. Si no se crea, la función no tendrá los permisos adecuados para interactuar con la API de administración y realizar acciones en los objetos gráficos. Las acciones que realiza la función definida por el usuario no están exentas del control de acceso basado en roles dentro de las API de administración de Azure Digital Twins. Es posible limitar su ámbito mediante la especificación de determinados roles o rutas de control de acceso. Para más información, consulte la documentación de [Control de acceso basado en rol](./security-role-based-access-control.md).

1. Realice una consulta a los roles y obtenga el identificador del rol que quiere asignar a la función definida por el usuario. Páselo a **RoleId**:

    ```plaintext
    GET YOUR_MANAGEMENT_API_URL/system/roles
    ```

1. **ObjectId** será el identificador de la función definida por el usuario que se creó anteriormente.
1. Busque el valor de **Path** mediante la realización de una consulta de los espacios con `fullpath`.
1. Copie el valor de `spacePaths` devuelto, Lo usará en el código siguiente:

    ```plaintext
    GET YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Valor del parámetro | Reemplazar por |
    | --- | --- |
    | *YOUR_SPACE_NAME* | El nombre del espacio que quiere usar |

1. Pegue el valor de `spacePaths` devuelto en **Path** para crear una asignación de roles de UDF:

    ```plaintext
    POST YOUR_MANAGEMENT_API_URL/roleassignments
    {
      "RoleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "ObjectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "ObjectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "Path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | Su valor | Reemplazar por |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | El identificador para el rol deseado |
    | YOUR_USER_DEFINED_FUNCTION_ID | El identificador de la función definida por el usuario que quiere usar |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | Identificador que especifica el tipo de UDF |
    | YOUR_ACCESS_CONTROL_PATH | La ruta del control de acceso |

## <a name="send-telemetry-to-be-processed"></a>Envío de la telemetría a procesarse

Los datos de telemetría generados por el sensor que se describe en el grafo deben desencadenar la ejecución de la función definida por el usuario que se cargó. El procesador de datos recoge los datos de telemetría. A continuación, se crea un plan de ejecución para invocar la función definida por el usuario.

1. Recuperar los buscadores de coincidencias del sensor del que se generó la lectura.
1. En función de qué buscadores de coincidencia se evalúen correctamente, recuperar las funciones definidas por el usuario asociadas.
1. Ejecute cada función definida por el usuario.

## <a name="client-reference"></a>Referencia de cliente

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Dado un identificador de espacio, esta función recupera el espacio del grafo.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Identificador del espacio |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Dado un identificador de sensor, esta función recupera el sensor del grafo.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Identificador del sensor |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Dado un identificador de dispositivo, esta función recupera el dispositivo del grafo.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | Identificador de dispositivo |

### <a name="getsensorvaluesensorid-datatype--value"></a>getSensorValue(sensorId, dataType) ⇒ `value`

Dado un identificador de sensor y su tipo de datos, esta función recupera el valor actual de ese sensor.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | Identificador del sensor |
| *dataType*  | `string` | Tipo de datos del sensor |

### <a name="getspacevaluespaceid-valuename--value"></a>getSpaceValue(spaceId, valueName) ⇒ `value`

Dado un identificador de espacio y el nombre del valor, esta función recupera el valor actual de esa propiedad de espacio.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | Identificador del espacio |
| *valueName* | `string` | Nombre de la propiedad de espacio |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>getSensorHistoryValues(sensorId, dataType) ⇒ `value[]`

Dado un identificador de sensor y su tipo de datos, esta función recupera el historial de valores de ese sensor.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador del sensor |
| *dataType* | `string` | Tipo de datos del sensor |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>getSpaceHistoryValues(spaceId, dataType) ⇒ `value[]`

Dado un identificador de espacio y el nombre del valor, esta función recupera el historial de valores de esa propiedad de espacio.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |
| *valueName* | `string` | Nombre de la propiedad de espacio |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Dado un identificador de espacio, esta función recupera los espacios secundarios de ese espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Dado un identificador de espacio, esta función recupera los sensores secundarios de ese espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Dado un identificador de espacio, esta función recupera los dispositivos secundarios de ese espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Dado un identificador de espacio, esta función recupera los sensores secundarios de ese dispositivo primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Identificador de dispositivo |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Dado un identificador de espacio, esta función recupera su espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | Identificador del espacio |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Dado un identificador de sensor, esta función recupera su espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Identificador del sensor |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Dado un identificador de dispositivo, esta función recupera su espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Identificador de dispositivo |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Dado un identificador de sensor, esta función recupera su dispositivo primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Identificador del sensor |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>getSpaceExtendedProperty(spaceId, propertyName) ⇒ `extendedProperty`

Dado un identificador de espacio, esta función recupera la propiedad y su valor desde el espacio.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |
| *propertyName* | `string` | Nombre de la propiedad de espacio |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>getSensorExtendedProperty(sensorId, propertyName) ⇒ `extendedProperty`

Dado un identificador de sensor, esta función recupera la propiedad y su valor desde el sensor.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador del sensor |
| *propertyName* | `string` | Nombre de la propiedad del sensor |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>getDeviceExtendedProperty(deviceId, propertyName) ⇒ `extendedProperty`

Dado un identificador de dispositivo, esta función recupera la propiedad y su valor desde el espacio.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Identificador de dispositivo |
| *propertyName* | `string` | Nombre de la propiedad del dispositivo |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue(sensorId, dataType, value)

Esta función establece un valor en el objeto de sensor con el tipo de datos indicado.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador del sensor |
| *dataType*  | `string` | Tipo de datos del sensor |
| *value*  | `string` | Valor |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue(spaceId, dataType, value)

Esta función establece un valor en el objeto de espacio con el tipo de datos indicado.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |
| *dataType* | `string` | Tipo de datos |
| *value* | `string` | Valor |

### <a name="logmessage"></a>log(Message)

Esta función registra el mensaje siguiente dentro de la función definida por el usuario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *message* | `string` | Mensaje que se va a registrar |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification(topologyObjectId, topologyObjectType, payload)

Esta función envía una notificación personalizada para que se distribuya.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | Identificador de objeto gráfico. Algunos ejemplos son el espacio, el sensor y el identificador de dispositivo.|
| *topologyObjectType*  | `string` | Algunos ejemplos son el sensor y el dispositivo.|
| *payload*  | `string` | La carga útil de JSON que se envía con la notificación. |

## <a name="return-types"></a>Tipos de valor devuelto

Los modelos siguientes describen a los objetos devueltos por la referencia de cliente anterior.

### <a name="space"></a>Espacio

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000000",
  "Name": "Space",
  "FriendlyName": "Conference Room",
  "TypeId": 0,
  "ParentSpaceId": "00000000-0000-0000-0000-000000000001",
  "SubtypeId": 0
}
```

### <a name="space-methods"></a>Métodos del espacio

#### <a name="parent--space"></a>Parent() ⇒ `space`

Esta función devuelve el espacio primario del espacio actual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Esta función devuelve los sensores secundarios del espacio actual.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Esta función devuelve los dispositivos secundarios del espacio actual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Esta función devuelve la propiedad extendida y su valor para el espacio actual.

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Nombre de la propiedad extendida |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Esta función devuelve el valor del espacio actual.

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Nombre del valor |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Esta función devuelve los valores históricos del espacio actual.

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Nombre del valor |

#### <a name="notifypayload"></a>Notify(Payload)

Esta función envía una notificación con la carga útil especificada.

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | la carga útil de JSON que se incluye en la notificación |

### <a name="device"></a>Dispositivo

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000002",
  "Name": "Device",
  "FriendlyName": "Temperature Sensing Device",
  "Description": "This device contains a sensor that captures temperature readings.",
  "Type": "None",
  "Subtype": "None",
  "TypeId": 0,
  "SubtypeId": 0,
  "HardwareId": "ABC123",
  "GatewayId": "ABC",
  "SpaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Métodos de dispositivo

#### <a name="parent--space"></a>Parent() ⇒ `space`

Esta función devuelve el espacio primario del dispositivo actual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Esta función devuelve los sensores secundarios del dispositivo actual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Esta función devuelve la propiedad extendida y su valor para el dispositivo actual.

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Nombre de la propiedad extendida |

#### <a name="notifypayload"></a>Notify(Payload)

Esta función envía una notificación con la carga útil especificada.

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | la carga útil de JSON que se incluye en la notificación |

### <a name="sensor"></a>Sensor

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000003",
  "Port": "30",
  "PollRate": 3600,
  "DataType": "Temperature",
  "DataSubtype": "None",
  "Type": "Classic",
  "PortType": "None",
  "DataUnitType": "FahrenheitTemperature",
  "SpaceId": "00000000-0000-0000-0000-000000000000",
  "DeviceId": "00000000-0000-0000-0000-000000000001",
  "PortTypeId": 0,
  "DataUnitTypeId": 0,
  "DataTypeId": 0,
  "DataSubtypeId": 0,
  "TypeId": 0  
}
```

### <a name="sensor-methods"></a>Métodos del sensor

#### <a name="space--space"></a>Space() ⇒ `space`

Esta función devuelve el espacio primario del sensor actual.

#### <a name="device--device"></a>Device() ⇒ `device`

Esta función devuelve el dispositivo primario del sensor actual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Esta función devuelve la propiedad extendida y su valor para el sensor actual.

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Nombre de la propiedad extendida |

#### <a name="value--value"></a>Value() ⇒ `value`

Esta función devuelve el valor del sensor actual.

#### <a name="history--value"></a>History() ⇒ `value[]`

Esta función devuelve los valores históricos del sensor actual.

#### <a name="notifypayload"></a>Notify(Payload)

Esta función envía una notificación con la carga útil especificada.

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | la carga útil de JSON que se incluye en la notificación |

### <a name="value"></a>Valor

```JSON
{
  "DataType": "Temperature",
  "Value": "70",
  "CreatedTime": ""
}
```

### <a name="extended-property"></a>Propiedad extendida

```JSON
{
  "Name": "OccupancyStatus",
  "Value": "Occupied"
}
```

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear puntos de conexión de Azure Digital Twins](how-to-egress-endpoints.md) a los que enviar eventos.

- Para más información sobre los puntos de conexión de Azure Digital Twins, aprenda [más sobre los puntos de conexión](concepts-events-routing.md).
