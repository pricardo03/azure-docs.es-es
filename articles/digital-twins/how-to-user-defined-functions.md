---
title: Uso de funciones definidas por el usuario en Azure Digital Twins | Microsoft Docs
description: Instrucciones sobre cómo crear funciones definidas por el usuario, buscadores de coincidencias y asignaciones de roles con Azure Digital Twins.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: 8094965da5fb0a5fad0313fd96e2878f86d78aa7
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215504"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Uso de funciones definidas por el usuario en Azure Digital Twins

[Las funciones definidas por el usuario](./concepts-user-defined-functions.md) le permiten ejecutar una lógica personalizada en los mensajes de telemetría entrantes y los metadatos de gráficos espaciales, de modo que pueda enviar eventos a los puntos de conexión predefinidos. En esta guía, le analizaremos un ejemplo de reacción ante eventos de temperatura para detectar y notificar sobre cualquier lectura que supere una temperatura determinada.

En los ejemplos siguientes, `https://yourManagementApiUrl` hace referencia al identificador URI de la API de Digital Twins:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suNombreDeInstancia* | El nombre de la instancia de Azure Digital Twins |
| *suUbicación* | La región de servidor en la que está hospedada la instancia |

## <a name="client-library-reference"></a>Referencia de la biblioteca cliente

Las funciones que están disponibles como métodos auxiliares en el tiempo de ejecución de las funciones definidas por el usuario se enumeran en la siguiente [referencia de clientes](#Client-Reference).

## <a name="create-a-matcher"></a>Creación de un buscador de coincidencias

Los buscadores de coincidencias son objetos gráficos que determinan qué funciones definidas por el usuario se ejecutarán para un mensaje de telemetría determinado.

Comparaciones de condiciones válidas para el buscador de coincidencias:

- `Equals`
- `NotEquals`
- `Contains`

Destinos de condiciones válidas para el buscador de coincidencias:

- `Sensor`
- `SensorDevice`
- `SensorSpace`

El buscador de coincidencias de ejemplo a continuación evaluará como true a cualquier evento de telemetría de sensores con `"Temperature"` como su valor de tipo de datos. Puede crear varios buscadores de coincidencias en una función definida por el usuario.

```plaintext
POST https://yourManagementApiUrl/api/v1.0/matchers
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
  "SpaceId": "yourSpaceIdentifier"
}
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suURLDeAPIDeAdministración* | La ruta de dirección URL completa para la API de administración  |
| *suIdentificadorDeEspacio* | La región de servidor en la que está hospedada la instancia |

## <a name="create-a-user-defined-function-udf"></a>Creación de una función definida por el usuario

Tras la creación de los buscadores de coincidencias, cargue el fragmento de código de la función con la siguiente llamada POST:

> [!IMPORTANT]
> - En los encabezados, configure el siguiente valor `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - El cuerpo está formado por varias partes:
>   - La primera contiene los metadatos necesarios para la función definida por el usuario.
>   - La segunda es la lógica de ejecución de Javascript.
> - En la sección `userDefinedBoundary`, reemplace los GUID `SpaceId` y `Machers`.

```plaintext
POST https://yourManagementApiUrl/api/v1.0/userdefinedfunctions with Content-Type: multipart/form-data; boundary="userDefinedBoundary"
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suURLDeAPIDeAdministración* | La ruta de dirección URL completa para la API de administración  |

Cuerpo:

```plaintext
--userDefinedBoundary
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "yourSpaceIdentifier",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["yourMatcherIdentifier"]
}
--userDefinedBoundary
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--userDefinedBoundary--
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suIdentificadorDeEspacio* | El identificador de espacio  |
| *suIdentificadorDeBuscadorDeCoincidencias* | El identificador del buscador de coincidencias que quiere usar |

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

El parámetro *telemetry* expone los atributos **SensorId** y **Message** (que corresponden a un mensaje enviado por un sensor). El parámetro *executionContext* expone los siguientes atributos:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

En el ejemplo siguiente, registraremos un mensaje si la lectura de telemetría del sensor supera un umbral definido previamente. Si la configuración de diagnóstico está habilitada en la instancia de Digital Twins, también se reenviarán los registros de las funciones definidas por el usuario:

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

El código siguiente desencadenará una notificación si el nivel de temperatura supera la constante predefinida.

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

Para obtener un ejemplo de código más complejo para una función definida por el usuario, consulte [comprobación de espacios disponibles con una función definida por el usuario para aire fresco](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js)

## <a name="create-a-role-assignment"></a>Crear una asignación de rol

