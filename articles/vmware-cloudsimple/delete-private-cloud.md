---
title: Eliminación de una nube privada de Azure VMware Solutions (AVS)
description: Describe cómo eliminar una nube privada de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 77cbfb19c3861bac517142f7491e6b1a5fb4ca27
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024762"
---
# <a name="delete-an-avs-private-cloud"></a>Eliminación de una nube privada de AVS

AVS ofrece la flexibilidad de eliminar una nube privada de AVS. Una nube privada de AVS se compone de uno o varios clústeres de vSphere. Cada clúster puede tener de 3 a 16 nodos. Al eliminar una nube privada de AVS, se eliminarán todos los clústeres.

## <a name="before-you-begin"></a>Antes de empezar

Con la eliminación de una nube privada de AVS se elimina toda la nube privada de AVS. Se eliminarán todos los componentes de la nube privada de AVS. Si quiere conservar los datos, asegúrese de que ha realizado una copia de seguridad de ellos en el almacenamiento local o en Azure Storage.

Los componentes de una nube privada de AVS incluyen:

* Nodos de AVS
* Máquinas virtuales
* VLAN o subredes
* Todos los datos de usuario almacenados en la nube privada de AVS
* Todos los datos adjuntos de reglas de firewall a una VLAN o subred

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="delete-an-avs-private-cloud"></a>Eliminación de una nube privada de AVS

1. [Acceda al portal de AVS](access-cloudsimple-portal.md).

2. Abra la página **Resources** (Recursos).

3. Haga clic en la nube privada de AVS que quiera eliminar.

4. En la página de resumen, haga clic en **Delete** (Eliminar).

    ![Eliminación de una nube privada de AVS](media/delete-private-cloud.png)

5. En la página de confirmación, escriba el nombre de la nube privada de AVS y haga clic en **Eliminar**. 

    ![Eliminación de la nube privada de AVS: confirmación](media/delete-private-cloud-confirm.png)

La nube privada de AVS está marcada para su eliminación. El proceso de eliminación se inicia al cabo de tres horas y elimina la nube privada de AVS.

> [!CAUTION]
> Los nodos deben eliminarse después de la eliminación de la nube privada de AVS. La medición de los nodos continuará hasta que se eliminen los nodos de la suscripción.

## <a name="next-steps"></a>Pasos siguientes

* [Eliminación de nodos](delete-nodes.md)
