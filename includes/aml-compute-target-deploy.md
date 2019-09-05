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
ms.date: 08/23/2019
ms.openlocfilehash: e402989c6bdbbc90ae10047fcf19f4f2d74328a6
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "70014812"
---
| Destino de proceso | Uso | Compatibilidad con GPU | Compatibilidad con FPGA | DESCRIPCIÓN |
| ----- | ----- | ----- | ----- | ----- |
| [Servicio&nbsp;web&nbsp;local](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Pruebas y depuración | &nbsp; | &nbsp; | Bueno para pruebas limitadas y solución de problemas. La aceleración de hardware depende del uso de bibliotecas en el sistema local.
| [Servicio web&nbsp;de máquina virtual&nbsp;de Notebook](../articles/machine-learning/service/how-to-deploy-and-where.md#notebookvm) | Pruebas y depuración | &nbsp; | &nbsp; | Bueno para pruebas limitadas y solución de problemas. 
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Inferencia en tiempo real |  [sí](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [sí](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Es útil para las implementaciones de producción a gran escala. Proporciona un tiempo de respuesta rápido y el escalado automático del servicio implementado. No se admite el escalado automático de clúster a través del SDK de Azure Machine Learning. Para cambiar los nodos del clúster de AKS, use la interfaz de usuario para el clúster de AKS en Azure Portal. AKS es la única opción disponible para la interfaz visual. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Pruebas o desarrollo | &nbsp;  | &nbsp; | Adecuado para cargas de trabajo de pequeña escala basadas en CPU que requieren <48 GB de RAM |
| [Proceso de Azure Machine Learning](../articles/machine-learning/service/how-to-run-batch-predictions.md) | Inferencia de lote&nbsp;(versión preliminar) | &nbsp; | &nbsp;  | Ejecute la puntuación por lotes en un proceso sin servidor. Admite máquinas virtuales de prioridad normal y baja. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | Módulo de IoT&nbsp;(versión preliminar) |  &nbsp; | &nbsp; | Implemente y entregue modelos de Machine Learning en dispositivos IoT. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | a través de IoT Edge |  &nbsp; | Sí | Implemente y entregue modelos de Machine Learning en dispositivos IoT. |

> [!NOTE]
> Mientras que los destinos de proceso como local, máquina virtual de Notebook y Proceso de Azure Machine Learning admiten GPU para entrenamiento y experimentación, el uso de la GPU para la inferencia solo se admite en Azure Kubernetes Service.