---
title: Use Azure Data Box para migrar datos desde un entorno local HDFS almacenar en Azure Storage
description: Migrar datos desde un almacén HDFS local a Azure Storage
services: storage
author: normesta
ms.service: storage
ms.date: 06/05/2019
ms.author: normesta
ms.topic: article
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 9a42135df38cde91cc6626a3f7d0328334af0a5d
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729068"
---
# <a name="use-azure-data-box-to-migrate-data-from-an-on-premises-hdfs-store-to-azure-storage"></a>Usar Azure Data Box para migrar datos desde un almacén HDFS local a Azure Storage

Puede migrar datos desde un almacén HDFS local de su clúster de Hadoop en Azure Storage (blob storage o Data Lake Storage Gen2) mediante el uso de un dispositivo Data Box. Puede elegir desde un cuadro 80 TB de datos o un gruesa de datos de 770 TB.

En este artículo le ayudará a completar estas tareas:

:heavy_check_mark: Copiar los datos en un cuadro de datos o un dispositivo de datos cuadro pesado.

:heavy_check_mark: Devuelva el dispositivo a Microsoft.

:heavy_check_mark: Mover los datos a la cuenta de almacenamiento de Data Lake Storage Gen2.

## <a name="prerequisites"></a>Requisitos previos

Necesita estas cosas para completar la migración.

* Cuenta de Azure Storage que **no** tienen espacios de nombres jerárquico habilitado en ella.

* Si desea usar la cuenta de Azure Data Lake Storage Gen2 (cuenta de almacenamiento que **does** tienen espacios de nombres jerárquico habilitado), tal vez le interese crearlo en este momento.

* Un clúster de Hadoop local que contiene los datos de origen.

