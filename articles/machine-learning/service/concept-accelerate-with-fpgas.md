---
title: ¿Qué es una FPGA? – Project Brainwave – Azure Machine Learning
description: Obtenga información sobre cómo acelerar modelos y redes neurales profundas con FPGA.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/31/2018
ms.openlocfilehash: 5c8efcbb5f2e9014c9edabfc2aee37c8c29ea0b4
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "35638396"
---
# <a name="what-is-fpga-and-project-brainwave"></a>¿Qué es FPGA y Project Brainwave?

En este artículo se proporciona una introducción a las matrices de puertas programables en campo (FPGA) y cómo se integran con Azure Machine Learning para brindar IA en tiempo real.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA en comparación con CPU, GPU y ASIC

Las FPGA contienen una matriz de bloques lógicos programables y una jerarquía de interconexiones reconfigurables que permiten que los bloques se configuren de distintas maneras después de la fabricación.

Las FPGA entregan una combinación de capacidad de programación y rendimiento en comparación con otros chips:

![Comparación de FPGA de Azure Machine Learning](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

- Las **unidades centrales de procesamiento (CPU)** son procesadores de uso general. El rendimiento de la CPU no es el recomendado para el procesamiento de vídeo y gráficos.
- Las **unidades de procesamiento de gráficos (GPU)** ofrecen procesamiento paralelo y son una opción popular para los cálculos de IA. El procesamiento paralelo con las GPU genera una representación de imágenes más rápida que con las CPU.
- Los **circuitos integrados específicos de aplicación (ASIC)**, como las unidades de procesador de TensorFlow de Google, son circuitos personalizados. Si bien estos chips brindan la máxima eficacia, los ASIC no son flexibles.
- Las **FPGA**, como las disponibles en Azure, proporcionan un rendimiento similar al de los ASIC, pero ofrecen la flexibilidad para volver a configurarlos más adelante.

Las FPGA ahora están en cada servidor nuevo de Azure. Microsoft ya usa las FPGA para la clasificación de Bing Search, la evaluación de red neuronal profunda (DNN) y la aceleración de redes definidas por software (SDN). Estas implementaciones de FPGA reducen la latencia a la vez que liberan las CPU para otras tareas.

## <a name="project-brainwave-on-azure"></a>Project Brainwave en Azure

Project Brainwave es una arquitectura de hardware diseñada en función de los dispositivos de FPGA de Intel y que se usa para acelerar los cálculos de IA en tiempo real. Con esta arquitectura habilitada para FPGA económica, una red neuronal entrenada se puede ejecutar tan rápido como sea posible y con una menor latencia. Project Brainwave puede paralelizar DNN entradas previamente entre las FPGA para realizar el escalado horizontal del servicio.

- Rendimiento

    Las FPGA permiten alcanzar una latencia baja para las solicitudes de inferencia en tiempo real. El procesamiento por lotes significa dividir una solicitud en partes más pequeñas e insertarlas en un procesador para mejorar la utilización del hardware. El procesamiento por lotes no es eficaz y puede generar latencia. Brainwave no requiere procesamiento por lotes, por lo que la latencia es 10 veces menor en comparación con la CPU y la GPU.

- Flexibilidad

    Las FPGA se pueden volver a configurar para distintos tipos de modelos de aprendizaje automático. Esta flexibilidad facilita la aceleración de las aplicaciones en función del modelo de memoria y la precisión numérica más óptima que se usa.

    Se desarrollan técnicas de aprendizaje automático nuevas de manera regular y el diseño de hardware de Project Brainwave también evoluciona rápidamente. Como las FPGA se pueden volver a configurar, es posible mantenerse al día con los requisitos de cambiar rápidamente los algoritmos de IA.

- Escala

    Microsoft Azure es la mayor inversión en la nube del mundo en FPGA. Puede ejecutar Brainwave en la infraestructura de escala de Azure.

Actualmente hay disponible una versión preliminar de Project Brainwave integrada con Azure Machine Learning. También hay disponible una versión preliminar limitada para poner a Project Brainwave a la vanguardia, de manera que pueda aprovechar esa velocidad de cálculo en sus propias empresas e instalaciones.

En la versión preliminar actual, Brainwave se limita a la implementación de TensorFlow y a las redes neuronales basadas en ResNet50 en hardware de FPGA de Intel para la clasificación y el reconocimiento de imágenes. Existen planes para admitir más modelos de galerías y otros marcos.

Los escenarios siguientes usan FPGA en la arquitectura de Project Brainwave:

- Sistema automatizado de inspección óptica. Consulte [Real-time AI: Microsoft announces preview of Project Brainwave](https://blogs.microsoft.com/ai/build-2018-project-brainwave/) (IA en tiempo real: Microsoft anuncia la versión preliminar de Project Brainwave).
- Asignación de uso de terrenos. Consulte [How to Use FPGAs for Deep Learning Inference to Perform Land Cover Mapping on Terabytes of Aerial Images](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/) (Cómo usar las FPGA de inferencia de aprendizaje profundo para realizar la asignación de uso de terrenos en terabytes de imágenes aéreas).

## <a name="how-to-deploy-a-web-service-to-an-fpga"></a>¿Cómo implementar un servicio web en una FPGA?

El flujo de alto nivel para crear un servicio de reconocimiento de imágenes en Azure con ResNet50 como caracterizador es el siguiente:

1. ResNet50 ya está implementado en Brainwave. Puede compilar otros grafos (entrada de fecha, clasificación, etc.) con TensorFlow y definir una canalización (ingresar -> caracterizar -> clasificar) mediante un archivo JSON de definición de servicio. Comprima la definición y los grafos en un archivo ZIP y cargue este archivo en Azure Blog Storage.
2. Registre el modelo con la API de Administración de modelos de Azure Machine Learning con el archivo ZIP en Blob Storage.
3. Implemente el servicio con el modelo registrado mediante la API de Administración de modelos de Azure Machine Learning.

Obtenga más información sobre este proceso en el artículo [Implementación de un modelo como servicio web en una FPGA con Azure Machine Learning](how-to-deploy-fpga-web-service.md).


## <a name="start-using-fpga"></a>Empezar a usar FPGA

Los modelos acelerados mediante hardware de Azure Machine Learning permiten implementar modelos de DNN entrenados en las FPGA de la nube de Azure. Para empezar, consulte:

- [Implementación de un modelo como un servicio web en una FPGA](how-to-deploy-fpga-web-service.md)
- [Microsoft Azure Machine Learning Hardware Accelerated Models Powered by Project Brainwave](https://github.com/azure/aml-real-time-ai) (Modelos acelerados mediante hardware de Microsoft Azure Machine Learning con tecnología de Project Brainwave).

## <a name="next-steps"></a>Pasos siguientes

[Implementación de un modelo como un servicio web en una FPGA](how-to-deploy-fpga-web-service.md)

[Más información sobre cómo instalar el SDK de FPGA](reference-fpga-package-overview.md)

[Hyperscale hardware: ML at scale on top of Azure + FPGA : Build 2018 (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY) (Hardware de hiperescala: ML a escala sobre Azure + FPGA: compilación 2018 [vídeo])

[Inside the Microsoft FPGA-based configurable cloud (video)](https://channel9.msdn.com/Events/Build/2017/B8063) (Dentro de la nube configurable basada en FPGA de Microsoft [vídeo])

[Microsoft unveils Project Brainwave for real-time AI](https://www.microsoft.com/research/blog/microsoft-unveils-project-brainwave/) (Microsoft presenta Project Brainwave para IA en tiempo real)