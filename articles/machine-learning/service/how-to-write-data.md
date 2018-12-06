---
title: Escribir datos con el SDK de preparación de datos de Azure Machine Learning - Python
description: Obtenga información sobre cómo escribir datos con el SDK de preparación de datos de Azure Machine Learning. Puede escribir datos en cualquier punto de un flujo de datos, y en los archivos de cualquiera de nuestras ubicaciones admitidas (sistema de archivos local, Azure Blob Storage y Azure Data Lake Storage).
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 4a2af832fda8a85ee8a4aba395a8f436172153ed
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308569"
---
# <a name="write-data-using-the-azure-machine-learning-data-prep-sdk"></a>Escribir datos con el SDK de preparación de datos de Azure Machine Learning

En este artículo, conocerá diferentes métodos para escribir datos utilizando el SDK de preparación de datos de Azure Machine Learning. Los datos de salida se pueden escribir en cualquier punto del flujo de datos, ya que las operaciones de escritura se agregarán como pasos al flujo de datos resultante y se ejecutarán junto con el flujo de datos. Los datos se escriben en múltiples archivos de partición para permitir escrituras paralelas.

Como no hay limitaciones en cuanto a la cantidad de pasos de escritura que hay en una canalización, puede agregar fácilmente pasos de escritura adicionales para obtener resultados intermedios para solucionar problemas o para otras canalizaciones.

Cada vez que ejecuta un paso de escritura, se produce una incorporación de cambios completa de los datos en el flujo de datos. Por ejemplo, un flujo de datos con tres pasos de escritura leerá y procesará tres veces cada registro del conjunto de datos.

## <a name="supported-data-types-and-location"></a>Ubicación y tipos de datos admitidos

Se admiten los siguientes formatos de archivo
-   Archivos delimitados (CSV, TSV, etc.)
-   Archivos de Parquet

Puede usar el [SDK de preparación de datos de Azure Machine Learning para python](https://aka.ms/data-prep-sdk) para escribir datos en:
+ un sistema de archivos local
+ Azure Blob Storage
+ Azure Data Lake Storage

## <a name="spark-considerations"></a>Consideraciones sobre Spark

Al ejecutar un flujo de datos en Spark, debe escribir en una carpeta vacía. Al intentar escribir en una carpeta existente, se produce un error. Asegúrese de que su carpeta de destino esté vacía o use una ubicación de destino diferente para cada ejecución; de lo contrario, se producirá un error.

## <a name="monitoring-write-operations"></a>Supervisión de operaciones de escritura

Para su comodidad, se genera un archivo centinela denominado SUCCESS cada vez que se completa una escritura. Su presencia le ayuda a identificar el momento en que se completa una escritura intermedia sin tener que esperar a que se complete toda la canalización.

## <a name="example-write-code"></a>Código de escritura de ejemplo

En este ejemplo, comience cargando los datos en un flujo de datos. Volverá a utilizar estos datos con formatos diferentes.

```python
import azureml.dataprep as dprep
t = dprep.smart_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(10)
```

Salida de ejemplo:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   None|       NO|     NO  |   ENRS    |NaN    |   NaN |   NaN|    
|   1|      10003.0 |   99999.0 |   None|       NO|     NO  |   ENSO|       NaN|        NaN |NaN|   
|   2|  10010.0|    99999.0|    None|   NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    None|   NO| NO| |   NaN|    NaN|    NaN|
|4| 10014.0|    99999.0|    None|   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    None|   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   None|   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    None|   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    None|   NO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    None|   NO| SV|     |77000.0|   15500.0|    120.0|

### <a name="delimited-file-example"></a>Ejemplo de archivo delimitado

En el código siguiente, la función `write_to_csv` se utiliza para escribir datos en un archivo delimitado.

```python
# Create a new data flow using `write_to_csv` 
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))

# Run the data flow to begin the write operation.
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

Salida de ejemplo:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   ERROR |       NO|     NO  |   ENRS    |ERROR    |   ERROR |   ERROR|    
|   1|      10003.0 |   99999.0 |   ERROR |       NO|     NO  |   ENSO|       ERROR|        ERROR |ERROR|   
|   2|  10010.0|    99999.0|    ERROR |   NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    ERROR |   NO| NO| |   ERROR|    ERROR|    ERROR|
|4| 10014.0|    99999.0|    ERROR |   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    ERROR |   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   ERROR |   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    ERROR |   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    ERROR |   NO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    ERROR |   NO| SV|     |77000.0|   15500.0|    120.0|

En la salida anterior, aparecen varios errores en las columnas numéricas porque hay números que no se analizaron correctamente. Cuando se escriben en CSV, de forma predeterminada, los valores null se reemplazan con la cadena "ERROR".

Puede agregar parámetros en la llamada a la operación de escritura y especificar una cadena para que se utilice como representación de los valores null.

```python
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

El código anterior produce este resultado:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   BadData |       NO|     NO  |   ENRS    |BadData    |   BadData |   BadData|    
|   1|      10003.0 |   99999.0 |   BadData |       NO|     NO  |   ENSO|       BadData|        BadData |BadData|   
|   2|  10010.0|    99999.0|    BadData |   NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    BadData |   NO| NO| |   BadData|    BadData|    BadData|
|4| 10014.0|    99999.0|    BadData |   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    BadData |   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   BadData |   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    BadData |   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    BadData |   NO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    BadData |   NO| SV|     |77000.0|   15500.0|    120.0|

### <a name="parquet-file-example"></a>Ejemplo de archivo PARQUET

De forma similar a `write_to_csv`, la función `write_to_parquet` devuelve un nuevo flujo de datos con un paso de escritura de tipo PARQUET que se ejecuta cuando se ejecuta el flujo de datos.

```python
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```

Ejecute el flujo de datos para iniciar la operación de escritura.

```python
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(10)
```

El código anterior produce este resultado:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   MiscreantData |       NO|     NO  |   ENRS    |MiscreantData    |   MiscreantData |   MiscreantData|    
|   1|      10003.0 |   99999.0 |   MiscreantData |       NO|     NO  |   ENSO|       MiscreantData|        MiscreantData |MiscreantData|   
|   2|  10010.0|    99999.0|    MiscreantData |   NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    MiscreantData |   NO| NO| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0|    99999.0|    MiscreantData |   NO| NO| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    MiscreantData |   NO| NO| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   MiscreantData |   NO| NO|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    MiscreantData |   NO| NO| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    MiscreantData |   NO| SV|     |80050.0|   16250.0|    80.0|
|9| 10030.0|    99999.0|    MiscreantData |   NO| SV|     |77000.0|   15500.0|    120.0|
