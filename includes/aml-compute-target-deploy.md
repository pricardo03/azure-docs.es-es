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
ms.openlocfilehash: 722893fca90e5a5d0958d1de1698b625af4a5e21
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497023"
---
| Destino de proceso | Usado para | Compatibilidad con GPU | Compatibilidad con FPGA | DESCRIPCIÓN |
| ----- | ----- | ----- | ----- | ----- |
| [Servicio&nbsp;web&nbsp;local](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Pruebas y depuración | &nbsp; | &nbsp; | Se usa para pruebas limitadas y solución de problemas. La aceleración de hardware depende del uso de bibliotecas en el sistema local.
| [Servicio&nbsp;web&nbsp;de la instancia de proceso de Azure Machine Learning](../articles/machine-learning/service/how-to-deploy-and-where.md#notebookvm) | Pruebas y depuración | &nbsp; | &nbsp; | Se usa para pruebas limitadas y solución de problemas.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Inferencia en tiempo real |  [Sí](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md) (implementación del servicio web) | [Sí](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Se usa para las implementaciones de producción a gran escala. Proporciona un tiempo de respuesta rápido y el escalado automático del servicio implementado. No se admite el escalado automático de clúster a través del SDK de Azure Machine Learning. Para cambiar los nodos del clúster de AKS, use la interfaz de usuario para el clúster de AKS en Azure Portal. AKS es la única opción disponible para el diseñador. |
| [Azure Container Instances](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Pruebas o desarrollo | &nbsp;  | &nbsp; | Se usa para cargas de trabajo basadas en CPU a pequeña escala que requieren menos de 48 GB de RAM. |
| [Clústeres de proceso de Azure Machine Learning](../articles/machine-learning/service/how-to-run-batch-predictions.md) | Inferencia de lote&nbsp;(versión preliminar) | [Sí](../articles/machine-learning/service/how-to-run-batch-predictions.md) (canalización de aprendizaje automático) | &nbsp;  | Ejecute la puntuación por lotes en un proceso sin servidor. Admite máquinas virtuales de prioridad normal y baja. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | Módulo de IoT&nbsp;(versión preliminar) |  &nbsp; | &nbsp; | Implementación y entrega de modelos de Machine Learning en dispositivos IoT. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | A través de IoT Edge |  &nbsp; | Sí | Implementación y entrega de modelos de Machine Learning en dispositivos IoT. |

> [!NOTE]
> Aunque los destinos de proceso, como la instancia local de proceso de Azure Machine Learning y los clústeres de proceso de Azure Machine Learning, admiten GPU para el aprendizaje y la experimentación, el uso de la GPU para la inferencia __cuando se implementa como un servicio web__ solo se admite en Azure Kubernetes Service.
>
> El uso de una GPU para la inferencia __cuando se realiza la puntuación con una canalización de aprendizaje automático__ solo se admite en el Proceso de Azure Machine Learning.