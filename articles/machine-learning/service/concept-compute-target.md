---
title: Destinos de proceso
titleSuffix: Azure Machine Learning service
description: Un destino de proceso le permite especificar el recurso de proceso donde ejecute el script de entrenamiento o host de la implementación del servicio. Esta ubicación puede ser el equipo local o un recurso de proceso en la nube.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/30/2019
ms.openlocfilehash: 42c0f5460a63b781aafdd43410761e2d7b17944d
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755356"
---
#  <a name="what-is-a-compute-target-in-azure-machine-learning-service"></a>¿Qué es un destino de proceso en el servicio de Azure Machine Learning? 

Un destino de proceso le permite especificar el recurso de proceso donde ejecute el script de entrenamiento o host de la implementación del servicio. Esta ubicación puede ser el equipo local o un recurso de proceso en la nube.

Destinos de proceso fácil cambiar el entorno de proceso sin cambiar el código.  Un ciclo de vida de desarrollo de modelo típico:

* Comience con el desarrollo y experimentación en una pequeña cantidad de datos. En esta fase, se recomienda usar un entorno local. Por ejemplo, un equipo local o una máquina virtual basada en la nube.
* Escalar verticalmente su aprendizaje en grandes conjuntos de datos o distribuidas entrenamiento mediante uno de los [destinos entrenamiento](#train).  
* Implementar en varios entornos de hospedaje o los dispositivos de IoT mediante uno de los [destinos de implementación](#deploy).

Los recursos de proceso que se usa para los destinos de proceso están asociados a un [área de trabajo](concept-workspace.md). Proceso que los usuarios del área de trabajo comparten recursos distintos de la máquina local.

## <a name="train"></a> Objetivos de aprendizaje

Servicio Azure Machine Learning tiene compatibilidad con diferente a través de diferentes recursos de proceso.  También puede adjuntar su propio recurso de proceso, aunque el soporte técnico para varios escenarios puede variar como se detalla a continuación:

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]


## <a name="deploy"></a>Destinos de implementación

Los siguientes recursos de proceso pueden usarse para hospedar la implementación de modelo.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]


## <a name="managed-compute"></a>Proceso administrado

Un recurso de proceso administrado creado y administrado por servicio de Azure Machine Learning. Este proceso está optimizado para cargas de trabajo de machine learning. Proceso de Azure Machine Learning es el único proceso administrado a partir del 30 de mayo de 2019. Recursos de proceso administrados adicionales pueden agregarse en el futuro.

### <a name="amlcompute"></a> Azure Machine Learning Compute

Puede usar Azure Machine Learning Compute para el entrenamiento y para inferencia de batch (versión preliminar).  Con este recurso de proceso, debe:

* Node único o varios clústeres
* Escala automáticamente cada vez que envíe una ejecución 
* Administración automática del clúster y la programación de trabajos 
* Es compatible con recursos de CPU y GPU

Puede crear instancias de Azure Machine Learning Compute con cualquiera de las siguientes acciones:

* El Portal de Azure
* Azure Machine Learning SDK
* La CLI de Azure

Todos los demás recursos de proceso deben crearse fuera del área de trabajo y, a continuación, conectadas a ella.

## <a name="unmanaged-compute"></a>Proceso no administrado

Es un recurso de proceso no administrado *no* administrado por servicio de Azure Machine Learning. Crear este tipo de proceso fuera de Azure Machine Learning y luego adjuntarlo al área de trabajo. Recursos de proceso no administrado pueden requerir pasos adicionales para mantener o mejorar el rendimiento para cargas de trabajo de machine learning.

## <a name="next-steps"></a>Pasos siguientes

* [Configurar destinos de proceso de entrenamiento del modelo](how-to-set-up-training-targets.md)
* [Implementar modelos con el servicio de Azure Machine Learning](how-to-deploy-and-where.md)