* Un [dispositivo Azure Data Box](https://azure.microsoft.com/services/storage/databox/).

    - [Ordenar los datos de cuadro](https://docs.microsoft.com/azure/databox/data-box-deploy-ordered) o [datos gruesa](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-ordered). Durante la ordenación del dispositivo, recuerde que debe elegir un almacenamiento de la cuenta que **no** tienen espacios de nombres jerárquico habilitado en ella. Esto es porque los dispositivos del cuadro de datos aún no admiten la ingesta directa en Azure Data Lake Storage Gen2. Deberá copiar en una cuenta de almacenamiento y, a continuación, realice una segunda copia en la cuenta de ADLS Gen2. Se proporcionan instrucciones para esto en los pasos siguientes.
    - Conectar los cables y conectar su [Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) o [intensivo de datos de cuadro](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-set-up) a una red local.

Si está preparado, empecemos.

## <a name="copy-your-data-to-a-data-box-device"></a>Copiar los datos en un dispositivo Data Box

Para copiar los datos desde el almacén HDFS local a un dispositivo Data Box, deberá configurar algunas cosas y, a continuación, utilice el [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) herramienta.

Si la cantidad de datos que está copiando es mayor que la capacidad de un único cuadro de datos o del nodo único en el cuadro intensivo de datos, dividir el conjunto de datos en tamaños de los que caben en los dispositivos.

Siga estos pasos para copiar datos mediante el almacenamiento de las API REST de Blob/objeto en el dispositivo Data Box. La interfaz API de REST hará que el dispositivo aparezca como un almacén de HDFS para el clúster. 


1. Antes de copiar los datos a través de REST, identifique las primitivas de seguridad y de conexión para conectarse a la interfaz REST en el cuadro de datos o el cuadro intensivo de datos. Inicie sesión en la interfaz de usuario de Data Box de web local y vaya a **Connect y copie** página. En el almacenamiento de Azure de la cuenta para el dispositivo, en **tener acceso a configuración**, busque y seleccione **REST**.

    ![Página "Conectar y copiar"](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connect-rest.png)

2. En la cuenta de almacenamiento de acceso y el cuadro de diálogo de carga de datos, copiar el **extremo de Blob service** y **clave de cuenta de almacenamiento**. Desde el punto de conexión de servicio de blob, omita el `https://` y la barra diagonal final.

    En este caso, es el punto de conexión: `https://mystorageaccount.blob.mydataboxno.microsoftdatabox.com/`. La parte del host del URI que va a usar es: `mystorageaccount.blob.mydataboxno.microsoftdatabox.com`. Para obtener un ejemplo, vea cómo [conectar con REST sobre http](/azure/databox/data-box-deploy-copy-data-via-rest). 

     ![Cuadro de diálogo "Obtener acceso a la cuenta de almacenamiento y carga de datos"](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connection-string-http.png)

3. Agregar el punto de conexión y el cuadro de datos o intensivo de datos de cuadro de dirección IP del nodo a `/etc/hosts` en cada nodo.

    ```    
    10.128.5.42  mystorageaccount.blob.mydataboxno.microsoftdatabox.com
    ```
    Si usas algún otro mecanismo para DNS, debe asegurarse de que el cuadro de datos se puede resolver el punto de conexión.
    
4. Establecer una variable del shell `azjars` para que apunte a la `hadoop-azure` y `microsoft-windowsazure-storage-sdk` archivos jar. Estos archivos están bajo el directorio de instalación de Hadoop (puede comprobar si estos archivos se encuentran con este comando `ls -l $<hadoop_install_dir>/share/hadoop/tools/lib/ | grep azure` donde `<hadoop_install_dir>` es el directorio donde ha instalado Hadoop) usa las rutas de acceso completas. 
    
    ```
    # azjars=$hadoop_install_dir/share/hadoop/tools/lib/hadoop-azure-2.6.0-cdh5.14.0.jar
    # azjars=$azjars,$hadoop_install_dir/share/hadoop/tools/lib/microsoft-windowsazure-storage-sdk-0.6.0.jar
    ```

5. Cree el contenedor de almacenamiento que desea usar para la copia de datos. También debe especificar una carpeta de destino como parte de este comando. En este punto podría tratarse de una carpeta de destino ficticio.

    ```
    # hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -mkdir -p  wasb://[container_name]@[blob_service_endpoint]/[destination_folder]
    ```

6. Ejecutar un comando de lista para asegurarse de que se han creado el contenedor y la carpeta.

    ```
    # hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -ls -R  wasb://[container_name]@[blob_service_endpoint]/
    ```

7. Copiar datos desde Hadoop HDFS al almacenamiento de blobs de cuadro de datos, en el contenedor que creó anteriormente. Si no se encuentra la carpeta que se va a copiar en, el comando lo crea automáticamente.

    ```
    # hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -strategy dynamic -m 4 -update \
    [source_directory] \
           wasb://[container_name]@[blob_service_endpoint]/[destination_folder]       
    ```
   El `-libjars` opción se utiliza para realizar la `hadoop-azure*.jar` y dependiente `azure-storage*.jar` archivos estén disponibles para `distcp`. Esto puede ocurrir ya para algunos clústeres.
   
   El ejemplo siguiente se muestra cómo el `distcp` comando se usa para copiar los datos.
   
   ```
   # hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.mystorageaccount.blob.mydataboxno.microsoftdatabox.com=myaccountkey \
    -strategy dynamic -m 4 -update \
    /data/testfiles \
    wasb://hdfscontainer@mystorageaccount.blob.mydataboxno.microsoftdatabox.com/testfiles
   ```
  
Para mejorar la velocidad de copia:
- Intente cambiar el número de asignadores. (El ejemplo anterior usa `m` = 4 asignadores.)
- Intente ejecutar varios `distcp` en paralelo.
- Recuerde que los archivos grandes es mejor que los archivos pequeños.
    
## <a name="ship-the-data-box-to-microsoft"></a>El cuadro de datos a Microsoft de envío

Siga estos pasos para preparar y enviar el dispositivo Data Box a Microsoft.

1. Una vez completada la copia de datos, ejecute:
    
    - [Preparación para el envío en el cuadro de datos o intensivo de datos de cuadro](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest).
    - Una vez completada la preparación del dispositivo, descargue los archivos de la lista de materiales. Usará estos BOM o los archivos más adelante para comprobar los datos cargados en Azure de manifiesto. 
    - Apagar el dispositivo y quite los cables.
2.  Póngase en contacto con UPS para programar la recogida del dispositivo. Siga las instrucciones para:

    - [Envíe su Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up) 
    - [Envíe su datos gruesa](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-picked-up).
3.  Después de que Microsoft recibe el dispositivo, está conectado a la red de centros de datos y se cargan los datos a la cuenta de almacenamiento especificada (con espacios de nombres jerárquico deshabilitado) cuando se realizó el pedido de dispositivo. Comprobar los archivos de la lista de materiales que todos los datos se carga en Azure. Ahora puede pasar estos datos a una cuenta de almacenamiento de Data Lake Storage Gen2.


## <a name="move-the-data-onto-your-data-lake-storage-gen2-storage-account"></a>Mover los datos a la cuenta de almacenamiento de Data Lake Storage Gen2

Este paso es necesario si usa Azure Data Lake Storage Gen2 como almacén de datos. Si usas solo una cuenta de almacenamiento de blobs sin espacio de nombres jerárquico como almacén de datos, no es necesario realizar este paso.

Puede hacerlo de dos maneras.

- Use [Azure Data Factory para mover datos a ADLS Gen2](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2). Tendrá que especificar **Azure Blob Storage** como el origen.

- Use el clúster de Hadoop basados en Azure. Puede ejecutar este comando DistCp:

    ```bash
    hadoop distcp -Dfs.azure.account.key.{source_account}.dfs.windows.net={source_account_key} abfs://{source_container} @{source_account}.dfs.windows.net/[path] abfs://{dest_container}@{dest_account}.dfs.windows.net/[path]
    ```

Este comando copia los datos y metadatos de la cuenta de almacenamiento en su cuenta de almacenamiento de Data Lake Storage Gen2.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo funciona Data Lake Storage Gen2 con clústeres de HDInsight. Consulte [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md).
