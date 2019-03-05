---
title: 'Tutorial: Acceso a los datos de Azure Data Lake Storage Gen2 con Azure Databricks mediante Spark | Microsoft Docs'
description: Este tutorial muestra cómo ejecutar consultas de Spark en un clúster de Azure Databricks para acceder a los datos de una cuenta de almacenamiento de Azure Data Lake Storage Gen2.
services: storage
author: dineshmurthy
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 01/29/2019
ms.author: dineshm
ms.openlocfilehash: b5d7be25ba18e256352d8793689bcb63a013e20b
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56452615"
---
# <a name="tutorial-access-data-lake-storage-gen2-data-with-azure-databricks-using-spark"></a>Tutorial: Acceso a los datos de Azure Data Lake Storage Gen2 con Azure Databricks mediante Spark

Este tutorial muestra cómo conectarse con un clúster de Azure Databricks para consultar los datos almacenados en una cuenta de almacenamiento de Azure que tiene Azure Data Lake Storage Gen2 habilitado. Esta conexión le permite ejecutar forma nativa las consultas y los análisis del clúster en los datos.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Creación de un clúster de Databricks
> * Ingesta de datos no estructurados en una cuenta de almacenamiento
> * Ejecución de análisis en los datos de almacenamiento de blobs

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Cree una cuenta de Azure Data Lake Storage Gen2

  Consulte [Creación de una cuenta de Azure Data Lake Storage Gen2](data-lake-storage-quickstart-create-account.md).

* Asegúrese de que la cuenta de usuario tiene asignado el rol [Colaborador de datos de Storage Blob](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac).

