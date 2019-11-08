---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/30/2019
ms.openlocfilehash: aa1120db5451583153928bd2cbc383a46781a267
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489564"
---
**Los destinos de proceso se pueden reutilizar de un trabajo de entrenamiento al siguiente**. Por ejemplo, una vez que se adjunta una VM remota al área de trabajo, puede reutilizarla para varios trabajos.

|&nbsp;Objetivos del aprendizaje| Compatibilidad con GPU |[Machine Learning automatizado](../articles/machine-learning/service/concept-automated-ml.md) | [Canalizaciones de Machine Learning](../articles/machine-learning/service/concept-ml-pipelines.md) | [Diseñador de Azure Machine Learning](../articles/machine-learning/service/concept-designer.md)
|----|:----:|:----:|:----:|:----:|
|[Equipo local](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| es posible | Sí | &nbsp; | &nbsp; |
|[Instancia de proceso de Azure Machine Learning](../articles/machine-learning/service/concept-compute-instance.md)| Sí | | Sí |  |
|[Clúster de proceso de Azure Machine Learning](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| Sí | Sí <br/>&nbsp;ajuste de hiperparámetros | Sí | Sí |
|[Máquina virtual remota](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) |Sí | Sí <br/>ajuste de hiperparámetros | Sí | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| &nbsp; | Sí | Sí | &nbsp; |
|[Análisis con Azure Data Lake](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | Sí | &nbsp; |
|[HDInsight de Azure](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight)| &nbsp; | &nbsp; | Sí | &nbsp; |
|[Azure Batch](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch)| &nbsp; | &nbsp; | Sí | &nbsp; |
