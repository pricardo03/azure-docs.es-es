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
ms.date: 09/24/2018
ms.openlocfilehash: 57fd69542a5d92b9afd1e003d8b94c1ebb64953e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65031799"
---
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con las credenciales de la suscripción de Azure que va a usar. 

   ![Azure Portal](./media/aml-create-in-portal/portal-dashboard.png)

1. En la esquina superior izquierda del portal, seleccione **Crear un recurso**.

   ![Creación de un recurso en Azure Portal](./media/aml-create-in-portal/portal-create-a-resource.png)

1. Escriba **Machine Learning** en la barra de búsqueda. Seleccione el resultado de la búsqueda llamado **Área de trabajo del servicio de Machine Learning**.

   ![Búsqueda de un área de trabajo](./media/aml-create-in-portal/allservices-search.png)

1. En el panel **Área de trabajo del servicio de ML**, desplácese hasta la parte inferior y seleccione **Crear** para comenzar.

   ![Crear](./media/aml-create-in-portal/portal-create-button.png)

1. En el panel **Área de trabajo del servicio de Machine Learning**, configure el área de trabajo.

   Campo|DESCRIPCIÓN
   ---|---
   Nombre del área de trabajo |Escriba un nombre único que identifique el área de trabajo. En este ejemplo, se usa **docs-ws**. Los nombres deben ser únicos en el grupo de recursos. Utilice un nombre que sea fácil de recordar y que se diferencie de las áreas de trabajo creadas por otros.  
   Subscription |Seleccione la suscripción de Azure que quiera usar.
   Grupos de recursos | Use un grupo de recursos existente en su suscripción o escriba un nombre para crear un nuevo grupo de recursos. Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. En este ejemplo, se usa **docs-aml**. 
   Location | Seleccione la ubicación más cercana a los usuarios y los recursos de datos. Esta ubicación es donde se crea el área de trabajo.

   ![Creación del espacio de trabajo](./media/aml-create-in-portal/workspace-create.png)

1. Seleccione **Crear** para comenzar el proceso de creación. El área de trabajo puede tardar unos momentos en crearse.

1. Para comprobar el estado de la implementación, seleccione el icono Notificaciones (**campana**) en la barra de herramientas.

1. Cuando finalice el proceso, aparecerá un mensaje de implementación correcta. También está presente en la sección de notificaciones. Para ver la nueva área de trabajo, seleccione **Ir al recurso**.

   ![Estado de creación del área de trabajo](./media/aml-create-in-portal/notifications.png)
