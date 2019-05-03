---
title: 'Entrenar el modelo de clúster: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo entrenar modelo de agrupación en clústeres en el servicio de Azure Machine Learning para entrenar modelos de agrupación en clústeres.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 41cdec1d7f1c3932b17da6f9b1de518071f3f542
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028086"
---
# <a name="train-clustering-model"></a>Entrenar el modelo de agrupación en clústeres

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para entrenar un modelo de agrupación en clústeres.

El módulo toma un modelo de agrupación en clústeres no entrenado que ya ha configurado mediante el [agrupación en clústeres K-Means](k-means-clustering.md) módulo y se entrena el modelo utilizando un conjunto de datos sin etiqueta o con la etiqueta. El módulo crea tanto un modelo entrenado que puede usar para la predicción y un conjunto de asignaciones de clúster para cada caso de los datos de entrenamiento.

> [!NOTE]
> Ser una agrupación en clústeres modelo entrenado con el [Train Model](train-model.md) módulo, que es el módulo genérico para el entrenamiento de modelos de aprendizaje automático. Eso es porque [Train Model](train-model.md) solo funciona con algoritmos de aprendizaje supervisado. K-means y otros algoritmos de agrupación en clústeres permiten aprendizaje no supervisado, lo que significa que el algoritmo puede aprender de los datos sin etiqueta.  
  
## <a name="how-to-use-train-clustering-model"></a>Uso de entrenar el modelo de agrupación en clústeres  
  
1.  Agregar el **entrenar el modelo de agrupación en clústeres** módulo al experimento en Studio. Puede encontrar el módulo en **módulos de aprendizaje automático**, en el **Train** categoría.  
  
2. Agregar el [agrupación en clústeres K-Means](k-means-clustering.md) módulo o en otro módulo personalizado que crea una agrupación compatible del modelo y establezca los parámetros del modelo de agrupación en clústeres.  
    
3.  Asociar un conjunto de datos de entrenamiento a la entrada derecha de **entrenar el modelo de agrupación en clústeres**.
  
5.  En **conjunto de columnas**, seleccione las columnas del conjunto de datos desea utilizar para generar clústeres. Asegúrese de seleccionar las columnas que hacen que las características de buena: por ejemplo, evite utilizar identificadores u otras columnas que tienen valores únicos o columnas que tienen los mismos valores.

    Si hay una etiqueta, puede usarlo como una característica o dejarlo.  
  
6. Seleccione la opción **comprobar para anexar o desactive la casilla resultado solo**, si desea enviar los datos de entrenamiento junto con la nueva etiqueta de clúster.

    Si desactiva esta opción, solo las asignaciones de clúster son de salida. 

7. Ejecute el experimento, o haga clic en el **entrenar el modelo de agrupación en clústeres** módulo y seleccione **ejecutar seleccionado**.  
  
### <a name="results"></a>Results

Una vez completada la formación:


+  Para ver los valores del conjunto de datos, haga clic en el módulo, seleccione **como resultado conjuntos de datos**y haga clic en **visualizar**.

+ Para guardar el modelo entrenado para volver a usarlo más adelante, haga clic en el módulo, seleccione **calificado modelo**y haga clic en **Guardar como modelo entrenado**.

+ Para generar puntuaciones a partir del modelo, utilice [asignar datos a clústeres](assign-data-to-clusters.md).



## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 