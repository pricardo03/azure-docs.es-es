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
ms.openlocfilehash: 42e983ead6f7562c6a31cf9ef4ad2d97d0ff9707
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673521"
---
1. En [Azure Portal](https://portal.azure.com), seleccione la red virtual de Resource Manager para la que quiere crear una puerta de enlace de red virtual.

2. En la sección **Configuración** de la página de redes virtuales, seleccione **Subredes** para expandir la página **Subredes**.

3. En la página **Subredes**, haga clic en **Subred de puerta de enlace** para abrir la página **Agregar subred**.

   ![Agregar la subred de la puerta de enlace](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Agregar la subred de la puerta de enlace")

4. El **nombre** de la subred se rellena automáticamente con el valor *GatewaySubnet*. Este valor es necesario para que Azure reconozca que se trata de subred de una puerta de enlace. Ajuste los valores de **Intervalo de direcciones** que se rellenan automáticamente para que coincidan con sus requisitos de configuración y seleccione **Aceptar** para crear la subred.

   ![Agregar la subred](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Agregar la subred")
