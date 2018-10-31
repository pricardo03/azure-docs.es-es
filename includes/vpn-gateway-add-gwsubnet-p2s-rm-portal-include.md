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
ms.openlocfilehash: bc1e2803a420b95e1abec0886733c5e42a8cfdb4
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026649"
---
1. En el [portal](http://portal.azure.com), navegue a la red virtual de Resource Manager para la que desea crear una puerta de enlace de red virtual.
2. En la sección **Configuración** de la página de redes virtuales, haga clic en **Subredes** para expandir la página **Subredes**.
3. En la página **Subredes**, haga clic en **+Subred** para abrir la página **Agregar subred**. 

  ![Agregar la subred de la puerta de enlace](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Agregar la subred de la puerta de enlace")
4. El **nombre** de la subred se rellena automáticamente con el valor "GatewaySubnet". Este valor es necesario para que Azure reconozca que se trata de subred de puerta de enlace. Modifique los valores de **Intervalo de direcciones** rellenados automáticamente para ajustarlos a sus requisitos de configuración. No configure los punto de conexión de servicio o de tabla de rutas.

  ![Agregar la subred](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Agregar la subred")
5.  Haga clic en **Aceptar** en la parte inferior de la página para crear la subred.