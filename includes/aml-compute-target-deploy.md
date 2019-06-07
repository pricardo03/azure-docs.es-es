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
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753111"
---
| Destino de proceso | Uso | Compatibilidad con GPU | Soporte técnico FPGA | DESCRIPCIÓN |
| ----- | ----- | ----- | ----- | ----- |
| [Servicio web local](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Las pruebas y depuración | Quizás | &nbsp; | Bueno para limitado de probar y solucionar problemas. Aceleración de hardware depende de usar las bibliotecas en el sistema local.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Inferencia en tiempo real |  [yes](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [yes](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Es útil para las implementaciones de producción a gran escala. Proporciona el escalado automático y tiempos de respuesta rápidos.  AKS es la única opción disponible para la interfaz visual. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Las pruebas o desarrollo | &nbsp;  | &nbsp; | Bueno para la pequeña escala, cargas de trabajo basadas en CPU que requieren < 48 GB de RAM |
| [Proceso de Azure Machine Learning](../articles/machine-learning/service/how-to-run-batch-predictions.md) | (Versión preliminar) Inferencia de lote | Sí | &nbsp;  | Ejecute el proceso sin servidor de puntuación por lotes. Es compatible con máquinas virtuales normales y de baja prioridad. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | (Versión preliminar) Módulo de IoT |  &nbsp; | &nbsp; | Implementación y servir de modelos de aprendizaje automático en dispositivos IoT. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | via IoT Edge |  &nbsp; | Sí | Implementación y servir de modelos de aprendizaje automático en dispositivos IoT. |