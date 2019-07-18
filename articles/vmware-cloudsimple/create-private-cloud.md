---
title: Crear una nube privada de solución de VMware en Azure por de CloudSimple
description: Describe cómo crear una nube privada de CloudSimple para ampliar las cargas de trabajo de VMware a la nube con continuidad y flexibilidad de funcionamiento.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e5c03c1d8a865b792ce79e3e2b576a629b71e02c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332246"
---
# <a name="create-a-cloudsimple-private-cloud"></a>Crear una nube privada de CloudSimple

Crear una nube privada le ayuda a satisfacer una variedad de necesidades comunes de infraestructura de red:

* **Crecimiento**. Si ha alcanzado un punto de actualización de hardware para la infraestructura existente, una nube privada le permite expandirse sin necesidad de ninguna inversión en hardware nuevo.

* **Rápida expansión**. Si surgen necesidades de capacidad temporales o no planeadas, una nube privada le permite crear la capacidad adicional sin demora.

* **Mayor protección**. Con una nube privada de tres nodos o más, obtiene protección automática de redundancia y alta disponibilidad.

* **Necesidades de infraestructura a largo plazo**. Si sus centros de datos trabajan a capacidad o quiere reestructurar para reducir los costos, una nube privada le permite retirar los centros de datos y migrar a una solución basada en la nube, a la vez que sigue siendo compatible con las operaciones empresariales.

Al crear una nube privada, obtendrá un clúster de vSphere único y todas las VM de administración que se crean en ese clúster.

## <a name="before-you-begin"></a>Antes de empezar

Los nodos deben aprovisionarse antes de poder crear su propia nube privada.  Para más información sobre el aprovisionamiento de nodos, consulte el artículo [Provision nodes for VMware Solution by CloudSimple - Azure](create-nodes.md) (Aprovisionar nodos para la solución de VMware en Azure de CloudSimple).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Acceso al portal de CloudSimple

Acceda al [portal de CloudSimple](access-cloudsimple-portal.md).

## <a name="create-a-new-private-cloud"></a>Crear una nube privada

1. En la página **Recursos**, haga clic en **New Private Cloud** (Nueva nube privada).

    ![Crear una nube privada: cómo comenzar](media/create-pc-button.png)

2. Seleccione la ubicación para hospedar los recursos de la nube privada.

3. Elija el tipo de nodo, CS28 o CS36, que ha aprovisionado para la nube privada. Esta última opción incluye la capacidad de proceso y memoria máxima.

4. Seleccione el número de nodos de la nube privada. Puede seleccionar como máximo el número de nodos disponibles que ha [aprovisionado](create-nodes.md).

    ![Crear una nube privada: configuración básica](media/create-private-cloud-basic-info.png)

5. Haga clic en **Siguiente: Opciones avanzadas**.

6. Escriba el intervalo CIDR de subredes de vSphere/vSAN. Asegúrese de que el intervalo CIDR no se superponga con ninguna de las subredes locales u otras subredes de Azure (redes virtuales) ni con la subred de la puerta de enlace.  No use ningún intervalo de CIDR definido en redes virtuales de Azure.
    
    **Opciones de intervalo de CIDR:** /24, /23, /22 o /21. Un intervalo de CIDR /24 admite hasta nueve nodos, un intervalo de CIDR /23 admite hasta 41 nodos y un intervalo de CIDR /22 y /21 admite hasta 64 nodos (el número máximo de nodos en una nube privada).

    > [!CAUTION]
    > Las direcciones IP en el intervalo CIDR de vSphere/vSAN está reservado para su uso por la infraestructura de nube privada.  No use la dirección IP de este intervalo en ninguna máquina virtual.

7. Haga clic en **Siguiente: Revisar y crear**.

8. Revise la configuración. Si necesita cambiar la configuración, haga clic en **Anterior**.

9. Haga clic en **Create**(Crear).

El aprovisionamiento de la nube privada se iniciará al hacer clic en Crear.  Puede supervisar el progreso desde la página de [tareas](https://docs.azure.cloudsimple.com/activity/#tasks) en el portal de CloudSimple.  El aprovisionamiento puede tardar de 30 minutos a dos horas.  Una vez completado el aprovisionamiento, recibirá un correo electrónico.

## <a name="next-steps"></a>Pasos siguientes

* [Expandir la nube privada](expand-private-cloud.md)