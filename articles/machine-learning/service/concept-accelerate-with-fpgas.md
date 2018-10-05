---
title: ¿Qué es FPGA y Project Brainwave? - Azure Machine Learning
description: Obtenga información sobre cómo acelerar modelos y redes neurales profundas con FPGA en Azure. En este artículo se proporciona una introducción a las matrices de puertas programables por campo (FPGA) y cómo Azure Machine Learning ofrece inteligencia artificial (IA) en tiempo real al implementar el modelo en una FPGA de Azure.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: tedway
author: tedway
ms.reviewer: jmartens
ms.date: 9/24/2018
ms.openlocfilehash: adcd812bc63f98e4cbde949946bf32a037c6d704
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47158698"
---
# <a name="what-is-fpga-and-project-brainwave"></a>¿Qué es FPGA y Project Brainwave?

En este artículo se proporciona una introducción a las matrices de puertas programables por campo (FPGA) y cómo Azure Machine Learning ofrece inteligencia artificial (IA) en tiempo real al implementar el modelo en una FPGA de Azure.

Las FPGA contienen una matriz de bloques de lógica programables y una jerarquía de interconexiones reconfigurables. Las interconexiones permiten que estos bloques se configuren de distintas maneras después de su fabricación. Las FPGA ofrecen una combinación de capacidad de programación y rendimiento en comparación con otros chips.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA en comparación con CPU, GPU y ASIC

![Comparación de FPGA de Azure Machine Learning](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|Procesador||DESCRIPCIÓN|
|---|:-------:|------|
|Circuitos integrados específicos de la aplicación|ASIC|Los circuitos personalizados, como las unidades de procesador TensorFlow (TPU) de Google, proporcionan la máxima eficacia. No pueden reconfigurarse según las necesidades.|
|Matrices de puertas programables por campo|FPGA|Las FPGA, como las disponibles en Azure, proporcionan un rendimiento próximo al de los ASIC, pero son flexibles y se pueden volver a configurar con el tiempo para implementar una lógica nueva.|
|Unidades de procesamiento gráfico|GPU|Una opción popular para los cálculos de inteligencia artificial que ofrece funcionalidades de procesamiento en paralelo, lo que permite una representación de imágenes más rápida que las CPU.|
|Unidades centrales de procesamiento|CPU|Procesadores de uso general cuyo rendimiento no es óptimo para el procesamiento de vídeo y gráficos.|

## <a name="project-brainwave-on-azure"></a>Project Brainwave en Azure

[Project Brainwave](https://www.microsoft.com/research/project/project-brainwave/) es la arquitectura de hardware económica de Microsoft, basada en dispositivos FPGA de Intel, que los científicos de datos y los desarrolladores usan para acelerar los cálculos de inteligencia artificial en tiempo real.  Esta arquitectura habilitada para FPGA ofrece **rendimiento**, **flexibilidad** y **escala**, y está disponible en Azure.

**Las FPGA permiten alcanzar una latencia baja para las solicitudes de inferencia en tiempo real.** El procesamiento por lotes implica recopilar una gran cantidad de datos y colocarlos en un procesador para mejorar la utilización del hardware. El procesamiento por lotes puede causar latencia porque se deben procesar más datos, pero puede mejorar el rendimiento. Las implementaciones de Project Brainwave de unidades de procesamiento neuronal no requieren procesamiento por lotes; por lo tanto, la latencia puede ser muchas veces menor en comparación con las unidades CPU y GPU.

### <a name="reconfigurable-power"></a>Capacidad reconfigurable
**Las FPGA se pueden volver a configurar para distintos tipos de modelos de aprendizaje automático.** Esta flexibilidad facilita la aceleración de las aplicaciones en función del modelo de memoria y la precisión numérica más óptima que se usa.

Se desarrollan técnicas de aprendizaje automático nuevas de manera regular y el diseño de hardware de Project Brainwave también evoluciona rápidamente. Como las FPGA se pueden volver a configurar, es posible mantenerse al día con los requisitos de cambiar rápidamente los algoritmos de IA.

### <a name="whats-supported-on-azure"></a>Qué se admite en Azure
**Microsoft Azure es la mayor inversión en la nube del mundo en FPGA.** Puede ejecutar Project Brainwave en la infraestructura de escala de Azure.

Actualmente Project Brainwave admite lo siguiente:
+ Escenarios de reconocimiento y clasificación de imágenes
+ Implementación de TensorFlow
+ DNN: ResNet 50, ResNet 152, VGG-16, SSD-VGG y DenseNet-121
+ Hardware de FPGA de Intel 

Con esta arquitectura de hardware habilitada para FPGA, las redes neurales entrenadas se ejecutan rápidamente y con una latencia menor. Project Brainwave puede paralelizar redes neuronales profundas (DNN) entrenadas previamente en las FPGA para realizar el escalado horizontal del servicio. Las DNN pueden entrenarse previamente, como un caracterizador profundo para la transferencia de aprendizaje o ajustarse con cargas actualizadas.

### <a name="scenarios-and-applications"></a>Escenarios y aplicaciones

Project Brainwave se integra con Azure Machine Learning. Microsoft usa las FPGA para la evaluación de DNN, la clasificación de búsquedas de Bing y la aceleración de redes definidas por software (SDN) para reducir la latencia y liberar las CPU para otras tareas.

Los escenarios siguientes usan FPGA en la arquitectura de Project Brainwave:
+ [Sistema automatizado de inspección óptica](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Asignación de uso de terrenos](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)

## <a name="deploy-to-fpgas-on-azure"></a>Implementación en FPGA en Azure

Este es el flujo de trabajo para crear un servicio de reconocimiento de imágenes en Azure usando las DNN compatibles como un caracterizador para la implementación en FPGA de Azure:

1. Use el SDK de Azure Machine Learning para Python para crear una definición de servicio, que es un archivo que describe una canalización de gráficos (entrada, caracterizador y clasificador) basado en TensorFlow. El comando de implementación comprimirá automáticamente la definición y los grafos en un archivo ZIP, que cargará en Azure Blog Storage.  La DNN ya está implementada en Project Brainwave para ejecutarse en la FPGA.

1. Registre el modelo mediante el SDK con el archivo ZIP en Azure Blob Storage.

1. Implemente el servicio con el modelo registrado mediante el SDK.

Puede empezar a implementar los modelos de DNN entrenados en las FPGA en Azure Cloud con este artículo, **"[Implementación de un modelo como servicio web en una FPGA](how-to-deploy-fpga-web-service.md)"**.


## <a name="next-steps"></a>Pasos siguientes

Consulte estos vídeos y blogs:

+ [Hyperscale hardware: ML at scale on top of Azure + FPGA : Build 2018 (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY) (Hardware de hiperescala: ML a escala sobre Azure + FPGA: compilación 2018 [vídeo])

+ [Inside the Microsoft FPGA-based configurable cloud (video)](https://channel9.msdn.com/Events/Build/2017/B8063) (Dentro de la nube configurable basada en FPGA de Microsoft [vídeo])

+ [Project Brainwave for real-time AI](https://www.microsoft.com/research/project/project-brainwave/) (Project Brainwave para IA en tiempo real): página principal del proyecto
