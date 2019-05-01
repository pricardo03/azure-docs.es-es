---
title: 'Transformaciones: SDK de Python para la preparación de datos'
titleSuffix: Azure Machine Learning service
description: Aprenda a transformar datos con Azure Machine Learning Data Prep SDK. Utilice métodos de transformación para agregar columnas, filtrar las filas o columnas no deseadas y atribuir los valores que faltan.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: d2bd271557ae0deefeb12a2dc7343c46fbd35363
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60817553"
---
# <a name="transform-data-with-the-azure-machine-learning-data-prep-sdk"></a>Transformación de datos con Azure Machine Learning Data Prep SDK

En este artículo, obtendrá información sobre diferentes métodos para transformar los datos mediante el SDK de Azure Machine Learning Data Prep. El SDK ofrece funciones que simplifican la forma de agregar columnas, filtrar las columnas o filas no deseadas e imputar valores ausentes. Para ver la documentación de referencia para el SDK, consulte el [Introducción](https://aka.ms/data-prep-sdk).

Este tema de procedimientos muestra ejemplos de las siguientes tareas:

- Adición de una columna mediante una expresión
- [Atribución de valores que faltan](#impute-missing-values)
- [Derivación de columnas por ejemplos](#derive-column-by-example)
- [Filtrado](#filtering)
- [Transformaciones personalizadas de Python](#custom-python-transforms)

## <a name="add-column-using-an-expression"></a>Adición de una columna mediante una expresión

Azure Machine Learning Data Prep SDK incluye expresiones `substring` que puede usar para calcular un valor de las columnas existentes y, después, colocar ese valor en una columna nueva. En este ejemplo cargará los datos e intentará agregar columnas a los datos de entrada.

```python
import azureml.dataprep as dprep

# loading data
dflow = dprep.read_csv(path=r'data\crime0-10.csv')
dflow.head(3)
```

||ID|Case Number|Date|Block|IUCR|Primary Type|DESCRIPCIÓN|Location Description|Arrest|Domestic|...|Ward|Community Area|FBI Code|X Coordinate|Y Coordinate|Year|Updated On|Latitud|Longitud|Location|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
|0|10140490|HY329907|07/05/2015 11:50:00 PM|050XX N NEWLAND AVE|0820|THEFT|$500 AND UNDER|STREET|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 12:42:46 PM|41.973309466|-87.800174996|(41.973309466, -87.800174996)|
|1|10139776|HY329265|07/05/2015 11:30:00 PM|011XX W MORSE AVE|0460|BATTERY|SIMPLE|STREET|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017, -87.65955018)|
|2|10140270|HY329253|07/05/2015 11:20:00 PM|121XX S FRONT AVE|0486|BATTERY|DOMESTIC BATTERY SIMPLE|STREET|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|


Use la expresión `substring(start, length)` para extraer el prefijo de la columna Case Number y poner la cadena en una nueva columna `Case Category`. Al pasar la variable `substring_expression` al parámetro `expression` se crea una nueva columna calculada que ejecuta la expresión en cada registro.

```python
substring_expression = dprep.col('Case Number').substring(0, 2)
case_category = dflow.add_column(new_column_name='Case Category',
                                    prior_column='Case Number',
                                    expression=substring_expression)
case_category.head(3)
```

||ID|Case Number|Case Category|Date|Block|IUCR|Primary Type|DESCRIPCIÓN|Location Description|Arrest|Domestic|...|Ward|Community Area|FBI Code|X Coordinate|Y Coordinate|Year|Updated On|Latitud|Longitud|Location|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|HY|07/05/2015 11:50:00 PM|050XX N NEWLAND AVE|0820|THEFT|$500 AND UNDER|STREET|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 12:42:46 PM|41.973309466|-87.800174996|(41.973309466, -87.800174996)|
|1|10139776|HY329265|HY|07/05/2015 11:30:00 PM|011XX W MORSE AVE|0460|BATTERY|SIMPLE|STREET|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017, -87.65955018)|
|2|10140270|HY329253|HY|07/05/2015 11:20:00 PM|121XX S FRONT AVE|0486|BATTERY|DOMESTIC BATTERY SIMPLE|STREET|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|



Use la expresión `substring(start)` para extraer solo el número de la columna Case Number y crear una nueva columna. Conviértala a un tipo de datos numérico mediante la función `to_number()` y pase el nombre de la columna de cadena como parámetro.

```python
substring_expression2 = dprep.col('Case Number').substring(2)
case_id = dflow.add_column(new_column_name='Case Id',
                              prior_column='Case Number',
                              expression=substring_expression2)
case_id = case_id.to_number('Case Id')
```

## <a name="impute-missing-values"></a>Atribución de valores que faltan

El SDK puede atribuir los valores que faltan a las columnas especificadas. En este ejemplo se cargarán los valores de latitud y longitud y, a continuación, se intentarán atribuir los valores que faltan a los datos de entrada.

```python
import azureml.dataprep as dprep

# loading input data
dflow = dprep.read_csv(r'data\crime0-10.csv')
dflow = dflow.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
dflow = dflow.to_number(['Latitude', 'Longitude'])
dflow.head(3)
```

||ID|Arrest|Latitud|Longitud|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|NaN|NaN|

En el tercer registro faltan los valores de latitud y longitud. Para atribuir los valores que faltan, usa [ `ImputeMissingValuesBuilder` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py) para obtener información sobre una expresión de tipo fixed. Puede atribuir las columnas con un valor calculado `MIN`, `MAX` o `MEAN` o un valor `CUSTOM`. Cuando se especifica `group_by_columns`, se atribuirán los valores que faltan por grupo a `MIN`, `MAX` y `MEAN` calculados por grupo.

Compruebe el `MEAN` valor de la columna de latitud utilizando el [ `summarize()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#summarize-summary-columns--typing-union-typing-list-azureml-dataprep-api-dataflow-summarycolumnsvalue---nonetype----none--group-by-columns--typing-union-typing-list-str---nonetype----none--join-back--bool---false--join-back-columns-prefix--typing-union-str--nonetype----none-----azureml-dataprep-api-dataflow-dataflow) función. Esta función acepta una matriz de columnas en el parámetro `group_by_columns` para especificar el nivel de agregación. El parámetro `summary_columns` acepta una llamada `SummaryColumnsValue`. Esta llamada a la función especifica el nombre de la columna actual, el nuevo nombre del campo calculado y el valor de `SummaryFunction` que se va a realizar.

```python
dflow_mean = dflow.summarize(group_by_columns=['Arrest'],
                       summary_columns=[dprep.SummaryColumnsValue(column_id='Latitude',
                                                                 summary_column_name='Latitude_MEAN',
                                                                 summary_function=dprep.SummaryFunction.MEAN)])
dflow_mean = dflow_mean.filter(dprep.col('Arrest') == 'false')
dflow_mean.head(1)
```

||Arrest|Latitude_MEAN|
|-----|-----|----|
|0|false|41.878961|

Si el valor de latitudes `MEAN` se ve preciso, use la función `ImputeColumnArguments` para atribuirlo. Esta función acepta una cadena `column_id` y `ReplaceValueFunction` para especificar el tipo de atribución. En cuanto al valor de longitud que falta, debe atribuir 42 en función de la información externa.

Los pasos atribuidos se pueden encadenar en un objeto `ImputeMissingValuesBuilder` mediante la función de generador `impute_missing_values()`. El parámetro `impute_columns` acepta una matriz de objetos `ImputeColumnArguments`. Llame a la función `learn()` para almacenar los pasos de atribución y luego aplíquela a un objeto de flujo de datos mediante `to_dataflow()`.

```python
# impute with MEAN
impute_mean = dprep.ImputeColumnArguments(column_id='Latitude',
                                          impute_function=dprep.ReplaceValueFunction.MEAN)
# impute with custom value 42
impute_custom = dprep.ImputeColumnArguments(column_id='Longitude',
                                            custom_impute_value=42)
# get instance of ImputeMissingValuesBuilder
impute_builder = dflow.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])

impute_builder.learn()
dflow_imputed = impute_builder.to_dataflow()
dflow_imputed.head(3)
```

||ID|Arrest|Latitud|Longitud|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|41.878961|42.000000|

Como se muestra en el resultado anterior, la latitud que faltan se ha asignado al valor `MEAN` del grupo `Arrest=='false'`. La longitud que falta se atribuye a 42.

```python
imputed_longitude = dflow_imputed.to_pandas_dataframe()['Longitude'][2]
assert imputed_longitude == 42
```

## <a name="derive-column-by-example"></a>Derivación de columnas por ejemplos

Una de las herramientas más avanzadas en el SDK de preparación de datos de Azure Machine Learning es la capacidad de derivar columnas mediante ejemplos con los resultados deseados. Esto le permite dar al SDK un ejemplo para que pueda generar código y así conseguir la transformación prevista.

```python
import azureml.dataprep as dprep
dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/BostonWeather.csv')
dflow.head(4)
```

||DATE|REPORTTPYE|HOURLYDRYBULBTEMPF|HOURLYRelativeHumidity|HOURLYWindSpeed|
|----|----|----|----|----|----|
|0|1/1/2015 0:54|FM-15|22|50|10|
|1|1/1/2015 1:00|FM-12|22|50|10|
|2|1/1/2015 1:54|FM-15|22|50|10|
|3|1/1/2015 2:54|FM-15|22|50|11|

Suponga que necesita combinar este archivo con un conjunto de datos donde la fecha y hora están en un formato "Mar 10, 2018 | 2AM-4AM".

```python
builder = dflow.builders.derive_column_by_example(source_columns=['DATE'], new_column_name='date_timerange')
builder.add_example(source_data=dflow.iloc[1], example_value='Jan 1, 2015 12AM-2AM')
builder.preview(count=5) 
```

||DATE|date_timerange|
|----|----|----|
|0|1/1/2015 0:54|Jan 1, 2015 12AM-2AM|
|1|1/1/2015 1:00|Jan 1, 2015 12AM-2AM|
|2|1/1/2015 1:54|Jan 1, 2015 12AM-2AM|
|3|1/1/2015 2:54|Jan 1, 2015 2AM-4AM|
|4|1/1/2015 3:54|Jan 1, 2015 2AM-4AM|

El código anterior crea primero un generador para la columna derivada. Ha proporcionado una matriz de columnas de origen que hay que tener en cuenta (`DATE`) y un nombre para la nueva columna que se va a agregar. Tal como se indicó en el primer ejemplo, debe pasar la segunda fila (índice 1) y proporcionar un valor esperado para la columna derivada.

Finalmente, debe llamar a `builder.preview(skip=30, count=5)` y así podrá ver la columna derivada junto a la columna de origen. El formato parece ser correcto, pero solo verá los valores de la misma fecha "Jan 1, 2015".

Ahora, pase el número de filas que desea `skip` desde arriba para ver las filas más abajo.

> [!NOTE]
> La función preview() omite las filas, pero no volver a numerar el índice de salida. En el ejemplo siguiente, el índice 0 en la tabla corresponde al índice 30 en el flujo de datos.

```python
builder.preview(skip=30, count=5)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|Jan 1, 2015 10PM-12AM|
|1|1/1/2015 23:54|Jan 1, 2015 10PM-12AM|
|2|1/1/2015 23:59|Jan 1, 2015 10PM-12AM|
|3|1/2/2015 0:54|Feb 1, 2015 12AM-2AM|
|4|1/2/2015 1:00|Feb 1, 2015 12AM-2AM|

Aquí puede ver un problema con el programa generado. Basado únicamente en el ejemplo que proporcionó anteriormente, el programa derivado ha elegido analizar la fecha como "Día/Mes/Año", que no es lo que se quiere hacer en este caso. Para corregir este problema, como destino un índice de registro específico y proporcionan otro ejemplo utilizando el `add_example()` funcionan en el `builder` variable.

```python
builder.add_example(source_data=dflow.iloc[3], example_value='Jan 2, 2015 12AM-2AM')
builder.preview(skip=30, count=5)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|Jan 1, 2015 10PM-12AM|
|1|1/1/2015 23:54|Jan 1, 2015 10PM-12AM|
|2|1/1/2015 23:59|Jan 1, 2015 10PM-12AM|
|3|1/2/2015 0:54|Jan 2, 2015 12AM-2AM|
|4|1/2/2015 1:00|Jan 2, 2015 12AM-2AM|

Ahora las filas se controlan correctamente ' 2/1/2015' como "2 de enero de 2015", pero si echa un vistazo más allá de índice 76 de la columna derivada, verá que los valores al final no tienen nada en la columna derivada.

```python
builder.preview(skip=75, count=5)
```


||DATE|date_timerange|
|-----|-----|-----|
|0|1/3/2015 7:00|3 de enero de 2015 6: 00 - 8 a. M.|
|1|1/3/2015 7:54|3 de enero de 2015 6: 00 - 8 a. M.|
|2|1/29/2015 6:54|None|
|3|1/29/2015 7:00|None|
|4|1/29/2015 7:54|None|

```python
builder.add_example(source_data=dflow.iloc[77], example_value='Jan 29, 2015 6AM-8AM')
builder.preview(skip=75, count=5)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/3/2015 7:00|3 de enero de 2015 6: 00 - 8 a. M.|
|1|1/3/2015 7:54|3 de enero de 2015 6: 00 - 8 a. M.|
|2|1/29/2015 6:54|29 de enero de 2015 6: 00 - 8 a. M.|
|3|1/29/2015 7:00|29 de enero de 2015 6: 00 - 8 a. M.|
|4|1/29/2015 7:54|29 de enero de 2015 6: 00 - 8 a. M.|

 Para ver una lista de derivaciones de ejemplo actual llame a `list_examples()` en el objeto de generador.

```python
examples = builder.list_examples()
```

| |DATE|ejemplo|example_id|
| -------- | -------- | -------- | -------- |
|0|1/1/2015 1:00|Jan 1, 2015 12AM-2AM|-1|
|1|1/2/2015 0:54|Jan 2, 2015 12AM-2AM|-2|
|2|1/29/2015 20:54|Jan 29, 2015 8PM-10PM|-3|


En algunos casos si desea eliminar los ejemplos que son incorrectos, puede pasar cualquiera `example_row` desde el pandas.dataframe, o `example_id` valor. Por ejemplo, si ejecuta `builder.delete_example(example_id=-1)`, elimina el primer ejemplo de transformación.


Llamar a `to_dataflow()` en el generador, que devuelve un flujo de datos con las columnas derivadas que desee agregar.

```python
dflow = builder.to_dataflow()
df = dflow.to_pandas_dataframe()
```

## <a name="filtering"></a>Filtros

El SDK incluye los métodos [ `drop_columns()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#drop-columns-columns--multicolumnselection-----azureml-dataprep-api-dataflow-dataflow) y [ `filter()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py) para que pueda filtrar las columnas o filas.

### <a name="initial-setup"></a>Configuración inicial

```python
import azureml.dataprep as dprep
from datetime import datetime
dflow = dprep.read_csv(path='https://dprepdata.blob.core.windows.net/demo/green-small/*')
dflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Store_and_fwd_flag|RateCodeID|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|None|None|None|None|None|None|None|None|None|None|None|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|N|1|0|0|0|0|1|.00|0|0|21.25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|N|1|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|N|5|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|N|1|0|0|0|0|1|.00|0|0|3.25|

### <a name="filtering-columns"></a>Filtrado de columnas

Para filtrar las columnas, use `drop_columns()`. Este método toma una lista de columnas para quitar o un argumento más complejo denominado [ `ColumnSelector` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.columnselector?view=azure-dataprep-py).

#### <a name="filtering-columns-with-list-of-strings"></a>Filtrado de columnas con la lista de cadenas

En este ejemplo, `drop_columns()` toma una lista de cadenas. Cada cadena debe coincidir exactamente con la columna que se desea anular.

```python
dflow = dflow.drop_columns(['Store_and_fwd_flag', 'RateCodeID'])
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|None|None|None|None|None|None|None|None|None|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|0|0|0|0|1|.00|0|0|21.25|

#### <a name="filtering-columns-with-regex"></a>Filtrado de columnas con expresiones regulares

También puede usar la expresión `ColumnSelector` para anular columnas que coincidan con una expresión regular. En este ejemplo se anulan todas las columnas que coinciden con la expresión `Column*|.*longitude|.*latitude`.

```python
dflow = dflow.drop_columns(dprep.ColumnSelector('Column*|.*longitud|.*latitude', True, True))
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|None|None|None|None|None|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|1|.00|0|0|21.25|

## <a name="filtering-rows"></a>Filtrado de filas

Para filtrar las filas, utilice `filter()`. Este método toma una expresión de Azure Machine Learning Data Prep SDK como argumento y devuelve un nuevo flujo de datos con las filas que la expresión evalúa como True. Las expresiones se crean mediante los generadores de expresiones (`col`, `f_not`, `f_and`, `f_or`) y operadores regulares (>, <, >=, <=, ==, ¡¡¡¡, !=).

### <a name="filtering-rows-with-simple-expressions"></a>Filtrado de filas con expresiones simples

Use el generador de expresiones `col` y especifique el nombre de la columna como un argumento de cadena `col('column_name')`. Use esta expresión junto con uno de los siguientes operadores estándar >, <, >=, <=, ==,! = para crear una expresión como `col('Tip_amount') > 0`. Por último, pase la expresión creada a la función `filter()`.

En este ejemplo, `dflow.filter(col('Tip_amount') > 0)` devuelve un nuevo flujo de datos con las filas en el que el valor de `Tip_amount` es mayor que 0.

> [!NOTE] 
> `Tip_amount` se convierte primero a numérico, lo que permite generar una expresión comparándola con otros valores numéricos.

```python
dflow = dflow.to_number(['Tip_amount'])
dflow = dflow.filter(dprep.col('Tip_amount') > 0)
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-01 19:33:28|2013-08-01 19:35:21|5|.00|0.08|0|4.58|
|1|2013-08-05 13:16:38|2013-08-05 13:18:24|1|.00|0,30|0|3.8|

### <a name="filtering-rows-with-complex-expressions"></a>Filtrado de filas con expresiones complejas

Para filtrar mediante expresiones complejas, combine una o más expresiones simples con los generadores de expresiones `f_not`, `f_and` o `f_or`.

En este ejemplo, `dflow.filter()` devuelve un nuevo flujo de datos con las filas donde `'Passenger_count'` es menor que 5 y `'Tolls_amount'` es mayor que 0.

```python
dflow = dflow.to_number(['Passenger_count', 'Tolls_amount'])
dflow = dflow.filter(dprep.f_and(dprep.col('Passenger_count') < 5, dprep.col('Tolls_amount') > 0))
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-08 12:16:00|2013-08-08 12:16:00|1.0|.00|2.25|5.00|19.75|
|1|2013-08-12 14:43:53|2013-08-12 15:04:50|1.0|5.28|6.46|5.33|32.29|

También es posible filtrar filas que combinan más de un generador de expresiones para crear una expresión anidada.

> [!NOTE]
> `lpep_pickup_datetime` y `Lpep_dropoff_datetime` se convierten primero a fecha y hora, lo que permite generar una expresión comparándola con otros valores de fecha y hora.

```python
dflow = dflow.to_datetime(['lpep_pickup_datetime', 'Lpep_dropoff_datetime'], ['%Y-%m-%d %H:%M:%S'])
dflow = dflow.to_number(['Total_amount', 'Trip_distance'])
mid_2013 = datetime(2013,7,1)
dflow = dflow.filter(
    dprep.f_and(
        dprep.f_or(
            dprep.col('lpep_pickup_datetime') > mid_2013,
            dprep.col('Lpep_dropoff_datetime') > mid_2013),
        dprep.f_and(
            dprep.col('Total_amount') > 40,
            dprep.col('Trip_distance') < 10)))
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-13 06:11:06+00:00|2013-08-13 06:30:28+00:00|1.0|9.57|7.47|5.33|44.80|
|1|2013-08-23 12:28:20+00:00|2013-08-23 12:50:28+00:00|2.0|8,22|8.08|5.33|40.41|

## <a name="custom-python-transforms"></a>Transformaciones personalizadas de Python

Siempre habrá escenarios en los que la opción más sencilla para realizar una transformación sea escribir su propio script. El SDK proporciona tres puntos de extensión que se pueden usar para crear scripts personalizados de Python.

- Nueva columna de script
- Nuevo filtro de script
- Transformar partición

Cada una de las extensiones se admite en el entorno de ejecución del escalado vertical y horizontal. Una ventaja clave del uso de estos puntos de extensión es que no es necesario extraer todos los datos para crear una trama de datos. El código Python personalizado se ejecutará igual que otras transformaciones, a escala, por partición y, normalmente, en paralelo.

### <a name="initial-data-preparation"></a>Preparación de datos inicial

Comience por cargar algunos datos de blob de Azure.

```python
import azureml.dataprep as dprep
col = dprep.col

dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv', skip_rows=1)
dflow.head(2)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|
|0|ALABAMA|1|101710|Hale County|10171002158| |
|1|ALABAMA|1|101710|Hale County|10171002162| |

Reducir el conjunto de datos y realizar algunas transformaciones básicas, como quitar columnas, reemplazando los valores y conversión de tipos.

```python
dflow = dflow.keep_columns(['stnam', 'leanm10', 'ncessch', 'MAM_MTH00numvalid_1011'])
dflow = dflow.replace_na(columns=['leanm10', 'MAM_MTH00numvalid_1011'], custom_na_list='.')
dflow = dflow.to_number(['ncessch', 'MAM_MTH00numvalid_1011'])
dflow.head(2)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|
|0|ALABAMA|Hale County|1.017100e+10|None|
|1|ALABAMA|Hale County|1.017100e+10|None|

Busque valores nulos mediante el siguiente filtro.

```python
dflow.filter(col('MAM_MTH00numvalid_1011').is_null()).head(2)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|
|0|ALABAMA|Hale County|1.017100e+10|None|
|1|ALABAMA|Hale County|1.017100e+10|None|

### <a name="transform-partition"></a>Transformar partición

Use [ `transform_partition()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#transform-partition-script--str-----azureml-dataprep-api-dataflow-dataflow) para reemplazar todos los valores null con un 0. Este código se ejecutará por partición, no en todo el conjunto de datos a la vez. Esto significa que, en un conjunto de datos grande, este código puede ejecutarse en paralelo a medida que el entorno de ejecución procesa los datos, partición por partición.

El script de Python debe definir una función denominada `transform()` que toma dos argumentos, `df` y `index`. El argumento `df` será una trama de datos de Pandas que contiene los datos de la partición, y el argumento `index` es un identificador único de la partición. La función de transformación puede editar completamente la trama de datos pasada, pero debe devolver una trama de datos. Cualquier biblioteca que el script Python importe debe existir en el entorno donde se ejecuta el flujo de datos.

```python
df = df.transform_partition("""
def transform(df, index):
    df['MAM_MTH00numvalid_1011'].fillna(0,inplace=True)
    return df
""")
df.head(2)
```

||stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|
|0|ALABAMA|Hale County|1.017100e+10|0.0|
|1|ALABAMA|Hale County|1.017100e+10|0.0|

### <a name="new-script-column"></a>Nueva columna de script

Puede usar un script de Python para crear una nueva columna que tiene el nombre del condado y el nombre del estado y también para aprovechar el nombre del estado. Para ello, use el [ `new_script_column()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#new-script-column-new-column-name--str--insert-after--str--script--str-----azureml-dataprep-api-dataflow-dataflow) método en el flujo de datos.

El script de Python debe definir una función denominada `newvalue()` que toma un argumento `row`. El argumento `row` es un elemento dict (`key`: nombre de columna; `val`: valor actual) y se pasará a esta función para cada fila del conjunto de datos. Esta función debe devolver un valor que se usará en la nueva columna. Cualquier biblioteca que el script Python importe debe existir en el entorno donde se ejecuta el flujo de datos.

```python
dflow = dflow.new_script_column(new_column_name='county_state', insert_after='leanm10', script="""
def newvalue(row):
    return row['leanm10'] + ', ' + row['stnam'].title()
""")
dflow.head(2)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|1|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|

### <a name="new-script-filter"></a>Nuevo filtro de script

Creación de una expresión de Python mediante [ `new_script_filter()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#new-script-filter-script--str-----azureml-dataprep-api-dataflow-dataflow) para filtrar el conjunto de datos a solo las filas donde 'Hale' no está en el nuevo `county_state` columna. La expresión devuelve `True` si queremos conservar la fila, y `False` para quitar la fila.

```python
dflow = dflow.new_script_filter("""
def includerow(row):
    val = row['county_state']
    return 'Hale' not in val
""")
dflow.head(2)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|1.0|
|1|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|0.0|

## <a name="next-steps"></a>Pasos siguientes

* Consulte el SDK de [Introducción](https://aka.ms/data-prep-sdk) para obtener ejemplos de uso y patrones de diseño
* Consulte el SDK de Azure Machine Learning Data Prep [tutorial](tutorial-data-prep.md) para obtener un ejemplo para resolver un escenario específico
