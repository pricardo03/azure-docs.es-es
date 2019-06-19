---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: eb2555cdc163ee0f88149248b9e7d83a51d34a1b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186161"
---
1. En [Azure Portal](http://portal.azure.com), seleccione la red virtual de Resource Manager para la que quiere crear una puerta de enlace de red virtual.

2. En la sección **Configuración** de la página de redes virtuales, seleccione **Subredes** para expandir la página **Subredes**.

3. En la página **Subredes**, haga clic en **Subred de puerta de enlace** para abrir la página **Agregar subred**.

   ![Agregar la subred de la puerta de enlace](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Agregar la subred de la puerta de enlace")

4. El **nombre** de la subred se rellena automáticamente con el valor *GatewaySubnet*. Este valor es necesario para que Azure reconozca que se trata de subred de una puerta de enlace. Ajuste los valores de **Intervalo de direcciones** que se rellenan automáticamente para que coincidan con sus requisitos de configuración y seleccione **Aceptar** para crear la subred.

   ![Agregar la subred](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Agregar la subred")