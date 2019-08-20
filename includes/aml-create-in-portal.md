---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/31/2019
ms.openlocfilehash: e571c65e64fad73c646aa05366cab685aa745caa
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968937"
---
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con las credenciales de la suscripción de Azure que va a usar. 

1. En la esquina superior izquierda de Azure Portal, seleccione **Crear un recurso**.

1. Use la barra de búsqueda para encontrar el **área de trabajo de Machine Learning Service**.

1. Seleccione **Área de trabajo de Azure Machine Learning Service**.

1. En el panel **Área de trabajo del servicio ML**, seleccione **Crear** para comenzar.

1. Configure el área de trabajo nueva proporcionando el nombre del área de trabajo, la suscripción, el grupo de recursos y la ubicación.

    ![Creación del espacio de trabajo](./media/aml-create-in-portal/workspace-create-main-tab.png)

   Campo|DESCRIPCIÓN 
   ---|---
   Nombre del área de trabajo |Escriba un nombre único que identifique el área de trabajo. En este ejemplo, se usa **docs-ws**. Los nombres deben ser únicos en el grupo de recursos. Utilice un nombre que sea fácil de recordar y que se diferencie del de las áreas de trabajo creadas por otros.  
   Subscription |Seleccione la suscripción de Azure que quiera usar.
   Resource group | Use un grupo de recursos existente en su suscripción o escriba un nombre para crear un nuevo grupo de recursos. Un grupo de recursos almacena los recursos relacionados con una solución de Azure. En este ejemplo, se usa **docs-aml**. 
   Location | Seleccione la ubicación más cercana a los usuarios y los recursos de datos para crear el área de trabajo.

1. Cuando haya terminado de configurarla, seleccione **Crear**. 

   El área de trabajo puede tardar unos momentos en crearse.

   Cuando finalice el proceso, aparecerá un mensaje de implementación correcta. También está presente en la sección de notificaciones. Para ver la nueva área de trabajo, seleccione **Ir al recurso**.

   ![Estado de creación del área de trabajo](./media/aml-create-in-portal/notifications.png)

1. Si va a usar una opción sin código del portal, como la interfaz visual o los experimentos automatizados de Machine Learning, ya está preparado. Si va a crear una [máquina virtual de Notebook](../articles/machine-learning/service/tutorial-1st-experiment-sdk-setup.md#azure), también está preparado. 

1. Si tiene previsto usar código en el entorno local que haga referencia a esta área de trabajo, seleccione **Descargar config.json** en la sección **Información general** del área de trabajo.  

   ![Descargar config.json](./media/aml-create-in-portal/configure.png)
   
   Coloque el archivo en la estructura de directorios que contiene los scripts de Python o las instancias de Jupyter Notebook. Puede estar en el mismo directorio, en un subdirectorio denominado *.azureml* o en un directorio principal. Al crear una máquina virtual de Notebook, este archivo se agrega al directorio correcto de la máquina virtual.

    

