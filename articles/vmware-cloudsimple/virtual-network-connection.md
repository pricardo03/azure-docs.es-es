---
title: Conexión de la red virtual de Azure a AVS mediante ExpressRoute
description: Describe cómo obtener información de emparejamiento para una conexión entre la red virtual de Azure y el entorno de AVS
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1ff11bbafe6f9057ce70c799e0437691d89ccb40
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014409"
---
# <a name="connect-azure-virtual-network-to-avs-using-expressroute"></a>Conexión de la red virtual de Azure a AVS mediante ExpressRoute

Puede extender la red de la nube privada de AVS a la red virtual de Azure y a los recursos de Azure. Una conexión de ExpressRoute permite acceder a recursos que se ejecutan en la suscripción de Azure desde la nube privada de AVS.

## <a name="request-authorization-key"></a>Solicitud de la clave de autorización

Se requiere una clave de autorización para la conexión de ExpressRoute entre la nube privada de AVS y la red virtual de Azure. Para obtenerla, abra una incidencia de <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">soporte técnico</a>. Use la siguiente información para la solicitud:

* Tipo de problema: **Técnico**
* Suscripción: **Seleccione la suscripción en la que se implementa el servicio AVS**.
* Servicio: **VMware Solutions (AVS)**
* Tipo de problema: **Solicitud de servicio**
* Subtipo de problema: **Clave de autorización para la conexión de red virtual de Azure**
* Asunto: **Solicitud de clave de autorización para la conexión de red virtual de Azure**

## <a name="get-peering-information-from-avs-portal"></a>Obtención de información de emparejamiento desde el portal de AVS

Para configurar la conexión, debe establecer una conexión entre la red virtual de Azure y el entorno de AVS. Como parte del procedimiento, debe proporcionar el URI del circuito de emparejamiento y la clave de autorización. Obtenga el URI y la clave de autorización del [portal de AVS](access-cloudsimple-portal.md). Seleccione **Red** en el menú lateral y **Azure Network Connection** (Conexión de red de Azure). O bien, seleccione **Cuenta** en el menú lateral y **Azure network connection** (Conexión de red de Azure).

Copie el URI del circuito de emparejamiento y de la clave de autorización para cada una de las regiones con el icono de *copia*. Para cada región de AVS que quiera conectar:

1. Haga clic en **Copiar** para copiar el URI. Péguelo en un archivo donde pueda estar disponible para agregarlo a Azure Portal. 
2. Haga clic en **Copiar** para copiar la clave de autorización y péguela también en el archivo.

Copie la clave de autorización y el URI del circuito de emparejamiento que está en estado **Disponible**. El estado **Usado** indica que la clave ya se ha usado para crear una conexión de red virtual.

![Página de conexión de la red virtual](media/virtual-network-connection.png)

Para más información para configurar el vínculo de la red virtual de Azure a AVS, consulte [Conexión del entorno de nube privada de AVS a la red virtual de Azure mediante ExpressRoute](azure-expressroute-connection.md).

## <a name="next-steps"></a>Pasos siguientes

* [Conexión de red virtual de Azure a la nube privada de AVS](azure-expressroute-connection.md)
* [Conexión a la red local mediante Azure ExpressRoute](on-premises-connection.md)
