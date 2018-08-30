---
title: Acceso a los datos de Azure Data Lake Storage Gen2 (versión preliminar) con Azure Databricks mediante Spark | Microsoft Docs
description: Aprenda a ejecutar consultas de Spark en un clúster de Azure Databricks para acceder a los datos de una cuenta de almacenamiento de Azure Data Lake Storage Gen2.
services: hdinsight,storage
tags: azure-portal
author: dineshm
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 6/27/2018
ms.author: dineshm
ms.openlocfilehash: 83e6b6817f33cb19c3839a94a8919f3516576d68
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886585"
---
# <a name="tutorial-access-azure-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Tutorial: Acceso a los datos de Azure Data Lake Storage Gen2 (versión preliminar) con Azure Databricks mediante Spark

En este tutorial, aprenderá a ejecutar consultas Spark en un clúster de Azure Databricks para consultar los datos de la cuenta compatibles con Azure Data Lake Storage Gen2 (versión preliminar).

> [!div class="checklist"]
> * Creación de un clúster de Databricks
> * Ingesta de datos no estructurados en una cuenta de almacenamiento
> * Desencadenamiento de una instancia de Azure Function para procesar datos
> * Ejecución de análisis en los datos de almacenamiento de blobs

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se muestra cómo consumir y consultar los datos de los vuelos de las líneas aéreas, que están disponibles en el [Departamento de Transporte de Estados Unidos](https://transtats.bts.gov/Tables.asp?DB_ID=120&DB_Name=Airline%20On-Time%20Performance%20Data&DB_Short_Name=On-Time). Descargue al menos dos años de datos de líneas aéreas (seleccionando todos los campos) y guarde el resultado en la máquina. Asegúrese de tomar nota del nombre del archivo y de la ruta de acceso de la descarga; necesita esta información en un paso posterior.

> [!NOTE]
> Active la casilla **Prezipped file** (Archivo precomprimido) para seleccionar todos los campos de datos. La descarga tendrá un tamaño de muchos gigabytes, pero esta cantidad de datos es necesaria para el análisis.

## <a name="create-an-azure-data-lake-storage-gen2-account"></a>Creación de una cuenta de Azure Data Lake Storage Gen2

Para comenzar, cree una nueva [cuenta de Azure Data Lake Storage Gen2](quickstart-create-account.md) y asígnele un nombre único. A continuación, vaya a la cuenta de almacenamiento para recuperar los ajustes de configuración.

> [!IMPORTANT]
> Durante la versión preliminar, Azure Functions solo funciona con las cuentas de Azure Data Lake Storage Gen2 creadas con un espacio de nombres plano.

1. En **Configuración**, haga clic en **Claves de acceso**.
3. Haga clic en el botón **Copiar** junto a **key1** para copiar el valor de la clave.

El nombre de cuenta y la clave se necesitan en los pasos posteriores de este tutorial. Abra un editor de texto y reserve el nombre de cuenta y la clave para futuras referencias.

## <a name="create-a-databricks-cluster"></a>Creación de un clúster de Databricks

El paso siguiente consiste en crear un [clúster de Databricks](https://docs.azuredatabricks.net/) para crear un área de trabajo de datos.

1. Cree un [servicio Databricks](https://ms.portal.azure.com/#create/Microsoft.Databricks) y asígnele el nombre **myFlightDataService** (asegúrese de activar la casilla *Anclar al panel* al crear el servicio).
2. Haga clic en **Launch Workspace** (Iniciar área de trabajo) para abrir el área de trabajo en una nueva ventana del explorador.
3. Haga clic en **Clusters** (Clústeres) en la barra de navegación izquierda.
4. Haga clic en **Create Cluster** (Crear clúster).
5. Escriba el nombre **myFlightDataCluster** en el campo *Cluster name* (Nombre del clúster).
6. Seleccione **Standard_D8s_v3** en el campo *Worker Type* (Tipo de trabajo).
7. Cambie el valor de **Min Workers** (Trabajos mínimos) en *4*.
8. Haga clic en **Create Cluster** (Crear clúster) en la parte superior de la página (este proceso puede tardar hasta 5 minutos en completarse).
9. Cuando se completa el proceso, seleccione **Azure Databricks** en la parte superior izquierda de la barra de navegación.
10. Seleccione **Notebook** en la sección **Nuevo** en la mitad inferior de la página.
11. Escriba el nombre que desee en el campo **Nombre** y seleccione **Python** como lenguaje.
12. Todos los demás campos pueden dejarse con los valores predeterminados.
13. Seleccione **Crear**.
14. Pegue el código siguiente en la celda **Cmd 1**, reemplace los valores por los que se conservan en la cuenta de almacenamiento.

    ```bash
    spark.conf.set("fs.azure.account.key.<account_name>.dfs.core.windows.net", "<account_key>") 
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
    dbutils.fs.ls("abfss://<file_system>@<account_name>.dfs.core.windows.net/")
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")
    ```

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
6. Pegue el código siguiente en la celda **Cmd 1** (este código se guarda automáticamente en el editor).

    ```python
    #mount Azure Blob Storage as an HDFS file system to your databricks cluster
    #you need to specify a storage account and container to connect to. 
    #use a SAS token or an account key to connect to Blob Storage.  
    accountname = "<insert account name>"
    accountkey = " <insert account key>"
    fullname = "fs.azure.account.key." +accountname+ ".dfs.core.windows.net"
    accountsource = "abfs://dbricks@" +accountname+ ".dfs.core.windows.net/folder1"
    #create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load(accountsource + "/On_Time_On_Time*.csv")
    #read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet(accountsource + '/parquet/flights')

    #read the flight details parquet file 
    #flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load(accountsource + "/parquet/flights")
    print("Done")
    ```

## <a name="explore-data-using-hadoop-distributed-file-system"></a>Exploración de datos con un sistema de archivos distribuido de Hadoop

Vuelva al área de trabajo de Databricks y haga clic en el icono de **Recientes** en la barra de navegación izquierda.

1. Haga clic en el cuaderno **Flight Data Analytics** (Análisis de datos de vuelo).
2. Presione **Ctrl + Alt + N** para crear una nueva celda.

Escriba cada uno de los siguientes bloques de código en **Cmd 1** y presione **Cmd + ENTRAR** para ejecutar el script de Python.

Para obtener una lista de archivos CSV cargados a través de AzCopy, ejecute el script siguiente:

```python
import os.path
import IPython
from pyspark.sql import SQLContext
source = "abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/"
dbutils.fs.ls(source + "/temp")
display(dbutils.fs.ls(source + "/temp/"))
```

Para crear un nuevo archivo y enumerar los archivos de la carpeta *parquet/flights*, ejecute este script:

```python
source = "abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/"

dbutils.fs.help()

dbutils.fs.put(source + "/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls(source + "/temp/parquet/flights")
```
Con estos ejemplos de código, ha explorado la naturaleza jerárquica de HDFS usando datos almacenados en una cuenta compatible con Azure Data Lake Storage Gen2.

## <a name="query-the-data"></a>Consulta de los datos

A continuación, puede empezar a consultar los datos cargados en Azure Data Lake Storage. Escriba cada uno de los siguientes bloques de código en **Cmd 1** y presione **Cmd + ENTRAR** para ejecutar el script de Python.

### <a name="simple-queries"></a>Consultas sencillas

Para crear tramas de datos para los orígenes de datos, ejecute el script siguiente:

> [!IMPORTANT]
> Asegúrese de reemplazar el marcador de posición **<SU_NOMBRE_ARCHIVO_CSV>** por el nombre del archivo descargado al principio de este tutorial.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load(accountsource + "/<YOUR_CSV_FILE_NAME>.csv")
acDF.write.parquet(accountsource + '/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created via the Azure Function
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load(accountsource + "/parquet/flights")

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
#preferrably run this in a separate cmd cell
display(flightDF)
```

Para ejecutar las consultas de análisis con los datos, ejecute el script siguiente:

```python
#Run each of these queries, preferrably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet(accountsource + '/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet(accountsource + '/parquet/airlinecodes')
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
### <a name="complex-queries"></a>Consultas complejas

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

## <a name="next-steps"></a>Pasos siguientes

* [Extracción, transformación y carga de datos mediante Apache Hive en Azure HDInsight](tutorial-extract-transform-load-hive.md)
