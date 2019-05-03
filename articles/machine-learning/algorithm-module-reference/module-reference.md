---
title: Referencia de módulos y algoritmos
titleSuffix: Azure Machine Learning service
description: Obtenga información acerca de los módulos disponibles en la interfaz visual de Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 6602eb4bacdc3b6382c1b6873a465cdfc0632693
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029346"
---
# <a name="algorithm--module-reference-overview"></a>Información general de referencia de módulos y algoritmos

Este contenido de referencia proporciona la experiencia técnica en cada uno de los módulos disponibles en la interfaz visual (versión preliminar) del servicio de Azure Machine Learning y algoritmos de aprendizaje automático. 

Cada módulo representa un conjunto de código que puede ejecutarse de forma independiente y realizar un machine learning tarea, dadas las entradas necesarias. Un módulo podría contener un algoritmo determinado o realizar una tarea que es importante en machine learning, como reemplazo de valor que falta o realizar análisis estadísticos. 

> [!TIP]
> En todos los experimentos en la interfaz visual, puede obtener información acerca de un módulo específico. Seleccione el módulo y luego seleccione el **más ayuda** vincular en el **ayuda rápida** panel.

Los módulos se organizan por funcionalidad:

**Conversiones de formato de datos**

  + [Convertir a CSV ](convert-to-csv.md)

**Datos de entrada y salida módulos** realizan el trabajo de movimiento de datos desde orígenes en la nube en el experimento. Puede escribir sus resultados o los datos intermedios en el almacenamiento de Azure, una base de datos SQL o Hive, mientras se ejecuta un experimento, o usar almacenamiento en la nube para intercambiar datos entre los experimentos.  

  + [Import Data](import-data.md)

  + [Export Data](export-data.md)

  + [Escribir manualmente los datos](enter-data-manually.md)


**Módulos de transformación de datos** admiten operaciones en los datos que son exclusivas de machine learning, como la normalización o discretizar los datos, la selección de características y reducción de dimensionalidad.

  + [Seleccionar columnas de conjunto de datos](select-columns-in-dataset.md)

  + [Editar metadatos](edit-metadata.md)

  + [Limpiar datos que faltan](clean-missing-data.md)

  + [Agregar columnas](add-columns.md)

  + [Agregar filas](add-rows.md)

  + [Quitar filas duplicadas](remove-duplicate-rows.md)

  + [Dividir los datos](split-data.md)

  + [Normalizar datos](normalize-data.md)

  + [Partición y ejemplo](partition-and-sample.md)


**Algoritmos de aprendizaje automático** como agrupación en clústeres, la máquina de vectores de soporte técnico o las redes neuronales, están disponibles dentro de los módulos individuales que le permiten personalizar la tarea de aprendizaje automático con los parámetros adecuados.  
  + [Puntuar modelo](score-model.md)

  + [Asignar datos a clústeres ](assign-data-to-clusters.md)

  + [Entrenar modelo](train-model.md)

  + [Entrenar el modelo de agrupación en clústeres](train-clustering-model.md)

  + [Evaluar modelo](evaluate-model.md)

  + [Aplicar transformación](apply-transformation.md)

  + [Regresión lineal](linear-regression.md)

  + [Regresión de red neuronal](neural-network-regression.md)

  + [Regresión de bosque de decisión](decision-forest-regression.md)

  + [Regresión de árbol de decisión impulsado](boosted-decision-tree-regression.md)

  + [Árbol de decisión ampliado de dos clases](two-class-boosted-decision-tree.md)

  + [Regresión logística de dos clases](two-class-logistic-regression.md)

  + [Regresión logística multiclase](multiclass-logistic-regression.md)

  + [Red neuronal multiclase](multiclass-neural-network.md)

  + [Bosque de decisión multiclase](multiclass-decision-forest.md)

  + [Perceptrón promedio de dos clases](two-class-averaged-perceptron.md)

  + [Bosque de decisión Multiclase](two-class-decision-forest.md)

  + [Red neuronal de dos clases](two-class-neural-network.md)

  + [Máquina de vectores de soporte de dos clases](two-class-support-vector-machine.md)
  
  + [Agrupación en clústeres K-Means](k-means-clustering.md)


**Módulo de Python** facilita la tarea Ejecutar una función personalizada. Escribir el código e incrustarlo en un módulo de integración de Python con el servicio de un experimento.
  + [Ejecutar Script de Python](execute-python-script.md)

  + [Crear el modelo de Python](create-python-model.md)


