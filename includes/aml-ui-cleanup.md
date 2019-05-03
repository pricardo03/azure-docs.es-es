---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 05/06/2019
ms.openlocfilehash: 623e993dfbe6bbb3297fa6470865ab1a04f55b37
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028596"
---
>[!IMPORTANT]
>Puede usar los recursos que ha creado como requisitos previos para otros tutoriales de servicio de Azure Machine Learning y artículos de procedimientos.


### <a name="delete-everything"></a>Eliminar todo el contenido

Si no va a usar todo lo que ha creado, elimine el grupo de recursos completo para no incurrir en cargos:

1. En el portal de Azure, seleccione **grupos de recursos** en el lado izquierdo de la ventana.
 
   ![Eliminación de un grupo de recursos en Azure Portal](./media/aml-ui-cleanup/delete-resources.png)

1. En la lista, seleccione el grupo de recursos que ha creado.

1. En el lado derecho de la ventana, seleccione el botón de puntos suspensivos (**...** ).

1. Seleccione **Eliminar grupo de recursos**.

Eliminando el grupo de recursos, también elimina todos los recursos que creó en la interfaz visual.  

### <a name="delete-only-the-compute-target"></a>Eliminar solo el destino de proceso

El destino de proceso que ha creado aquí *automáticamente escala automáticamente* a cero los nodos cuando no se utiliza. Esto sirve para minimizar los costos. Si desea eliminar el destino de proceso, siga estos pasos:

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com).

    ![Eliminar el destino de proceso](./media/aml-ui-cleanup/delete-compute-target.png)

1. En el **proceso** sección del área de trabajo, seleccione el recurso.

1. Seleccione **Eliminar**.

### <a name="delete-individual-assets"></a>Eliminación de recursos individuales

En la interfaz visual, donde se ha creado el experimento, eliminar recursos individuales seleccionándolas y, a continuación, seleccionando la **eliminar** botón.

![Eliminar experimentos](./media/aml-ui-cleanup/delete-experiment.png)
