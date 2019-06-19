---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5642533fe1015e88c3b27e83139bfd26cb0b1a53
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186183"
---
1. En el [portal](http://portal.azure.com), navegue a la red virtual de Resource Manager para la que desea crear una puerta de enlace de red virtual.
2. En la sección **Configuración** de la página de redes virtuales, haga clic en **Subredes** para expandir la página **Subredes**.
3. En la página **Subredes**, haga clic en **+Subred** para abrir la página **Agregar subred**. 

   ![Agregar la subred de la puerta de enlace](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Agregar la subred de la puerta de enlace")
4. El **nombre** de la subred se rellena automáticamente con el valor "GatewaySubnet". Este valor es necesario para que Azure reconozca que se trata de subred de puerta de enlace. Modifique los valores de **Intervalo de direcciones** rellenados automáticamente para ajustarlos a sus requisitos de configuración. No configure los punto de conexión de servicio o de tabla de rutas.

   ![Agregar la subred](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Agregar la subred")
5. Haga clic en **Aceptar** en la parte inferior de la página para crear la subred.
