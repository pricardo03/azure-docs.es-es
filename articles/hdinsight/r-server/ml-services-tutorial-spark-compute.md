---
title: 'Tutorial: Uso de R en el contexto de proceso de Spark en Azure HDInsight'
description: 'Tutorial: Introducción a R y Spark en ML Services.'
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/21/2019
ms.openlocfilehash: 244c62467f187417bbb9f0e54173aad5a7d26d0a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67451746"
---
# <a name="tutorial-use-r-in-a-spark-compute-context-in-azure-hdinsight"></a>Tutorial: Uso de R en el contexto de proceso de Spark en Azure HDInsight

En este tutorial se proporciona una introducción paso a paso al uso de las funciones de R en Apache Spark que se ejecutan en un clúster de ML Services en Azure HDInsight.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Descargar datos de ejemplo en el almacenamiento local
> * Copiar datos en el almacenamiento predeterminado
> * Configurar un conjunto de datos
> * Creación de un origen de datos
> * Crear el contexto de proceso para Spark
> * Ajustar un modelo lineal
> * Usar archivos XDF compuestos
> * Convertir XDF a CSV

## <a name="prerequisites"></a>Requisitos previos

* Un clúster de ML Services en HDInsight. Consulte el artículo sobre la [Creación de clústeres de Apache Hadoop mediante Azure Portal](../hdinsight-hadoop-create-linux-clusters-portal.md) y seleccione **ML Services** como **Tipo de clúster**.

## <a name="connect-to-rstudio-server"></a>Conexión a RStudio Server

RStudio Server se ejecuta en el nodo perimetral del clúster. Vaya a la siguiente dirección URL, donde `CLUSTERNAME` es el nombre del clúster de ML Services que ha creado:

```
https://CLUSTERNAME.azurehdinsight.net/rstudio/
```

La primera vez que inicie sesión deberá autenticarse dos veces. En el primer mensaje de autenticación, proporcione el identificador de usuario administrador del clúster y la contraseña, el valor predeterminado es `admin`. En el segundo, proporcione el identificador de usuario administrador de SSH y la contraseña, el valor predeterminado es `sshuser`. En los sucesivos inicios de sesión solo se requieren las credenciales de SSH.

## <a name="download-sample-data"></a>Descarga de los datos de ejemplo

El *conjunto de datos Airline 2012 On-Time* consta de 12 archivos separados por comas que contienen los detalles de llegada y salida de todos los vuelos comerciales dentro de Estados Unidos del año 2012. Se trata de un conjunto de macrodatos con más de seis millones de observaciones.

1. Inicialice algunas variables de entorno. Escriba el siguiente código en la consola de RStudio Server:

    ```R
    bigDataDirRoot <- "/tutorial/data" # root directory on cluster default storage
    localDir <- "/tmp/AirOnTimeCSV2012" # directory on edge node
    remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012" # location of data
    ```

    Las variables se mostrarán en el lado derecho de la pantalla en la pestaña **Environment** (Entorno).

    ![RStudio](./media/ml-services-tutorial-spark-compute/rstudio.png)

2.  Cree el directorio local y descargue los datos de ejemplo. Escriba el siguiente código en RStudio:

    ```R
    # Create local directory
    dir.create(localDir)
    
    # Download data to the tmp folder(local)
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(localDir, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(localDir, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(localDir, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(localDir, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(localDir, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(localDir, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(localDir, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(localDir, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(localDir, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(localDir, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(localDir, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(localDir, "airOT201212.csv"))
    ```

    La descarga debería tardar unos 9,5 minutos.

## <a name="copy-data-to-default-storage"></a>Copiar datos en el almacenamiento predeterminado

La ubicación del HDFS se especifica con la variable `airDataDir`. Escriba el siguiente código en RStudio:

```R
# Set directory in bigDataDirRoot to load the data into
airDataDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

# Create directory (default storage)
rxHadoopMakeDir(airDataDir)

# Copy data from local storage to default storage
rxHadoopCopyFromLocal(localDir, bigDataDirRoot)
    
# Optional. Verify files
rxHadoopListFiles(airDataDir)
```

Este paso debería tardar unos 10 segundos.

## <a name="set-up-data-set"></a>Configurar un conjunto de datos

1. Cree un objeto de sistema de archivos que use los valores predeterminados. Escriba el siguiente código en RStudio:

    ```R
    # Define the HDFS (WASB) file system
    hdfsFS <- RxHdfsFileSystem()
    ```

