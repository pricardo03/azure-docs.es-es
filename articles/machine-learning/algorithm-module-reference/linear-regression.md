---
title: 'Regresión lineal: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Aprenda a usar el módulo de regresión lineal en Azure Machine Learning Service para crear un modelo de regresión lineal para usarlo en un experimento.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 040f726a703d34a95bae7d5b7cdd766655c62a4e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029721"
---
# <a name="linear-regression-module"></a>Módulo de regresión lineal
En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Utilice este módulo para crear un modelo de regresión lineal para usarlo en un experimento.  La regresión lineal intenta establecer una relación lineal entre una o más variables independientes y un resultado numérico o la variable dependiente. 

Utilice este módulo para definir un método de regresión lineal y entrenar un modelo con un conjunto de datos etiquetado. El modelo entrenado se podrá usar para realizar predicciones.

## <a name="about-linear-regression"></a>Acerca de la regresión lineal

La regresión lineal es un método estadístico habitual que se han adoptado en Machine Learning y se ha mejorado con muchos métodos nuevos de ajuste de las líneas y medición de los errores. En el sentido más básico, la regresión se refiere a la predicción de un objetivo numérico. La regresión lineal sigue siendo una buena opción cuando se desee un modelo simple para una tarea de predicción básica. La regresión lineal también suele funcionar bien en conjuntos de datos muy dimensionales, dispersos y sin complejidad.

Azure Machine Learning admite una variedad de modelos de regresión, además de la regresión lineal. Sin embargo, el término "regresión" se puede interpretar de manera imprecisa y algunos tipos de regresión que se proporcionan en otras herramientas no se admiten.

+ El problema de regresión clásico implica una sola variable independiente y una variable dependiente. Esto se denomina *regresión simple*,  que es lo que admite este módulo.

+ La *regresión lineal múltiple* implica dos o más variables independientes que contribuyen a una sola variable dependiente. Los problemas en los que se usan varias entradas para predecir un solo resultado numérico también se denominan *regresión lineal multivariante*.

    El módulo de **regresión lineal** puede resolver estos problemas, igual que la mayoría de los demás módulos de regresión.

+ La *regresión multietiqueta* es la tarea de predecir diferentes variables dependientes en un único modelo. Por ejemplo, en una regresión logística multietiqueta, una muestra puede asignarse a varias etiquetas diferentes. (Esto varía de la tarea de predicción de varios niveles dentro de una variable de clase única).

    Este tipo de regresión no se admite en Azure Machine Learning. Para predecir varias variables, cree un aprendiz independiente para cada salida que desee predecir.

Durante años los estadísticos han estado desarrollando métodos cada vez más avanzados para la regresión. Esto es así incluso para la regresión lineal. Este módulo admite dos métodos para medir el error y ajustar la recta de regresión: método de los mínimos cuadrados y el algoritmo del gradiente descendiente.

- El algoritmo del **gradiente descendiente** es un método que minimiza la cantidad de errores en cada paso del proceso de entrenamiento del modelo. Existen muchas variaciones del algoritmo y se ha estudiado ampliamente su optimización para distintos problemas de aprendizaje. Si elige esta opción como **Solution method** (Método de solución), puede establecer diversos parámetros para controlar el tamaño de los intervalos, la velocidad de aprendizaje, etc. Esta opción también admite el uso de un barrido de parámetros integrado.

- La de los **mínimos cuadrados** es una de las técnicas que se emplean con más frecuencia en regresión lineal. Por ejemplo, la técnica de los mínimos cuadrados es el método que se usa en las herramientas de análisis de Microsoft Excel.

    Los mínimos cuadrados hacen referencia a la función de pérdida, que calcula el error como la suma del cuadrado de la distancia entre el valor real y la línea predicha, y ajusta el modelo minimizando el error cuadrático. Este método presupone una sólida relación lineal entre las entradas y la variable dependiente.

## <a name="configure-linear-regression"></a>Configuración de la regresión lineal

Este módulo admite dos métodos de ajuste de un modelo de regresión con distintas opciones:

