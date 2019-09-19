---
title: 'Destinos de proceso: dónde entrenar e implementar los modelos'
titleSuffix: Azure Machine Learning service
description: Defina dónde desea entrenar o implementar el modelo con Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 07/10/2019
ms.openlocfilehash: b2e4c9160ccad66b04dd30979f0568da68836d76
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860590"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning-service"></a>¿Qué son los destinos de proceso en Azure Machine Learning Service? 

Un **destino de proceso** es un entorno o recurso de proceso en el que se ejecuta el script de entrenamiento o se hospeda la implementación del servicio web. Esta ubicación puede ser su equipo local o un recurso de proceso en la nube. El uso de los destinos de proceso le facilita cambiar posteriormente el entorno de proceso sin tener que cambiar el código.  

En un ciclo de vida de desarrollo de modelos típico, puede:
1. Comenzar desarrollando y experimentando con una pequeña cantidad de datos. En esta etapa, le recomendamos su entorno local (equipo local o máquina virtual basada en la nube) como destino de proceso. 
2. Escalar verticalmente a datos más grandes o realizar un entrenamiento distribuido mediante uno de estos [destinos de proceso de entrenamiento](#train).  
3. Una vez que el modelo esté listo, impleméntelo en un entorno de hospedaje web o en un dispositivo de IoT con uno de estos [destinos de proceso de implementación](#deploy).

Los recursos de proceso que use para los destinos de proceso están asociados a un [área de trabajo](concept-workspace.md). Los usuarios del área de trabajo comparten los recursos de proceso que no sean el equipo local.

## <a name="train"></a> Entrenamiento de destinos de proceso

Azure Machine Learning Service tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso.  También puede adjuntar su propio recurso de proceso, aunque el soporte técnico para varios escenarios puede variar.

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]

Obtenga más información sobre la [configuración y el uso de un destino de proceso para el entrenamiento del modelo](how-to-set-up-training-targets.md).

## <a name="deploy"></a>Destinos de implementación

Los siguientes recursos de proceso se pueden usar hospedar la implementación del modelo.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

Aprenda [dónde y cómo implementar el modelo en un destino de proceso](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Proceso de Azure Machine Learning (administrado)

Azure Machine Learning Service crea y administra un recurso de proceso administrado. Dicho proceso está optimizado para cargas de trabajo de Machine Learning. Proceso de Azure Machine Learning es el único destino de proceso administrado desde el 30 de mayo de 2019. En el futuro podrán agregarse recursos de proceso administrados adicionales.

Puede usar Proceso de Azure Machine Learning para el entrenamiento y para inferencia de lotes (versión preliminar).  Con este recurso de proceso, debe:

* Clúster de uno o varios nodos
* Se escala automáticamente cada vez que se envía una ejecución 
* Administración del clúster automático y programación de trabajos 
* Es compatible con recursos de CPU y GPU

Puede crear instancias de Proceso de Azure Machine Learning en Azure Portal o su [página de aterrizaje del área de trabajo (versión preliminar)](https://ml.azure.com), con el SDK o la CLI. Cuando se crea, forma parte automáticamente del área de trabajo, a diferencia de otros tipos de destinos de proceso.

## <a name="unmanaged-compute"></a>Proceso no administrado

Azure Machine Learning Service *no*administra un destino de proceso no administrado. Cree este tipo de destino de proceso fuera de Azure Machine Learning y luego adjúntelo al área de trabajo. Estos recursos de proceso no administrados pueden requerir pasos adicionales para mantener o mejorar el rendimiento de las cargas de trabajo de Machine Learning.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo:
* [Configurar un destino de proceso para entrenar el modelo](how-to-set-up-training-targets.md)
* [Implementar el modelo para un destino de proceso](how-to-deploy-and-where.md)