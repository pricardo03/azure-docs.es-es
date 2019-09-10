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
ms.date: 08/14/2019
ms.openlocfilehash: 11287ee5ca0df3e51bca20b39c35eb5ce33b88bd
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70231075"
---
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con las credenciales de la suscripción de Azure que va a usar. 

1. En la esquina superior izquierda de Azure Portal, seleccione **+ Crear un recurso**.

      ![Crear un nuevo recurso](media/aml-create-in-portal/portal-create-resource.png)

1. Use la barra de búsqueda para encontrar el **área de trabajo de Machine Learning Service**.

1. Seleccione **Área de trabajo de Azure Machine Learning Service**.

1. En el panel **Área de trabajo del servicio Machine Learning**, seleccione **Crear** para comenzar.

1. Especifique la siguiente información para configurar la nueva área de trabajo:

   Campo|DESCRIPCIÓN 
   ---|---
   Nombre del área de trabajo |Escriba un nombre único que identifique el área de trabajo. En este ejemplo, se usa **docs-ws**. Los nombres deben ser únicos en el grupo de recursos. Utilice un nombre que sea fácil de recordar y que se diferencie del de las áreas de trabajo creadas por otros.  
   Subscription |Seleccione la suscripción de Azure que quiera usar.
   Resource group | Use un grupo de recursos existente en su suscripción o escriba un nombre para crear un nuevo grupo de recursos. Un grupo de recursos almacena los recursos relacionados con una solución de Azure. En este ejemplo, se usa **docs-aml**. 
   Location | Seleccione la ubicación más cercana a los usuarios y los recursos de datos para crear el área de trabajo.

1. Cuando haya terminado de configurarla, seleccione **Crear**. 

   > [!Warning] 
   > La creación del área de trabajo en la nube puede tardar varios minutos.

   Cuando finalice el proceso, aparecerá un mensaje de implementación correcta. 
 
 1. Para ver la nueva área de trabajo, seleccione **Ir al recurso**.