* Instale AzCopy v10. Consulte [Transferencia de datos con AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

*  Crear una entidad de servicio. Consulte [Configuración de los portal para crear una aplicación de Azure AD y una entidad de servicio que puedan acceder a los recursos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

   Hay un par de cosas que tendrá que hacer cuando realice los pasos de este artículo.

   :heavy_check_mark: Al realizar los pasos que se describen en la sección [Asignación de la aplicación a un rol](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role), asegúrese de asignar el rol de **Colaborador de datos de blobs de almacenamiento** a la entidad de servicio.

   > [!IMPORTANT]
   > Asegúrese de asignar el rol en el ámbito de la cuenta de almacenamiento de Data Lake Storage Gen2. Puede asignar un rol al grupo de recursos o suscripción primario, pero recibirá errores relacionados con los permisos hasta que esas asignaciones de roles se propaguen a la cuenta de almacenamiento.

   :heavy_check_mark: Al realizar los pasos que se describen en la sección [Obtención de valores para el inicio de sesión](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) del artículo, pegue el identificador del inquilino, el identificador de la aplicación y los valores de la clave de autenticación en una clave de texto, ya que los necesitará pronto.

### <a name="download-the-flight-data"></a>Descarga de los datos de vuelo

Este tutorial se utilizan datos de vuelo de Bureau of Transportation Statistics para demostrar cómo realizar una operación de ETL. Debe descargar estos datos para completar el tutorial.

1. Vaya a [Research and Innovative Technology Administration, Bureau of Transportation Statistics](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time).

2. Active la casilla **Prezipped File** (Archivo precomprimido) para seleccionar todos los campos de datos.

3. Seleccione el botón **Download** (Descargar) y guarde los resultados en el equipo. 

4. Descomprima el contenido del archivo comprimido y anote el nombre y la ruta de acceso del archivo. Necesitará esta información en pasos posteriores.

## <a name="create-an-azure-databricks-service"></a>Creación de un servicio de Azure Databricks

En esta sección, va a crear un servicio de Azure Databricks con Azure Portal.

1. En Azure Portal, seleccione **Crear un recurso** > **Análisis** > **Azure Databricks**.

    ![Databricks en Azure Portal](./media/data-lake-storage-use-databricks-spark/azure-databricks-on-portal.png "Databricks en Azure Portal")

2. En **Servicio de Azure Databricks**, proporcione los valores siguientes para crear un servicio de Databricks:

    |Propiedad  |Descripción  |
    |---------|---------|
    |**Workspace name** (Nombre del área de trabajo)     | Proporcione un nombre para el área de trabajo de Databricks.  |
    |**Suscripción**     | En el cuadro desplegable, seleccione la suscripción de Azure.        |
    |**Grupos de recursos**     | Especifique si desea crear un nuevo grupo de recursos o utilizar uno existente. Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. Para más información, consulte [Información general del grupo de recursos de Azure](../../azure-resource-manager/resource-group-overview.md). |
    |**Ubicación**     | Seleccione **Oeste de EE. UU. 2**. Para otras regiones disponibles, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services/).       |
    |**Plan de tarifa**     |  Seleccione **Estándar**.     |

    ![Creación de un área de trabajo de Azure Databricks](./media/data-lake-storage-use-databricks-spark/create-databricks-workspace.png "Create an Azure Databricks service")

3. Seleccione **Anclar al panel** y, después, seleccione **Crear**.

4. La cuenta tarda unos minutos en crearse. Durante la creación de la cuenta, el portal muestra el icono **Enviando implementación para Azure Databricks** a la derecha. Para supervisar el estado de la operación, consulte la barra de progreso en la parte superior.

    ![Icono de implementación de Databricks](./media/data-lake-storage-use-databricks-spark/databricks-deployment-tile.png "Databricks deployment tile")

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Creación de un clúster de Spark en Azure Databricks

1. En Azure Portal, vaya al servicio de Databricks que ha creado y, después, seleccione **Launch Workspace** (Iniciar área de trabajo).

2. Se le redirigirá al portal de Azure Databricks. En el portal, seleccione **Cluster**.

    ![Databricks en Azure](./media/data-lake-storage-use-databricks-spark/databricks-on-azure.png "Databricks en Azure")

3. En la página **Nuevo clúster**, proporcione los valores para crear un clúster.

    ![Creación de un clúster de Databricks Spark en Azure](./media/data-lake-storage-use-databricks-spark/create-databricks-spark-cluster.png "Creación de un clúster de Databricks Spark en Azure")

4. Rellene los valores para los campos siguientes y acepte los valores predeterminados para el resto de campos:

    * Escriba un nombre para el clúster.

    * Para este artículo, cree un clúster con el entorno de ejecución **5.1**.

    * Asegúrese de que selecciona la casilla **Terminate after \_\_ minutes of inactivity** (Terminar después de \_\_ minutos de inactividad). Si no se usa el clúster, proporcione una duración (en minutos) para terminar el clúster.

    * Seleccione **Create cluster** (Crear clúster). Después de ejecutar el clúster, puede asociarle cuadernos y ejecutar trabajos de Spark.

## <a name="create-a-file-system-and-mount-it"></a>Creación de un sistema de archivos y su montaje

En esta sección, va a crear un sistema de archivos y una carpeta en la cuenta de almacenamiento.

1. En [Azure Portal](https://portal.azure.com), vaya al servicio de Azure Databricks que ha creado y, después, seleccione **Launch Workspace** (Iniciar área de trabajo).

2. A la izquierda, seleccione **Workspace** (Área de trabajo). En la lista desplegable **Workspace** (Área de trabajo), seleccione **Create** > **Notebook** (Crear > Cuaderno).

    ![Creación de un cuaderno en Databricks](./media/data-lake-storage-use-databricks-spark/databricks-create-notebook.png "Create notebook in Databricks")

3. En el cuadro de diálogo **Create Notebook** (Crear cuaderno), escriba un nombre para el cuaderno. Seleccione **Python** como lenguaje y, a continuación, seleccione el clúster de Spark que creó anteriormente.

4. Seleccione **Crear**.

5. Copie y pegue el siguiente bloque de código en la primera celda, pero no ejecute el código aún.

    ```Python
    configs = {"fs.azure.account.auth.type": "OAuth",
           "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
           "fs.azure.account.oauth2.client.id": "<application-id>",
           "fs.azure.account.oauth2.client.secret": "<authentication-id>",
           "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token",
           "fs.azure.createRemoteFileSystemDuringInitialization": "true"}

    dbutils.fs.mount(
    source = "abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/folder1",
    mount_point = "/mnt/flightdata",
    extra_configs = configs)
    ```

18. En este bloque de código, reemplace los valores de marcador de posición `application-id`, `authentication-id`, `tenant-id` y `storage-account-name` por los valores que recopiló al completar los requisitos previos de este tutorial. Reemplace el valor del marcador de posición `file-system-name` por el nombre que desea dar al sistema de archivos.

   * `application-id` y `authentication-id` proceden de la aplicación que registró con Active Directory como parte de la creación de una entidad de servicio.

   * `tenant-id` procede de su suscripción.

   * `storage-account-name` es el nombre de la cuenta de almacenamiento de Azure Data Lake Storage Gen2.

19. Presione las teclas **MAYÚS + ENTRAR** para ejecutar el código de este bloque.

    Mantenga este cuaderno abierto ya que le agregará comandos más tarde.

## <a name="ingest-data"></a>Introducción de datos

### <a name="copy-source-data-into-the-storage-account"></a>Copia de los datos de origen en la cuenta de almacenamiento

Use AzCopy para copiar datos del archivo *.csv* en su cuenta de Data Lake Storage Gen2.

1. Abra una ventana del símbolo del sistema y escriba el siguiente comando para iniciar sesión en la cuenta de almacenamiento.

   ```bash
   azcopy login
   ```

   Siga las instrucciones que aparecen en la ventana del símbolo del sistema para autenticar la cuenta de usuario.

2. Para copiar datos de la cuenta *.csv*, escriba el siguiente comando.

   ```bash
   azcopy cp "<csv-folder-path>" https://<storage-account-name>.dfs.core.windows.net/<file-system-name>/folder1/On_Time
   ```
   * Reemplace el valor del marcador de posición `<csv-folder-path>` por la ruta de acceso de directorio al archivo *.csv* (excluido el nombre del archivo).

   * Reemplace el valor de marcador de posición `storage-account-name` por el nombre de la cuenta de almacenamiento.

   * Reemplace el marcador de posición `file-system-name` por cualquier nombre que desee asignar al sistema de archivos.

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Uso de Databricks Notebook para convertir CSV en Parquet

En el cuaderno que creó previamente, agregue una nueva celda y pegue el siguiente código en esa celda. Reemplace el valor del marcador de posición `storage-account-name` en este fragmento de código por el nombre de la carpeta en la que ha guardado el archivo csv.

```python
# Use the previously established DBFS mount point to read the data.
# create a data frame to read data.

flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time/<your-folder-name>/*.csv")

# read the airline csv file and write the output to parquet format for easy query.
 flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
 print("Done")
 ```

## <a name="explore-data"></a>Exploración de los datos

En una nueva celda, pegue el código siguiente para obtener una lista de archivos CSV cargados a través de AzCopy. Reemplace el valor del marcador de posición `<csv-folder-path>` por el mismo valor del marcador de posición que ha usado anteriormente.

```python
import os.path
import IPython
from pyspark.sql import SQLContext
display(dbutils.fs.ls("/mnt/flightdata/On_Time/<your-folder-name>"))
```

Para crear un nuevo archivo y enumerar los archivos de la carpeta *parquet/flights*, ejecute este script:

```python
dbutils.fs.put("/mnt/flightdata/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/parquet/flights")
```

Con estos ejemplos de código, ha explorado la naturaleza jerárquica de HDFS usando datos almacenados en una cuenta de almacenamiento con Azure Data Lake Storage Gen2 habilitado.

## <a name="query-the-data"></a>Consulta de los datos

A continuación, puede empezar a consultar los datos cargados en la cuenta de almacenamiento. Escriba cada uno de los siguientes bloques de código en **Cmd 1** y presione **Cmd + ENTRAR** para ejecutar el script de Python.

Para crear tramas de datos para los orígenes de datos, ejecute el script siguiente:

* Reemplace el valor del marcador de posición `<csv-folder-path>` por la ruta de acceso de directorio al archivo *.csv* (excluido el nombre del archivo).

* Reemplace el valor de marcador de posición `<your-csv-file-name` por el nombre del archivo *csv*.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time/<your-folder-name>/<your-csv-file-name>.csv")
acDF.write.parquet('/mnt/flightdata/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created earlier
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load("/mnt/flightdata/parquet/flights")

#print the schema of the dataframes
acDF.printSchema()
flightDF.printSchema()

#print the flight database size
print("Number of flights in the database: ", flightDF.count())

#show the first 20 rows (20 is the default)
#to show the first n rows, run: df.show(n)
acDF.show(100, False)
flightDF.show(20, False)

#Display to run visualizations
#preferably run this in a separate cmd cell
display(flightDF)
```

Introduzca este script para ejecutar algunas consultas de análisis básico en los datos.

```python
#Run each of these queries, preferably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet('/mnt/flightdata/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet('/mnt/flightdata/parquet/airlinecodes')
AirlineCodes.createOrReplaceTempView('AirlineCodes')

#using spark sql, query the parquet file to return total flights in January and February 2016
out1 = spark.sql("SELECT * FROM FlightTable WHERE Month=1 and Year= 2016")
NumJan2016Flights = out1.count()
out2 = spark.sql("SELECT * FROM FlightTable WHERE Month=2 and Year= 2016")
NumFeb2016Flights=out2.count()
print("Jan 2016: ", NumJan2016Flights," Feb 2016: ",NumFeb2016Flights)
Total= NumJan2016Flights+NumFeb2016Flights
print("Total flights combined: ", Total)

# List out all the airports in Texas
out = spark.sql("SELECT distinct(OriginCityName) FROM FlightTable where OriginStateName = 'Texas'") 
print('Airports in Texas: ', out.show(100))

#find all airlines that fly from Texas
out1 = spark.sql("SELECT distinct(Reporting_Airline) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos y todos los recursos relacionados. Para ello, seleccione el grupo de recursos de la cuenta de almacenamiento y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

[!div class="nextstepaction"] 
> [Extracción, transformación y carga de datos mediante Apache Hive en Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)
