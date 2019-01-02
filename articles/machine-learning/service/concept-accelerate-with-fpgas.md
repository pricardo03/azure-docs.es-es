---
title: ¿Qué son las matrices de puertas programables (FPGA) y Project Brainwave?
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo acelerar modelos y redes neurales profundas con FPGA en Azure. En este artículo se proporciona una introducción a las matrices de puertas programables (FPGA) y cómo el servicio Azure Machine Learning ofrece inteligencia artificial (IA) en tiempo real al implementar el modelo en una FPGA de Azure.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: tedway
author: tedway
ms.reviewer: jmartens
ms.date: 10/24/2018
ms.custom: seodec18
ms.openlocfilehash: bc08025f070fb31d83fed26bfec00cec11cee061
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53313639"
---
# <a name="what-are-fpgas-and-project-brainwave"></a>¿Qué son las matrices de puertas programables (FPGA) y Project Brainwave?

En este artículo se proporciona una introducción a las matrices de puertas programables (FPGA) y cómo el servicio Azure Machine Learning ofrece inteligencia artificial (IA) en tiempo real al implementar el modelo en una FPGA de Azure.

Las FPGA contienen una matriz de bloques de lógica programables y una jerarquía de interconexiones reconfigurables. Las interconexiones permiten que estos bloques se configuren de distintas maneras después de su fabricación. Las FPGA ofrecen una combinación de capacidad de programación y rendimiento en comparación con otros chips.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA en comparación con CPU, GPU y ASIC

El diagrama y la tabla siguientes muestran cómo se comparan las FPGA con otros procesadores.

![Diagrama de comparación de FPGA del servicio Azure Machine Learning](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|Procesador||DESCRIPCIÓN|
|---|:-------:|------|
|Circuitos integrados específicos de la aplicación|ASIC|Los circuitos personalizados, como las unidades de procesador TensorFlow (TPU) de Google, proporcionan la máxima eficacia. No pueden reconfigurarse según las necesidades.|
|Matrices de puertas programables por campo|FPGA|Las FPGA como, por ejemplo, las disponibles en Azure, proporcionan un rendimiento similar al de los ASIC. También son flexibles y se pueden volver a configurar con el tiempo, para implementar una lógica nueva.|
|Unidades de procesamiento gráfico|GPU|Una opción popular para los cálculos de inteligencia artificial. GPU ofrece funcionalidades de procesamiento en paralelo, lo que permite una representación de imágenes más rápida que las CPU.|
|Unidades centrales de procesamiento|CPU|Procesadores de uso general cuyo rendimiento no es óptimo para el procesamiento de vídeo y gráficos.|

## <a name="project-brainwave-on-azure"></a>Project Brainwave en Azure

[Project Brainwave](https://www.microsoft.com/research/project/project-brainwave/) es una arquitectura de hardware de Microsoft. Se basa en los dispositivos de FPGA de Intel, cuyos datos usan los científicos de datos y los desarrolladores para acelerar los cálculos de inteligencia artificial en tiempo real. Esta arquitectura habilitada para FPGA ofrece rendimiento, flexibilidad y escalado, y está disponible en Azure.

Las FPGA permiten alcanzar una latencia baja para las solicitudes de inferencia en tiempo real. Las solicitudes asincrónicas (procesamiento por lotes) no son necesarias. El procesamiento por lotes puede causar latencia porque se deben procesar más datos. Las implementaciones de Project Brainwave de unidades de procesamiento neuronal no requieren procesamiento por lotes; por lo tanto, la latencia puede ser muchas veces menor en comparación con los procesadores de CPU y GPU.

### <a name="reconfigurable-power"></a>Capacidad reconfigurable
Puede volver a configurar las FPGA para distintos tipos de modelos de aprendizaje automático. Esta flexibilidad facilita la aceleración de las aplicaciones en función del modelo de memoria y la precisión numérica más óptima que se usa. Como las FPGA se pueden volver a configurar, puede mantenerse al día con los requisitos de los algoritmos de inteligencia artificial que están en constante evolución.

### <a name="whats-supported-on-azure"></a>Qué se admite en Azure
Microsoft Azure es la mayor inversión en la nube del mundo en FPGA. Puede ejecutar Project Brainwave en la infraestructura de escala de Azure.

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

Para crear un servicio de reconocimiento de imágenes en Azure, puede usar las DNN compatibles como un caracterizador para la implementación en FPGA de Azure:

1. Use el [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) para crear una definición de servicio. Una definición de servicio es un archivo que describe una canalización de grafos (entrada, caracterizador y clasificador) basado en TensorFlow. El comando de implementación comprime automáticamente la definición y los grafos en un archivo ZIP, que cargará en Azure Blog Storage. La DNN ya está implementada en Project Brainwave para ejecutarse en la FPGA.

1. Registre el modelo mediante el SDK con el archivo ZIP en Azure Blob Storage.

1. Implemente el servicio con el modelo registrado mediante el SDK.

Para empezar a implementar los modelos de DNN entrenados en las FPGA en Azure Cloud, consulte, [Implementación de un modelo como servicio web en una FPGA](how-to-deploy-fpga-web-service.md).


## <a name="next-steps"></a>Pasos siguientes

Consulte estos vídeos y blogs:

+ [Hyperscale hardware: ML at scale on top of Azure + FPGA : Build 2018 (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY) (Hardware de hiperescala: ML a escala sobre Azure + FPGA: compilación 2018 [vídeo])

+ [Inside the Microsoft FPGA-based configurable cloud (video)](https://channel9.msdn.com/Events/Build/2017/B8063) (Dentro de la nube configurable basada en FPGA de Microsoft [vídeo])

+ [Project Brainwave for real-time AI](https://www.microsoft.com/research/project/project-brainwave/) (Project Brainwave para IA en tiempo real): página principal del proyecto
