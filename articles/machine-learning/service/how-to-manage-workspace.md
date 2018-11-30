---
title: Creación y administración de áreas de trabajo del servicio Azure Machine Learning
description: Aprenda a crear, ver y eliminar áreas de trabajo del servicio Azure Machine Learning en Azure Portal.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.openlocfilehash: 1565c54779278b440cfe631951e964921cc85720
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51709889"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Creación y administración de áreas de trabajo del servicio Azure Machine Learning

En este artículo creará, verá y eliminará [**áreas de trabajo del servicio Azure Machine Learning**](concept-azure-machine-learning-architecture.md#workspace) en Azure Portal para el [servicio Azure Machine Learning](overview-what-is-azure-ml.md).  También puede crear y eliminar áreas de trabajo [mediante la CLI](reference-azure-machine-learning-cli.md) o [con código de Python](https://aka.ms/aml-sdk).

Para crear un área de trabajo, necesita una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLfree) antes de empezar.

## <a name="create-a-workspace"></a>Crear un área de trabajo 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view-a-workspace"></a>Vista de un área de trabajo

1. En la esquina superior izquierda del portal, seleccione **Todos los servicios**. 

1. En el campo de filtro **Todos los servicios**, escriba **Área de trabajo del servicio Machine Learning**.  

   ![búsqueda del área de trabajo del servicio Azure Machine Learning](media/how-to-manage-workspace/allservices-search1.png)

1. En los resultados del filtro, seleccione **Área de trabajo del servicio Machine Learning** para mostrar una lista de las áreas de trabajo. 

   ![búsqueda del área de trabajo del servicio Azure Machine Learning](media/how-to-manage-workspace/allservices-search.PNG)

1. Examine la lista de áreas de trabajo encontradas. Puede filtrar en función de suscripción, grupos de recursos y ubicaciones.  

   ![Lista de áreas de trabajo del servicio Azure Machine Learning](media/how-to-manage-workspace/allservices_view_workspace.PNG)

1. Seleccione el área de trabajo que acaba de crear para mostrar sus propiedades.

   ![png](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Eliminar un área de trabajo

Use el botón Eliminar en la parte superior del área de trabajo que desea eliminar.

  ![png](media/how-to-manage-workspace/delete-workspace.png)


## <a name="clean-up-resources"></a>Limpieza de recursos 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

Siga el tutorial completo para obtener información sobre cómo usar un área de trabajo para compilar, entrenar e implementar modelos con el servicio Azure Machine Learning.

> [!div class="nextstepaction"]
> [Tutorial: Modelos de entrenamiento](tutorial-train-models-with-aml.md)
