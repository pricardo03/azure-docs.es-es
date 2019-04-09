---
title: 'Tutorial del modelo de regresión: Preparación de los datos'
titleSuffix: Azure Machine Learning service
description: En la primera parte de este tutorial, aprenderá a preparar datos en Python para el modelado de regresión mediante el SDK de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: sihhu
ms.author: MayMSFT
ms.reviewer: trbye
ms.date: 03/29/2019
ms.custom: seodec18
ms.openlocfilehash: d85f1ddcfe264e027a0f9d6c5f291d0005cad67c
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665022"
---
# <a name="tutorial-prepare-data-for-regression-modeling"></a>Tutorial: Preparación de datos para el modelado de regresión

En este tutorial, aprenderá a preparar datos para los modelos de regresión mediante el [SDK de preparación de datos de Azure Machine Learning para Python](https://aka.ms/data-prep-sdk). Realizará varias transformaciones para filtrar y combinar dos conjuntos de datos diferentes de taxis de la ciudad de Nueva York.

Este tutorial es la **primera de dos partes**. Después de completar la serie de tutoriales, puede predecir el costo de un viaje en taxi mediante el entrenamiento de un modelo sobre las características de los datos. Estas características incluyen el día y la hora de la recogida, el número de pasajeros y la ubicación de la recogida.

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Configurar un entorno de Python e importar los paquetes
> * Cargar dos conjuntos de datos con distintos nombres de campos
> * Limpiar los datos para quitar las anomalías
> * Transformar los datos mediante transformaciones inteligentes para crear nuevas características
> * Guardar el objeto de flujo de datos que se va a usar en un modelo de regresión

## <a name="prerequisites"></a>Requisitos previos

Vaya a [Configuración del entorno de desarrollo](#start) para leer los pasos del cuaderno, o utilice las siguientes instrucciones para obtener el cuaderno y ejecutarlo en Azure Notebooks o en su propio servidor de cuadernos. Para ejecutar el cuaderno, necesita:

* Un servidor de cuadernos de Python 3.6 con lo siguiente instalado:
    * El SDK de preparación de datos de Azure Machine Learning para Python
* El cuaderno del tutorial

Obtenga todos estos requisitos previos de cualquiera de las siguientes secciones.

* Uso de [Azure Notebooks](#azure)
* Uso del [propio servidor de Notebook](#server)

### <a name="azure"></a>Uso de Azure Notebooks: cuadernos de Jupyter gratuitos en la nube

Es fácil empezar a usar Azure Notebooks. El SDK de preparación de datos de Azure Machine Learning se instala y configura automáticamente en [Azure Notebooks](https://notebooks.azure.com/). Tanto la instalación como las actualizaciones futuras se administran automáticamente mediante servicios de Azure.

Después de completar los pasos siguientes, ejecute el cuaderno **tutorials/regression-part1-data-prep.ipynb** en el proyecto **Getting Started**.

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

### <a name="server"></a>Uso del propio servidor de Jupyter Notebook

Siga estos pasos para crear un servidor de Jupyter Notebook local en su equipo.  Después de completar los pasos siguientes, ejecute el cuaderno **tutorials/regression-part1-data-prep.ipynb**.

1. Complete los pasos de instalación del [[inicio rápido de Azure Machine Learning en Python](quickstart-run-local-notebook.md)](setup-create-workspace.md#python) para crear un entorno de Miniconda.  Puede omitir la sección **Creación de un área de trabajo** si lo desea, pero tenga en cuenta que la necesitará para la [parte 2](tutorial-auto-train-models.md) de esta serie de tutoriales.
1. Instale el SDK de preparación de datos en su entorno mediante `pip install azureml-dataprep`.
1. Clone [el repositorio de GitHub](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Inicie el servidor del cuaderno desde el directorio clonado.

    ```shell
    jupyter notebook
    ```

## <a name="start"></a>Configuración de su entorno de desarrollo

Toda la configuración para el trabajo de desarrollo puede realizarse en un cuaderno de Python. La configuración incluye las siguientes acciones:

* Instalación del SDK
* La importación de los paquetes de Python

### <a name="install-and-import-packages"></a>Instalación e importación de paquetes

Use lo siguiente para instalar los paquetes necesarios si aún no los tiene.

```shell
pip install azureml-dataprep==1.0.17
```

Importe el SDK.

```python
import azureml.dataprep as dprep
```

> [!IMPORTANT]
> Asegúrese de instalar la versión 1.0.17. Este tutorial no funciona con la versión más reciente (1.1.0).

## <a name="load-data"></a>Carga de datos

Descargue dos conjuntos de datos de taxis de Nueva York diferentes en objetos de flujo de datos. Los conjuntos de datos contienen campos ligeramente diferentes. El método `auto_read_file()` reconoce automáticamente el tipo de archivo de entrada.

```python
from IPython.display import display
dataset_root = "https://dprepdata.blob.core.windows.net/demo"

green_path = "/".join([dataset_root, "green-small/*"])
yellow_path = "/".join([dataset_root, "yellow-small/*"])

green_df_raw = dprep.read_csv(path=green_path, header=dprep.PromoteHeadersMode.GROUPED)
# auto_read_file automatically identifies and parses the file type, which is useful when you don't know the file type.
yellow_df_raw = dprep.auto_read_file(path=yellow_path)

display(green_df_raw.head(5))
display(yellow_df_raw.head(5))
```

Un objeto `Dataflow` es similar a una trama de datos y representa una serie de operaciones diferidas e inmutables sobre los datos. Las operaciones se pueden agregar mediante la invocación de los diferentes métodos de transformación y filtrado disponibles. El resultado de agregar una operación a `Dataflow` siempre es un nuevo objeto `Dataflow`.

## <a name="cleanse-data"></a>Limpieza de datos

Ahora rellene algunas variables con transformaciones de acceso directo que se aplicarán a todos los flujos de datos. La variable `drop_if_all_null` se utiliza para eliminar registros en los que todos los campos sean nulos. La variable `useful_columns` contiene una serie de descripciones de columnas que se mantienen en cada flujo de datos.

```python
all_columns = dprep.ColumnSelector(term=".*", use_regex=True)
drop_if_all_null = [all_columns, dprep.ColumnRelationship(dprep.ColumnRelationship.ALL)]
useful_columns = [
    "cost", "distance", "dropoff_datetime", "dropoff_latitude", "dropoff_longitude",
    "passengers", "pickup_datetime", "pickup_latitude", "pickup_longitude", "store_forward", "vendor"
]
```

Primero trabaja con los datos de los taxis verdes y los pone en una forma válida que se puede combinar con los datos de los taxis amarillos. Llame a las funciones `replace_na()`, `drop_nulls()` y `keep_columns()` mediante las variables de transformación de acceso directo que ha creado. Además, cambie el nombre de todas las columnas de la trama de datos para que coincidan con los nombres de la variable `useful_columns`.


```python
green_df = (green_df_raw
    .replace_na(columns=all_columns)
    .drop_nulls(*drop_if_all_null)
    .rename_columns(column_pairs={
        "VendorID": "vendor",
        "lpep_pickup_datetime": "pickup_datetime",
        "Lpep_dropoff_datetime": "dropoff_datetime",
        "lpep_dropoff_datetime": "dropoff_datetime",
        "Store_and_fwd_flag": "store_forward",
        "store_and_fwd_flag": "store_forward",
        "Pickup_longitude": "pickup_longitude",
        "Pickup_latitude": "pickup_latitude",
        "Dropoff_longitude": "dropoff_longitude",
        "Dropoff_latitude": "dropoff_latitude",
        "Passenger_count": "passengers",
        "Fare_amount": "cost",
        "Trip_distance": "distance"
     })
    .keep_columns(columns=useful_columns))
green_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vendor</th>
      <th>pickup_datetime</th>
      <th>dropoff_datetime</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>passengers</th>
      <th>distancia</th>
      <th>cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 08:14:37</td>
      <td>2013-08-01 09:09:06</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>21.25</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 09:13:00</td>
      <td>2013-08-01 11:38:00</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>.00</td>
      <td>74.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-01 09:48:00</td>
      <td>2013-08-01 09:49:00</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-01 10:38:35</td>
      <td>2013-08-01 10:38:51</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>3.25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-01 11:51:45</td>
      <td>2013-08-01 12:03:52</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>8.5</td>
    </tr>
  </tbody>
</table>
</div>

Realice los mismos pasos de transformación en los datos del taxi amarillo. Estas funciones aseguran que los datos nulos se quiten del conjunto de datos, lo que ayudará a aumentar la precisión del modelo de aprendizaje automático.

```python
yellow_df = (yellow_df_raw
    .replace_na(columns=all_columns)
    .drop_nulls(*drop_if_all_null)
    .rename_columns(column_pairs={
        "vendor_name": "vendor",
        "VendorID": "vendor",
        "vendor_id": "vendor",
        "Trip_Pickup_DateTime": "pickup_datetime",
        "tpep_pickup_datetime": "pickup_datetime",
        "Trip_Dropoff_DateTime": "dropoff_datetime",
        "tpep_dropoff_datetime": "dropoff_datetime",
        "store_and_forward": "store_forward",
        "store_and_fwd_flag": "store_forward",
        "Start_Lon": "pickup_longitude",
        "Start_Lat": "pickup_latitude",
        "End_Lon": "dropoff_longitude",
        "End_Lat": "dropoff_latitude",
        "Passenger_Count": "passengers",
        "passenger_count": "passengers",
        "Fare_Amt": "cost",
        "fare_amount": "cost",
        "Trip_Distance": "distance",
        "trip_distance": "distance"
    })
    .keep_columns(columns=useful_columns))
yellow_df.head(5)
```

Llame a la función `append_rows()` en los datos del taxi verde para agregar los datos del taxi amarillo. Se crea una nueva trama de datos combinada.

```python
combined_df = green_df.append_rows([yellow_df])
```

### <a name="convert-types-and-filter"></a>Conversión de tipos y filtro

Examine las estadísticas de resumen de las coordenadas de recogida y de llegada al destino para ver cómo se distribuyen los datos. Primero, defina un objeto `TypeConverter` para cambiar los campos de latitud y longitud a tipo decimal. A continuación, llame a la función `keep_columns()` para restringir la salida solo a los campos de latitud y longitud, y después llame a la función `get_profile()`. Estas llamadas de función crean una vista resumida del flujo de datos para mostrar solo los campos de latitud o longitud, lo que facilita la evaluación de las coordenadas que faltan o están fuera de ámbito.


```python
decimal_type = dprep.TypeConverter(data_type=dprep.FieldType.DECIMAL)
combined_df = combined_df.set_column_types(type_conversions={
    "pickup_longitude": decimal_type,
    "pickup_latitude": decimal_type,
    "dropoff_longitude": decimal_type,
    "dropoff_latitude": decimal_type
})
combined_df.keep_columns(columns=[
    "pickup_longitude", "pickup_latitude",
    "dropoff_longitude", "dropoff_latitude"
]).get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min</th>
      <th>max</th>
      <th>Recuento</th>
      <th>Falta el recuento</th>
      <th>No falta el recuento</th>
      <th>Falta el porcentaje</th>
      <th>Recuento de errores</th>
      <th>Recuento vacío</th>
      <th>Cuantil 0,1 %</th>
      <th>Cuantil 1 %</th>
      <th>Cuantil 5 %</th>
      <th>Cuantil 25 %</th>
      <th>Cuantil 50 %</th>
      <th>Cuantil 75 %</th>
      <th>Cuantil 95 %</th>
      <th>Cuantil 99 %</th>
      <th>Cuantil 99,9 %</th>
      <th>Desviación estándar</th>
      <th>Media</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-115.179337</td>
      <td>0.000000</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-88.114046</td>
      <td>-73.961840</td>
      <td>-73.961964</td>
      <td>-73.947693</td>
      <td>-73.922097</td>
      <td>-73.846670</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>18.792672</td>
      <td>-68.833579</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>0.000000</td>
      <td>40.919121</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>40.682889</td>
      <td>40.675541</td>
      <td>40.721075</td>
      <td>40.756159</td>
      <td>40.803909</td>
      <td>40.849406</td>
      <td>40.870681</td>
      <td>40.891244</td>
      <td>10.345967</td>
      <td>37.936742</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-115.179337</td>
      <td>0.000000</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-87.699611</td>
      <td>-73.984734</td>
      <td>-73.985777</td>
      <td>-73.956250</td>
      <td>-73.928948</td>
      <td>-73.866208</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>18.696526</td>
      <td>-68.896978</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>0.000000</td>
      <td>41.008934</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>40.662763</td>
      <td>40.654851</td>
      <td>40.717821</td>
      <td>40.756534</td>
      <td>40.784688</td>
      <td>40.852437</td>
      <td>40.879289</td>
      <td>40.937291</td>
      <td>10.290780</td>
      <td>37.963774</td>
    </tr>
  </tbody>
</table>



A partir de la salida de las estadísticas de resumen, se puede ver que faltan coordenadas y que hay coordenadas que no están en la ciudad de Nueva York (esto se determina a partir del análisis subjetivo). Filtre las coordenadas para aquellas ubicaciones que están fuera de los límites de la ciudad. Encadene los comandos de filtro de columna dentro de la función `filter()` y defina los límites mínimo y máximo para cada campo. A continuación, llame a la función `get_profile()` de nuevo para comprobar la transformación.


```python
latlong_filtered_df = (combined_df
    .drop_nulls(
        columns=["pickup_longitude", "pickup_latitude", "dropoff_longitude", "dropoff_latitude"],
        column_relationship=dprep.ColumnRelationship(dprep.ColumnRelationship.ANY)
    )
    .filter(dprep.f_and(
        dprep.col("pickup_longitude") <= -73.72,
        dprep.col("pickup_longitude") >= -74.09,
        dprep.col("pickup_latitude") <= 40.88,
        dprep.col("pickup_latitude") >= 40.53,
        dprep.col("dropoff_longitude") <= -73.72,
        dprep.col("dropoff_longitude") >= -74.09,
        dprep.col("dropoff_latitude") <= 40.88,
        dprep.col("dropoff_latitude") >= 40.53
    )))
latlong_filtered_df.keep_columns(columns=[
    "pickup_longitude", "pickup_latitude",
    "dropoff_longitude", "dropoff_latitude"
]).get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min</th>
      <th>max</th>
      <th>Recuento</th>
      <th>Falta el recuento</th>
      <th>No falta el recuento</th>
      <th>Falta el porcentaje</th>
      <th>Recuento de errores</th>
      <th>Recuento vacío</th>
      <th>Cuantil 0,1 %</th>
      <th>Cuantil 1 %</th>
      <th>Cuantil 5 %</th>
      <th>Cuantil 25 %</th>
      <th>Cuantil 50 %</th>
      <th>Cuantil 75 %</th>
      <th>Cuantil 95 %</th>
      <th>Cuantil 99 %</th>
      <th>Cuantil 99,9 %</th>
      <th>Desviación estándar</th>
      <th>Media</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.078156</td>
      <td>-73.736481</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.076314</td>
      <td>-73.962542</td>
      <td>-73.962893</td>
      <td>-73.948975</td>
      <td>-73.927856</td>
      <td>-73.866662</td>
      <td>-73.830438</td>
      <td>-73.823160</td>
      <td>-73.769750</td>
      <td>0.048711</td>
      <td>-73.913865</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.575485</td>
      <td>40.879852</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.632884</td>
      <td>40.713105</td>
      <td>40.711600</td>
      <td>40.721403</td>
      <td>40.758142</td>
      <td>40.805145</td>
      <td>40.848855</td>
      <td>40.867567</td>
      <td>40.877690</td>
      <td>0.048348</td>
      <td>40.765226</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.085747</td>
      <td>-73.720871</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.078828</td>
      <td>-73.985650</td>
      <td>-73.985813</td>
      <td>-73.959041</td>
      <td>-73.936681</td>
      <td>-73.884846</td>
      <td>-73.815507</td>
      <td>-73.776697</td>
      <td>-73.733471</td>
      <td>0.055961</td>
      <td>-73.920718</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.583530</td>
      <td>40.879734</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.597741</td>
      <td>40.695376</td>
      <td>40.695115</td>
      <td>40.727549</td>
      <td>40.758160</td>
      <td>40.788378</td>
      <td>40.850372</td>
      <td>40.867968</td>
      <td>40.878586</td>
      <td>0.050462</td>
      <td>40.759487</td>
    </tr>
  </tbody>
</table>

### <a name="split-and-rename-columns"></a>División y cambio de nombre de las columnas

Examine el perfil de datos para la columna `store_forward`. Este campo es un indicador booleano que es `Y` cuando el taxi no tenía conexión con el servidor después del viaje y, por lo tanto, tenía que almacenar los datos del viaje en la memoria y después reenviarlos al servidor cuando estaba conectado.


```python
latlong_filtered_df.keep_columns(columns='store_forward').get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min</th>
      <th>max</th>
      <th>Recuento</th>
      <th>Falta el recuento</th>
      <th>No falta el recuento</th>
      <th>Falta el porcentaje</th>
      <th>Recuento de errores</th>
      <th>Recuento vacío</th>
      <th>Cuantil 0,1 %</th>
      <th>Cuantil 1 %</th>
      <th>Cuantil 5 %</th>
      <th>Cuantil 25 %</th>
      <th>Cuantil 50 %</th>
      <th>Cuantil 75 %</th>
      <th>Cuantil 95 %</th>
      <th>Cuantil 99 %</th>
      <th>Cuantil 99,9 %</th>
      <th>Desviación estándar</th>
      <th>Media</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>Y</td>
      <td>7059.0</td>
      <td>99.0</td>
      <td>6960.0</td>
      <td>0.014025</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>



Observe que la salida del perfil de datos de la columna `store_forward` muestra que los datos son incoherentes y que faltan valores o son nulos. Utilice las funciones `replace()` y `fill_nulls()` para sustituir estos valores por la cadena "N":


```python
replaced_stfor_vals_df = latlong_filtered_df.replace(columns="store_forward", find="0", replace_with="N").fill_nulls("store_forward", "N")
```

Ejecute la función `replace` en el campo `distance`. La función cambia el formato de los valores de distancia que están etiquetados incorrectamente como `.00` y rellena todos los valores NULL con ceros. Convierta el campo `distance` a un formato numérico. Estos puntos de datos incorrectos son probablemente anomalías del sistema de recopilación de datos de los taxis.


```python
replaced_distance_vals_df = replaced_stfor_vals_df.replace(columns="distance", find=".00", replace_with=0).fill_nulls("distance", 0)
replaced_distance_vals_df = replaced_distance_vals_df.to_number(["distance"])
```

Divida los valores de fecha y hora de recogida y destino en columnas de fecha y hora respectivas. Use la función `split_column_by_example()` para realizar la división. En este caso, el parámetro opcional `example` de la función `split_column_by_example()` se omite. Por lo tanto, la función determina automáticamente dónde dividir en función de los datos.


```python
time_split_df = (replaced_distance_vals_df
    .split_column_by_example(source_column="pickup_datetime")
    .split_column_by_example(source_column="dropoff_datetime"))
time_split_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vendor</th>
      <th>pickup_datetime</th>
      <th>pickup_datetime_1</th>
      <th>pickup_datetime_2</th>
      <th>dropoff_datetime</th>
      <th>dropoff_datetime_1</th>
      <th>dropoff_datetime_2</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>passengers</th>
      <th>distancia</th>
      <th>cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 17:22:00</td>
      <td>2013-08-01</td>
      <td>17:22:00</td>
      <td>2013-08-01 17:22:00</td>
      <td>2013-08-01</td>
      <td>17:22:00</td>
      <td>N</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>1</td>
      <td>0.0</td>
      <td>2.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 17:24:00</td>
      <td>2013-08-01</td>
      <td>17:24:00</td>
      <td>2013-08-01 17:25:00</td>
      <td>2013-08-01</td>
      <td>17:25:00</td>
      <td>N</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>1</td>
      <td>0.0</td>
      <td>2.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-06 06:51:19</td>
      <td>2013-08-06</td>
      <td>06:51:19</td>
      <td>2013-08-06 06:51:36</td>
      <td>2013-08-06</td>
      <td>06:51:36</td>
      <td>N</td>
      <td>-73.937721</td>
      <td>40.758404</td>
      <td>-73.937721</td>
      <td>40.758369</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-06 13:26:34</td>
      <td>2013-08-06</td>
      <td>13:26:34</td>
      <td>2013-08-06 13:26:57</td>
      <td>2013-08-06</td>
      <td>13:26:57</td>
      <td>N</td>
      <td>-73.937691</td>
      <td>40.758419</td>
      <td>-73.937790</td>
      <td>40.758358</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-06 13:27:53</td>
      <td>2013-08-06</td>
      <td>13:27:53</td>
      <td>2013-08-06 13:28:08</td>
      <td>2013-08-06</td>
      <td>13:28:08</td>
      <td>N</td>
      <td>-73.937805</td>
      <td>40.758396</td>
      <td>-73.937775</td>
      <td>40.758450</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
  </tbody>
</table>
</div>

Cambie el nombre de las columnas que ha generado la función `split_column_by_example()` para usar nombres significativos.

```python
renamed_col_df = (time_split_df
    .rename_columns(column_pairs={
        "pickup_datetime_1": "pickup_date",
        "pickup_datetime_2": "pickup_time",
        "dropoff_datetime_1": "dropoff_date",
        "dropoff_datetime_2": "dropoff_time"
    }))
renamed_col_df.head(5)
```

Llame a la función `get_profile()` para ver el resumen estadístico completo después de todos los pasos de limpieza.

```python
renamed_col_df.get_profile()
```

## <a name="transform-data"></a>Transformación de datos

Divida la fecha de recogida y de llegada al destino en los valores de día de la semana, días del mes y mes. Para obtener el valor del día de la semana, utilice la función `derive_column_by_example()`. La función toma un parámetro de matriz de objetos de ejemplo que definen los datos de entrada y la salida preferida. La función determina automáticamente la transformación preferida. Para las columnas del tiempo de recogida y de llegada al destino, divida el tiempo en horas, minutos y segundos mediante la función `split_column_by_example()` sin ningún parámetro de ejemplo.

Después de generar las nuevas características, utilice la función `drop_columns()` para eliminar los campos originales, ya que se prefieren las nuevas características generadas. Cambie el nombre del resto de los campos para utilizar descripciones significativas.

La transformación de los datos de esta manera para crear nuevas características basadas en el tiempo mejorará la precisión del modelo de aprendizaje automático. Por ejemplo, generar una nueva característica para el día de la semana ayudará a establecer una relación entre el día de la semana y el precio del taxi, que a menudo es más caro en ciertos días de la semana debido a la alta demanda.


```python
transformed_features_df = (renamed_col_df
    .derive_column_by_example(
        source_columns="pickup_date",
        new_column_name="pickup_weekday",
        example_data=[("2009-01-04", "Sunday"), ("2013-08-22", "Thursday")]
    )
    .derive_column_by_example(
        source_columns="dropoff_date",
        new_column_name="dropoff_weekday",
        example_data=[("2013-08-22", "Thursday"), ("2013-11-03", "Sunday")]
    )

    .split_column_by_example(source_column="pickup_time")
    .split_column_by_example(source_column="dropoff_time")
    # The following two calls to split_column_by_example reference the column names generated from the previous two calls.
    .split_column_by_example(source_column="pickup_time_1")
    .split_column_by_example(source_column="dropoff_time_1")
    .drop_columns(columns=[
        "pickup_date", "pickup_time", "dropoff_date", "dropoff_time",
        "pickup_date_1", "dropoff_date_1", "pickup_time_1", "dropoff_time_1"
    ])

    .rename_columns(column_pairs={
        "pickup_date_2": "pickup_month",
        "pickup_date_3": "pickup_monthday",
        "pickup_time_1_1": "pickup_hour",
        "pickup_time_1_2": "pickup_minute",
        "pickup_time_2": "pickup_second",
        "dropoff_date_2": "dropoff_month",
        "dropoff_date_3": "dropoff_monthday",
        "dropoff_time_1_1": "dropoff_hour",
        "dropoff_time_1_2": "dropoff_minute",
        "dropoff_time_2": "dropoff_second"
    }))

transformed_features_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vendor</th>
      <th>pickup_datetime</th>
      <th>pickup_weekday</th>
      <th>pickup_hour</th>
      <th>pickup_minute</th>
      <th>pickup_second</th>
      <th>dropoff_datetime</th>
      <th>dropoff_weekday</th>
      <th>dropoff_hour</th>
      <th>dropoff_minute</th>
      <th>dropoff_second</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>passengers</th>
      <th>distancia</th>
      <th>cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 17:22:00</td>
      <td>Jueves</td>
      <td>17</td>
      <td>22</td>
      <td>00</td>
      <td>2013-08-01 17:22:00</td>
      <td>Jueves</td>
      <td>17</td>
      <td>22</td>
      <td>00</td>
      <td>N</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>1</td>
      <td>0.0</td>
      <td>2.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 17:24:00</td>
      <td>Jueves</td>
      <td>17</td>
      <td>24</td>
      <td>00</td>
      <td>2013-08-01 17:25:00</td>
      <td>Jueves</td>
      <td>17</td>
      <td>25</td>
      <td>00</td>
      <td>N</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>1</td>
      <td>0.0</td>
      <td>2.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-06 06:51:19</td>
      <td>Martes</td>
      <td>06</td>
      <td>51</td>
      <td>19</td>
      <td>2013-08-06 06:51:36</td>
      <td>Martes</td>
      <td>06</td>
      <td>51</td>
      <td>36</td>
      <td>N</td>
      <td>-73.937721</td>
      <td>40.758404</td>
      <td>-73.937721</td>
      <td>40.758369</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-06 13:26:34</td>
      <td>Martes</td>
      <td>13</td>
      <td>26</td>
      <td>34</td>
      <td>2013-08-06 13:26:57</td>
      <td>Martes</td>
      <td>13</td>
      <td>26</td>
      <td>57</td>
      <td>N</td>
      <td>-73.937691</td>
      <td>40.758419</td>
      <td>-73.937790</td>
      <td>40.758358</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-06 13:27:53</td>
      <td>Martes</td>
      <td>13</td>
      <td>27</td>
      <td>53</td>
      <td>2013-08-06 13:28:08</td>
      <td>Martes</td>
      <td>13</td>
      <td>28</td>
      <td>08</td>
      <td>N</td>
      <td>-73.937805</td>
      <td>40.758396</td>
      <td>-73.937775</td>
      <td>40.758450</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
  </tbody>
</table>
</div>

Observe que los datos muestran que los componentes de fecha y hora de recogida y de llegada al destino producidos a partir de las transformaciones derivadas son correctos. Quite las columnas `pickup_datetime` y `dropoff_datetime` porque ya no son necesarias (las características de tiempo granular como hora, minuto y segundo son más útiles para el entrenamiento de modelos).


```python
processed_df = transformed_features_df.drop_columns(columns=["pickup_datetime", "dropoff_datetime"])
```

Utilice la funcionalidad de inferencia de tipo para comprobar automáticamente el tipo de datos de cada campo y muestre los resultados de la inferencia.


```python
type_infer = processed_df.builders.set_column_types()
type_infer.learn()
type_infer
```

La salida resultante de `type_infer` es como sigue.

    Column types conversion candidates:
    'pickup_weekday': [FieldType.STRING],
    'pickup_hour': [FieldType.DECIMAL],
    'pickup_minute': [FieldType.DECIMAL],
    'pickup_second': [FieldType.DECIMAL],
    'dropoff_hour': [FieldType.DECIMAL],
    'dropoff_minute': [FieldType.DECIMAL],
    'dropoff_second': [FieldType.DECIMAL],
    'store_forward': [FieldType.STRING],
    'pickup_longitude': [FieldType.DECIMAL],
    'dropoff_longitude': [FieldType.DECIMAL],
    'passengers': [FieldType.DECIMAL],
    'distance': [FieldType.DECIMAL],
    'vendor': [FieldType.STRING],
    'dropoff_weekday': [FieldType.STRING],
    'pickup_latitude': [FieldType.DECIMAL],
    'dropoff_latitude': [FieldType.DECIMAL],
    'cost': [FieldType.DECIMAL]

Los resultados de la inferencia parecen correctos según los datos. A continuación, aplique las conversiones de tipo al flujo de datos.


```python
type_converted_df = type_infer.to_dataflow()
type_converted_df.get_profile()
```

Antes de empaquetar el flujo de datos, ejecute dos filtros finales en el conjunto de datos. Para eliminar los puntos de datos incorrectamente capturados, filtre el flujo de datos en los registros donde los valores de variable `cost` y `distance` son mayores que cero. Este paso mejorará significativamente la precisión del modelo de Machine Learning, ya que los puntos de datos con un costo o distancia cero representan valores atípicos importantes que desvían la precisión de las predicciones.

```python
final_df = type_converted_df.filter(dprep.col("distance") > 0)
final_df = final_df.filter(dprep.col("cost") > 0)
```

Ahora tiene un objeto de flujo de datos totalmente transformado y preparado para utilizarlo en un modelo de Machine Learning. El SDK incluye funcionalidad de serialización de objetos, que se utiliza como se muestra en el código siguiente.

```python
import os

file_path = os.path.join(os.getcwd(), "dflows.dprep")
package = dprep.Package([final_df])
package.save(file_path)
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para continuar con la segunda parte del tutorial, necesita el archivo **dflows.dprep** en el directorio actual.

Si no piensa continuar con la segunda parte, elimine el archivo **dflows.dprep** del directorio actual. Elimine este archivo tanto si está realizando la ejecución localmente como en [Azure Notebooks](https://notebooks.azure.com/).

## <a name="next-steps"></a>Pasos siguientes

En la primera parte de este tutorial, ha:

> [!div class="checklist"]
> * Configurar su entorno de desarrollo
> * Cargado y limpiado conjuntos de datos.
> * Usado transformaciones inteligentes para predecir la lógica basada en un ejemplo.
> * Fusionado y empaquetado conjunto de datos para el entrenamiento en aprendizaje automático.

Está listo para usar los datos de entrenamiento en la segunda parte del tutorial:

> [!div class="nextstepaction"]
> [Tutorial (segunda parte): Entrenamiento del modelo de regresión](tutorial-auto-train-models.md)
