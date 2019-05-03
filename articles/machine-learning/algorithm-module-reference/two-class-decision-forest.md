---
title: 'Bosque de decisión de dos clases: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de bosque de decisión de dos clases en el servicio de Azure Machine Learning para crear un modelo basado en el algoritmo de bosques de decisión de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f3e7cab6e33fa9373095441685f6ecb04f1d91a5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029376"
---
# <a name="two-class-decision-forest-module"></a>Módulo de bosque de decisión Multiclase

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo basado en el algoritmo de bosques de decisión de aprendizaje automático.  

Bosques de decisión son modelos de conjunto rápidos y supervisados. Este módulo es una buena opción si desea predecir un destino con un máximo de dos resultados. 

## <a name="understanding-decision-forests"></a>Descripción de los bosques de decisión

Este algoritmo de bosque de decisión es un método diseñado para las tareas de clasificación de aprendizaje de conjunto. Métodos de conjunto se basan en el principio general que en lugar de depender de un único modelo, puede obtener mejores resultados y un modelo más generalizado mediante la creación de varios modelos relacionados y combinarlas de alguna manera. Por lo general, los modelos de conjunto proporcionan una mejor cobertura y precisión que los árboles de decisión únicos. 

Hay muchas maneras de crear modelos individuales y combinarlos en un conjunto. En esta implementación de un bosque de decisión funciona creando varios árboles de decisión y, a continuación, **votación** en la clase de salida más popular. El voto es uno de los métodos más conocidos para generar los resultados en un modelo de conjunto. 

+ Se crean varios árboles de clasificación individuales, con todo el conjunto de datos, pero diferentes (normalmente aleatorio) puntos de partida. Esto difiere del enfoque de bosque aleatorio, en el que los árboles de decisión individual pueden usar solo una parte aleatoria de los datos o las características.
+ Cada árbol del árbol de decisión bosque da como resultado un histograma de frecuencia no normalizado de etiquetas. 
+ El proceso de agregación suma estos histogramas y normaliza el resultado para obtener las "probabilidades" para cada etiqueta. 
+ Los árboles que se tienen confianza en la predicción alta tendrán un peso mayor en la decisión final del conjunto.

En general, los árboles de decisión tienen numerosas ventajas para las tareas de clasificación:
  
- Puede capturar los límites de decisión no lineales.
- Puede entrenar y predecir en grandes cantidades de datos, ya que son eficaces en el uso de memoria y de cálculo.
- Selección de características está integrada en los procesos de aprendizaje y la clasificación.  
- Árboles pueden adaptarse a los datos con ruido y muchas características.  
- Son modelos no paramétricos, lo que significa que pueden controlar los datos con distribuciones variadas. 

Sin embargo, los árboles de decisión simple pueden saturar en los datos y son menos generalizables de árboles de árbol.

Para obtener más información, consulte [bosques de decisión](http://go.microsoft.com/fwlink/?LinkId=403677).  

## <a name="how-to-configure"></a>Configuración
  
1.  Agregar el **bosque de decisión de dos clases** módulo al experimento en Azure Machine Learning y abra el **propiedades** panel del módulo. 

    Puede encontrar el módulo en **Machine Learning**. Expanda **inicializar**y, a continuación, **clasificación**.  
  
2.  Para **volver a muestrear método**, elija el método utilizado para crear los árboles individuales.  Puede elegir entre **Bagging** o **replicar**.  
  
    -   **Bagging**: También se denomina ensacado *agregación bootstrap*. En este método, cada árbol se aumentó en un nuevo ejemplo creado mediante el muestreo de forma aleatoria el conjunto de datos original con el reemplazo de hasta que haya un conjunto de datos, el tamaño del original.  
  
         Las salidas de los modelos se combinan mediante *votación*, que es una forma de agregación. Cada árbol en un bosque de decisión de clasificación da como resultado un histograma de frecuencia sin normalizar de etiquetas. La agregación es sumar estos histogramas y normalizar para obtener las "probabilidades" para cada etiqueta. De esta manera, los árboles que se tienen confianza en la predicción alta tendrán un peso mayor en la decisión final del conjunto.  
  
         Para obtener más información, vea la entrada de Wikipedia para la agregación de Bootstrap.  
  
    -   **Replicar**: En la replicación, cada árbol se entrena con exactamente los mismos datos de entrada. La determinación de qué división predicado se utiliza para cada nodo de árbol permanece aleatoria y los árboles será diversos.   
  
3.  Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.  
  
    -   **Único parámetro**: Si sabe cómo desea configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos.
  
4.  Para **número de árboles de decisión**, escriba el número máximo de árboles de decisión que se pueden crear en el conjunto. Si crea más árboles de decisión, puede obtener una mejor cobertura, pero aumenta el tiempo de entrenamiento.  
  
    > [!NOTE]
    >  Este valor también controla el número de árboles que se muestra cuando se visualice el modelo entrenado. Si desea ver o imprimir un único árbol, puede establecer el valor en 1. Sin embargo, puede ser un único árbol generado (el árbol con el conjunto inicial de parámetros) y no se realizan las iteraciones adicionales.
  
5.  Para **profundidad máxima de los árboles de decisión**, escriba un número para limitar la profundidad máxima de cualquier árbol de decisión. Al aumentar la profundidad del árbol podría aumentar la precisión, con un tiempo de entrenamiento de sobreajuste y aumente el riesgo.
  
6.  Para **número de divisiones aleatorias por nodo**, escriba el número de divisiones que se usará al crear cada nodo del árbol. Un *dividir* significa que las características de cada nivel del árbol (nodo) se divide al azar.
  
7.  Para **número mínimo de muestras por nodo hoja**, indicar el número mínimo de casos que son necesarios para crear cualquier nodo terminal (hoja) en un árbol.
  
     Al aumentar este valor, aumente el umbral para la creación de nuevas reglas. Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener al menos cinco casos que cumplen las condiciones.  
  
8.  Seleccione el **permitir valores desconocidos para características de categorías** opción para crear un grupo de valores desconocidos en los conjuntos de entrenamiento o validación. El modelo podría ser menos preciso con valores conocidos, pero proporcione mejores predicciones para los valores nuevos (desconocidos). 

     Si desactiva esta opción, el modelo podrá aceptar únicamente los valores que se encuentran en los datos de entrenamiento.
  
9. Asociar un conjunto de datos con la etiqueta y uno de los [módulos de formación](module-reference.md):  
  
    -   Si establece **crear modo de entrenador** a **único parámetro**, utilice el [Train Model](./train-model.md) módulo.  
  
    
## <a name="results"></a>Results

Una vez completada la formación:

+ Para ver el árbol de la que se creó en cada iteración, haga clic en la salida de la [Train Model](./train-model.md) módulo y seleccione **visualizar**.
  
    Haga clic en cada árbol para explorar en profundidad las divisiones y ver las reglas para cada nodo.

+ Para guardar una instantánea del modelo, haga clic en el **entrenado** de salida y seleccione **Guardar modelo**. No se actualiza el modelo guardado en ejecuciones sucesivas del experimento.

+ Para usar el modelo de puntuación, agregue el **Score Model** módulo en un experimento.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 