Es necesario crear una asignación de roles para que se ejecute la función definida por el usuario. Si no se crea, la función no tendrá los permisos adecuados para interactuar con la API de administración y realizar acciones en los objetos gráficos. Las acciones que realiza la función definida por el usuario no están exentas del control de acceso basado en roles dentro de las API de administración de Digital Twins. Es posible limitar su ámbito mediante la especificación de determinados roles o rutas de control de acceso. Para más información, vea la documentación sobre [control de acceso basado en roles](./security-role-based-access-control.md).

1. Realice una consulta a los roles y obtenga el Id. del rol que quiere asignar a la función definida por el usuario; después, páselo al atributo **RoleId** siguiente.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/system/roles
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suURLDeAPIDeAdministración* | La ruta de dirección URL completa para la API de administración  |

2. **ObjectId** será el identificador de la función definida por el usuario que se creó anteriormente.
3. Busque el valor de **Path** mediante la realización de una consulta de los espacios con `fullpath`.
4. Copie el valor de `spacePaths` devuelto, ya que lo utilizará a continuación.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/spaces?name=yourSpaceName&includes=fullpath
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suURLDeAPIDeAdministración* | La ruta de dirección URL completa para la API de administración  |
| *suEspacioDeNombres* | El nombre del espacio que quiere usar |

4. Ahora, pegue el valor de `spacePaths` devuelto en **Path** para crear una asignación de roles de UDF.

