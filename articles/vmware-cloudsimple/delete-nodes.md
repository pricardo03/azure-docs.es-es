---
title: 'Eliminación de nodos de VMware Solutions (AVS): Azure'
description: Aprenda a eliminar nodos de su instancia de VMWare con la implementación de AVS.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 30d128d6bd2f2e1e2705a7b742f02d11fd947a03
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024745"
---
# <a name="delete-nodes-from-azure-vmware-solution-by-avs"></a>Eliminación de nodos de Azure VMware Solution by AVS

Los nodos de AVS se miden una vez que se crean. Los nodos deben eliminarse para detener la medición. Los nodos que no se usan se eliminan en Azure Portal.

## <a name="before-you-begin"></a>Antes de empezar

Un nodo solo se puede eliminar en las siguientes condiciones:

* Se ha eliminado una nube privada de AVS creada con los nodos. Para eliminar una nube privada de AVS, consulte [Eliminación de una nube privada de Azure VMware Solution by AVS](delete-private-cloud.md).
* El nodo se ha quitado de la nube privada de AVS mediante la reducción de dicha nube. Para reducir una nube privada de AVS, consulte [Reducción de una nube privada de Azure VMware Solution by AVS](shrink-private-cloud.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="delete-avs-node"></a>Eliminación de un nodo de AVS

1. Seleccione **Todos los servicios**.

2. Busque **AVS Nodes** (Nodos de AVS).

   ![Buscar nodos de AVS](media/create-cloudsimple-node-search.png)

3. Seleccione **AVS Nodes** (Nodos de AVS).

4. Seleccione los nodos que no pertenecen a una nube privada de AVS para eliminarlos. En la columna **AVS PRIVATE CLOUD NAME** (NOMBRE DE LA NUBE PRIVADA DE AVS) se muestra el nombre de la nube privada de AVS a la que pertenece el nodo. Si un nodo no se usa en una nube privada de AVS, el valor estará vacío. 

    ![Seleccionar nodos de AVS](media/select-delete-cloudsimple-node.png)

> [!NOTE]
> Solo se pueden eliminar los nodos que no formen parte de la nube privada de AVS.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre la [nube privada de AVS](cloudsimple-private-cloud.md).
