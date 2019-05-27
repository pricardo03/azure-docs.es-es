---
title: ¿Qué es un área de trabajo
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre qué un área de trabajo y por qué necesita uno para el servicio Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/21/2019
ms.openlocfilehash: 5785bf8f6538f1d91e7a23178e29487ebee14f29
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989830"
---
# <a name="what-is-an-azure-machine-learning-workspace"></a>¿Qué es un área de trabajo de Azure Machine Learning?

El área de trabajo es el recurso de nivel superior de Azure Machine Learning Service. Proporciona un lugar centralizado para trabajar con todos los artefactos que cree al usar Azure Machine Learning Service.

El área de trabajo mantiene un historial de las ejecuciones de entrenamiento, incluidos los registros, métricas, salida y una instantánea de las secuencias de comandos. Esta información se usa para determinar qué ejecución de entrenamiento crea el mejor modelo.  

Una vez que un modelo que desee, regístrelo con el área de trabajo. Utilice el modelo registrado y la puntuación de secuencias de comandos para implementar en Azure Container Instances, Azure Kubernetes Service, o en una matriz de puerta programable de campo (FPGA) como un punto de conexión HTTP basado en REST. También puede implementar el modelo en un dispositivo Azure IoT Edge como un módulo.

## <a name="taxonomy"></a>Taxonomía 

El diagrama siguiente es una taxonomía del área de trabajo:

[![Taxonomía del área de trabajo](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

El diagrama muestra los siguientes componentes de un área de trabajo:

+ Un área de trabajo puede contener [máquinas virtuales de Bloc de notas](quickstart-run-cloud-notebook.md), configurados con el entorno de Python necesario para ejecutar Azure Machine Learning de recursos en la nube.
+ [Roles de usuario](how-to-assign-roles.md) le permite compartir el área de trabajo con otros usuarios, equipos o proyectos.
+ [Los destinos de proceso](concept-azure-machine-learning-architecture.md#compute-target) se utilizan para ejecutar sus experimentos.
+ Cuando se crea el área de trabajo, [recursos asociados](#resources) también se crean automáticamente.
+ [Experimentos](concept-azure-machine-learning-architecture.md#experiment) se muestran las ejecuciones de entrenamiento se utiliza para compilar los modelos.  Puede crear y ejecutar experimentos con
    + El [de Azure Machine Learning SDK para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
    + El [automatizada experimentos de machine learning (versión preliminar)](how-to-create-portal-experiments.md) sección en el portal de Azure.
    + El [interfaz visual (versión preliminar)](ui-concept-visual-interface.md).
+ [Las canalizaciones](concept-azure-machine-learning-architecture.md#pipeline) son flujos de trabajo reutilizables para entrenamiento y el reciclaje del modelo.
+ [Los conjuntos de datos](concept-azure-machine-learning-architecture.md#dataset) ayuda en la administración de los datos utilizados para la creación del modelo entrenamiento y la canalización.
+ Una vez que un modelo que desea implementar, se crea un [modelo registrado](concept-azure-machine-learning-architecture.md#model-registry).
+ Usar el modelo registrado y un script de puntuación para crear un [implementación](concept-azure-machine-learning-architecture.md#image-registry).

## <a name="tools-for-workspace-interaction"></a>Herramientas para la interacción del área de trabajo

Puede interactuar con el área de trabajo de las maneras siguientes:

+ En la web:
    + [Azure Portal](https://azure.portal.com)
    + El [interfaz visual (versión preliminar)](ui-concept-visual-interface.md)
+ En Python con Azure Machine Learning [SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
+ En la línea de comandos mediante el Azure Machine Learning [extensión de la CLI](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli)

## <a name="machine-learning-with-a-workspace"></a>Aprendizaje automático con un área de trabajo

Tareas de aprendizaje automático leer o escriben los artefactos en el área de trabajo. 

+ Ejecutar un experimento para entrenar un modelo: escrituras experimentar resultados de la ejecución al área de trabajo.
+ Use automatizada ML para entrenar un modelo - escribe los resultados del entrenamiento en el área de trabajo.
+ Registrar un modelo en el área de trabajo.
+ Implementar un modelo: utiliza el modelo registrado para crear una implementación.
+ Crear y ejecutar flujos de trabajo reutilizables.
+ Máquina de vista artefactos, como experimentos, canalizaciones, modelos, las implementaciones de aprendizaje.
+ Modelos de seguimiento y control.

## <a name="workspace-management"></a>Administración del área de trabajo

También puede realizar las siguientes tareas de administración del área de trabajo:

| Tarea de administración del área de trabajo   | Portal              | SDK        | CLI        |
|---------------------------|------------------|------------|------------|
| Crear un área de trabajo        | **&check;**     | **&check;** | **&check;** |
| Crear y administrar recursos de proceso    | **&check;**   | **&check;** |  **&check;**   |
| Administrar el acceso de área de trabajo    | **&check;**   | |  **&check;**    |
| Creación de una máquina virtual de Notebook | **&check;**   | |     |

Empezar a trabajar con el servicio de forma [crear un área de trabajo](setup-create-workspace.md).

## <a name="resources"></a> Recursos asociados

Al crear una nueva área de trabajo, se crean automáticamente varios recursos de Azure que el área de trabajo utiliza:

+ [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): registra los contenedores de docker que usa durante el entrenamiento y al implementar un modelo. Para minimizar los costos, ACR es **carga diferida** hasta que se crean las imágenes de implementación.
+ [Cuenta de Azure Storage](https://azure.microsoft.com/services/storage/): se usa como almacén de datos predeterminado para el área de trabajo.
+ [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): almacena información sobre la supervisión de los modelos.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): almacena secretos que usan los destinos de proceso y otra información confidencial que el área de trabajo necesita.

> [!NOTE]
> Además de crear nuevas versiones, también puede usar los servicios de Azure existentes.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar a usar Azure Machine Learning Service, consulte:

+ [Información general del servicio de Azure Machine Learning](overview-what-is-azure-ml.md)
+ [Creación de un área de trabajo](setup-create-workspace.md)
+ [Administración de un área de trabajo](how-to-manage-workspace.md)
+ [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md)
