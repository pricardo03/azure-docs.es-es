---
title: 'Normalizar datos: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Normalize Data en el servicio de Azure Machine Learning para transformar un conjunto de datos a través de *normalización*...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 95069bafa94770511c7ee40e82068960298fd6c5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029451"
---
# <a name="normalize-data-module"></a>Normalizar el módulo de datos

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para transformar un conjunto de datos a través de *normalización*.

La normalización es una técnica que se aplican a menudo como parte de la preparación de datos para el aprendizaje automático. El objetivo de la normalización es cambiar los valores de las columnas numéricas del conjunto de datos para usar una escala común, sin eliminar las diferencias en los intervalos de valores o perder información. Normalización también es necesaria para algunos algoritmos modelar los datos correctamente.

Por ejemplo, suponga que el conjunto de datos de entrada contiene una columna con valores comprendidos entre 0 y 1 y otra columna con valores comprendidos entre 10 000 y 100 000. La gran diferencia en el *escala* de los números podría producir problemas al intentar combinar los valores como características durante el modelado.

*Normalización* evita estos problemas mediante la creación de nuevos valores que mantienen la distribución general y relaciones en los datos de origen, conservando los valores dentro de una escala que se aplican en todas las columnas numéricas que se usan en el modelo.

Este módulo ofrece varias opciones para transformar los datos numéricos:

- Puede cambiar todos los valores a 0-1 escalar o transformar los valores que se representan como percentil clasifica en lugar de valores absolutos.
- Puede aplicar la normalización a una sola columna o a varias columnas en el mismo conjunto de datos.
- Si tiene que repetir el experimento, o se aplican los mismos pasos de normalización a otros datos, puede guardar los pasos como una transformación de normalización y aplicarla a otros conjuntos de datos que tienen el mismo esquema.

> [!WARNING]
> Algunos algoritmos requieren que se normalicen los datos antes de entrenar un modelo. Otros algoritmos realizan sus propios datos escalado o normalización. Por lo tanto, al elegir un algoritmo que se utiliza en la creación de un modelo predictivo de aprendizaje automático, asegúrese de revisar los requisitos de datos del algoritmo antes de aplicar la normalización a los datos de entrenamiento.

##  <a name="configure-normalize-data"></a>Configurar Normalizar datos

Puede aplicar solo un método de normalización a la vez con este módulo. Por lo tanto, el mismo método de normalización se aplica a todas las columnas que seleccione. Para usar los métodos de normalización diferente, use una segunda instancia de **Normalize Data**.

1. Agregar el **Normalize Data** módulo al experimento. Puede encontrar el módulo de Azure Machine Learning en **transformación de datos**, en el **escalar y reducir** categoría.

2. Conectar un conjunto de datos que contiene al menos una columna de todos los números.

3. Utilice el Selector de columnas para elegir las columnas numéricas para normalizar. Si no elige las columnas individuales, de forma predeterminada **todas** se incluyen las columnas de tipo numérico en la entrada y el mismo proceso de normalización se aplica a todas las columnas seleccionadas. 

    Esto puede conducir a resultados extraños si incluye las columnas numéricas que no deberían normalizarse! Compruebe siempre las columnas con cuidado.

    Si no hay columnas numéricas se detectan, compruebe los metadatos de columna para comprobar que el tipo de datos de la columna es un tipo numérico admitido.

    > [!TIP]
    > Para asegurarse de que las columnas de un tipo específico se proporcionan como entrada, pruebe a usar el [Select Columns in Dataset](./select-columns-in-dataset.md) módulo antes de **Normalize Data**.

4. **Use 0 para las columnas constantes cuando está activada**:  Seleccione esta opción cuando cualquier columna numérica contiene un único valor que no cambian. Esto garantiza que estas columnas no se usan en las operaciones de normalización.

5. Desde el **método de transformación** lista desplegable, elija una sola función matemática que se aplicará a todas las columnas seleccionadas. 
  
    - **Zscore**: Convierte todos los valores en una puntuación z.
    
      Los valores de la columna se transforman mediante la fórmula siguiente:  
  
      ![normalización mediante z&#45;puntuaciones](media/module/aml-normalization-z-score.png)
  
      Desviación media y estándar se calculan por separado para cada columna. Se usa la desviación estándar de población.
  
    - **MinMax**: El normalizador mínimo-máximo cambia la escala linealmente todas las características del intervalo [0,1].
    
      Cambiar la escala del intervalo [0,1] se realiza cambiando los valores de cada característica para que el valor mínimo es 0, y, a continuación, al dividir por el nuevo valor máximo (que es la diferencia entre los valores máximos y mínimos originales).
      
      Los valores de la columna se transforman mediante la fórmula siguiente:  
  
      ![normalización mediante el número mínimo de&#45;max función](media/module/aml-normalization-minmax.png "AML_normalization minmax")  
  
    - **Logística**: Los valores de la columna se transforman mediante la fórmula siguiente:

      ![fórmula de normalización por logística función](media/module/aml-normalization-logistic.png "AML_normalization logística")  
  
    - **LogNormal**: Esta opción convierte todos los valores en una escala logarítmico-normal.
  
      Los valores de la columna se transforman mediante la fórmula siguiente:
  
      ![registro fórmulas&#45;distribución normal](media/module/aml-normalization-lognormal.png "AML_normalization logarítmico-normal")
    
      Aquí μ y σ son los parámetros de la distribución, calculada empíricamente a partir de los datos como estimaciones de verosimilitud máxima para cada columna por separado.  
  
    - **TanH**: Todos los valores se convierten en una tangente hiperbólica.
    
      Los valores de la columna se transforman mediante la fórmula siguiente:
    
      ![normalización mediante la función tanh](media/module/aml-normalization-tanh.png "AML_normalization tanh")

6. Ejecute el experimento, o haga doble clic en el **Normalize Data** módulo y seleccione **ejecutar seleccionado**. 

## <a name="results"></a>Results

El **Normalize Data** módulo genera dos salidas:

- Para ver los valores transformados, haga clic en el módulo, seleccione **transforma el conjunto de datos**y haga clic en **visualizar**.

    De forma predeterminada, los valores se transforman en su lugar. Si desea comparar los valores transformados a los valores originales, utilice el [agregar columnas](./add-columns.md) módulo para combinar los conjuntos de datos y ver las columnas en paralelo.

- Para guardar la transformación para que pueda aplicar el mismo método de normalización para otro conjunto de datos similar, haga clic en el módulo, seleccione **función de transformación**y haga clic en **Guardar como transformación**.

    A continuación, puede cargar las transformaciones guardadas desde el **transforma** grupo del panel de navegación izquierdo y aplicarlo a un conjunto de datos con el mismo esquema mediante el uso de [. / Aplicar transformación](apply-transformation.md).  


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 