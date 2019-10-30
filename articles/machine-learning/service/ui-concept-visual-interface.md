---
title: Interfaz visual
titleSuffix: Azure Machine Learning
description: Obtenga información acerca de los términos, los conceptos y el flujo de trabajo que conforman la interfaz visual (versión preliminar) de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.date: 9/23/2019
ms.openlocfilehash: a23f123c6ffadaad4f830e1f4eab3820e6ef56f6
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692219"
---
# <a name="what-is-the-visual-interface-for-azure-machine-learning"></a>¿Qué es la interfaz visual de Azure Machine Learning? 

La interfaz visual (versión preliminar) de Azure Machine Learning permite preparar datos, entrenar, probar, implementar, administrar y realizar un seguimiento de los modelos de Machine Learning sin escribir código.

No se requiere ningún tipo de programación, conecte visualmente [conjuntos de datos](#datasets) y [módulos](#module) para construir el modelo.

La interfaz visual usa el [área de trabajo](concept-workspace.md) de Azure Machine Learning para:

+ Cree, edite y ejecute [canalizaciones](#pipeline) en el área de trabajo.
+ Acceder a [conjuntos de datos](#datasets).
+ Use los [recursos de proceso](#compute) en el área de trabajo para ejecutar la canalización. 
+ Registrar [modelos](concept-azure-machine-learning-architecture.md#models).
+ [Publique](#publish) canalizaciones como puntos de conexión REST.
+ [Implemente](#deployment) modelos como puntos de conexión de canalización (para la inferencia por lotes) o puntos de conexión en tiempo real en recursos de proceso en el área de trabajo.

![Información general de la interfaz visual](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>Flujo de trabajo

La interfaz visual le ofrece un lienzo visual interactivo visual para compilar, probar y realizar iteraciones en un modelo rápidamente. 

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


Para obtener información sobre cómo empezar a usar la interfaz visual, consulte el [Tutorial: Predicción del precio de un automóvil con la interfaz visual](ui-tutorial-automobile-price-train-score.md).

## <a name="datasets"></a>Conjuntos de datos

Un conjuntos de datos de aprendizaje automático permite acceder a los datos y trabajar con ellos fácilmente. Se incluyen varios conjuntos de datos de ejemplo en la interfaz visual para que experimente con ellos. Puede [registrar](./how-to-create-register-datasets.md) más conjuntos de datos a medida que los necesite.

## <a name="module"></a>Módulo

Un módulo es un algoritmo que puede aplicar sobre sus datos. La interfaz visual cuenta con diversos módulos que van desde las funciones de incorporación de datos hasta procesos de entrenamiento, puntuación y validación.

Un módulo puede tener un conjunto de parámetros que puede utilizar para configurar los algoritmos internos del módulo. Al seleccionar un módulo en el lienzo, los parámetros del módulo se muestran en el panel Propiedades a la derecha del lienzo. Puede modificar los parámetros en ese panel para ajustar su modelo.

![Propiedades del módulo](media/ui-concept-visual-interface/properties.png)

Si desea ayuda para desplazarse por la biblioteca de algoritmos de aprendizaje automático disponibles, consulte [Información general sobre la referencia de módulos y algoritmos](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a>Recursos de proceso

Use recursos de proceso del área de trabajo para ejecutar la canalización y hospedar los modelos implementados como puntos de conexión en tiempo real o como puntos de conexión de canalización (para la inferencia por lotes). Los destinos de proceso admitidos son los siguientes:

| Destino de proceso | Cursos | Implementación |
| ---- |:----:|:----:|
| Proceso de Azure Machine Learning | ✓ | |
| Azure Kubernetes Service | | ✓ |

Los destinos de proceso están conectados a su [área de trabajo](concept-workspace.md) de Machine Learning. Los destinos de proceso del área de trabajo se administran en [Azure Portal](https://portal.azure.com) o en la [página de aterrizaje del área de trabajo (versión preliminar)](https://ml.azure.com).

## <a name="publish"></a>Publicar

Una vez que tenga una canalización lista, puede publicarla como punto de conexión REST. Se puede enviar la canalización [PublishedPipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.publishedpipeline?view=azure-ml-py) sin el código Python que la construyó.

Además, la canalización PublishedPipeline se puede usar para volver a enviar una canalización con valores y entradas de PipelineParameter diferentes.

## <a name="deployment"></a>Implementación

Una vez que el modelo predictivo esté listo, impleméntelo como punto de conexión de canalización o como punto de conexión en tiempo real desde la interfaz visual.

El punto de conexión de canalización es una canalización PublishedPipeline, que permite enviar una ejecución de canalización con valores y entradas de PipelineParameter diferentes para la inferencia por lotes.

El punto de conexión en tiempo real proporciona una interfaz entre una aplicación y el modelo de puntuación. Una aplicación externa puede comunicarse con un modelo de puntuación en tiempo real. Una llamada a un punto de conexión en tiempo real devuelve los resultados de la predicción a una aplicación externa. Para realizar una llamada a un punto de conexión en tiempo real, es necesario pasar una clave de API que se creó al implementar el punto de conexión. El punto de conexión se basa en REST, una arquitectura muy usada para proyectos de programación web.

Para aprender a implementar el modelo, consulte [Tutorial: Implementación de un modelo de Machine Learning con la interfaz visual](ui-tutorial-automobile-price-deploy.md).

## <a name="next-steps"></a>Pasos siguientes

* Aprenda los aspectos básicos del análisis predictivo y del aprendizaje automático con el [Tutorial: Predicción del precio de un automóvil con la interfaz visual](ui-tutorial-automobile-price-train-score.md)
* Utilice uno de los ejemplos y modifíquelo cuanto necesite para adaptarlo a sus necesidades:

    * [Ejemplo 1 - Regresión: predicción del precio](how-to-ui-sample-regression-predict-automobile-price-basic.md)
    * [Ejemplo 2 - Regresión: predicción del precio y comparación de algoritmos](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
    * [Ejemplo 3 - Clasificación: predicción del riesgo crediticio](how-to-ui-sample-classification-predict-credit-risk-basic.md)
    * [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
    * [Ejemplo 5 - Clasificación: predicción de la deserción de clientes, la apetencia y la venta vertical](how-to-ui-sample-classification-predict-churn.md)
    * [Ejemplo 6 - Clasificación: Predicción de retrasos en los vuelos](how-to-ui-sample-classification-predict-flight-delay.md)

