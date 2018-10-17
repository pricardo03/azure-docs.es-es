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
ms.openlocfilehash: fb874d06fbccc7e1db542a5d063b9b6acd35b83f
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48240061"
---
>[!IMPORTANT]
>Los recursos que creó pueden usarse como requisitos previos para otros tutoriales y artículos de procedimientos del servicio Azure Machine Learning. 


Si no va a usar lo que ha creado aquí, elimine los recursos que acaba de crear en esta guía de inicio rápido para no incurrir en gastos.

1. En Azure Portal, seleccione **Grupos de recursos** a la izquierda del todo.  
 
   ![Eliminación en Azure Portal](./media/aml-delete-resource-group/delete-resources.png)

1. En la lista, seleccione el grupo de recursos que creó.

1. Seleccione **Eliminar grupo de recursos**.

1. Escriba el nombre del grupo de recursos y, a continuación, seleccione **Eliminar**.

   Si recibe el error "Can not delete resource before nested resources are deleted" (No se puede eliminar el recurso hasta que se eliminen los recursos anidados), debe eliminar primero todos los recursos anidados. Para más información, [consulte esta sección de solución de problemas](../articles/machine-learning/desktop-workbench/known-issues-and-troubleshooting-guide.md#cant-delete-experimentation-account). 
