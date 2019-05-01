---
title: Copia de datos desde un origen HTTP mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde un origen HTTP —en la nube o en un entorno local— a almacenes de datos receptores compatibles a través de una actividad de copia de una canalización de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: f25b0f2c7b5e3148bae778c4b50a3f0bd0c148da
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64875942"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Copia de datos desde un punto de conexión HTTP mediante Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1](v1/data-factory-http-connector.md)
> * [Versión actual](connector-http.md)

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos desde un punto de conexión HTTP. El artículo se basa en [Actividad de copia en Azure Data Factory](copy-activity-overview.md), en el que se ofrece información general acerca de la actividad de copia.

Las diferencias entre este conector HTTP, el [conector REST](connector-rest.md) y el [conector de tabla web](connector-web-table.md) son:

- El **conector REST** admite específicamente la copia de datos desde API RESTful. 
- El **conector HTTP** es genérico y puede recuperar datos desde cualquier punto de conexión HTTP, por ejemplo, para descargar archivos. Antes de que esté disponible el conector REST, puede usar el conector HTTP para copiar datos de la API RESTful, lo cual se admite, pero es menos funcional en comparación con el conector REST.
- El **conector de tabla web** extrae contenido de la tabla de una página web HTML.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde un origen HTTP a cualquier almacén de datos receptor compatible. Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).

Puede usar este conector HTTP para:

- Recuperar datos desde un punto de conexión HTTP o HTTPS mediante los métodos HTTP **GET** o **POST**.
- Recuperar datos con uno de los siguientes tipos de autenticación: **Anónima**, **Básica**, **Implícita**, **Windows** o **ClientCertificate**.
- Copiar la respuesta HTTP tal cual, o bien analizarla con los [códecs de compresión y los formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md).

