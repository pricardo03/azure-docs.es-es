---
title: Interfaz visual
titleSuffix: Azure Machine Learning service
description: Obtenga información acerca de los términos, los conceptos y el flujo de trabajo que conforman la interfaz visual (versión preliminar) de Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/15/2019
ms.openlocfilehash: 70141e3ad438ff7ff6d4486e6c27ff4163416e95
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860407"
---
# <a name="what-is-the-visual-interface-for-azure-machine-learning-service"></a>¿Qué es la interfaz visual de Azure Machine Learning Service? 

La interfaz visual (versión preliminar) de servicio Azure Machine Learning Service permite preparar datos, entrenar, probar, implementar, administrar y realizar un seguimiento de los modelos de Machine Learning sin escribir código.

No se requiere ningún tipo de programación, conecte visualmente [conjuntos de datos](#dataset) y [módulos](#module) para construir el modelo.

La interfaz visual usa el [área de trabajo](concept-workspace.md) de Azure Machine Learning Service para:

+ Escribir artefactos de las ejecuciones de [experimentos](#experiment) en el área de trabajo.
+ Acceder a [conjuntos de datos](#dataset).
+ Use los [recursos de proceso](#compute) en el área de trabajo para ejecutar el experimento. 
+ Registrar [modelos](concept-azure-machine-learning-architecture.md#models).
+ [Implementar](#deployment) modelos como servicios web en los recursos de proceso del área de trabajo.

![Información general de la interfaz visual](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>Flujo de trabajo

La interfaz visual le ofrece un lienzo visual interactivo visual para compilar, probar y realizar iteraciones en un modelo rápidamente. 

+ Arrastre y coloque los [módulos](#module) en el lienzo.
+ Conecte los módulos juntos para formar un [experimento](#experiment).
+ Ejecute el experimento mediante el recurso de proceso del área de trabajo de Machine Learning Service.
+ Itere en el diseño del modelo editando el experimento y ejecutándolo de nuevo.
+ Cuando esté listo, convierta el **experimento de entrenamiento** en un **experimento predictivo**.
+ [Implemente](#deployment) el experimento predictivo como un servicio web para que otros usuarios puedan acceder al modelo.

## <a name="experiment"></a>Experimento

Cree un experimento desde cero o use un experimento de ejemplo existente como plantilla.  Cada vez que ejecuta un experimento, los artefactos se almacenan en su área de trabajo.

Un experimento consta de conjuntos de datos y módulos analíticos, que se conectan entre ellos para construir un modelo. En concreto, un experimento válido tiene estas características:

* Los conjuntos de datos pueden estar solo se pueden conectar a módulos.
* Los módulos pueden conectarse a conjuntos de datos o a otros módulos.
* Todos los puertos de entrada de los módulos deben tener alguna conexión al flujo de datos.
* Deben establecerse todos los parámetros necesarios para cada módulo.


Para obtener información sobre cómo empezar a usar la interfaz visual, consulte el [Tutorial: Predicción del precio de un automóvil con la interfaz visual](ui-tutorial-automobile-price-train-score.md).

## <a name="dataset"></a>Dataset

Un conjunto de datos son los datos que se han cargado en la interfaz visual para usarlos en el proceso de modelado. Se incluyen varios conjuntos de datos de ejemplo con los que puede experimentar y puede cargar más a medida que los necesite.

## <a name="module"></a>Módulo

Un módulo es un algoritmo que puede aplicar sobre sus datos. La interfaz visual cuenta con diversos módulos que van desde las funciones de incorporación de datos hasta procesos de entrenamiento, puntuación y validación.

Un módulo puede tener un conjunto de parámetros que puede utilizar para configurar los algoritmos internos del módulo. Al seleccionar un módulo en el lienzo, los parámetros del módulo se muestran en el panel Propiedades a la derecha del lienzo. Puede modificar los parámetros en ese panel para ajustar su modelo.

![Propiedades del módulo](media/ui-concept-visual-interface/properties.png)

Si desea ayuda para desplazarse por la biblioteca de algoritmos de aprendizaje automático disponibles, consulte [Información general sobre la referencia de módulos y algoritmos](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a>Recursos de proceso

Use los recursos de proceso del área de trabajo para ejecutar el experimento u hospedar los modelos implementados como servicios web. Los destinos de proceso admitidos son los siguientes:


| Destino de proceso | Cursos | Implementación |
| ---- |:----:|:----:|
| Proceso de Azure Machine Learning | ✓ | |
| Azure Kubernetes Service | | ✓ |

Los destinos de proceso están conectados a su [área de trabajo](concept-workspace.md) de Machine Learning. Los destinos de proceso del área de trabajo se administran en [Azure Portal](https://portal.azure.com) o en la [página de aterrizaje del área de trabajo (versión preliminar)](https://ml.azure.com).

## <a name="deployment"></a>Implementación

Cuando el modelo de análisis predictivo esté listo, impleméntelo como servicio web directamente desde la interfaz visual.

Los servicios web proporcionan una interfaz entre una aplicación y el modelo de puntuación. Una aplicación externa puede comunicarse con un modelo de puntuación en tiempo real. Una llamada a un servicio web devuelve los resultados de la predicción a una aplicación externa. Para realizar una llamada a un servicio web Machine Learning, es necesario pasar una clave de API que se creó al implementar el servicio web. El servicio web se basa en REST, una arquitectura muy usada para proyectos de programación web.

Para aprender a implementar el modelo, consulte [Tutorial: Implementación de un modelo de Machine Learning con la interfaz visual](ui-tutorial-automobile-price-deploy.md).

## <a name="next-steps"></a>Pasos siguientes

* Aprenda los aspectos básicos del análisis predictivo y del aprendizaje automático con el [Tutorial: Predicción del precio de un automóvil con la interfaz visual](ui-tutorial-automobile-price-train-score.md)
* Utilice uno de los ejemplos y modifíquelo cuanto necesite para adaptarlo a sus necesidades:
    * [Ejemplo 1 - Regresión: predicción del precio](ui-sample-regression-predict-automobile-price-basic.md)
    * [Ejemplo 2 - Regresión: predicción del precio y comparación de algoritmos](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
    * [Ejemplo 3 - Clasificación: predicción del riesgo crediticio](ui-sample-classification-predict-credit-risk-basic.md)
    * [Ejemplo 4 - Clasificación: predicción del riesgo crediticio (sensible a los costos)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
    * [Ejemplo 5 - Clasificación: predicción de la deserción de clientes, la apetencia y la venta vertical](ui-sample-classification-predict-churn.md)
