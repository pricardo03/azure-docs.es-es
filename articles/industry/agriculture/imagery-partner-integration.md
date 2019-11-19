---
title: Integración de asociados de imágenes
description: Se describe la integración de asociados de imágenes.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 788ffd9e7036996f6ac1bc7fcbc33137aca40ee2
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132027"
---
# <a name="imagery-partner-integration"></a>Integración de asociados de imágenes

En este artículo se describe cómo usar el componente traductor de Azure FarmBeats para enviar datos de imágenes a FarmBeats. Los datos de imágenes agrarias pueden proceder de diversos orígenes, como cámaras multiespectrales, satélites y drones. Los asociados de imágenes agrarias pueden integrarse con FarmBeats para proporcionar a los clientes mapas generados de forma personalizada para sus granjas.

Los datos, una vez disponibles, pueden visualizarse mediante el acelerador de FarmBeats y usarse para la fusión de datos y la creación de un modelo de ML/IA (aprendizaje automático/inteligencia artificial) por parte de las empresas agrarias o los integradores de sistemas de clientes.

FarmBeats ofrece la posibilidad de:

- Definir los tipos, el origen y el formato de archivo de imágenes personalizadas mediante instancias de Extended Type API
- Ingerir datos de imágenes de varios orígenes a través de Scene API y SceneFile API.

La siguiente información va dirigida a obtener cualquier forma de imagen en el sistema FarmBeats.

Al seleccionar la sección de imágenes de dron, se abre un elemento emergente que muestra una imagen de alta resolución del ortomosaico del dron. Puede acceder al software del asociado, que ayuda a planear vuelos de dron y obtener datos sin procesar. El software del asociado se seguirá usando para el planeamiento de la ruta y la unión de imágenes de ortomosaico.

Los asociados del dron deben permitir que los clientes vinculen su cuenta a su instancia de FarmBeats en Azure.

Es necesario usar las siguientes credenciales en el software del asociado del dron para vincular FarmBeats:

- Punto de conexión de API
- Id. de inquilino
- Id. de cliente
- Secreto del cliente

## <a name="api-development"></a>Desarrollo de API