+ [Creación de un modelo de regresión con gradiente descendiente en línea](#bkmk_GradientDescent)

    El gradiente descendiente es una mejor función de pérdida para los modelos más complejos o que tienen demasiado pocos datos de entrenamiento dado el número de variables.



+ [Ajuste de un modelo de regresión mediante mínimos cuadrados](#bkmk_OrdinaryLeastSquares)

    Para conjuntos de datos pequeños es mejor seleccionar la técnica de los mínimos cuadrados. Esta debería proporcionar resultados similares a Excel.

## <a name="bkmk_OrdinaryLeastSquares"></a> Creación de un modelo de regresión mediante mínimos cuadrados

1. Agregue el módulo **Linear Regression Model** (Modelo de regresión lineal) al experimento en la interfaz.

    Puede encontrar este módulo en la categoría **Machine Learning**. Expanda **Initialize Model** (Inicializar modelo) y **Regression** (Regresión) y arrastre el módulo **Linear Regression Model** (Modelo de regresión lineal) al experimento.

2. En la lista desplegable **Solution method** (Método de solución) del panel **Propiedades**, seleccione **Ordinary Least Squares** (Mínimos cuadrados). Esta opción especifica el método de cálculo que se utiliza para buscar la recta de regresión.

3. En **L2 regularization weight** (Peso de regularización L2), escriba el valor que se usará como peso de regularización L2. Se recomienda usar un valor distinto de cero para evitar el sobreajuste.

     Para más información sobre cómo afecta la regularización al ajuste del modelo, consulte este artículo: [Regularización L1 y L2 para Machine Learning](https://msdn.microsoft.com/magazine/dn904675.aspx)

4. Seleccione la opción **Include intercept term** (Incluir término para intercepción), si desea ver el término para la intercepción.

    Desactive esta opción si no necesita revisar la fórmula de regresión.

5. En **Random number seed** (Inicializar número aleatorio) también puede escribir un valor para inicializar el generador de números aleatorios que usa el modelo.

    Utilizar un valor de inicialización es útil si desea mantener los mismos resultados en distintas ejecuciones del mismo experimento. De lo contrario, lo normal es usar un valor del reloj del sistema.


7. Agregue el módulo [Train Model](./train-model.md) (Entrenar modelo) al experimento y conecte un conjunto de datos con etiqueta.

8. Ejecute el experimento.

## <a name="results-for-ordinary-least-squares-model"></a>Resultados del modelo de mínimos cuadrados

Una vez completado el entrenamiento:

+ Para ver los parámetros del modelo, haga clic con el botón derecho en la salida del entrenador y seleccione **Visualize** (Visualizar).

+ Para realizar predicciones, conecte el modelo entrenado al módulo [Score Model](./score-model.md) (Modelo de puntuación) junto con un conjunto de datos de valores nuevos. 


## <a name="bkmk_GradientDescent"></a> Creación de un modelo de regresión con gradiente descendiente en línea

1. Agregue el módulo **Linear Regression Model** (Modelo de regresión lineal) al experimento en la interfaz.

    Puede encontrar este módulo en la categoría **Machine Learning**. Expanda **Initialize Model** (Inicializar modelo) y **Regression** (Regresión) y arrastre el módulo **Linear Regression Model** (Modelo de regresión lineal) al experimento.

2. En la lista desplegable **Solution method** (Método de solución) del panel **Properties** (Propiedades), elija **Online Gradient Descent** (Gradiente descendiente en línea) como método de cálculo para buscar la recta de regresión.

3. En **Create trainer mode** (Crear modo de entrenador), indique si desea entrenar el modelo con un conjunto de parámetros predefinido o si desea optimizarlo con un barrido de parámetros.

    + **Parámetro único**: Si sabe cómo quiere configurar la red de regresión lineal, puede proporcionar un conjunto específico de valores como argumentos.

   
4. En **Learning rate** (Velocidad de aprendizaje), especifique la velocidad de aprendizaje inicial para el optimizador estocástico de gradiente descendiente.

5. En **Number of training epochs** (Épocas de entrenamiento), escriba un valor que indique el número de veces que el algoritmo debe iterarse mediante ejemplos. Para conjuntos de datos con pocos ejemplos, este número debe ser grande para alcanzar la convergencia.

6. **Normalizar las características**: si ya ha normalizado los datos numéricos que se usan para entrenar el modelo, puede anular la selección de esta opción. De forma predeterminada, el módulo normaliza todas las entradas numéricas a un intervalo de entre 0 y 1.

    > [!NOTE]
    > 
    > Recuerde aplicar el mismo método de normalización a los nuevos datos que use para la puntuación.

7. En **L2 regularization weight** (Peso de regularización L2), escriba el valor que se usará como peso de regularización L2. Se recomienda usar un valor distinto de cero para evitar el sobreajuste.

    Para más información sobre cómo afecta la regularización al ajuste del modelo, consulte este artículo: [Regularización L1 y L2 para Machine Learning](https://msdn.microsoft.com/magazine/dn904675.aspx)


9. Seleccione la opción **Decrease learning rate** (Reducir velocidad de aprendizaje), si desea que la velocidad de aprendizaje se reduzca con las iteraciones.  

10. En **Random number seed** (Inicializar número aleatorio) también puede escribir un valor para inicializar el generador de números aleatorios que usa el modelo. Utilizar un valor de inicialización es útil si desea mantener los mismos resultados en distintas ejecuciones del mismo experimento.


12. Agregue un conjunto de datos etiquetados y uno de los módulos de entrenamiento.

    Si no utiliza un barrido de parámetros, use el módulo [Train Model](train-model.md) (Entrenar modelo).

13. Ejecute el experimento.

## <a name="results-for-online-gradient-descent"></a>Resultados del gradiente descendiente en línea

Una vez completado el entrenamiento:

+ Para realizar predicciones, conecte el modelo entrenado al módulo [Score Model](./score-model.md) (Modelo de puntuación) junto con los datos de entrada nuevos.


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 