2. Los archivos CSV originales tienen nombres de variable bastante difíciles; he aquí una lista `colInfo` para facilitar su administración. Escriba el siguiente código en RStudio:

    ```R
    airlineColInfo <- list(
         MONTH = list(newName = "Month", type = "integer"),
        DAY_OF_WEEK = list(newName = "DayOfWeek", type = "factor",
            levels = as.character(1:7),
            newLevels = c("Mon", "Tues", "Wed", "Thur", "Fri", "Sat",
                          "Sun")),
        UNIQUE_CARRIER = list(newName = "UniqueCarrier", type =
                                "factor"),
        ORIGIN = list(newName = "Origin", type = "factor"),
        DEST = list(newName = "Dest", type = "factor"),
        CRS_DEP_TIME = list(newName = "CRSDepTime", type = "integer"),
        DEP_TIME = list(newName = "DepTime", type = "integer"),
        DEP_DELAY = list(newName = "DepDelay", type = "integer"),
        DEP_DELAY_NEW = list(newName = "DepDelayMinutes", type =
                             "integer"),
        DEP_DEL15 = list(newName = "DepDel15", type = "logical"),
        DEP_DELAY_GROUP = list(newName = "DepDelayGroups", type =
                               "factor",
           levels = as.character(-2:12),
           newLevels = c("< -15", "-15 to -1","0 to 14", "15 to 29",
                         "30 to 44", "45 to 59", "60 to 74",
                         "75 to 89", "90 to 104", "105 to 119",
                         "120 to 134", "135 to 149", "150 to 164",
                         "165 to 179", ">= 180")),
        ARR_DELAY = list(newName = "ArrDelay", type = "integer"),
        ARR_DELAY_NEW = list(newName = "ArrDelayMinutes", type =
                             "integer"),  
        ARR_DEL15 = list(newName = "ArrDel15", type = "logical"),
        AIR_TIME = list(newName = "AirTime", type =  "integer"),
        DISTANCE = list(newName = "Distance", type = "integer"),
        DISTANCE_GROUP = list(newName = "DistanceGroup", type =
                             "factor",
         levels = as.character(1:11),
         newLevels = c("< 250", "250-499", "500-749", "750-999",
             "1000-1249", "1250-1499", "1500-1749", "1750-1999",
             "2000-2249", "2250-2499", ">= 2500")))
    
    varNames <- names(airlineColInfo)
    ```

## <a name="create-data-source"></a>Creación de un origen de datos

En un contexto de proceso de Spark, puede crear orígenes de datos mediante las siguientes funciones:

|Función | DESCRIPCIÓN |
|---------|-------------|
|`RxTextData` | Origen de datos de texto delimitado por comas. |
|`RxXdfData` | Datos en formato de archivo XDF. En RevoScaleR el formato de archivo XDF se modifica para que Hadoop almacene los datos en un conjunto de archivos compuesto en lugar de en un único archivo. |
|`RxHiveData` | Genera un objeto de origen de datos de Hive.|
|`RxParquetData` | Genera un objeto de origen de datos de Parquet.|
|`RxOrcData` | Genera un objeto de origen de datos de Orc.|

Cree un objeto [RxTextData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxtextdata) con los archivos que copió en HDFS. Escriba el siguiente código en RStudio:

```R
airDS <- RxTextData( airDataDir,
                        colInfo = airlineColInfo,
                        varsToKeep = varNames,
                        fileSystem = hdfsFS ) 
```

## <a name="create-compute-context-for-spark"></a>Crear el contexto de proceso para Spark

Para cargar los datos y ejecutar análisis en nodos de trabajo, establezca el contexto de proceso del script en [RxSpark](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxspark). En este contexto, las funciones de R distribuyen automáticamente la carga de trabajo entre todos los nodos de trabajo, sin requisitos incorporados de administración de los trabajos o la cola. El contexto de proceso de Spark se establece a mediante `RxSpark` o `rxSparkConnect()` para crear el contexto de proceso de Spark y usa `rxSparkDisconnect()` para volver a un contexto de proceso local. Escriba el siguiente código en RStudio:

```R
# Define the Spark compute context
mySparkCluster <- RxSpark()

# Set the compute context
rxSetComputeContext(mySparkCluster)
```

## <a name="fit-a-linear-model"></a>Ajustar un modelo lineal

