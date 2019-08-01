---
title: Uso de Azure Virtual WAN para crear una conexión de sitio a sitio a Azure | Microsoft Docs
description: En este tutorial, aprenda a usar Azure Virtual WAN para crear una conexión VPN de sitio a sitio a Azure.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 07/25/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: e9be7ef5c4f37c66f7cbf2c6226936438b367108
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68515162"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan"></a>Tutorial: Creación de una conexión de sitio a sitio mediante Azure Virtual WAN

Este tutorial muestra cómo usar Virtual WAN para conectarse a los recursos de Azure a través de una conexión VPN de IPsec/IKE (IKEv1 e IKEv2). Este tipo de conexión requiere un dispositivo VPN local que tenga una dirección IP pública asignada. Para más información sobre Virtual WAN, consulte la [Introducción a Virtual WAN](virtual-wan-about.md).

> [!NOTE]
> Si tiene muchos sitios, normalmente usará un [asociado de Virtual WAN](https://aka.ms/virtualwan) para crear esta configuración. De todas formas, puede crear esta configuración usted mismo si está familiarizado con las redes y tiene experiencia en la configuración de su propio dispositivo VPN.
>

![Diagrama de Virtual WAN](./media/virtual-wan-about/virtualwan.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una red de área extensa (WAN)
> * Crear un sitio
> * Crear un concentrador
> * Conectar un concentrador con un sitio
> * Crear una red virtual compatible (si aún no tiene una)
> * Conectar una red virtual a un concentrador
> * Descargar y aplicar la configuración de dispositivo VPN
> * Visualizar la instancia de Virtual WAN
> * Visualizar el estado de los recursos
> * Supervisar una conexión

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="openvwan"></a>1. Creación de una instancia de Virtual WAN

Desde un explorador, navegue al [Portal de Azure](https://aka.ms/azurevirtualwanpreviewfeatures) e inicie sesión con su cuenta de Azure.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="site"></a>2. Crear un sitio

Cree tantos sitios como necesite que se correspondan con sus ubicaciones físicas. Por ejemplo, si tiene una sucursal en Nueva York, otra en Londres y otra en Los Ángeles, crearía tres sitios independientes. Estos sitios contienen los puntos de conexión de dispositivo VPN local. Ahora puede especificar solo un espacio de direcciones privadas para su sitio.

1. Haga clic en la red WAN que ha creado. En la página de la red WAN, en **WAN Architecture** (Arquitectura de WAN), haga clic en **Sitios de VPN** para abrir la página de los sitios de VPN.
2. En la página **Sitios de VPN**, haga clic en **+Crear sitio**.
3. En la página **Crear sitio** rellene los campos siguientes:

   * **Nombre**: el nombre que desee usar para hacer referencia a su sitio local.
   * **Dirección IP pública**: la dirección IP pública del dispositivo VPN que reside en su sitio local.
   * **Espacio de direcciones privadas**: es el espacio de direcciones IP que se encuentra en el sitio local. El tráfico destinado a este espacio de direcciones se enruta al sitio local.
   * **Suscripción**: compruebe la suscripción.
   * **Grupo de recursos:** : el grupo de recursos que desea utilizar.
   * **Ubicación**
4. Haga clic en **Mostrar opciones avanzadas** para ver opciones adicionales. 

   Puede seleccionar **BGP** para habilitar BGP, que habilitará la funcionalidad de BGP en todas las conexiones creadas para este sitio en Azure. La configuración de BGP en Virtual WAN es equivalente a la configuración de BGP en Azure VPN Gateway. Su dirección del par BGP local *no debe* ser la misma que la dirección IP pública del dispositivo VPN ni que el espacio de direcciones de red virtual del sitio VPN. Use otra dirección IP en el dispositivo VPN para la dirección IP del par BGP. Puede ser una dirección asignada a la interfaz de bucle invertido en el dispositivo. Sin embargo, *no puede* ser una dirección APIPA (169.254.*x*.*x*). Especifique esta dirección en la puerta de enlace de red local correspondiente que representa la ubicación. Para conocer los requisitos previos de BGP, consulte [Acerca de BGP con Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md).

   También puede incluir **Información del dispositivo** (campos opcionales). Esto puede ayudar al equipo de Azure a comprender mejor su entorno para agregar las posibilidades de optimización adicionales en el futuro, así como para ayudarle a solucionar problemas.
   
5. Haga clic en **Confirmar**.
6. Tras hacer clic en **Confirmar**, vea el estado en la página de sitios de VPN. El sitio pasará de **Aprovisionando** a **Aprovisionado**.

## <a name="hub"></a>3. Crear un concentrador

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="associate"></a>4. Asociar los sitios con el concentrador

Por lo general, los concentradores se deben asociar a sitios que estén en la misma región que en que reside la red virtual.

1. En la página **Sitios de VPN**, seleccione el sitio, o los sitios, que desea asociar con el concentrador y haga clic en **+ Nueva asociación de central**.
2. En la página **Associate sites with one or more hubs** (Asociar sitios con uno o varios concentradores), seleccione un concentrador en la lista desplegable. Para asociar un sitio con concentradores adicionales, haga clic en **+ Agregar una asociación**.
3. También puede agregar un **PSK** concreto aquí, o bien usar el predeterminado.
4. Haga clic en **Confirmar**.
5. Puede ver el estado de la conexión en la pestaña **Sitios de VPN**.

## <a name="vnet"></a>5. Creación de una red virtual

Si no dispone de una red virtual, puede crear rápidamente una mediante PowerShell o en Azure Portal. Si ya tiene una red virtual, verifique que cumple los criterios necesarios y no tiene una puerta de enlace de red virtual.

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="vnet"></a>6. Conexión de la red virtual a un concentrador

En este paso, creará la conexión de emparejamiento entre una red virtual y el concentrador. Repita estos pasos para cada red virtual que desee conectar.

1. En la página de la red WAN virtual, haga clic en **Conexiones de red virtual**.
2. En la página de conexión de red virtual, haga clic en **+ Agregar conexión**.
3. En la página **Agregar conexión**, rellene los campos siguientes:

    * **Nombre de la conexión**: asigne un nombre a la conexión.
    * **Centros**: seleccione el concentrador que desea asociar a esta conexión.
    * **Suscripción**: compruebe la suscripción.
    * **Red virtual**: seleccione la red virtual que quiere conectar con este concentrador. La red virtual no puede tener una puerta de enlace de red virtual ya existente.
4. Haga clic en **Aceptar** para crear la conexión de emparejamiento.

## <a name="device"></a>7. Descarga de la configuración de VPN

Use la configuración del dispositivo VPN para configurar el dispositivo VPN local.

1. En la página de la red virtual WAN, haga clic en **Introducción**.
2. En la parte superior de la página de información general, haga clic en **Descargar configuración VPN**. Azure crea una cuenta de almacenamiento en el grupo de recursos "microsoft - network-[ubicación]", donde ubicación es la ubicación de la red WAN. Una vez que haya aplicado la configuración a los dispositivos VPN, puede eliminar esta cuenta de almacenamiento.
3. Una vez el archivo se haya terminado de crear, puede hacer clic en el vínculo para descargarlo.
4. Aplique la configuración al dispositivo VPN.

### <a name="understanding-the-vpn-device-configuration-file"></a>Información sobre el archivo de configuración del dispositivo VPN

El archivo de configuración de dispositivo contiene la configuración que se debe usar al configurar el dispositivo VPN local. Cuando visualice este archivo, tenga en cuenta la siguiente información:

* **vpnSiteConfiguration**: en esta sección se indica la configuración de los detalles del dispositivo como un sitio de conexión a la red virtual WAN. Incluye el nombre y la dirección IP pública del dispositivo de rama.
* **vpnSiteConnections**: en esta sección se proporciona información sobre la siguiente configuración:

    * **Espacio de direcciones** de la red virtual de concentradores virtuales<br>Ejemplo:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Espacio de direcciones** de las redes virtuales que están conectadas al concentrador<br>Ejemplo:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **Direcciones IP** de vpngateway del concentrador virtual. Dado que cada conexión del elemento vpngateway consta de dos túneles con una configuración activo-activo, verá ambas direcciones IP en este archivo. En este ejemplo puede ver "Instance0" e "Instance1" para cada sitio.<br>Ejemplo:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Detalles de configuración de conexión de Vpngateway** como BGP, clave precompartida, etc. La PSK es la clave precompartida que se genera automáticamente para usted. Puede modificar la conexión cuando quiera en la página de información general para una PSK personalizada.
  
### <a name="example-device-configuration-file"></a>Archivo de configuración de dispositivo de ejemplo

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>Configuración del dispositivo VPN

>[!NOTE]
> Si trabaja con una solución de asociado de WAN virtual, se produce automáticamente la configuración de los dispositivos de la VPN. El controlador de dispositivos obtiene el archivo de configuración de Azure y lo aplica al dispositivo para configurar la conexión con Azure. Esto significa que no es necesario saber cómo configurar manualmente el dispositivo VPN.
>

Si necesita instrucciones para configurar el dispositivo, puede utilizar las que se proporcionan en la [página de scripts de configuración de dispositivo VPN](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) con las siguientes advertencias:

* Las instrucciones que aparecen en la página de dispositivos VPN no están escritas para Virtual WAN, pero puede usar los valores de Virtual WAN desde el archivo de configuración para configurar manualmente el dispositivo VPN. 
* Los scripts descargables de configuración de dispositivo que son para VPN Gateway no funcionan para Virtual WAN, ya que la configuración es diferente.
* Las redes WAN virtuales nuevas admiten IKEv1 e IKEv2.
* Virtual WAN solo puede usar dispositivos VPN e instrucciones de dispositivo basados en rutas.

## <a name="viewwan"></a>8. Visualizar la instancia de Virtual WAN

1. Vaya a la instancia de Virtual WAN.
2. En la página Información general, cada punto del mapa representa un concentrador. Mantenga el mouse sobre cualquier punto para ver el resumen de estado del concentrador.
3. En la sección de concentradores y conexiones, puede ver estado del concentrador, sitio, región, estado de la conexión VPN y bytes de entrada y salida.

## <a name="viewhealth"></a>9. Visualización del estado de los recursos

1. Vaya a su red WAN.
2. En la página de la WAN, en la sección de **Soporte técnico y solución de problemas**, haga clic en **Mantenimiento** y visualice los recursos.

## <a name="connectmon"></a>10. Supervisar una conexión

Cree una conexión para supervisar la comunicación entre una máquina virtual de Azure y un sitio remoto. Para información acerca de cómo configurar una supervisión de conexión, consulte [Supervisar la comunicación de red](~/articles/network-watcher/connection-monitor.md). El campo de origen es la IP de la máquina virtual en Azure y la dirección IP de destino es la dirección IP del sitio.

## <a name="cleanup"></a>11. Limpieza de recursos

Cuando ya no necesite estos recursos, puede usar [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene. Reemplace "myResourceGroup" con el nombre del grupo de recursos y ejecute el siguiente comando de PowerShell:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear una red de área extensa (WAN)
> * Crear un sitio
> * Crear un concentrador
> * Conectar un concentrador con un sitio
> * Conectar una red virtual a un concentrador
> * Descargar y aplicar la configuración de dispositivo VPN
> * Visualizar la instancia de Virtual WAN
> * Visualizar el estado de los recursos
> * Supervisar una conexión

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).
