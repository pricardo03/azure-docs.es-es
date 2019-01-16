---
title: 'Tutorial: Aprenda a realizar operaciones de extracción, carga y transferencia con Azure Databricks.'
description: En este tutorial, aprenderá a extraer datos desde Azure Data Lake Storage Gen2 (versión preliminar) a Azure Databricks, transformarlos y luego cargarlos en Azure SQL Data Warehouse.
services: storage
author: jamesbak
ms.service: storage
ms.author: jamesbak
ms.topic: tutorial
ms.date: 12/06/2018
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 6b2812e31174c4e5d61ae9941563e39357de9522
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107096"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Tutorial: Extracción, transformación y carga de datos mediante Azure Databricks

En este tutorial, se utilizará Azure Databricks para realizar una operación de ETL (extracción, transformación y carga de datos). Los datos se mueven de una cuenta de Azure Storage con Azure Data Lake Storage Gen2 habilitado a Azure SQL Data Warehouse.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un área de trabajo de Azure Databricks.
> * Crear un clúster de Spark en Azure Databricks.
> * Crear una cuenta compatible con Azure Data Lake Storage Gen2.
> * Cargar datos en Azure Data Lake Storage Gen2.
> * Crear un cuaderno en Azure Databricks.
> * Extraer datos desde Data Lake Storage Gen2.
> * Transformar datos en Azure Databricks.
> * Cargar datos en Azure SQL Data Warehouse

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

* Cree una instancia de Azure SQL Data Warehouse y una regla de firewall de nivel de servidor y conéctese al servidor como administrador. Siga las instrucciones del artículo [Inicio rápido: Cree una instancia de Azure SQL Data Warehouse](../../sql-data-warehouse/create-data-warehouse-portal.md).
* Cree una clave maestra de base de datos para la instancia de Azure SQL Data Warehouse. Siga las instrucciones que se indican en el artículo [Crear la clave maestra de una base de datos](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key).
* [Cree una cuenta de Azure Data Lake Storage Gen2](data-lake-storage-quickstart-create-account.md).
* Descargue (**small_radio_json.json**) desde el repositorio [U-SQL Examples and Issue Tracking](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) (Ejemplos y seguimiento de problemas de U-SQL) y anote la ruta donde guarde el archivo.
* Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-the-workspace"></a>Creación del área de trabajo

En esta sección, va a crear un área de trabajo de Azure Databricks con Azure Portal.

1. En Azure Portal, seleccione **Crear un recurso** > **Análisis** > **Azure Databricks**.

    ![Databricks en Azure Portal](./media/data-lake-storage-handle-data-using-databricks/azure-databricks-on-portal.png "Databricks en Azure Portal")

1. En **Azure Databricks Service**, proporcione los valores siguientes para crear un área de trabajo de Databricks.

    |Propiedad  |Descripción  |
    |---------|---------|
    |**Workspace name** (Nombre del área de trabajo)     | Proporcione un nombre para el área de trabajo de Databricks.        |
    |**Suscripción**     | En el cuadro desplegable, seleccione la suscripción de Azure.        |
    |**Grupos de recursos**     | Especifique si desea crear un nuevo grupo de recursos o utilizar uno existente. Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. Para más información, consulte [Información general del grupo de recursos de Azure](../../azure-resource-manager/resource-group-overview.md). |
    |**Ubicación**     | Seleccione **Oeste de EE. UU. 2**.        |
    |**Plan de tarifa**     |  Seleccione **Estándar**.     |

    ![Creación de un área de trabajo de Azure Databricks](./media/data-lake-storage-handle-data-using-databricks/create-databricks-workspace.png "Creación de un área de trabajo de Azure Databricks")

1. Seleccione **Anclar al panel** y, después, seleccione **Crear**.

1. La cuenta tarda unos minutos en crearse. Durante la creación de la cuenta, el portal muestra el icono **Enviando implementación para Azure Databricks** a la derecha. Para supervisar el estado de la operación, consulte la barra de progreso en la parte superior.

    ![Icono de implementación de Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-deployment-tile.png "Databricks deployment tile")

