---
title: Creación de una conexión VPN de sitio a sitio a través del emparejamiento privado de ExpressRoute en Azure virtual WAN | Microsoft Docs
description: En este tutorial, aprenderá a usar Azure Virtual WAN para crear una conexión VPN de sitio a sitio a través del emparejamiento privado de ExpressRoute.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 10/11/2019
ms.author: cherylmc
Customer intent: I want to connect my on-premises networks to my VNets using S2S VPN connection over my ExpressRoute private peering using Azure Virtual WAN.
ms.openlocfilehash: 6272d6fe6f8c35c06a8121e10be2dd5a2e5512a8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510961"
---
# <a name="create-a-site-to-site-vpn-connection-over-expressroute-private-peering-using-azure-virtual-wan"></a>Creación de una conexión VPN de sitio a sitio a través del emparejamiento privado de ExpressRoute con Azure Virtual WAN

En este artículo se muestra cómo usar Virtual WAN para establecer una conexión VPN de IPsec/IKE desde la red local a Azure a través del emparejamiento privado de un circuito ExpressRoute. Esto puede proporcionar un tránsito cifrado entre las redes locales y las redes virtuales de Azure a través de ExpressRoute, sin pasar por la red pública de Internet o mediante direcciones IP públicas.

## <a name="topology-and-routing"></a>Topología y enrutamiento

En el diagrama siguiente se muestra un ejemplo de conectividad de emparejamiento privado de VPN a través de ExpressRoute:

![VPN a través de ExpressRoute](./media/vpn-over-expressroute/vwan-vpn-over-er.png)

El diagrama muestra una red dentro de la red local conectada a la puerta de enlace de VPN del centro de conectividad de Azure a través del emparejamiento privado de ExpressRoute. El establecimiento de la conectividad es sencillo:

1. Establezca la conectividad de ExpressRoute con un circuito ExpressRoute y el emparejamiento privado
2. Establezca la conectividad VPN como se describe en este documento

Un aspecto importante de esta configuración es el enrutamiento entre las redes locales y Azure a través de las rutas de acceso de ExpressRoute y VPN.

### <a name="traffic-from-on-premises-networks-to-azure"></a>Tráfico desde redes locales a Azure

Para el tráfico de las redes locales a Azure, los prefijos de Azure (incluido el concentrador virtual y todas las redes virtuales de radios conectadas al concentrador) se anunciarán a través del BGP del emparejamiento privado de ExpressRoute y el BGP de VPN. Esto dará como resultado dos rutas de red (rutas de acceso) hacia Azure desde las redes locales: una a través de la ruta de acceso protegida por IPsec y otra directamente a través de ExpressRoute **sin** protección de IPsec. Para garantizar que el cifrado se aplica a la comunicación, debe asegurarse de que para la red conectada a VPN en el diagrama, se prefieren las rutas de Azure a través de la puerta de enlace de VPN local sobre la ruta de acceso de ExpressRoute directa.

### <a name="traffic-from-azure-to-on-premises-networks"></a>Tráfico desde Azure a las redes locales

El mismo requisito se aplica al tráfico de Azure a las redes locales. Para asegurarse de que la ruta de acceso de IPsec es preferible a la ruta de acceso directa de ExpressRoute (sin IPsec), tiene dos opciones:

- Anuncie prefijos más específicos en la sesión BGP de VPN para la red conectada a VPN. Puede anunciar un intervalo mayor que abarque la "red conectada a VPN" a través del emparejamiento privado de ExpressRoute y, después, intervalos más específicos en la sesión BGP de VPN. Por ejemplo, anuncie 10.0.0.0/16 a través de ExpressRoute y 10.0.1.0/24 a través de VPN.

- Anuncie prefijos disjuntos para VPN y ExpressRoute. Si los intervalos de red conectados a VPN no están unidos a otra red conectada de ExpressRoute, puede anunciar los prefijos en las sesiones de BGP ExpressRoute y VPN, respectivamente. Por ejemplo, anuncie 10.0.0.0/24 a través de ExpressRoute y 10.0.1.0/24 a través de VPN.

En ambos ejemplos, Azure enviará tráfico a 10.0.1.0/24 a través de la conexión VPN en lugar de hacerlo directamente a través de ExpressRoute sin protección VPN.

> [!WARNING]
> Si anuncia los **mismos** prefijos a través de conexiones de ExpressRoute y VPN, Azure **usará la ruta de acceso de ExpressRoute directamente sin protección de VPN**.
>

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="openvwan"></a>1. Creación de una red virtual WAN y un concentrador con puertas de enlace

