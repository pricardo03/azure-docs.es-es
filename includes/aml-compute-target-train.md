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
ms.openlocfilehash: 50905eb987defac612f1055b450b682726f0a56f
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752956"
---
|Entrenamiento &nbsp;destinos| Compatibilidad con GPU |[ML automatizada](../articles/machine-learning/service/concept-automated-ml.md) | [Canalizaciones de ML](../articles/machine-learning/service/concept-ml-pipelines.md) | [Interfaz Visual](../articles/machine-learning/service/ui-concept-visual-interface.md)
|----|:----:|:----:|:----:|:----:|
|[Equipo local](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| Quizás | Sí | &nbsp; | &nbsp; |
|[Proceso de Azure Machine Learning](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| Sí | Sí & <br/>hiperparámetros&nbsp;optimización | Sí | Sí |
|[Máquina virtual remota](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) |Sí | Sí & <br/>ajuste de hiperparámetros | Sí | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| &nbsp; | Sí | Sí | &nbsp; |
|[Análisis con Azure Data Lake](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | Sí | &nbsp; |
|[HDInsight de Azure](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight)| &nbsp; | &nbsp; | Sí | &nbsp; |
|[Azure Batch](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch)| &nbsp; | &nbsp; | Sí | &nbsp; |

**Todos los destinos de proceso se pueden reutilizar para varios trabajos de entrenamiento**. Por ejemplo, una vez que se adjunta una VM remota al área de trabajo, puede reutilizarla para varios trabajos.