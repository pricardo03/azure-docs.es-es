---
title: 'Bosque de decisión multiclase: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de bosque de decisión Multiclase en el servicio de Azure Machine Learning para crear un modelo de machine learning en función de la *bosque de decisión* algoritmo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: d5ff1809886198f7f1e4f4b9c36f877ce61d512e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029436"
---
# <a name="multiclass-decision-forest-module"></a>Módulo de bosque de decisión multiclase

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de machine learning en función de la *bosque de decisión* algoritmo. Un bosque de decisión es un modelo de conjunto que genera rápidamente una serie de árboles de decisión, durante el aprendizaje de datos etiquetados.

## <a name="more-about-decision-forests"></a>Más información acerca de los bosques de decisión

El algoritmo de bosque de decisión es un método para la clasificación de aprendizaje de conjunto. El algoritmo funciona mediante la creación de varios árboles de decisión y, a continuación, *votación* en la clase de salida más popular. Votación es una forma de agregación, que cada árbol en una decisión de clasificación de bosque da como resultado un histograma de frecuencia no normalizado de etiquetas. El proceso de agregación suma estos histogramas y normaliza el resultado para obtener las "probabilidades" para cada etiqueta. Los árboles que se tienen confianza en la predicción alta tienen un peso mayor en la decisión final del conjunto.

En general, los árboles de decisión son modelos no paramétricos, lo que significa que admiten datos con distribuciones variadas. En cada árbol, se ejecuta una secuencia de pruebas simples para cada clase, aumentando los niveles de una estructura de árbol hasta que se alcanza un nodo hoja (decisión).

Árboles de decisión tienen numerosas ventajas:

+ Pueden representar límites de decisión no lineales.
+ Son eficientes en los cálculos y el uso de memoria durante el entrenamiento y predicción.
+ Realizar la clasificación y selección de características integradas.
+ Son resistentes en presencia de características ruidosas.

El clasificador de bosque de decisión en Azure Machine Learning consta de un conjunto de árboles de decisión. Por lo general, los modelos de conjunto proporcionan una mejor cobertura y precisión que los árboles de decisión únicos. Para obtener más información, consulte [árboles de decisión](http://go.microsoft.com/fwlink/?LinkId=403677).

## <a name="how-to-configure-multiclass-decision-forest"></a>Cómo configurar el bosque de decisión Multiclase



1. Agregar el **bosque de decisión Multiclase** módulo al experimento en la interfaz. Puede encontrar este módulo en **Machine Learning**, **Initialize Model**, y **clasificación**.

2. Haga doble clic en el módulo para abrir el **propiedades** panel.

3. Para **volver a muestrear método**, elija el método utilizado para crear los árboles individuales.  Puede elegir entre bagging o replicación.

    + **Bagging**: También se denomina ensacado *agregación bootstrap*. En este método, cada árbol se aumentó en un nuevo ejemplo creado mediante el muestreo de forma aleatoria el conjunto de datos original con el reemplazo de hasta que haya un conjunto de datos, el tamaño del original. Las salidas de los modelos se combinan mediante *votación*, que es una forma de agregación. Para obtener más información, vea la entrada de Wikipedia para la agregación de Bootstrap.

    + **Replicar**: En la replicación, cada árbol se entrena con exactamente los mismos datos de entrada. La determinación de qué división predicado se utiliza para cada nodo de árbol permanece aleatoria, crear árboles distintos.

   

4. Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.

    + **Único parámetro**: Seleccione esta opción si sabe cómo desea configurar el modelo y proporcionan un conjunto de valores como argumentos.


5. **Número de árboles de decisión**: Escriba el número máximo de árboles de decisión que se pueden crear en el conjunto. Si crea más árboles de decisión, puede obtener una mejor cobertura, pero podría aumentar el tiempo de entrenamiento.

    Este valor también controla el número de árboles que se muestran en los resultados, cuando se visualice el modelo entrenado. Para ver o imprimir un único árbol, puede establecer el valor en 1; Sin embargo, esto significa que un único árbol puede ser generado (el árbol con el conjunto inicial de parámetros) y no se realizan las iteraciones adicionales.

6. **Profundidad máxima de los árboles de decisión**: Escriba un número para limitar la profundidad máxima de cualquier árbol de decisión. Al aumentar la profundidad del árbol podría aumentar la precisión, con un tiempo de entrenamiento de sobreajuste y aumente el riesgo.

7. **Número de divisiones aleatorias por nodo**: Escriba el número de divisiones que se usará al crear cada nodo del árbol. Un *dividir* significa que las características de cada nivel del árbol (nodo) se divide al azar.

8. **Número mínimo de muestras por nodo hoja**: Indicar el número mínimo de casos que son necesarios para crear cualquier nodo terminal (hoja) en un árbol. Al aumentar este valor, aumente el umbral para la creación de nuevas reglas.

    Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener al menos cinco casos que cumplen las condiciones.



10. Conecte un conjunto de datos con la etiqueta y uno de los módulos de entrenamiento:

    + Si establece **crear modo de entrenador** a **único parámetro**, utilice el [Train Model](./train-model.md) módulo.

11. Ejecute el experimento.

## <a name="results"></a>Results

Una vez completada la formación:

+ Para ver el árbol de la que se creó en cada iteración, haga clic en la salida de la [Train Model](./train-model.md) módulo y seleccione **visualizar**.
+ Para ver las reglas para cada nodo, haga clic en cada árbol para explorar en profundidad las divisiones.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 