Antes de continuar, los siguientes recursos de Azure y las configuraciones locales correspondientes deben estar en vigor:

1. Una Azure Virtual WAN
2. Un concentrador WAN virtual con una [puerta de enlace de ExpressRoute](virtual-wan-expressroute-portal.md) y una [puerta de enlace de VPN](virtual-wan-site-to-site-portal.md)

Consulte [Creación de una asociación de ExpressRoute mediante Azure Virtual WAN](virtual-wan-expressroute-portal.md) para conocer los pasos para crear una red WAN virtual de Azure y un concentrador con una asociación de ExpressRoute y [Creación de una conexión de sitio a sitio mediante la Azure Virtual WAN](virtual-wan-site-to-site-portal.md) para ver los pasos para crear una puerta de enlace de VPN en la WAN virtual.

## <a name="site"></a>2. Creación de un sitio para la red local

El recurso de sitio es el mismo que el de los sitios VPN que no son de ExpressRoute para la WAN virtual. Lo más importante que debe tener en cuenta es que la dirección IP del dispositivo VPN local ahora puede ser una dirección IP privada o una dirección IP pública en la red local, accesible mediante el emparejamiento privado de ExpressRoute creado en el paso 1.

> [!NOTE]
> La dirección IP del dispositivo VPN local DEBE formar parte de los prefijos de dirección anunciados en el concentrador Virtual WAN a través del emparejamiento privado de ExpressRoute de Azure.
>

1. En el explorador, vaya a Azure Portal. Haga clic en la red WAN que ha creado. En la página de la red WAN, en **Conectividad**, haga clic en **Sitios de VPN** para abrir la página de los sitios de VPN.

2. En la página **Sitios de VPN**, haga clic en **+Crear sitio**.

3. En la página **Crear sitio** rellene los campos siguientes:

   * **Suscripción**: compruebe la suscripción.
   * **Grupos de recursos:** : seleccione o cree el grupo de recursos que desea utilizar.
   * **Región**: la región de Azure para el recurso de sitio VPN.
   * **Nombre**: el nombre que desee usar para hacer referencia a su sitio local.
   * **Proveedor de dispositivos**: el proveedor del dispositivo VPN local.
   * **Protocolo de puerta de enlace de borde**: seleccione "Habilitar" si la red local usa BGP.
   * **Espacio de direcciones privadas**: es el espacio de direcciones IP que se encuentra en el sitio local. El tráfico destinado a este espacio de direcciones se enruta a la red local a través de la puerta de enlace de VPN.
   * **Concentradores**: seleccione uno o más concentradores para conectar este sitio VPN. Los concentradores seleccionados deben tener puertas de enlace de VPN ya creadas.

4. Haga clic en **Siguiente: Vínculos >** para la configuración de vínculo de VPN:

   * **Nombre del vínculo**: el nombre que desee usar para hacer referencia a esta conexión.
   * **Nombre del proveedor**: el nombre del proveedor de servicios de Internet para este sitio. En el caso de la red local de ExpressRoute, el nombre del proveedor de servicios de ExpressRoute.
   * **Velocidad**: la velocidad del vínculo de servicio de Internet o el circuito ExpressRoute.
   * **Dirección IP**: la dirección IP pública del dispositivo VPN que reside en su sitio local. O bien, en el caso de ExpressRoute local, la dirección IP privada del dispositivo VPN a través de ExpressRoute.

   Si BGP está habilitado, se aplicará a todas las conexiones creadas para este sitio en Azure. La configuración de BGP en Virtual WAN es equivalente a la configuración de BGP en Azure VPN Gateway. Su dirección del mismo nivel BGP local *no debe* ser la misma que la dirección IP del VPN al dispositivo ni que el espacio de direcciones de red virtual del sitio VPN. Use otra dirección IP en el dispositivo VPN para la dirección IP del par BGP. Puede ser una dirección asignada a la interfaz de bucle invertido en el dispositivo. Sin embargo, *no puede* ser una dirección APIPA (169.254.*x*.*x*). Especifique esta dirección en la puerta de enlace de red local correspondiente que representa la ubicación. Para conocer los requisitos previos de BGP, consulte [Acerca de BGP con Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md).

