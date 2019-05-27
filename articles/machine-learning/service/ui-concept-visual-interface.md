---
title: Interfaz visual
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre los conceptos, términos y flujo de trabajo que componen la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/15/2019
ms.openlocfilehash: 0b158a1d713e0de8f3d1b2672aed442fce66e724
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65917166"
---
# <a name="what-is-the-visual-interface-for-azure-machine-learning-service"></a>¿Qué es la interfaz visual para el servicio Azure Machine Learning? 

La interfaz visual (versión preliminar) para el servicio Azure Machine Learning le permite preparar datos, entrenar, probar, implementar, administrar y realizar un seguimiento de los modelos de aprendizaje automático sin escribir código.

No se requiere ninguna programación, conectar visualmente [conjuntos de datos](#dataset) y [módulos](#module) para construir el modelo.

La interfaz visual utiliza el servicio Azure Machine Learning [área de trabajo](concept-workspace.md) para:

+ Escribir los artefactos de [experimentar](#experiment) se ejecuta en el área de trabajo.
+ Acceso [conjuntos de datos](#dataset).
+ Use la [recursos de proceso](#compute) en el área de trabajo para ejecutar el experimento. 
+ Registrar [modelos](concept-azure-machine-learning-architecture.md#model).
+ [Implementar](#deployment) modelos como servicios web en recursos de proceso en el área de trabajo.

![Información general de la interfaz visual](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>Flujo de trabajo

La interfaz visual le ofrece un lienzo interactivo, visual para compilar, probar e iterar en un modelo rápidamente. 

+ Puede arrastrar y colocar [módulos](#module) al lienzo.
+ Conectar los módulos juntos para formar un [experimentar](#experiment).
+ Ejecute el experimento con el recurso de proceso del área de trabajo del servicio Machine Learning.
+ Recorrer en iteración en el modelo de diseño, edite el experimento y ejecutarlo de nuevo.
+ Cuando esté listo, convertir su **experimento de entrenamiento** a un **experimento predictivo**.
+ [Implementar](#deployment) el experimento predictivo como un sitio web de servicio para que el modelo puede tener acceso a otros usuarios.

## <a name="experiment"></a>Experimento

Crear un experimento desde cero, o usar un experimento de ejemplo existente como plantilla.  Cada vez que ejecute un experimento, los artefactos se almacenan en el área de trabajo.

Un experimento consta de conjuntos de datos y módulos analíticos, que se conectan en conjunto para construir un modelo. En concreto, un experimento válido tiene estas características:

* Los conjuntos de datos pueden estar conectado solo a los módulos.
* Los módulos pueden conectarse a conjuntos de datos o de otros módulos.
* Todos los puertos de entrada para los módulos deben tener alguna conexión al flujo de datos.
* Todos los requeridos se deben establecer parámetros para cada módulo.

Para obtener un ejemplo de un experimento simple, vea [inicio rápido: Preparar y visualizar los datos sin escribir código en Azure Machine Learning](ui-quickstart-run-experiment.md).

Para obtener un tutorial más completo de una solución de análisis predictivo, consulte [Tutorial: Predecir los precios de automóviles con la interfaz visual](ui-tutorial-automobile-price-train-score.md).

## <a name="dataset"></a>Dataset

Un conjunto de datos es datos que se ha cargado en la interfaz visual para usar en el proceso de modelado. Se incluye para que pueda experimentar con un número de conjuntos de datos de ejemplo y puede cargar varios conjuntos de datos según sea necesario.

## <a name="module"></a>Módulo

Un módulo es un algoritmo que puede aplicar sobre sus datos. La interfaz visual tiene un número de módulos que van desde las funciones de entrada de datos para entrenamiento, puntuación y los procesos de validación.

Un módulo puede tener un conjunto de parámetros que puede utilizar para configurar los algoritmos internos del módulo. Cuando se selecciona un módulo en el lienzo, se muestran los parámetros del módulo en el panel de propiedades a la derecha del lienzo. Puede modificar los parámetros en ese panel para ajustar su modelo.

![Propiedades del módulo](media/ui-concept-visual-interface/properties.png)

Para algunos ayuda navegar por la biblioteca de algoritmos de aprendizaje automático disponibles, consulte [información general de referencia de módulos y algoritmos](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a> Recursos de proceso

Use los recursos del área de trabajo para ejecutar el experimento o host de los modelos implementados como servicios web de proceso. Los destinos de proceso admitidos son los siguientes:


| Destino de proceso | Aprendizaje | Implementación |
| ---- |:----:|:----:|
| Proceso de Azure Machine Learning | ✓ | |
| Azure Kubernetes Service | | ✓ |

Destinos están conectados a su aprendizaje automático de proceso [área de trabajo](concept-workspace.md). Administrar los destinos de proceso en el área de trabajo en el [portal Azure](https://portal.azure.com).

## <a name="deployment"></a>Implementación

Una vez que el modelo de análisis predictivo esté listo, implementarlo como un servicio web directamente desde la interfaz visual.

Los servicios web proporcionan una interfaz entre una aplicación y el modelo de puntuación. Una aplicación externa puede comunicarse con el modelo de puntuación en tiempo real. Una llamada a un servicio web devuelve los resultados de predicción a una aplicación externa. Para realizar una llamada a un servicio web Machine Learning, es necesario pasar una clave de API que se creó al implementar el servicio web. El servicio web se basa en REST, una opción popular de arquitectura para proyectos de programación web.

Para obtener información sobre cómo implementar el modelo, vea [Tutorial: Implementar un modelo de aprendizaje automático con la interfaz visual](ui-tutorial-automobile-price-deploy.md).

## <a name="next-steps"></a>Pasos siguientes

* Conozca los aspectos básicos de análisis predictivo y machine learning con [inicio rápido: Preparar y visualizar los datos sin escribir código en Azure Machine Learning](ui-quickstart-run-experiment.md).
* Utilice uno de los ejemplos y modificar al conjunto de sus necesidades:
    * [Ejemplo 1: regresión: Predecir el precio](ui-sample-regression-predict-automobile-price-basic.md)
    * [Ejemplo 2: regresión: Predecir el precio y compare de algoritmos](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
    * [Ejemplo 3: clasificación: Predecir el riesgo de crédito](ui-sample-classification-predict-credit-risk-basic.md)
    * [Ejemplo 4: clasificación: Predecir el riesgo de crédito (costo confidencial)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
    * [Ejemplo 5: clasificación: Predecir el abandono, apetencia y venta vertical](ui-sample-classification-predict-churn.md)
