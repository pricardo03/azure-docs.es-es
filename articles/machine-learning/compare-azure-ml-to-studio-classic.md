---
title: Azure Machine Learning frente a ML Studio (clásico)
description: Diferencias entre Azure Machine Learning y ML Studio (clásico)
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 10/29/2019
ms.openlocfilehash: 00a2268442f94822064f86d95b14160d67e1516c
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74084755"
---
# <a name="how-azure-machine-learning-differs-from-ml-studio-classic"></a>Diferencias entre Azure Machine Learning y ML Studio (clásico)

En este artículo se comparan las características, las funcionalidades y la interfaz de Azure Machine Learning con ML Studio (clásico). 

## <a name="about-ml-studio-classic"></a>Acerca de ML Studio (clásico)
[ML Studio (clásico)](studio/what-is-ml-studio.md) es un área de trabajo visual colaborativa de arrastrar y colocar, en la que se pueden crear, probar e implementar soluciones de aprendizaje automático sin necesidad de escribir código. Utiliza algoritmos de aprendizaje automático y módulos de control de datos pregenerados y preconfigurados así como una plataforma de procesamiento propietario.

## <a name="about-azure-machine-learning"></a>Acerca de Azure Machine Learning

Mientras, [Azure Machine Learning](service/overview-what-is-azure-ml.md) proporciona una interfaz web denominada diseñador (versión preliminar) **y** varios SDK y CLI para preparar rápidamente los datos, entrenar e implementar los modelos de aprendizaje automático. Con Azure Machine Learning se obtiene escalado, compatibilidad con varias plataformas, características avanzadas de aprendizaje automático, como el aprendizaje automático automatizado y la compatibilidad con canalizaciones.

El diseñador de Azure Machine Learning proporciona una experiencia de arrastrar y colocar similar a Studio (clásico). Sin embargo, a diferencia de la plataforma de proceso propietaria de Studio (clásico), el diseñador utiliza sus propios recursos de proceso, es escalable y está totalmente integrado en Azure Machine Learning.  

> [!TIP]
> Se recomienda a los clientes que usen o evalúen Machine Learning Studio (clásico) que prueben el [diseñador de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/ui-concept-visual-interface) (versión preliminar), que proporciona módulos de Machine Learning de arrastrar y colocar, __además de__ escalabilidad, control de versiones y seguridad de la empresa.

## <a name="comparison-azure-machine-learning-vs-ml-studio-classic"></a>Comparación: Azure Machine Learning frente a ML Studio (clásico)

Esta es una comparación rápida.

||  Diseñador de Azure Machine Learning|Studio (clásico) |
|---| --- | --- |
||El diseñador está en versión preliminar, Azure Machine Learning está disponible con carácter general.|Disponible con carácter general | 
|Interfaz de arrastrar y colocar| Sí | Sí|
|Experimento| Escalar con destino de proceso|Escalar (límite de datos de entrenamiento de 10 GB) | 
|Módulos de la interfaz| [Muchos módulos populares](algorithm-module-reference/module-reference.md) | Muchos |
|Entrenamiento de destinos de proceso| Proceso de AML (GPU/CPU)|Destino de proceso propietario, solo CPU|
|Destinos de proceso de inferencia| Azure Kubernetes Service para la inferencia en tiempo real <br/>Proceso de AML para la inferencia de lotes|Formato del servicio web propietario, no personalizable | 
|Canalización de Machine Learning| Creación de canalizaciones <br/> Canalización publicada <br/> Punto de conexión de canalización <br/> [Más información sobre la canalización de Machine Learning](service/concept-ml-pipelines.md)|No compatible | 
|Operaciones de Machine Learning| Implementación configurable, modelo y control de versiones de canalizaciones|Implementación y administración básicas de modelos | 
|Modelo| Formato estándar, depende del trabajo de entrenamiento|Formato propietario y no portátil| 
|Entrenamiento de modelos automatizado|Todavía no se encuentra en el diseñador, pero es posible mediante la interfaz y los SDK.| Sin | 

## <a name="get-started-with-azure-machine-learning"></a>Introducción a Azure Machine Learning

Los siguientes recursos pueden ayudarle a empezar a trabajar con Azure Machine Learning.

- Lea la [introducción a Azure Machine Learning](service/tutorial-first-experiment-automated-ml.md). 

- [Cree su primera canalización de diseñador](service/tutorial-designer-automobile-price-train-score.md) para predecir precios automáticos.

![Ejemplo del diseñador de Azure Machine Learning](service/media/concept-ml-pipelines/designer-drag-and-drop.gif)

## <a name="next-steps"></a>Pasos siguientes

Además de las funcionalidades de arrastrar y colocar en el diseñador, Azure Machine Learning tiene otras herramientas disponibles:  
  + [Uso de cuadernos de Python para entrenar e implementar modelos de aprendizaje automático](./service/tutorial-1st-experiment-sdk-setup.md)
  + [Uso de R Markdown para entrenar e implementar modelos de aprendizaje automático](./service/tutorial-1st-r-experiment.md) 
  + [Uso del aprendizaje automático automatizado para entrenar e implementar modelos de ML](./service/tutorial-designer-automobile-price-train-score.md) 
  + [Uso de la CLI de Machine Learning para entrenar e implementar un modelo](./service/tutorial-train-deploy-model-cli.md)