5. Haga clic en **Siguiente: Revisar + Crear >** para comprobar los valores de configuración y crear el sitio VPN. Si seleccionó **Concentradores** para conectarse, se establecerá la conexión entre la red local y la puerta de enlace de VPN de concentrador.

## <a name="hub"></a>3. Actualización de la configuración de conexión VPN para usar ExpressRoute

Después de crear el sitio VPN y conectarse al concentrador, siga estos pasos para configurar la conexión para usar el emparejamiento privado de ExpressRoute:

1. Vuelva a la página de recursos de WAN virtual y haga clic en el recurso de concentrador. O bien, vaya desde el sitio VPN hasta el concentrador conectado.

2. En **Conectividad**, haga clic en **VPN (de sitio a sitio)**

3. Haga clic en "..." en el sitio VPN sobre ExpressRoute y elija "**Editar conexión VPN para este concentrador**"

4. Seleccione "Sí" en "**Usar dirección IP privada de Azure**". El ajuste configura la puerta de enlace de VPN de concentrador para que use direcciones IP privadas dentro del intervalo de direcciones del concentrador en la puerta de enlace para esta conexión, en lugar de las direcciones IP públicas. Esto garantizará que el tráfico de la red local atraviesa las rutas de acceso de emparejamiento privado de ExpressRoute en lugar de usar la red pública de Internet para esta conexión VPN. En la captura de pantalla siguiente se muestra la ventana de configuración.

   ![Configuración de la conexión VPN](./media/vpn-over-expressroute/vpn-link-configuration.png)
   
5. Haga clic en **Save**(Guardar).

Una vez guardada, la puerta de enlace de VPN de concentrador usará las direcciones IP privadas en la puerta de enlace de VPN para establecer las conexiones de IPsec/IKE con el dispositivo VPN local a través de ExpressRoute.

## <a name="associate"></a>4. Obtención de direcciones IP privadas de puerta de enlace de VPN de concentrador

Descargue la configuración del dispositivo VPN para obtener las direcciones IP privadas de la puerta de enlace de VPN de concentrador. Estas son necesarias para configurar el dispositivo VPN local.

1. En la página del concentrador, haga clic en **VPN (sitio a sitio)** en **Conectividad**

2. En la parte superior de la página de información general, haga clic en **Descargar configuración VPN**. Azure crea una cuenta de almacenamiento en el grupo de recursos "microsoft - network-[ubicación]", donde ubicación es la ubicación de la red WAN. Una vez que haya aplicado la configuración a los dispositivos VPN, puede eliminar esta cuenta de almacenamiento.

3. Una vez el archivo se haya terminado de crear, puede hacer clic en el vínculo para descargarlo.

4. Aplique la configuración al dispositivo VPN.

### <a name="understanding-the-vpn-device-configuration-file"></a>Información sobre el archivo de configuración del dispositivo VPN

El archivo de configuración de dispositivo contiene la configuración que se debe usar al configurar el dispositivo VPN local. Cuando visualice este archivo, tenga en cuenta la siguiente información:

* **vpnSiteConfiguration**: en esta sección se indica la configuración de los detalles del dispositivo como un sitio de conexión a la red virtual WAN. Incluye el nombre y la dirección IP pública del dispositivo de rama.
* **vpnSiteConnections**: en esta sección se proporciona información sobre la siguiente configuración:

    * **Espacio de direcciones** de la red virtual de concentradores virtuales<br/>Ejemplo:
           ```
           "AddressSpace":"10.51.230.0/24"
           ```
    * **Espacio de direcciones** de las redes virtuales que están conectadas al concentrador<br>Ejemplo:
           ```
           "ConnectedSubnets":["10.51.231.0/24"]
            ```
    * **Direcciones IP** de vpngateway del concentrador virtual. Dado que cada conexión del elemento vpngateway consta de dos túneles con una configuración activo-activo, verá ambas direcciones IP en este archivo. En este ejemplo, verá "Instance0" y "Instance1" para cada sitio, son direcciones IP privadas en lugar de direcciones IP públicas.<br>Ejemplo:
           ``` 
           "Instance0":"10.51.230.4"
           "Instance1":"10.51.230.5"
           ```
    * **Detalles de configuración de conexión de Vpngateway** como BGP, clave precompartida, etc. La PSK es la clave precompartida que se genera automáticamente para usted. Puede modificar la conexión cuando quiera en la página de información general para una PSK personalizada.
  
### <a name="example-device-configuration-file"></a>Archivo de configuración de dispositivo de ejemplo