1. Use la función [rxLinMod](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlinmod) para ajustar un modelo lineal con el origen de datos `airDS`. Escriba el siguiente código en RStudio:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDS,
              cube = TRUE)
    )
    ```
    
    Este paso debería tardar entre 2 y 3 minutos.

1. Vea los resultados. Escriba el siguiente código en RStudio:

    ```R
    summary(delayArr)
    ```

    Debería ver los siguientes resultados:

    ```output
    Call:
    rxLinMod(formula = ArrDelay ~ DayOfWeek, data = airDS, cube = TRUE)
    
    Cube Linear Regression Results for: ArrDelay ~ DayOfWeek
    Data: airDataXdf (RxXdfData Data Source)
    File name: /tutorial/data/AirOnTimeCSV2012
    Dependent variable(s): ArrDelay
    Total independent variables: 7 
    Number of valid observations: 6005381
    Number of missing observations: 91381 
     
    Coefficients:
                   Estimate Std. Error t value Pr(>|t|)     | Counts
    DayOfWeek=Mon   3.54210    0.03736   94.80 2.22e-16 *** | 901592
    DayOfWeek=Tues  1.80696    0.03835   47.12 2.22e-16 *** | 855805
    DayOfWeek=Wed   2.19424    0.03807   57.64 2.22e-16 *** | 868505
    DayOfWeek=Thur  4.65502    0.03757  123.90 2.22e-16 *** | 891674
    DayOfWeek=Fri   5.64402    0.03747  150.62 2.22e-16 *** | 896495
    DayOfWeek=Sat   0.91008    0.04144   21.96 2.22e-16 *** | 732944
    DayOfWeek=Sun   2.82780    0.03829   73.84 2.22e-16 *** | 858366
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 35.48 on 6005374 degrees of freedom
    Multiple R-squared: 0.001827 (as if intercept included)
    Adjusted R-squared: 0.001826 
    F-statistic:  1832 on 6 and 6005374 DF,  p-value: < 2.2e-16 
    Condition number: 1 
    ```

    Tenga en cuenta que los resultados indican que hemos procesado todos los datos, seis millones de observaciones, con todos los archivos .csv del directorio especificado. Observe también que, dado que especificamos `cube = TRUE`, tenemos un coeficiente estimado para cada día de la semana (y no la intersección).

## <a name="use-composite-xdf-files"></a>Usar archivos XDF compuestos

Como hemos visto, puede analizar los archivos CSV directamente con R en Hadoop, pero se puede realizar más rápidamente si los datos se almacenan en un formato más eficiente. El formato .xdf de R es realmente muy eficiente, pero se modifica un poco para HDFS de manera que los archivos individuales sigan dentro de un único bloque HDFS. (El tamaño del bloque HDFS varía en función de la instalación, pero normalmente es 64 MB o 128 MB). Al usar [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) en Hadoop, especifique un origen de datos `RxTextData` como `AirDS` para inData y un origen de datos `RxXdfData` con el sistema de archivos como argumento outFile para crear un conjunto compuesto de archivos .xdf. El objeto `RxXdfData` se puede usar como argumento de datos en análisis posteriores de R.

1. Defina un objeto `RxXdfData`. Escriba el siguiente código en RStudio:

    ```R
    airDataXdfDir <- file.path(bigDataDirRoot,"AirOnTimeXDF2012")
    
    airDataXdf <- RxXdfData( airDataXdfDir,
                            fileSystem = hdfsFS )
    ```

1. Establezca un tamaño de bloque de 250 000 filas y especifique que se lean todos los datos. Escriba el siguiente código en RStudio:

    ```R
    blockSize <- 250000
    numRowsToRead = -1
    ```

1. Importe los datos mediante `rxImport`. Escriba el siguiente código en RStudio:

    ```R
    rxImport(inData = airDS,
             outFile = airDataXdf,
             rowsPerRead = blockSize,
             overwrite = TRUE,
             numRows = numRowsToRead )
    ```
    
    Este paso debería tardar unos minutos.

1. Vuelva a calcular el mismo modelo lineal con el origen de datos nuevo y más rápido. Escriba el siguiente código en RStudio:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDataXdf,
              cube = TRUE)
    )
    ```
    
    Este paso debería tardar menos de un minuto.

1. Vea los resultados. Los resultados deben ser los mismos que con los archivos CSV. Escriba el siguiente código en RStudio:

    ```R
    summary(delayArr)
    ```

## <a name="convert-xdf-to-csv"></a>Convertir XDF a CSV

### <a name="in-a-spark-context"></a>En un contexto de Spark

Si ha convertido el archivo CSV a XDF para aprovechar la eficiencia al ejecutar análisis, pero ahora desea convertir los datos a CSV de nuevo, puede hacerlo mediante [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep).

