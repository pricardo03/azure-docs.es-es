---
title: Formato binario en Azure Data Factory
description: En este tema se describe cómo tratar con el formato binario en Azure Data Factory.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: jingwang
ms.openlocfilehash: 82f7c380c66dc6b42f4ca5c67c13524428c78221
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73674810"
---
# <a name="binary-format-in-azure-data-factory"></a>Formato binario en Azure Data Factory

El formato binario se admite para los conectores siguientes: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [File System](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md) y [SFTP](connector-sftp.md).

Puede utilizar un conjunto de datos binarios en la [actividad de copia](copy-activity-overview.md), la [actividad GetMetadata](control-flow-get-metadata-activity.md) o la [actividad de eliminación](delete-activity.md). Cuando se usa un conjunto de datos binario, ADF no analiza el contenido del archivo, sino que lo trata tal cual. 

>[!NOTE]
>Al utilizar un conjunto de archivos binario en la actividad de copia, solo puede copiar de un conjunto de datos binario a un conjunto de datos binario.

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos binarios.

| Propiedad         | DESCRIPCIÓN                                                  | Obligatorio |
| ---------------- | ------------------------------------------------------------ | -------- |
| Tipo             | La propiedad type del conjunto de datos debe establecerse en **binario**. | Sí      |
| location         | Configuración de ubicación de los archivos. Cada conector basado en archivos tiene su propio tipo de ubicación y propiedades compatibles en `location`. **Vea los detalles en el artículo de conectores -> sección de propiedades del conjunto de datos**. | Sí      |
| compression | Grupo de propiedades para configurar la compresión de archivo. Configure esta sección si desea realizar la compresión o descompresión durante la ejecución de la actividad. | Sin |
| Tipo | El códec de compresión usado para leer y escribir archivos binarios. <br>Los valores permitidos son **bzip2**, **gzip**, **deflate** y **ZipDeflate**. para usar al guardar el archivo. | Sin       |
| level | La razón de compresión. Se aplica cuando se utiliza el conjunto de datos en el receptor de la actividad de copia.<br>Los valores permitidos son **Optimal** o **Fastest**.<br>- **Fastest:** la operación de compresión debe completarse tan pronto como sea posible, incluso si el archivo resultante no se comprime de forma óptima.<br>- **Optimal**: la operación de compresión se debe comprimir óptimamente, incluso si tarda más tiempo en completarse. Para más información, consulte el tema [Nivel de compresión](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) . | Sin       |

A continuación se muestra un ejemplo de un conjunto de datos binarios en Azure Blob Storage:

```json
{
    "name": "BinaryDataset",
    "properties": {
        "type": "Binary",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "ZipDeflate"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admiten el receptor y el origen binarios.

>[!NOTE]
>Al utilizar un conjunto de archivos binario en la actividad de copia, solo puede copiar de un conjunto de datos binario a un conjunto de datos binario.

### <a name="binary-as-source"></a>Binario como origen

En la sección ***\*source\**** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad      | DESCRIPCIÓN                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| Tipo          | La propiedad type del origen de la actividad de copia debe establecerse en **BinarySource**. | Sí      |
| storeSettings | Un grupo de propiedades sobre cómo leer datos de un almacén de datos. Cada conector basado en archivos tiene su propia configuración de lectura admitida en `storeSettings`. **Vea los detalles en el artículo de conectores -> sección de propiedades de la actividad de copia**. | Sin       |

### <a name="binary-as-sink"></a>Binario como receptor

En la sección ***\*sink\**** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad      | DESCRIPCIÓN                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| Tipo          | La propiedad type del origen de la actividad de copia debe establecerse en: **BinarySink**. | Sí      |
| storeSettings | Un grupo de propiedades sobre cómo escribir datos en un almacén de datos. Cada conector basado en archivos tiene su propia configuración de escritura admitida en `storeSettings`. **Vea los detalles en el artículo de conectores -> sección de propiedades de la actividad de copia**. | Sin       |

## <a name="next-steps"></a>Pasos siguientes

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
- [Actividad de eliminación](delete-activity.md)
