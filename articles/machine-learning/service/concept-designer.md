---
title: Crear modelos de aprendizaje automático con el diseñador
titleSuffix: Azure Machine Learning
description: Más información sobre los términos, los conceptos y el flujo de trabajo que componen el diseñador para Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 11/04/2019
ms.openlocfilehash: ee97322e58fe7ab3a1474f55c6294822b8ce90da
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511833"
---
# <a name="what-is-azure-machine-learning-designer-preview"></a>¿Qué es el diseñador de Azure Machine Learning (versión preliminar)? 
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

El diseñador de Azure Machine Learning permite preparar los datos, formar, probar, implementar, administrar y realizar un seguimiento de los modelos de aprendizaje automático sin necesidad de escribir código.

No se requiere ningún tipo de programación, conecte visualmente [conjuntos de datos](#datasets) y [módulos](#module) para construir el modelo.

El diseñador utiliza el [área de trabajo](concept-workspace.md) de Azure Machine Learning para:

+ Cree, edite y ejecute [canalizaciones](#pipeline) en el área de trabajo.
+ Acceder a [conjuntos de datos](#datasets).
+ Use los [recursos de proceso](#compute) en el área de trabajo para ejecutar la canalización. 
+ Registrar [modelos](concept-azure-machine-learning-architecture.md#models).
+ [Publique](#publish) canalizaciones como puntos de conexión REST.
+ [Implemente](#deployment) modelos como puntos de conexión de canalización (para la inferencia por lotes) o puntos de conexión en tiempo real en recursos de proceso en el área de trabajo.

![Información general sobre el diseñador](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>Flujo de trabajo

El diseñador proporciona un lienzo visual interactivo para compilar, probar e iterar rápidamente en un modelo. 

+ Arrastre y coloque los [conjuntos de datos](#datasets) y [módulos](#module) en el lienzo.
+ Conecte los módulos juntos para formar una [canalización](#pipeline).
+ Ejecute la canalización mediante el recurso de proceso del área de trabajo de Machine Learning Service.
+ Itere en el diseño del modelo editando la canalización y ejecutándola de nuevo.
+ Cuando esté listo, convierta la **canalización de entrenamiento** en una **canalización de inferencia**.
+ [Publique](#publish) la canalización como un punto de conexión REST si desea volver a enviarla sin el código de Python que la construyó.
+ [Implemente](#deployment) la canalización de inferencia como punto de conexión de canalización o como punto de conexión en tiempo real para que otros usuarios puedan acceder al modelo.

## <a name="pipeline"></a>Canalización

Cree una [canalización](concept-azure-machine-learning-architecture.md#ml-pipelines) de Machine Learning desde cero o use una canalización de ejemplo existente como plantilla. Cada vez que ejecuta una canalización, los artefactos se almacenan en su área de trabajo. Las ejecuciones de canalización se agrupan en [experimentos](concept-azure-machine-learning-architecture.md#experiments).

Una canalización consta de conjuntos de datos y módulos analíticos, que se conectan entre ellos para construir un modelo. En concreto, una canalización válida tiene estas características:

* Los conjuntos de datos pueden estar solo se pueden conectar a módulos.
* Los módulos pueden conectarse a conjuntos de datos o a otros módulos.
* Todos los puertos de entrada de los módulos deben tener alguna conexión al flujo de datos.
* Deben establecerse todos los parámetros necesarios para cada módulo.


Para más información sobre cómo empezar a trabajar con el diseñador, consulte [Tutorial: Predecir el precio del automóvil con el diseñador](tutorial-designer-automobile-price-train-score.md).

## <a name="datasets"></a>Conjuntos de datos

Un conjuntos de datos de aprendizaje automático permite acceder a los datos y trabajar con ellos fácilmente. En el diseñador se incluyen varios conjuntos de valores de ejemplo para experimentar con ellos. Puede [registrar](./how-to-create-register-datasets.md) más conjuntos de datos a medida que los necesite.

## <a name="module"></a>Módulo

Un módulo es un algoritmo que puede aplicar sobre sus datos. El diseñador tiene una serie de módulos que van desde las funciones de entrada de datos hasta los procesos de entrenamiento, puntuación y validación.

Un módulo puede tener un conjunto de parámetros que puede utilizar para configurar los algoritmos internos del módulo. Al seleccionar un módulo en el lienzo, los parámetros del módulo se muestran en el panel Propiedades a la derecha del lienzo. Puede modificar los parámetros en ese panel para ajustar su modelo.

![Propiedades del módulo](media/ui-concept-visual-interface/properties.png)

Si desea ayuda para desplazarse por la biblioteca de algoritmos de aprendizaje automático disponibles, consulte [Información general sobre la referencia de módulos y algoritmos](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a>Recursos de proceso

Use recursos de proceso del área de trabajo para ejecutar la canalización y hospedar los modelos implementados como puntos de conexión en tiempo real o como puntos de conexión de canalización (para la inferencia por lotes). Los destinos de proceso admitidos son los siguientes:

| Destino de proceso | Cursos | Implementación |
| ---- |:----:|:----:|
| Proceso de Azure Machine Learning | ✓ | |
| Azure Kubernetes Service | | ✓ |

Los destinos de proceso están conectados a su [área de trabajo](concept-workspace.md) de Machine Learning. Los destinos de proceso se administran en el área de trabajo de [Azure Machine Learning Studio](https://ml.azure.com).

## <a name="publish"></a>Publicar

Una vez que tenga una canalización lista, puede publicarla como punto de conexión REST. Se puede enviar la canalización [PublishedPipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.publishedpipeline?view=azure-ml-py) sin el código Python que la construyó.

Además, la canalización PublishedPipeline se puede usar para volver a enviar una canalización con valores y entradas de PipelineParameter diferentes.

## <a name="deployment"></a>Implementación

Una vez que el modelo predictivo esté listo, impleméntelo como punto de conexión de la canalización o punto de conexión en tiempo real desde el diseñador.

El punto de conexión de canalización es un PublishedPipeline, que puede enviar una ejecución de canalización con distintos valores de PipelineParameter y entradas para la inferencia de lotes.

El punto de conexión en tiempo real proporciona una interfaz entre una aplicación y el modelo de puntuación. Una aplicación externa puede comunicarse con un modelo de puntuación en tiempo real. Una llamada a un punto de conexión en tiempo real devuelve los resultados de la predicción a una aplicación externa. Para realizar una llamada a un punto de conexión en tiempo real, es necesario pasar una clave de API que se creó al implementar el punto de conexión. El punto de conexión se basa en REST, una arquitectura muy usada para proyectos de programación web.

Para aprender a implementar el modelo, consulte [Tutorial: Implemente un modelo de aprendizaje automático con el diseñador](tutorial-designer-automobile-price-deploy.md).

## <a name="moving-from-the-visual-interface-to-the-designer"></a>Pasar de la interfaz visual al diseñador

La interfaz visual (versión preliminar) se ha actualizado y ahora es el diseñador Azure Machine Learning (versión preliminar). El diseñador se ha rediseñado para usar un back-end basado en canalización que se integra totalmente con las otras características de Azure Machine Learning. 

Como resultado de estas actualizaciones, algunos conceptos y términos de la interfaz visual se han cambiado o renombrado. Consulte la tabla siguiente para obtener los cambios conceptuales más importantes. 

| Concepto en el diseñador | Anteriormente en la interfaz visual |
| ---- |:----:|
| Borrador de canalización | Experimento |
| Punto de conexión en tiempo real | Servicio web |

### <a name="migrating-to-the-designer"></a>Migrar al diseñador

Puede convertir los experimentos de interfaz visual y los servicios Web existentes en canalizaciones y puntos de conexión en tiempo real en el diseñador. Siga estos pasos para migrar los recursos de la interfaz visual:

[!INCLUDE [migrate from the visual interface](../../../includes/aml-vi-designer-migration.md)]


## <a name="next-steps"></a>Pasos siguientes

* Aprenda los aspectos básicos del análisis predictivo y del aprendizaje automático con el [Tutorial: Predicción del precio del automóvil con el diseñador](tutorial-designer-automobile-price-train-score.md)
* Utilice uno de los ejemplos y modifíquelo cuanto necesite para adaptarlo a sus necesidades:

- [Ejemplo 1 - Regresión: predicción del precio de un automóvil](how-to-designer-sample-regression-automobile-price-basic.md)
- [Ejemplo 2 - Regresión: comparación de algoritmos para la predicción de precios de automóvil](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Ejemplo 3: clasificación con selección de características: Predicción de ingresos](how-to-designer-sample-classification-predict-income.md)
- [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Ejemplo 5 - Clasificación: predicción de la renovación](how-to-designer-sample-classification-churn.md)
- [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-designer-sample-classification-flight-delay.md)
- [Ejemplo 7 - Clasificación de texto: Conjunto de datos de SP 500 de Wikipedia](how-to-designer-sample-text-classification.md)

