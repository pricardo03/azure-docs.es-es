---
title: 'Entrenar modelo: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Aprenda a usar el módulo **Entrenar modelo** en Azure Machine Learning Service para entrenar un modelo de clasificación o regresión.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 29d0f698456b83c1520a92bc7df47b26540325f4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028116"
---
# <a name="train-model-module"></a>Módulo Entrenar modelo

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Utilice este módulo para entrenar un modelo de clasificación o regresión. El entrenamiento tiene lugar después de definir un modelo y establecer sus parámetros y requiere datos etiquetados. También puede usar **Entrenar modelo** para volver a entrenar un modelo existente con datos nuevos. 

## <a name="how-the-training-process-works"></a>Funcionamiento del proceso de entrenamiento

En Azure Machine Learning, crear y usar un modelo suele ser un proceso de tres pasos. 

1. Un modelo se configura eligiendo un determinado tipo de algoritmo y definiendo sus parámetros o hiperparámetros. Elija cualquiera de los siguientes tipos de modelo: 

    + Los modelos de **clasificación**, que se basan en redes neuronales, árboles de decisión y bosques de decisión, así como otros algoritmos.
    + Los modelos de **regresión**, que pueden incluir la regresión lineal estándar o que usan otros algoritmos, incluidas las redes neuronales y la regresión bayesiana.  

2. Proporcione un conjunto de datos que esté etiquetado y tenga datos compatibles con el algoritmo. Conecte los datos y el modelo a **Entrenar modelo**.

    El entrenamiento produce un formato binario concreto, iLearner, que encapsula los patrones estadísticos aprendidos a partir de los datos. No se puede modificar o leer directamente este formato; sin embargo, otros módulos pueden usar el modelo entrenado. 
    
    También puede ver las propiedades del modelo. Para más información, consulte la sección Resultados.

3. Una vez completado el entrenamiento, utilice el modelo entrenado con uno de los [módulos de puntuación](./score-model.md), para realizar predicciones sobre nuevos datos.

## <a name="how-to-use-train-model"></a>Procedimiento para usar **Entrenar modelo**  
  
1.  En Azure Machine Learning, configure un modelo de regresión o modelo de clasificación.
    
2. Agregue el módulo **Entrenar modelo** al experimento.  Puede encontrar este módulo en la categoría **Machine Learning**. Expanda **Entrenar** y, a continuación, arrastre el módulo **Entrenar modelo** al experimento.
  
3.  En la entrada izquierda, adjunte el modo no entrenado. Adjunte el conjunto de datos de entrenamiento a la entrada de la derecha de **Entrenar modelo**.

    El conjunto de datos de entrenamiento debe contener una columna de etiqueta. Se omiten todas las filas sin etiquetas.
  
4.  Para **Columna de etiqueta**, haga clic en **Launch column selector** (Iniciar selector de columna) y elija una sola columna que contenga los resultados que el modelo puede usar para el entrenamiento.
  
    - Para problemas de clasificación, la columna de etiqueta debe contener valores **categoriales** o valores **discretos**. Algunos ejemplos podrían ser una calificación de sí o no, un código o nombre de clasificación de enfermedades o un grupo de ingresos.  Si elige una columna no categorial, el módulo devolverá un error durante el entrenamiento.
  
    -   Para problemas de regresión, la columna de etiqueta debe contener datos **numéricos** que representen la variable de respuesta. Lo ideal es que los datos numéricos representen una escala continua. 
    
    Podrían ser ejemplos una puntuación de riesgo de crédito, el tiempo proyectado para el error de una unidad de disco duro o el número previsto de llamadas a un centro de llamadas en un día u hora determinadas.  Si no elige una columna numérica, puede obtener un error.
  
    -   Si no especifica qué columna de etiqueta debe utilizar, Azure Machine Learning intentará inferir cuál es la columna de etiqueta adecuada usando los metadatos del conjunto de datos. Si elige la columna errónea, utilice el selector de columnas para corregirlo.
  
    > [!TIP] 
    > Si tiene problemas para usar el selector de columnas, consulte el artículo [Select Columns in Dataset](./select-columns-in-dataset.md) (Seleccionar columnas en conjunto de datos) para obtener sugerencias. En él, se describen algunos escenarios y sugerencias comunes para usar las opciones **WITH RULES** (CON REGLAS) y **BY NAME** (POR NOMBRE).
  
5.  Ejecute el experimento. Si tiene una gran cantidad de datos, puede tardar.

## <a name="bkmk_results"></a> Resultados

Cuando el modelo está entrenado:

+ Para ver los parámetros del modelo y los pesos de característica, haga clic con el botón derecho en la salida y seleccione **Visualizar**.
+ Para usar el modelo en otros experimentos, haga clic con el botón derecho en el modelo y seleccione **Guardar modelo**. Escriba un nombre para el modelo. 

    De este modo se guarda el modelo como una instantánea que las ejecuciones repetidas del experimento no actualizan.
+ Para usar el modelo para predecir nuevos valores, conéctelo al módulo [Puntuar modelo](./score-model.md), junto con nuevos datos de entrada.


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 