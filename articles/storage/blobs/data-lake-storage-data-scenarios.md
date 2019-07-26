---
title: Escenarios de datos relacionados con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Descripción de los diferentes escenarios y herramientas mediante los cuales los datos se pueden ingerir, procesar, descargar y visualizar en Data Lake Storage Gen2 (anteriormente conocido como Azure Data Lake Store).
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: normesta
ms.openlocfilehash: 010b7bc38caf83c12dd0d8b8e731fdbad6e45256
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422869"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Uso de Azure Data Lake Storage Gen2 para requisitos de macrodatos

Hay cuatro fases principales en el procesamiento de macrodatos:

> [!div class="checklist"]
> * Introducción de grandes cantidades de datos en un almacén de datos, en tiempo real o por lotes
> * Procesamiento de los datos
> * Descarga de los datos
> * Visualización de los datos

Empiece por crear una cuenta de almacenamiento y un sistema de archivos. Después, conceda acceso a los datos. Las primeras secciones de este artículo le ayudan a realizar esas tareas. En las secciones restantes, destacaremos las opciones y herramientas para cada fase del procesamiento.

## <a name="create-a-data-lake-storage-gen2-account"></a>Creación de una cuenta de Data Lake Storage Gen2

Una cuenta de Data Lake Storage Gen2 es una cuenta de almacenamiento que tiene un espacio de nombres jerárquico. 

