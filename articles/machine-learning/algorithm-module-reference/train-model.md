---
title: 'Entrenar el modelo: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Aprenda a usar el **entrenar modelo** módulo en el servicio de Azure Machine Learning para entrenar un modelo de clasificación o regresión.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 29d0f698456b83c1520a92bc7df47b26540325f4
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028116"
---
# <a name="train-model-module"></a>Módulo Entrenar modelo

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para entrenar un modelo de clasificación o regresión. Formación tiene lugar después de haber definido un modelo y establecer sus parámetros y requiere que los datos etiquetados. También puede usar **Train Model** volver a entrenar un modelo existente con datos nuevos. 

## <a name="how-the-training-process-works"></a>Cómo funciona el proceso de entrenamiento

En Azure Machine Learning, crear y usar un modelo de aprendizaje automático suele ser un proceso de tres pasos. 

1. Configuración de un modelo, eligiendo un determinado tipo de algoritmo y definir sus parámetros o los hiperparámetros. Elija cualquiera de los siguientes tipos de modelo: 

    + **Clasificación** modelos, basados en redes neuronales, árboles de decisión y los bosques de decisión y otros algoritmos.
    + **Regresión** modelos, que puede incluir la regresión lineal estándar o que usan otros algoritmos, incluidas las redes neuronales y regresión bayesiana.  

2. Proporcionar un conjunto de datos que tiene la etiqueta y tiene datos compatibles con el algoritmo. Conecte los datos y el modelo que **Train Model**.

    Genera el aprendizaje es un formato binario concreto, el iLearner, que encapsula los patrones estadísticos de los datos. No se puede modificar directamente o leer este formato; Sin embargo, otros módulos pueden usar este modelo entrenado. 
    
    También puede ver las propiedades del modelo. Para obtener más información, consulte la sección de resultados.

3. Una vez completado el entrenamiento, utilice el modelo entrenado con uno de los [módulos de puntuación](./score-model.md), para realizar predicciones sobre nuevos datos.

## <a name="how-to-use-train-model"></a>Cómo usar **entrenar modelo**  
  
1.  En Azure Machine Learning, configure un modelo de regresión o modelo de clasificación.
    
2. Agregar el **Train Model** módulo en el experimento.  Puede encontrar este módulo en el **Machine Learning** categoría. Expanda **"Train"** y, a continuación, arrastre el **Train Model** módulo en el experimento.
  
3.  En la entrada izquierda, adjunte el modo no entrenado. Asociar el conjunto de datos de entrenamiento a la entrada derecha de **Train Model**.

    El conjunto de datos de entrenamiento debe contener una columna de etiqueta. Se omiten todas las filas sin etiquetas.
  
4.  Para **columna de etiqueta**, haga clic en **iniciar el selector de columna**y elegir una sola columna que contiene los resultados que se puede usar el modelo para el entrenamiento.
  
    - Para problemas de clasificación, la columna de etiqueta debe contener **categorías** valores o **discretos** valores. Algunos ejemplos podrían ser una clasificación de sí/no, un código de clasificación enfermedades o nombre o un grupo de ingresos.  Si elige una columna de no categoría, el módulo devolverá un error durante el entrenamiento.
  
    -   Problemas de regresión, debe contener la columna de etiqueta **numérico** datos que representa la variable de respuesta. Lo ideal es que los datos numéricos representan una escala continua. 
    
    Ejemplos pueden ser una puntuación de riesgo de crédito, el tiempo proyectado para el error para una unidad de disco duro o el número previsto de llamadas a un centro de llamadas en un día determinado o una hora.  Si no elige una columna numérica, podría obtener un error.
  
    -   Si no especifica qué columna de etiqueta debe utilizar, Azure Machine Learning intentará inferir cuál es la columna de etiqueta adecuada usando los metadatos del conjunto de datos. Si elige la columna errónea, utilice el selector de columnas para corregirlo.
  
    > [!TIP] 
    > Si tiene problemas para usar el Selector de columnas, consulte el artículo [Select Columns in Dataset](./select-columns-in-dataset.md) para obtener sugerencias. Se describen algunos escenarios comunes y sugerencias para usar el **con reglas** y **por nombre** opciones.
  
5.  Ejecute el experimento. Si tiene una gran cantidad de datos, esto puede tardar unos minutos.

## <a name="bkmk_results"></a> Resultados

Después del modelo se entrena:

+ Para ver los parámetros del modelo y los pesos de característica, haga clic en los resultados y seleccionar **visualizar**.
+ Para usar el modelo en otros experimentos, haga clic en el modelo y seleccione **Guardar modelo**. Escriba un nombre para el modelo. 

    Esto guarda el modelo como una instantánea que no se actualiza mediante ejecuciones repetidas del experimento.
+ Para usar el modelo para predecir nuevos valores, conéctelo a la [Score Model](./score-model.md) módulo, junto con nuevos datos de entrada.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 