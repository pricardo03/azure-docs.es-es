---
title: 'Escritura: SDK de Python para la preparación de datos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo escribir datos con el SDK de preparación de datos de Azure Machine Learning. Puede escribir datos en cualquier punto de un flujo de datos, y en los archivos de cualquiera de nuestras ubicaciones admitidas (sistema de archivos local, Azure Blob Storage y Azure Data Lake Storage).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: jmartens
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 6206ad1a7356221bf94134e5d293c27d778cc187
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66752875"
---
# <a name="write-and-configure-data--with-the-azure-machine-learning-data-prep-sdk"></a>Escritura y configuración de datos con el SDK de preparación de datos de Azure Machine Learning

En este artículo, aprenderá diferentes métodos para escribir datos mediante el [SDK de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk) y cómo configurar esos datos para la experimentación con el [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).  Los datos de salida se pueden escribir en cualquier punto de un flujo de datos. Las opciones de escritura se agregan como pasos al flujo de datos resultante y se ejecutan cada vez que el flujo de datos se activa. Los datos se escriben en múltiples archivos de partición para permitir escrituras paralelas.

> [!Important]
> Si está creando una solución, pruebe los [conjuntos de datos de Azure Machine Learning](how-to-explore-prepare-data.md) (versión preliminar) para transformar sus datos, realizar instantáneas de los datos y almacenar definiciones de conjuntos de datos con versiones. Los conjuntos de datos son la próxima versión del SDK de preparación de datos y ofrecen funcionalidad ampliada para administrar conjuntos de datos en soluciones de inteligencia artificial.
> Si usa el paquete `azureml-dataprep` para crear un flujo de datos con las transformaciones en lugar de usar el paquete `azureml-datasets` para crear un conjunto de datos, no podrá usar más adelante instantáneas o conjuntos de datos con versiones.

Como no hay limitaciones en cuanto a la cantidad de pasos de escritura que hay en una canalización, puede agregar fácilmente pasos de escritura adicionales para obtener resultados intermedios para solucionar problemas o para otras canalizaciones.

Cada vez que ejecuta un paso de escritura, se produce una incorporación de cambios completa de los datos en el flujo de datos. Por ejemplo, un flujo de datos con tres pasos de escritura leerá y procesará tres veces cada registro del conjunto de datos.

## <a name="supported-data-types-and-location"></a>Ubicación y tipos de datos admitidos

Se admiten los siguientes formatos de archivo
-   Archivos delimitados (CSV, TSV, etc.)
-   Archivos de Parquet

Puede usar el SDK de preparación de datos de Azure Machine Learning para python para escribir datos en:
+ un sistema de archivos local
+ Azure Blob Storage
+ Azure Data Lake Storage

## <a name="spark-considerations"></a>Consideraciones sobre Spark

Al ejecutar un flujo de datos en Spark, debe escribir en una carpeta vacía. Al intentar escribir en una carpeta existente, se produce un error. Asegúrese de que su carpeta de destino esté vacía o use una ubicación de destino diferente para cada ejecución; de lo contrario, se producirá un error.

## <a name="monitoring-write-operations"></a>Supervisión de operaciones de escritura

Para su comodidad, se genera un archivo centinela denominado SUCCESS cada vez que se completa una escritura. Su presencia le ayuda a identificar el momento en que se completa una escritura intermedia sin tener que esperar a que se complete toda la canalización.

## <a name="example-write-code"></a>Código de escritura de ejemplo

En este ejemplo, comience cargando los datos en un flujo de datos mediante `auto_read_file()`. Volverá a utilizar estos datos con formatos diferentes.

```python
import azureml.dataprep as dprep
t = dprep.auto_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(5)
```

Salida de ejemplo:

| | Column1 | Column2 | Column3 | Column4 | Column5 | Column6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | None | NO | NO | ENRS | NaN | NaN | NaN |   
|1| 10003.0 | 99999.0 | None | NO | NO | ENSO | NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | None | NO | JN | ENJA | 70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | None | NO | NO |      | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | None | NO | NO | ENSO | 59783.0 | 5350.0 |  500.0|

### <a name="delimited-file-example"></a>Ejemplo de archivo delimitado

En el código siguiente, la función [`write_to_csv()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow#write-to-csv-directory-path--datadestination--separator--str--------na--str----na---error--str----error------azureml-dataprep-api-dataflow-dataflow) se utiliza para escribir datos en un archivo delimitado.

```python
# Create a new data flow using `write_to_csv` 
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))

