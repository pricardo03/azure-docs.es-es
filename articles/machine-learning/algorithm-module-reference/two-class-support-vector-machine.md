---
title: 'Máquina de vectores de soporte de dos clases: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Aprenda a usar el **Two-Class Support Vector Machine** módulo en el servicio de Azure Machine Learning para crear un modelo que se basa en el algoritmo de máquina de vectores de soporte técnico.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 2f076dd3a5b1ceb9e24548652a71fda5b9aa48b7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027936"
---
# <a name="two-class-support-vector-machine-module"></a>Módulo Two-Class Support Vector Machine

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo que se basa en el algoritmo de máquina de vectores de soporte técnico. 

Las máquinas de vectores de soporte (SVM) son una clase contrastada de métodos de aprendizaje supervisado. En esta implementación es adecuada para la predicción de dos resultados posibles, en función de las variables continuas o de categorías.

Después de definir los parámetros del modelo, entrenar el modelo utilizando los módulos de entrenamiento, y proporcionando un *etiquetados dataset* que incluye una columna de etiqueta o resultado.

## <a name="about-support-vector-machines"></a>Acerca de las máquinas de vectores de soporte técnico

Las máquinas de vectores de soporte técnico son entre los primeros algoritmos de aprendizaje automático y modelos SVM se han usado en muchas aplicaciones, desde la recuperación de información para la clasificación de texto e imagen. SVM pueden usarse para tareas de clasificación y regresión.

Este modelo SVM es un modelo de aprendizaje supervisado que requiere datos etiquetados. En el proceso de entrenamiento, el algoritmo analiza los datos de entrada y reconoce patrones en un espacio de características multidimensionales llama el *hiperplano*.  Todos los ejemplos de entrada se representan como puntos en este espacio y están asignados a las categorías de tal manera que las categorías se dividen por de ancho y borrar un espacio como sea posible de resultados.

Para la predicción, el algoritmo SVM asigna nuevos ejemplos a una categoría o el otro, asignándolos a ese mismo espacio. 

## <a name="how-to-configure"></a>Configuración 

Para este tipo de modelo, se recomienda normalizar el conjunto de datos antes de usarlo para entrenar al clasificador.
  
1.  Agregar el **Two-Class Support Vector Machine** módulo al experimento.  
  
2.  Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.  
  
    -   **Único parámetro**: Si sabe cómo desea configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos.  

3.  Para **número de iteraciones**, escriba un número que indica el número de iteraciones usadas al generar el modelo.  
  
     Este parámetro puede usarse para controlar el equilibrio entre velocidad de aprendizaje y la precisión.  
  
4.  Para **Lambda**, escriba un valor que se usará como el peso de regularización L1.  
  
     Este coeficiente de regularización puede utilizarse para optimizar el modelo. Los valores mayores penalizan modelos más complejos.  
  
5.  Seleccione la opción **normalizar características**, si va a normalizar características antes del entrenamiento.
  
     Si aplica la normalización, antes del entrenamiento, los puntos de datos son centrados en la media y escala para tener una unidad de desviación estándar.
  
6.  Seleccione la opción **proyecto a la esfera unitaria**para normalizar los coeficientes.
  
     Proyectar valores al espacio de unidad significa que, antes del entrenamiento, los puntos de datos son centrados en 0 y escala para tener una unidad de desviación estándar.
  
7.  En **valor de inicialización aleatorio número**, escriba un valor entero para usarlo como un valor de inicialización si desea garantizar la reproducibilidad entre ejecuciones.  En caso contrario, se usa un valor de reloj del sistema como un valor de inicialización, que puede dar lugar a resultados ligeramente diferentes entre ejecuciones.
  
9. Conectar un conjunto de datos con la etiqueta y uno de los [módulos de formación](module-reference.md):
  
    -   Si establece **crear modo de entrenador** a **único parámetro**, utilice el [Train Model](train-model.md) módulo.
  

10. Ejecute el experimento.

## <a name="results"></a>Results

Una vez completada la formación:

+ Para ver un resumen de los parámetros del modelo, junto con los pesos de característica aprendidos de entrenamiento, haga clic en la salida de [Train Model](./train-model.md)y seleccione **visualizar**.

+ Para utilizar los modelos entrenados para realizar predicciones, conecte el modelo entrenado para la [Score Model](score-model.md) módulo.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 