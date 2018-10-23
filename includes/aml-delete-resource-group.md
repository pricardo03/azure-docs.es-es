---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: j-martens
ms.service: machine-learning
ms.author: jmartens
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/27/2018
ms.openlocfilehash: 300b4a3e5c6c52f1c09f8e70a72f08406861a5d1
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400171"
---
>[!IMPORTANT]
>Los recursos que creó pueden usarse como requisitos previos para otros tutoriales y artículos de procedimientos del servicio Azure Machine Learning. 


Si no va a usar los recursos creados de aquí, elimínelos para no incurrir en cargos.

1. En Azure Portal, seleccione **Grupos de recursos** a la izquierda del todo.
 
   ![Eliminación en Azure Portal](./media/aml-delete-resource-group/delete-resources.png)

1. En la lista, seleccione el grupo de recursos que creó.

1. Seleccione **Eliminar grupo de recursos**.

1. Escriba el nombre del grupo de recursos y, a continuación, seleccione **Eliminar**.

   Si ve el mensaje de error "Cannot delete resource before nested resources are deleted" (No se puede eliminar el recurso hasta que se eliminen los recursos anidados), debe eliminar primero todos los recursos anidados. Para más información sobre cómo eliminar los recursos anidados, consulte [esta sección de solución de problemas](../articles/machine-learning/desktop-workbench/known-issues-and-troubleshooting-guide.md#cant-delete-experimentation-account). 
