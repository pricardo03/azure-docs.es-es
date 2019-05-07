---
title: Formato parquet en Azure Data Factory | Microsoft Docs
description: Este tema describe cómo tratar con formato Parquet en Azure Data Factory.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 360b794f0d8ba9c145a92f015f264eb624fbb0f1
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65144878"
---
# <a name="parquet-format-in-azure-data-factory"></a>Formato parquet en Azure Data Factory

Siga este artículo cuando desee **analizar los archivos Parquet o escribir los datos en formato Parquet**. 

Formato parquet es compatible con los conectores siguientes: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [Sistema de archivos](connector-file-system.md), [FTP](connector-ftp.md), [almacenamiento en nube de Google](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md)y [ SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Parquet.

| Propiedad         | DESCRIPCIÓN                                                  | Obligatorio |
| ---------------- | ------------------------------------------------------------ | -------- |
| Tipo             | La propiedad type del conjunto de datos debe establecerse en **Parquet**. | Sí      |
| location         | Configuración de ubicación de los archivos. Cada conector basados en archivos tiene su propio tipo de ubicación y propiedades en compatibles `location`. **Ver detalles de artículo del conector -> sección de propiedades del conjunto de datos**. | Sí      |
| compressionCodec | El códec de compresión que se usará al escribir en archivos Parquet. Al leer desde archivos Parquet, Data Factory determina automáticamente el códec de compresión basado en los metadatos del archivo.<br>Tipos admitidos son "**ninguno**","**gzip**","**snappy**" (predeterminado), y "**lzo**". Tenga en cuenta actualmente no admite la actividad de copia LZO. | Sin        |

> [!NOTE]
> Espacio en blanco en el nombre de columna no se admite para archivos Parquet.

A continuación es un ejemplo de conjunto de datos de Parquet en Azure Blob Storage:

```json
{
    "name": "ParquetDataset",
    "properties": {
        "type": "Parquet",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de propiedades admitidas por el receptor y el origen de Parquet.

### <a name="parquet-as-source"></a>Parquet, como origen

Se admiten las siguientes propiedades en la actividad de copia ***\*origen\**** sección.

| Propiedad      | DESCRIPCIÓN                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| Tipo          | La propiedad type del origen de la actividad de copia debe establecerse en **ParquetSource**. | Sí      |
| storeSettings | Un grupo de propiedades sobre cómo leer datos desde un almacén de datos. Cada conector basados en archivos tiene su propia configuración leída admitidos en `storeSettings`. **Ver detalles de artículo del conector -> sección de propiedades de actividad de copia**. | Sin        |

### <a name="parquet-as-sink"></a>Parquet como receptor

Se admiten las siguientes propiedades en la actividad de copia ***\*receptor\**** sección.

| Propiedad      | DESCRIPCIÓN                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| Tipo          | La propiedad type del origen de la actividad de copia debe establecerse en **ParquetSink**. | Sí      |
| storeSettings | Un grupo de propiedades sobre cómo escribir datos en un almacén de datos. Cada conector basados en archivos tiene su propia configuración de escritura compatible en `storeSettings`. **Ver detalles de artículo del conector -> sección de propiedades de actividad de copia**. | Sin        |

## <a name="mapping-data-flow-properties"></a>Propiedades de flujo de datos de asignación

Obtenga información detallada de [transformación del origen](data-flow-source.md) y [receptor transformación](data-flow-sink.md) en asignación de flujo de datos.

## <a name="data-type-support"></a>Compatibilidad con los tipos de datos

Parquet, tipos de datos complejos no están actualmente admite (por ejemplo, mapa, lista, STRUCT).

## <a name="using-self-hosted-integration-runtime"></a>Uso de Integration Runtime autohospedado

> [!IMPORTANT]
> En el caso de las copias autorizadas por el entorno de ejecución de integración (IR) autohospedado (por ejemplo, entre almacenes de datos locales y almacenes de datos en la nube), si no va a copiar archivos Parquet **tal y como están**, tendrá que instalar **JRE (Java Runtime Environment) 8 de 64 bits u OpenJDK** en la máquina de IR. Consulte el párrafo siguiente para más información.

En el caso de las copias que se ejecutan en el IR autohospedado con la serialización o deserialización de archivos Parquet, para encontrar el entorno de ejecución de Java, ADF consulta primero el Registro *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* de JRE; si no lo encuentra, comprueba la variable del sistema *`JAVA_HOME`* de OpenJDK.

- **Para usar JRE**: el IR de 64 bits necesita JRE de 64 bits. Puede encontrarlo [aquí](https://go.microsoft.com/fwlink/?LinkId=808605).
- **Para usar OpenJDK**: se admite desde la versión 3.13 de IR. Empaquete jvm.dll con todos los demás ensamblados de OpenJDK necesarios en la máquina del IR autohospedado y establezca la variable de entorno del sistema JAVA_HOME en el valor que corresponda.

> [!TIP]
> Si copia datos desde o hacia Parquet mediante Integration Runtime autohospedado y recibe un error que indica que "Se produjo un error al invocar Java, mensaje: **Espacio en el montón java.lang.OutOfMemoryError:Java**", puede agregar una variable de entorno `_JAVA_OPTIONS` en la máquina que hospeda IR autohospedado para ajustar el tamaño del montón mínimo y máximo para JVM a fin de facilitar dicha copia y, a continuación, volver a ejecutar la canalización.

![Establecimiento del tamaño del montón JVM en IR autohospedado](C:/AzureContent/azure-docs-pr/articles/data-factory/media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Ejemplo: establecimiento de la variable `_JAVA_OPTIONS` con el valor `-Xms256m -Xmx16g`. La marca `Xms` especifica el grupo de asignación de memoria inicial para una máquina virtual Java (JVM), mientras que `Xmx` especifica el grupo de asignación de memoria máxima. Esto significa que JVM se iniciará con la cantidad de memoria `Xms` y podrá utilizar `Xmx` como máximo. De forma predeterminada, ADF usa 64 MB como mínimo y 1 GB como máximo.

## <a name="next-steps"></a>Pasos siguientes

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Asignación de flujo de datos](concepts-data-flow-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)