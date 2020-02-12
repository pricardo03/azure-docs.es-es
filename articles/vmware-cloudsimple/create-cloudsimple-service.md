---
title: 'Azure VMware Solutions (AVS): creación de un servicio de AVS'
description: Describe cómo crear el servicio de AVS en Azure Portal.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 468ef8751438812422d7eee83d98acc9e3aedb82
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024830"
---
# <a name="create-the-azure-vmware-solutions-avs-service"></a>Creación del servicio de Azure VMware Solutions (AVS)

Para empezar a trabajar con Azure VMware Solutions (AVS), cree este servicio en Azure Portal.

## <a name="before-you-begin"></a>Antes de empezar

Asigne un bloque CIDR /28 para la subred de puerta de enlace. Se requiere una subred de puerta de enlace por cada servicio de AVS y es única para la región en la que se crea. La subred de puerta de enlace se usa para los servicios de red perimetral y requiere un bloque CIDR /28. El espacio de direcciones de subred de puerta de enlace debe ser único. No se debe superponer con otras redes que se comuniquen con el entorno de AVS. Las redes que se comunican con AVS incluyen redes locales y redes virtuales de Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-the-service"></a>Creación del servicio

1. Seleccione **Todos los servicios**.
2. Busque **AVS Services** (Servicios de AVS).
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

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [aprovisionar nodos](create-nodes.md).
* Aprenda a [crear una nube privada de AVS](create-private-cloud.md)
* Aprenda a [configurar un entorno de nube privada de AVS](quickstart-create-private-cloud.md)
