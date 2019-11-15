---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 11/06/2019
ms.openlocfilehash: 66f5c72fcabb62e21f0110cb981b7271244c0648
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73800127"
---
>[!IMPORTANT]
>Los recursos que creó pueden usarse como requisitos previos de otros tutoriales y artículos de procedimientos de Azure Machine Learning.

### <a name="delete-everything"></a>Eliminar todo el contenido

Si no va a usar nada de lo que ha creado, elimine el grupo de recursos completo para que no le genere gastos:

1. En Azure Portal, seleccione **Grupos de recursos** en la parte izquierda de la ventana.
 
   ![Eliminación de un grupo de recursos en Azure Portal](./media/aml-ui-cleanup/delete-resources.png)

1. En la lista, seleccione el grupo de recursos que creó.

1. Seleccione **Eliminar grupo de recursos**.

Al eliminar el grupo de recursos también se eliminan todos los recursos que creó en el diseñador.  

### <a name="delete-individual-assets"></a>Eliminación de recursos individuales

En diseñador donde creó el experimento, elimine recursos individuales; para ello, selecciónelos y, luego, haga clic en el botón **Eliminar**.

El destino de proceso que ha creado aquí se *escala automáticamente* a cero nodos cuando no se usa. De esta forma se reducen los costos. Si quiere eliminar el destino de proceso, siga estos pasos:

![Eliminación de recursos](./media/aml-ui-cleanup/delete-asset.png)

Se puede anular el registro de los conjuntos de datos del área de trabajo seleccionando cada conjunto de datos y **Anular el registro**.

![Anulación del registro de conjunto de datos](./media/aml-ui-cleanup/unregister-dataset.png)

Para eliminar un conjunto de datos, vaya a la cuenta de almacenamiento mediante Azure Portal o el Explorador de Storage y elimine manualmente esos recursos.


