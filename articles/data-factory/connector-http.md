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
ms.date: 09/09/2019
ms.author: jingwang
ms.openlocfilehash: 880f5624af03e08e3a91ec5b230e593025d979a5
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2019
ms.locfileid: "70812999"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Copia de datos desde un punto de conexión HTTP mediante Azure Data Factory

> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
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

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Primeros pasos

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que puede usar para definir entidades de Data Factory específicas del conector HTTP.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de HTTP:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** debe establecerse en: **HttpServer**. | Sí |
| url | La dirección URL base para el servidor web. | Sí |
| enableServerCertificateValidation | Especifique si desea habilitar la validación de certificados SSL al conectarse al punto de conexión HTTP. Si el servidor HTTPS usa un certificado autofirmado, establezca esta propiedad en **false**. | Sin<br /> (El valor predeterminado es: **true**) |
| authenticationType | Especifica el tipo de autenticación. Los valores permitidos son: **Anonymous**, **Basic**, **Digest**, **Windows** y **ClientCertificate**. <br><br> Consulte las secciones que se encuentran después de esta tabla para obtener más propiedades y ejemplos de JSON para estos tipos de autenticación. | Sí |
| connectVia | Instancia de [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Obtenga más información en la sección [Requisitos previos](#prerequisites). Si no se especifica, se usa el valor predeterminado de Azure Integration Runtime. |Sin |

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
| password | La contraseña asociada con el certificado. Marque este campo como de tipo **SecureString** para almacenarlo de forma segura en Data Factory. También puede [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sin |

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

- Para información sobre el **formato binario, de texto delimitado, JSON, Parquet y Avro**, consulte la sección [Conjunto de datos de formato binario, de texto delimitado, JSON, Parquet y Avro](#format-based-dataset).
- En el caso de otros formatos como **ORC**, consulte la sección [Otro conjunto de datos de formato](#other-format-dataset).

### <a name="format-based-dataset"></a> Conjunto de datos de formato binario, de texto delimitado, JSON, Parquet y Avro

Para copiar datos desde y hacia el **formato binario, de texto delimitado, JSON, Parquet y Avro**, consulte los artículos [Formato Parquet](format-parquet.md), [Formato de texto delimitado](format-delimited-text.md), [Formato Avro](format-avro.md) y [Formato binario](format-binary.md) sobre conjuntos de datos basados en el formato y configuraciones admitidas. Las propiedades siguientes se admiten para HTTP en la configuración `location` del conjunto de datos basado en formato:

| Propiedad    | DESCRIPCIÓN                                                  | Obligatorio |
| ----------- | ------------------------------------------------------------ | -------- |
| type        | La propiedad type de `location` en el conjunto de datos se debe establecer en **HttpServerLocation**. | Sí      |
| relativeUrl | Dirección URL relativa al recurso que contiene los datos.       | Sin       |

> [!NOTE]
> El tamaño de carga de la solicitud HTTP admitido es aproximadamente 500 KB. Si el que desea pasar al punto de conexión web supera los 500 KB, considere la posibilidad de agrupar la carga en fragmentos menores.

> [!NOTE]
> El conjunto de datos de tipo **HttpFile** con formato Parquet o Texto que se menciona en la sección siguiente todavía se admite tal cual en las actividades de copia o búsqueda para compatibilidad con versiones anteriores. A partir de ahora se sugiere usar este modelo nuevo, y la interfaz de usuario de creación de ADF ha cambiado para generar estos nuevos tipos.

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

Para copiar datos de HTTP en **formato ORC**, se admiten las propiedades siguientes:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del conjunto de datos debe establecerse en **HttpFile**. | Sí |
| relativeUrl | Dirección URL relativa al recurso que contiene los datos. Cuando no se especifica la propiedad, solo se usa la dirección URL especificada en la definición del servicio vinculado. | Sin |
| requestMethod | Método HTTP. Los valores permitidos son **Get** (valor predeterminado) y **Post**. | Sin |
| additionalHeaders | Encabezados de solicitud HTTP adicionales. | Sin |
| requestBody | Cuerpo de la solicitud HTTP. | Sin |
| format | Si desea recuperar datos tal cual desde el punto de conexión HTTP sin analizarlos y, después, copiarlos en un almacén basado en archivos, ignore la sección de **formato** de las definiciones de los conjuntos de datos de entrada y salida.<br/><br/>Si desea analizar el contenido de la respuesta HTTP durante la copia, se admiten los siguientes tipos de formato de archivos: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** y **ParquetFormat**. En **formato**, establezca la propiedad **type** de formato en uno de los siguientes valores. Para obtener más información, consulte las secciones sobre [formato JSON](supported-file-formats-and-compression-codecs.md#json-format), [formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [formato Orc](supported-file-formats-and-compression-codecs.md#orc-format) y [formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |Sin |
| compression | Especifique el tipo y el nivel de compresión de los datos. Para más información, consulte el artículo sobre [códecs de compresión y formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md#compression-support).<br/><br/>Tipos admitidos: **GZip**, **Deflate**, **BZip2** y **ZipDeflate**.<br/>Niveles que se admiten:  **Optimal** y **Fastest**. |Sin |

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

- Para copiar desde el **formato binario, de texto delimitado, JSON, Parquet y Avro**, consulte la sección [Origen de formato binario, de texto delimitado, JSON, Parquet y Avro](#format-based-source).
- Para copiar desde otros formatos como **ORC**, consulte la sección [Otro origen de formato](#other-format-source).

#### <a name="format-based-source"></a> Origen de formato binario, de texto delimitado, JSON, Parquet y Avro

Para copiar datos desde el **formato binario, de texto delimitado, JSON, Parquet y Avro**, consulte los artículos [Formato Parquet](format-parquet.md), [Formato de texto delimitado](format-delimited-text.md), [Formato Avro](format-avro.md) y [Formato binario](format-binary.md) sobre el origen de la actividad de copia basada en el formato y las configuraciones admitidas. Las propiedades siguientes se admiten para HTTP en la configuración `storeSettings` del origen de copia basado en formato:

| Propiedad                 | DESCRIPCIÓN                                                  | Obligatorio |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | La propiedad type de `storeSettings` se debe establecer en **HttpReadSetting**. | Sí      |
| requestMethod            | Método HTTP. <br>Los valores permitidos son **Get** (valor predeterminado) y **Post**. | Sin       |
| addtionalHeaders         | Encabezados de solicitud HTTP adicionales.                             | Sin       |
| requestBody              | Cuerpo de la solicitud HTTP.                               | Sin       |
| requestTimeout           | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para leer los datos de la respuesta. El valor predeterminado es **00:01:40**. | Sin       |
| maxConcurrentConnections | Número de conexiones para conectarse al almacén de almacenamiento de forma simultánea. Solo se especifica cuando se quiere limitar la conexión simultánea al almacén de datos. | Sin       |

> [!NOTE]
> Para el formato de texto delimitado o Parquet, todavía se admite tal cual el origen de actividad de copia de tipo **HttpSource** mencionado en la sección siguiente para la compatibilidad con versiones anteriores. A partir de ahora se sugiere usar este modelo nuevo, y la interfaz de usuario de creación de ADF ha cambiado para generar estos nuevos tipos.

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

Para copiar datos de HTTP en **formato ORC**, se admiten las propiedades siguientes en la sección **origen** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del origen de la actividad de copia debe establecerse en: **HttpSource**. | Sí |
| httpRequestTimeout | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para leer los datos de la respuesta. El valor predeterminado es **00:01:40**.  | Sin |

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
