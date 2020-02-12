---
title: Reducción de la nube privada de Azure VMware Solutions (AVS)
description: Describe cómo reducir una nube privada de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0ea764081cd0b4d5c6d44cd7364d1e9a89a3cec3
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014273"
---
# <a name="shrink-an-avs-private-cloud"></a>Reducción de una nube privada de AVS

AVS ofrece la flexibilidad para reducir dinámicamente una nube privada de AVS. Una nube privada de AVS se compone de uno o varios clústeres de vSphere. Cada clúster puede tener de 3 a 16 nodos. Cuando se reduce una nube privada de AVS, se elimina un nodo del clúster existente o se elimina un clúster completo. 

## <a name="before-you-begin"></a>Antes de empezar

Deben cumplirse las siguientes condiciones antes de reducir una nube privada AVS. El clúster de administración (primer clúster) se crea cuando se creó la nube privada de AVS. No se puede eliminar.

* Un clúster de vSphere debe tener tres nodos. No se puede reducir un clúster con solo tres nodos.
* El almacenamiento total consumido no debe exceder la capacidad total después de la reducción del clúster.
* Compruebe si alguna de las reglas de Distributed Resource Scheduler (DRS) impiden vMotion de una máquina virtual. Si las reglas están activas, deshabilítelas o elimínelas. Entre las reglas de DRS se incluyen las reglas de afinidad de máquina virtual a host.


## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="shrinking-an-avs-private-cloud"></a>Reducción de una nube privada de AVS

1. [Acceda al portal de AVS](access-cloudsimple-portal.md).

2. Abra la página **Resources** (Recursos).

3. Haga clic en la nube privada de AVS que quiera reducir.

4. En la página de resumen, haga clic en **Shrink** (Reducir).

    ![Reducción de una nube privada de AVS](media/shrink-private-cloud.png)

5. Seleccione el clúster que desea reducir o eliminar. 

    ![Reducción de la nube privada de AVS: selección de clúster](media/shrink-private-cloud-select-cluster.png)

6. Seleccione **Remove one node** (Quitar un nodo) o **Delete the whole cluster** (Eliminar todo el clúster). 

7. Compruebe la capacidad del clúster.

8. Haga clic en **Enviar** para reducir la nube privada.

Se inicia la reducción de la nube privada de AVS. Puede supervisar el progreso en las tareas. El proceso de reducción puede tardar unas horas según los datos, que debe volver a sincronizarse en vSAN.

> [!NOTE]
> 1. Si reduce una nube privada eliminando el último o el único clúster del centro de datos, el centro de datos no se eliminará.
> 2. Si se produce una infracción de las reglas de DRS, el nodo no se quitará del clúster y la descripción de la tarea mostrará que la eliminación de un nodo infringirá las reglas de DRS en el clúster.    


## <a name="next-steps"></a>Pasos siguientes

* [Uso de máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* Más información sobre las [nubes privadas de AVS](cloudsimple-private-cloud.md)
