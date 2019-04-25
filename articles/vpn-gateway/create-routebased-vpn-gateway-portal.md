---
title: 'Crear una puerta de enlace VPN basada en rutas: Azure Portal | Microsoft Docs'
description: Creación de una instancia de VPN Gateway basada en rutas mediante Azure Portal
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 10/18/2018
ms.author: cherylmc
ms.openlocfilehash: ddc42023bae3403e7778327a40316462c85222c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60390077"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Creación de una instancia de VPN Gateway basada en rutas mediante Azure Portal

En este artículo se explica cómo crear rápidamente una instancia de Azure VPN Gateway basada en rutas mediante Azure Portal.  Una instancia de VPN Gateway se usa al crear una conexión VPN a la red local. También puede utilizar una instancia de VPN Gateway para conectar redes virtuales. 

Los pasos que se describen en este artículo crearán una red virtual, una subred, una subred de puerta de enlace y una instancia de VPN Gateway basada en rutas (puerta de enlace de red virtual). Una vez completada la creación de la puerta de enlace, puede crear las conexiones. Estos pasos requieren una suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="vnet"></a>Creación de una red virtual

1. Desde un explorador, navegue al [Portal de Azure](https://portal.azure.com) e inicie sesión con su cuenta de Azure.
2. Haga clic en **Crear un recurso**. En el campo **Buscar en el Marketplace**, escriba "Red virtual". En la lista de resultados, busque **Virtual Network** y haga clic para abrir la página **Virtual Network**.
3. En la parte inferior de la página Virtual Network, en la lista **Seleccionar un modelo de implementación**, verifique que **Resource Manager** está seleccionado en la lista desplegable y haga clic en **Crear**. Se abre la página **Crear red virtual**.
4. En la página **Crear red virtual**, configure los valores de la red virtual. Al rellenar los campos, el signo de exclamación rojo se convierte en una marca de verificación verde cuando los caracteres escritos en el campo sean válidos. Use los valores siguientes:

   - **Nombre**: TestVNet1
   - **Espacio de direcciones**: 10.1.0.0/16
   - **Suscripción**: Compruebe que la suscripción que aparece es el que desea usar. Puede cambiar las suscripciones mediante la lista desplegable.
   - **Grupo de recursos**: TestRG1
   - **Ubicación**: Este de EE. UU
   - **Subred**: Front-end
   - **Intervalo de direcciones**: 10.1.0.0/24

   ![Página Crear red virtual](./media/create-routebased-vpn-gateway-portal/create-virtual-network.png "Página Crear red virtual")
5. Después de escribir los valores, seleccione **Pin to dashboard** (Anclar al panel) para que sea fácil de encontrar la red virtual en el panel y luego haga clic en **Crear**. Después de hacer clic en **Crear**, aparece un icono en el panel que refleja el progreso de la red virtual. El icono cambiará a medida que se vaya creando la red virtual.

## <a name="gwsubnet"></a>Incorporación de una subred de puerta de enlace

La subred de puerta de enlace contiene las direcciones IP reservadas que usan los servicios de puerta de enlace de la red virtual. Cree una subred de puerta de enlace.

1. En el portal, navegue a la red virtual para la que desea crear una puerta de enlace de red virtual.
2. En la página de la red virtual, haga clic en **Subredes** para expandir la página **VNet1 - Subredes**.
3. Haga clic en **+Subred de puerta de enlace** en la parte superior para abrir la página **Agregar subred**.

   ![Agregar la subred de la puerta de enlace](./media/create-routebased-vpn-gateway-portal/gateway-subnet.png "Agregar la subred de la puerta de enlace")
4. El campo **Nombre** de la subred se rellena automáticamente con el valor requerido "GatewaySubnet". Ajuste los valores de **Intervalo de direcciones** rellenados automáticamente para que coincidan con los siguientes valores:

   **Intervalo de direcciones (bloque CIDR)**: 10.1.255.0/27

   ![Adición de la subred de la puerta de enlace](./media/create-routebased-vpn-gateway-portal/add-gateway-subnet.png "Adición de la subred de la puerta de enlace")
5. Para crear la subred de puerta de enlace, haga clic en **Aceptar** en la parte inferior de la página.

## <a name="gwvalues"></a>Configuración de la puerta de enlace

1. En el lado izquierdo de la página del portal, haga clic en **+ Crear un recurso** y escriba "Virtual Network Gateway" en el cuadro de búsqueda. A continuación, presione **Entrar**. En **Resultados**, busque y haga clic en **Puerta de enlace de red virtual**.
2. En la parte inferior de la página "Puerta de enlace de red virtual", haga clic en **Crear** para abrir la página **Crear puerta de enlace de red virtual**.
3. En la página **Crear puerta de enlace de red virtual**, especifique los valores de la puerta de enlace de red virtual.

   - **Nombre**: Vnet1GW
   - **Tipo de puerta de enlace**: VPN 
   - **Tipo de VPN**: basada en rutas
   - **SKU**: VpnGw1
   - **Ubicación**: Este de EE. UU
   - **Red virtual**: Haga clic en **red Virtual/elegir una red virtual** para abrir el **elegir una red virtual** página. Seleccione **VNet1**.
   - **Dirección IP pública**: esta configuración especifica el objeto de dirección IP pública que se asocia a la puerta de enlace de VPN. La dirección IP pública se asigna dinámicamente a este objeto cuando se crea la puerta de enlace de VPN. Actualmente, VPN Gateway solo admite la asignación de direcciones IP públicas *dinámicas*. Sin embargo, esto no significa que la dirección IP cambia después de que se ha asignado a una puerta de enlace VPN. La única vez que la dirección IP pública cambia es cuando la puerta de enlace se elimina y se vuelve a crear. No cambia cuando se cambia el tamaño, se restablece o se realizan actualizaciones u otras operaciones de mantenimiento interno de una puerta de enlace VPN.

     - Mantenga la opción **Crear nueva** seleccionada.
     - En el cuadro de texto, escriba un **Nombre** para la dirección IP pública. Para este ejercicio, use **VNet1GWIP**.<br>

     ![Configuración de la puerta de enlace](./media/create-routebased-vpn-gateway-portal/gw.png "Configure gateway settings")

## <a name="creategw"></a>Creación de la puerta de enlace de VPN

1. Compruebe la configuración en la página **Crear puerta de enlace de red virtual**. Ajuste los valores si es necesario.
2. En la parte inferior de la página, haga clic en **Crear**.

   Después de hacer clic en **Crear**, la configuración se validará y el icono **Implementado puerta de enlace de red virtual** aparecerá en el panel. Una instancia de VPN Gateway puede tardar hasta 45 minutos. Es posible que tenga que actualizar la página de portal para ver el estado completado.

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
