---
title: Conexión de la red virtual de Azure a CloudSimple mediante ExpressRoute
description: Describe cómo obtener información de emparejamiento para una conexión entre la red virtual de Azure y el entorno de CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 00d49d763dedc5d86557dadd10f5d727e7893dbe
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69563060"
---
# <a name="connect-azure-virtual-network-to-cloudsimple-using-expressroute"></a>Conexión de la red virtual de Azure a CloudSimple mediante ExpressRoute

Puede extender la red de la nube privada a la red virtual de Azure y a los recursos de Azure. Una conexión de ExpressRoute permite acceder a los recursos que se ejecutan en la suscripción de Azure desde la nube privada.

## <a name="request-authorization-key"></a>Solicitud de la clave de autorización

Se requiere una clave de autorización para la conexión de ExpressRoute entre la nube privada y la red virtual de Azure. Para obtenerla, abra una incidencia de <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">soporte técnico</a>.  Use la siguiente información para la solicitud:

* Tipo de problema: **Técnico**
* Suscripción: **Seleccione la suscripción en la que se implementa el servicio de CloudSimple**
* Servicio: **VMware Solution by CloudSimple**
* Tipo de problema: **Solicitud de servicio**
* Subtipo de problema: **Clave de autorización para la conexión de red virtual de Azure**
* Asunto: **Solicitud de clave de autorización para la conexión de red virtual de Azure**

## <a name="obtain-peering-information-for-azure-virtual-network-to-cloudsimple-connection"></a>Obtención de la información de emparejamiento para una conexión de red virtual de Azure a CloudSimple

Para configurar la conexión, debe establecer un vínculo entre la red virtual de Azure y el entorno de CloudSimple.  Como parte del procedimiento, debe proporcionar el URI del circuito de emparejamiento y la clave de autorización. Obtenga el URI y la clave de autorización del [portal de CloudSimple](access-cloudsimple-portal.md).  Seleccione **Red** en el menú lateral y **Azure Network Connection** (Conexión de red de Azure). O bien, seleccione **Cuenta** en el menú lateral y **Azure network connection** (Conexión de red de Azure).

Observe los iconos de copia del URI del circuito de emparejamiento y de la clave de autorización para cada una de las regiones. Para cada nube privada que desee conectar:

* Haga clic en **Copiar** para copiar el URI. Péguelo en un archivo donde pueda estar disponible para agregarlo a Azure Portal.  
* Haga clic en **Copiar** para copiar la clave de autorización y péguela también en el archivo.

![Página de conexión de la red virtual](media/network-virt-conn-page.png)

Para más información para configurar el vínculo de la red virtual de Azure a CloudSimple, consulte [Connect your CloudSimple Private Cloud environment to the Azure virtual network using ExpressRoute](azure-expressroute-connection.md) (Conexión del entorno de nube privada de CloudSimple a la red virtual de Azure mediante ExpressRoute).
