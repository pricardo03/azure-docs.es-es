---
title: Copia de datos en y desde WASB en Azure Data Lake Storage Gen1 mediante Distcp| Microsoft Docs
description: Use la herramienta Distcp para copiar datos en y desde los blobs de Azure Storage en Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: fbefe233ce0d2477982faf0a9f38a73062e0c7a1
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884472"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-azure-data-lake-storage-gen1"></a>Use Distcp to copy data between Azure Storage Blobs and Azure Data Lake Storage Gen1 (Usar Distcp para copiar datos entre los blobs de Azure Storage y Azure Data Lake Storage Gen1)
> [!div class="op_single_selector"]
> * [Uso de DistCp](data-lake-store-copy-data-wasb-distcp.md)
> * [Uso de AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Si tiene un clúster de HDInsight con acceso a Azure Data Lake Storage Gen1, puede usar herramientas del ecosistema de Hadoop, como Distcp, tanto para copiar datos **en un almacenamiento de clúster de HDInsight (WASB) como para copiarlos desde este** en una cuenta de Data Lake Storage Gen1. En este artículo se proporcionan instrucciones sobre cómo usar la herramienta Distcp.

## <a name="prerequisites"></a>Requisitos previos

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
* **Una cuenta de Azure Data Lake Storage Gen1**. Para instrucciones sobre cómo crear una, consulte la [introducción a Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* **Clúster de Azure HDInsight** con acceso a una cuenta de Data Lake Storage Gen1. Consulte [Creación de un clúster de HDInsight con Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md). Asegúrese de habilitar el Escritorio remoto para el clúster.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Usar Distcp desde un clúster de HDInsight de Linux

Un clúster de HDInsight incluye la utilidad Distcp, que puede utilizarse para copiar datos de orígenes diferentes en un clúster de HDInsight. Si ha configurado el clúster de HDInsight para usar Data Lake Storage Gen1 como almacenamiento adicional, la utilidad Distcp puede usarse también directamente y sin configuraciones adicionales para copiar datos en y desde una cuenta de Data Lake Storage Gen1. En esta sección veremos cómo usar la utilidad Distcp.

1. Desde el escritorio, use SSH para conectarse al clúster. Consulte [Conexión a un clúster de HDInsight basado en Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Ejecute los comandos desde el símbolo del sistema SSH.

2. Compruebe si puede tener acceso a Azure Storage Blob (WASB). Ejecute el siguiente comando:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    La salida debería proporcionar una lista de contenido en el blob de almacenamiento.

3. De forma similar, compruebe si puede tener acceso a la cuenta de Data Lake Storage Gen1 desde el clúster. Ejecute el siguiente comando:

        hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/

    La salida debería proporcionar una lista de archivos o carpetas en la cuenta de Data Lake Storage Gen1.

4. Utilice Distcp para copiar datos desde WASB en una cuenta de Data Lake Storage Gen1.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder

    El comando copia el contenido de la carpeta **/example/data/gutenberg/** de WASB en **/myfolder** en la cuenta de Data Lake Storage Gen1.

5. Asimismo, utilice Distcp para copiar datos de la cuenta de Data Lake Storage Gen1 en WASB.

        hadoop distcp adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    El comando copia el contenido de **/myfolder** de la cuenta de Data Lake Storage Gen1 en la carpeta **/example/data/gutenberg/** de WASB.

## <a name="performance-considerations-while-using-distcp"></a>Consideraciones de rendimiento sobre el uso de DistCp

Dado que la granularidad más baja de DistCp es un único archivo, configurar el número máximo de copias simultáneas es el parámetro más importante para optimizar con respecto a Data Lake Storage Gen1. El número de copias simultáneas se controla mediante la definición del número de parámetros de mapeador ("m") en la línea de comandos. Este parámetro especifica el número máximo de mapeadores que se usan para copiar los datos. El valor predeterminado es 20.

**Ejemplo**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>¿Cómo se puede determinar el número de asignadores que se debe usar?

A continuación hay algunas instrucciones que puede usar.

* **Paso 1: Determinación de la memoria YARN total** -el primer paso es determinar la memoria YARN disponible para el clúster donde se ejecuta el trabajo DistCp. Esta información está disponible en el portal de Ambari asociado con el clúster. Vaya a YARN y haga clic en la pestaña de configuración para ver la memoria YARN. Para obtener la memoria de YARN total, multiplique la memoria de YARN por cada nodo por el número de nodos que tiene en el clúster.

* **Paso 2: Cálculo del número de mapeadores**: el valor de **m** es igual al cociente de la memoria de YARN total dividido por el tamaño del contenedor de YARN. La información del tamaño de contenedor de YARN está también disponible en el portal del Ambari. Vaya a YARN y examine la pestaña de configuración. En esta ventana se muestra el tamaño del contenedor de YARN. La ecuación para llegar al número de asignadores (**m**) es

        m = (number of nodes * YARN memory for each node) / YARN container size

**Ejemplo**

Supongamos que tiene 4 nodos D14v2s en el clúster y que intenta transferir 10 TB de datos desde 10 carpetas diferentes. Cada una de las carpetas contiene diferentes cantidades de datos y los tamaños de archivo dentro de cada carpeta son diferentes.

* Memoria de YARN total: en el portal de Ambari determinará que la memoria de YARN es de 96 GB para un nodo D14. Por lo tanto, la memoria de YARN total para el clúster de cuatro nodos es: 

        YARN memory = 4 * 96GB = 384GB

* Número de asignadores: en el portal de Ambari determinará que el tamaño del contenedor de YARN es 3072 para un nodo del clúster D14. Por lo tanto, el número de asignadores es:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Si otras aplicaciones usan memoria, solo puede elegir usar una parte de la memoria de YARN del clúster para DistCp.

### <a name="copying-large-datasets"></a>Copia de grandes conjuntos de datos

Cuando el tamaño del conjunto de datos que se va a mover es grande (por ejemplo, >1 TB) o si tiene muchas carpetas diferentes, debe considerar el uso de varios trabajos de DistCp. Aunque es probable que no haya ganancia de rendimiento, los trabajos se reparten para que si alguno da error, solo sea necesario reiniciar ese trabajo específico en lugar del trabajo entero.

### <a name="limitations"></a>Limitaciones

* DistCp intenta crear a asignadores que tengan un tamaño similar para optimizar el rendimiento. El aumento del número de asignadores no siempre aumenta el rendimiento.

* DistCp está limitado a solo un asignador por archivo. Por lo tanto, no debería tener más asignadores que archivos. Como DistCp solo puede asignar a un asignador a un archivo, esto limita la cantidad de simultaneidad que puede usarse para copiar archivos de gran tamaño.

* Si tiene un número pequeño de archivos grandes, debe dividirlos en fragmentos de archivo de 256 MB para ofrecerle una mayor simultaneidad en potencia. 
 
* Si va a copiar desde una cuenta de Azure Blob Storage, el trabajo de copia puede estar limitado por el lado del almacenamiento de blobs. Esta situación degrada el rendimiento de su trabajo de copia. Para aprender sobre los límites de Azure Blob Storage, consulte la información al respecto en [Límites de suscripciones y servicios de Azure](../azure-subscription-service-limits.md).

## <a name="see-also"></a>Vea también
* [Copiar datos desde Azure Storage BLOB a Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md)
* [Proteger los datos en Data Lake Storage Gen1](data-lake-store-secure-data.md)
* [Usar Azure Data Lake Analytics con Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Uso de Azure HDInsight con Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)
