---
title: Uso de Azure Data Lake Storage (ADLS) Gen2 con Apache Hadoop en Azure HDInsight
description: Proporciona información general de Azure Data Lake Storage Gen2 y cómo funciona con Azure HDInsight.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/21/2018
ms.openlocfilehash: 2ae11afe1ecbe500a4851aab6d56e612fbe79ee6
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52976749"
---
# <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Uso de Azure Data Lake Storage Gen2 con Apache Hadoop en Azure HDInsight

Azure Data Lake Storage (ADLS) Gen2 adopta las características principales de Azure Data Lake Storage Gen1, como un sistema de archivos compatible con Hadoop, Azure Active Directory y listas de control de acceso basadas en POSIX, y las integra en Azure Blob Storage. Esta combinación permite aprovechar las ventajas de rendimiento de Azure Data Lake Storage Gen1, al mismo tiempo que se usan la administración del ciclo de vida de los datos y las capas de Blob Storage.

## <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Funcionalidad básica de Azure Data Lake Storage Gen2

- Acceso compatible con Hadoop: Azure Data Lake Storage Gen2 permite administrar datos y acceder a ellos igual que lo haría con un sistema de archivos distribuido de Hadoop (HDFS). El controlador ABFS está disponible en todos los entornos de Apache Hadoop, incluidos Azure HDInsight y Azure Databricks, para obtener acceso a los datos almacenados en Data Lake Storage Gen2.

- Superconjunto de permisos POSIX: el modelo de seguridad de Data Lake Gen2 es compatible con los permisos de ACL y POSIX junto con granularidad adicional específica de Data Lake Storage Gen2. Se puede realizar la configuración mediante herramientas de administración o marcos, como Apache Hive y Apache Spark.

- Rentabilidad: Data Lake Storage Gen2 ofrece transacciones y capacidad de almacenamiento de bajo costo. Características como el ciclo de vida de Azure Blob Storage ayudan a reducir los costos mediante el ajuste de las tarifas de facturación a medida que los datos avanzan en su ciclo de vida.

- Funciona con aplicaciones, marcos y herramientas de Blob Storage: Data Lake Storage Gen2 sigue funcionando con una amplia gama de herramientas, marcos y aplicaciones que existen actualmente para Blob Storage.

- Controlador optimizado: el controlador ABFS está optimizado específicamente para el análisis de macrodatos. Las API de REST correspondientes se exponen a través del punto de conexión dfs, dfs.core.windows.net.

## <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Novedades de Azure Data Lake Storage Gen2

### <a name="managed-identities-for-secure-file-access"></a>Identidades administradas para proteger el acceso a archivos

Azure HDInsight usa identidades administradas para proteger el acceso del clúster a los archivos de Azure Data Lake Storage Gen2. Las identidades administradas son una característica de Azure Active Directory que proporciona servicios de Azure con un conjunto de credenciales administradas automáticamente. Estas credenciales se pueden usar para autenticarse en cualquier servicio que admita la autenticación de Active Directory. El uso de identidades administradas no requiere almacenar las credenciales en archivos de código o de configuración.

Para obtener más información, vea [What is managed identities for Azure resources?](../active-directory/managed-identities-azure-resources/overview.md) (¿Qué son las identidades administradas de Azure?).

### <a name="azure-blob-filesystem-abfs-driver"></a>Controlador del sistema de archivos de Azure Blob (ABFS)

Las aplicaciones de Apache Hadoop de forma nativa esperan leer y escribir datos desde el almacenamiento en disco local. Un controlador del sistema de archivos de Hadoop como ABFS permite que las aplicaciones de Hadoop funcionen con el almacenamiento en la nube mediante la emulación de las operaciones habituales del sistema de archivos de Hadoop en la aplicación. El controlador convierte después dichas operaciones en operaciones que la plataforma real de almacenamiento en la nube reconoce.

Anteriormente, el controlador del sistema de archivos de Hadoop podría convertir todas las operaciones del sistema de archivos en llamadas API de REST de Azure Storage en el lado cliente y, a continuación, invocar la API de REST. Sin embargo, esta conversión del lado cliente resultaba en varias llamadas API de REST para una única operación del sistema de archivos, como cambiar el nombre a un archivo. ABFS movió parte de la lógica del sistema de archivos de Hadoop del lado cliente al lado servidor y la API de Azure Data Lake Storage Gen2 ahora se ejecuta en paralelo con la API de Blob. Esta migración mejora el rendimiento porque ahora se pueden ejecutar operaciones comunes del sistema de archivos de Hadoop con una llamada API de REST.

Para más información, vea [Controlador Azure Blob FileSystem (ABFS): un controlador de Azure Storage dedicado para Hadoop](../storage/data-lake-storage/abfs-driver.md).

### <a name="adls-gen-2-uri-scheme"></a>Esquema del identificador URI de ADLS Gen 2

ADLS Gen2 usa un nuevo esquema de identificador URI para acceder a los archivos de Azure Storage desde HDInsight:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

El esquema de URI proporciona acceso cifrado SSL (con el prefijo `abfss://`) y acceso sin cifrar (con el prefijo `abfs://`). Se recomienda usar `abfss` siempre que sea posible, incluso cuando se acceda a datos que se encuentren en la misma región de Azure.

`<FILE_SYSTEM_NAME>` identifica la ruta de acceso del sistema de archivos de Data Lake Storage Gen2.

`<ACCOUNT_NAME>` identifica el nombre de la cuenta de Azure Storage. Se necesita el nombre completo de dominio (FQDN).

`<PATH>` es el nombre de la ruta HDFS del archivo o el directorio.

Si no se especifican los valores de `<FILE_SYSTEM_NAME>` y `<ACCOUNT_NAME>`, se utiliza el sistema de archivos predeterminado. Para los archivos del sistema de archivos predeterminado, puede usar una ruta relativa o absoluta. Por ejemplo, se puede hacer referencia al archivo `hadoop-mapreduce-examples.jar` que se incluye con los clústeres de HDInsight mediante alguna de las rutas de acceso siguientes:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> El nombre del archivo es `hadoop-examples.jar` en las versiones 2.1 y 1.6 de los clústeres de HDInsight. Al trabajar con archivos fuera de HDInsight, la mayoría de las utilidades no reconocen el formato ABFS y, en su lugar, esperan un formato básico de ruta de acceso, como `example/jars/hadoop-mapreduce-examples.jar`.

Para más información, vea [Uso del URI de Azure Data Lake Storage Gen2](../storage/data-lake-storage/introduction-abfs-uri.md).

## <a name="next-steps"></a>Pasos siguientes
- [Introducción a Azure Data Lake Storage Gen2](../storage/data-lake-storage/introduction.md)
- [Uso de Azure Data Lake Storage Gen2 (versión preliminar) con clústeres de Azure HDInsight](../storage/data-lake-storage/use-hdi-cluster.md)