> [!TIP]
> Para probar una solicitud HTTP para la recuperación de datos antes de configurar el conector HTTP en Data Factory, obtenga información acerca de la especificación de API para los requisitos del cuerpo y del encabezado. Puede usar herramientas como Postman o un explorador web para validar.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que puede usar para definir entidades de Data Factory específicas del conector HTTP.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de HTTP:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** debe establecerse en: **HttpServer**. | Sí |
| url | La dirección URL base para el servidor web. | Sí |
| enableServerCertificateValidation | Especifique si desea habilitar la validación de certificados SSL al conectarse al punto de conexión HTTP. Si el servidor HTTPS usa un certificado autofirmado, establezca esta propiedad en **false**. | Sin <br /> (El valor predeterminado es: **true**) |
| authenticationType | Especifica el tipo de autenticación. Los valores permitidos son: **Anonymous**, **Basic**, **Digest**, **Windows** y **ClientCertificate**. <br><br> Consulte las secciones que se encuentran después de esta tabla para obtener más propiedades y ejemplos de JSON para estos tipos de autenticación. | Sí |
| connectVia | Instancia de [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Se puede usar Azure Integration Runtime o un IR autohospedado (si el almacén de datos se encuentra en una red privada). Si no se especifica, esta propiedad se usará Azure Integration Runtime. |Sin  |

### <a name="using-basic-digest-or-windows-authentication"></a>Uso de la autenticación Basic, Digest o Windows

Establezca la propiedad **authenticationType** en **Basic**, **Digest** o **Windows**. Además de las propiedades genéricas descritas en las secciones anteriores, especifique las siguientes:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| userName | El nombre de usuario para acceder al punto de conexión HTTP. | Sí |
| password | Contraseña del usuario (valor **userName**). Marque este campo como de tipo **SecureString** para almacenarlo de forma segura en Data Factory. También puede [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |

**Ejemplo**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<user name>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>Uso de la autenticación ClientCertificate

Para usar la autenticación ClientCertificate, establezca la propiedad **authenticationType** en **ClientCertificate**. Además de las propiedades genéricas descritas en las secciones anteriores, especifique las siguientes:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| embeddedCertData | Datos del certificado con codificación Base64. | Especifique **embeddedCertData** o **certThumbprint**. |
| certThumbprint | La huella digital del certificado que se instaló en el almacén de certificados de la máquina Integration Runtime (autohospedado). Solo se aplica cuando se especifica el tipo autohospedado de un entorno Integration Runtime en la propiedad **connectVia**. | Especifique **embeddedCertData** o **certThumbprint**. |
| password | La contraseña asociada con el certificado. Marque este campo como de tipo **SecureString** para almacenarlo de forma segura en Data Factory. También puede [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sin  |

Si utiliza **certThumbprint** para la autenticación y el certificado está instalado en el almacén personal del equipo local, conceda permisos de lectura al entorno Integration Runtime (autohospedado):

1. Abra Microsoft Management Console (MMC). Agregue el complemento **Certificados** que tenga como destino **Equipo local**.
2. Expanda **Certificados** > **Personal** y, luego, seleccione **Certificados**.
3. Haga clic con el botón derecho en el certificado del almacén personal y seleccione **Todas las tareas** > **Administrar claves privadas**.
3. En la pestaña **Seguridad**, agregue la cuenta de usuario en que se ejecuta el servicio de host del entorno Integration Runtime (DIAHostService) con acceso de lectura al certificado.

**Ejemplo 1: Uso de certThumbprint**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo 2: Uso de embeddedCertData**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<Base64-encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). 

- Para **Parquet y formato de texto delimitado**, consulte [conjunto de datos con formato Parquet y texto delimitado](#parquet-and-delimited-text-format-dataset) sección.
- Para otros formatos como **formato ORC/Avro/JSON/binario**, consulte [otro conjunto de datos de formato](#other-format-dataset) sección.

### <a name="parquet-and-delimited-text-format-dataset"></a>Conjunto de datos con formato parquet y texto delimitado

Para copiar datos desde HTTP en **Parquet o formato de texto delimitado**, consulte [formato Parquet](format-parquet.md) y [formato de texto delimitado](format-delimited-text.md) artículo en el conjunto de datos de formato y compatibles Configuración. Las siguientes propiedades son compatibles con HTTP en `location` configuración en el conjunto de datos basado en el formato:

| Propiedad    | DESCRIPCIÓN                                                  | Obligatorio |
| ----------- | ------------------------------------------------------------ | -------- |
| Tipo        | La propiedad type en `location` en el conjunto de datos debe establecerse en **HttpServerLocation**. | Sí      |
| relativeUrl | Dirección URL relativa al recurso que contiene los datos.       | Sin        |

> [!NOTE]
> El tamaño de carga de la solicitud HTTP admitido es aproximadamente 500 KB. Si el que desea pasar al punto de conexión web supera los 500 KB, considere la posibilidad de agrupar la carga en fragmentos menores.

> [!NOTE]
> **HttpFile** todavía se admite el tipo de conjunto de datos con formato Parquet/texto que se mencionan en la siguiente sección como-actividad de copia y búsqueda para la compatibilidad con versiones anteriores de. Se sugieren para usar este nuevo modelo a partir de ahora, y ha cambiado la interfaz de usuario de creación de ADF para generar estos nuevos tipos.

**Ejemplo:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HttpServerLocation",
                "relativeUrl": "<relative url>"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Otro conjunto de datos de formato

Para copiar datos desde HTTP en **formato ORC/Avro/JSON/binario**, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del conjunto de datos debe establecerse en **HttpFile**. | Sí |
| relativeUrl | Dirección URL relativa al recurso que contiene los datos. Cuando no se especifica la propiedad, solo se usa la dirección URL especificada en la definición del servicio vinculado. | Sin  |
| requestMethod | Método HTTP. Los valores permitidos son **Get** (valor predeterminado) y **Post**. | Sin  |
| additionalHeaders | Encabezados de solicitud HTTP adicionales. | Sin  |
| requestBody | Cuerpo de la solicitud HTTP. | Sin  |
| formato | Si desea recuperar datos tal cual desde el punto de conexión HTTP sin analizarlos y, después, copiarlos en un almacén basado en archivos, ignore la sección de **formato** de las definiciones de los conjuntos de datos de entrada y salida.<br/><br/>Si desea analizar el contenido de la respuesta HTTP durante la copia, se admiten los siguientes tipos de formato de archivos: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** y **ParquetFormat**. En **formato**, establezca la propiedad **type** de formato en uno de los siguientes valores. Para obtener más información, consulte las secciones sobre [formato JSON](supported-file-formats-and-compression-codecs.md#json-format), [formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [formato Orc](supported-file-formats-and-compression-codecs.md#orc-format) y [formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |Sin  |
| compresión | Especifique el tipo y el nivel de compresión de los datos. Para más información, consulte el artículo sobre [códecs de compresión y formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md#compression-support).<br/><br/>Tipos admitidos: **GZip**, **Deflate**, **BZip2** y **ZipDeflate**.<br/>Niveles que se admiten:  **Optimal** y **Fastest**. |Sin  |

> [!NOTE]
> El tamaño de carga de la solicitud HTTP admitido es aproximadamente 500 KB. Si el que desea pasar al punto de conexión web supera los 500 KB, considere la posibilidad de agrupar la carga en fragmentos menores.

**Ejemplo 1: Uso del método Get (valor predeterminado)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Ejemplo 2: Uso del método POST**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

En esta sección se proporciona una lista de las propiedades que admite el origen de HTTP.

Para ver una lista completa de las secciones y propiedades que hay disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md). 

### <a name="http-as-source"></a>HTTP como origen

- Para copiar desde **Parquet y formato de texto delimitado**, consulte [Parquet y origen del formato de texto delimitado](#parquet-and-delimited-text-format-source) sección.
- Para copiar desde otros formatos como **formato ORC/Avro/JSON/binario**, consulte [otro origen de formato](#other-format-source) sección.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet y origen del formato de texto delimitado

Para copiar datos desde HTTP en **Parquet o formato de texto delimitado**, consulte [formato Parquet](format-parquet.md) y [formato de texto delimitado](format-delimited-text.md) artículo sobre el origen de la actividad de copia basada en el formato y configuraciones admitidas. Las siguientes propiedades son compatibles con HTTP en `storeSettings` configuración en el origen de copia basada en el formato:

| Propiedad                 | DESCRIPCIÓN                                                  | Obligatorio |
| ------------------------ | ------------------------------------------------------------ | -------- |
| Tipo                     | La propiedad type en `storeSettings` debe establecerse en **HttpReadSetting**. | Sí      |
| requestMethod            | Método HTTP. <br>Los valores permitidos son **Get** (valor predeterminado) y **Post**. | Sin        |
| addtionalHeaders         | Encabezados de solicitud HTTP adicionales.                             | Sin        |
| requestBody              | Cuerpo de la solicitud HTTP.                               | Sin        |
| requestTimeout           | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para leer los datos de la respuesta. El valor predeterminado es **00:01:40**. | Sin        |
| maxConcurrentConnections | El número de conexiones para conectarse al almacén de almacenamiento al mismo tiempo. Especifique solo cuando desee limitar las conexiones simultáneas al almacén de datos. | Sin        |

> [!NOTE]
> Para Parquet/texto delimitado con **HttpSource** todavía se admite como origen de la actividad de copia tipo mencionado en la siguiente sección: es para la compatibilidad con versiones anteriores. Se sugieren para usar este nuevo modelo a partir de ahora, y ha cambiado la interfaz de usuario de creación de ADF para generar estos nuevos tipos.

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "HttpReadSetting",
                    "requestMethod": "Post",
                    "additionalHeaders": "<header key: header value>\n<header key: header value>\n",
                    "requestBody": "<body for POST HTTP request>"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Otro origen de formato

Para copiar datos desde HTTP en **formato ORC/Avro/JSON/binario**, se admiten las siguientes propiedades en la actividad de copia **origen** sección:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del origen de la actividad de copia debe establecerse en: **HttpSource**. | Sí |
| httpRequestTimeout | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para leer los datos de la respuesta. El valor predeterminado es **00:01:40**.  | Sin  |

**Ejemplo**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Pasos siguientes

Para ver una lista de los almacenes de datos que la actividad de copia admite como orígenes y receptores en Azure Data Factory, consulte los [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
