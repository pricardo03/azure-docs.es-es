---
title: Creación de áreas de trabajo de Azure Machine Learning en el portal
titleSuffix: Azure Machine Learning
description: Aprenda a crear, ver y eliminar áreas de trabajo de Azure Machine Learning en Azure Portal.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 05/10/2019
ms.custom: seodec18
ms.openlocfilehash: 511c737e160c0f0753e570314c9b29346972cb04
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "71269266"
---
# <a name="create-and-manage-azure-machine-learning-workspaces-in-the-azure-portal"></a>Creación y administración de áreas de trabajo de Azure Machine Learning en Azure Portal

En este artículo creará, verá y eliminará [**áreas de trabajo de Azure Machine Learning**](concept-workspace.md) en Azure Portal para [Azure Machine Learning](overview-what-is-azure-ml.md).  El portal es la forma más sencilla de empezar a trabajar con áreas de trabajo, pero a medida que cambian las necesidades o aumentan los requisitos de automatización, también puede crear y eliminar áreas de trabajo [mediante la CLI](reference-azure-machine-learning-cli.md), [con código de Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) o [a través de la extensión de VS Code](how-to-vscode-tools.md#get-started-with-azure-machine-learning-for-visual-studio-code).

## <a name="create-a-workspace"></a>Crear un área de trabajo

Para crear un área de trabajo, necesita una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

### <a name="download-a-configuration-file"></a>Descarga de un archivo de configuración

1. Si va a crear una [máquina virtual de Notebook](tutorial-1st-experiment-sdk-setup.md#azure), omita este paso.

1. Si tiene previsto usar código en el entorno local que haga referencia a esta área de trabajo, seleccione **Descargar config.json** en la sección **Información general** del área de trabajo.  

   ![Descargar config.json](./media/how-to-manage-workspace/configure.png)
   
   Coloque el archivo en la estructura de directorios que contiene los scripts de Python o las instancias de Jupyter Notebook. Puede estar en el mismo directorio, en un subdirectorio denominado *.azureml* o en un directorio principal. Al crear una máquina virtual de Notebook, este archivo se agrega al directorio correcto de la máquina virtual.


## <a name="view"></a>Vista de un área de trabajo

1. En la esquina superior izquierda del portal, seleccione **Todos los servicios**.

1. En el campo de filtro **Todos los servicios**, escriba **servicio Machine Learning**.  

1. Seleccione **Áreas de trabajo del servicio Machine Learning**.

   ![Búsqueda del área de trabajo de Azure Machine Learning](media/how-to-manage-workspace/all-services.png)

1. Examine la lista de áreas de trabajo encontradas. Puede filtrar en función de suscripción, grupos de recursos y ubicaciones.  

1. Seleccione un área de trabajo para mostrar sus propiedades.
   ![Propiedades del área de trabajo](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Eliminar un área de trabajo

Use el botón Eliminar en la parte superior del área de trabajo que desea eliminar.

  ![Botón Eliminar](media/how-to-manage-workspace/delete-workspace.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

Siga el tutorial completo para obtener información sobre cómo usar un área de trabajo para compilar, entrenar e implementar modelos con Azure Machine Learning.

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento de modelos](tutorial-train-models-with-aml.md)