## <a name="create-the-spark-cluster"></a>Creación del clúster de Spark

Para realizar las operaciones en este tutorial, necesita un clúster de Spark. Para crear el clúster de Spark, siga estos pasos.

1. En Azure Portal, vaya al área de trabajo de Databricks que ha creado y, después, seleccione **Launch Workspace** (Iniciar área de trabajo).

1. Se le redirigirá al portal de Azure Databricks. En el portal, seleccione **Cluster**.

    ![Databricks en Azure](./media/data-lake-storage-handle-data-using-databricks/databricks-on-azure.png "Databricks en Azure")

1. En la página **Nuevo clúster**, proporcione los valores para crear un clúster.

    ![Creación de un clúster de Databricks Spark en Azure](./media/data-lake-storage-handle-data-using-databricks/create-databricks-spark-cluster.png "Creación de un clúster de Databricks Spark en Azure")

1. Rellene los valores para los campos siguientes y acepte los valores predeterminados para el resto de campos:

    * Escriba un nombre para el clúster.
    * Para este artículo, cree un clúster con el entorno de ejecución **5.1**.
    * Asegúrese de que selecciona la casilla **Terminate after \_\_ minutes of inactivity** (Terminar después de \_\_ minutos de inactividad). Si no se usa el clúster, proporcione una duración (en minutos) para terminar el clúster.

1. Seleccione **Create cluster** (Crear clúster).

Después de ejecutar el clúster, puede asociarle cuadernos y ejecutar trabajos de Spark.

## <a name="create-a-file-system"></a>Creación de un sistema de archivos

Para almacenar datos en la cuenta de almacenamiento Data Lake Storage Gen2, debe crear un sistema de archivos.

En primer lugar, va a crear un cuaderno en el área de trabajo de Azure Databricks y, después, ejecutará fragmentos de código para crear el sistema de archivos en la cuenta de almacenamiento.