# Run the data flow to begin the write operation.
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(5)
```

Salida de ejemplo:

| | Column1 | Column2 | Column3 | Column4 | Column5 | Column6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | ERROR | NO | NO | ENRS | NaN    | NaN | NaN |   
|1| 10003.0 | 99999.0 | ERROR | NO | NO | ENSO |    NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | ERROR | NO | JN | ENJA |    70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | ERROR | NO | NO |     | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | ERROR | NO | NO | ENSO |    59783.0 | 5350.0 |  500.0|

En la salida anterior, aparecen varios errores en las columnas numéricas porque hay números que no se analizaron correctamente. Cuando se escriben en CSV, de forma predeterminada, los valores null se reemplazan con la cadena "ERROR".

Puede agregar parámetros en la llamada a la operación de escritura y especificar una cadena para que se utilice como representación de los valores null.

```python
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(5)
```

El código anterior produce este resultado:

| | Column1 | Column2 | Column3 | Column4 | Column5 | Column6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | BadData | NO | NO | ENRS | NaN  | NaN | NaN |   
|1| 10003.0 | 99999.0 | BadData | NO | NO | ENSO |  NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | BadData | NO | JN | ENJA |  70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | BadData | NO | NO |   | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | BadData | NO | NO | ENSO |  59783.0 | 5350.0 |  500.0|

### <a name="parquet-file-example"></a>Ejemplo de archivo PARQUET

De forma similar a `write_to_csv()`, la función [`write_to_parquet()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow#write-to-parquet-file-path--typing-union--datadestination--nonetype----none--directory-path--typing-union--datadestination--nonetype----none--single-file--bool---false--error--str----error---row-groups--int---0-----azureml-dataprep-api-dataflow-dataflow) devuelve un nuevo flujo de datos con un paso de escritura de tipo PARQUET que se ejecuta cuando se ejecuta el flujo de datos.

```python
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```

Ejecute el flujo de datos para iniciar la operación de escritura.

```python
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(5)
```

El código anterior produce este resultado:

|   | Column1 | Column2 | Column3 | Column4 | Column5 | Column6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |-------- |
|0| 10000.0 | 99999.0 | MiscreantData | NO | NO | ENRS | MiscreantData | MiscreantData | MiscreantData |
|1| 10003.0 | 99999.0 | MiscreantData | NO | NO | ENSO | MiscreantData | MiscreantData | MiscreantData |   
|2| 10010.0 | 99999.0 | MiscreantData | NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0 | 99999.0 | MiscreantData | NO| NO| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0 | 99999.0 | MiscreantData | NO| NO| ENSO|   59783.0|    5350.0| 500.0|

## <a name="configure-data-for-automated-machine-learning-training"></a>Configuración de datos para el entrenamiento de aprendizaje automático automatizado

Pase el archivo de datos recién escrito a un objeto [`AutoMLConfig`](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#automlconfig) como preparación para el entrenamiento de aprendizaje automático automatizado. 

En el ejemplo de código siguiente se muestra cómo convertir su flujo de datos en una trama de datos de Pandas que dividirá posteriormente en conjuntos de datos de entrenamiento y prueba para el entrenamiento de aprendizaje automático automatizado.

```Python
from azureml.train.automl import AutoMLConfig
from sklearn.model_selection import train_test_split

dflow = dprep.auto_read_file(path="")
X_dflow = dflow.keep_columns([feature_1,feature_2, feature_3])
y_dflow = dflow.keep_columns("target")

X_df = X_dflow.to_pandas_dataframe()
y_df = y_dflow.to_pandas_dataframe()

X_train, X_test, y_train, y_test = train_test_split(X_df, y_df, test_size=0.2, random_state=223)

# flatten y_train to 1d array
y_train.values.flatten()

#configure 
automated_ml_config = AutoMLConfig(task = 'regression',
                               X = X_train.values,  
                   y = y_train.values.flatten(),
                   iterations = 30,
                       Primary_metric = "AUC_weighted",
                       n_cross_validation = 5
                       )

```

Si no necesita ningún paso de preparación de datos intermedio como en el ejemplo anterior, puede pasar el flujo de datos directamente a `AutoMLConfig`.

```Python
automated_ml_config = AutoMLConfig(task = 'regression', 
                   X = X_dflow,   
                   y = y_dflow, 
                   iterations = 30, 
                   Primary_metric = "AUC_weighted",
                   n_cross_validation = 5
                   )
```

## <a name="next-steps"></a>Pasos siguientes
* Consulte la [introducción](https://aka.ms/data-prep-sdk) del SDK para ver ejemplos de uso y patrones de diseño. 
* Consulte el [tutorial](tutorial-auto-train-models.md) de aprendizaje automático automatizado para ver un ejemplo de modelo de regresión.
