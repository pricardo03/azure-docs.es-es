---
title: 'Asignación de datos a clústeres: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Asignación de datos a clústeres en Azure Machine Learning Service para puntuar un modelo de agrupación en clústeres.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ms.openlocfilehash: 56f608044716ea3655576c11aa7a62343215f508
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128986"
---
# <a name="module-assign-data-to-clusters"></a>Módulo: Asignación de datos a clústeres

En este artículo se describe cómo usar el módulo *Asignación de datos a clústeres* en la interfaz visual de Azure Machine Learning. El módulo genera predicciones mediante un modelo de agrupación en clústeres que se ha entrenado con el algoritmo de *agrupación en clústeres K-means*.

El módulo Asignación de datos a clústeres devuelve un conjunto de datos que contiene las asignaciones probables para cada punto de datos nuevo. 


## <a name="how-to-use-assign-data-to-clusters"></a>Cómo utilizar Asignación de datos a clústeres
  
1. En la interfaz visual de Azure Machine Learning, busque un modelo de agrupación en clústeres entrenado previamente. Puede crear y entrenar un modelo de agrupación en clústeres mediante cualquiera de los métodos siguientes:  
  
    - Configure el algoritmo de agrupación en clústeres K-means mediante el módulo [Agrupación en clústeres K-Means](k-means-clustering.md) y, para entrenar el modelo, utilice un conjunto de datos y el módulo Entrenar el modelo de agrupación en clústeres (en este artículo).  
  
    - También puede agregar un modelo de agrupación en clústeres entrenado existente desde el grupo **Modelos guardados** en el área de trabajo.

2. Adjunte el modelo entrenado al puerto de entrada izquierdo de **Asignación de datos a clústeres**.  

3. Adjunte un conjunto de datos nuevo como entrada. 

   En este conjunto de datos, las etiquetas son opcionales. Por lo general, la agrupación en clústeres es un método de aprendizaje no supervisado. No se espera que conozca de antemano las categorías. Sin embargo, las columnas de entrada deben ser iguales que las columnas que se usan para entrenar el modelo de agrupación en clústeres; de lo contrario, se produce un error.

    > [!TIP]
    > Para reducir el número de columnas que se escriben en la interfaz desde las predicciones de clúster, use [Seleccionar columnas del conjunto de datos](select-columns-in-dataset.md) y seleccione un subconjunto de columnas. 
    
4. Deje la casilla **Marcar para anexar o desmarcar solo para obtener el resultado** activada si quiere que los resultados contengan el conjunto de datos de entrada completo, incluida una columna en que se muestran los resultados (asignaciones de clúster).
  
    Si desactiva esta casilla, solo se devuelven los resultados. Esta opción puede ser útil al crear predicciones como parte de un servicio web.
  
5.  Ejecute el experimento.  
  
### <a name="results"></a>Results

+  Para ver los valores del conjunto de datos, haga clic con el botón derecho en el módulo, seleccione **Result datasets** (Conjuntos de datos del resultado) y, después, **Visualizar**.

