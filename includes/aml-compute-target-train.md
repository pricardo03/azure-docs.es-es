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
ms.openlocfilehash: 1b62d4b2ac1bb69e2270c3202d29eb595df7aac4
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806056"
---
**Los destinos de proceso se pueden reutilizar de un trabajo de entrenamiento al siguiente**. Por ejemplo, una vez que se adjunta una VM remota al área de trabajo, puede reutilizarla para varios trabajos.

|&nbsp;Objetivos del aprendizaje| Compatibilidad con GPU |[Machine Learning automatizado](../articles/machine-learning/service/concept-automated-ml.md) | [Canalizaciones de Machine Learning](../articles/machine-learning/service/concept-ml-pipelines.md) | [Interfaz visual](../articles/machine-learning/service/ui-concept-visual-interface.md)
|----|:----:|:----:|:----:|:----:|
|[Equipo local](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| es posible | Sí | &nbsp; | &nbsp; |
|[Proceso de Azure Machine Learning](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| Sí | Sí <br/>&nbsp;ajuste de hiperparámetros | Sí | Sí |
|[Máquina virtual remota](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) |Sí | Sí <br/>ajuste de hiperparámetros | Sí | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| &nbsp; | Sí | Sí | &nbsp; |
|[Análisis con Azure Data Lake](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | Sí | &nbsp; |
|[HDInsight de Azure](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight)| &nbsp; | &nbsp; | Sí | &nbsp; |
|[Azure Batch](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch)| &nbsp; | &nbsp; | Sí | &nbsp; |
