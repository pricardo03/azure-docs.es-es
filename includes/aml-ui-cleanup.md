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
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66123162"
---
>[!IMPORTANT]
>Los recursos que creó pueden usarse como requisitos previos de otros tutoriales y artículos de procedimientos de Azure Machine Learning Service.


### <a name="delete-everything"></a>Eliminar todo el contenido

Si no va a usar nada de lo que ha creado, elimine el grupo de recursos completo para que no le genere gastos:

1. En Azure Portal, seleccione **Grupos de recursos** en la parte izquierda de la ventana.
 
   ![Eliminación de un grupo de recursos en Azure Portal](./media/aml-ui-cleanup/delete-resources.png)

1. En la lista, seleccione el grupo de recursos que creó.

1. En la parte derecha de la ventana, seleccione el botón de puntos suspensivos (**...** ).

1. Seleccione **Eliminar grupo de recursos**.

Al eliminar el grupo de recursos también se eliminan todos los recursos que creó en la interfaz visual.  

### <a name="delete-only-the-compute-target"></a>Eliminación solo del destino de proceso

El destino de proceso que ha creado aquí se *escala automáticamente* a cero nodos cuando no se usa. De esta forma se reducen los costos. Si quiere eliminar el destino de proceso, siga estos pasos:

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com).

    ![Eliminación del destino de proceso](./media/aml-ui-cleanup/delete-compute-target.png)

1. En la sección **Proceso** del área de trabajo, seleccione el recurso.

1. Seleccione **Eliminar**.

### <a name="delete-individual-assets"></a>Eliminación de recursos individuales

En la interfaz visual donde creó el experimento, elimine recursos individuales; para ello, selecciónelos y, luego, haga clic en el botón **Eliminar**.

![Eliminación de experimentos](./media/aml-ui-cleanup/delete-experiment.png)
