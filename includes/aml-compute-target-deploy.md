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
ms.openlocfilehash: a463ac9f9584cb13cadbcf79674d56b2f8e47c2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753111"
---
| Destino de proceso | Uso | Compatibilidad con GPU | Compatibilidad con FPGA | DESCRIPCIÓN |
| ----- | ----- | ----- | ----- | ----- |
| [Servicio web local](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Pruebas y depuración | es posible | &nbsp; | Bueno para pruebas limitadas y solución de problemas. La aceleración de hardware depende del uso de bibliotecas en el sistema local.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Inferencia en tiempo real |  [sí](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [sí](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Es útil para las implementaciones de producción a gran escala. Proporciona escalabilidad automática y rápidos tiempos de respuesta.  AKS es la única opción disponible para la interfaz visual. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Pruebas o desarrollo | &nbsp;  | &nbsp; | Adecuado para cargas de trabajo de pequeña escala basadas en CPU que requieren <48 GB de RAM |
| [Proceso de Azure Machine Learning](../articles/machine-learning/service/how-to-run-batch-predictions.md) | (Versión preliminar) Inferencia de Batch | Sí | &nbsp;  | Ejecute la puntuación por lotes en un proceso sin servidor. Admite máquinas virtuales de prioridad normal y baja. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | (Versión preliminar) Módulo de IoT |  &nbsp; | &nbsp; | Implemente y entregue modelos de Machine Learning en dispositivos IoT. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | a través de IoT Edge |  &nbsp; | Sí | Implemente y entregue modelos de Machine Learning en dispositivos IoT. |