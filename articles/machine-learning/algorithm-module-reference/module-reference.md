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
ms.openlocfilehash: 8825f1dc3b66a5c4981ba25a90813aec63975b1f
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145133"
---
# <a name="algorithm--module-reference-overview"></a>Información general de referencia de módulos y algoritmos

Este contenido de referencia proporciona la experiencia técnica en cada uno de los módulos disponibles en la interfaz visual (versión preliminar) del servicio de Azure Machine Learning y algoritmos de aprendizaje automático.

Cada módulo representa un conjunto de código que puede ejecutarse de forma independiente y realizar un machine learning tarea, dadas las entradas necesarias. Un módulo podría contener un algoritmo determinado o realizar una tarea que es importante en machine learning, como reemplazo de valor que falta o realizar análisis estadísticos.

> [!TIP]
> En todos los experimentos en la interfaz visual, puede obtener información acerca de un módulo específico. Seleccione el módulo y luego seleccione el **más ayuda** vincular en el **ayuda rápida** panel.

## <a name="modules"></a>Módulos

Los módulos se organizan por funcionalidad:

| Funcionalidad | DESCRIPCIÓN | Módulo |
| --- |--- | ---- |
| Conversiones de formato de datos | Convertir datos entre diversos formatos de archivo usados en aprendizaje automático, | [Convertir a CSV](convert-to-csv.md) |
| La entrada y salida | Mover datos desde orígenes en la nube en el experimento. Escribir los resultados o los datos intermedios en el almacenamiento de Azure, una base de datos SQL o Hive, mientras se ejecuta un experimento o usar el almacenamiento en la nube para intercambiar datos entre los experimentos.  | [Import Data](import-data.md)<br/>[Export Data](export-data.md)<br/>[Escribir manualmente los datos](enter-data-manually.md) |
| Transformación de datos | Operaciones en los datos que son exclusivas de machine learning, como la normalización o discretizar los datos, la selección de características y reducción de dimensionalidad.| [Seleccionar columnas de conjunto de datos](select-columns-in-dataset.md) <br/> [Editar metadatos](edit-metadata.md) <br/> [Limpiar datos que faltan](clean-missing-data.md) <br/> [Agregar columnas](add-columns.md) <br/> [Agregar filas](add-rows.md) <br/> [Quitar filas duplicadas](remove-duplicate-rows.md) <br/> [Dividir los datos](split-data.md) <br/> [Normalizar datos](normalize-data.md) <br/> [Partición y ejemplo](partition-and-sample.md) |
| Módulo de Python | Escribir código y lo insertará en un módulo de integración de Python con el experimento. | [Ejecutar Script de Python](execute-python-script.md)   <br/> [Crear el modelo de Python](create-python-model.md)
|  | **Algoritmos de aprendizaje automático**: | |
| clasificación | Predecir una clase.  Elija en binario (dos clases) o algoritmos multiclase.| [Bosque de decisión multiclase](multiclass-decision-forest.md) <br/> [Regresión logística multiclase](multiclass-logistic-regression.md)  <br/> [Red neuronal multiclase](multiclass-neural-network.md)  <br/>  [Regresión logística de dos clases](two-class-logistic-regression.md)  <br/>[Perceptrón promedio de dos clases](two-class-averaged-perceptron.md) <br/> [Dos clases&nbsp;impulsado&nbsp;decisión&nbsp;árbol](two-class-boosted-decision-tree.md)  <br/> [Bosque de decisión Multiclase](two-class-decision-forest.md)  <br/> [Red neuronal de dos clases](two-class-neural-network.md)  <br/> [Dos&#8209;clase&nbsp;soporte&nbsp;Vector&nbsp;máquina](two-class-support-vector-machine.md) 
| Agrupación en clústeres | Agrupar datos.| [Agrupación en clústeres K-Means](k-means-clustering.md)
| Regresión | Predecir un valor. | [Regresión lineal](linear-regression.md)  <br/> [Regresión de red neuronal](neural-network-regression.md)  <br/> [Regresión de bosque de decisión](decision-forest-regression.md)  <br/> [Impulsado&nbsp;decisión&nbsp;árbol&nbsp;regresión](boosted-decision-tree-regression.md)
|  | **Crear y evaluar modelos**: | |
| Train   | Datos de ejecución a través del algoritmo. | [Entrenar modelo](train-model.md)  <br/> [Entrenar el modelo de agrupación en clústeres](train-clustering-model.md)    |
| Evaluar modelo | Medir la precisión del modelo entrenado. |  [Evaluar modelo](evaluate-model.md)
| Score | Obtener las predicciones del modelo que acaba de entrenar. | [Aplicar transformación](apply-transformation.md)<br/>[Asignar&nbsp;datos&nbsp;a&nbsp;clústeres](assign-data-to-clusters.md) <br/>[Puntuar modelo](score-model.md)

## <a name="error-messages"></a>mensajes de error

Obtenga información sobre la [mensajes de error y códigos de excepción](machine-learning-module-error-codes.md) pueden surgir con los módulos de la interfaz visual del servicio de Azure Machine Learning.
