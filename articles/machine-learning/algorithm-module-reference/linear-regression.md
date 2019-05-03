---
title: 'Regresión lineal: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de regresión lineal en el servicio Azure Machine Learning para crear un modelo de regresión lineal para su uso en un experimento.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 040f726a703d34a95bae7d5b7cdd766655c62a4e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029721"
---
# <a name="linear-regression-module"></a>Módulo de regresión lineal
En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de regresión lineal para su uso en un experimento.  Regresión lineal intenta establecer una relación lineal entre una o más variables independientes y un resultado numérico o la variable dependiente. 

Utilice este módulo para definir un método de regresión lineal y, a continuación, entrenar un modelo con un conjunto de datos etiquetado. El modelo entrenado, a continuación, se puede usar para realizar predicciones.

## <a name="about-linear-regression"></a>Acerca de la regresión lineal

Regresión lineal es un método estadístico comunes, que se han adoptado en machine learning y ha mejorado con muchos métodos nuevos para ajustar la línea y medir el error. En el sentido más básico, regresión se refiere a la predicción de un destino numérico. Regresión lineal sigue siendo una buena opción cuando desee un modelo simple para una tarea de predicción básica. Regresión lineal suele funcionar bien en conjuntos de datos muy dimensionales, dispersos sin complejidad también.

Azure Machine Learning admite una variedad de modelos de regresión, además de regresión lineal. Sin embargo, se puede interpretar el término "regresión" imprecisa y no se admiten algunos tipos de regresión que se proporciona en otras herramientas.

+ El problema de regresión clásico implica una sola variable independiente y una variable dependiente. Esto se denomina *regresión simple*.  Este módulo admite regresión simple.

+ *Regresión lineal múltiple* implica dos o más variables independientes que contribuyen a una sola variable dependiente. Problemas en el que se usan varias entradas para predecir un resultado numérico único también se denominan *regresión lineal multivariada*.

    El **regresión lineal** módulo puede resolver estos problemas, igual que la mayoría de los demás módulos de regresión.

+ *Regresión multietiqueta* es la tarea de predicción de diferentes variables dependientes dentro de un único modelo. Por ejemplo, en una regresión logística multietiqueta, una muestra puede asignarse a varias etiquetas diferentes. (Esto es diferente de la tarea de predicción de varios niveles dentro de una variable de clase única).

    No se admite este tipo de regresión en Azure Machine Learning. Para predecir varias variables, crear un aprendiz independiente para cada salida que desea predecir.

Durante años estadísticos llevan tiempo desarrollando métodos cada vez más avanzados para la regresión. Esto es cierto incluso para la regresión lineal. Este módulo admite dos métodos para medir el error y se ajustan a la línea de regresión: método de mínimos cuadrados ordinarios y descenso de gradiente.

- **Descenso de gradiente** es un método que minimiza la cantidad de errores en cada paso del proceso de entrenamiento de modelo. Existen muchas variaciones del descenso de gradiente y su optimización para los diversos problemas de aprendizaje ha sido estudiado. Si elige esta opción para **método solución**, puede establecer diversos parámetros para controlar el tamaño de paso, velocidad de aprendizaje y así sucesivamente. Esta opción también admite el uso de un barrido de parámetros integrado.

- **Ordinaria de mínimos cuadrados** es una de las técnicas empleadas con más frecuencia en la regresión lineal. Por ejemplo, de mínimos cuadrados es el método que se usa en las herramientas de análisis de Microsoft Excel.

    Ordinaria de mínimos cuadrados hace referencia a la función de pérdida, que calcula el error como la suma del cuadrado de la distancia entre el valor real y la línea predicha y se ajusta al modelo minimizando el error cuadrático. Este método supone una sólida relación lineal entre las entradas y la variable dependiente.

## <a name="configure-linear-regression"></a>Configurar la regresión lineal

Este módulo admite dos métodos para un modelo de regresión, con diferentes opciones de conexión:

+ [Crear un modelo de regresión con descenso de gradiente en línea](#bkmk_GradientDescent)

    Descenso de gradiente es una función de pérdida mejor para los modelos que son más complejas o que tienen muy pocos datos de entrenamiento, dados el número de variables.



+ [Ajustar un modelo de regresión con mínimos cuadrados ordinarios](#bkmk_OrdinaryLeastSquares)

    Para conjuntos de datos pequeños, es mejor seleccionar ordinaria de mínimos cuadrados. Se mostrará resultados similares a Excel.

## <a name="bkmk_OrdinaryLeastSquares"></a> Crear un modelo de regresión con mínimos cuadrados ordinarios

1. Agregar el **modelo de regresión lineal** módulo al experimento en la interfaz.

    Puede encontrar este módulo en el **Machine Learning** categoría. Expanda **Initialize Model**, expanda **regresión**y, a continuación, arrastre el **modelo de regresión lineal** módulo al experimento.

2. En el **propiedades** panel, en el **método solución** lista desplegable, seleccione **ordinaria de mínimos cuadrados**. Esta opción especifica el método de cálculo que se utiliza para buscar la recta de regresión.

3. En **peso de regularización L2**, escriba el valor que se usará como el peso de regularización L2. Se recomienda que utilice un valor distinto de cero para evitar el sobreajuste.

     Para obtener más información sobre cómo regularización afecta a la conexión de modelo, consulte este artículo: [Regularización L1 y L2 para el aprendizaje automático](https://msdn.microsoft.com/magazine/dn904675.aspx)

4. Seleccione la opción **término de intercepción de inclusión**, si desea ver el término para la intercepción.

    Desactive esta opción si no es necesario revisar la fórmula de regresión.

5. Para **valor de inicialización aleatorio número**, también puede escribir un valor para inicializar el generador de números aleatorios utilizado por el modelo.

    Utilizar un valor de inicialización es útil si desea mantener los mismos resultados entre distintas ejecuciones del mismo experimento. En caso contrario, el valor predeterminado es usar un valor comprendido entre el reloj del sistema.


7. Agregar el [Train Model](./train-model.md) módulo al experimento y conecte un conjunto de datos con la etiqueta.

8. Ejecute el experimento.

## <a name="results-for-ordinary-least-squares-model"></a>Resultados para el modelo de mínimos cuadrados ordinarios

Una vez completada la formación:

+ Para ver los parámetros del modelo, haga clic en la salida de instructor y seleccione **visualizar**.

+ Para realizar predicciones, conecte el modelo entrenado para la [Score Model](./score-model.md) módulo, junto con un conjunto de datos de los nuevos valores. 


## <a name="bkmk_GradientDescent"></a> Crear un modelo de regresión con descenso de gradiente en línea

1. Agregar el **modelo de regresión lineal** módulo al experimento en la interfaz.

    Puede encontrar este módulo en el **Machine Learning** categoría. Expanda **Initialize Model**, expanda **regresión**y arrastre el **modelo de regresión lineal** módulo al experimento

2. En el **propiedades** panel, en el **método solución** lista desplegable, elija **descenso de gradiente en línea** como el método de cálculo que se utiliza para buscar la recta de regresión.

3. Para **crear modo de entrenador**, indicar si desea entrenar el modelo con un conjunto predefinido de parámetros, o si desea optimizar el modelo mediante el uso de un barrido de parámetros.

    + **Único parámetro**: Si sabe cómo desea configurar la red de regresión lineal, puede proporcionar un conjunto específico de valores como argumentos.

   
4. Para **velocidad de aprendizaje**, especificar la velocidad de aprendizaje inicial para el optimizador de descenso de gradiente estocástico.

5. Para **número de épocas de entrenamiento**, escriba un valor que indica el número de veces que el algoritmo debe iterar a través de ejemplos. Para conjuntos de datos con un pequeño número de ejemplos, este número debe ser grande para alcanzar la convergencia.

6. **Normalizar características**: Si ya dispone de normalizar los datos numéricos utilizados para entrenar el modelo, puede anular la selección de esta opción. De forma predeterminada, el módulo normaliza todas las entradas numéricas a un intervalo entre 0 y 1.

    > [!NOTE]
    > 
    > Recuerde que para aplicar el mismo método de normalización a los nuevos datos que se usa para la puntuación.

7. En **peso de regularización L2**, escriba el valor que se usará como el peso de regularización L2. Se recomienda que utilice un valor distinto de cero para evitar el sobreajuste.

    Para obtener más información sobre cómo regularización afecta a la conexión de modelo, consulte este artículo: [Regularización L1 y L2 para el aprendizaje automático](https://msdn.microsoft.com/magazine/dn904675.aspx)


9. Seleccione la opción **disminuir velocidad de aprendizaje**, si desea que la velocidad de aprendizaje para reducir a medida que las iteraciones progresan.  

10. Para **valor de inicialización aleatorio número**, también puede escribir un valor para inicializar el generador de números aleatorios utilizado por el modelo. Utilizar un valor de inicialización es útil si desea mantener los mismos resultados entre distintas ejecuciones del mismo experimento.


12. Agregue un conjunto de datos con la etiqueta y uno de los módulos de entrenamiento.

    Si no utiliza un barrido de parámetros, use la [Train Model](train-model.md) módulo.

13. Ejecute el experimento.

## <a name="results-for-online-gradient-descent"></a>Resultados de descenso de gradiente en línea

Una vez completada la formación:

+ Para realizar predicciones, conecte el modelo entrenado para la [Score Model](./score-model.md) módulo, junto con nuevos datos de entrada.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 