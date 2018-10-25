---
title: 'Guía de inicio rápido: Ingesta de datos mediante la biblioteca de Python del Explorador de datos de Azure'
description: En esta guía de inicio rápido, obtendrá información sobre cómo ingerir (cargar) datos en el Explorador de datos de Azure con Python.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 10/16/2018
ms.openlocfilehash: 5ebf7b580acb404c8016ba39fb522bc3b2ba7b84
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953927"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-python-library"></a>Guía de inicio rápido: Ingesta de datos mediante la biblioteca de Python del Explorador de datos de Azure

El Explorador de datos de Azure es un servicio de exploración de datos altamente escalable y rápido para datos de telemetría y registro. El Explorador de datos de Azure proporciona dos bibliotecas cliente para Python: una [biblioteca de ingesta](https://github.com/Azure/azure-kusto-python/tree/master/azure-kusto-ingest) y una [biblioteca de datos](https://github.com/Azure/azure-kusto-python/tree/master/azure-kusto-data). Estas bibliotecas permiten ingerir (cargar) datos en un clúster y consultar datos desde el código. En esta guía de inicio rápido, primero creará una tabla y la asignación de datos en un clúster de prueba. A continuación, pondrá en cola la ingesta en el clúster y validará los resultados.

Esta guía de inicio rápido también está disponible como un [Azure Notebook](https://notebooks.azure.com/ManojRaheja/libraries/KustoPythonSamples/html/QueuedIngestSingleBlob.ipynb).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Además de una suscripción de Azure, necesita lo siguiente para completar esta guía de inicio rápido:

* [Una base de datos y un clúster de prueba](create-cluster-database-portal.md)

* [Python](https://www.python.org/downloads/) instalado en el equipo de desarrollo

## <a name="install-the-data-and-ingest-libraries"></a>Instalación de los datos y las bibliotecas de ingesta

Instale *azure-kusto-data* y *azure-kusto-ingest*.

```
pip install azure-kusto-data
pip install azure-kusto-ingest
```

## <a name="add-import-statements-and-constants"></a>Incorporación de instrucciones de importación y constantes

Importe clases de las bibliotecas, así como *datetime* y *pandas*, una biblioteca de análisis de datos.

```python
from azure.kusto.data.request import KustoClient, KustoConnectionStringBuilder
from azure.kusto.data.exceptions import KustoServiceError
import pandas as pd
import datetime
```

Para autenticar una aplicación, el Explorador de datos de Azure usa el identificador del inquilino de AAD. Para buscar el identificador de inquilino, use la dirección URL siguiente, sustituyendo su dominio por *SuDominio*.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Por ejemplo, si el nombre de dominio es *contoso.com*, la dirección URL es: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Haga clic en esta dirección URL para ver los resultados. la primera línea es como sigue. 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

En este caso es el id. de inquilino es `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. Establezca los valores para AAD_TENANT_ID, KUSTO_URI, KUSTO_INGEST_URI y KUSTO_DATABASE antes de ejecutar este código.

```python
AAD_TENANT_ID = "<TenantId>"
KUSTO_URI = "https://<ClusterName>.<Region>.kusto.windows.net:443/"
KUSTO_INGEST_URI = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/"
KUSTO_DATABASE  = "<DatabaseName>"
```

Ahora, cree la cadena de conexión. En este ejemplo se utiliza la autenticación de dispositivos para acceder al clúster. También puede usar el certificado de la aplicación de AAD, la clave de aplicación de AAD y el usuario y la contraseña de AAD.

Creará la tabla de destino y la asignación en un paso posterior.

```python
KCSB_INGEST = KustoConnectionStringBuilder.with_aad_device_authentication(KUSTO_INGEST_URI, AAD_TENANT_ID)

KCSB_DATA = KustoConnectionStringBuilder.with_aad_device_authentication(KUSTO_URI, AAD_TENANT_ID)

DESTINATION_TABLE = "StormEvents"
DESTINATION_TABLE_COLUMN_MAPPING = "StormEvents_CSV_Mapping"
```

## <a name="set-source-file-information"></a>Definición de la información del archivo de origen

Importe clases adicionales y defina las constantes para el archivo de origen de datos. Este ejemplo utiliza un archivo de ejemplo hospedado en Azure Blob Storage. El conjunto de datos de ejemplo de **StormEvents** contiene datos relacionados con el tiempo de los [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/).

```python
from azure.storage.blob import BlockBlobService
from azure.kusto.ingest import KustoIngestClient, IngestionProperties, FileDescriptor, BlobDescriptor, DataFormat, ReportLevel, ReportMethod

CONTAINER = "samplefiles"
ACCOUNT_NAME = "kustosamplefiles"
SAS_TOKEN = "?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D"
FILE_PATH = "StormEvents.csv"
FILE_SIZE = 64158321    # in bytes

BLOB_PATH = "https://" + ACCOUNT_NAME + ".blob.core.windows.net/" + CONTAINER + "/" + FILE_PATH + SAS_TOKEN
```

## <a name="create-a-table-on-your-test-cluster"></a>Creación de una tabla en el clúster de prueba

Cree una tabla que coincida con el esquema de los datos en el archivo StormEvents.csv. Cuando se ejecuta este código, devuelve un mensaje similar al siguiente: *Para iniciar sesión, use un explorador web para abrir la página https://microsoft.com/devicelogin y escriba el código F3W4VWZDM para autenticarse*. Siga los pasos para iniciar sesión y, a continuación, vuelva a ejecutar el siguiente bloque de código. Los bloques de código subsiguientes que establecen una conexión requieren que vuelva a iniciar sesión.

```python
KUSTO_CLIENT = KustoClient(KCSB_DATA)
CREATE_TABLE_COMMAND = ".create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)"

df_table_create_output = KUSTO_CLIENT.execute_mgmt(KUSTO_DATABASE, CREATE_TABLE_COMMAND).primary_results[0].to_dataframe()

df_table_create_output
```

## <a name="define-ingestion-mapping"></a>Definición de la asignación de ingesta

Asigna los datos de CSV entrantes a los nombres de columna y tipos de datos utilizados al crear la tabla.

```python
CREATE_MAPPING_COMMAND = """.create table StormEvents ingestion csv mapping 'StormEvents_CSV_Mapping' '[{"Name":"StartTime","datatype":"datetime","Ordinal":0}, {"Name":"EndTime","datatype":"datetime","Ordinal":1},{"Name":"EpisodeId","datatype":"int","Ordinal":2},{"Name":"EventId","datatype":"int","Ordinal":3},{"Name":"State","datatype":"string","Ordinal":4},{"Name":"EventType","datatype":"string","Ordinal":5},{"Name":"InjuriesDirect","datatype":"int","Ordinal":6},{"Name":"InjuriesIndirect","datatype":"int","Ordinal":7},{"Name":"DeathsDirect","datatype":"int","Ordinal":8},{"Name":"DeathsIndirect","datatype":"int","Ordinal":9},{"Name":"DamageProperty","datatype":"int","Ordinal":10},{"Name":"DamageCrops","datatype":"int","Ordinal":11},{"Name":"Source","datatype":"string","Ordinal":12},{"Name":"BeginLocation","datatype":"string","Ordinal":13},{"Name":"EndLocation","datatype":"string","Ordinal":14},{"Name":"BeginLat","datatype":"real","Ordinal":16},{"Name":"BeginLon","datatype":"real","Ordinal":17},{"Name":"EndLat","datatype":"real","Ordinal":18},{"Name":"EndLon","datatype":"real","Ordinal":19},{"Name":"EpisodeNarrative","datatype":"string","Ordinal":20},{"Name":"EventNarrative","datatype":"string","Ordinal":21},{"Name":"StormSummary","datatype":"dynamic","Ordinal":22}]'"""

df_mapping_create_output = KUSTO_CLIENT.execute_mgmt(KUSTO_DATABASE, CREATE_MAPPING_COMMAND).primary_results[0].to_dataframe()

df_mapping_create_output
```

## <a name="queue-a-message-for-ingestion"></a>Colocación de un mensaje en cola para la ingesta

Coloca en cola un mensaje para extraer datos desde Blob Storage e introduce esos datos en el Explorador de datos de Azure.

```python
INGESTION_CLIENT = KustoIngestClient(KCSB_INGEST)

# All ingestion properties are documented here: https://docs.microsoft.com/azure/kusto/management/data-ingest#ingestion-properties
INGESTION_PROPERTIES  = IngestionProperties(database=KUSTO_DATABASE, table=DESTINATION_TABLE, dataFormat=DataFormat.csv, mappingReference=DESTINATION_TABLE_COLUMN_MAPPING, additionalProperties={'ignoreFirstRecord': 'true'})
BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)  # 10 is the raw size of the data in bytes
INGESTION_CLIENT.ingest_from_blob(BLOB_DESCRIPTOR,ingestion_properties=INGESTION_PROPERTIES)

print('Done queuing up ingestion with Azure Data Explorer')

```

## <a name="validate-that-data-was-ingested-into-the-table"></a>Validación de la ingesta de los datos en la tabla

Espere entre cinco y diez minutos para que la ingesta en cola programe la ingesta y cargue los datos en el Explorador de datos de Azure. A continuación, ejecute el siguiente código para obtener el recuento de registros en la tabla StormEvents.

```python
QUERY = "StormEvents | count"

df = KUSTO_CLIENT.execute_query(KUSTO_DATABASE, QUERY).primary_results[0].to_dataframe()

df
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

Si tiene previsto seguir nuestros tutoriales y guías de inicio rápido, conserve los recursos que creó. De lo contrario, ejecute el siguiente comando en la base de datos para limpiar la tabla StormEvents.

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Escritura de consultas](write-queries.md)