Para crear una carpeta de archivos CSV, cree primero un objeto `RxTextData` utilizando un nombre de directorio como argumento de archivo; esto representa la carpeta en la que se van a crear los archivos CSV. Este directorio se crea al ejecutar `rxDataStep`. A continuación, apunte a este objeto `RxTextData` en el argumento `outFile` de `rxDataStep`. Los archivos CSV creados se nombrarán en función del nombre del directorio seguido de un número.

Imaginemos que queremos escribir una carpeta de CSV en HDFS desde nuestro XDF compuesto `airDataXdf` después de haber realizado la regresión logística y la predicción para que los nuevos archivos CSV contengan los valores de predicción y los valores residuales. Escriba el siguiente código en RStudio:

```R
airDataCsvDir <- file.path(bigDataDirRoot,"AirDataCSV2012")
airDataCsvDS <- RxTextData(airDataCsvDir,fileSystem=hdfsFS)
rxDataStep(inData=airDataXdf, outFile=airDataCsvDS)
```

Este paso debería tardar unos 2,5 minutos.

Observe que `rxDataStep` escribió un CSV por cada archivo .xdfd del archivo XDF compuesto de entrada. Este es el comportamiento predeterminado para escribir los CSV a partir de XDF compuestos en HDFS cuando se establece el contexto de proceso en `RxSpark`.

### <a name="in-a-local-context"></a>En un contexto local

Como alternativa, podría cambiar el contexto de proceso de nuevo a `local` cuando haya terminado realizar los análisis y aprovechar las ventajas de dos argumentos de `RxTextData`, que ofrecen un poco más control al escribir archivos CSV en HDFS: `createFileSet` y `rowsPerOutFile`. Cuando `createFileSet` está establecido en `TRUE`, se escribe una carpeta de archivos CSV en el directorio especificado. Cuando `createFileSet` está establecido en `FALSE` se escribe un solo archivo CSV. El segundo argumento, `rowsPerOutFile`, se puede establecer en un entero para indicar el número de filas que escribir en cada CSV archivo cuando `createFileSet` es `TRUE`.

Escriba el siguiente código en RStudio:

```R
rxSetComputeContext("local")
airDataCsvRowsDir <- file.path(bigDataDirRoot,"AirDataCSVRows2012")
airDataCsvRowsDS <- RxTextData(airDataCsvRowsDir, fileSystem=hdfsFS, createFileSet=TRUE, rowsPerOutFile=1000000)
rxDataStep(inData=airDataXdf, outFile=airDataCsvRowsDS)
```

Este paso debería tardar unos 10 minutos.

Al usar un contexto de proceso `RxSpark`, el valor predeterminado de `createFileSet` es `TRUE` y `rowsPerOutFile` no tiene efecto. Por lo tanto, si desea crear un solo CSV o personalizar el número de filas por archivo, debe ejecutar `rxDataStep` en un contexto de proceso `local` (los datos pueden permanecer en HDFS).

## <a name="final-steps"></a>Pasos finales

1. Limpie los datos. Escriba el siguiente código en RStudio:

    ```R
    rxHadoopRemoveDir(airDataDir)
    rxHadoopRemoveDir(airDataXdfDir)
    rxHadoopRemoveDir(airDataCsvDir)
    rxHadoopRemoveDir(airDataCsvRowsDir)
    rxHadoopRemoveDir(bigDataDirRoot)
    ```

1. Detenga la aplicación de Spark remota. Escriba el siguiente código en RStudio:

    ```R
    rxStopEngine(mySparkCluster)
    ```

1. Cierre la sesión de R. Escriba el siguiente código en RStudio:

    ```R
    quit()
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Después de completar el tutorial, puede ser conveniente eliminar el clúster. Con HDInsight, los datos se almacenan en Azure Storage, por lo que puede eliminar un clúster de forma segura cuando no se esté usando. También se le cobrará por un clúster de HDInsight aunque no se esté usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se estén usando.

Para eliminar un clúster, consulte [Eliminación de un clúster de HDInsight con el explorador, PowerShell o la CLI de Azure](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a usar las funciones de R en Apache Spark que se ejecutan en un clúster de ML Services en Azure HDInsight. Para más información, consulte los siguientes artículos.

* [Opciones de contexto de proceso para de ML Services en HDInsight](r-server-compute-contexts.md)
* [Funciones de R para Spark en Hadoop](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler-hadoop-functions)
