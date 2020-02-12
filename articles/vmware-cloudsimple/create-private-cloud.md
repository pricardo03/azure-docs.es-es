---
title: 'Azure VMware Solutions (AVS): creación de una nube privada de AVS'
description: Describe cómo crear una nube privada de AVS para ampliar las cargas de trabajo de VMware a la nube con continuidad y flexibilidad de funcionamiento.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 128a0a1eec03878d0deba93048c54324aab7d888
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024796"
---
# <a name="create-an-avs-private-cloud"></a>Creación de una nube privada de AVS

Una nube privada de AVS es una pila de VMware aislada que admite hosts ESXi, vCenter, vSAN y NSX. Las nubes privadas de AVS se administran desde el portal de AVS. Tienen su propio servidor vCenter en su propio dominio de administración. La pila se ejecuta en nodos dedicados y nodos de hardware sin sistema operativo aislados.

Crear una nube privada de AVS le ayuda a satisfacer una variedad de necesidades comunes de infraestructura de red:

* **Crecimiento**. Si ha alcanzado un punto de actualización de hardware para la infraestructura existente, una nube privada de AVS le permite expandirse sin necesidad de ninguna inversión en hardware nuevo.

* **Rápida expansión**. Si surgen necesidades de capacidad temporales o no planeadas, una nube privada de AVS le permite crear la capacidad adicional sin demora.

* **Mayor protección**. Con una nube privada de AVS de tres nodos o más, obtiene protección automática de redundancia y alta disponibilidad.

* **Necesidades de infraestructura a largo plazo**. Si sus centros de datos trabajan a capacidad o quiere reestructurar para reducir los costos, una nube privada de AVS le permite retirar los centros de datos y migrar a una solución basada en la nube, a la vez que sigue siendo compatible con las operaciones empresariales.

Al crear una nube privada de AVS, obtiene un clúster de vSphere único y todas las VM de administración que se crean en ese clúster.

## <a name="before-you-begin"></a>Antes de empezar

Los nodos deben aprovisionarse para poder crear su propia nube privada de AVS. Para más información sobre el aprovisionamiento de nodos, consulte el artículo [Aprovisionamiento de nodos para Azure VMware Solutions (AVS)](create-nodes.md).

Asigne un intervalo de CIDR para las subredes vSphere/vSAN de la nube privada de AVS. Una nube privada de AVS se crea como un entorno de pila VMware aislada (con hosts ESXi, vCenter, vSAN y NSX) administrado por un servidor vCenter. Los componentes de administración se implementan en la red que se ha seleccionado para CIDR de subredes vSphere/vSAN. El intervalo de CIDR de red se divide en subredes diferentes durante la implementación. El espacio de direcciones de la subred vSphere/vSAN debe ser único. No se debe superponer con otras redes que se comuniquen con el entorno de AVS. Las redes que se comunican con AVS incluyen redes locales y redes virtuales de Azure. Para más información sobre las subredes vSphere/vSAN, consulte la introducción a las VLAN y subredes.

* Prefijo de intervalo de CIDR de subredes vSphere/vSAN mínimo: /24
* Prefijo de intervalo de CIDR de subredes vSphere/vSAN máximo: /21


## <a name="access-the-avs-portal"></a>Acceso al portal de AVS

Acceso al [portal de AVS](access-cloudsimple-portal.md).

## <a name="create-a-new-avs-private-cloud"></a>Creación de una nueva nube privada de AVS

1. Seleccione **Todos los servicios**.
2. Busque **AVS Services** (Servicios de AVS).
3. Seleccione el servicio de AVS en el que quiere crear la nube privada de AVS.
4. En **Información general**, haga clic en **Crear nube privada de AVS** para abrir una nueva pestaña del explorador del portal de AVS. Si se le solicita, inicie sesión con las credenciales de inicio de sesión en Azure.

    ![Creación de una nube privada de AVS desde Azure](media/create-private-cloud-from-azure.png)

5. En el portal de AVS, proporcione un nombre para su nube privada de AVS.
6. Seleccione una opción de **Ubicación** para la nube privada de AVS.
7. Seleccione **tipo de nodo** de acuerdo con lo que haya comprado en Azure.
8. Especifique el **número de nodos**. Para crear una nube privada de AVS, se requiere un mínimo de tres nodos.
5. En el portal de CloudSimple, proporcione un nombre para su nube privada.
6. Seleccione la **ubicación** para la nube privada.
7. Seleccione **Tipo de nodo** de acuerdo con lo que haya aprovisionado en Azure.
8. Especifique el **número de nodos**.  Para crear una nube privada, se requiere un mínimo de tres nodos.

    ![Creación de una nube privada de AVS: información básica](media/create-private-cloud-basic-info.png)

9. Haga clic en **Siguiente: Opciones avanzadas**.
10. Escriba el intervalo CIDR de subredes de vSphere/vSAN. Asegúrese de que el intervalo CIDR no se superponga con ninguna de las subredes locales u otras subredes de Azure (redes virtuales) ni con la subred de la puerta de enlace.

    **Opciones de intervalo de CIDR:** /24, /23, /22 o /21. Un intervalo de CIDR /24 admite hasta nueve nodos, un intervalo de CIDR /23 admite hasta 41 nodos y un intervalo de CIDR /22 y /21 admite hasta 64 nodos (el número máximo de nodos en una nube privada de AVS).

    > [!IMPORTANT]
    > Las direcciones IP en el intervalo CIDR de vSphere/vSAN están reservadas para que las use la infraestructura de nube privada de AVS. No use la dirección IP de este intervalo en ninguna máquina virtual.

11. Haga clic en **Siguiente: Revisar y crear**.
12. Revise la configuración. Si necesita cambiar la configuración, haga clic en **Anterior**.
13. Haga clic en **Crear**.

Se inicia el proceso de aprovisionamiento de la nube privada de AVS. La nube privada de AVS puede tardar hasta dos horas en aprovisionarse.

Para obtener instrucciones sobre cómo expandir una nube privada de AVS existente, consulte [Expansión de una nube privada de AVS](expand-private-cloud.md).
