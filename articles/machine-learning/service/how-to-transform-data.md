---
title: 'Transformación de datos con Azure Machine Learning Data Prep SDK: Python'
description: Aprenda a transformar datos con Azure Machine Learning Data Prep SDK. Utilice métodos de transformación para agregar columnas, filtrar las filas o columnas no deseadas y atribuir los valores que faltan.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 06e7d227511a9b651a905df3172f59a191acce01
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945678"
---
# <a name="transform-data-with-the-azure-machine-learning-data-prep-sdk"></a>Transformación de datos con Azure Machine Learning Data Prep SDK

[Azure Machine Learning Data Prep SDK](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py) ofrece varios métodos de transformación para limpiar los datos. Dichos métodos facilitan la adición de columnas, el filtrado de filas o columnas no deseadas, y la atribución de valores que faltan.

Actualmente hay métodos para las siguientes tareas:
- [Adición de una columna mediante una expresión](#column)
- [Atribución de valores que faltan](#impute-missing-values)
- [Derivación de columnas por ejemplos](#derive-column-by-example)
- [Filtrado](#filtering)
- [Transformaciones personalizadas de Python](#custom-python-transforms)

<a name=column>
## <a name="add-column-using-an-expression"></a>Adición de una columna mediante una expresión

Azure Machine Learning Data Prep SDK incluye expresiones `substring` que puede usar para calcular un valor de las columnas existentes y, después, colocar ese valor en una columna nueva. En este ejemplo, se van a cargar los datos e intentar agregar columnas a los datos de entrada.

```
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



Use la expresión `substring(start, length)` para extraer el prefijo de la columna Case Number y poner los datos en una nueva columna: Case Category.

```
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



Use la expresión `substring(start)` para extraer solo el número de la columna Case Number, después conviértalo a un tipo de datos numéricos y póngalo en una nueva columna: Case Id.
```
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

Azure Machine Learning Data Prep SDK puede atribuir los valores que faltan a las columnas especificadas. En este ejemplo, se cargarán los valores de latitud y longitud y, después, se intentará atribuir los valores que faltan a los datos de entrada.

```
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

En el tercer registro faltan los valores de latitud y longitud. Para atribuir esos valores que faltan, puede usar `ImputeMissingValuesBuilder` para aprender un programa fijo. Pueden atribuir las columnas con un valor `MIN`, `MAX` o `MEAN` calculado o un valor `CUSTOM`. Cuando se especifica `group_by_columns`, se atribuirán los valores que faltan por grupo a `MIN`, `MAX` y `MEAN` calculados por grupo.

En primer lugar, compruebe rápidamente el valor `MEAN` de la columna de latitud.
```
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

El valor `MEAN` de las latitudes parece correcta, por lo que puede usarlo para atribuir la latitud. Para el valor de longitud que falta, se le atribuirá a 42 en función de información externa.


```
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
```
imputed_longitude = df_imputed.to_pandas_dataframe()['Longitude'][2]
assert imputed_longitude == 42
```

## <a name="derive-column-by-example"></a>Derivación de columnas por ejemplos
Una de las herramientas más avanzadas en Azure Machine Learning Data Prep SDK es la capacidad de derivar columnas mediante ejemplos con los resultados deseados. Esto le permite dar al SDK un ejemplo para que pueda generar código y conseguir la derivación prevista.

```
import azureml.dataprep as dprep
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/BostonWeather.csv')
df = dataflow.head(10)
df
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

Como puede ver, este archivo es bastante sencillo. Sin embargo, suponga que necesita combinar este archivo con un conjunto de datos donde la fecha y hora están en un formato "Mar 10, 2018 | 2AM-4AM".

Puede transformar los datos en la forma que necesita.

```
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

El código anterior crea primero un generador para la columna derivada. Ha proporcionado una matriz de columnas de origen que hay que considerar (`DATE`) y un nombre para la nueva columna que se va a agregar.

A continuación, como primer ejemplo, pasa a la segunda fila (índice 1) y da un valor esperado para la columna derivada.

Finalmente, llama a `builder.preview()` y puede ver la columna derivada junto a la columna de origen. El formato parece estar en orden, pero solo verá los valores de la misma fecha "Jan 1, 2015".

Ahora, pase el número de filas que desea `skip` desde arriba para ver las filas más abajo.

```
preview_df = builder.preview(skip=30)
preview_df
```

||DATE|date_timerange|
|-----|-----|-----|
|30|11/1/2015 22:54|Jan 1, 2015 10PM-12AM|
|31|11/1/2015 23:54|Jan 1, 2015 10PM-12AM|
|32|11/1/2015 23:59|Jan 1, 2015 10PM-12AM|
|33|11/2/2015 0:54|Feb 1, 2015 12AM-2AM|
|34|11/2/2015 1:00|Feb 1, 2015 12AM-2AM|
|35|11/2/2015 1:54|Feb 1, 2015 12AM-2AM|
|36|11/2/2015 2:54|Feb 1, 2015 2AM-4AM|
|37|11/2/2015 3:54|Feb 1, 2015 2AM-4AM|
|38|11/2/2015 4:00|Feb 1, 2015 4AM-6AM|
|39|11/2/2015 4:54|Feb 1, 2015 4AM-6AM|

Aquí puede ver un problema con el programa generado: basado únicamente en el ejemplo que proporcionó anteriormente, el programa derivado ha elegido analizar la fecha como "Día/Mes/Año", que no es lo que se desea en este caso.

Para solucionar este problema, deberá proporcionar otro ejemplo.

```
builder.add_example(source_data=preview_df.iloc[3], example_value='Jan 2, 2015 12AM-2AM')
preview_df = builder.preview(skip=30, count=10)
preview_df
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


Ahora, las filas tratan correctamente "1/2/2015" como "Jan 2, 2015", pero si se mira más abajo en la columna derivada, puede ver que los valores al final no tienen nada en la columna derivada. Para solucionarlo, deberá proporcionar otro ejemplo para la fila 66.

```
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

Todo es correcto pero observará que no es exactamente lo que se quería. Necesita separar la fecha y hora con "|" para generar el formato correcto.

Para solucionarlo, puede agregar otro ejemplo. Esta vez, en lugar de pasar una fila de la vista previa, construya un diccionario de nombre de la columna al valor para el parámetro `source_data`.

```
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

Esto claramente tuvo efectos negativos, ya que ahora las únicas filas que tienen valores en la columna derivada son las que coinciden exactamente con los ejemplos que se han proporcionado.

Veamos los ejemplos:
```
examples = builder.list_examples()
examples
```

| |DATE|ejemplo|example_id|
| -------- | -------- | -------- | -------- |
|0|1/1/2015 1:00|Jan 1, 2015 12AM-2AM|-1|
|1|1/2/2015 0:54|Jan 2, 2015 12AM-2AM|-2|
|2|1/29/2015 20:54|Jan 29, 2015 8PM-10PM|-3|
|3|11/11/2015 0:54|Nov 11, 2015 \| 12AM-2AM|-4|

Puede ver que hemos proporcionado ejemplos incoherentes. Para solucionar el problema, necesitamos reemplazar los tres primeros ejemplos por los correctos (incluyendo "|" entre fecha y hora).

Podemos lograrlo eliminando los ejemplos que son incorrectos (ya sea pasando `example_row` del DataFrame de pandas, o pasando el valor `example_id`) y luego agregando nuevos ejemplos modificados de nuevo.

```
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

Ahora los datos parecen correctos y finalmente podemos llamar `to_dataflow()` en el generador, que devolverá un flujo de datos con las columnas derivadas deseadas agregadas.

```
dataflow = builder.to_dataflow()
df = dataflow.to_pandas_dataframe()
df
```

## <a name="filtering"></a>Filtros

El SDK incluye los métodos `Dataflow.drop_columns` y `Dataflow.filter` para que pueda filtrar las columnas o filas.

### <a name="initial-setup"></a>Configuración inicial
```
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

``` 
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
También puede usar la expresión `ColumnSelector` para anular columnas que coincidan con una expresión regular. En este ejemplo, se anulan todas las columnas que coinciden con la expresión `Column*|.*longitude|.*latitude`.

```
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

Utilice el generador de expresiones `col`, especifique el nombre de la columna como argumento de cadena `col('column_name')` y, en combinación con uno de los siguientes operadores estándar >, <, >=, <=, ==, !=, cree una expresión como `col('Tip_amount') > 0`. Por último, pase la expresión creada a la función `Dataflow.filter`.

En este ejemplo, `dataflow.filter(col('Tip_amount') > 0)` devuelve un nuevo flujo de datos con las filas en el que el valor de `Tip_amount` es mayor que 0.

> [!NOTE] 
> `Tip_amount` se convierte primero a numérico, lo que permite generar una expresión comparándola con otros valores numéricos.

```
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

```
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

```
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

Habrá escenarios en los que lo más fácil será escribir algo de código Python. El SDK proporciona tres puntos de extensión que se pueden usar.

- Nueva columna de script
- Nuevo filtro de script
- Transformar partición

Cada una de las extensiones se admite en el entorno de ejecución del escalado vertical y horizontal. Una ventaja clave del uso de estos puntos de extensión es que no es necesario extraer todos los datos para crear una trama de datos. El código Python personalizado se ejecutará igual que otras transformaciones, a escala, por partición y, normalmente, en paralelo.

### <a name="initial-data-preparation"></a>Preparación de datos inicial

Comience por cargar algunos datos de blob de Azure.

```
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

```
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

Busque valores nulos mediante un filtro. Encontrará algunos, así que ahora rellene estos valores que faltan.

```
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

Puede usar una útil función de Pandas para reemplazar todos los valores nulos por un 0. Este código se ejecutará por partición, no en todo el conjunto de datos a la vez. Esto significa que, en un conjunto de datos grande, este código puede ejecutarse en paralelo a medida que el entorno de ejecución procesa los datos, partición por partición.

```
df = df.transform_partition("""
def transform(df, index):
    df['MAM_MTH00numvalid_1011'].fillna(0,inplace=True)
    return df
""")
h = df.head(5)
h
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

```
df = df.new_script_column(new_column_name='county_state', insert_after='leanm10', script="""
def newvalue(row):
    return row['leanm10'] + ', ' + row['stnam'].title()
""")
h = df.head(5)
h
```
||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|1|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|2|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|3|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|2.0|
|4|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
### <a name="new-script-filter"></a>Nuevo filtro de script

Ahora, cree una expresión de Python para filtrar el conjunto de datos solo en las filas en las que "Hale" no esté en la nueva columna `county_state`. La expresión devuelve `True` si queremos conservar la fila, y `False` para quitar la fila.

```
df = df.new_script_filter("""
def includerow(row):
    val = row['county_state']
    return 'Hale' not in val
""")
h = df.head(5)
h
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|1.0|
|1|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|0.0|
|2|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|0.0|
|3|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|0.0|
|4|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e + 10|0.0|
