---
title: 'Guía de inicio rápido: Ingesta de datos mediante la biblioteca de Node de Azure Data Explorer'
description: En esta guía de inicio rápido, obtendrá información sobre cómo ingerir (cargar) datos en Azure Data Explorer con Node.js.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 10/25/2018
ms.openlocfilehash: fa322ee685d09717ac5b98398d4d1d61de2be1e9
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706643"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-node-library"></a>Guía de inicio rápido: Ingesta de datos mediante la biblioteca de Node de Azure Data Explorer

El Explorador de datos de Azure es un servicio de exploración de datos altamente escalable y rápido para datos de telemetría y registro. Azure Data Explorer proporciona dos bibliotecas cliente para Node: una [biblioteca de ingesta](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-ingest) y una [biblioteca de datos](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-data). Estas bibliotecas permiten ingerir (cargar) datos en un clúster y consultar datos desde el código. En esta guía de inicio rápido, primero creará una tabla y la asignación de datos en un clúster de prueba. A continuación, pondrá en cola la ingesta en el clúster y validará los resultados.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Además de una suscripción de Azure, necesita lo siguiente para completar esta guía de inicio rápido:

* [Base de datos y clúster de prueba](create-cluster-database-portal.md)

* [Node.js](https://nodejs.org/en/download/) instalado en el equipo de desarrollo

## <a name="install-the-data-and-ingest-libraries"></a>Instalación de los datos y las bibliotecas de ingesta

Instale *azure-kusto-ingest* y *azure-kusto-data*.

```bash
npm i --save azure-kusto-ingest azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>Incorporación de instrucciones de importación y constantes

Importación de clases de las bibliotecas

```javascript
const KustoClient = require('azure-kusto-data').KustoClient;
const KustoIngestClient = require('azure-kusto-ingest').KustoIngestClient;
const KustoConnectionStringBuilder = require('azure-kusto-ingest').KustoConnectionStringBuilder;
```

Para autenticar una aplicación, Azure Data Explorer usa el identificador de inquilino de Azure Active Directory. Para buscar el identificador de inquilino, consulte [Busque el identificador del inquilino de Office 365](https://docs.microsoft.com/onedrive/find-your-office-365-tenant-id).

Establezca los valores de `authorityId`, `kustoUri`, `kustoIngestUri` y `kustoDatabase` antes de ejecutar este código.

```javascript
const authorityId = "<TenantId>";
const kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
const kustoIngestUri = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/"
const kustoDatabase  = "<DatabaseName>"
```

Ahora, cree la cadena de conexión. En este ejemplo se utiliza la autenticación de dispositivos para acceder al clúster. También puede usar el certificado de aplicación, la clave de aplicación y el usuario y la contraseña de Azure Active Directory.

Creará la tabla de destino y la asignación en un paso posterior.

```javascript
const kcsbIngest = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoIngestUri, authorityId);

const kcsbData = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoUri, authorityId);

const destTable = "StormEvents";
const destTableMapping = "StormEvents_CSV_Mapping";
```

## <a name="set-source-file-information"></a>Definición de la información del archivo de origen

Importe clases adicionales y defina las constantes para el archivo de origen de datos. Este ejemplo utiliza un archivo de ejemplo hospedado en Azure Blob Storage. El conjunto de datos de ejemplo de **StormEvents** contiene datos relacionados con el tiempo de los [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/).

```javascript
from azure.storage.blob import BlockBlobService
from azure.kusto.ingest import KustoIngestClient, IngestionProperties, FileDescriptor, BlobDescriptor, DataFormat, ReportLevel, ReportMethod

const container = "samplefiles";
const account = "kustosamplefiles";
const sas = "?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D"
const filePath = "StormEvents.csv";
const fileSize = 64158321    # in bytes

const blobPath = `https://${account}.blob.core.windows.net/${container}/${filePath}${sas}";
```

## <a name="create-a-table-on-your-test-cluster"></a>Creación de una tabla en el clúster de prueba

Cree una tabla que coincida con el esquema de los datos del archivo `StormEvents.csv`. Cuando se ejecuta este código, devuelve un mensaje similar al siguiente: *Para iniciar sesión, use un explorador web para abrir la página https://microsoft.com/devicelogin y escriba el código XXXXXXXXX para autenticarse*. Siga los pasos para iniciar sesión y, a continuación, vuelva a ejecutar el siguiente bloque de código. Los bloques de código subsiguientes que establecen una conexión requieren que vuelva a iniciar sesión.

```javascript
const kustoClient = new KustoClient(kcsbData);
const createTableCommand = ".create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)"

kustoClient.executeMgmt(kustoDatabase, createTableCommand, (err, results) => {
    console.log(result.primaryResults[0]);
});
```

## <a name="define-ingestion-mapping"></a>Definición de la asignación de ingesta

Asigna los datos de CSV entrantes a los nombres de columna y tipos de datos utilizados al crear la tabla.

