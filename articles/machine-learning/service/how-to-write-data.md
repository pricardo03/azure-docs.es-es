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
ms.openlocfilehash: da5823473b7f69fa0a6c65d5ea7319bfd2e92720
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946772"
---
# <a name="write-data-with-the-azure-machine-learning-data-prep-sdk"></a>Escribir datos con el SDK de preparación de datos de Azure Machine Learning
Puede escribir datos en cualquier punto de un flujo de datos. Estas opciones de escritura se agregan como pasos al flujo de datos resultante, y se ejecutan cada vez que el flujo de datos se activa. Como no hay limitaciones en cuanto a la cantidad de pasos de escritura que hay en una canalización, es fácil escribir los resultados intermedios para solucionar problemas o para que los seleccionen otras canalizaciones. Es importante tener en cuenta que la ejecución de cada paso de escritura produce una extracción total de los datos en el flujo de datos. Por ejemplo, un flujo de datos con tres pasos de escritura leerá y procesará cada registro del conjunto de datos tres veces.

## <a name="writing-to-files"></a>Escribir en archivos
Con el [SDK de preparación de datos de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py), puede escribir datos en archivos de cualquiera de nuestras ubicaciones admitidas (sistema de archivos local, Azure Blob Storage y Azure Data Lake Storage). Los datos se escriben en múltiples archivos de partición para permitir escrituras paralelas. Una vez que se completa una escritura, también se genera un archivo centinela denominado SUCCESS. Esto le ayudará a identificar el momento en que se completa una escritura intermedia sin tener que esperar a que se complete toda la canalización.

Al ejecutar un flujo de datos en Spark, debe escribir en una carpeta vacía. Si intenta ejecutar una escritura en una carpeta existente, se producirá un error. Asegúrese de que su carpeta de destino esté vacía o use una ubicación de destino diferente para cada ejecución; de lo contrario, se producirá un error.

El SDK de preparación de datos de Azure Machine Learning es compatible con los siguientes formatos de archivo:
-   Archivos delimitados (CSV, TSV, etc.)
-   Archivos de Parquet

En este ejemplo, comience cargando los datos en un flujo de datos. Volverá a usar estos datos con formatos diferentes.

```

import azureml.dataprep as dprep
t = dprep.smart_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(10)
```   

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

## <a name="delimited-files"></a>Archivos delimitados
En la siguiente línea se crea un nuevo flujo de datos con un paso de escritura, pero la escritura real aún no se ha llevado a cabo. Cuando el flujo de datos se ejecuta, se ejecutará a su vez la operación de escritura.

```
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))
```
Ahora ejecute el flujo de datos, que a su vez ejecutará la operación de escritura.
```
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```
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

Los datos escritos contienen varios errores en las columnas numéricas, porque hay números que no se analizaron correctamente. Cuando se escriben en CSV, estos valores null se reemplazan con la cadena "ERROR" por defecto. Puede agregar parámetros como parte de su llamada a la operación de escritura, y especificar una cadena que se usará para representar valores null.

```
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```
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
## <a name="parquet-files"></a>Archivos de Parquet

 De forma similar a la `write_to_csv`función anterior, `write_to_parquet` devuelve un nuevo flujo de datos con un paso de escritura de tipo Parquet que se ejecutará en cuanto se ejecute el flujo de datos.

```
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```
 Ahora ejecute el flujo de datos, que a su vez ejecutará la operación de escritura.

```
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(10)
```
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
