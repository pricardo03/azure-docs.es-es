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
ms.openlocfilehash: 1c2d2a02ecfb617551dd9174b87f363d57b151a8
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65467194"
---
# <a name="module-assign-data-to-clusters"></a>Módulo: Asignar datos a clústeres

En este artículo se describe cómo usar el *asignar datos a clústeres* módulo en la interfaz visual de Azure Machine Learning. El módulo genera predicciones mediante un modelo de agrupación en clústeres que se ha entrenado con el *agrupación en clústeres K-means* algoritmo.

Los datos de asignar al módulo de clústeres devuelve un conjunto de datos que contiene las asignaciones de probables para cada nuevo punto de datos. 


## <a name="how-to-use-assign-data-to-clusters"></a>Cómo usar datos de asignar a clústeres
  
1. En la interfaz visual de Azure Machine Learning, busque un modelo de agrupación en clústeres entrenado previamente. Puede crear y entrenar un modelo de agrupación en clústeres mediante cualquiera de los métodos siguientes:  
  
    - Configurar el algoritmo de agrupación en clústeres K-means utilizando el [agrupación en clústeres K-Means](k-means-clustering.md) módulo y entrenar el modelo mediante el uso de un conjunto de datos y el módulo entrenar modelo de agrupación en clústeres (en este artículo).  
  
    - También puede agregar un modelo de agrupación en clústeres entrenado existente desde el **guardar modelos** grupo en el área de trabajo.

2. Adjuntar el modelo entrenado para el puerto de entrada izquierdo **asignar datos a clústeres**.  

3. Adjunte un nuevo conjunto de datos como entrada. 

   En este conjunto de datos, las etiquetas son opcionales. Por lo general, la agrupación en clústeres es un método de aprendizaje no supervisado. No se espera que conoce de antemano las categorías. Sin embargo, las columnas de entrada deben ser igual que las columnas que se usan para el entrenamiento que se produce el modelo de agrupación en clústeres o un error.

    > [!TIP]
    > Para reducir el número de columnas que se escriben en la interfaz de las predicciones de clúster, use [seleccionar columnas del conjunto de datos](select-columns-in-dataset.md)y seleccionar un subconjunto de las columnas. 
    
4. Deje el **comprobar para anexar o desactive la casilla resultado solo** casilla activada si desea que los resultados que contengan el conjunto de datos entrada completo, incluida una columna que muestra los resultados (asignaciones de clúster).
  
    Si desactiva esta casilla, solo los resultados se devuelven. Esta opción puede ser útil al crear predicciones como parte de un servicio web.
  
5.  Ejecute el experimento.  
  
### <a name="results"></a>Resultados

+  Para ver los valores del conjunto de datos, haga clic en el módulo, seleccione **como resultado conjuntos de datos**y, a continuación, seleccione **visualizar**.

