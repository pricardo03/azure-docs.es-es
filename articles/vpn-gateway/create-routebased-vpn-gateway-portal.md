---
title: 'Creación de una puerta de enlace VPN basada en ruta: Azure Portal | Microsoft Docs'
description: Creación de una instancia de VPN Gateway basada en rutas mediante Azure Portal
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 08/02/2019
ms.author: cherylmc
ms.openlocfilehash: 2a04c0fa2d92514103377c2aef420290d1bdd057
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2019
ms.locfileid: "68781169"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Creación de una instancia de VPN Gateway basada en rutas mediante Azure Portal

En este artículo se explica cómo crear rápidamente una instancia de Azure VPN Gateway basada en rutas mediante Azure Portal.  Una instancia de VPN Gateway se usa al crear una conexión VPN a la red local. También puede utilizar una instancia de VPN Gateway para conectar redes virtuales. 

Los pasos que se describen en este artículo crearán una red virtual, una subred, una subred de puerta de enlace y una instancia de VPN Gateway basada en rutas (puerta de enlace de red virtual). Una vez completada la creación de la puerta de enlace, puede crear las conexiones. Estos pasos requieren una suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="vnet"></a>Creación de una red virtual

[!INCLUDE [create-gateway](../../includes/vpn-gateway-create-virtual-network-portal-include.md)]

## <a name="gwsubnet"></a>Incorporación de una subred de puerta de enlace

[!INCLUDE [gateway subnet](../../includes/vpn-gateway-add-gateway-subnet-portal-include.md)]

## <a name="gwvalues"></a>Configuración y creación de la puerta de enlace

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

>[!NOTE]
>La SKU de puerta de enlace de nivel Básico no admite la autenticación de IKEv2 o RADIUS. Si planea que clientes Mac se conecten a la red virtual, no use la SKU de nivel Básico.

## <a name="viewgw"></a>Visualización de VPN Gateway

1. Después de crea la puerta de enlace, vaya a VNet1 en el portal. La instancia de VPN Gateway aparece en la página Información general como un dispositivo conectado.

   ![Dispositivos conectados](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "Connected devices")

2. En la lista de dispositivos, haga clic en **VNet1GW** para ver más información.

   ![Ver instancia de VPN Gateway](./media/create-routebased-vpn-gateway-portal/view-gateway.png "View VPN gateway")

## <a name="next-steps"></a>Pasos siguientes

Una vez creada la puerta de enlace, puede crear una conexión entre su red virtual y otra red virtual. O bien, cree una conexión entre su red virtual y una ubicación local.

> [!div class="nextstepaction"]
> [Creación de una conexión de sitio a sitio](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Creación de una conexión de punto a sitio](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Creación de una conexión a otra red virtual](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
