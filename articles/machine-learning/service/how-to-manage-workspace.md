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
ms.date: 05/10/2019
ms.custom: seodec18
ms.openlocfilehash: 7f0806a1d68cd2cede1ae51f0a50a8125c1e7c8b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66016531"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Creación y administración de áreas de trabajo del servicio Azure Machine Learning

En este artículo creará, verá y eliminará [**áreas de trabajo del servicio Azure Machine Learning**](concept-workspace.md) en Azure Portal para el [servicio Azure Machine Learning](overview-what-is-azure-ml.md).  También puede crear y eliminar áreas de trabajo [mediante la CLI](reference-azure-machine-learning-cli.md), [con código de Python](https://aka.ms/aml-sdk) o [a través de la extensión de VS Code](how-to-vscode-tools.md#get-started-with-azure-machine-learning).

## <a name="create-a-workspace"></a>Crear un área de trabajo

Para crear un área de trabajo, necesita una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view"></a>Vista de un área de trabajo

1. En la esquina superior izquierda del portal, seleccione **Todos los servicios**.

1. En el campo de filtro **Todos los servicios**, escriba **servicio Machine Learning**.  

1. Seleccione **Áreas de trabajo del servicio Machine Learning**.

   ![Búsqueda del área de trabajo de Azure Machine Learning Service](media/how-to-manage-workspace/all-services.png)

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
