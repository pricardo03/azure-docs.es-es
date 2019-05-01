---
title: Crear y administrar áreas de trabajo
titleSuffix: Azure Machine Learning service
description: Aprenda a crear, ver y eliminar áreas de trabajo del servicio Azure Machine Learning en Azure Portal.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: e65f739a9641181381205c7255d0472325e8055c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60819637"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Creación y administración de áreas de trabajo del servicio Azure Machine Learning

En este artículo creará, verá y eliminará [**áreas de trabajo del servicio Azure Machine Learning**](concept-azure-machine-learning-architecture.md#workspace) en Azure Portal para el [servicio Azure Machine Learning](overview-what-is-azure-ml.md).  También puede crear y eliminar áreas de trabajo [mediante la CLI](reference-azure-machine-learning-cli.md) o [con código de Python](https://aka.ms/aml-sdk).

## <a name="create-a-workspace"></a>Crear un área de trabajo 

Para crear un área de trabajo, necesita una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view"></a>Ver un área de trabajo

1. En la esquina superior izquierda del portal, seleccione **Todos los servicios**. 

1. En el **todos los servicios** campo de filtro, escriba **service de aprendizaje automático**.  

1. Seleccione **las áreas de trabajo del servicio de Machine Learning**.

   ![Busque el área de trabajo de servicio de Azure Machine Learning](media/how-to-manage-workspace/all-services.png)

1. Examine la lista de áreas de trabajo encontradas. Puede filtrar en función de suscripción, grupos de recursos y ubicaciones.  

1. Seleccione un área de trabajo para mostrar sus propiedades.
   ![Propiedades del área de trabajo](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Eliminar un área de trabajo

Use el botón Eliminar en la parte superior del área de trabajo que desea eliminar.

  ![Botón Eliminar](media/how-to-manage-workspace/delete-workspace.png)


## <a name="clean-up-resources"></a>Limpieza de recursos 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

Siga el tutorial completo para obtener información sobre cómo usar un área de trabajo para compilar, entrenar e implementar modelos con el servicio Azure Machine Learning.

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento de modelos](tutorial-train-models-with-aml.md)
