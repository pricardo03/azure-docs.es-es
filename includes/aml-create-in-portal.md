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
ms.date: 05/21/2019
ms.openlocfilehash: 72f23b10047928f32886d9054f4dd1abdc569bd8
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66396915"
---
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con las credenciales de la suscripción de Azure que va a usar. 

   ![Portal de Azure](./media/aml-create-in-portal/portal-dashboard-05-2019.png)

1. En la esquina superior izquierda del portal, seleccione **Crear un recurso**.

   ![Creación de un recurso en Azure Portal](./media/aml-create-in-portal/portal-create-a-resource-05-2019.png)

1. Escriba **Machine Learning** en la barra de búsqueda. Seleccione el resultado de la búsqueda llamado **Área de trabajo del servicio de Machine Learning**.

   ![Búsqueda de un área de trabajo](./media/aml-create-in-portal/allservices-search-05-2019.png)

1. En el panel **Área de trabajo del servicio ML**, seleccione **Crear** para comenzar.

    ![Botón Crear](./media/aml-create-in-portal/portal-create-button-05-2019.png)

1. En el panel **Área de trabajo del servicio de Machine Learning**, configure el área de trabajo.

   Campo|DESCRIPCIÓN
   ---|---
   Nombre del área de trabajo |Escriba un nombre único que identifique el área de trabajo. En este ejemplo, se usa **docs-ws**. Los nombres deben ser únicos en el grupo de recursos. Utilice un nombre que sea fácil de recordar y que se diferencie de las áreas de trabajo creadas por otros.  
   Subscription |Seleccione la suscripción de Azure que quiera usar.
   Grupos de recursos | Use un grupo de recursos existente en su suscripción o escriba un nombre para crear un nuevo grupo de recursos. Un grupo de recursos almacena los recursos relacionados con una solución de Azure. En este ejemplo, se usa **docs-aml**. 
   Ubicación | Seleccione la ubicación más cercana a los usuarios y los recursos de datos. Esta ubicación es donde se crea el área de trabajo.

   ![Creación del espacio de trabajo](./media/aml-create-in-portal/workspace-create-main-tab-05-2019.png)

1. Seleccione **Revisar + crear** para comenzar el proceso de creación.

    ![Crear](./media/aml-create-in-portal/workspace-create-main-review-button-05-2019.png)

1. Revise la configuración del área de trabajo. Si es correcta, seleccione **Crear**. El área de trabajo puede tardar unos momentos en crearse.

    ![Crear](./media/aml-create-in-portal/workspace-create-review-tab-05-2019.png)

1. Para comprobar el estado de la implementación, seleccione el icono Notificaciones (**campana**) en la barra de herramientas.

1. Cuando finalice el proceso, aparecerá un mensaje de implementación correcta. También está presente en la sección de notificaciones. Para ver la nueva área de trabajo, seleccione **Ir al recurso**.

   ![Estado de creación del área de trabajo](./media/aml-create-in-portal/notifications-05-2019.png)
