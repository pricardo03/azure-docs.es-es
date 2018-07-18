---
title: Uso de Data Lake Storage Gen2 (versión preliminar) con clústeres de Azure HDInsight
description: Aprenda a consultar datos desde Azure Data Lake Storage Gen2 (versión preliminar) para almacenar los resultados del análisis.
keywords: hdfs, datos estructurados, datos no estructurados, data lake store, entrada Hadoop, salida Hadoop, almacenamiento hadoop, entrada hdfs, salida hdfs, almacenamiento hdfs, wasb azure
services: hdinsight,storage
documentationcenter: ''
tags: azure-portal
author: jamesbak
manager: jahogg
ms.component: data-lake-storage-gen2
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: e9fd28ac21ce843655697c5d58849d940e305fce
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344961"
---
# <a name="use-azure-data-lake-storage-gen2-preview-with-azure-hdinsight-clusters"></a>Uso de Data Lake Storage Gen2 (versión preliminar) con clústeres de Azure HDInsight

Para analizar datos en el clúster HDInsight, puede almacenar los datos en cualquier combinación de Azure Storage, Azure Data Lake Storage Gen1 o Azure Data Lake Storage Gen2 (versión preliminar). Todas las opciones de almacenamiento permiten eliminar de forma segura clústeres de HDInsight que se usan para el cálculo sin perder datos del usuario.

Hadoop admite una noción del sistema de archivos predeterminado. El sistema de archivos predeterminado implica una autoridad y un esquema predeterminados. También se puede usar para resolver rutas de acceso relativas. Durante el proceso de creación del clúster de HDInsight, puede especificar un contenedor de blobs en Azure Storage o Azure Data Lake Storage como el sistema de archivos predeterminado. Además, con HDInsight 3.5, puede seleccionar Azure Storage o Azure Data Lake Storage como el sistema de archivos predeterminado, con algunas excepciones.

En este artículo, aprenderá cómo funciona Azure Data Lake Storage Gen2 con clústeres de HDInsight. Para más información sobre la creación de un clúster HDInsight, consulte el artículo sobre [configuración de clústeres HDInsight mediante Azure Data Lake Storage con Hadoop, Spark, Kafka, y más](quickstart-create-connect-hdi-cluster.md).

Azure Storage es una solución de almacenamiento sólida y de uso general, que se integra sin problemas con HDInsight. HDInsight puede usar Azure Data Lake Storage como el sistema de archivos predeterminado para el clúster. A través de una interfaz del sistema de archivos distribuidos de Hadoop (HDFS), el conjunto completo de componentes de HDInsight puede operar directamente en archivos de Azure Data Lake Storage.

No se recomienda usar el sistema de archivos predeterminado para almacenar datos empresariales. Conviene eliminar el sistema de archivos predeterminado después de cada uso para reducir los costos de almacenamiento. Tenga en cuenta que el contenedor predeterminado contiene los registros del sistema y de la aplicación. Asegúrese de recuperar los registros antes de eliminar el contenedor.

No se permite compartir un sistema de archivos entre varios clústeres.

## <a name="hdinsight-storage-architecture"></a>Arquitectura de almacenamiento de HDInsight

El diagrama siguiente proporciona una panorámica de la arquitectura de almacenamiento de HDInsight disponible al utilizar Azure Storage:

![Los clústeres de Hadoop usan la API de HDFS para acceder y almacenar datos estructurados y no estructurados en Blob Storage.](./media/use-hdi-cluster/HDI.ABFS.Arch.png "Arquitectura de almacenamiento para HDInsight")

HDInsight brinda acceso al sistema de archivos distribuidos que se adjunta localmente a los nodos de ejecución. Se puede acceder a este sistema de archivos usando el URI completo, por ejemplo:

    hdfs://<NAME_NODE_HOST>/<PATH>

Además, HDInsight le permite acceder a los datos almacenados en Azure Data Lake Storage. La sintaxis es:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<path>

A la hora de usar una cuenta de Azure Storage con clústeres de HDInsight, es necesario considerar lo siguiente.

* **Los archivos de las cuentas de almacenamiento que están conectados a un clúster** tienen asociado el nombre de cuenta y la clave del clúster durante la creación. Esta configuración le da acceso completo a los archivos en el sistema de archivos.

