---
title: Copia de datos en Azure Data Lake Storage Gen2 (versión preliminar) con Distcp | Microsoft Docs
description: Uso de la herramienta Distcp para copiar datos hacia y desde Data Lake Storage Gen2 (versión preliminar)
services: storage
author: seguler
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 06/27/2018
ms.author: seguler
ms.openlocfilehash: 065c4c4315bda209484cc1b2449980e55d4ac798
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522703"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-storage-gen2-preview"></a>Uso de Distcp para copiar datos entre Azure Storage Blob y Data Lake Storage Gen2 (versión preliminar)

Si tiene un clúster de HDInsight con acceso a Azure Data Lake Storage Gen2 (versión preliminar), puede usar herramientas del ecosistema de Hadoop, como [Distcp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html), tanto para copiar datos **a un almacenamiento de clúster de HDInsight (WASB) como para copiarlos desde este** a una cuenta compatible con Data Lake Storage Gen2. En este artículo se proporcionan instrucciones sobre cómo usar la herramienta Distcp.

## <a name="prerequisites"></a>Requisitos previos

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
* **Una cuenta de Azure Storage con la característica de Azure Data Lake Storage (versión preliminar) habilitada**. Para obtener instrucciones para crear uno, consulte [Creación de una cuenta de almacenamiento de Azure Data Lake Storage Gen2 (versión preliminar)](quickstart-create-account.md)
* **Clúster de Azure HDInsight** con acceso a una cuenta de Data Lake Storage. Consulte [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](use-hdi-cluster.md). Asegúrese de habilitar el Escritorio remoto para el clúster.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Usar Distcp desde un clúster de HDInsight de Linux

Un clúster de HDInsight incluye la utilidad Distcp, que puede utilizarse para copiar datos de orígenes diferentes en un clúster de HDInsight. Si ha configurado el clúster de HDInsight para usar conjuntamente Azure Blob Storage y Azure Data Lake Storage, la utilidad Distcp puede utilizarse también directamente para copiar datos entre ellos también. En esta sección veremos cómo usar la utilidad Distcp.

1. Desde el escritorio, use SSH para conectarse al clúster. Consulte [Conexión a un clúster de HDInsight basado en Linux](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Ejecute los comandos desde el símbolo del sistema SSH.

2. Compruebe si puede tener acceso a Azure Storage Blob (WASB). Ejecute el siguiente comando:

        hdfs dfs –ls wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/

    La salida debería proporcionar una lista de contenido en el blob de almacenamiento.

3. De forma similar, compruebe si puede tener acceso a la cuenta de Data Lake Storage desde el clúster. Ejecute el siguiente comando:

        hdfs dfs -ls abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/

    La salida debería proporcionar una lista de archivos o carpetas en la cuenta de Data Lake Storage.

4. Utilice Distcp para copiar datos desde WASB a una cuenta de Data Lake Storage.

        hadoop distcp wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder

    El comando copia el contenido de la carpeta **/example/data/gutenberg/** de Blob Storage a **/myfolder** en la cuenta de Data Lake Storage.

5. Asimismo, utilice Distcp para copiar datos de la cuenta de Data Lake Storage a Blob Storage (WASB).

        hadoop distcp abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg

    El comando copia el contenido de **/myfolder** de la cuenta de Data Lake Store en la carpeta **/example/data/gutenberg/** de WASB.

## <a name="performance-considerations-while-using-distcp"></a>Consideraciones de rendimiento sobre el uso de DistCp

Dado que la granularidad más baja de DistCp es un único archivo, configurar el número máximo de copias simultáneas es el parámetro más importante para optimizar con respecto a Data Lake Storage. El número de copias simultáneas se controla mediante la definición del número de parámetros de mapeador (**m**) en la línea de comandos. Este parámetro especifica el número máximo de mapeadores que se usan para copiar los datos. El valor predeterminado es 20.

**Ejemplo**

    hadoop distcp wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>¿Cómo se puede determinar el número de asignadores que se debe usar?

A continuación hay algunas instrucciones que puede usar.

* **Paso 1: Determinación de la memoria total de YARN**: el primer paso consiste en determinar la memoria YARN disponible para el clúster donde se ejecuta el trabajo DistCp. Esta información está disponible en el portal de Ambari asociado con el clúster. Vaya a YARN y haga clic en la pestaña de configuración para ver la memoria YARN. Para obtener la memoria de YARN total, multiplique la memoria de YARN por cada nodo por el número de nodos que tiene en el clúster.

* **Paso 2: Cálculo del número de asignadores**: el valor de **m** es igual al cociente de la memoria de YARN total dividido por el tamaño del contenedor de YARN. La información del tamaño de contenedor de YARN está también disponible en el portal del Ambari. Vaya a YARN y examine la pestaña de configuración. En esta ventana se muestra el tamaño del contenedor de YARN. La ecuación para llegar al número de asignadores (**m**) es

        m = (number of nodes * YARN memory for each node) / YARN container size

**Ejemplo**

Supongamos que tiene 4 nodos D14v2s en el clúster y que intenta transferir 10 TB de datos desde 10 carpetas diferentes. Cada una de las carpetas contiene diferentes cantidades de datos y los tamaños de archivo dentro de cada carpeta son diferentes.

* **Memoria de YARN total**: en el portal de Ambari determinará que la memoria de YARN es de 96 GB para un nodo D14. Por lo tanto, la memoria de YARN total para el clúster de cuatro nodos es: 

        YARN memory = 4 * 96GB = 384GB

* **Número de asignadores**: en el portal de Ambari determinará que el tamaño del contenedor de YARN es 3072 para un nodo del clúster D14. Por lo tanto, el número de asignadores es:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Si otras aplicaciones usan memoria, solo puede elegir usar una parte de la memoria de YARN del clúster para DistCp.

### <a name="copying-large-datasets"></a>Copia de grandes conjuntos de datos

Cuando el tamaño del conjunto de datos que se va a mover es grande (por ejemplo, >1 TB) o si tiene muchas carpetas diferentes, debe considerar el uso de varios trabajos de DistCp. Aunque es probable que no haya ganancia de rendimiento, los trabajos se reparten para que si alguno da error, solo sea necesario reiniciar ese trabajo específico en lugar del trabajo entero.

### <a name="limitations"></a>Limitaciones

* DistCp intenta crear a asignadores que tengan un tamaño similar para optimizar el rendimiento. El aumento del número de asignadores no siempre aumenta el rendimiento.

* DistCp está limitado a solo un asignador por archivo. Por lo tanto, no debería tener más asignadores que archivos. Como DistCp solo puede asignar a un asignador a un archivo, esto limita la cantidad de simultaneidad que puede usarse para copiar archivos de gran tamaño.

* Si tiene un número pequeño de archivos grandes, debe dividirlos en fragmentos de archivo de 256 MB para ofrecerle una mayor simultaneidad en potencia. 
