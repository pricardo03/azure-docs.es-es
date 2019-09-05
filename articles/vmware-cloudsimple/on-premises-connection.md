---
title: 'Solución de VMware en Azure de CloudSimple: conexión local mediante ExpressRoute'
description: En este artículo se describe cómo solicitar una conexión local mediante ExpressRoute desde la red de la región de CloudSimple.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ee359b76072da3caee9ae1f5fab3d0fc28d25c0e
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69972684"
---
# <a name="connect-from-on-premises-to-cloudsimple-using-expressroute"></a>Conexión desde una red local a CloudSimple mediante ExpressRoute

Si ya tiene una conexión de Azure ExpressRoute desde una ubicación externa (por ejemplo, local) a Azure, puede conectarla a su entorno de CloudSimple. Puede hacerlo a través de una característica de Azure que permite que dos circuitos ExpressRoute se conecten entre sí. Este método establece entre los dos entornos una conexión segura, privada, de ancho de banda alto y baja latencia.

[![Conexión de ExpressRoute local: Global Reach](media/cloudsimple-global-reach-connection.png)](media/cloudsimple-global-reach-connection.png)

## <a name="prerequisites"></a>Requisitos previos

* Se requiere un circuito Azure ExpressRoute para poder establecer la conexión entre el circuito y las redes de nube privada de CloudSimple.
* Se requiere un usuario con privilegios para crear claves de autorización en un circuito ExpressRoute.

## <a name="scenarios"></a>Escenarios

El hecho de conectar la red local con la red de nube privada permite usar la nube privada de varias maneras, entre las que se incluyen las siguientes:

* Acceso a la red de nube privada sin crear una conexión VPN de sitio a sitio.
* Uso de la instancia local de Active Directory como origen de identidades en la nube privada.
* Migración de máquinas virtuales que se ejecutan de forma local a la nube privada.
* Uso de la nube privada como parte de una solución de recuperación ante desastres.
* Consumo de recursos locales en las máquinas virtuales de carga de trabajo de la nube privada.

## <a name="connecting-expressroute-circuits"></a>Conexión de circuitos ExpressRoute

Para establecer la conexión de ExpressRoute, debe crear una autorización en el circuito ExpressRoute y proporcionar la información de autorización a CloudSimple.

### <a name="create-expressroute-authorization"></a>Creación de la autorización de ExpressRoute

1. Inicie sesión en el Portal de Azure.

2. En la barra de búsqueda superior, busque **Circuito ExpressRoute** y haga clic en **Circuitos ExpressRoute** en **Servicios**.
    [![Circuitos ExpressRoute](media/azure-expressroute-transit-search.png)](media/azure-expressroute-transit-search.png)

3. Seleccione el circuito ExpressRoute que quiere conectar a la red de CloudSimple.

4. En la página de ExpressRoute, haga clic en **Autorizaciones**, escriba el nombre de la autorización y haga clic en **Guardar**.
    [![Autorización de circuito ExpressRoute](media/azure-expressroute-transit-authorizations.png)](media/azure-expressroute-transit-authorizations.png)

5. Para copiar el identificador de recurso y la clave de autorización, haga clic en el icono de copia. Pegue el identificador y la clave en un archivo de texto.
    [![Copia de la autorización de circuito ExpressRoute](media/azure-expressroute-transit-authorization-copy.png)](media/azure-expressroute-transit-authorization-copy.png)

    > [!IMPORTANT]
    > El **identificador de recurso** debe copiarse de la interfaz de usuario y debe tener el formato ```/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/expressRouteCircuits/<express-route-circuit-name>``` cuando se proporcione al servicio de soporte técnico.

6. Abra una incidencia de <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">soporte técnico</a> para la conexión que se va a crear.
    * Tipo de problema: **Técnico**
    * Suscripción: **Suscripción en la que se implementa el servicio CloudSimple**
    * Servicio: **VMware Solution by CloudSimple**
    * Tipo de problema: **Solicitud de servicio**
    * Subtipo de problema: **Creación de una conexión de ExpressRoute a local**
    * Proporcione el identificador de recurso y la clave de autorización que copió y guardó en el panel de detalles.
