---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 10/22/2019
ms.openlocfilehash: 5a66212122745d0f4426e48e9487e9d674cec53f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489943"
---
>[!IMPORTANT]
>Los recursos que creó pueden usarse como requisitos previos de otros tutoriales y artículos de procedimientos de Azure Machine Learning.

### <a name="delete-everything"></a>Eliminar todo el contenido

Si no va a usar nada de lo que ha creado, elimine el grupo de recursos completo para que no le genere gastos:

1. En Azure Portal, seleccione **Grupos de recursos** en la parte izquierda de la ventana.
 
   ![Eliminación de un grupo de recursos en Azure Portal](./media/aml-ui-cleanup/delete-resources.png)

1. En la lista, seleccione el grupo de recursos que creó.

1. En la parte derecha de la ventana, seleccione el botón de puntos suspensivos ( **...** ).

1. Seleccione **Eliminar grupo de recursos**.

Al eliminar el grupo de recursos también se eliminan todos los recursos que creó en el diseñador.  

### <a name="delete-only-the-compute-target"></a>Eliminación solo del destino de proceso

El destino de proceso que ha creado aquí se *escala automáticamente* a cero nodos cuando no se usa. De esta forma se reducen los costos. Si quiere eliminar el destino de proceso, siga estos pasos:

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com).

    ![Eliminación del destino de proceso](./media/aml-ui-cleanup/delete-compute-target.png)

1. En la sección **Proceso** del área de trabajo, seleccione el recurso.

1. Seleccione **Eliminar**.

### <a name="delete-individual-assets"></a>Eliminación de recursos individuales

En diseñador donde creó el experimento, elimine recursos individuales; para ello, selecciónelos y, luego, haga clic en el botón **Eliminar**.

![Eliminación de recursos](./media/aml-ui-cleanup/delete-asset.png)

Se puede anular el registro de los conjuntos de datos del área de trabajo seleccionando cada conjunto de datos y **Anular el registro**.

![Anulación del registro de conjunto de datos](./media/aml-ui-cleanup/unregister-dataset.png)


