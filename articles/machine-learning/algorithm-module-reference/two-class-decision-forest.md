---
title: 'Bosque de decisión de dos clases: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de bosque de decisión de dos clases en el servicio Azure Machine Learning para crear un modelo de Machine Learning basado en el algoritmo de bosques de decisión.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 73b7822c56e2b07eeefdedce1bce6d410d110ebc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65411477"
---
# <a name="two-class-decision-forest-module"></a>Módulo de bosque de decisión de dos clases

En este artículo se describe un módulo de la interfaz visual (versión preliminar) del servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de Machine Learning basado en el algoritmo de bosques de decisión.  

Los bosques de decisión son modelos de conjunto rápidos y supervisados. Este módulo es una buena opción si quiere predecir un destino con un máximo de dos resultados. 

## <a name="understanding-decision-forests"></a>Descripción de los bosques de decisión

Este algoritmo de bosque de decisión es un método de aprendizaje de conjunto diseñado para las tareas de clasificación. Los métodos de conjunto se basan en el principio general de que, en lugar de depender de un único modelo, puede obtener mejores resultados y un modelo más generalizado mediante la creación de varios modelos relacionados y combinarlos de alguna manera. Por lo general, los modelos de conjunto proporcionan mejor cobertura y precisión que los árboles de decisión únicos. 

Hay muchas maneras de crear modelos individuales y combinarlos en un conjunto. En esta implementación concreta de un bosque de decisión se crean varios árboles de decisión y, a continuación, **se vota** la clase de resultado más popular. Votar es uno de los métodos más conocidos para generar resultados en un modelo de conjunto. 

+ Se crean muchos árboles de clasificación individuales, con todo el conjunto de datos, pero puntos de partida diferentes (normalmente aleatorios). Esto difiere del enfoque de bosque aleatorio, en el que los árboles de decisión individuales pueden usar solo una parte aleatoria de los datos o de las características.
+ Cada árbol del bosque de decisión produce un histograma de etiquetas de frecuencia no normalizada. 
+ El proceso de agregación suma estos histogramas y normaliza el resultado para obtener las "probabilidades" de cada etiqueta. 
+ Los árboles con un nivel alto de confianza en la predicción tendrán un peso mayor en la decisión final del conjunto.

En general, los árboles de decisión tienen numerosas ventajas para las tareas de clasificación:
  
- Pueden capturar los límites de decisión no lineales.
- Usted puede entrenar grandes cantidades de datos y hacer predicciones con ellos, ya que son eficaces en el cálculo y el uso de memoria.
- La selección de características está integrada en los procesos de entrenamiento y clasificación.  
- Los árboles pueden adaptarse a los datos con ruido y a muchas características.  
- Son modelos no paramétricos, lo que significa que pueden controlar datos con distribuciones variadas. 

Sin embargo, los árboles de decisión simples pueden sobreajustar los datos y son menos generalizables que los conjuntos de árboles.

Para obtener más información, consulte [Bosques de decisión](https://go.microsoft.com/fwlink/?LinkId=403677).  

## <a name="how-to-configure"></a>Configuración
  
1.  Agregue el módulo del **bosque de decisión de dos clases** al experimento en Azure Machine Learning y abra el panel **Propiedades** del módulo. 

    Puede encontrar el módulo en **Machine Learning**. Expanda **Inicializar** y, a continuación, **Clasificación**.  
  
2.  Para obtener información sobre el **método de nuevo muestreo**, elija el método utilizado para crear los árboles individuales.  Puede elegir entre **agregación** o **replicación**.  
  
    -   **Agregación**: La agregación también se denomina *agregación de arranque*. En este método, cada árbol crece en una muestra nueva, creada al muestrear de forma aleatoria el conjunto de datos original con el conjunto de reemplazo hasta que haya un conjunto de datos con el tamaño del original.  
  
         Los resultados de los modelos se combinan mediante *votación*, que es una forma de agregación. Cada árbol de un bosque de decisión de clasificación produce un histograma de etiquetas de frecuencia no normalizada. En el proceso de agregación se suman estos histogramas y se normalizan para obtener las "probabilidades" de cada etiqueta. De esta manera, los árboles con un nivel alto de confianza en la predicción tendrán un peso mayor en la decisión final del conjunto.  
  
         Para obtener más información, consulte la entrada de Wikipedia sobre la agregación de arranque.  
  
    -   **Replicación**: En la replicación, cada árbol se entrena exactamente con los mismos datos de entrada. La determinación de qué predicado de división se utiliza para cada nodo de árbol sigue siendo aleatoria y los árboles serán diversos.   
  
3.  Para especificar cómo quiere que se entrene el modelo, establezca la opción **Create trainer mode** (Crear modo entrenador).  
  
    -   **Parámetro único**: Si sabe cómo quiere configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos.
  
4.  En **Número de árboles de decisión**, escriba el número máximo de árboles de decisión que se pueden crear en el conjunto. Si crea más árboles de decisión, puede obtener potencialmente mejor cobertura, pero aumenta el tiempo de entrenamiento.  
  
    > [!NOTE]
    >  Este valor también controla el número de árboles que se muestran al visualizar el modelo entrenado. Si quiere ver o imprimir un único árbol, puede establecer el valor en 1. Sin embargo, solo se puede producir un único árbol (el árbol con el conjunto inicial de parámetros) y no se realizan iteraciones adicionales.
  
5.  En **Profundidad máxima de los árboles de decisión**, escriba un número para limitar la profundidad máxima de cualquier árbol de decisión. Al aumentar la profundidad del árbol podría aumentar la precisión, a riesgo de que se produzca un sobreajuste y aumente el tiempo de entrenamiento.
  
6.  En **Número de divisiones aleatorias por nodo**, escriba el número de divisiones que se usarán al crear cada nodo del árbol. Una *división* significa que las características de cada nivel del árbol (nodo) se dividen al azar.
  
7.  En **Número mínimo de muestras por nodo hoja**, indique el número mínimo de casos que son necesarios para crear cualquier nodo terminal (hoja) en un árbol.
  
     Al aumentar este valor, aumenta el umbral para crear reglas nuevas. Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener cinco casos como mínimo que cumplan las mismas condiciones.  
  
8.  Seleccione la opción **Permitir valores desconocidos para las características categóricas** para crear un grupo de valores desconocidos en los conjuntos de entrenamiento o validación. El modelo podría ser menos preciso con valores conocidos, pero puede proporcionar mejores predicciones para los valores nuevos (desconocidos). 

     Si desactiva esta opción, el modelo puede aceptar únicamente los valores que se encuentran en los datos de entrenamiento.
  
9. Adjunte un conjunto de datos etiquetados y uno de los [módulos de entrenamiento](module-reference.md):  
  
    -   Si establece **Create trainer mode** (Crear modo entrenador) en **Single Parameter** (Parámetro único), use el módulo [Modelo de entrenamiento](./train-model.md).  
  
    
## <a name="results"></a>Results

Una vez completado el entrenamiento:

+ Para ver el árbol que se ha creado en cada iteración, haga clic con el botón derecho en el resultado del módulo [Modelo de entrenamiento](./train-model.md) y seleccione **Visualizar**.
  
    Haga clic en cada árbol para explorar en profundidad las divisiones y ver las reglas de cada nodo.

+ Para guardar una instantánea del modelo, haga clic con el botón derecho en el resultado de **Modelo de entrenamiento** y seleccione **Guardar modelo**. El modelo guardado no se actualiza en ejecuciones sucesivas del experimento.

+ Para usar el modelo de puntuación, agregue el módulo **Modelo de puntuación** a un experimento.


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 