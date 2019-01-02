---
title: 'Transformaciones: SDK de Python para la preparación de datos'
titleSuffix: Azure Machine Learning service
description: Aprenda a transformar datos con Azure Machine Learning Data Prep SDK. Utilice métodos de transformación para agregar columnas, filtrar las filas o columnas no deseadas y atribuir los valores que faltan.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: d32244cd49ebd42192b2388215f79a64cacb3caa
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53186147"
---
# <a name="transform-data-with-the-azure-machine-learning-data-prep-sdk"></a>Transformación de datos con Azure Machine Learning Data Prep SDK

En este artículo obtendrá información sobre los diferentes métodos de carga de datos mediante el [SDK de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk). El SDK ofrece funciones que facilitan la adición de columnas, el filtrado de filas o columnas no deseadas, y la atribución de valores que faltan.

Actualmente tiene disponibles funciones que puede usar para las siguientes tareas:

- [Adición de una columna mediante una expresión](#column)
- [Atribución de valores que faltan](#impute-missing-values)
- [Derivación de columnas por ejemplos](#derive-column-by-example)
- [Filtrado](#filtering)
- [Transformaciones personalizadas de Python](#custom-python-transforms)

## <a name="add-column-using-an-expression"></a>Adición de una columna mediante una expresión

Azure Machine Learning Data Prep SDK incluye expresiones `substring` que puede usar para calcular un valor de las columnas existentes y, después, colocar ese valor en una columna nueva. En este ejemplo cargará los datos e intentará agregar columnas a los datos de entrada.

```python
import azureml.dataprep as dprep

# loading data
dataflow = dprep.read_csv(path=r'data\crime0-10.csv')
dataflow.head(3)
```

||ID|Case Number|Date|Block|IUCR|Primary Type|DESCRIPCIÓN|Location Description|Arrest|Domestic|...|Ward|Community Area|FBI Code|X Coordinate|Y Coordinate|Year|Updated On|Latitud|Longitud|Ubicación|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
|0|10140490|HY329907|07/05/2015 11:50:00 PM|050XX N NEWLAND AVE|0820|THEFT|$500 AND UNDER|STREET|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 |12:42:46 PM|41.973309466|-87.800174996|(41.973309466, -87.800174996)|
|1|10139776|HY329265|07/05/2015 11:30:00 PM|011XX W MORSE AVE|0460|BATTERY|SIMPLE|STREET|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017, -87.65955018)|
|2|10140270|HY329253|07/05/2015 11:20:00 PM|121XX S FRONT AVE|0486|BATTERY|DOMESTIC BATTERY SIMPLE|STREET|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|


Use la expresión `substring(start, length)` para extraer el prefijo de la columna Case Number y poner la cadena en una nueva columna `Case Category`. Al pasar la variable `substring_expression` al parámetro `expression` se crea una nueva columna calculada que ejecuta la expresión en cada registro.

```python
substring_expression = dprep.col('Case Number').substring(0, 2)
case_category = dataflow.add_column(new_column_name='Case Category',
                                    prior_column='Case Number',
                                    expression=substring_expression)
case_category.head(3)
```

||ID|Case Number|Case Category|Date|Block|IUCR|Primary Type|DESCRIPCIÓN|Location Description|Arrest|...|Ward|Community Area|FBI Code|X Coordinate|Y Coordinate|Year|Updated On|Latitud|Longitud|Ubicación|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|HY|07/05/2015 11:50:00 PM|050XX N NEWLAND AVE|0820|THEFT|$500 AND UNDER|STREET|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 |12:42:46 PM|41.973309466|-87.800174996|(41.973309466, -87.800174996)|
|1|10139776|HY329265|HY|07/05/2015 11:30:00 PM|011XX W MORSE AVE|0460|BATTERY|SIMPLE|STREET|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017, -87.65955018)|
|2|10140270|HY329253|HY|07/05/2015 11:20:00 PM|121XX S FRONT AVE|0486|BATTERY|DOMESTIC BATTERY SIMPLE|STREET|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|



Use la expresión `substring(start)` para extraer solo el número de la columna Case Number y crear una nueva columna. Conviértala a un tipo de datos numérico mediante la función `to_number()` y pase el nombre de la columna de cadena como parámetro.

```python
substring_expression2 = dprep.col('Case Number').substring(2)
case_id = dataflow.add_column(new_column_name='Case Id',
                              prior_column='Case Number',
                              expression=substring_expression2)
case_id = case_id.to_number('Case Id')
case_id.head(3)
```

||ID|Case Number|Case Id|Date|Block|IUCR|Primary Type|DESCRIPCIÓN|Location Description|Arrest|...|Ward|Community Area|FBI Code|X Coordinate|Y Coordinate|Year|Updated On|Latitud|Longitud|Ubicación|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|329907.0|07/05/2015 11:50:00 PM|050XX N NEWLAND AVE|0820|THEFT|$500 AND UNDER|STREET|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 |12:42:46 PM|41.973309466|-87.800174996|(41.973309466, -87.800174996)|
|1|10139776|HY329265|329265.0|07/05/2015 11:30:00 PM|011XX W MORSE AVE|0460|BATTERY|SIMPLE|STREET|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017, -87.65955018)|
|2|10140270|HY329253|329253.0|07/05/2015 11:20:00 PM|121XX S FRONT AVE|0486|BATTERY|DOMESTIC BATTERY SIMPLE|STREET|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|

## <a name="impute-missing-values"></a>Atribución de valores que faltan

El SDK puede atribuir los valores que faltan a las columnas especificadas. En este ejemplo se cargarán los valores de latitud y longitud y, a continuación, se intentarán atribuir los valores que faltan a los datos de entrada.

```python
import azureml.dataprep as dprep

# loading input data
df = dprep.read_csv(r'data\crime0-10.csv')
df = df.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
df = df.to_number(['Latitude', 'Longitude'])
df.head(5)
```

||ID|Arrest|Latitud|Longitud|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|NaN|NaN|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

En el tercer registro faltan los valores de latitud y longitud. Para atribuir esos valores que faltan, puede usar `ImputeMissingValuesBuilder` para aprender una expresión fija. Puede atribuir las columnas con un valor calculado `MIN`, `MAX` o `MEAN` o un valor `CUSTOM`. Cuando se especifica `group_by_columns`, se atribuirán los valores que faltan por grupo a `MIN`, `MAX` y `MEAN` calculados por grupo.

Compruebe el valor `MEAN` de la columna de latitud mediante la función `summarize()`. Esta función acepta una matriz de columnas en el parámetro `group_by_columns` para especificar el nivel de agregación. El parámetro `summary_columns` acepta una llamada `SummaryColumnsValue`. Esta llamada a la función especifica el nombre de la columna actual, el nuevo nombre del campo calculado y el valor de `SummaryFunction` que se va a realizar.

```python
df_mean = df.summarize(group_by_columns=['Arrest'],
                       summary_columns=[dprep.SummaryColumnsValue(column_id='Latitude',
                                                                 summary_column_name='Latitude_MEAN',
                                                                 summary_function=dprep.SummaryFunction.MEAN)])
df_mean = df_mean.filter(dprep.col('Arrest') == 'false')
df_mean.head(1)
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
impute_builder = df.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])
# call learn() to learn a fixed program to impute missing values
impute_builder.learn()
# call to_dataflow() to get a data flow with impute step added
df_imputed = impute_builder.to_dataflow()

# check impute result
df_imputed.head(5)
```

||ID|Arrest|Latitud|Longitud|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|41.878961|42.000000|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

Como se muestra en el resultado anterior, la latitud que faltan se ha asignado al valor `MEAN` del grupo `Arrest=='false'`. La longitud que falta se atribuye a 42.

```python
imputed_longitude = df_imputed.to_pandas_dataframe()['Longitude'][2]
assert imputed_longitude == 42
```

## <a name="derive-column-by-example"></a>Derivación de columnas por ejemplos

Una de las herramientas más avanzadas en el SDK de preparación de datos de Azure Machine Learning es la capacidad de derivar columnas mediante ejemplos con los resultados deseados. Esto le permite dar al SDK un ejemplo para que pueda generar código y así conseguir la transformación prevista.

```python
import azureml.dataprep as dprep
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/BostonWeather.csv')
dataflow.head(10)
```

||DATE|REPORTTPYE|HOURLYDRYBULBTEMPF|HOURLYRelativeHumidity|HOURLYWindSpeed|
|----|----|----|----|----|----|
|0|1/1/2015 0:54|FM-15|22|50|10|
|1|1/1/2015 1:00|FM-12|22|50|10|
|2|1/1/2015 1:54|FM-15|22|50|10|
|3|1/1/2015 2:54|FM-15|22|50|11|
|4|1/1/2015 3:54|FM-15|24|46|13|
|5|1/1/2015 4:00|FM-12|24|46|13|
|6|1/1/2015 4:54|FM-15|22|52|15|
|7|1/1/2015 5:54|FM-15|23|48|17|
|8|1/1/2015 6:54|FM-15|23|50|14|
|9|1/1/2015 7:00|FM-12|23|50|14|

Suponga que necesita combinar este archivo con un conjunto de datos donde la fecha y hora están en un formato "Mar 10, 2018 | 2AM-4AM".

```python
builder = dataflow.builders.derive_column_by_example(source_columns=['DATE'], new_column_name='date_timerange')
builder.add_example(source_data=df.iloc[1], example_value='Jan 1, 2015 12AM-2AM')
builder.preview() 
```

||DATE|date_timerange|
|----|----|----|
|0|1/1/2015 0:54|Jan 1, 2015 12AM-2AM|
|1|1/1/2015 1:00|Jan 1, 2015 12AM-2AM|
|2|1/1/2015 1:54|Jan 1, 2015 12AM-2AM|
|3|1/1/2015 2:54|Jan 1, 2015 2AM-4AM|
|4|1/1/2015 3:54|Jan 1, 2015 2AM-4AM|
|5|1/1/2015 4:00|Jan 1, 2015 4AM-6AM|
|6|1/1/2015 4:54|Jan 1, 2015 4AM-6AM|
|7|1/1/2015 5:54|Jan 1, 2015 4AM-6AM|
|8|1/1/2015 6:54|Jan 1, 2015 6AM-8AM|
|9|1/1/2015 7:00|Jan 1, 2015 6AM-8AM|

El código anterior crea primero un generador para la columna derivada. Ha proporcionado una matriz de columnas de origen que hay que tener en cuenta (`DATE`) y un nombre para la nueva columna que se va a agregar. Tal como se indicó en el primer ejemplo, debe pasar la segunda fila (índice 1) y proporcionar un valor esperado para la columna derivada.

Finalmente, debe llamar a `builder.preview()` y así podrá ver la columna derivada junto a la columna de origen. El formato parece ser correcto, pero solo verá los valores de la misma fecha "Jan 1, 2015".

Ahora, pase el número de filas que desea `skip` desde arriba para ver las filas más abajo.

```
builder.preview(skip=30)
```

||DATE|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22:54|Jan 1, 2015 10PM-12AM|
|31|1/1/2015 23:54|Jan 1, 2015 10PM-12AM|
|32|1/1/2015 23:59|Jan 1, 2015 10PM-12AM|
|33|1/2/2015 0:54|Feb 1, 2015 12AM-2AM|
|34|1/2/2015 1:00|Feb 1, 2015 12AM-2AM|
|35|1/2/2015 1:54|Feb 1, 2015 12AM-2AM|
|36|1/2/2015 2:54|Feb 1, 2015 2AM-4AM|
|37|1/2/2015 3:54|Feb 1, 2015 2AM-4AM|
|38|1/2/2015 4:00|Feb 1, 2015 4AM-6AM|
|39|1/2/2015 4:54|Feb 1, 2015 4AM-6AM|

Aquí puede ver un problema con el programa generado. Basado únicamente en el ejemplo que proporcionó anteriormente, el programa derivado ha elegido analizar la fecha como "Día/Mes/Año", que no es lo que se quiere hacer en este caso. Para solucionar este problema, proporcione otro ejemplo utilizando la función `add_example()` en la variable `builder`.

```python
builder.add_example(source_data=preview_df.iloc[3], example_value='Jan 2, 2015 12AM-2AM')
builder.preview(skip=30, count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22:54|Jan 1, 2015 10PM-12AM|
|31|1/1/2015 23:54|Jan 1, 2015 10PM-12AM|
|32|1/1/2015 23:59|Jan 1, 2015 10PM-12AM|
|33|1/2/2015 0:54|Jan 2, 2015 12AM-2AM|
|34|1/2/2015 1:00|Jan 2, 2015 12AM-2AM|
|35|1/2/2015 1:54|Jan 2, 2015 12AM-2AM|
|36|1/2/2015 2:54|Jan 2, 2015 2AM-4AM|
|37|1/2/2015 3:54|Jan 2, 2015 2AM-4AM|
|38|1/2/2015 4:00|Jan 2, 2015 4AM-6AM|
|39|1/2/2015 4:54|Jan 2, 2015 4AM-6AM|

Ahora, las filas administran correctamente la fecha "1/2/2015" como "Jan 2, 2015", pero si mira más abajo en la columna derivada, puede ver que los valores al final no tienen nada en la columna derivada. Para solucionarlo, deberá proporcionar otro ejemplo para la fila 66.

```python
builder.add_example(source_data=preview_df.iloc[66], example_value='Jan 29, 2015 8PM-10PM')
builder.preview(count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|Jan 1, 2015 10PM-12AM|
|1|1/1/2015 23:54|Jan 1, 2015 10PM-12AM|
|2|1/1/2015 23:59|Jan 1, 2015 10PM-12AM|
|3|1/2/2015 0:54|Jan 2, 2015 12AM-2AM|
|4|1/2/2015 1:00|Jan 2, 2015 12AM-2AM|
|5|1/2/2015 1:54|Jan 2, 2015 12AM-2AM|
|6|1/2/2015 2:54|Jan 2, 2015 2AM-4AM|
|7|1/2/2015 3:54|Jan 2, 2015 2AM-4AM|
|8|1/2/2015 4:00|Jan 2, 2015 4AM-6AM|
|9|1/2/2015 4:54|Jan 2, 2015 4AM-6AM|

Para separar la fecha y la hora con "|", agregue otro ejemplo. Esta vez, en lugar de pasar una fila de la vista previa, construya un diccionario de nombre de la columna al valor para el parámetro `source_data`.

```python
builder.add_example(source_data={'DATE': '11/11/2015 0:54'}, example_value='Nov 11, 2015 | 12AM-2AM')
builder.preview(count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|None|
|1|1/1/2015 23:54|None|
|2|1/1/2015 23:59|None|
|3|1/2/2015 0:54|None|
|4|1/2/2015 1:00|None|
|5|1/2/2015 1:54|None|
|6|1/2/2015 2:54|None|
|7|1/2/2015 3:54|None|
|8|1/2/2015 4:00|None|
|9|1/2/2015 4:54|None|

Esto claramente tuvo efectos negativos, ya que ahora las únicas filas que tienen valores en la columna derivada son las que coinciden exactamente con los ejemplos que se han proporcionado. Llame a `list_examples()` en el objeto generador para ver una lista de las derivaciones de ejemplo actuales.

```python
examples = builder.list_examples()
```

| |DATE|ejemplo|example_id|
| -------- | -------- | -------- | -------- |
|0|1/1/2015 1:00|Jan 1, 2015 12AM-2AM|-1|
|1|1/2/2015 0:54|Jan 2, 2015 12AM-2AM|-2|
|2|1/29/2015 20:54|Jan 29, 2015 8PM-10PM|-3|
|3|11/11/2015 0:54|Nov 11, 2015 \| 12AM-2AM|-4|

En este caso, se proporcionaron ejemplos incoherentes. Para solucionar el problema, es necesario reemplazar los tres primeros ejemplos con otros ejemplos correctos (debe incluir "|" entre la fecha y la hora).

Puede corregir los ejemplos incoherentes si elimina aquellos que no sean correctos (ya sea pasando `example_row` desde DataFrame de Pandas o pasando el valor `example_id`) y volviendo a agregar nuevos ejemplos modificados.

```python
builder.delete_example(example_id=-1)
builder.delete_example(example_row=examples.iloc[1])
builder.delete_example(example_row=examples.iloc[2])
builder.add_example(examples.iloc[0], 'Jan 1, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[1], 'Jan 2, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[2], 'Jan 29, 2015 | 8PM-10PM')
builder.preview()
```

| | DATE | date_timerange |
| -------- | -------- | -------- |
| 0 | 1/1/2015 0:54 | Jan 1, 2015 \| 12AM-2AM |
| 1 | 1/1/2015 1:00 | Jan 1, 2015 \| 12AM-2AM |
| 2 | 1/1/2015 1:54 | Jan 1, 2015 \| 12AM-2AM |
| 3 | 1/1/2015 2:54 | Jan 1, 2015 \| 2AM-4AM |
| 4 | 1/1/2015 3:54 | Jan 1, 2015 \| 2AM-4AM |
| 5 | 1/1/2015 4:00 | Jan 1, 2015 \| 4AM-6AM|
| 6 | 1/1/2015 4:54 | Jan 1, 2015 \| 4AM-6AM|
| 7 | 1/1/2015 5:54 | Jan 1, 2015 \| 4AM-6AM|
| 8 | 1/1/2015 6:54 | Jan 1, 2015 \| 6AM-8AM|
| 9 | 1/1/2015 7:00 | Jan 1, 2015 \| 6AM-8AM|

Ahora los datos parecen correctos y finalmente podemos llamar a `to_dataflow()` en el generador, que devolverá un flujo de datos con las columnas derivadas agregadas que quiera.

```python
dataflow = builder.to_dataflow()
df = dataflow.to_pandas_dataframe()
```

## <a name="filtering"></a>Filtros

El SDK incluye los métodos `Dataflow.drop_columns` y `Dataflow.filter` para que pueda filtrar las columnas o filas.

### <a name="initial-setup"></a>Configuración inicial

```python
import azureml.dataprep as dprep
from datetime import datetime
dataflow = dprep.read_csv(path='https://dprepdata.blob.core.windows.net/demo/green-small/*')
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Store_and_fwd_flag|RateCodeID|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|None|None|None|None|None|None|None|None|None|None|None|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|N|1|0|0|0|0|1|.00|0|0|21.25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|N|1|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|N|5|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|N|1|0|0|0|0|1|.00|0|0|3.25|

### <a name="filtering-columns"></a>Filtrado de columnas

Para filtrar las columnas, use `Dataflow.drop_columns`. Este método toma una lista de columnas que se van a anular o un argumento más complejo denominado `ColumnSelector`.

#### <a name="filtering-columns-with-list-of-strings"></a>Filtrado de columnas con la lista de cadenas

En este ejemplo, `drop_columns` toma una lista de cadenas. Cada cadena debe coincidir exactamente con la columna que se desea anular.

```python
dataflow = dataflow.drop_columns(['Store_and_fwd_flag', 'RateCodeID'])
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|None|None|None|None|None|None|None|None|None|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|0|0|0|0|1|.00|0|0|21.25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|0|0|0|0|1|.00|0|0|3.25|

#### <a name="filtering-columns-with-regex"></a>Filtrado de columnas con expresiones regulares

También puede usar la expresión `ColumnSelector` para anular columnas que coincidan con una expresión regular. En este ejemplo se anulan todas las columnas que coinciden con la expresión `Column*|.*longitude|.*latitude`.

```python
dataflow = dataflow.drop_columns(dprep.ColumnSelector('Column*|.*longitud|.*latitude', True, True))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|None|None|None|None|None|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|1|.00|0|0|21.25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|1|.00|0|0|3.25|

## <a name="filtering-rows"></a>Filtrado de filas

Para filtrar las filas, utilice `DataFlow.filter`. Este método toma una expresión de Azure Machine Learning Data Prep SDK como argumento y devuelve un nuevo flujo de datos con las filas que la expresión evalúa como True. Las expresiones se crean mediante los generadores de expresiones (`col`, `f_not`, `f_and`, `f_or`) y operadores regulares (>, <, >=, <=, ==, ¡¡¡¡, !=).

### <a name="filtering-rows-with-simple-expressions"></a>Filtrado de filas con expresiones simples

Use el generador de expresiones `col` y especifique el nombre de la columna como un argumento de cadena `col('column_name')`. Use esta expresión junto con uno de los siguientes operadores estándar >, <, >=, <=, ==,! = para crear una expresión como `col('Tip_amount') > 0`. Por último, pase la expresión creada a la función `Dataflow.filter`.

En este ejemplo, `dataflow.filter(col('Tip_amount') > 0)` devuelve un nuevo flujo de datos con las filas en el que el valor de `Tip_amount` es mayor que 0.

> [!NOTE] 
> `Tip_amount` se convierte primero a numérico, lo que permite generar una expresión comparándola con otros valores numéricos.

```python
dataflow = dataflow.to_number(['Tip_amount'])
dataflow = dataflow.filter(dprep.col('Tip_amount') > 0)
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-01 19:33:28|2013-08-01 19:35:21|5|.00|0.08|0|4.58|
|1|2013-08-05 13:16:38|2013-08-05 13:18:24|1|.00|0,30|0|3.8|
|2|2013-08-05 14:11:42|2013-08-05 14:12:47|1|.00|1.05|0|4.55|
|3|2013-08-05 14:15:56|2013-08-05 14:18:04|5|.00|2.22|0|5.72|
|4|2013-08-05 14:42:14|2013-08-05 14:42:38|1|.00|0.88|0|4.38|

### <a name="filtering-rows-with-complex-expressions"></a>Filtrado de filas con expresiones complejas

Para filtrar mediante expresiones complejas, combine una o más expresiones simples con los generadores de expresiones `f_not`, `f_and` o `f_or`.

En este ejemplo, `Dataflow.filter` devuelve un nuevo flujo de datos con las filas donde `'Passenger_count'` es menor que 5 y `'Tolls_amount'` es mayor que 0.

```python
dataflow = dataflow.to_number(['Passenger_count', 'Tolls_amount'])
dataflow = dataflow.filter(dprep.f_and(dprep.col('Passenger_count') < 5, dprep.col('Tolls_amount') > 0))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-08 12:16:00|2013-08-08 12:16:00|1.0|.00|2.25|5.00|19.75|
|1|2013-08-12 14:43:53|2013-08-12 15:04:50|1.0|5.28|6.46|5.33|32.29|
|2|2013-08-12 19:48:12|2013-08-12 20:03:42|1.0|5.50|1.00|10.66|30.66|
|3|2013-08-13 06:11:06|2013-08-13 06:30:28|1.0|9.57|7.47|5.33|44.8|
|4|2013-08-16 20:33:50|2013-08-16 20:48:50|1.0|5.63|3.00|5.33|27.83|

También es posible filtrar filas que combinan más de un generador de expresiones para crear una expresión anidada.

> [!NOTE]
> `lpep_pickup_datetime` y `Lpep_dropoff_datetime` se convierten primero a fecha y hora, lo que permite generar una expresión comparándola con otros valores de fecha y hora.

```python
dataflow = dataflow.to_datetime(['lpep_pickup_datetime', 'Lpep_dropoff_datetime'], ['%Y-%m-%d %H:%M:%S'])
dataflow = dataflow.to_number(['Total_amount', 'Trip_distance'])
mid_2013 = datetime(2013,7,1)
dataflow = dataflow.filter(
    dprep.f_and(
        dprep.f_or(
            dprep.col('lpep_pickup_datetime') > mid_2013,
            dprep.col('Lpep_dropoff_datetime') > mid_2013),
        dprep.f_and(
            dprep.col('Total_amount') > 40,
            dprep.col('Trip_distance') < 10)))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-13 06:11:06+00:00|2013-08-13 06:30:28+00:00|1.0|9.57|7.47|5.33|44.80|
|1|2013-08-23 12:28:20+00:00|2013-08-23 12:50:28+00:00|2.0|8,22|8.08|5.33|40.41|
|2|2013-08-25 09:12:52+00:00|2013-08-25 09:34:34+00:00|1.0|8.80|8.33|5.33|41.66|
|3|2013-08-25 16:46:51+00:00|2013-08-25 17:13:55+00:00|2.0|9.66|7.37|5.33|44.20|
|4|2013-08-25 17:42:11+00:00|2013-08-25 18:02:57+00:00|1.0|9.60|6.87|5.33|41.20|

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

df = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv', skip_rows=1)
df.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|Hale County|10171002158| |
|1|ALABAMA|1|101710|Hale County|10171002162| |
|2|ALABAMA|1|101710|Hale County|10171002156| |
|3|ALABAMA|1|101710|Hale County|10171000588|2|
|4|ALABAMA|1|101710|Hale County|10171000589| |

Reduzca el conjunto de datos y realice algunas transformaciones básicas.

```python
df = df.keep_columns(['stnam', 'leanm10', 'ncessch', 'MAM_MTH00numvalid_1011'])
df = df.replace_na(columns=['leanm10', 'MAM_MTH00numvalid_1011'], custom_na_list='.')
df = df.to_number(['ncessch', 'MAM_MTH00numvalid_1011'])
df.head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e+10|None|
|1|ALABAMA|Hale County|1.017100e+10|None|
|2|ALABAMA|Hale County|1.017100e+10|None|
|3|ALABAMA|Hale County|1.017100e+10|2|
|4|ALABAMA|Hale County|1.017100e+10|None|

Busque valores nulos mediante el siguiente filtro.

```python
df.filter(col('MAM_MTH00numvalid_1011').is_null()).head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e+10|None|
|1|ALABAMA|Hale County|1.017100e+10|None|
|2|ALABAMA|Hale County|1.017100e+10|None|
|3|ALABAMA|Hale County|1.017100e+10|None|
|4|ALABAMA|Hale County|1.017100e+10|None|

### <a name="transform-partition"></a>Transformar partición

Puede usar una función de Pandas para reemplazar todos los valores nulos por un 0. Este código se ejecutará por partición, no en todo el conjunto de datos a la vez. Esto significa que, en un conjunto de datos grande, este código puede ejecutarse en paralelo a medida que el entorno de ejecución procesa los datos, partición por partición.

El script de Python debe definir una función denominada `transform()` que toma dos argumentos, `df` y `index`. El argumento `df` será una trama de datos de Pandas que contiene los datos de la partición, y el argumento `index` es un identificador único de la partición. La función de transformación puede editar completamente la trama de datos pasada, pero debe devolver una trama de datos. Cualquier biblioteca que el script Python importe debe existir en el entorno donde se ejecuta el flujo de datos.

```python
df = df.transform_partition("""
def transform(df, index):
    df['MAM_MTH00numvalid_1011'].fillna(0,inplace=True)
    return df
""")
df.head(5)
```

||stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e+10|0.0|
|1|ALABAMA|Hale County|1.017100e+10|0.0|
|2|ALABAMA|Hale County|1.017100e+10|0.0|
|3|ALABAMA|Hale County|1.017100e+10|2.0|
|4|ALABAMA|Hale County|1.017100e+10|0.0|

### <a name="new-script-column"></a>Nueva columna de script

Puede usar código de Python para crear una nueva columna que tenga el nombre del condado y el nombre del estado, y también para poner en mayúscula el nombre del estado. Para ello, use el método `new_script_column()` en el flujo de datos.

El script de Python debe definir una función denominada `newvalue()` que toma un argumento `row`. El argumento `row` es un elemento dict (`key`: nombre de columna; `val`: valor actual) y se pasará a esta función para cada fila del conjunto de datos. Esta función debe devolver un valor que se usará en la nueva columna. Cualquier biblioteca que el script Python importe debe existir en el entorno donde se ejecuta el flujo de datos.

```python
df = df.new_script_column(new_column_name='county_state', insert_after='leanm10', script="""
def newvalue(row):
    return row['leanm10'] + ', ' + row['stnam'].title()
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|1|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|2|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|3|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|2.0|
|4|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|

### <a name="new-script-filter"></a>Nuevo filtro de script

Cree una expresión de Python para filtrar el conjunto de datos solo en aquellas filas en las que "Hale" no esté en la nueva columna `county_state`. La expresión devuelve `True` si queremos conservar la fila, y `False` para quitar la fila.

```python
df = df.new_script_filter("""
def includerow(row):
    val = row['county_state']
    return 'Hale' not in val
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|1.0|
|1|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|0.0|
|2|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|0.0|
|3|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|0.0|
|4|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|0.0|
