---
title: 'Inicio rápido de Azure VMware Solutions (AVS): Creación de un servicio'
description: Obtenga información sobre cómo crear el servicio de AVS, y sobre cómo adquirir y reservar nodos.
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e7eb414e51ca38f524ab83bfb51f80f771524287
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024439"
---
# <a name="quickstart---create-azure-vmware-solutions-avs-service"></a>Inicio rápido: Creación de un servicio de Azure VMware Solutions (AVS)

Para empezar, cree la instancia de Azure VMware Solutions (AVS) en Azure Portal.

## <a name="vmware-solutions-avs---service-overview"></a>VMware Solutions (AVS): información general del servicio

El servicio de AVS le permite consumir Azure VMware Solution by AVS. Al crear el servicio, puede aprovisionar nodos y reservarlos, y crear nubes privadas de AVS. El servicio de AVS se agrega en cada región de Azure donde está disponible. El servicio define la red perimetral de Azure VMware Solution by AVS. Esta red perimetral se usa para servicios que incluyen VPN, ExpressRoute y conectividad de Internet a las nubes privadas de AVS.

Para agregar el servicio de AVS, debe crear una subred de puerta de enlace. La subred de puerta de enlace se usa al crear la red perimetral y requiere un bloque CIDR /28. El espacio de direcciones de subred de puerta de enlace debe ser único. No puede superponerse con ninguno de los espacios de direcciones de red local ni con el espacio de direcciones de red virtual de Azure.

## <a name="before-you-begin"></a>Antes de empezar

Asigne un bloque CIDR /28 para la subred de puerta de enlace. Se requiere una subred de puerta de enlace por cada servicio de AVS y es única para la región en la que se crea. Los servicios de red perimetral de Azure VMware Solution by AVS usan la subred de la puerta de enlace, que requiere un bloque CIDR /28. El espacio de direcciones de subred de puerta de enlace debe ser único. No se debe superponer con otras redes que se comuniquen con el entorno de AVS. Las redes que se comunican con AVS incluyen redes locales y redes virtuales de Azure.

Consulte [Requisitos previos de redes avanzadas](cloudsimple-network-checklist.md). 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-the-service"></a>Creación del servicio

1. Seleccione **Todos los servicios**.
2. Busque **AVS Service** (Servicio de AVS).

    ![Buscar el servicio de AVS](media/create-cloudsimple-service-search.png)

3. Seleccione **AVS Services** (Servicios de AVS).
4. Haga clic en **Agregar** para crear un servicio.

    ![Agregar el servicio de AVS](media/create-cloudsimple-service-add.png)

5. Seleccione la suscripción en la que quiere crear el servicio de AVS.
6. Seleccione el grupo de recursos del servicio. Para agregar un nuevo grupo de recursos, haga clic en **Crear nuevo**.
7. Escriba un nombre para identificar el servicio.
8. Escriba el CIDR de la puerta de enlace de servicio. Especifique una subred /28 que no se superponga con ninguna de sus subredes locales, las subredes de Azure ni las subredes planeadas de AVS. No puede cambiar el CIDR después de crear el servicio.

    ![Creación del servicio de AVS](media/create-cloudsimple-service.png)

9. Haga clic en **OK**.

El servicio se crea y se agrega a la lista de servicios.

## <a name="provision-nodes"></a>Aprovisionamiento de nodos

Para configurar la capacidad de pago por uso de un entorno de nube privada de AVS, aprovisione primero los nodos en Azure Portal.

1. Seleccione **Todos los servicios**.
2. Busque **AVS Nodes** (Nodos de AVS).

    ![Buscar nodos de AVS](media/create-cloudsimple-node-search.png)

3. Seleccione **AVS Nodes** (Nodos de AVS).
4. Haga clic en **Agregar** para crear los nodos.

    ![Agregar nodos de AVS](media/create-cloudsimple-node-add.png)

5. Seleccione la suscripción en la que quiere aprovisionar los nodos de AVS.
6. Seleccione el grupo de recursos de los nodos. Para agregar un nuevo grupo de recursos, haga clic en **Crear nuevo**.
7. Escriba el prefijo para identificar los nodos.
8. Seleccione la ubicación de los recursos de los nodos.
9. Seleccione la ubicación dedicada en la que hospedar los recursos de los nodos.
10. Seleccione el [tipo de nodo](cloudsimple-node.md).
11. Seleccione el número de nodos que quiere aprovisionar.
12. Seleccione **Revisar + crear**.
13. Revise la configuración. Para modificar la configuración, haga clic en **Anterior**.
14. Seleccione **Crear**.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de la nube privada de AVS y configuración del entorno](quickstart-create-private-cloud.md)
* Más información sobre el [servicio de AVS](https://docs.azure.cloudsimple.com/cloudsimple-service)
