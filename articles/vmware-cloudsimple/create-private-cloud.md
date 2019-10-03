---
title: 'Solución de VMware en Azure de CloudSimple: creación de una nube privada de CloudSimple'
description: Describe cómo crear una nube privada de CloudSimple para ampliar las cargas de trabajo de VMware a la nube con continuidad y flexibilidad de funcionamiento.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a6e3d466321fcd8f32f46359c97f67400a8f86c6
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71828155"
---
# <a name="create-a-cloudsimple-private-cloud"></a>Crear una nube privada de CloudSimple

Una nube privada es una pila de VMware aislada que admite hosts ESXi, vCenter, vSAN y NSX. Las nubes privadas se administran desde el portal de CloudSimple. Tienen su propio servidor vCenter en su propio dominio de administración. La pila se ejecuta en nodos dedicados y nodos de hardware sin sistema operativo aislados.

Crear una nube privada le ayuda a satisfacer una variedad de necesidades comunes de infraestructura de red:

* **Crecimiento**. Si ha alcanzado un punto de actualización de hardware para la infraestructura existente, una nube privada le permite expandirse sin necesidad de ninguna inversión en hardware nuevo.

* **Rápida expansión**. Si surgen necesidades de capacidad temporales o no planeadas, una nube privada le permite crear la capacidad adicional sin demora.

* **Mayor protección**. Con una nube privada de tres nodos o más, obtiene protección automática de redundancia y alta disponibilidad.

* **Necesidades de infraestructura a largo plazo**. Si sus centros de datos trabajan a capacidad o quiere reestructurar para reducir los costos, una nube privada le permite retirar los centros de datos y migrar a una solución basada en la nube, a la vez que sigue siendo compatible con las operaciones empresariales.

Al crear una nube privada, obtendrá un clúster de vSphere único y todas las VM de administración que se crean en ese clúster.

## <a name="access-the-cloudsimple-portal"></a>Acceso al portal de CloudSimple

Acceda al [portal de CloudSimple](access-cloudsimple-portal.md).

## <a name="create-a-new-private-cloud"></a>Crear una nube privada

1. Seleccione **Todos los servicios**.
2. Busque **Servicios de CloudSimple**.
3. Seleccione el servicio de CloudSimple en el que quiere crear la nube privada.
4. En **Información general**, haga clic en **Crear nube privada** para abrir una nueva pestaña del explorador del portal de CloudSimple. Si se le solicita, inicie sesión con las credenciales de inicio de sesión en Azure.

    ![Creación de una nube privada desde Azure](media/create-private-cloud-from-azure.png)

5. En el portal de CloudSimple, proporcione un nombre para su nube privada.
6. Seleccione la **ubicación** para la nube privada.
7. Seleccione **tipo de nodo** de acuerdo con lo que haya comprado en Azure.
8. Especifique el **número de nodos**.  Para crear una nube privada, se requiere un mínimo de tres nodos.

    ![Creación de una nube privada: información básica](media/create-private-cloud-basic-info.png)

9. Haga clic en **Siguiente: Opciones avanzadas**.
10. Escriba el intervalo CIDR de subredes de vSphere/vSAN. Asegúrese de que el intervalo CIDR no se superponga con ninguna de las subredes locales u otras subredes de Azure (redes virtuales) ni con la subred de la puerta de enlace.

    **Opciones de intervalo de CIDR:** /24, /23, /22 o /21. Un intervalo de CIDR /24 admite hasta nueve nodos, un intervalo de CIDR /23 admite hasta 41 nodos y un intervalo de CIDR /22 y /21 admite hasta 64 nodos (el número máximo de nodos en una nube privada).

    > [!IMPORTANT]
    > Las direcciones IP en el intervalo CIDR de vSphere/vSAN están reservadas para que las use la infraestructura de nube privada.  No use la dirección IP de este intervalo en ninguna máquina virtual.

11. Haga clic en **Siguiente: Revisar y crear**.
12. Revise la configuración. Si necesita cambiar la configuración, haga clic en **Anterior**.
13. Haga clic en **Create**(Crear).

Se inicia el proceso de aprovisionamiento de la nube privada. La nube privada puede tardar hasta dos horas en aprovisionarse.

Para obtener instrucciones sobre cómo expandir una nube privada existente, consulte [Expansión de una nube privada](expand-private-cloud.md).
