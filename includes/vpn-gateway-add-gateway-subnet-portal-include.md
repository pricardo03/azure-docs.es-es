---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 52ab0413dffeee59cb9d34c6276a0c806a4d0322
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780110"
---
La subred de puerta de enlace contiene las direcciones IP reservadas que usan los servicios de puerta de enlace de la red virtual. Cree una subred de puerta de enlace.

1. En el portal, navegue a la red virtual para la que desea crear una puerta de enlace de red virtual.
2. En la página de la red virtual, haga clic en **Subredes** para expandir la página **VNet1 - Subredes**.
3. Haga clic en **+Subred de puerta de enlace** en la parte superior para abrir la página **Agregar subred**.

   ![Agregar la subred de la puerta de enlace](./media/vpn-gateway-add-gateway-subnet-portal-include/gateway-subnet.png "Agregar la subred de la puerta de enlace")
4. El campo **Nombre** de la subred se rellena automáticamente con el valor requerido "GatewaySubnet". Ajuste **Intervalo de direcciones (bloque CIDR)** para que coincida con el valor siguiente:

   **Intervalo de direcciones (bloque CIDR)** : 10.1.255.0/27

   ![Agregar la subred de la puerta de enlace](./media/vpn-gateway-add-gateway-subnet-portal-include/add-gateway-subnet1.png "Agregar la subred de la puerta de enlace")
5. Deje el resto de la configuración como los valores predeterminados de **Ninguno** o **0 seleccionado**. Luego, haga clic en **Aceptar** para crear la subred de puerta de enlace.