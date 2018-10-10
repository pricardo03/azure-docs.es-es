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
ms.openlocfilehash: 6d6e6a2aea867c5b603d950a4bbaa33f14695f45
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47011035"
---
Inicie sesión en [Azure Portal](https://portal.azure.com/) con las credenciales de la suscripción de Azure que se va a utilizar. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ahora.

El panel del área de trabajo del portal solo es compatible con los exploradores Edge, Chrome y Firefox.

   ![Azure Portal](./media/aml-create-in-portal/portal-dashboard.png)

Seleccione el botón **Crear un recurso** (+) de la esquina superior izquierda del portal.

   ![Creación de un recurso en Azure Portal](./media/aml-create-in-portal/portal-create-a-resource.png)

Escriba **Machine Learning** en la barra de búsqueda. Seleccione el resultado de la búsqueda llamado **Área de trabajo de Machine Learning**.

   ![búsqueda del área de trabajo](./media/aml-create-in-portal/allservices-search.PNG)

En el panel **Área de trabajo de Machine Learning**, desplácese hasta la parte inferior y seleccione **Crear** para comenzar.

   ![create](./media/aml-create-in-portal/portal-create-button.png)

En el panel **Área de trabajo de Machine Learning**, configure el área de trabajo.

   Campo|DESCRIPCIÓN
   ---|---
   Nombre del área de trabajo |Escriba un nombre único que identifique el área de trabajo.  Aquí usaremos docs-ws. Los nombres deben ser únicos en el grupo de recursos. Utilice un nombre que sea fácil de recordar y que se diferencie de las áreas de trabajo creadas por otros.  
   Subscription |Elija la suscripción de Azure que quiera usar. Si tiene varias suscripciones, elija en la que se factura el recurso.
   Grupos de recursos | Use un grupo de recursos existente en su suscripción o escriba un nombre para crear un nuevo grupo de recursos. Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure.  Aquí usaremos docs-aml. 
   Ubicación | Elija la ubicación más cercana a los usuarios y los recursos de datos. Aquí es donde se crea el área de trabajo.

   ![creación del área de trabajo](./media/aml-create-in-portal/workspace-create.png)

Seleccione **Crear** para iniciar el proceso de creación.  El área de trabajo puede tardar unos momentos en crearse.

   Para comprobar el estado de la implementación, seleccione el icono Notificaciones (campana) en la barra de herramientas.

   ![creación del área de trabajo](./media/aml-create-in-portal/notifications.png)

   Cuando finalice, aparecerá un mensaje de implementación correcta.  También está presente en la sección de notificaciones.   Haga clic en el botón **Ir al recurso** para ver la nueva área de trabajo.
