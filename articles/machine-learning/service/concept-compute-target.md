---
title: Destinos de proceso
titleSuffix: Azure Machine Learning service
description: Un destino de proceso le permite especificar el recurso de proceso en el que ejecutar el script de entrenamiento o se hospeda la implementación del servicio web. Esta ubicación puede ser su equipo local o un recurso de proceso en la nube.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/30/2019
ms.openlocfilehash: 42c0f5460a63b781aafdd43410761e2d7b17944d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66755356"
---
#  <a name="what-is-a-compute-target-in-azure-machine-learning-service"></a>¿Qué es un destino de proceso en Azure Machine Learning Service? 

Un destino de proceso le permite especificar el recurso de proceso en el que ejecutar el script de entrenamiento o se hospeda la implementación del servicio web. Esta ubicación puede ser su equipo local o un recurso de proceso en la nube.

Los destinos de proceso facilitan el cambio entorno de proceso sin cambiar el código.  Un ciclo de vida de desarrollo de modelos típico:

* Comience con el desarrollo y la experimentación en una pequeña cantidad de datos. En esta fase, se recomienda usar un entorno local. Por ejemplo, un equipo local o una máquina virtual basada en la nube.
* Escale verticalmente su aprendizaje de grandes conjuntos de datos o realice un entrenamiento distribuido mediante uno de los [destinos de entrenamiento](#train).  
* Realice la implementación en varios entornos de hospedaje web o en dispositivos de IoT que usan uno de los [destinos de implementación](#deploy).

Los recursos de proceso que use para los destinos de proceso están asociados a un [área de trabajo](concept-workspace.md). Los usuarios del área de trabajo comparten los recursos de proceso que no sean el equipo local.

## <a name="train"></a> Objetivos del aprendizaje

Azure Machine Learning Service tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso.  También puede adjuntar su propio recurso de proceso, aunque el soporte técnico para varios escenarios puede variar como se detalla a continuación:

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]


## <a name="deploy"></a>Destinos de implementación

Los siguientes recursos de proceso se pueden usar hospedar la implementación del modelo.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]


## <a name="managed-compute"></a>Proceso administrado

Azure Machine Learning Service crea y administra un recurso de proceso administrado. Dicho proceso está optimizado para cargas de trabajo de Machine Learning. Proceso de Azure Machine Learning es el único destino de proceso administrado desde el 30 de mayo de 2019. En el futuro podrán agregarse recursos de proceso administrados adicionales.

### <a name="amlcompute"></a>Proceso de Azure Machine Learning

Puede usar Proceso de Azure Machine Learning para el entrenamiento y para inferencia de lotes (versión preliminar).  Con este recurso de proceso, debe:

* Clúster de uno o varios nodos
* Se escala automáticamente cada vez que se envía una ejecución 
* Administración del clúster automático y programación de trabajos 
* Es compatible con recursos de CPU y GPU

Puede crear instancias de Proceso de Azure Machine Learning con cualquiera de las siguientes acciones:

* El Portal de Azure
* El SDK de Azure Machine Learning
* La CLI de Azure

Los restantes recursos de proceso deben crearse fuera del área de trabajo y adjuntarse posteriormente.

## <a name="unmanaged-compute"></a>Proceso no administrado

Azure Machine Learning Service *no*administra un recurso de proceso no administrado. Cree este tipo de proceso fuera de Azure Machine Learning y luego adjúntelo al área de trabajo. Estos recursos de proceso no administrados pueden requerir pasos adicionales para mantener o mejorar el rendimiento de las cargas de trabajo de Machine Learning.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de destinos de proceso del entrenamiento del modelo](how-to-set-up-training-targets.md)
* [Implementación de modelos con Azure Machine Learning Service](how-to-deploy-and-where.md)