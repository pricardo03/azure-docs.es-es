---
title: 'Tutorial: Acceso a los datos de Azure Data Lake Storage Gen2 (versión preliminar) con Azure Databricks mediante Spark | Microsoft Docs'
description: Este tutorial muestra cómo ejecutar consultas de Spark en un clúster de Azure Databricks para acceder a los datos de una cuenta de almacenamiento de Azure Data Lake Storage Gen2.
services: storage
author: dineshmurthy
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: dineshm
ms.openlocfilehash: 31d18d7ea4ee195f7ffcfa04fb247b5dfd525c6a
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251492"
---
# <a name="tutorial-access-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Tutorial: Acceso a los datos de Data Lake Storage Gen2 (versión preliminar) con Azure Databricks mediante Spark

Este tutorial muestra cómo conectarse con un clúster de Azure Databricks para consultar los datos de una cuenta de almacenamiento de Azure que tiene Azure Data Lake Storage Gen2 (versión preliminar) habilitado. Esta conexión le permite ejecutar forma nativa las consultas y los análisis del clúster en los datos.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Creación de un clúster de Databricks
> * Ingesta de datos no estructurados en una cuenta de almacenamiento
> * Ejecución de análisis en los datos de almacenamiento de blobs

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se muestra cómo consumir y consultar los datos de los vuelos de las líneas aéreas, que están disponibles en el [Departamento de Transporte de Estados Unidos](https://transtats.bts.gov/DL_SelectFields.asp). 

1. Active la casilla **Prezipped file** (Archivo precomprimido) para seleccionar todos los campos de datos.
2. Seleccione **Descargar** y guarde los resultados en el equipo.
3. Anote el nombre del archivo y la ruta de acceso de la descarga; necesitará esta información en un paso posterior.

Para completar este tutorial, necesita una cuenta de almacenamiento con funcionalidades de análisis. Para crear uno, se recomienda completar nuestro [inicio rápido](data-lake-storage-quickstart-create-account.md) sobre el tema. 

## <a name="set-aside-storage-account-configuration"></a>Reserva de la configuración de la cuenta de almacenamiento

Necesitará el nombre de la cuenta de almacenamiento y un identificador URI del punto de conexión del sistema de archivos.

Para obtener el nombre de la cuenta de almacenamiento en Azure Portal, elija **Todos los servicios** y filtre por el término *almacenamiento*. Luego, seleccione **Cuentas de almacenamiento** y localice su cuenta de almacenamiento.

Para obtener el identificador URI del punto de conexión del sistema de archivos, elija **Propiedades**y en el panel Propiedades, busque el valor del campo **Primary ADLS FILE SYSTEM ENDPOINT** (PUNTO DE CONEXIÓN DEL SISTEMA DE ARCHIVOS ADLS principal).

Pegue los dos valores en un archivo de texto, ya que los necesitará pronto.

<a id="service-principal"/>

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Cree a una entidad de servicio siguiendo la guía de este tema: [Uso de portal para crear una aplicación de Azure AD y una entidad de servicio que puedan acceder a los recursos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Hay varias acciones concretas que tendrá que hacer al realizar los pasos de este artículo.

:heavy_check_mark: Al realizar los pasos que se describen en la sección [Creación de una aplicación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application) del artículo, asegúrese de que en el campo **URL de inicio de sesión** del cuadro de diálogo **Crear** selecciona el identificador URI del punto de conexión que acaba de recopilar.

:heavy_check_mark: Al realizar los pasos que se describen en la sección [Asignación de la aplicación a un rol](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role), asegúrese de asignar la aplicación al **rol de colaborador de Blob Storage**.

:heavy_check_mark: Al realizar los pasos que se describen en la sección [Obtención de valores para el inicio de sesión](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) del artículo, pegue el identificador del inquilino, el identificador de la aplicación y los valores de la clave de autenticación en una clave de texto, ya que los necesitará pronto.

## <a name="create-a-databricks-cluster"></a>Creación de un clúster de Databricks

El paso siguiente consiste en crear un clúster de Databricks para crear un área de trabajo de datos.

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso**.
2. Escriba **Azure Databricks** en el campo de búsqueda.
3. Seleccione **Crear** en la hoja de Azure Databricks.
4. Asigne el nombre **myFlightDataService** al servicio (asegúrese de activar la casilla *Anclar al panel* al crear el servicio).
5. Seleccione **Launch Workspace** (Iniciar área de trabajo) para abrir el área de trabajo en una nueva ventana del explorador.
6. Seleccione **Clusters** (Clústeres) en la barra de navegación de la izquierda.
7. Seleccione **Create cluster** (Crear clúster).
8. Escriba el nombre **myFlightDataCluster** en el campo **Cluster name** (Nombre del clúster).
9. Seleccione **Standard_D8s_v3** en el campo **Worker Type** (Tipo de trabajo).
10. Cambie el valor de **Min Workers** (Trabajos mínimos) en **4**.
11. Seleccione **Create Cluster** (Crear clúster) en la parte superior de la página. (Este proceso puede tardar hasta 5 minutos).
12. Una vez completado el proceso, seleccione **Azure Databricks** en la parte superior izquierda de la barra de navegación.
13. Seleccione **Notebook** en la sección **Nuevo** en la mitad inferior de la página.
14. Escriba el nombre que desee en el campo **Nombre** y seleccione **Python** como lenguaje.
15. Todos los demás campos pueden dejarse con los valores predeterminados.
16. Seleccione **Crear**.
17. Copie y pegue el siguiente bloque de código en la primera celda, pero no ejecute el código aún.

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
18. En este bloque de código, reemplace los valores `storage-account-name`, `application-id`, `authentication-id` y `tenant-id` del marcador de posición de este bloque de código por los valores que recopiló al completar los pasos de las secciones [Reserva de la configuración de la cuenta de almacenamiento](#config) y [Creación de una entidad de servicio](#service-principal) de este artículo. Reemplace el marcador de posición `file-system-name` por cualquier nombre que desee asignar al sistema de archivos.

19. Presione las teclas **MAYÚS + ENTRAR** para ejecutar el código de este bloque.

## <a name="ingest-data"></a>Introducción de datos

### <a name="copy-source-data-into-the-storage-account"></a>Copia de los datos de origen en la cuenta de almacenamiento

La siguiente tarea consiste en utilizar AzCopy para copiar datos desde el archivo *.csv* en el almacenamiento de Azure. Abra una ventana del símbolo del sistema y escriba los siguientes comandos. Asegúrese de reemplazar los marcadores de posición `<DOWNLOAD_FILE_PATH>`, `<ACCOUNT_NAME>` y `<ACCOUNT_KEY>` por los valores correspondientes que se reservaron en un paso anterior.

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_FILE_PATH>" https://<ACCOUNT_NAME>.dfs.core.windows.net/dbricks/folder1/On_Time --recursive 
```

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Uso de Databricks Notebook para convertir CSV en Parquet

Vuelva a abrir Databricks en el explorador y ejecute los siguientes pasos:

1. Seleccione **Azure Databricks** en la parte superior izquierda de la barra de navegación.
2. Seleccione **Notebook** en la sección **Nuevo** en la mitad inferior de la página.
3. Escriba **CSV2Parquet** en el campo **Nombre**.
4. Todos los demás campos pueden dejarse con los valores predeterminados.
5. Seleccione **Crear**.
6. Pegue el código siguiente en la celda **Cmd 1**. (Este código se guarda automáticamente en el editor).

    ```python
    # Use the previously established DBFS mount point to read the data
    # create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time_On_Time*.csv")
    # read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
    print("Done")
    ```

## <a name="explore-data"></a>Exploración de los datos

Vuelva al área de trabajo de Databricks y seleccione el icono **Recent** (Recientes) en la barra de navegación izquierda.

1. Seleccione el cuaderno **Flight Data Analytics** (Análisis de datos de vuelo).
2. Presione **Ctrl + Alt + N** para crear una nueva celda.

Escriba cada uno de los siguientes bloques de código en **Cmd 1** y presione **Cmd + ENTRAR** para ejecutar el script de Python.

Para obtener una lista de archivos CSV cargados a través de AzCopy, ejecute el script siguiente:

```python
import os.path
import IPython
from pyspark.sql import SQLContext
display(dbutils.fs.ls("/mnt/flightdata/temp/"))
```

Para crear un nuevo archivo y enumerar los archivos de la carpeta *parquet/flights*, ejecute este script:

```python
dbutils.fs.put("/mnt/flightdata/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/temp/parquet/flights")
```

Con estos ejemplos de código, ha explorado la naturaleza jerárquica de HDFS usando datos almacenados en una cuenta de almacenamiento con Azure Data Lake Storage Gen2 habilitado.

## <a name="query-the-data"></a>Consulta de los datos

A continuación, puede empezar a consultar los datos cargados en la cuenta de almacenamiento. Escriba cada uno de los siguientes bloques de código en **Cmd 1** y presione **Cmd + ENTRAR** para ejecutar el script de Python.

### <a name="run-simple-queries"></a>Ejecución de consultas sencillas

Para crear tramas de datos para los orígenes de datos, ejecute el script siguiente:

> [!IMPORTANT]
> Asegúrese de reemplazar el marcador de posición **<SU_NOMBRE_ARCHIVO_CSV>** por el nombre del archivo descargado al principio de este tutorial.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/<YOUR_CSV_FILE_NAME>.csv")
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

Para ejecutar las consultas de análisis con los datos, ejecute el script siguiente:

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
out1 = spark.sql("SELECT distinct(Carrier) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```

### <a name="run-complex-queries"></a>Ejecución de consultas complejas

Para ejecutar las siguientes consultas más complejas, ejecute cada segmento a la vez en el cuaderno e inspeccione los resultados.

```python
#find the airline with the most flights

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")

#show the top row without truncation
output.show(1, False)

#show the top 10 airlines
output.show(10, False)

#Determine which is the least on time airline

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE DepDelay>60 or ArrDelay>60 group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("select * from v")
#output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")
#show the top row without truncation
output.show(1, False)

#which airline improved its performance
#find the airline with the most improvement in delays
#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v1")
spark.sql("DROP VIEW IF EXISTS v2")
spark.sql("CREATE TEMPORARY VIEW v1 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2016 group by Carrier order by NumFlights desc LIMIT 10")
spark.sql("CREATE TEMPORARY VIEW v2 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2017 group by Carrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT distinct ac.AirlineName, v1.Carrier, v1.NumFlights, v2.NumFlights from v1 INNER JOIN v2 ON v1.Carrier = v2.Carrier INNER JOIN AirlineCodes ac ON v2.Carrier = ac.AirlineCode WHERE v1.NumFlights > v2.NumFlights")
#show the top row without truncation
output.show(10, False)

#display for visual analysis
display(output)
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos y todos los recursos relacionados. Para ello, seleccione el grupo de recursos de la cuenta de almacenamiento y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

[!div class="nextstepaction"] 
> [Extracción, transformación y carga de datos mediante Apache Hive en Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)

