---
title: 'Consulta de datos desde Azure Storage compatible con HDFS: Azure HDInsight'
description: Aprenda a consultar datos desde Azure Storage y Azure Data Lake Storage para almacenar los resultados del análisis.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/23/2019
ms.openlocfilehash: 6e0192029decef95dcaecc0c60dce5fd5b6f99ff
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479904"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Uso de Azure Storage con clústeres de Azure HDInsight

Para analizar datos en el clúster de HDInsight, puede almacenar los datos ya sea en [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Storage Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake Storage Gen 2](../storage/blobs/data-lake-storage-introduction.md), o una combinación. Estas opciones de almacenamiento permiten eliminar de forma segura los clústeres de HDInsight que se usan para el cálculo sin perder datos del usuario.

Apache Hadoop admite una noción del sistema de archivos predeterminado. El sistema de archivos predeterminado implica una autoridad y un esquema predeterminados. También se puede usar para resolver rutas de acceso relativas. Durante el proceso de creación del clúster de HDInsight, puede especificar un contenedor de blobs en Azure Storage como el sistema de archivos predeterminado; también, con HDInsight 3.6, puede seleccionar Azure Storage o Azure Data Lake Storage Gen 1/Azure Data Lake Storage Gen 2 como el sistema de archivos predeterminado con algunas excepciones. Para más información sobre la compatibilidad con el uso de Data Lake Store Gen 1 como almacenamiento predeterminado y como almacenamiento vinculado, consulte [Disponibilidades de los clústeres de HDInsight](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

En este artículo, aprenderá cómo funciona Azure Storage con clústeres de HDInsight. Para más información sobre cómo funciona Data Lake Storage Gen 1 con clústeres de HDInsight, consulte [Use Azure Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md) (Uso de Azure Data Lake Store con clústeres de Azure HDInsight). Consulte [Creación de clústeres de Apache Hadoop en HDInsight](hdinsight-hadoop-provision-linux-clusters.md) para obtener información sobre la creación de un clúster de HDInsight.

Azure Storage es una solución de almacenamiento sólida y de uso general, que se integra sin problemas con HDInsight. HDInsight puede usar un contenedor de blobs en Azure Storage como el sistema de archivos predeterminado para el clúster. Mediante una interfaz del sistema de archivos distribuido de Hadoop (HDFS), el conjunto completo de componentes de HDInsight puede operar directamente en datos estructurados o no estructurados almacenados como blobs.

> [!WARNING]  
> Tipo de cuenta de almacenamiento **BlobStorage** sólo puede usarse como almacenamiento secundario para los clústeres de HDInsight.

| Tipo de cuenta de almacenamiento | Servicios admitidos | Niveles de rendimiento admitidos | Niveles de acceso admitidos |
|----------------------|--------------------|-----------------------------|------------------------|
| StorageV2 (uso general v2)  | Blob     | Estándar                    | Frecuente, acceso esporádico, archivar\*   |
| Almacenamiento (uso general v1)   | Blob     | Estándar                    | N/D                    |
| BlobStorage                    | Blob     | Estándar                    | Frecuente, acceso esporádico, archivar\*   |

No se recomienda usar el contenedor de blobs predeterminado para almacenar datos empresariales. Conviene eliminar el contenedor de blobs predeterminado después de cada uso para reducir los costos de almacenamiento. El contenedor predeterminado contiene los registros del sistema y de la aplicación. Asegúrese de recuperar los registros antes de eliminar el contenedor.

No se permite compartir un contenedor de blobs como el sistema de archivos predeterminado entre varios clústeres.

> [!NOTE]  
> El nivel de acceso de archivo es un nivel sin conexión que tiene una latencia de recuperación de varias horas y no se recomienda para su uso con HDInsight. Para más información, consulte [Nivel de acceso de archivo](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

Si decide proteger la cuenta de almacenamiento con el **Firewalls y redes virtuales** restricciones en **redes seleccionadas**, asegúrese de habilitar la excepción **permitir confianza de Microsoft servicios...**  para que HDInsight puede acceder a su cuenta de almacenamiento.

## <a name="hdinsight-storage-architecture"></a>Arquitectura de almacenamiento de HDInsight
El diagrama siguiente proporciona una panorámica de la arquitectura de almacenamiento de HDInsight disponible al utilizar Azure Storage:

![Los clústeres de Hadoop usan la API de HDFS para acceder y almacenar datos estructurados y no estructurados en Blob Storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "Arquitectura de almacenamiento para HDInsight")

HDInsight brinda acceso al sistema de archivos distribuidos que se adjunta localmente a los nodos de ejecución. Se puede acceder a este sistema de archivos usando el URI completo, por ejemplo:

    hdfs://<namenodehost>/<path>

Además, HDInsight le permite acceder a los datos almacenados en Azure Storage. La sintaxis es:

    wasb://<containername>@<accountname>.blob.core.windows.net/<path>

A la hora de usar una cuenta de Azure Storage con clústeres de HDInsight, es necesario tener en cuenta algunas cosas.

* **Contenedores de las cuentas de almacenamiento que se conectan a un clúster:** dado que el nombre y la clave de la cuenta se asocian al clúster durante la creación, tiene acceso total a los blobs de dichos contenedores.

* **Contenedores públicos o blobs públicos de las cuentas de almacenamiento que NO se conectan a un clúster:** tiene permiso de solo lectura de los blobs de los contenedores.
  
> [!NOTE]  
> Los contenedores públicos le permiten obtener una lista de todos los blobs disponibles del contenedor en cuestión y obtener sus metadatos. Los blobs públicos le permiten acceder a los blobs solo si conoce la URL exacta. Para más información, consulte el artículo sobre la [Administración del acceso a los contenedores y los blobs](../storage/blobs/storage-manage-access-to-resources.md).

* **Contenedores privados de las cuentas de almacenamiento que NO se conectan a un clúster:** no puede tener acceso a los blobs de los contenedores a menos que defina la cuenta de almacenamiento al enviar los trabajos de WebHCat. Esto se explica posteriormente en este artículo.

Las cuentas de almacenamiento que se definen en el proceso de creación y sus claves se almacenan en `%HADOOP_HOME%/conf/core-site.xml` en los nodos del clúster. El comportamiento predeterminado de HDInsight es usar las cuentas de almacenamiento definidas en el archivo core-site.xml. Puede modificar esta configuración mediante [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Varios trabajos de WebHCat, incluidos Apache Hive, MapReduce, streaming de Apache Hadoop y Apache Pig, pueden llevar una descripción de cuentas de almacenamiento y metadatos con ellos. (Actualmente esto funciona para Pig con cuentas de almacenamiento pero no para metadatos). Para obtener más información, consulte [Uso de un clúster de HDInsight con cuentas de almacenamiento y tiendas de metadatos alternativas](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Los blobs se pueden usar para datos estructurados y no estructurados. Los contenedores de blobs almacenan los datos como pares de clave-valor y no hay jerarquía de directorios. No obstante, el carácter de barra diagonal ( / ) se puede usar en el nombre de la clave para que parezca que el archivo está almacenado dentro de una estructura de directorios. Por ejemplo, la clave de un blob puede ser *input/log1.txt*. No hay directorios *input* , pero dada la presencia del carácter de barra diagonal en el nombre de la clave, parece la ruta de un archivo.

## <a id="benefits"></a>Ventajas de Azure Storage
Se reduce el costo de rendimiento implícito de colocación no de clústeres de cálculo y los recursos de almacenamiento por la forma en que los clústeres de cálculo se crean cerca de los recursos de la cuenta de almacenamiento dentro de la región de Azure, donde la red de alta velocidad que sean eficaces para el los nodos para tener acceso a los datos del almacenamiento de Azure de proceso.

Hay varias ventajas asociadas al almacenamiento de datos en Azure Storage en lugar de en HDFS:

* **Uso compartido y reutilización de datos**: los datos de HDFS se ubican dentro del clúster de proceso. Solamente las aplicaciones que tengan acceso al clúster de cálculo podrán usar los datos usando las API HDFS. Los datos de almacenamiento de Azure pueden obtenerse mediante las API HDFS o con el [API de REST de Blob Storage](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API). Por lo tanto, se puede usar un conjunto mayor de aplicaciones (incluyendo otros clústeres de HDInsight) y herramientas para producir y consumir los datos.

* **Archivado de datos**: almacenar los datos en Azure Storage permite que los clústeres de HDInsight usados para el cálculo se eliminen de forma segura sin perder datos del usuario.

* **Costo del almacenamiento de datos:** almacenar datos en DFS a largo plazo es más caro que almacenarlos en Azure Storage, ya que el costo de un clúster de proceso es superior al de Azure Storage. Además, como no hay que volver a cargar los datos para cada generación de clúster de cálculo, también se ahorra en costes de carga de datos.

* **Escalabilidad horizontal elástica**: aunque HDFS proporciona un sistema de archivos escalable en horizontal, la escala se determina en función del número de nodos que cree para su clúster. Cambiar la escala puede ser un proceso más complicado que basarse en las funcionalidades de escalado elástico que se obtienen automáticamente en Azure Storage.

* **Replicación geográfica:** su almacenamiento de Azure Storage se puede replicar geográficamente. Aunque esto le aporta recuperación geográfica y redundancia de datos, una conmutación por error en la ubicación replicada geográficamente afecta gravemente a su rendimiento y puede incurrir en costes adicionales. Por lo tanto, nuestra recomendación es que elija la replicación geográfica de forma inteligente y únicamente si merece la pena pagar el coste adicional por el valor de los datos.

Determinados trabajos y paquetes de MapReduce podrían crear resultados intermedios que realmente no desea almacenar en Azure Storage. En tal caso, puede optar por almacenar los datos en el HDFS local. De hecho, HDInsight usa DFS para varios de estos resultados intermedios en los trabajos de Hive y otros procesos.

> [!NOTE]  
> La mayoría de los comandos HDFS (por ejemplo, `ls`, `copyFromLocal` y `mkdir`) siguen funcionando según lo previsto. Únicamente los comandos específicos de la implementación nativa de HDFS (a la que nos referiremos como DFS), como `fschk` y `dfsadmin`, muestran comportamientos diferentes en Azure Storage.

## <a name="address-files-in-azure-storage"></a>Archivos adicionales en Azure Storage
El esquema de URI para acceder a los archivos de Azure Storage desde HDInsight es:

```config
wasb://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>
```

El esquema URI proporciona acceso sin cifrar (con el prefijo *wasb:* ) y acceso cifrado SSL con *wasbs*). Se recomienda usar *wasbs* siempre que sea posible, incluso cuando se acceda a datos que se encuentren en la misma región de Azure.

El `<BlobStorageContainerName>` identifica el nombre del contenedor de blobs en almacenamiento de Azure.
El `<StorageAccountName>` identifica el nombre de cuenta de almacenamiento de Azure. Se necesita el nombre completo de dominio (FQDN).

Si no `<BlobStorageContainerName>` ni `<StorageAccountName>` se ha especificado, se usa el sistema de archivos predeterminado. Para los archivos del sistema de archivos predeterminado, puede usar una ruta relativa o absoluta. Por ejemplo, se puede hacer referencia al archivo *hadoop-mapreduce-examples.jar* que se incluye con los clústeres de HDInsight usando uno de los siguientes:

```config
wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
wasb:///example/jars/hadoop-mapreduce-examples.jar
/example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> El nombre del archivo es `hadoop-examples.jar` en las versiones 2.1 y 1.6 de los clústeres de HDInsight.

La ruta de acceso es el nombre de ruta de acceso HDFS de archivo o directorio. Dado que los contenedores de Azure Storage son almacenes de pares clave-valor, no hay ningún sistema de archivos jerárquico real. Un carácter de barra inclinada ( / ) dentro de la clave de blob se interpreta como separador de directorios. Por ejemplo, el nombre del blob para *hadoop-mapreduce-examples.jar* es:

```bash
example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> Al trabajar con blobs fuera de HDInsight, la mayoría de las utilidades no reconocen el formato WASB y en su lugar, espera un formato básico de ruta de acceso, como `example/jars/hadoop-mapreduce-examples.jar`.

##  <a name="blob-containers"></a>Contenedores de blobs
Para usar blobs, cree primero un [cuenta de Azure Storage](../storage/common/storage-create-storage-account.md). Como parte de esto, especifica una región de Azure donde se crea la cuenta de almacenamiento. El clúster y la cuenta de almacenamiento deben ubicarse en la misma región. La base de datos de SQL Server de la tienda de metadatos Hive y la base de datos de SQL Server de la tienda de metadatos Apache Oozie también deben encontrarse en la misma región.

Cualquiera que sea su ubicación, todos los blobs que cree pertenecerán a un contenedor de su cuenta de Azure Storage. Este contenedor puede ser un blob existente creado fuera de HDInsight, o bien un contenedor que se crea para un clúster de HDInsight.

El contenedor de blobs predeterminado almacena información específica del clúster, como registros y el historial de trabajos. No comparta un contenedor de blobs predeterminado con varios clústeres de HDInsight. Se podría dañar el historial de trabajos. Es recomendable usar un contenedor diferente para cada clúster y colocar los datos compartidos en una cuenta de almacenamiento vinculada especificada en la implementación de todos los clústeres pertinentes en lugar de la cuenta de almacenamiento predeterminada. Para obtener más información sobre cómo configurar cuentas de almacenamiento vinculadas, consulte [de clústeres de HDInsight crear](hdinsight-hadoop-provision-linux-clusters.md). Sin embargo, puede volver a usar un contenedor de almacenamiento predeterminado después de que se haya eliminado el clúster de HDInsight original. En el caso de los clústeres de HBase, para conservar el esquema y los datos de tabla de HBase se puede crear un nuevo clúster de HBase mediante el contenedor de blobs predeterminado que usaba un clúster de HBase eliminado.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="interacting-with-azure-storage"></a>Interactuar con el almacenamiento de Azure

Microsoft proporciona las siguientes herramientas para trabajar con Azure Storage:

| Herramienta | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [CLI de Azure](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="use-additional-storage-accounts"></a>Uso de cuentas de almacenamiento adicionales

Al crear un clúster de HDInsight, se especifica la cuenta de Azure Storage a la que quiere asociarlo. Además de esta cuenta de almacenamiento, puede agregar otras desde la misma suscripción de Azure o desde otras diferentes tanto durante el proceso de creación como después de que el clúster se haya creado. Para obtener instrucciones sobre cómo agregar más cuentas de almacenamiento, consulte [Creación de clústeres de HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> No se admite el uso de una cuenta de almacenamiento adicional en una ubicación diferente a la del clúster de HDInsight.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a usar Azure Storage compatible con HDFS con HDInsight. Esto le permite crear soluciones de adquisición de datos de archivado escalables y a largo plazo y usar HDInsight para desbloquear la información que hay dentro de los datos estructurados y no estructurados almacenados.

Para más información, consulte:

* [Introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Introducción a Azure Data Lake Storage](../data-lake-store/data-lake-store-get-started-portal.md)
* [Carga de datos en HDInsight](hdinsight-upload-data.md)
* [Uso de Apache Hive con HDInsight](hadoop/hdinsight-use-hive.md)
* [Uso de Apache Pig con HDInsight](hadoop/hdinsight-use-pig.md)
* [Usar firmas de acceso compartido de Azure Storage para restringir el acceso a datos con HDInsight](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](hdinsight-hadoop-use-data-lake-storage-gen2.md)