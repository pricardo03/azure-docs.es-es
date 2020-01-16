---
title: Uso de Azure Machine Learning en Azure Notebooks Preview
description: Información general de los cuadernos de ejemplo de Azure Machine Learning que puede usar con Azure Notebooks Preview.
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: 435abca83255221d438d530b63c237c08bb0b672
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2020
ms.locfileid: "75860578"
---
# <a name="use-azure-machine-learning-in-azure-notebooks-preview"></a>Uso de Azure Machine Learning en Azure Notebooks Preview

Azure Notebooks viene preconfigurado con el entorno necesario para que funcione con [Azure Machine Learning](/azure/machine-learning/). Puede clonar fácilmente un proyecto de ejemplo en su cuenta de Notebooks para explorar una variedad de escenarios de Machine Learning.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="clone-the-sample-into-your-account"></a>Clonación del ejemplo en su cuenta

1. Inicie sesión en [Azure Notebooks](https://notebooks.azure.com/).
1. Seleccione **Mis proyectos** para ir al panel de proyectos.
1. Seleccione el botón **Cargar repositorio de GitHub** (la flecha arriba) para abrir la ventana emergente **Upload GitHub Repository** (Cargar repositorio de GitHub).
1. En la ventana emergente, escriba `Azure/MachineLearningNotebooks` en **Repositorio de GitHub**, proporcione un nombre para el proyecto en **Nombre del proyecto**, como "Azure Machine Learning", proporcione un identificador en **Id. de proyecto**, desactive **Público** si lo desea y seleccione **Importar**.

    ![Importar el ejemplo de Azure Machine Learning en su cuenta de Notebooks](media/azureml-import-project.png)

1. Después de un minuto o dos, Azure Notebooks le llevará automáticamente al panel del nuevo proyecto.

## <a name="run-a-sample-notebook"></a>Ejecución de un cuaderno de ejemplo

1. Seleccione **00 - configuration.ipynb** para iniciar la sección de configuración del cuaderno y siga las instrucciones para crear un área de trabajo de Azure Machine Learning.

    - Dado que Azure Notebooks ya contiene los paquetes de Python necesarios, puede ejecutar simplemente el fragmento de código del paso 2 de los requisitos previos para verificar la versión del SDK de Azure ML.

1. Una vez completada la configuración, seleccione **01.getting-started** para abrir la carpeta que contiene trece cuadernos de ejemplo diferentes, cada uno de ellos se explica por sí solo.

## <a name="next-steps"></a>Pasos siguientes

La documentación de Azure Machine Learning contiene una variedad de otros recursos que le guían a través del trabajo con Machine Learning dentro de cuadernos:

- [Inicio rápido: Uso de Python para empezar a usar Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/how-to-configure-environment#local)
- [Tutorial 1: Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/tutorial-train-models-with-aml)
- [Tutorial 2: Implementación de un modelo de clasificación de imágenes en Azure Container Instances (ACI)](https://docs.microsoft.com/azure/machine-learning/tutorial-deploy-models-with-aml)
- [Tutorial: Entrenamiento de un modelo de clasificación con aprendizaje automático automatizado en Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/tutorial-auto-train-models)

Consulte también la documentación del [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
