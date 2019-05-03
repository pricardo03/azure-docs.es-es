---
title: 'Regresión de árbol de decisión ampliado: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de regresión de árbol de decisión impulsado en el servicio Azure Machine Learning para crear un conjunto de árboles de regresión mediante impulso.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: b4ebf1740ec2b0288d8052cb075a61b720b031a2
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028341"
---
# <a name="boosted-decision-tree-regression-module"></a>Módulo de regresión de árbol de decisión impulsado

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un conjunto de árboles de regresión mediante impulso. *Potenciación* significa que cada árbol depende de árboles anteriores. El algoritmo aprende ajustando la varianza residual de los árboles que le preceden. Por lo tanto, impulsar un conjunto de árboles de decisión tiende a mejorar la precisión con algún pequeño riesgo de menos cobertura.  
  
Este método de regresión es un método de aprendizaje supervisado y, por lo tanto, requiere una *conjunto de datos con la etiqueta*. La columna de etiqueta debe contener valores numéricos.  

> [!NOTE]
> Utilice este módulo solo con conjuntos de datos que utilizan variables numéricas.  

Después de haber definido el modelo, entrenar usando el [entrenar modelo](./train-model.md).

> [!TIP]
> ¿Desea obtener más información acerca de los árboles que se crearon? Una vez entrenado el modelo, haga clic en la salida de la [Train Model](./train-model.md) módulo y seleccione **visualizar** para ver el árbol que se creó en cada iteración. Puede explorar en profundidad las divisiones para cada árbol y ver las reglas para cada nodo.  
  
## <a name="more-about-boosted-regression-trees"></a>Más información acerca de los árboles de regresión impulsados  

Boosting es uno de los distintos métodos clásicos para crear modelos de conjunto, junto con bagging, bosques aleatorios y así sucesivamente.  En Azure Machine Learning, árboles de decisión impulsados utilizan una implementación eficaz de gradiente mart. algoritmo de impulso. Potenciación del gradiente es una técnica para los problemas de regresión de aprendizaje automático. Genera cada árbol de regresión en forma escalonada, usando una función de pérdida predefinida para medir el error en cada paso y corregirlo en el siguiente. Por lo tanto, el modelo de predicción es realmente un conjunto de modelos de predicción más débiles.  
  
En los problemas de regresión impulso genera una serie de árboles en forma escalonada y, a continuación, selecciona el árbol óptimo mediante una función de pérdida diferenciable arbitraria.  
  
Para obtener más información, consulte estos artículos:  
  
+ [https://wikipedia.org/wiki/Gradient_boosting#Gradient_tree_boosting](https://wikipedia.org/wiki/Gradient_boosting)

    Este artículo de Wikipedia sobre potenciación del gradiente proporciona cierta información general sobre los árboles impulsados. 
  
-  [http://research.microsoft.com/apps/pubs/default.aspx?id=132652](http://research.microsoft.com/apps/pubs/default.aspx?id=132652)  

    Microsoft Research: De RankNet a lambdarank y a LambdaMART: Información general. Por J.C. Burges.

También se pueden usar el método de potenciación de gradientes para problemas de clasificación reduciéndolos a regresión con una función de pérdida adecuada. Para obtener más información sobre la implementación de árboles impulsados para tareas de clasificación, vea [Two-Class Boosted Decision Tree](./two-class-boosted-decision-tree.md).  

## <a name="how-to-configure-boosted-decision-tree-regression"></a>Cómo configurar la regresión de árbol de decisión impulsado

1.  Agregar el **Boosted Decision Tree** módulo al experimento. Puede encontrar este módulo en **Machine Learning**, **inicializar**, en el **regresión** categoría. 
  
2.  Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.  
  
    -   **Único parámetro**: Seleccione esta opción si sabe cómo desea configurar el modelo y proporcionar un conjunto específico de valores como argumentos.  
   
  
3. **Número máximo de hojas por árbol**: Indicar el número máximo de nodos terminales (hojas) que se pueden crear en cualquier árbol.  

    Al aumentar este valor, podría aumentar el tamaño del árbol y obtener una mayor precisión, con el riesgo de sobreajuste y de mayor tiempo de entrenamiento.  

4. **Número mínimo de muestras por nodo hoja**: Indicar el número mínimo de casos necesarios para crear un nodo terminal (hoja) en un árbol.

    Al aumentar este valor, aumente el umbral para la creación de nuevas reglas. Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tendrían que contener al menos 5 casos que cumplen las condiciones.

5. **Velocidad de aprendizaje**: Escriba un número entre 0 y 1 que define el tamaño de paso durante el aprendizaje. La velocidad de aprendizaje determina la rapidez o lentitud el aprendiz converge en la solución óptima. Si el tamaño de paso es demasiado grande, podría pasar por alto la solución óptima. Si el tamaño de paso es demasiado pequeño, entrenamiento tarda más tiempo para converger la mejor solución.

6. **Número de árboles construidos**: Indica el número total de árboles de decisión para crear en el conjunto. Si crea más árboles de decisión, puede obtener una mejor cobertura, pero aumenta el tiempo de entrenamiento.

    Este valor también controla el número de árboles que se muestra cuando se visualice el modelo entrenado. Si desea ver o imprimir un árbol único, puede establecer el valor en 1; Sin embargo, se genera un único árbol (es decir, el árbol con el conjunto inicial de parámetros) y no se realizan las iteraciones adicionales.

7. **Valor de inicialización de números aleatorios**: Escriba un entero no negativo opcional que se usará como el valor de inicialización aleatorio. Especificar un valor de inicialización garantiza la reproducibilidad durante las ejecuciones que tienen los mismos datos y los parámetros.

    De forma predeterminada, el valor de inicialización aleatorio se establece en 0, lo que significa que el valor de inicialización inicial se obtiene del reloj del sistema.
  
8. **Permitir que los niveles de categorías desconocidos**: Seleccione esta opción para crear un grupo de valores desconocidos en los conjuntos de entrenamiento y validación. Si desactiva esta opción, el modelo podrá aceptar únicamente los valores que se encuentran en los datos de entrenamiento. El modelo podría ser menos preciso con valores conocidos, pero proporcione mejores predicciones para los valores nuevos (desconocidos).

9. Agregue un conjunto de datos de entrenamiento y uno de los módulos de entrenamiento:

    - Si establece **crear modo de entrenador** opción **único parámetro**, utilice el [Train Model](train-model.md) módulo.  
  
    

10. Ejecute el experimento.  
  
### <a name="results"></a>Results

Una vez completada la formación:

+ Para ver el árbol de la que se creó en cada iteración, haga clic en la salida de la [Train Model](train-model.md) módulo y seleccione **visualizar**.
  
     Haga clic en cada árbol para explorar en profundidad las divisiones y ver las reglas para cada nodo.  

+ Para usar el modelo para puntuar, conéctelo al [Score Model](./score-model.md), para predecir valores para nuevos ejemplos de entrada.

+ Para guardar una instantánea del modelo entrenado, haga clic en el **calificado modelo** salida del módulo de aprendizaje y seleccione **Guardar como**. No se actualiza la copia del modelo entrenado que se guarda en las ejecuciones sucesivas del experimento.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 