* **Los archivos públicos en cuentas de almacenamiento que NO están conectados a un clúster** exponen permisos de solo lectura a los archivos en el sistema de archivos.
  
  > [!NOTE]
  > Los sistemas de archivos públicos le permiten obtener una lista de todos los archivos disponibles en el sistema de archivos y acceder a los metadatos. Los sistemas de archivos públicos le permiten acceder a los archivos solo si conoce la URL exacta. Para más información, consulte el artículo sobre cómo [restringir el acceso a contenedores y blobs](http://msdn.microsoft.com/library/windowsazure/dd179354.aspx) (las reglas para contenedores y blobs funcionan de la misma manera con los mismo archivos y sistema de archivos).
 
* **Los sistemas de archivos privados de las cuentas de almacenamiento que no están conectados a un clúster:** no permiten el acceso al sistema de archivos a menos que defina la cuenta de almacenamiento al enviar los trabajos de WebHCat. Los motivos de esta restricción se explican más adelante en este artículo.

Las cuentas de almacenamiento definidas en el proceso de creación y sus claves se almacenan en *%HADOOP_HOME%/conf/core-site.xml* en los nodos de clúster. El comportamiento predeterminado de HDInsight es usar las cuentas de almacenamiento definidas en el archivo *core-site.xml*. Puede modificar esta configuración mediante [Ambari](../../hdinsight/hdinsight-hadoop-manage-ambari.md)

Varios trabajos de WebHCat, incluidos Hive, MapReduce, streaming de Hadoop y Pig, pueden llevar una descripción de cuentas de almacenamiento y metadatos con ellos. (Actualmente este enfoque funciona para Pig con cuentas de almacenamiento pero no para metadatos). Para obtener más información, consulte [Uso de un clúster de HDInsight con cuentas de almacenamiento y tiendas de metadatos alternativas](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

## <a id="benefits"></a>Ventajas de Azure Storage

El costo de rendimiento implícito por no tener ubicados juntos los recursos de almacenamiento y clústeres de proceso se ve mitigado por el modo en que los clústeres de proceso se crean cerca de los recursos de la cuenta de almacenamiento dentro de la región de Azure, donde la red de alta velocidad consigue que los nodos de proceso sean eficientes en el acceso a los datos de Azure Storage.

Hay varias ventajas asociadas al almacenamiento de datos en Azure Storage en lugar de en HDFS:

* **Reutilización de uso compartido de datos:** los datos de HDFS se ubican dentro del clúster de cálculo. Solamente las aplicaciones que tengan acceso al clúster de cálculo podrán usar los datos usando las API HDFS. Se puede acceder a los datos de Azure Storage mediante las API de HDFS o las [API de REST de Blob Storage][blob-storage-restAPI]. Por lo tanto, se puede usar un conjunto mayor de aplicaciones (incluyendo otros clústeres de HDInsight) y herramientas para producir y consumir los datos.

* **Archivo de datos:** almacenar los datos en Azure Storage permite que los clústeres de HDInsight usados para el cálculo se eliminen de forma segura sin perder datos del usuario.

* **Costo de almacenamiento de datos:** almacenar datos en HDFS nativo es más caro a largo plazo que almacenarlos en Azure Storage, ya que el costo de un clúster de proceso es superior al de Azure Storage. Además, como no hay que volver a cargar los datos para cada generación de clúster de cálculo, también se ahorra en costes de carga de datos.

* **Escalación horizontal elástica:** aunque HDFS proporciona un sistema de archivos escalable en horizontal, la escala se determina en función del número de nodos que cree para su clúster. Cambiar la escala puede ser un proceso más complicado que basarse en las funcionalidades de escalado elástico que se obtienen automáticamente en Azure Storage.

* **Replicación geográfica:** los datos de Azure Storage se pueden replicar geográficamente. Aunque esta capacidad le aporta recuperación geográfica y redundancia de datos, la compatibilidad de una conmutación por error en la ubicación replicada geográficamente afecta gravemente a su rendimiento y puede dar lugar a costes adicionales. Por lo tanto, elija la replicación geográfica con prudencia y únicamente si merece la pena pagar el costo adicional por el valor de los datos.

* **Administración del ciclo de vida de datos:** todos los datos de cualquier sistema de archivos pasan a través de su propio ciclo de vida. Habitualmente, los datos son muy valiosos al principio y se acceden con frecuencia, luego pasan a ser menos valiosos y a requerir menos acceso y, por último, se deben archivar o eliminar. Azure Storage proporciona directivas de administración de capas de datos y de ciclo de vida que clasifican los datos apropiadamente para su etapa de ciclo de vida.

Determinados trabajos y paquetes de MapReduce podrían crear resultados intermedios que realmente no desea almacenar en Azure Storage. En tal caso, puede optar por almacenar los datos en el HDFS local. De hecho, HDInsight usa la implementación de HDFS nativo (a lo que se hace referencia como DFS) para varios de estos resultados intermedios en los trabajos de Hive y otros procesos.

> [!NOTE]
> La mayoría de los comandos HDFS (por ejemplo, `ls`, `copyFromLocal` y `mkdir`) siguen funcionando según lo previsto. Únicamente los comandos específicos de DFS, como `fschk` y `dfsadmin`, muestran comportamientos diferentes en Azure Storage.

## <a name="create-an-data-lake-storage-file-system"></a>Creación de un sistema de archivos de Data Lake Storage

Para usar el sistema de archivos, en primer lugar debe crear una [cuenta de Azure Storage][azure-storage-create]. Como parte de este proceso, debe especificar una región de Azure donde se crea la cuenta de almacenamiento. El clúster y la cuenta de almacenamiento deben ubicarse en la misma región. La base de datos de SQL Server de la tienda de metadatos Hive y la base de datos de SQL Server de la tienda de metadatos Oozie también deben encontrarse en la misma región.

Cualquiera que sea su ubicación, todos los blobs que cree pertenecerán a un sistema de archivos de su cuenta de Azure Data Lake Storage. 

El sistema de archivos predeterminado de Data Lake Storage almacena información específica del clúster, como registros y el historial de trabajos. No comparta un sistema de archivos predeterminado de Data Lake Storage con varios clústeres HDInsight. Se podría dañar el historial de trabajos. Es recomendable usar un sistema de archivos diferente para cada clúster y colocar los datos compartidos en una cuenta de almacenamiento vinculada especificada en la implementación de todos los clústeres pertinentes en lugar de la cuenta de almacenamiento predeterminada. Para más información acerca de cómo configurar cuentas de almacenamiento vinculadas, consulte [Creación de clústeres de HDInsight][hdinsight-creation]. Sin embargo, puede volver a usar un sistema de archivos de almacenamiento predeterminado después de que se haya eliminado el clúster de HDInsight original. En el caso de los clústeres de HBase, para conservar el esquema y los datos de tabla de HBase se puede crear un nuevo clúster de HBase mediante el contenedor de blobs predeterminado que usaba un clúster de HBase eliminado.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Uso de Azure Portal

Al crear un clúster de HDInsight desde el Portal, tendrá las opciones (tal y como se muestra en la siguiente captura de pantalla) para proporcionar los detalles de la cuenta de almacenamiento. También puede especificar si quiere una cuenta de almacenamiento adicional asociada al clúster y, en este caso, elegir entre cualquiera de las opciones de almacenamiento disponibles como almacenamiento adicional.

![Origen de datos de creación de un clúster de Hadoop en HDInsight](./media/use-hdi-cluster/create-storage-account.png)

> [!WARNING]
> No se admite el uso de una cuenta de almacenamiento adicional en una ubicación diferente a la del clúster de HDInsight.

### <a name="use-azure-powershell"></a>Uso de Azure PowerShell

Si ha [instalado y configurado Azure PowerShell][powershell-install], puede usar el código siguiente desde el símbolo del sistema de Azure PowerShell para crear un contenedor y una cuenta de almacenamiento:

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "WEST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Connect-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
      -Name StorageAccountName `
      -Location $Location `
      -SkuName Standard_LRS `
      -Kind StorageV2 
      -HierarchialNamespace $True

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

> [!NOTE]
> Crear un contenedor es sinónimo de crear un sistema de archivos en Azure Data Lake Storage.

### <a name="use-azure-cli"></a>Uso de CLI de Azure

[!INCLUDE [use-latest-version](../../../includes/hdinsight-use-latest-cli.md)]

Si ha [instalado y configurado la CLI Azure](../../cli-install-nodejs.md), se puede usar el comando siguiente para una cuenta de almacenamiento y contenedor.

```bash
az storage account create \
    --name <STORAGE_ACCOUNT_NAME> \
    --resource-group <RESOURCE_GROUP_NAME> \
    --location westus2 \
    --sku Standard_LRS \
    --kind StorageV2 \
    --Enable-hierarchical-namespace true
```

> [!NOTE]
> Durante la versión preliminar pública de Data Lake Storage Gen2 solo se admite `--sku Standard_LRS`.

Se le pide que especifique la región geográfica en la que se crea la cuenta de almacenamiento. Cree la cuenta de almacenamiento en la misma región en la que planea crear el clúster de HDInsight.

Una vez creada la cuenta de almacenamiento, use el siguiente comando para recuperar las claves de la cuenta de almacenamiento:

    azure storage account keys list <STORAGE_ACCOUNT_NAME>

Para crear un contenedor, use el comando siguiente:

    azure storage container create <CONTAINER_NAME> --account-name <STORAGE_ACCOUNT_NAME> --account-key <STORAGE_ACCOUNT_KEY>

> [!NOTE]
> Crear un contenedor es sinónimo de crear un sistema de archivos en Azure Data Lake Storage.

## <a name="address-files-in-azure-storage"></a>Archivos adicionales en Azure Storage

El esquema de URI para acceder a los archivos de Azure Storage desde HDInsight es:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.widows.net/<PATH>

El esquema URI proporciona acceso sin cifrar (con el prefijo *abfs:*) y acceso cifrado SSL (con *abfss*). Se recomienda usar *abfss* siempre que sea posible, incluso cuando se acceda a datos que se encuentren en la misma región de Azure.

* &lt;FILE_SYSTEM_NAME&gt; identifica la ruta de acceso del sistema de archivos de Azure Data Lake Storage.
* &lt;ACCOUNT_NAME&gt; identifica el nombre de la cuenta de Azure Storage. Se necesita el nombre completo de dominio (FQDN).

    Si se han especificado valores para &lt;FILE_SYSTEM_NAME&gt; o &lt;ACCOUNT_NAME&gt;, se utiliza el sistema de archivos predeterminado. Para los archivos del sistema de archivos predeterminado, puede usar una ruta relativa o absoluta. Por ejemplo, se puede hacer referencia al archivo *hadoop-mapreduce-examples.jar* que se incluye con los clústeres de HDInsight usando una de las rutas de acceso siguientes:
    
        abfs://myfilesystempath@myaccount.dfs.core.widows.net/example/jars/hadoop-mapreduce-examples.jar
        abfs:///example/jars/hadoop-mapreduce-examples.jar
        /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> El nombre del archivo es *hadoop-examples.jar* en las versiones 2.1 y 1.6 de los clústeres de HDInsight.

* &lt;PATH&gt; es el nombre de la ruta HDFS del archivo o el directorio.

> [!NOTE]
> Al trabajar con archivos fuera de HDInsight, la mayoría de las utilidades no reconocen el formato ABFS y, en su lugar, esperan un formato básico de ruta de acceso, como `example/jars/hadoop-mapreduce-examples.jar`.
 
## <a name="use-additional-storage-accounts"></a>Uso de cuentas de almacenamiento adicionales

Al crear un clúster de HDInsight, se especifica la cuenta de Azure Storage a la que quiere asociarlo. Además de esta cuenta de almacenamiento, puede agregar otras desde la misma suscripción de Azure o desde otras diferentes tanto durante el proceso de creación como después de que el clúster se haya creado. Para obtener instrucciones sobre cómo agregar más cuentas de almacenamiento, consulte [Creación de clústeres de HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> No se admite el uso de una cuenta de almacenamiento adicional en una ubicación diferente a la del clúster de HDInsight.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a usar Azure Storage compatible con HDFS con HDInsight. Este enfoque le permite crear soluciones de adquisición de datos de archivado escalables y a largo plazo y usar HDInsight para desbloquear la información que hay dentro de los datos estructurados y no estructurados almacenados.

Para más información, consulte:

* [El controlador ABFS Hadoop Filesystem para Azure Data Lake Storage Gen2](abfs-driver.md)
* [Introducción a Azure Data Lake Storage](introduction.md)
* [Configuración de clústeres HDInsight mediante Azure Data Lake Storage con Hadoop, Spark, Kafka y más](quickstart-create-connect-hdi-cluster.md)
* [Ingesta de datos en Azure Data Lake Storage con Distcp](use-distcp.md)

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: ../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/use-hdi-cluster/HDI.PowerShell.BlobCommands.png
