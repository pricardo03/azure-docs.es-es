---
title: 'Regresión logística multiclase: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo regresión logística Multiclase en el servicio de Azure Machine Learning para crear un modelo de regresión logística que puede utilizarse para predecir varios valores.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ac4310e851808d6e6d89d1a2b506975eea3b1d69
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029331"
---
# <a name="multiclass-logistic-regression-module"></a>Módulo de regresión logística multiclase

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de regresión logística que puede utilizarse para predecir varios valores.

Clasificación mediante regresión logística es un método de aprendizaje supervisado y, por lo tanto, requiere un conjunto de datos etiquetado. Entrenar el modelo, ya que proporciona el modelo y el conjunto de datos etiquetado como entrada a un módulo como [entrenar modelo](./train-model.md). A continuación, se puede usar el modelo entrenado para predecir valores para nuevos ejemplos de entrada.

Azure Machine Learning también proporciona un [regresión logística de dos clases](./two-class-logistic-regression.md) módulo, que es adecuado para la clasificación de variables dicotómicas o binarias.

## <a name="about-multiclass-logistic-regression"></a>Acerca de la regresión logística multiclase

Regresión logística es un método conocido en estadística que se usa para predecir la probabilidad de un resultado y es popular para las tareas de clasificación. El algoritmo predice la probabilidad de aparición de un evento ajustando los datos a una función logística. 

En la regresión logística multiclase, el clasificador puede usarse para predecir varios resultados.

## <a name="configure-a-multiclass-logistic-regression"></a>Configurar una regresión logística multiclase

1. Agregar el **regresión logística Multiclase** módulo en el experimento.

2. Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.

    + **Único parámetro**: Use esta opción si sabe cómo desea configurar el modelo y proporcionar un conjunto específico de valores como argumentos.

    + **Intervalo de parámetros**: Use esta opción si no está seguro de los mejores parámetros y desea utilizar un barrido de parámetros.

3. **Tolerancia de optimización**, especifique el valor de umbral de convergencia del optimizador. Si la mejora entre iteraciones es menor que el umbral, el algoritmo se detiene y devuelve el modelo actual.

4. **Peso de regularización L1**, **peso de regularización L2**: Escriba un valor que se usará para los parámetros de regularización L1 y L2. Se recomienda un valor distinto de cero para ambos.

    La regularización es un método para evitar el sobreajuste penalizando los modelos con valores de coeficiente extreme. La regularización funciona agregando la penalización asociada con los valores de coeficiente al error de la hipótesis. Un modelo preciso con valores de coeficiente de extremos se penalizaría más, pero un modelo menos preciso con valores más conservadores se penalizaría menos.

     Regularización L1 y L2 tienen efectos diferentes y usa. L1 puede aplicarse a modelos dispersos, lo que resulta útil cuando se trabaja con datos muy dimensionales. En cambio, la regularización L2 es preferible para los datos que no están dispersas.  Este algoritmo es compatible con una combinación lineal de los valores de regularización L1 y L2: es decir, si `x = L1` y `y = L2`, `ax + by = c` define el intervalo lineal de los términos de regularización.

     Diferentes combinaciones de lineal de los términos de L1 y L2 se han diseñado para los modelos de regresión logística, tales como [elástica regularización neta](https://wikipedia.org/wiki/Elastic_net_regularization).

6. **Valor de inicialización de números aleatorios**: Escriba un valor entero a usar como valor de inicialización para el algoritmo si desea que los resultados se puedan repetir entre ejecuciones. En caso contrario, un valor de reloj del sistema se utiliza como valor de inicialización, lo que puede producir resultados ligeramente diferentes en las ejecuciones del mismo experimento.

8. Conecte un conjunto de datos con la etiqueta y uno de los módulos "Train":

    + Si establece **crear modo de entrenador** a **único parámetro**, utilice el [Train Model](./train-model.md) módulo.

9. Ejecute el experimento.

## <a name="results"></a>Results

Una vez completado el entrenamiento, puede ver un resumen de los parámetros del modelo, junto con los pesos de característica aprendidos de entrenamiento, haga clic en la salida de la [Train Model](./train-model.md) módulo y seleccione **visualizar**.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 