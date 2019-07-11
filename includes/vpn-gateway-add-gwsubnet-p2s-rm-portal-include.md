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
ms.openlocfilehash: b044912fd8f52f3f4fdbe1b3a74b64f9b565ddf0
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673526"
---
1. En el [portal](https://portal.azure.com), navegue a la red virtual de Resource Manager para la que desea crear una puerta de enlace de red virtual.
2. En la sección **Configuración** de la página de redes virtuales, haga clic en **Subredes** para expandir la página **Subredes**.
3. En la página **Subredes**, haga clic en **+Subred** para abrir la página **Agregar subred**. 

   ![Agregar la subred de la puerta de enlace](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Agregar la subred de la puerta de enlace")
4. El **nombre** de la subred se rellena automáticamente con el valor "GatewaySubnet". Este valor es necesario para que Azure reconozca que se trata de subred de puerta de enlace. Modifique los valores de **Intervalo de direcciones** rellenados automáticamente para ajustarlos a sus requisitos de configuración. No configure los punto de conexión de servicio o de tabla de rutas.

   ![Agregar la subred](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Agregar la subred")
5. Haga clic en **Aceptar** en la parte inferior de la página para crear la subred.
