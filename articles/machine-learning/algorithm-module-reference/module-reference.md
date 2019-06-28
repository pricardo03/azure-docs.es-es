---
title: Referencia de módulos y algoritmos
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre los módulos disponibles en la interfaz visual de Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 8825f1dc3b66a5c4981ba25a90813aec63975b1f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65145133"
---
# <a name="algorithm--module-reference-overview"></a>Información general sobre la referencia de módulos y algoritmos

Este contenido de referencia proporciona la experiencia técnica sobre cada uno de los algoritmos y módulos de aprendizaje automático disponibles en la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Cada módulo representa un conjunto de código que puede ejecutarse de forma independiente y realizar una tarea de aprendizaje automático, dadas las entradas necesarias. Un módulo puede contener un algoritmo en particular o realizar una tarea que sea importante para el aprendizaje automático, como el reemplazo de un valor que falta o análisis estadísticos.

> [!TIP]
> En cualquier experimento en la interfaz visual, puede obtener información sobre un módulo específico. Seleccione el módulo y luego seleccione el vínculo **más ayuda** en el panel **Ayuda rápida**.

## <a name="modules"></a>Módulos

Los módulos se organizan por funcionalidad:

| Funcionalidad | DESCRIPCIÓN | Módulo |
| --- |--- | ---- |
| Conversiones de formato de datos | Convierta datos entre diversos formatos de archivo usados en aprendizaje automático. | [Conversión a CSV](convert-to-csv.md) |
| Entrada y salida de datos | Mueva datos desde orígenes en la nube a su experimento. Escriba los resultados o los datos intermedios en Azure Storage, una base de datos SQL o Hive, mientras ejecuta un experimento, o use el almacenamiento en la nube para intercambiar datos entre experimentos.  | [Import Data](import-data.md)<br/>[Export Data](export-data.md)<br/>[Introducción manual de datos](enter-data-manually.md) |
| Transformación de datos | Operaciones con los datos que son exclusivas del aprendizaje automático, como la normalización o discretización de datos, la selección de características y la reducción de la dimensionalidad.| [Seleccionar columnas de conjunto de datos](select-columns-in-dataset.md) <br/> [Edición de metadatos](edit-metadata.md) <br/> [Limpiar datos que faltan](clean-missing-data.md) <br/> [Adición de columnas](add-columns.md) <br/> [Adición de filas](add-rows.md) <br/> [Supresión de filas duplicadas](remove-duplicate-rows.md) <br/> [División de datos](split-data.md) <br/> [Normalización de datos](normalize-data.md) <br/> [Partición y ejemplo](partition-and-sample.md) |
| Módulo de Python | Escriba código e insértelo en un módulo para integrar Python en el experimento. | [Ejecución de script de Python](execute-python-script.md)   <br/> [Creación de modelo Python](create-python-model.md)
|  | **Algoritmos de aprendizaje automático**: | |
| clasificación | Prediga una clase.  Elija en algoritmos binarios (dos clases) o multiclase.| [Bosque de decisión multiclase](multiclass-decision-forest.md) <br/> [Regresión logística multiclase](multiclass-logistic-regression.md)  <br/> [Red neuronal multiclase](multiclass-neural-network.md)  <br/>  [Regresión logística de dos clases](two-class-logistic-regression.md)  <br/>[Perceptrón promedio de dos clases](two-class-averaged-perceptron.md) <br/> [Árbol de&nbsp;decisión&nbsp;ampliado de&nbsp;dos clases](two-class-boosted-decision-tree.md)  <br/> [Bosque de decisión de dos clases](two-class-decision-forest.md)  <br/> [Red neuronal de dos clases](two-class-neural-network.md)  <br/> [Máquina de&nbsp;soporte de&nbsp;vectores de&nbsp;dos clases](two-class-support-vector-machine.md) 
| Agrupación en clústeres | Agrupe datos.| [Agrupación en clústeres K-Means](k-means-clustering.md)
| Regresión | Prediga un valor. | [Regresión lineal](linear-regression.md)  <br/> [Regresión de red neuronal](neural-network-regression.md)  <br/> [Regresión de bosque de decisión](decision-forest-regression.md)  <br/> [Regresión de&nbsp;árbol de&nbsp;decisión&nbsp;incrementado](boosted-decision-tree-regression.md)
|  | **Compilar y evaluar modelos**: | |
| Train   | Ejecute datos a través del algoritmo. | [Entrenamiento de modelo](train-model.md)  <br/> [Entrenamiento del modelo de agrupación en clústeres](train-clustering-model.md)    |
| Evaluate Model (Evaluar modelo) | Mida la precisión del modelo entrenado. |  [Evaluación de módulo](evaluate-model.md)
| Score | Obtenga predicciones del modelo que acaba de entrenar. | [Aplicación de la transformación](apply-transformation.md)<br/>[Asignación de&nbsp;datos&nbsp;a&nbsp;clústeres](assign-data-to-clusters.md) <br/>[Puntuación de modelo](score-model.md)

## <a name="error-messages"></a>mensajes de error

Obtenga información sobre los [mensajes de error y códigos de excepción](machine-learning-module-error-codes.md) que pueden surgir al usar módulos en la interfaz visual de Azure Machine Learning Service.