```
[{
      "configurationVersion":{
        "LastUpdatedTime":"2019-10-11T05:57:35.1803187Z",
        "Version":"5b096293-edc3-42f1-8f73-68c14a7c4db3"
      },
      "vpnSiteConfiguration":{
        "Name":"VPN-over-ER-site",
        "IPAddress":"172.24.127.211",
        "LinkName":"VPN-over-ER"
      },
      "vpnSiteConnections":[{
        "hubConfiguration":{
          "AddressSpace":"10.51.230.0/24",
          "Region":"West US 2",
          "ConnectedSubnets":["10.51.231.0/24"]
        },
        "gatewayConfiguration":{
          "IpAddresses":{
            "Instance0":"10.51.230.4",
            "Instance1":"10.51.230.5"
          }
        },
        "connectionConfiguration":{
          "IsBgpEnabled":false,
          "PSK":"abc123",
          "IPsecParameters":{"SADataSizeInKilobytes":102400000,"SALifeTimeInSeconds":3600}
        }
      }]
    },
    {
      "configurationVersion":{
        "LastUpdatedTime":"2019-10-11T05:57:35.1803187Z",
        "Version":"fbdb34ea-45f8-425b-9bc2-4751c2c4fee0"
      },
      "vpnSiteConfiguration":{
        "Name":"VPN-over-INet-site",
        "IPAddress":"13.75.195.234",
        "LinkName":"VPN-over-INet"
      },
      "vpnSiteConnections":[{
        "hubConfiguration":{
          "AddressSpace":"10.51.230.0/24",
          "Region":"West US 2",
          "ConnectedSubnets":["10.51.231.0/24"]
        },
        "gatewayConfiguration":{
          "IpAddresses":{
            "Instance0":"51.143.63.104",
            "Instance1":"52.137.90.89"
          }
        },
        "connectionConfiguration":{
          "IsBgpEnabled":false,
          "PSK":"abc123",
          "IPsecParameters":{"SADataSizeInKilobytes":102400000,"SALifeTimeInSeconds":3600}
        }
      }]
}]
```

### <a name="configuring-your-vpn-device"></a>Configuración del dispositivo VPN

Si necesita instrucciones para configurar el dispositivo, puede utilizar las que se proporcionan en la [página de scripts de configuración de dispositivo VPN](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) con las siguientes advertencias:

* Las instrucciones que aparecen en la página de dispositivos VPN no están escritas para Virtual WAN, pero puede usar los valores de Virtual WAN desde el archivo de configuración para configurar manualmente el dispositivo VPN. 
* Los scripts descargables de configuración de dispositivo que son para VPN Gateway no funcionan para Virtual WAN, ya que la configuración es diferente.
* Las redes WAN virtuales nuevas admiten IKEv1 e IKEv2.
* Virtual WAN solo puede usar dispositivos VPN e instrucciones de dispositivo basados en rutas.

## <a name="viewwan"></a>5. Visualizar la instancia de Virtual WAN

1. Vaya a la instancia de Virtual WAN.

2. En la página Información general, cada punto del mapa representa un concentrador. Mantenga el mouse sobre cualquier punto para ver el resumen de estado del concentrador.

3. En la sección de concentradores y conexiones, puede ver estado del concentrador, sitio, región, estado de la conexión VPN y bytes de entrada y salida.

## <a name="viewhealth"></a>6. Visualización del estado de los recursos

1. Vaya a su red WAN.

2. En la página de la WAN, en la sección de **Soporte técnico y solución de problemas**, haga clic en **Mantenimiento** y visualice los recursos.

## <a name="connectmon"></a>7. Supervisar una conexión

Cree una conexión para supervisar la comunicación entre una máquina virtual de Azure y un sitio remoto. Para información acerca de cómo configurar una supervisión de conexión, consulte [Supervisar la comunicación de red](~/articles/network-watcher/connection-monitor.md). El campo de origen es la IP de la máquina virtual en Azure y la dirección IP de destino es la dirección IP del sitio.

## <a name="cleanup"></a>8. Limpieza de recursos

Cuando ya no necesite estos recursos, puede usar [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene. Reemplace "myResourceGroup" con el nombre del grupo de recursos y ejecute el siguiente comando de PowerShell:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

Este artículo le ayuda a crear una conexión VPN a través del emparejamiento privado de ExpressRoute con Virtual WAN. Para más información sobre Virtual WAN y otras características relacionadas, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).