Las API contienen documentación técnica de Swagger. Consulte [Swagger](https://aka.ms/FarmBeatsDatahubSwagger) para información sobre las API y las solicitudes y respuestas correspondientes.

## <a name="authentication"></a>Authentication

FarmBeats aprovecha la autenticación de [Active Directory](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization) de Microsoft Azure. Azure App Service proporciona compatibilidad integrada con la autenticación y la autorización. 

Para más información al respecto, consulte [Azure Active Directory](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization).   

El centro de datos de FarmBeats usa la autenticación de portador, que necesita las credenciales siguientes:

- Id. de cliente
- Secreto del cliente
- Id. de inquilino

Con las credenciales anteriores, el autor de la llamada puede solicitar un token de acceso, que debe enviarse en las sucesivas solicitudes de API en la sección de encabezado de la siguiente manera:

```
headers = {"Authorization": "Bearer " + access_token, …} 
```

A continuación se muestra un ejemplo de código de Python que recupera el token de acceso. El token se puede usar luego en las sucesivas llamadas API a FarmBeat:   
import azure 

```python
from azure.common.credentials import ServicePrincipalCredentials 
import adal 
#FarmBeats API Endpoint 
ENDPOINT = "https://<yourdatahub>.azurewebsites.net"   
CLIENT_ID = "<Your Client ID>"   
CLIENT_SECRET = "<Your Client Secret>"   
TENANT_ID = "<Your Tenant ID>" 
AUTHORITY_HOST = 'https://login.microsoftonline.com' 
AUTHORITY = AUTHORITY_HOST + '/' + TENANT_ID 
#Authenticating with the credentials 
context = adal.AuthenticationContext(AUTHORITY) 
token_response = context.acquire_token_with_client_credentials(ENDPOINT, CLIENT_ID, CLIENT_SECRET) 
#Should get an access token here 
access_token = token_response.get('accessToken') 
```

## <a name="http-request-headers"></a>Encabezados de solicitud HTTP

Estos son los encabezados de solicitud más comunes que deben especificarse al realizar una llamada API al centro de datos de FarmBeats:

**Encabezado** | **Descripción y ejemplo**
--- | ---
Content-Type  | El formato de la solicitud (Content-Type: application/<format>), que es JSON para las API del centro de datos de FarmBeats. Content-Type: application/json
Authorization | Especifica el token de acceso necesario para realizar una llamada API. Autorización: Bearer <Access-Token>
Accept  | El formato de respuesta. En el caso de las API del centro de datos de FarmBeats, el formato es JSON: Accept: Application/JSON


## <a name="api-requests"></a>Solicitudes de API

Para realizar una solicitud de API REST, se combina el método HTTP (GET/POST/PUT), la dirección URL del servicio de API, el URI del recurso (para consultar, enviar, actualizar o eliminar datos) y uno o más encabezados de solicitud HTTP.

También se pueden incluir parámetros de consulta en las llamadas GET para filtrar y ordenar los datos en las respuestas y limitar su tamaño.

La siguiente solicitud de ejemplo se usa para obtener la lista de dispositivos:

```bash
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H
"Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>”
```

La mayoría de las llamadas GET, POST y PUT requieren un cuerpo de solicitud JSON.

La siguiente solicitud de ejemplo se usa para crear un dispositivo (contiene un código JSON de entrada con el cuerpo de la solicitud).


```bash
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  
"accept: application/json" -H  
"Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d
"{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  
\"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

## <a name="data-format"></a>Formato de datos

JSON (notación de objetos JavaScript) es un formato de datos común independiente del lenguaje que proporciona una representación de texto simple de estructuras de datos arbitrarias. Para más información, consulte [JSON.org](https://JSON.org).

## <a name="ingesting-imagery-into-farmbeats"></a>Ingesta de imágenes en FarmBeats

Una vez que el asociado tiene credenciales para conectarse al centro de datos de FarmBeats, realiza lo siguiente en el componente traductor:

1.  Crea un tipo extendido para los siguientes campos, de acuerdo con el tipo de imagen que se va a cargar:

    - Scene Source (Origen de escena): por ejemplo, <nombre_de_asociado_del_dron>
    - Scene Type (Tipo de escena): Por ejemplo: <drone>
    - Scene File Type (Tipo de archivo de escena): Por ejemplo: <chlorophyll index>
    - Scene File Content Type (Tipo de contenido del archivo de escena): por ejemplo, <imagen/tiff>

2.  Llama a Farms API para obtener la lista de granjas del sistema Azure FarmBeats.
3.  Proporciona al cliente la posibilidad de elegir una sola granja de la lista.

    El sistema del asociado debe mostrar la granja en el software del asociado para planear la ruta y el vuelo del dron, así como para recopilar imágenes.

4.  Llama a Scene API y proporciona los detalles necesarios para crear una escena con un identificador único.
5.  Recibe una dirección URL de SAS de blob para cargar las imágenes necesarias en el centro de datos de FarmBeats, en el contexto de la granja elegida, en el sistema FarmBeats.

A continuación se muestra un flujo detallado de las llamadas API:

### <a name="step-1-extendedtype"></a>Paso 1: ExtendedType

Inserte en el repositorio ExtendedType API si el tipo y el origen del archivo están disponibles en FarmBeats. Para ello, puede llamar a GET en /ExtendedType API.

A continuación se indican los valores definidos por el sistema:

```json
{
  "items": [
    {
      "id": "ae2aa527-7d27-42e2-8fcf-841937c651d7",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:47:42.5910344Z",
      "key": "SceneFileType",
      "value": [
        "evi",
        "ndvi",
        "ndwi",
        "tci",
        "soil-moisture",
        "sensor-placement",
        "sentinel-b01",
        "sentinel-b02",
        "sentinel-b03",
        "sentinel-b04",
        "sentinel-b05",
        "sentinel-b06",
        "sentinel-b07",
        "sentinel-b08",
        "sentinel-b8a",
        "sentinel-b09",
        "sentinel-b10",
        "sentinel-b11",
        "sentinel-b12",
        "cloud-mask",
        "farm-mask",
        "image/tiff",
        "orthomosaic"
      ],
      "description": "List of scene file types available in system. User can add more values."
    },
    {
      "id": "b7824d2e-3e43-46d3-8fd4-ec7ba8918d84",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:45:44.8914691Z",
      "key": "SceneFileContentType",
      "value": [
        "image/tiff",
        "image/png",
        "image/jpeg",
        "text/csv",
        "text/plain",
        "text/tab-separated-values",
        "application/json",
        "application/octet-stream",
        "orthomosaic"
      ],
      "description": "List of scene file content types available in system. User can add more values."
    },
    {
      "id": "21d2924d-8b3e-4ce4-a7f8-318e85c0f7f2",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:45:37.5096445Z",
      "key": "SceneSource",
      "value": [
        "sentinel-l1c",
        "sentinel-l2a",
        "farmbeats-model",
        "dji",
        "SlantRange-3P",
        "DJI"
      ],
      "description": "List of scene sources available in system. User can add more values."
    },
    {
      "id": "6e09ca69-cacf-4b53-b63a-53c659aae4a4",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:45:34.7483899Z",
      "key": "SceneType",
      "value": [
        "base-bands",
        "sensor-placement",
        "soil-moisture",
        "evi",
        "ndwi",
        "ndvi",
        "drone",
        "orthomosaic-drone"
      ],
      "description": "List of scene types available in system. User can add more values."
    },
    {
      "id": "55d8436b-3a86-4bea-87ab-e1b51226525f",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-06-17T07:32:30.1135716Z",
      "key": "SensorMeasureType",
      "value": [
        "AmbientTemperature",
        "CO2",
        "Depth",
        "ElectricalConductivity",
        "LeafWetness",
        "Length",
        "LiquidLevel",
        "Nitrate",
        "O2",
        "PH",
        "Phosphate",
        "PointInTime",
        "Potassium",
        "Pressure",
        "RainGauge",
        "RelativeHumidity",
        "Salinity",
        "SoilMoisture",
        "SoilTemperature",
        "SolarRadiation",
        "State",
        "TimeDuration",
        "UVRadiation",
        "UVIndex",
        "Volume",
        "WindDirection",
        "WindRun",
        "WindSpeed",
        "Evapotranspiration",
        "PAR"
      ],
      "description": "List of sensor measure types available in system. User can add more values."
    },
    {
      "id": "0dfd6e6b-df58-428f-9ab8-a0674bfdcbe5",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-06-17T07:32:30.1135716Z",
      "key": "SensorMeasureUnit",
      "value": [
        "NoUnit",
        "Celsius",
        "Fahrenheit",
        "Kelvin",
        "Rankine",
        "Pascal",
        "Mercury",
        "PSI",
        "MilliMeter",
        "CentiMeter",
        "Meter",
        "Inch",
        "Feet",
        "Mile",
        "KiloMeter",
        "MilesPerHour",
        "MilesPerSecond",
        "KMPerHour",
        "KMPerSecond",
        "MetersPerHour",
        "MetersPerSecond",
        "Degree",
        "WattsPerSquareMeter",
        "KiloWattsPerSquareMeter",
        "MilliWattsPerSquareCentiMeter",
        "MilliJoulesPerSquareCentiMeter",
        "VolumetricWaterContent",
        "Percentage",
        "PartsPerMillion",
        "MicroMol",
        "MicroMolesPerLiter",
        "SiemensPerSquareMeterPerMole",
        "MilliSiemensPerCentiMeter",
        "Centibar",
        "DeciSiemensPerMeter",
        "KiloPascal",
        "VolumetricIonContent",
        "Liter",
        "MilliLiter",
        "Seconds",
        "UnixTimestamp",
        "MicroMolPerMeterSquaredPerSecond",
        "InchesPerHour"
      ],
      "description": "List of sensor measure units available in system. User can add more values."
    }
  ]
}
```

Se trata de una configuración de una sola vez, y el ámbito de este nuevo tipo de escena se limita a la suscripción en la que se implementa el proyecto de FarmBeats.

Ejemplo: Para agregar SceneSource: "SlantRange", use PUT en el identificador de /ExtendedType con la clave: Carga de entrada de "SceneSource":

```json
{

      "key": "SceneSource",
      "value": [
        "sentinel-l1c",
        "sentinel-l2a",
        "farmbeats-model",
        "dji",
        "SlantRange-3P"
      ],
      "description": "List of scene sources available in system. User can add more values."
}

```

El campo verde es la nueva adición a los valores de origen de la escena definidos por el sistema.

### <a name="step-2-get-farmdetails"></a>Paso 2: Obtención de FarmDetails

Las escenas (archivos TIFF o CSV) se situarán en el contexto de una granja. Para obtener los detalles de la granja, use GET en /Farm API. La API le devolverá la lista de granjas disponibles en FarmBeats, y podrá seleccionar aquella para la que quiere ingerir los datos.

Obtención de la respuesta de /Farm:

```json
{
  "id": "07f3e09c-89aa-4619-8d50-e57fb083d8f9",
  "createdAt": "2019-11-01T13:55:41.8804663Z",
  "lastModifiedAt": "2019-11-01T13:55:41.8804663Z",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          78.34252251428694,
          17.402556352862675
        ],
        [
          78.34278000635095,
          17.407920852463022
        ],
        [
          78.34883106989963,
          17.40878079576528
        ],
        [
          78.3498181228054,
          17.403539173730607
        ],
        [
          78.34805859369493,
          17.39977166504127
        ],
        [
          78.34252251428694,
          17.402556352862675
        ]
      ]
    ]
  },
  "name": "Samplefarm",
  "properties": {
    "crops": "Corn",
    "address": "Sample street"
  }
}
 ```

### <a name="step-3-create-ascene-id-post-call"></a>Paso 3: Creación de un identificador de /Scene (llamada POST)

Cree una escena (archivo TIFF o CSV) con la información dada y proporcione la fecha, la secuencia y el identificador de la granja con la que se asociará. Los metadatos asociados a la escena pueden definirse en las propiedades, incluida la duración y el tipo de medida.

Esta acción crea un valor de SceneID, que se asociará a la granja. Una vez creado el valor de SceneID, el usuario puede usarlo para crear un archivo (TIFF o CSV) y almacenar su contenido.

Ejemplo de carga de entrada para la llamada POST en /Scene API

```json
{
  "sceneId": "a0505928-c480-491b-ba31-d38285a28c1d",
  "type": "newtype",
  "contentType": "image/tiff",
  "name": "test scene file",
  "description": "test scene file description",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

Respuesta de la API:

```json
{
  "id": "a0505928-c480-491b-ba31-d38285a28c1d",
  "createdAt": "2019-10-04T16:19:12.4838584Z",
  "lastModifiedAt": "2019-10-04T16:19:12.4838584Z",
  "type": "new type",
  "source": "SlantRange-3P",
  "farmId": "d41a33e7-b73e-480e-9279-0fcb3207332b",
  "date": "2019-10-04T16:13:39.064Z",
  "sequence": 5,
  "name": "test scene",
  "description": "test scene description",
  "properties": {}
}

```

**Create/SceneFile**

El identificador de escena devuelto en el paso tres es la entrada de SceneFile. SceneFile devuelve un token de dirección URL de SAS, que es válido durante 24 horas.

Si el usuario necesita una manera programática de cargar una secuencia de imágenes, se puede usar el SDK de almacenamiento de blobs para definir un método mediante el identificador, la ubicación y la dirección URL de SceneFile.

Ejemplo de carga de entrada para la llamada POST en /Scenefile API:

```json
{
  "sceneId": "a0505928-c480-491b-ba31-d38285a28c1d",
  "type": "newtype",
  "contentType": "image/tiff",
  "name": "test scene file",
  "description": "test scene file description",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```
Respuesta de la API:

```json
{
  "uploadSASUrl": "https://storagej2lho.blob.core.windows.net/farm-scene/2019/a0505928-c480-491b-ba31-d38285a28c1d/e91139a7-4ebd-4e2f-b17c-c677822dc840?sv=2018-03-28&sr=b&sig=%2F1426JkDcIFE5g3d%2BjOevCVMIn%2FJo9YKwBn3La5zL8Y%3D&se=2019-10-05T16%3A23%3A57Z&sp=w",
  "id": "e91139a7-4ebd-4e2f-b17c-c677822dc840",
  "createdAt": "2019-10-04T16:23:57.1192916Z",
  "lastModifiedAt": "2019-10-04T16:23:57.1192916Z",
  "blobUrl": "https://storagej2lho.blob.core.windows.net/farm-scene/2019/a0505928-c480-491b-ba31-d38285a28c1d/e91139a7-4ebd-4e2f-b17c-c677822dc840",
  "sceneId": "a0505928-c480-491b-ba31-d38285a28c1d",
  "type": "newtype",
  "contentType": "image/tiff",
  "name": "test scene file",
  "description": "test scene file description",
  "properties": {}
}


```

La llamada POST a /SceneFile API devuelve una dirección URL de carga de SAS, que se puede usar para cargar el archivo CSV o TIFF mediante el cliente o la biblioteca de Azure Blob Storage.


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la integración basada en la API REST, consulte [API REST](references-for-farmbeats.md#rest-api).
