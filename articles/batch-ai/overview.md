---
title: 'Servicio Azure Batch AI: entrenamiento de la AI | Microsoft Docs'
description: Obtenga más información sobre el uso del servicio Azure Batch AI para entrenar la inteligencia artificial (AI) y otros modelos de aprendizaje automático en clústeres de GPU y CPU.
services: batch-ai
documentationcenter: ''
author: alexsuttonms
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/01/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 37f1bf5dc20d097f7f5f560e3bf1fdd25dc38045
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408058"
---
# <a name="what-is-azure-batch-ai"></a>¿Qué es Azure Batch AI?

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Azure Batch AI es un servicio administrado creado para ayudar a los científicos de datos y a los investigadores de inteligencia artificial a entrenar y probar el aprendizaje automático y los modelos de inteligencia artificial a escala de Azure. Todo ello sin tener que administrar una infraestructura compleja. Describa los recursos de proceso, los trabajos que desea ejecutar, dónde desea almacenar las entradas y las salidas del modelo, y Batch AI se encarga del resto.

Puede usar Batch AI tanto de forma independiente o para realizar el entrenamiento del modelo como parte de un flujo de trabajo de desarrollo mayor:

* Use Batch AI independientemente para entrenar, probar y puntuar modelos de inteligencia artificial y aprendizaje automático en lote en clústeres de [GPU](../virtual-machines/linux/sizes-gpu.md) o CPU. 

* Elija un clúster de Batch AI como destino en un flujo de trabajo con [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) u otras [herramientas de la plataforma Azure AI](https://azure.microsoft.com/overview/ai-platform/). Azure Machine Learning proporciona una experiencia enriquecida para la preparación de datos, la experimentación y el historial de trabajos. También puede empaquetar un modelo entrenado en un contenedor e implementar un modelo para la inferencia o para dispositivos IoT.  

## <a name="train-machine-learning-and-ai-models"></a>Entrenamiento de modelos de inteligencia artificial y aprendizaje automático

Use Batch AI para entrenar modelos de aprendizaje automático, redes neuronales profundas (aprendizaje profundo) y otros enfoques de inteligencia artificial. Batch AI tiene compatibilidad integrada para las plataformas y las bibliotecas de inteligencia artificial populares de código abierto, como [TensorFlow](https://github.com/tensorflow/tensorflow), [PyTorch](https://github.com/pytorch/pytorch), [Chainer](https://github.com/chainer/chainer) y [Microsoft Cognitive Toolkit (CNTK)](https://github.com/Microsoft/CNTK).

Una vez identificada la zona problemática y preparados los datos, trabaje interactivamente con Batch AI para probar las ideas de modelo. A continuación, cuando esté listo para experimentar a escala, inicie trabajos en varias GPU con MPI u otras bibliotecas de comunicación y ejecute más experimentos en paralelo.

Batch AI le ayuda a entrenar modelos a escala de varias maneras. Por ejemplo:  

|  |  |
|---------|---------|
| **Distribuir el entrenamiento**<br/>![Entrenamiento distribuido](./media/overview/distributed-training.png)  | Escale verticalmente un trabajo en distintas GPU conectadas en red para entrenar redes mayores con grandes volúmenes de datos.|
| **Experimentar**<br/>![Experimentación](./media/overview/experimentation.png) | Escale horizontalmente el entrenamiento con varios trabajos. Ejecute barridos de parámetros para probar nuevas ideas o ajustar los hiperparámetros y optimizar el rendimiento y la precisión. |
| **Ejecutar en paralelo**![Ejecución en paralelo](./media/overview/parallel-execution.png) | Entrene o puntúe muchos modelos simultáneamente, al ejecutarlos en paralelo en una flota de servidores para que los trabajos se realicen más rápidamente.|

Cuando haya entrenado un modelo, puede usar Batch AI para probarlo si esto no formaba parte del script de entrenamiento, o también puede realizar una puntuación por lotes.

## <a name="how-it-works"></a>Cómo funciona

Use los SDK de Batch AI, los scripts de línea de comandos o Azure Portal para administrar recursos de proceso y programar trabajos para entrenar y probar la inteligencia artificial: 

* **Aprovisionamiento y escalado de los clústeres de máquinas virtuales**: elija el número de nodos (máquinas virtuales) y seleccione una máquina virtual con GPU activado o de otro tamaño que responda a sus necesidades de entrenamiento. Escale o reduzca verticalmente el número de nodos de forma automática o manual para usar los recursos solo cuando sea necesario. 

* **Administración de las dependencias y los contenedores**: de forma predeterminada, los clústeres de Batch AI ejecutan imágenes de máquina virtual Linux con dependencias preinstaladas para ejecutar las plataformas de aprendizaje basadas en contenedores, ya sea en GPU o en CPU. Para configuración adicional, elija imágenes personalizadas o ejecute scripts de inicio.

* **Distribución de datos**: elija una o varias opciones de almacenamiento para administrar los scripts y datos de entrada y la salida del trabajo: Azure Files, Azure Blob Storage o un servidor NFS administrado. Batch AI también admite soluciones de almacenamiento personalizadas, como los sistemas de archivos paralelos de alto rendimiento. Monte los sistemas de archivos de almacenamiento en los nodos del clúster y los contenedores de trabajo mediante archivos de configuración sencillos.

* **Programación de trabajos**: envíe trabajaos a una cola de trabajo basado en prioridades para compartir recursos de clúster y aprovechar las ventajas de las instancias reservadas y de las máquinas virtuales de prioridad baja.

* **Control de los errores**: supervise el estado del trabajo y reinicie los trabajos en el caso de error en la máquina virtual durante los trabajos de larga duración.

* **Recopilación de resultados**: acceda fácilmente a los registros de salida, Stdout, Stderr y a los modelos entrenados. Configure los trabajos de Batch AI para insertar la salida directamente en el almacenamiento montado.

Como servicio de Azure, Batch AI admite las herramientas comunes como el control de acceso basado en rol (RBAC) y las redes virtuales de Azure para la seguridad.  

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre los [recursos de Batch AI](resource-concepts.md) para entrenar un modelo de inteligencia artificial o aprendizaje automático.

* Introducción al [entrenamiento de un modelo de aprendizaje profundo de ejemplo](quickstart-tensorflow-training-cli.md) con Batch AI.

* Extraiga del repositorio [instrucciones de entrenamiento](https://github.com/Azure/BatchAI/blob/master/recipes) de ejemplo para diferentes plataformas de inteligencia artificial.
