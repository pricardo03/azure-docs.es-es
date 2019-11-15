---
title: Reducción de la solución de VMware en Azure por la nube privada de CloudSimple
description: Describe cómo reducir una nube privada de CloudSimple.
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1ae2f87a3719853f4a91cb8ba801be6d578597d3
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825680"
---
# <a name="shrink-a-cloudsimple-private-cloud"></a>Reducción de una nube privada de CloudSimple

CloudSimple ofrece la flexibilidad para reducir dinámicamente una nube privada.  Una nube privada se compone de uno o varios clústeres de vSphere. Cada clúster puede tener de 3 a 16 nodos. Cuando se reduce una nube privada, se elimina un nodo del clúster existente o se elimina un clúster completo. 

## <a name="before-you-begin"></a>Antes de empezar

Las siguientes condiciones deben cumplirse para la reducción de una nube privada.  No se puede eliminar el clúster de administración (primer clúster) creado cuando se creó una nube privada.

* Un clúster de vSphere debe tener tres nodos.  No se puede reducir un clúster con solo tres nodos.
* El almacenamiento total consumido no debe exceder la capacidad total después de la reducción del clúster. 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="shrink-a-private-cloud"></a>Reducción de una nube privada

1. [Acceda al portal de CloudSimple](access-cloudsimple-portal.md).

2. Abra la página **Resources** (Recursos).

3. Haga clic en la nube privada que quiera reducir.

4. En la página de resumen, haga clic en **Shrink** (Reducir).

    ![Reducción de una nube privada](media/shrink-private-cloud.png)

5. Seleccione el clúster que desea reducir o eliminar. 

    ![Reducción de la nube privada - seleccionar clúster](media/shrink-private-cloud-select-cluster.png)

6. Seleccione **Remove one node** (Quitar un nodo) o **Delete the whole cluster** (Eliminar todo el clúster). 

7. Compruebe la capacidad del clúster.

8. Haga clic en **Submit** (Enviar) para reducir la nube privada.

Se inicia la reducción de la nube privada.  Puede supervisar el progreso en las tareas.  El proceso de reducción puede tardar unas horas según los datos, que debe volver a sincronizarse en vSAN.

> [!NOTE]
> Si reduce una nube privada eliminando el último o el único clúster del centro de datos, el centro de datos no se eliminará.  


## <a name="next-steps"></a>Pasos siguientes

* [Uso de máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* Más información sobre las [nubes privadas](cloudsimple-private-cloud.md)
