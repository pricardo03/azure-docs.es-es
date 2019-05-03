---
title: 'Regresión logística de dos clases: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo regresión logística de dos clases en el servicio de Azure Machine Learning para crear un modelo de regresión logística que puede usarse para predecir los resultados de dos (y sólo dos).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: aacaf6c64ef77d0e694f97e3675060eca33794ed
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029256"
---
# <a name="two-class-logistic-regression-module"></a>Módulo de regresión logística de dos clases

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de regresión logística que puede usarse para predecir los resultados de dos (y sólo dos). 

Regresión logística es una técnica estadística conocida que se usa para el modelado de muchos tipos de problemas. Este algoritmo es un *aprendizaje supervisado* método;  por lo tanto, debe proporcionar un conjunto de datos que ya contiene los resultados para entrenar el modelo.  

### <a name="about-logistic-regression"></a>Acerca de la regresión logística  

Regresión logística es un método conocido en estadística que se usa para predecir la probabilidad de un resultado y es especialmente popular para las tareas de clasificación. El algoritmo predice la probabilidad de aparición de un evento ajustando los datos a una función logística.
  
En este módulo, el algoritmo de clasificación está optimizado para las variables dicotómicas o binarias. Si necesita clasificar varios resultados, use el [regresión logística Multiclase](./multiclass-logistic-regression.md) módulo.

##  <a name="how-to-configure"></a>Configuración  

Para entrenar este modelo, debe proporcionar un conjunto de datos que contiene una columna de etiqueta o una clase. Dado que este módulo está pensado para problemas de dos clases, la columna de etiqueta o una clase debe contener exactamente dos valores. 

Por ejemplo, la columna de etiqueta podría [votado] con los valores posibles de "Sí" o "No". O bien, podría ser [riesgo de crédito,] con los valores posibles de "Alta" o "Baja". 
  
1.  Agregar el **regresión logística de dos clases** módulo al experimento.  
  
2.  Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.  
  
    -   **Único parámetro**: Si sabe cómo desea configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos.  
  
3.  Para **tolerancia de optimización**, especifique un valor de umbral que se usará al optimizar el modelo. Si la mejora entre iteraciones cae por debajo del umbral especificado, se considera que el algoritmo convergieron en una solución, y deja de entrenamiento.  
  
4.  Para **peso de regularización L1** y **peso de regularización L2**, escriba un valor que se usará para los parámetros de regularización L1 y L2. Se recomienda un valor distinto de cero para ambos.  
  
     *La regularización* es un método para evitar el sobreajuste sancionar modelos con valores de coeficiente extreme. La regularización funciona agregando la penalización asociada con los valores de coeficiente al error de la hipótesis. Por lo tanto, un modelo preciso con valores de coeficiente de extremos se penalizaría más, pero un modelo menos preciso con valores más conservadores se penalizaría menos.  
  
     Regularización L1 y L2 tienen efectos diferentes y usa.  
  
    -   L1 puede aplicarse a modelos dispersos, lo que resulta útil cuando se trabaja con datos muy dimensionales.  
  
    -   En cambio, la regularización L2 es preferible para los datos que no están dispersas.  
  
     Este algoritmo es compatible con una combinación lineal de los valores de regularización L1 y L2: es decir, si <code>x = L1</code> y <code>y = L2</code>, a continuación, <code>ax + by = c</code> define el intervalo lineal de los términos de regularización.  
  
    > [!NOTE]
    >  ¿Desea obtener más información sobre la regularización L1 y L2? El siguiente artículo proporciona una explicación de cómo la regularización L1 y L2 son diferentes y cómo afectan a la conexión de modelo, con ejemplos de código para los modelos de red neuronal y regresión logística:  [Regularización L1 y L2 para el aprendizaje automático](https://msdn.microsoft.com/magazine/dn904675.aspx)  
    >
    > Se han diseñado para los modelos de regresión logística diferentes combinaciones de lineal de los términos de L1 y L2: por ejemplo, [elástica regularización neta](https://wikipedia.org/wiki/Elastic_net_regularization). Se recomienda que haga referencia a estas combinaciones para definir una combinación lineal que es efectiva en el modelo.
      
5.  Para **tamaño de memoria para L-BFGS**, especifique la cantidad de memoria que se usará para *L-BFGS* optimización.  
  
     L-BFGS significa "Broyden-Fletcher-Goldfarb-Shanno de memoria limitada". Es un algoritmo de optimización que es popular para la estimación de parámetro. Este parámetro indica el número de gradientes y posiciones anteriores para almacenar para el cálculo del siguiente paso.  
  
     Este parámetro de optimización limita la cantidad de memoria que se usa para calcular el siguiente paso y la dirección. Cuando especifica menos memoria, el entrenamiento es más rápido pero menos preciso.  
  
6.  Para **valor de inicialización aleatorio número**, escriba un valor entero. Definir un valor de inicialización es importante si desea que los resultados se puede reproducir a través de varias ejecuciones del mismo experimento.  
  
  
8. Agregue un conjunto de datos etiquetado para el experimento y conecte uno de los [módulos de formación](module-reference.md).  
  
    -   Si establece **crear modo de entrenador** a **único parámetro**, utilice el [Train Model](./train-model.md) módulo.  
  
9. Ejecute el experimento.  
  
## <a name="results"></a>Results

Una vez completada la formación:

+ Para ver un resumen de los parámetros del modelo, junto con los pesos de característica aprendidos de entrenamiento, haga clic en la salida de [Train Model](./train-model.md) y seleccione **visualizar**.   
  
+ Para realizar predicciones sobre nuevos datos, utilice el modelo entrenado y los nuevos datos como entrada para el [Score Model](./score-model.md) módulo. 


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 