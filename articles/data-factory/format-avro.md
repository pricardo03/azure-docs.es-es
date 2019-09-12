---
title: Formato Avro en Azure Data Factory | Microsoft Docs
description: En este tema se describe cómo tratar con el formato Avro en Azure Data Factory.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: jingwang
ms.openlocfilehash: c1f55da2f1aada8c7e4a0d979b2e38e6782e564c
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70293774"
---
# <a name="avro-format-in-azure-data-factory"></a>Formato Avro en Azure Data Factory

Siga este artículo cuando quiera **analizar los archivos Avro o escribir los datos en formato Avro**. 

El formato Avro se admite para los conectores siguientes: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [File System](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md) y [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Avro.

| Propiedad         | DESCRIPCIÓN                                                  | Obligatorio |
| ---------------- | ------------------------------------------------------------ | -------- |
| Tipo             | La propiedad type del conjunto de datos debe establecerse en **Avro**. | Sí      |
| location         | Configuración de ubicación de los archivos. Cada conector basado en archivos tiene su propio tipo de ubicación y propiedades compatibles en `location`. **Vea los detalles en el artículo de conectores -> sección de propiedades del conjunto de datos**. | Sí      |
| avroCompressionCodec | El códec de compresión que se usará al escribir en archivos Avro. Al leer desde archivos Avro, Data Factory determina automáticamente el códec de compresión según los metadatos del archivo.<br>Los tipos admitidos son "**none**" (valor predeterminado), "**deflate**", "**snappy**". | Sin       |

> [!NOTE]
> No se admiten espacios en blanco en el nombre de columna de los archivos Avro.

A continuación se muestra un ejemplo de un conjunto de datos de Avro en Azure Blob Storage:

```json
{
    "name": "AvroDataset",
    "properties": {
        "type": "Avro",
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
            "avroCompressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admiten el receptor y el origen de Avro.

### <a name="avro-as-source"></a>Avro como origen

En la sección ***\*source\**** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad      | DESCRIPCIÓN                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| Tipo          | La propiedad type del origen de la actividad de copia debe establecerse en **AvroSource**. | Sí      |
| storeSettings | Un grupo de propiedades sobre cómo leer datos de un almacén de datos. Cada conector basado en archivos tiene su propia configuración de lectura admitida en `storeSettings`. **Vea los detalles en el artículo de conectores -> sección de propiedades de la actividad de copia**. | Sin       |

### <a name="avro-as-sink"></a>Avro como receptor

En la sección ***\*sink\**** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad      | DESCRIPCIÓN                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| Tipo          | La propiedad type del origen de la actividad de copia debe establecerse en **AvroSink**. | Sí      |
| storeSettings | Un grupo de propiedades sobre cómo escribir datos en un almacén de datos. Cada conector basado en archivos tiene su propia configuración de escritura admitida en `storeSettings`. **Vea los detalles en el artículo de conectores -> sección de propiedades de la actividad de copia**. | Sin       |

## <a name="data-type-support"></a>Compatibilidad con los tipos de datos

No se admiten [tipos de datos complejos](https://avro.apache.org/docs/current/spec.html#schema_complex) de Avro (registros, enumeraciones, matrices, asignaciones, uniones y fijos).

## <a name="next-steps"></a>Pasos siguientes

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
