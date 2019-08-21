---
title: 'Eliminación de nodos de la solución de VMware de CloudSimple: Azure'
description: Aprenda a eliminar nodos de su instancia de VMWare con implementación de CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9dbe1751ac77f370991af75ea161247c21700ff2
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68886571"
---
# <a name="delete-nodes-from-vmware-solution-by-cloudsimple---azure"></a>Eliminación de nodos de la solución de VMware de CloudSimple: Azure

Los nodos de CloudSimple se miden una vez que se crean.  Los nodos deben eliminarse para detener la medición.  Los nodos que no se usan se eliminan en Azure Portal. 

## <a name="before-you-begin"></a>Antes de empezar

Un nodo solo se puede eliminar en las siguientes condiciones:

* Se elimina una nube privada creada con los nodos.  Para eliminar una nube privada, consulte [Eliminación de la nube privada de una solución de Azure VMware de CloudSimple](delete-private-cloud.md).
* El nodo se ha quitado de la nube privada mediante la reducción de esta.  Para reducir una nube privada, consulte [Reducción de una solución de VMware de Azure mediante una nube privada de CloudSimple](shrink-private-cloud.md).


## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="delete-cloudsimple-node"></a>Eliminación de un nodo de CloudSimple

1. Seleccione **Todos los servicios**.

2. Busque **Nodos CloudSimple**.

   ![Búsqueda de los nodos CloudSimple](media/create-cloudsimple-node-search.png)

3. Seleccione **Nodos CloudSimple**.

4. Seleccione los nodos que no pertenecen a una nube privada para eliminarlos.  En la columna **PRIVATE CLOUD NAME** (NOMBRE DE LA NUBE PRIVADA) se muestra el nombre de la nube privada a la que pertenece el nodo.  Si un nodo no se usa en una nube privada, el valor estará vacío. 

    ![Selección de nodos de CloudSimple](media/select-delete-cloudsimple-node.png)

> [!NOTE]
> Solo se pueden eliminar los nodos que no formen parte de la nube privada.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre la [nube privada](cloudsimple-private-cloud.md).