```javascript
const createMappingCommand = `.create table StormEvents ingestion csv mapping ${destTableMapping} '[{\"Name\":\"StartTime\",\"datatype\":\"datetime\",\"Ordinal\":0}, {\"Name\":\"EndTime\",\"datatype\":\"datetime\",\"Ordinal\":1},{\"Name\":\"EpisodeId\",\"datatype\":\"int\",\"Ordinal\":2},{\"Name\":\"EventId\",\"datatype\":\"int\",\"Ordinal\":3},{\"Name\":\"State\",\"datatype\":\"string\",\"Ordinal\":4},{\"Name\":\"EventType\",\"datatype\":\"string\",\"Ordinal\":5},{\"Name\":\"InjuriesDirect\",\"datatype\":\"int\",\"Ordinal\":6},{\"Name\":\"InjuriesIndirect\",\"datatype\":\"int\",\"Ordinal\":7},{\"Name\":\"DeathsDirect\",\"datatype\":\"int\",\"Ordinal\":8},{\"Name\":\"DeathsIndirect\",\"datatype\":\"int\",\"Ordinal\":9},{\"Name\":\"DamageProperty\",\"datatype\":\"int\",\"Ordinal\":10},{\"Name\":\"DamageCrops\",\"datatype\":\"int\",\"Ordinal\":11},{\"Name\":\"Source\",\"datatype\":\"string\",\"Ordinal\":12},{\"Name\":\"BeginLocation\",\"datatype\":\"string\",\"Ordinal\":13},{\"Name\":\"EndLocation\",\"datatype\":\"string\",\"Ordinal\":14},{\"Name\":\"BeginLat\",\"datatype\":\"real\",\"Ordinal\":16},{\"Name\":\"BeginLon\",\"datatype\":\"real\",\"Ordinal\":17},{\"Name\":\"EndLat\",\"datatype\":\"real\",\"Ordinal\":18},{\"Name\":\"EndLon\",\"datatype\":\"real\",\"Ordinal\":19},{\"Name\":\"EpisodeNarrative\",\"datatype\":\"string\",\"Ordinal\":20},{\"Name\":\"EventNarrative\",\"datatype\":\"string\",\"Ordinal\":21},{\"Name\":\"StormSummary\",\"datatype\":\"dynamic\",\"Ordinal\":22}]'`;

kustoClient.executeMgmt(kustoDatabase, createMappingCommand, (err, results) => {
    console.log(result.primaryResults[0]);
});
```

## <a name="queue-a-message-for-ingestion"></a>Colocación de un mensaje en cola para la ingesta

Coloca en cola un mensaje para extraer datos desde Blob Storage e introduce esos datos en el Explorador de datos de Azure.

```javascript
const { DataFormat, JsonColumnMapping } =  require("azure-kusto-ingest").IngestionPropertiesEnums;
const { BlobDescriptor } = require("azure-kusto-ingest").Descriptors;
const ingestClient = new KustoIngestClient(kcsbIngest);


// All ingestion properties are documented here: https://docs.microsoft.com/azure/kusto/management/data-ingest#ingestion-properties
const ingestionProps  = new IngestionProperties(kustoDatabase, destTable, DataFormat.csv, null,destTableMapping, {'ignoreFirstRecord': 'true'});
const blobDesc = new BlobDescriptor(blobPath, 10);
ingestClient.ingestFromBlob(blobDesc,ingestionProps, (err) => {
    if (err) throw new Error(err);
});
```

## <a name="validate-that-table-contains-data"></a>Validación de que la tabla contiene datos

Valide la ingesta de los datos en la tabla. Espere entre cinco y diez minutos para que la ingesta en cola programe la ingesta y cargue los datos en el Explorador de datos de Azure. A continuación, ejecute el siguiente código para obtener el recuento de registros de la tabla `StormEvents`.

```javascript
const query = "StormEvents | count";

kustoClient.execute(kustoDatabse, query, (err, results) => {
    if (err) throw new Error(err);  
    console.log(results.primaryResults[0].toString());
});
```

## <a name="run-troubleshooting-queries"></a>Ejecución de consultas de solución de problemas

Inicie sesión en [https://dataexplorer.azure.com](https://dataexplorer.azure.com) y conéctese al clúster Ejecute el siguiente comando en la base de datos para ver si se ha producido algún error de ingesta en las últimas cuatro horas. Reemplace el nombre de la base de datos antes de ejecutarlo.
    
```Kusto
.show ingestion failures
| where FailedOn > ago(4h) and Database == "<DatabaseName>"
```

Ejecute el siguiente comando para ver el estado de todas las operaciones de ingesta en las últimas cuatro horas. Reemplace el nombre de la base de datos antes de ejecutarlo.

```Kusto
.show operations
| where StartedOn > ago(4h) and Database == "<DatabaseName>" and Operation == "DataIngestPull"
| summarize arg_max(LastUpdatedOn, *) by OperationId
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si tiene previsto seguir nuestros tutoriales y guías de inicio rápido, conserve los recursos que creó. De lo contrario, ejecute el siguiente comando en la base de datos para limpiar la tabla `StormEvents`.

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Escritura de consultas](write-queries.md)
