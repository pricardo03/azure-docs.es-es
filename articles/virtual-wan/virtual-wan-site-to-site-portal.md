---
title: Uso de Azure Virtual WAN para crear una conexión de sitio a sitio a Azure | Microsoft Docs
description: En este tutorial, aprenda a usar Azure Virtual WAN para crear una conexión VPN de sitio a sitio a Azure.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 07/13/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: ea36a3d4a2471cee6a18d70275aaf2e83ffc6f39
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159658"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan-preview"></a>Tutorial: Creación de una conexión de sitio a sitio mediante Azure Virtual WAN (Versión preliminar)

Este tutorial muestra cómo usar Virtual WAN para conectarse a los recursos de Azure a través de una conexión VPN de IPsec/IKE (IKEv2). Este tipo de conexión requiere un dispositivo VPN local que tenga una dirección IP pública asignada. Para más información sobre Virtual WAN, consulte la [Introducción a Virtual WAN](virtual-wan-about.md)

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
> * Conectar una red virtual a un concentrador
> * Descargar y aplicar la configuración de dispositivo VPN
> * Visualizar la instancia de Virtual WAN
> * Visualizar el estado de los recursos
> * Supervisar una conexión

> [!IMPORTANT]
> Azure Virtual WAN se encuentra actualmente en versión preliminar pública administrada. Para usar Virtual WAN, es preciso [inscribirse en la versión preliminar](#enroll).
>
> Esta versión preliminar pública se proporciona sin un contrato de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="before-you-begin"></a>Antes de empezar

Antes de comenzar con la configuración, compruebe que se cumplen los criterios siguientes:

* Asegúrese de tener un dispositivo VPN compatible basado en rutas que pueda utilizar IKEv2, y alguien que pueda configurarlo. Si está trabajando con un asociado de Virtual WAN, los valores de configuración se crean automáticamente y no deberá preocuparse de saber cómo configurar manualmente el dispositivo.
* Compruebe que tiene una dirección IPv4 pública externa para el dispositivo VPN. Esta dirección IP no puede estar detrás de un NAT.
* Si ya tiene una red virtual a la que desea conectarse, verifique que ninguna de las subredes de la red local se superpone a las redes virtuales con las que desee conectarse. La red virtual no requiere una subred de puerta de enlace y no puede tener ninguna puerta de enlace de red virtual. Si no tiene una red virtual, puede crear una siguiendo los pasos de este artículo.
* Obtenga un intervalo de direcciones IP para la región del concentrador. El concentrador es una red virtual y el intervalo de direcciones que especifique para la región del concentrador no se puede superponer a ninguna de las redes virtuales existentes a las que ya esté conectado. Igualmente no se puede superponer a los intervalos de direcciones con las que esté conectadas en el entorno local. Si no está familiarizado con los intervalos de direcciones IP ubicados en la red local, necesita trabajar con alguien que pueda proporcionarle estos detalles.
* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="enroll"></a>1. Inscripción en la versión preliminar

Para poder configurar una instancia de Virtual WAN, primero hay que inscribir una suscripción en la versión preliminar. En caso contrario, no podrá trabajar con Virtual WAN en el portal. Para inscribirse, envíe un correo electrónico a **azurevirtualwan@microsoft.com** con su identificador de suscripción. Recibirá un correo electrónico una vez inscrita la suscripción.

## <a name="vnet"></a>2. Creación de una red virtual

Si no dispone de una red virtual, puede crear rápidamente una mediante PowerShell. También puede crear una red virtual mediante Azure Portal.

* Asegúrese de que el espacio de direcciones para la red virtual que cree no se superpone a ninguno de los intervalos de direcciones de otras redes virtuales a las que desee conectarse, o a los espacios de direcciones de red local. 
* Si ya tiene una red virtual, verifique que cumple los criterios necesarios y no tiene una puerta de enlace de red virtual.

Puede crear fácilmente su red virtual haciendo clic en "Pruébelo" en este artículo para abrir una consola de PowerShell. Ajuste los valores, y luego copie y pegue los comandos en la ventana de consola.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Ajuste los comandos de PowerShell y luego cree un grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Creación de una red virtual

Ajuste los comandos de PowerShell para crear la red virtual que sea compatible con su entorno.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzureRmVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```

## <a name="wan"></a>3. Creación de una instancia de Virtual WAN

1. Desde un explorador, navegue al [Portal de Azure](https://portal.azure.com) e inicie sesión con su cuenta de Azure.
2. En este momento encontrará Virtual WAN acudiendo a **Todos los servicios** y buscando Virtual WAN. También puede buscar Virtual WAN en el cuadro de búsqueda de la parte superior de Azure Portal. Haga clic en **Virtual WAN** para abrir la página.
3. Haga clic en **Crear** para abrir la página **Crear una red WAN**. Si la página no está disponible, es que aún no han sido aprobada para esta versión preliminar.

  ![Creación de una red WAN](./media/virtual-wan-site-to-site-portal/createwan.png)
4. En la página Crear una red WAN rellene los campos siguientes.

  * **Nombre**: seleccione el nombre que desea dar a la WAN.
  * **Suscripción**: seleccione la suscripción que quiere usar.
  * **Grupo de recursos**: cree uno nuevo o utilice uno ya existente.
  * **Ubicación recursos**: elija una ubicación de recursos en la lista desplegable. Una red WAN es un recurso global y no reside en una región determinada. De todas formas, tiene que seleccionar una región con el fin de administrar y ubicar más fácilmente el recurso WAN que cree.
5. Haga clic en **Crear** para crear la WAN.

## <a name="site"></a>4. Crear un sitio

Cree tantos sitios como necesite que se correspondan con sus ubicaciones físicas. Por ejemplo, si tiene una sucursal en Nueva York, otra en Londres y otra en Los Ángeles, crearía tres sitios independientes. Estos sitios contienen los puntos de conexión de dispositivo VPN local. Ahora puede especificar solo un espacio de direcciones privadas para su sitio.

1. Vaya a **Todos los recursos**.
2. Haga clic en la red WAN virtual que ha creado.
3. Haga clic en **+ Crear sitio** en la parte superior de la página para abrir la página **Crear sitio**.

  ![nuevo sitio](media/virtual-wan-site-to-site-portal/createsite.png)
4. En la página **Crear sitio** rellene los campos siguientes:

  *  **Nombre**: éste es el nombre que desee usar para hacer referencia a su sitio local.
  *  **Dirección IP pública**: es la dirección IP pública del dispositivo VPN que reside en su sitio local.
  *  **Espacio de direcciones privadas**: es el espacio de direcciones IP que se encuentra en el sitio local. El tráfico destinado a este espacio de direcciones se enruta al sitio local.
  *  **Suscripción**: compruebe la suscripción.
  *  **Grupo de recursos:**: el grupo de recursos que desea utilizar.
5. Haga clic en **Mostrar opciones avanzadas** para ver opciones adicionales. También puede **habilitar BGP** (campo opcional, que habilitará esta funcionalidad en todas las conexiones creadas para este sitio en Azure). También puede incluir **Información del dispositivo** (campo opcional). Esto puede ayudar al equipo de Azure a comprender mejor su entorno para agregar las posibilidades de optimización adicionales en el futuro, así como para ayudarle a solucionar problemas.

  ![BGP](media/virtual-wan-site-to-site-portal/sitebgp.png)
6. Haga clic en **Confirmar** para crear el sitio.
7. Repita estos pasos para cada sitio que desee crear.

## <a name="hub"></a>5. Creación de un concentrador y conexión a sitios

1. En la página de la red virtual WAN, haga clic en **Sitios**.
2. En **Sitios no asociados** verá una lista de sitios que aún no se han conectado a un concentrador.
3. Seleccione los sitios que desea asociar.
4. En la lista desplegable, seleccione la región a la que se asociará el concentrador. Debe asociar el concentrador a la región en la que residen las redes virtuales a las que desea conectarse.
5. Haga clic en **Confirmar**. Si aún no dispone de un concentrador en esta región, se creará automáticamente una red virtual de concentrador virtual. En este caso, aparece la página **Crear centros regionales**.
6. En la página **Crear centros regionales**, escriba el intervalo de direcciones para la red virtual del concentrador. Se trata de la red virtual que contendrá los servicios de concentrador. El intervalo que especifique aquí tiene que ser un intervalo de direcciones IP privadas y no superponerse con ninguno de los espacios de direcciones locales o de direcciones de red virtual. Se creará un punto de conexión VPN subsiguiente en la red virtual del concentrador. (La creación automática de concentrador y puerta de enlace solo está disponible en el portal).
7. Haga clic en **Create**(Crear).

## <a name="vnet"></a>6. Conexión de la red virtual a un concentrador

En este paso, creará la conexión de emparejamiento entre una red virtual y el concentrador. Repita estos pasos para cada red virtual que desee conectar.

1. En la página de la red virtual WAN, haga clic en **Conexión de red virtual**.
2. En la página de conexión de red virtual, haga clic en **+ Agregar conexión**.
3. En la página **Agregar conexión**, rellene los campos siguientes:

    * **Nombre de la conexión**: asigne un nombre a la conexión.
    * **Centros**: seleccione el concentrador que desea asociar a esta conexión.
    * **Suscripción**: compruebe la suscripción.
    * **Red virtual**: seleccione la red virtual que quiere conectar con este concentrador. La red virtual no puede tener una puerta de enlace de red virtual ya existente.

## <a name="device"></a>7. Descarga de la configuración de VPN

Use la configuración del dispositivo VPN para configurar el dispositivo VPN local.

1. En la página de la red virtual WAN, haga clic en **Introducción**.
2. En la parte superior de la página de información general, haga clic en **Descargar configuración VPN**. Azure crea una cuenta de almacenamiento en el grupo de recursos "microsoft - network-[ubicación]", donde ubicación es la ubicación de la red WAN. Una vez que haya aplicado la configuración a los dispositivos VPN, puede eliminar esta cuenta de almacenamiento.
3. Una vez el archivo se haya terminado de crear, puede hacer clic en el vínculo para descargarlo.
4. Aplique la configuración al dispositivo VPN.

### <a name="understanding-the-vpn-device-configuration-file"></a>Información sobre el archivo de configuración del dispositivo VPN

El archivo de configuración de dispositivo contiene la configuración que se debe usar al configurar el dispositivo VPN local. Cuando visualice este archivo, tenga en cuenta la siguiente información:

* **vpnSiteConfiguration**: en esta sección se indica la configuración de los detalles del dispositivo como un sitio de conexión a la red virtual WAN. Incluye el nombre y la dirección IP pública del dispositivo de rama.
* **vpnSiteConnections**: en esta sección se proporciona información acerca de lo siguiente:

    * **Espacio de direcciones** de la red virtual de concentradores virtuales<br>Ejemplo:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Espacio de direcciones** de las redes virtuales que están conectadas al concentrador<br>Ejemplo:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **Direcciones IP** de vpngateway del concentrador virtual. Dado que en el elemento vpngateway cada conexión consta de 2 túneles en configuración activa-activa, verá ambas direcciones IP enumeradas en este archivo. En este ejemplo puede ver "Instance0" e "Instance1" para cada sitio.<br>Ejemplo:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Detalles de configuración de conexión de Vpngateway** como BGP, clave precompartida, etc. La PSK es la clave precompartida que se genera automáticamente para usted. Puede modificar la conexión cuando lo desee en la página de información general para una PSK personalizada.
  
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
> Si está trabajando con una solución de asociado de Virtual WAN, la configuración del dispositivo VPN se produce de forma automática, el controlador del dispositivo obtiene el archivo de configuración de Azure y lo aplica al dispositivo para establecer conexión con Azure. Esto significa que no es necesario saber cómo configurar manualmente el dispositivo VPN.
>

Si necesita instrucciones para configurar el dispositivo, puede utilizar las que se proporcionan en la [página de scripts de configuración de dispositivo VPN](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) con las siguientes advertencias:

* Las instrucciones que aparecen en la página de dispositivos VPN no están escritas para Virtual WAN, pero puede usar los valores de Virtual WAN desde el archivo de configuración para configurar manualmente el dispositivo VPN. 
* Los scripts descargables de configuración de dispositivo que son para VPN Gateway no funcionan para Virtual WAN, ya que la configuración es diferente.
* Virtual WAN solo puede usar IKEv2, no IKEv1.
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

Cuando ya no necesite estos recursos, puede usar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene. Reemplace "myResourceGroup" con el nombre del grupo de recursos y ejecute el siguiente comando de PowerShell:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="feedback"></a>Comentarios acerca de la versión preliminar

Agradeceríamos su participación. Envíe un correo electrónico a <azurevirtualwan@microsoft.com> para informar sobre cualquier problema o para proporcionar comentarios (positivos o negativos) acerca de Virtual WAN. Incluya el nombre de su compañía en la línea de asunto "[ ]". Incluya también su identificador de suscripción si está notificando un problema.

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
