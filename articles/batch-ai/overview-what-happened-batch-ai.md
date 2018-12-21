---
title: ¿Qué le va a suceder a Azure Batch AI? | Microsoft Docs
description: Obtenga información sobre lo que le va a suceder a Azure Batch AI y la opción de proceso de Azure Machine Learning Service.
services: batch-ai
author: garyericson
ms.service: batch-ai
ms.topic: overview
ms.date: 12/14/2018
ms.author: garye
ms.openlocfilehash: 6803a47ae77c072eff05df65e37156c90aabf3e6
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436882"
---
# <a name="whats-happening-to-azure-batch-ai"></a>¿Qué le va a suceder a Azure Batch AI?

**El servicio Azure Batch AI se va a retirar en marzo.** Las funciones de entrenamiento y puntuación a escala de Batch AI están ahora disponibles en el [Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md), que está disponible con carácter general desde el 4 de diciembre de 2018.

Junto con muchas otras funciones de aprendizaje automático, Azure Machine Learning Service incluye un destino de proceso administrado basado en la nube para el entrenamiento, la implementación y la puntuación de modelos de aprendizaje automático. Este destino de proceso se llama [Proceso de Machine Learning](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Empezar a migrar y usarlo hoy](#migrate). Puede interactuar con Azure Machine Learning Service a través de sus [SDK de Python](../machine-learning/service/quickstart-create-workspace-with-python.md), la interfaz de línea de comandos y [Azure Portal](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Escala de tiempo del soporte técnico

| Date | Detalles de soporte del servicio Batch AI |
| ---- |-----------------|
| Diciembre&nbsp;14&#x2c;&nbsp;2018| Siga usando las suscripciones existentes de Azure Batch AI como antes. Sin embargo, ya no se pueden registrar **nuevas suscripciones** ni realizar nuevas inversiones en este servicio.|
| Marzo&nbsp;31&#x2c;&nbsp;2019 | Tras esta fecha, las suscripciones existentes de Batch AI ya no funcionarán. |

<a name="migrate"></a>
## <a name="how-do-i-migrate"></a>¿Cómo realizo una migración?

Para evitar interrupciones en las aplicaciones y beneficiarse de las características más recientes, siga estos pasos antes del 31 de marzo de 2019:

1. Cree un área de trabajo de Azure Machine Learning Service y empiece:
    + [Inicio rápido basado en Python](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Inicio rápido basado en Azure Portal](../machine-learning/service/quickstart-get-started.md)

1. Configure una instancia de [Proceso de Machine Learning](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute) para entrenar el modelo.

1. Actualice los scripts para usar el Proceso de Machine Learning.

## <a name="support"></a>Soporte técnico

Hay soporte técnico disponible para los clientes existentes que deseen migrar a [Azure Machine Learning Service](https://aka.ms/aml-docs), más completo.

Si Azure Machine Learning Service no satisface sus necesidades donde existe una funcionalidad compatible en el servicio Batch AI, abra una solicitud de soporte de Batch AI con el equipo de soporte técnico para incluir en la lista blanca su suscripción a fin de usar Batch AI hasta que se retire el servicio.

## <a name="next-steps"></a>Pasos siguientes

+ Lea la [información general de Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Configure un destino de proceso para el entrenamiento de modelos](../machine-learning/service/how-to-set-up-training-targets.md) con Azure Machine Learning Service.

+ Revise la [hoja de ruta de Azure](https://azure.microsoft.com/updates/) para obtener información de otras actualizaciones de servicio de Azure.