1. En [Azure Portal](https://portal.azure.com), vaya al área de trabajo de Azure Databricks que ha creado y, después, seleccione **Launch Workspace** (Iniciar área de trabajo).

1. A la izquierda, seleccione **Workspace** (Área de trabajo). En la lista desplegable **Workspace** (Área de trabajo), seleccione **Create** > **Notebook** (Crear > Cuaderno).

    ![Creación de un cuaderno en Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-create-notebook.png "Create notebook in Databricks")

1. En el cuadro de diálogo **Create Notebook** (Crear cuaderno), escriba un nombre para el cuaderno. Seleccione **Scala** como lenguaje y, a continuación, seleccione el clúster de Spark que creó anteriormente.

    ![Proporcionar detalles para un cuaderno en Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-notebook-details.png "Provide details for a notebook in Databricks")

    Seleccione **Crear**.

1. Escriba el código siguiente en la primera celda del cuaderno y ejecute el código. Reemplace los marcadores de posición que se muestran entre paréntesis en el ejemplo por los suyos propios:

    ```scala
    %python%
    configs = {"fs.azure.account.auth.type": "OAuth",
        "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
        "fs.azure.account.oauth2.client.id": "<service-client-id>",
        "fs.azure.account.oauth2.client.secret": "<service-credentials>",
        "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"}
     
    dbutils.fs.mount(
        source = "abfss://<file-system-name>@<account-name>.dfs.core.windows.net/[<directory-name>]",
        mount_point = "/mnt/<mount-name>",
        extra_configs = configs)
    ```

1. Seleccione las teclas Mayús+Entrar para ejecutar el código.

Ahora el sistema de archivos se crea para la cuenta de almacenamiento.

## <a name="upload-the-sample-data"></a>Cargar los datos de ejemplo

El siguiente paso es cargar un archivo de datos de ejemplo en la cuenta de almacenamiento para transformarlo después en Azure Databricks.

Cargue los datos de ejemplo que ha descargado en la cuenta de almacenamiento. El método que utiliza para cargar datos en la cuenta de almacenamiento difiere dependiendo de si tiene habilitado el espacio de nombres jerárquico.

Puedes usar Azure Data Factory, distp, o AzCopy (versión 10) para hacer la carga. La versión 10 de AzCopy solo está disponible en este momento mediante la versión preliminar. Para usar AzCopy, pegue el código siguiente en una ventana de comandos:

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_PATH>\small_radio_json.json" https://<ACCOUNT_NAME>.dfs.core.windows.net/data --recursive 
```

## <a name="extract-the-data"></a>Extracción de los datos

Para trabajar con los datos de ejemplo en Databricks, necesita extraer los datos de la cuenta de almacenamiento.

Vuelva al cuaderno de Databricks y escriba el código siguiente en una nueva celda del cuaderno.

Agregue el siguiente fragmento de código a una celda de código vacía. Reemplace los marcadores de posición que se muestran entre paréntesis por los valores que ha guardado antes en la cuenta de almacenamiento.

```scala
dbutils.widgets.text("storage_account_name", "STORAGE_ACCOUNT_NAME", "<YOUR_STORAGE_ACCOUNT_NAME>")
dbutils.widgets.text("storage_account_access_key", "YOUR_ACCESS_KEY", "<YOUR_STORAGE_ACCOUNT_SHARED_KEY>")
```

Seleccione las teclas Mayús+Entrar para ejecutar el código.

Ahora puede cargar el archivo JSON de ejemplo como trama de datos en Azure Databricks. Pegue el código siguiente en una nueva celda. Reemplace los marcadores de posición que se muestran entre paréntesis por sus valores.

```scala
val df = spark.read.json("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/data/small_radio_json.json")
```

Seleccione las teclas Mayús+Entrar para ejecutar el código.

Ejecute el código siguiente para ver el contenido de la trama de datos:

```scala
df.show()
```

Debe ver una salida similar al siguiente fragmento de código:

```bash
+---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
|               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
+---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
| El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
| Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
| Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
...
...
```

Ahora ha extraído los datos de Azure Data Lake Storage Gen2 en Azure Databricks.

## <a name="transform-the-data"></a>Transformar los datos

Los datos de ejemplo sin procesar del archivo **small_radio_json.json** capturan la audiencia de una emisora de radio y presentan una variedad de columnas. En esta sección, va a transformar los datos para recuperar solo columnas específicas del conjunto de datos.

En primer lugar, recupere solo las columnas **firstName**, **lastName**, **gender**, **location** y **level** de la trama de datos que ha creado.

```scala
val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
```

Recibirá una salida como la que se muestra en el fragmento de código siguiente:

```bash
+---------+----------+------+--------------------+-----+
|firstname|  lastname|gender|            location|level|
+---------+----------+------+--------------------+-----+
| Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|     Tess|  Townsend|     F|Nashville-Davidso...| free|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
|   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Tess|  Townsend|     F|Nashville-Davidso...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
+---------+----------+------+--------------------+-----+
```

Puede transformar aún más estos datos para renombrar la columna **level** como **subscription_type**.

```scala
val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
renamedColumnsDF.show()
```

Recibirá una salida como la que se muestra en el fragmento de código siguiente.

```bash
+---------+----------+------+--------------------+-----------------+
|firstname|  lastname|gender|            location|subscription_type|
+---------+----------+------+--------------------+-----------------+
| Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|     Tess|  Townsend|     F|Nashville-Davidso...|             free|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
|   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Tess|  Townsend|     F|Nashville-Davidso...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
+---------+----------+------+--------------------+-----------------+
```

## <a name="load-the-data"></a>Carga de los datos

En esta sección, cargará los datos transformados en Azure SQL Data Warehouse. Utilice el conector de Azure SQL Data Warehouse para Azure Databricks para cargar directamente una trama de datos como una tabla en SQL Data Warehouse.

El conector de SQL Data Warehouse usa Azure Blob Storage como almacenamiento temporal para cargar datos entre Azure Databricks y Azure SQL Data Warehouse. Por lo tanto, para comenzar, proporcione la configuración para conectarse a la cuenta de almacenamiento. Ya debe haber creado la cuenta como parte de los requisitos previos de este artículo.

Proporcione la configuración para acceder a la cuenta de Azure Storage desde Azure Databricks.

```scala
val storageURI = "<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net"
val fileSystemName = "<FILE_SYSTEM_NAME>"
val accessKey =  "<ACCESS_KEY>"
```

Especifique una carpeta temporal que usará al mover datos entre Azure Databricks y Azure SQL Data Warehouse.

```scala
val tempDir = "abfs://" + fileSystemName + "@" + storageURI +"/tempDirs"
```

Ejecute el siguiente fragmento de código para almacenar las claves de acceso de Azure Blob Storage en la configuración. Esta acción garantiza que no tiene que guardar la clave de acceso en el cuaderno en texto sin formato.

```scala
val acntInfo = "fs.azure.account.key."+ storageURI
sc.hadoopConfiguration.set(acntInfo, accessKey)
```

Proporcione los valores para conectarse a la instancia de Azure SQL Data Warehouse. Debe haber creado una instancia de SQL Data Warehouse como requisito previo.

```scala
//SQL Data Warehouse related settings
val dwDatabase = "<DATABASE NAME>"
val dwServer = "<DATABASE SERVER NAME>" 
val dwUser = "<USER NAME>"
val dwPass = "<PASSWORD>"
val dwJdbcPort =  "1433"
val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
```

Ejecute el siguiente fragmento de código para cargar la trama de datos transformada, **renamedColumnsDf**, como una tabla en SQL Data Warehouse. Este fragmento de código crea una tabla denominada **SampleTable** en SQL Database.

```scala
spark.conf.set(
    "spark.sql.parquet.writeLegacyFormat",
    "true")
    
renamedColumnsDF.write
    .format("com.databricks.spark.sqldw")
    .option("url", sqlDwUrlSmall) 
    .option("dbtable", "SampleTable")
    .option( "forward_spark_azure_storage_credentials","True")
    .option("tempdir", tempDir)
    .mode("overwrite")
    .save()
```

Conéctese a la instancia de SQL Database y compruebe que aparece la base de datos denominada **SampleTable**.

![Comprobación de la tabla de ejemplo](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table.png "Verify sample table")

Ejecute una consulta select para comprobar el contenido de la tabla. La tabla debe tener los mismos datos que la trama de datos **renamedColumnsDF**.

![Comprobación del contenido de tabla de ejemplo](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table-content.png "Verify the sample table content")

## <a name="clean-up-resources"></a>Limpieza de recursos

Después de terminar el tutorial, puede finalizar el clúster. Desde el área de trabajo de Azure Databricks, seleccione **Clusters** (Clústeres) a la izquierda. Para que el clúster termine, en **Acciones**, apunte a los puntos suspensivos (...) y seleccione el icono **Terminate** (Finalizar).

![Detener un clúster de Databricks](./media/data-lake-storage-handle-data-using-databricks/terminate-databricks-cluster.png "Stop a Databricks cluster")

Si no finaliza manualmente el clúster, este se detendrá automáticamente si seleccionó la casilla **Terminate after \_\_ minutes of inactivity** (Finalizar después de \_\_ minutos de inactividad) al crear el clúster. En tal caso, el clúster se detiene automáticamente si ha estado inactivo durante el tiempo especificado.

## <a name="next-steps"></a>Pasos siguientes

Vaya al siguiente tutorial para aprender sobre el streaming de datos en tiempo real en Azure Databricks mediante Azure Event Hubs.

> [!div class="nextstepaction"]
>[Streaming de datos a Azure Databricks con Event Hubs](../../azure-databricks/databricks-stream-from-eventhubs.md)