Para crear una, consulte el [Inicio rápido: Creación de una cuenta de almacenamiento de Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="create-a-file-system"></a>Creación de un sistema de archivos

Un *sistema de archivos* es un contenedor para carpetas y archivos. Necesita al menos uno de ellos para empezar a ingerir datos en la cuenta de almacenamiento.  Esta es una lista de herramientas que puede usar para crearlos.

|Herramienta | Guía |
|---|--|
|Explorador de Azure Storage | [Creación de un sistema de archivos mediante el Explorador de Storage](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-explorer#create-a-file-system) |
|AzCopy | [Creación de un contenedor de blobs o recurso compartido de archivos mediante AzCopyV10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10#transfer-files)|
|Interfaz de la línea de comandos (CLI) de Hadoop File System (HDFS) con HDInsight |[Creación de un sistema de archivos mediante HDFS con HDInsight](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-hdfs-data-lake-storage?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-file-system) |
|Código en un cuaderno de Azure Databricks|[Creación de un sistema de archivos de la cuenta de almacenamiento (Scala)](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-databricks-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-storage-account-file-system) <br><br> [Creación de un sistema de archivos y su montaje (Python)](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-databricks-spark?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-file-system-and-mount-it)|

Es más fácil crear sistemas de archivos mediante el Explorador de Storage o AzCopy. La creación de sistemas de archivos mediante HDInsight y Databricks es un poco más laboriosa. Sin embargo, si piensa utilizar HDInsight o clústeres de Databricks para procesar los datos de todos modos, puede crear los clústeres primero y usar la CLI de HDFS para crear los sistemas de archivos.  

## <a name="grant-access-to-the-data"></a>Concesión de acceso a los datos

Configure los permisos de acceso adecuados a su cuenta y los datos de su cuenta antes de comenzar a ingerir datos.

Hay tres maneras de conceder acceso:

* Asignar uno de estos roles a un usuario, grupo, identidad administrada por el usuario o entidad de servicio:

  [Lector de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader)

  [Colaborador de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)

  [Propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

* Usar un token de firma de acceso compartido (SAS).

* Usar una clave de cuenta de almacenamiento.

En esta tabla se muestra cómo conceder acceso para cada herramienta o servicio de Azure.

|Herramienta | Para conceder acceso | Guía |
|---|--|---|
|Explorador de Storage| Asignación de un rol a usuarios y grupos | [Asignación de roles de administrador y de no administrador a usuarios con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal) |
|AzCopy| Asignación de un rol a usuarios y grupos <br>**or**<br> Uso de un token de SAS| [Asignación de roles de administrador y de no administrador a usuarios con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)<br><br>[Easily Create a SAS to Download a File from Azure Storage–Using Azure Storage Explorer](https://blogs.msdn.microsoft.com/jpsanders/2017/10/12/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer/) (Creación sencilla de una instancia de SAS para descargar un archivo de Azure Storage: mediante el Explorador de Azure Storage)|
|Apache DistCp | Asignación de un rol a una identidad administrada asignada por el usuario | [Creación de un clúster de HDInsight con Data Lake Storage Gen2](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure Data Factory| Asignación de un rol a una identidad administrada asignada por el usuario<br>**or**<br> Asignación de un rol a una entidad de servicio<br>**or**<br> Uso de una clave de cuenta de almacenamiento | [Propiedades del servicio vinculado](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#linked-service-properties) |
|HDInsight de Azure| Asignación de un rol a una identidad administrada asignada por el usuario | [Creación de un clúster de HDInsight con Data Lake Storage Gen2](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)|
|Azure Databricks| Asignación de un rol a una entidad de servicio | [Uso de Uso del portal para crear una aplicación de Azure AD y una entidad de servicio con acceso a los recursos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)|

Para conceder acceso a carpetas o archivos específicos, consulte estos artículos.

* [Establecimiento de permisos en el nivel de archivo y directorio mediante el Explorador de Azure Storage con Azure Data Lake Storage Gen2](https://review.docs.microsoft.com/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)

* [Listas de control de acceso en archivos y directorios](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control#access-control-lists-on-files-and-directories)

Para obtener información sobre cómo configurar otros aspectos de seguridad, consulte [Guía de seguridad de Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/common/storage-data-lake-storage-security-guide?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="ingest-the-data"></a>Introducción de los datos

En esta sección se resaltan los distintos orígenes de datos y las distintas formas en que esos datos se pueden ingerir en una cuenta de Data Lake Storage Gen2.

![Ingesta de datos en Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/ingest-data.png "Ingesta de datos en Data Lake Storage Gen2")

### <a name="ad-hoc-data"></a>Datos ad-hoc

Representan conjuntos de datos más pequeños que se utilizan para la creación de un prototipo de una aplicación de macrodatos. Hay diferentes maneras de introducir datos ad hoc, según el origen de los datos. 

Esta es una lista de herramientas que puede usar para ingerir datos ad hoc.

| Origen de datos | Introducir mediante |
| --- | --- |
| Equipo local |[Explorador de Storage](https://azure.microsoft.com/features/storage-explorer/)<br><br>[Herramienta AzCopy](../common/storage-use-azcopy-v10.md)|
| Blob de Azure Storage |[Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)<br><br>[Herramienta AzCopy](../common/storage-use-azcopy-v10.md)<br><br>[DistCp ejecutándose en un clúster de HDInsight](data-lake-storage-use-distcp.md)|

### <a name="streamed-data"></a>Datos de streaming

Representa los datos que se pueden generar por diversos orígenes, como aplicaciones, dispositivos o sensores, entre otros. Estos datos se pueden ingerir en Data Lake Storage Gen2 mediante distintas herramientas. Estas herramientas normalmente capturan y procesan los datos en eventos individuales y en tiempo real y, después, escriben los eventos en lotes en Data Lake Storage Gen2 para que puedan procesarse posteriormente.

Esta es una lista de herramientas que puede usar para ingerir datos de streaming.

|Herramienta | Guía |
|---|--|
|Azure HDInsight Storm | [Escritura en HDFS de Apache Hadoop desde Apache Storm en HDInsight](https://docs.microsoft.com/azure/hdinsight/storm/apache-storm-write-data-lake-store) |

### <a name="relational-data"></a>Datos relacionales

También se pueden originar datos desde bases de datos relacionales. Durante un tiempo, las bases de datos relacionales recopilan grandes cantidades de datos que pueden proporcionar información clave si se procesan a través de una canalización de macrodatos. Puede usar las herramientas siguientes para mover estos datos a Data Lake Storage Gen2.

Esta es una lista de herramientas que puede usar para ingerir datos relacionales.

|Herramienta | Guía |
|---|--|
|Azure Data Factory | [Actividad de copia en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |

### <a name="web-server-log-data-upload-using-custom-applications"></a>Datos de registro de servidor web (carga mediante aplicaciones personalizadas)

Este tipo de conjunto de datos es específicamente necesario porque el análisis de los datos de registro del servidor web es un caso de uso común para aplicaciones de macrodatos y requiere que se carguen grandes volúmenes de archivos de registro en Data Lake Storage Gen2. Puede utilizar cualquiera de las herramientas siguientes para escribir sus propios scripts o aplicaciones para cargar dichos datos.

Esta es una lista de herramientas que puede usar para ingerir datos de registro de servidor web.

|Herramienta | Guía |
|---|--|
|Azure Data Factory | [Actividad de copia en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview)  |

Para cargar datos de registro del servidor web y también para cargar otros tipos de datos (por ejemplo, datos de opiniones sociales), un buen enfoque es escribir sus propios aplicaciones o scripts personalizados, porque le ofrece la flexibilidad de incluir el componente que carga los datos como parte de su aplicación de macrodatos mayor. En algunos casos, este código puede adoptar la forma de un script o de una utilidad de línea de comandos simple. En otros casos, el código puede utilizarse para integrar el procesamiento de macrodatos en una aplicación o solución de negocio.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Datos asociados con los clústeres de HDInsight de Azure

La mayoría de los tipos de clúster de HDInsight (Hadoop, HBase, Storm) admiten Data Lake Storage Gen2 como repositorio de almacenamiento de datos. Los clústeres de HDInsight acceden a los datos de Azure Storage Blob (WASB). Para mejorar el rendimiento, puede copiar los datos de WASB en una cuenta de Data Lake Storage Gen2 asociada con el clúster. Puede usar las herramientas siguientes para copiar los datos.

Esta es una lista de herramientas que puede usar para ingerir datos asociados con los clústeres de HDInsight de Azure.

|Herramienta | Guía |
|---|--|
|Apache DistCp | [Uso de DistCp para copiar datos entre Azure Storage Blob y Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
|Herramienta AzCopy | [Transferencia de datos con AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10) |
|Azure Data Factory | [Copia de datos con Azure Data Lake Storage Gen2 como origen o destino mediante Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2) |

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Datos almacenados en clústeres locales o de IaaS de Hadoop

Es posible que haya grandes cantidades de datos almacenados en clústeres de Hadoop existentes, de forma local en los equipos mediante HDFS. Los clústeres de Hadoop pueden encontrarse en una implementación local o dentro de un clúster de IaaS en Azure. Podrían existir requisitos para copiar estos datos en Azure Data Lake Storage Gen2 de forma puntual o periódica. Existen diversas opciones que puede usar para lograr esto. A continuación se muestra una lista de alternativas, además de las ventajas y desventajas asociadas.

| Enfoque | Detalles | Ventajas | Consideraciones |
| --- | --- | --- | --- |
| Usar Azure Data Factory (ADF) para copiar datos directamente desde los clústeres de Hadoop hasta Azure Data Lake Storage Gen2 |[ADF admite HDFS como origen de datos](../../data-factory/connector-hdfs.md) |ADF proporciona compatibilidad inmediata con HDFS, así como una supervisión y administración integrales y de primera clase. |Requiere que se implemente Data Management Gateway localmente o en el clúster de IaaS. |
| Usar Distcp para copiar datos de Hadoop en Azure Storage. Después, copiar los datos de Azure Storage en Data Lake Store Gen2 con el mecanismo adecuado. |Puede copiar datos de Azure Storage a Data Lake Store Gen2 por medio de: <ul><li>[Azure Data Factory](../../data-factory/copy-activity-overview.md)</li><li>[Herramienta AzCopy](../common/storage-use-azcopy-v10.md)</li><li>[Apache DistCp ejecutándose en clústeres de HDInsight](data-lake-storage-use-distcp.md)</li></ul> |Puede usar herramientas de código abierto. |Proceso de varios pasos en el que participan varias tecnologías. |

### <a name="really-large-datasets"></a>Conjuntos de datos realmente grandes

Para cargar conjuntos de datos cuyo tamaño oscila en varios terabytes, el uso de los métodos descritos anteriormente puede a veces resultar lento y costoso. En esos casos, puede usar Azure ExpressRoute.  

Azure ExpressRoute permite crear conexiones privadas entre los centros de datos de Azure y la infraestructura de un entorno local. Esto ofrece una opción confiable para transferir grandes cantidades de datos. Para más información, consulte [Información general sobre ExpressRoute](../../expressroute/expressroute-introduction.md).

## <a name="process-the-data"></a>Procesamiento de los datos

Cuando los datos están disponibles en Data Lake Storage Gen2, puede ejecutar un análisis en esos datos mediante las aplicaciones de macrodatos admitidas. 

![Análisis de datos en Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/analyze-data.png "Análisis de datos en Data Lake Storage Gen2")

Esta es una lista de herramientas que puede usar para ejecutar trabajos de análisis de datos en los datos que se almacenan en Data Lake Storage Gen2.

|Herramienta | Guía |
|---|--|
|HDInsight de Azure | [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure Databricks | [Azure Data Lake Storage Gen2](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)<br><br>[Inicio rápido: Análisis de datos en Azure Data Lake Storage Gen2 con Azure Databricks](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-databricks-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br><br>[Tutorial: Extracción, transformación y carga de datos mediante Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

## <a name="visualize-the-data"></a>Visualización de los datos

Puede usar una combinación de servicios para crear representaciones visuales de los datos almacenados en Data Lake Storage Gen2.

![Visualización de datos en Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/visualize-data.png "Visualización de datos en Data Lake Storage Gen2")

* Puede comenzar con [Azure Data Factory para mover datos desde Data Lake Storage Gen2 hasta Azure SQL Data Warehouse](../../data-factory/copy-activity-overview.md).
* Después, puede [integrar Power BI con Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) para crear una representación visual de los datos.

## <a name="download-the-data"></a>Descarga de datos

También puede descargar o mover datos de Azure Data Lake Storage Gen2 en escenarios como los siguientes:

* Mover datos a otros repositorios para interactuar con las canalizaciones de procesamiento de datos existentes. Por ejemplo, puede mover los datos de Data Lake Storage Gen2 a Azure SQL Database o a SQL Server local.

* Descargar datos en el equipo local para procesarlos en entornos IDE durante la creación de prototipos de aplicaciones.

![Salida de datos de Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/egress-data.png "Salida de datos de Data Lake Storage Gen2")

Esta es una lista de herramientas que puede usar para descargar datos de Data Lake Storage Gen2.

|Herramienta | Guía |
|---|--|
|Azure Data Factory | [Actividad de copia en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |
|Apache DistCp | [Uso de DistCp para copiar datos entre Azure Storage Blob y Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
