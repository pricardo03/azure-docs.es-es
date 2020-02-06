---
title: 'Diseñador: Ejemplo de predicción de retrasos en los vuelos'
titleSuffix: Azure Machine Learning
description: Cree un clasificador y use código de R personalizado para predecir retrasos en los vuelos con el diseñador de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.date: 12/25/2019
ms.openlocfilehash: d459350572d68cc5dcbbfd56933483404b94f918
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963291"
---
# <a name="build-a-classifier--use-r-to-predict-flight-delays-with-azure-machine-learning-designer"></a>Creación de un clasificador y uso de R para predecir retrasos en los vuelos con el diseñador de Azure Machine Learning

**Ejemplo 6 del diseñador (versión preliminar)**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Esta canalización utiliza datos históricos relativos a los vuelos y la meteorología para predecir si un vuelo de pasajeros programado se retrasará más de 15 minutos. Este problema se puede abordar como un problema de clasificación, que predice dos clases: retrasado o puntual.

Este es el gráfico de la canalización final de este ejemplo:

[![Gráfico de la canalización](media/how-to-designer-sample-classification-flight-delay/pipeline-graph.png)](media/how-to-designer-sample-classification-flight-delay/pipeline-graph.png#lightbox)

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Haga clic en el ejemplo 6 para abrirlo.

## <a name="get-the-data"></a>Obtener los datos

En este ejemplo se usa el conjunto de datos **Flight Delays Data** (Datos de retrasos de vuelos). Forma parte de la colección de datos TranStats del Departamento de Transporte de EE. UU. El conjunto de datos contiene información sobre los retrasos de vuelos de abril a octubre de 2013. El conjunto de datos se ha procesado previamente de la siguiente manera:

* Se han filtrado para incluir los 70 aeropuertos con más actividad del territorio continental de Estados Unidos.
* Se han reetiquetado los vuelos cancelados como retrasados más de 15 minutos.
* Los vuelos desviados se quitaron de la muestra.
* Se seleccionaron 14 columnas.

Para complementar los datos de vuelo, se usa el lote **Weather Dataset** (conjunto de datos meteorológicos). Los datos meteorológicos contienen observaciones meteorológicas terrestres por hora de la NOAA y representan las observaciones desde las estaciones meteorológicas de los aeropuertos, que abarcan el mismo período que el conjunto de datos de vuelo. Se ha procesado previamente de la siguiente manera:

* Se asignaron los identificadores de las estaciones meteorológicas a los identificadores de aeropuerto correspondientes.
* Se excluyeron las estaciones meteorológicas no asociadas a los 70 aeropuertos con mayor tráfico.
* La columna Date (Fecha) se separó en columnas independientes con los valores Year, Month y Day (Año, Mes y Día).
* Se seleccionaron 26 columnas.

## <a name="pre-process-the-data"></a>Preprocesar los datos

Normalmente, un conjunto de datos requiere algún procesamiento previo antes de que se pueda analizar.

![procesamiento de datos](./media/how-to-designer-sample-classification-flight-delay/data-process.png)

### <a name="flight-data"></a>Datos de vuelos

Las columnas **Carrier**, **OriginAirportID** y **DestAirportID** se guardan como enteros. Sin embargo, son atributos de categorías, use el módulo **Edit Metadata** (Editar metadatos) para convertirlas en categorías.

![edición de metadatos](./media/how-to-designer-sample-classification-flight-delay/edit-metadata.png)

A continuación, utilice **Selección de columnas** en el módulo Dataset (Conjunto de datos) para excluir de este las columnas que son posibles elementos de fuga de destino: **DepDelay**, **DepDel15**, **ArrDelay**, **Canceled**, **Year**. 

Para unir los registros de vuelos con los registros de meteorología por hora, use la hora de salida programada como una de las claves de combinación. Para realizar la combinación, la columna CSRDepTime se debe redondear hacia abajo a la hora más cercana, lo cual se hace en el módulo **Execute R Script** (Ejecutar script R). 

### <a name="weather-data"></a>Datos meteorológicos

Se excluirán las columnas en las que faltan una gran cantidad de valores mediante el módulo **Project Columns** (Columnas del proyecto). Estas columnas incluyen todas las columnas con valores de cadena: **ValueForWindCharacter**, **WetBulbFarenheit**, **WetBulbCelsius**, **PressureTendency**, **PressureChange**, **SeaLevelPressure** y **StationPressure**.

El módulo **Clean Missing Data** (Limpiar datos que faltan) se aplica a continuación a las columnas restantes para quitar las filas a las que le falten datos.

Las horas de observación de la meteorología se redondean a la hora completa más cercana. Las horas programadas de los vuelos y las horas de observación de la meteorología se redondean en direcciones opuestas para garantizar que el modelo use solo la meteorología anterior a la hora del vuelo. 

Dado que los datos meteorológicos se notifican en la hora local, las diferencias de zona horaria se tienen en cuenta al restar las columnas de la zona horaria de la hora de salida programada y de la hora de observación de la meteorología. Estas operaciones se realizan mediante el módulo **Execute R Script** (Ejecutar script R).

### <a name="joining-datasets"></a>Combinación de conjuntos de datos

Los registros de vuelo se combinan con los datos meteorológicos en el origen del vuelo ( **OriginAirportID**) utilizando el módulo **Join Data** (Combinar datos).

 ![combinación de vuelo y meteorología por origen](./media/how-to-designer-sample-classification-flight-delay/join-origin.png)


Los registros de vuelo se combinan con los datos meteorológicos utilizando el destino del vuelo (**DestAirportID**).

 ![Combinación vuelo y meteorología por destino](./media/how-to-designer-sample-classification-flight-delay/join-destination.png)

### <a name="preparing-training-and-test-samples"></a>Preparación de ejemplos de entrenamiento y pruebas

El módulo **Split Data** (Dividir datos) divide los datos en registros de abril a septiembre para entrenamiento, y los registros de octubre para pruebas.

 ![División de datos de entrenamiento y prueba](./media/how-to-designer-sample-classification-flight-delay/split.png)

Las columnas Year (Año), Month (Mes) y Timezone (Zona horaria) se eliminan del conjunto de datos de entrenamiento mediante el módulo Seleccionar columnas.

## <a name="define-features"></a>Definir las características

En el aprendizaje automático, las características son propiedades mensurables individuales de algo que le interesa. La búsqueda de un sólido conjunto de características requiere experimentación y conocimientos del ámbito. Algunas características son mejores para predecir el destino que otras. Además, algunas características pueden tener una fuerte correlación con otras características y no agregarán nueva información al modelo. Estas características se pueden quitar.

Para crear un modelo, puede utilizar todas las características disponibles o seleccionar un subconjunto de características.

## <a name="choose-and-apply-a-learning-algorithm"></a>Elegir y aplicar un algoritmo de aprendizaje

Cree un modelo con el módulo **Two-Class Logistic Regression** (Regresión logística de dos clases) y entrénelo en el conjunto de datos de entrenamiento. 

El resultado del módulo **Train Model** (Entrenar modelo) es un modelo de clasificación entrenado que se puede usar para puntuar las nuevas muestras para realizar predicciones. Utilice el conjunto de pruebas para generar puntuaciones a partir de los modelos entrenados. A continuación, utilice el módulo **Evaluate Model** (Evaluación de modelo) para analizar y comparar la calidad de los modelos.
Después de ejecutar la canalización, puede ver la salida desde el módulo **Score Model** (Puntuar modelo) haciendo clic en el puerto de salida y seleccionando **Visualizar**. La salida incluye las etiquetas puntuadas y las probabilidades de las etiquetas.

Finalmente, para probar la calidad de los resultados, agregue el módulo**Evaluate Model** (Evaluar modelo) al lienzo de la canalización, y conecte el puerto de entrada izquierdo a la salida del módulo Score Model (Puntuar modelo). Ejecute la canalización y vea la salida desde el módulo **Evaluate Model** (Evaluar modelo) haciendo clic en el puerto de salida y seleccionando **Visualizar**.

## <a name="evaluate"></a>Evaluate
El modelo de regresión logística tiene un área bajo la curva de 0,631 en el conjunto de prueba.

 ![evaluate](media/how-to-designer-sample-classification-flight-delay/sample6-evaluate-1225.png)

## <a name="next-steps"></a>Pasos siguientes

Explore otros ejemplos disponibles para el diseñador:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-designer-sample-regression-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Ejemplo 3: clasificación con selección de características: Predicción de ingresos](how-to-designer-sample-classification-predict-income.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](how-to-designer-sample-classification-churn.md)
- [Ejemplo 7 - Clasificación de texto: Conjunto de datos de SP 500 de Wikipedia](how-to-designer-sample-text-classification.md)