```plaintext
POST https://yourManagementApiUrl/api/v1.0/roleassignments
{
  "RoleId": "yourDesiredRoleIdentifier",
  "ObjectId": "yourUserDefinedFunctionId",
  "ObjectIdType": "UserDefinedFunctionId",
  "Path": "yourAccessControlPath"
}
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suURLDeAPIDeAdministración* | La ruta de dirección URL completa para la API de administración  |
| *suIdentificadorDeRolDeseado* | El identificador para el rol deseado |
| *suIdDeFunciónDefinidaPorElUsuario* | El identificador de la función definida por el usuario que quiere usar |
| *suRutaDeControlDeAcceso* | La ruta del control de acceso |

## <a name="send-telemetry-to-be-processed"></a>Envío de la telemetría a procesarse

Los datos de telemetría generados por el sensor que se describe en el gráfico deben desencadenar la ejecución de la función definida por el usuario que se cargó. Una vez que el procesador de datos recopila los datos de telemetría, se crea un plan de ejecución para invocar a la función definida por el usuario.

1. Recuperar los buscadores de coincidencias del sensor del que se generó la lectura.
1. En función de qué buscadores de coincidencia se evalúen correctamente, recuperar las funciones definidas por el usuario asociadas.
1. Ejecutar cada función definida por el usuario.

## <a name="client-reference"></a>Referencia de cliente

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Dado un identificador de espacio, recupera el espacio del gráfico.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *id*  | `guid` | identificador del espacio |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Dado un identificador de sensor, recupera el sensor del gráfico.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *id*  | `guid` | identificador del sensor |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Dado un identificador de dispositivo, recupera el dispositivo del gráfico.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | identificador de dispositivo |

### <a name="getsensorvaluesensorid-datatype--value"></a>getSensorValue(sensorId, dataType) ⇒ `value`

Dado un identificador de sensor y su tipo de datos, recupera el valor actual de ese sensor.

**Tipo**: función global

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | identificador del sensor |
| *dataType*  | `string` | tipo de datos del sensor |

### <a name="getspacevaluespaceid-valuename--value"></a>getSpaceValue(spaceId, valueName) ⇒ `value`

Dado un identificador de espacio y el nombre del valor, recupera el valor actual de esa propiedad de espacio.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | identificador del espacio |
| *valueName* | `string` | nombre de la propiedad de espacio |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>getSensorHistoryValues(sensorId, dataType) ⇒ `value[]`

Dado un identificador de sensor y su tipo de datos, recupera el historial de valores de ese sensor.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | identificador del sensor |
| *dataType* | `string` | tipo de datos del sensor |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>getSpaceHistoryValues(spaceId, dataType) ⇒ `value[]`

Dado un identificador de espacio y el nombre del valor, recupera el historial de valores de esa propiedad de espacio.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | identificador del espacio |
| *valueName* | `string` | nombre de la propiedad de espacio |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Dado un identificador de espacio, recupera los espacios secundarios de ese espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | identificador del espacio |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Dado un identificador de espacio, recupera los sensores secundarios de ese espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | identificador del espacio |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Dado un identificador de espacio, recupera los dispositivos secundarios de ese espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | identificador del espacio |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Dado un identificador de espacio, recupera los sensores secundarios de ese dispositivo primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | identificador de dispositivo |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Dado un identificador de espacio, recupera su espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | identificador del espacio |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Dado un identificador de sensor, recupera su espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | identificador del sensor |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Dado un identificador de dispositivo, recupera su espacio primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | identificador de dispositivo |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Dado un identificador de sensor, recupera su dispositivo primario.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | identificador del sensor |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>getSpaceExtendedProperty(spaceId, propertyName) ⇒ `extendedProperty`

Dado un identificador de espacio, recupera la propiedad y su valor desde el espacio.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | identificador del espacio |
| *propertyName* | `string` | nombre de la propiedad de espacio |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>getSensorExtendedProperty(sensorId, propertyName) ⇒ `extendedProperty`

Dado un identificador de sensor, recupera la propiedad y su valor desde el sensor.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | identificador del sensor |
| *propertyName* | `string` | nombre de la propiedad del sensor |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>getDeviceExtendedProperty(deviceId, propertyName) ⇒ `extendedProperty`

Dado un identificador de dispositivo, recupera la propiedad y su valor desde el espacio.

**Tipo**: función global

| Parámetro  | Escriba                | DESCRIPCIÓN  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | identificador de dispositivo |
| *propertyName* | `string` | nombre de la propiedad del dispositivo |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue(sensorId, dataType, value)

Establece un valor en el objeto de sensor con el tipo de datos indicado.

**Tipo**: función global

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | identificador del sensor |
| *dataType*  | `string` | tipo de datos del sensor |
| *value*  | `string` | value |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue(spaceId, dataType, value)

Establece un valor en el objeto de espacio con el tipo de datos indicado.

**Tipo**: función global

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | identificador del espacio |
| *dataType* | `string` | tipo de datos |
| *value* | `string` | value |

### <a name="logmessage"></a>log(Message)

Registra el mensaje siguiente dentro de la función definida por el usuario.

**Tipo**: función global

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *message* | `string` | mensaje que se va a registrar |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification(topologyObjectId, topologyObjectType, payload)

Envía una notificación personalizada para que se distribuya.

**Tipo**: función global

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | identificador de objeto gráfico (Ejemplo: identificador de espacio, sensor o dispositivo)|
| *topologyObjectType*  | `string` | (Ejemplo: espacio, sensor o dispositivo)|
| *payload*  | `string` | la carga útil de JSON que se envía con la notificación |

## <a name="return-types"></a>Tipos de valor devuelto

Estos son los modelos que describen a los objetos devueltos por la referencia de cliente anterior.

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

Devuelve el espacio primario del espacio actual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Devuelve los sensores secundarios del espacio actual.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Devuelve los dispositivos secundarios del espacio actual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Devuelve la propiedad extendida y su valor para el espacio actual.

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | nombre de la propiedad extendida |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Devuelve el valor del espacio actual.

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | nombre del valor |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Devuelve el historial de valores del espacio actual.

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | nombre del valor |

#### <a name="notifypayload"></a>Notify(Payload)

Envía una notificación con la carga útil especificada.

| Parámetro  | Escriba                | Descripción  |
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

Devuelve el espacio primario del dispositivo actual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Devuelve los sensores secundarios del dispositivo actual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Devuelve la propiedad extendida y su valor para el dispositivo actual.

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | nombre de la propiedad extendida |

#### <a name="notifypayload"></a>Notify(Payload)

Envía una notificación con la carga útil especificada.

| Parámetro  | Escriba                | Descripción  |
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

Devuelve el espacio primario del sensor actual.

#### <a name="device--device"></a>Device() ⇒ `device`

Devuelve el dispositivo primario del sensor actual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Devuelve la propiedad extendida y su valor para el sensor actual.

| Parámetro  | Escriba                | Descripción  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | nombre de la propiedad extendida |

#### <a name="value--value"></a>Value() ⇒ `value`

Devuelve el valor del sensor actual.

#### <a name="history--value"></a>History() ⇒ `value[]`

Devuelve el historial de valores del sensor actual.

#### <a name="notifypayload"></a>Notify(Payload)

Envía una notificación con la carga útil especificada.

| Parámetro  | Escriba                | Descripción  |
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

Para obtener información sobre cómo crear puntos de conexión de Digital Twins para enviarles eventos, consulte [creación de puntos de conexión de Digital Twins](how-to-egress-endpoints.md).

Para obtener más detalles sobre los puntos de conexión de Digital Twins, consulte [más información sobre los puntos de conexión](concepts-events-routing.md).
