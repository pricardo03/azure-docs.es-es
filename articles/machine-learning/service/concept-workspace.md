---
title: Qué es un área de trabajo
titleSuffix: Azure Machine Learning
description: El área de trabajo es el recurso de nivel superior de Azure Machine Learning. Conserva un historial de todas las ejecuciones del entrenamiento, que incluye registros, métricas, resultados y una instantánea de sus scripts. Esta información se usa para determinar qué ejecución de entrenamiento crea el mejor modelo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 08/06/2019
ms.openlocfilehash: de215502f1ecb55bc331f29057a4f7c3f30b0132
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71720173"
---
# <a name="what-is-an-azure-machine-learning-workspace"></a>¿Qué es un área de trabajo de Azure Machine Learning?

El área de trabajo es el recurso de nivel superior para Azure Machine Learning, que proporciona un lugar centralizado para trabajar con todos los artefactos que crea al usar Azure Machine Learning.  El área de trabajo conserva un historial de las ejecuciones del entrenamiento, que incluye registros, métricas, resultados y una instantánea de sus scripts. Esta información se usa para determinar qué ejecución de entrenamiento crea el mejor modelo.  

Una vez que tenga un modelo que le guste, regístrelo con el área de trabajo. Después, usará el modelo registrado y los scripts de puntuación para implementar en Azure Container Instances, en Azure Kubernetes Service o en una matriz de puerta programable en el campo (FPGA) como un punto de conexión HTTP basado en REST. También puede implementar el modelo en un dispositivo de Azure IoT Edge como un módulo.

## <a name="taxonomy"></a>Taxonomía 

El diagrama siguiente es una taxonomía del área de trabajo:

[![Taxonomía del área de trabajo](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

En el diagrama se muestran los siguientes componentes de un área de trabajo:

+ Un área de trabajo puede incluir [máquinas virtuales de Notebook](tutorial-1st-experiment-sdk-setup.md), recursos en la nube configurados con el entorno de Python necesario para ejecutar Azure Machine Learning.
+ Los [roles de usuario](how-to-assign-roles.md) le permiten compartir su área de trabajo con otros usuarios, equipos o proyectos.
+ Los [destinos de proceso](concept-azure-machine-learning-architecture.md#compute-targets) se usan para ejecutar sus experimentos.
+ Al crear el área de trabajo, los [recursos asociados](#resources) también se crean automáticamente.
+ Los [experimentos](concept-azure-machine-learning-architecture.md#experiments) son ejecuciones de entrenamiento que usa para entrenar sus modelos.  Puede crear y ejecutar experimentos con
    + El [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
    + La sección de [experimentos de aprendizaje automático automatizados (versión preliminar)](how-to-create-portal-experiments.md) de Azure Portal o de la página de aterrizaje del área de trabajo (versión preliminar).
    + La [interfaz visual (versión preliminar)](ui-concept-visual-interface.md).
+ Las [canalizaciones](concept-azure-machine-learning-architecture.md#ml-pipelines) son flujos de trabajo reutilizables para entrenar el modelo y repetir dicho proceso.
+ Los [conjuntos de datos](concept-azure-machine-learning-architecture.md#datasets-and-datastores) ayudan en la administración de los datos que usa para el entrenamiento del modelo y la creación de canalizaciones.
+ Una vez que tenga un modelo que desee implementar, cree un modelo registrado.
+ Use el modelo registrado y un script de puntuación para crear una [implementación](concept-azure-machine-learning-architecture.md#deployment).

## <a name="tools-for-workspace-interaction"></a>Herramientas para la interacción con el área de trabajo

Puede interactuar con el área de trabajo de las siguientes formas:

+ En la Web:
    + [Azure Portal](https://portal.azure.com)
    + La [página de aterrizaje del área de trabajo (versión preliminar)](https://ml.azure.com)
    + La [interfaz visual (versión preliminar)](ui-concept-visual-interface.md)
+ En Python con el [SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) de Azure Machine Learning
+ En la línea de comandos con la [extensión de la CLI](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli) de Azure Machine Learning

## <a name="machine-learning-with-a-workspace"></a>Aprendizaje automático con un área de trabajo

Las tareas de aprendizaje automático leen o escriben artefactos en el área de trabajo. 

+ Ejecute un experimento para entrenar un modelo: escribe los resultados de la ejecución del experimento en el área de trabajo.
+ Use Machine Learning automatizado para entrenar un modelo: escribe los resultados del entrenamiento en el área de trabajo.
+ Registre un modelo en el área de trabajo.
+ Implemente un modelo: usa el modelo registrado para crear una implementación.
+ Cree y ejecute flujos de trabajo reutilizables.
+ Vea artefactos de aprendizaje automático como experimentos, canalizaciones, modelos o implementaciones.
+ Realice un seguimiento de los modelos y supervíselos.

## <a name="workspace-management"></a>Administración de áreas de trabajo

También puede realizar las siguientes tareas de administración de áreas de trabajo:

| Tarea de administración de áreas de trabajo   | Portal              | SDK        | CLI        |
|---------------------------|------------------|------------|------------|
| Crear un área de trabajo        | **&check;**     | **&check;** | **&check;** |
| Creación y administración de recursos de proceso    | **&check;**   | **&check;** |  **&check;**   |
| Administración del acceso al área de trabajo    | **&check;**   | |  **&check;**    |
| Creación de una máquina virtual de Notebook | **&check;**   | |     |

## <a name='create-workspace'></a> Creación de un área de trabajo

Hay varias maneras de crear un área de trabajo.

* Use [Azure Portal](how-to-manage-workspace.md) si quiere utilizar una interfaz de apuntar y hacer clic que le guíe por cada paso.
* Use el [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace) si quiere crear un área de trabajo al momento a partir de scripts de Python o cuadernos de Jupyter.
* Use una [plantilla de Azure Resource Manager](how-to-create-workspace-template.md) o la [CLI de Azure Machine Learning](reference-azure-machine-learning-cli.md) cuando necesite automatizar o personalizar la creación con estándares de seguridad corporativos.
* Si trabaja en Visual Studio Code, use la [extensión de VS Code](how-to-vscode-tools.md#get-started-with-azure-machine-learning-for-visual-studio-code).

## <a name="resources"></a> Recursos asociados

Al crear una nueva área de trabajo, se crean automáticamente varios recursos de Azure que el área de trabajo utiliza:

+ [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): registra los contenedores de docker que usa durante el entrenamiento y al implementar un modelo. Para reducir los costos, ACR se **carga de forma diferida** hasta que se crean imágenes de la implementación.
+ [Cuenta de Azure Storage](https://azure.microsoft.com/services/storage/): se usa como almacén de datos predeterminado para el área de trabajo.  Las instancias de Jupyter Notebook que se usan con sus máquinas virtuales de Notebook se almacenan aquí también.
+ [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): almacena información sobre la supervisión de los modelos.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): almacena secretos que usan los destinos de proceso y otra información confidencial que el área de trabajo necesita.

> [!NOTE]
> Además de crear nuevas versiones, también puede usar los servicios de Azure existentes.

## <a name="next-steps"></a>Pasos siguientes

Para una introducción a Azure Machine Learning, consulte:

+ [Información general de Azure Machine Learning](overview-what-is-azure-ml.md)
+ [Creación de un área de trabajo](how-to-manage-workspace.md)
+ [Administración de un área de trabajo](how-to-manage-workspace.md)
+ [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md)
