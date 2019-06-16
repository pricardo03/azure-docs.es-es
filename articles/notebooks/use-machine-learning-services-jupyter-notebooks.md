---
title: Uso de Azure Machine Learning Services en Azure Notebooks
description: Información general de los cuadernos de ejemplo para Azure Machine Learning Services que puede usar con Azure Notebooks.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 0dc4fc31-ae1c-422c-ac34-7b025e6651b4
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 2ef327721fd42e5274381834721fd987ec7e9d75
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60240503"
---
# <a name="use-azure-machine-learning-service-in-a-notebook"></a>Uso de Azure Machine Learning Service en un cuaderno

Azure Notebooks viene preconfigurado con el entorno necesario para que funcione con [Azure Machine Learning Service](/azure/machine-learning/service/). Puede clonar fácilmente un proyecto de ejemplo en su cuenta de Notebooks para explorar una variedad de escenarios de Machine Learning.

## <a name="clone-the-sample-into-your-account"></a>Clonación del ejemplo en su cuenta

1. Inicie sesión en [Azure Notebooks](https://notebooks.azure.com/).
1. Seleccione **Mis proyectos** para navegar hasta el panel de proyectos.
1. Seleccione el botón **Cargar repositorio de GitHub** (la flecha arriba) para abrir la ventana emergente **Upload GitHub Repository** (Cargar repositorio de GitHub).
1. En la ventana emergente, escriba `Azure/MachineLearningNotebooks` en **Repositorio de GitHub**, proporcione un nombre para el proyecto en **Nombre del proyecto**, como "Azure Machine Learning Service", proporcione un identificador en **Id. de proyecto**, desactive **Público** si lo desea y seleccione **Importar**.

    ![Importar el ejemplo de Azure Machine Learning en su cuenta de Notebooks](media/azureml-import-project.png)

1. Después de un minuto o dos, Azure Notebooks le llevará automáticamente al panel del nuevo proyecto.

## <a name="run-a-sample-notebook"></a>Ejecución de un cuaderno de ejemplo

1. Seleccione **00 - configuration.ipynb** para iniciar la sección de configuración del cuaderno y siga las instrucciones para crear un área de trabajo de Azure Machine Learning.

    - Dado que Azure Notebooks ya contiene los paquetes de Python necesarios, puede ejecutar simplemente el fragmento de código del paso 2 de los requisitos previos para verificar la versión del SDK de Azure ML.

1. Una vez completada la configuración, seleccione **01.getting-started** para navegar a la carpeta que contiene trece cuadernos de ejemplo diferentes, cada uno de ellos se explica por sí solo.

## <a name="next-steps"></a>Pasos siguientes

La documentación de Azure Machine Learning Services contiene una variedad de otros recursos que le guían a través del trabajo con Machine Learning Services dentro de cuadernos:

- [Guía de inicio rápido: Uso de Python para empezar a usar Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/quickstart-create-workspace-with-python)
- [Tutorial 1: Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml)
- [Tutorial 2: Implementación de un modelo de clasificación de imágenes en Azure Container Instances (ACI)](https://docs.microsoft.com/azure/machine-learning/service/tutorial-deploy-models-with-aml)
- [Tutorial: Entrenamiento de un modelo de clasificación con aprendizaje automático en Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/tutorial-auto-train-models)

Consulte también la documentación del [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
