---
title: 'Asignar datos al clúster: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar los datos de asignar al módulo de clúster en el servicio de Azure Machine Learning para puntuar el modelo de agrupación en clústeres.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: b370c6ef5be311ed9c8df2737fa1b01144d61011
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028776"
---
# <a name="assign-data-to-clusters"></a>Asignar datos a clústeres

En este artículo se describe cómo usar el [asignar datos a clústeres](assign-data-to-clusters.md) módulo en la interfaz visual, para generar predicciones mediante un modelo de agrupación en clústeres que se ha entrenado mediante el algoritmo de agrupación en clústeres K-Means.

El módulo devuelve un conjunto de datos que contiene las asignaciones de probables para cada nuevo punto de datos. 


## <a name="how-to-use-assign-data-to-clusters"></a>Cómo usar datos de asignar a clústeres
  
1.  En la interfaz visual, busque un modelo de agrupación en clústeres entrenado previamente. Puede crear y entrenar un modelo de agrupación en clústeres mediante cualquiera de estos métodos:  
  
    - Configurar el algoritmo K-means utilizando el [agrupación en clústeres K-Means](k-means-clustering.md) módulo y, a continuación, entrenar el modelo mediante un conjunto de datos y el [entrenar el modelo de agrupación en clústeres](train-clustering-model.md) módulo.  
  
  
    También puede agregar un modelo de agrupación en clústeres entrenado existente desde el **guardar modelos** grupo en el área de trabajo.

2. Adjuntar el modelo entrenado para el puerto de entrada izquierdo [asignar datos a clústeres](assign-data-to-clusters.md).  

3. Adjunte un nuevo conjunto de datos como entrada. En este conjunto de datos, las etiquetas son opcionales. Por lo general, la agrupación en clústeres es un método de aprendizaje no supervisado por lo que no se espera que se conoce de antemano las categorías.

    Sin embargo, las columnas de entrada deben ser igual que las columnas que se usan para el entrenamiento que se produce el modelo de agrupación en clústeres o un error.

    > [!TIP]
    > Para reducir el número de columnas de salida de predicciones de clúster, use [Select Columns in Dataset](select-columns-in-dataset.md)y seleccionar un subconjunto de las columnas. 
    
4. Deje la opción **comprobar para anexar o desactive la casilla resultado solo** activada si desea que los resultados que contengan el conjunto de datos entrada completo, junto con una columna que indica los resultados (asignaciones de clúster).
  
    Si desactiva esta opción, obtendrá los resultados. Esto podría ser útil al crear predicciones como parte de un servicio web.
  
5.  Ejecute el experimento.  
  
### <a name="results"></a>Results

 
+  Para ver los valores del conjunto de datos, haga clic en el módulo, seleccione **como resultado conjuntos de datos**y haga clic en **visualizar**.

