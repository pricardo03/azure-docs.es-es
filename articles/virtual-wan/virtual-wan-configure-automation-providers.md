---
title: Configurar automatización de Azure Virtual WAN para asociados de la red Virtual WAN | Microsoft Docs
description: Este artículo ayuda a los socios de soluciones de conectividad definidas por software a configurar la automatización de Azure Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: cherylmc
Customer intent: As a Virtual WAN software-defined connectivity provider, I want to set up a provisioning environment.
ms.openlocfilehash: a1ff4364e394b3807cf767722ee934ae024399b0
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114351"
---
# <a name="configure-virtual-wan-automation---for-virtual-wan-partners-preview"></a>Configurar automatización de Virtual WAN para asociados de la red Virtual WAN (versión preliminar)

Este artículo le ayudará a comprender cómo configurar el entorno de automatización para conectar y configurar un dispositivo de la rama (un dispositivo VPN de cliente en el entorno local o SDWAN) para Azure Virtual WAN. Si es un proveedor que proporciona dispositivos de la rama que pueden dar cabida a la conectividad VPN a través de IPsec/IKEv2, este artículo es para usted.

Las soluciones de conectividad definidas por software suelen usar un controlador o un centro de aprovisionamiento de dispositivos para administrar sus dispositivos de rama. El controlador puede usar las API de Azure para automatizar la conectividad a Azure Virtual WAN. Este tipo de conexión requiere un dispositivo SDWAN o VPN local que tenga una dirección IP pública asignada.

##  <a name="access"></a>Control de acceso

Los clientes deben poder configurar el control de acceso adecuado para la red Virtual WAN en la interfaz de usuario del dispositivo. Se recomienda usar una entidad de servicio de Azure. El acceso basado en la entidad de servicio proporciona la autenticación adecuada de controlador de dispositivo para cargar la información de rama.

##  <a name="site"></a>Cargar la información de rama

Diseñe la experiencia de usuario para cargar información de rama (sitio local) en Azure. [Las API REST](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) para **VPNSite** pueden usarse para crear la información del sitio en Virtual WAN. Puede proporcionar todos los dispositivos VPN o SDWAN de rama, o seleccionar las personalizaciones de dispositivo según corresponda.

##  <a name="hub"></a>Concentrador y servicios

Una vez que el dispositivo de rama se cargue en Azure, el cliente normalmente permitirá realizar selecciones de la región del concentrador o los servicios en Azure Portal, que llama a un conjunto de operaciones para crear la red virtual de concentrador y el extremo VPN dentro del concentrador. La puerta de enlace VPN es una puerta de enlace escalable cuyo tamaño se basa adecuadamente en las necesidades de ancho de banda y conexión.

## <a name="device"></a>Configuración del dispositivo

En este paso, un cliente que no está usando un proveedor podría descargar la configuración de Azure manualmente y aplicarla a su dispositivo SDWAN o VPN local. Como proveedor, debería automatizar este paso. El controlador puede llamar a la API REST **GetVpnConfiguration** para descargar la configuración de Azure, que normalmente tendrá un aspecto similar al siguiente archivo.

**Notas de la configuración**

  * Si las redes virtuales de Azure están conectadas al concentrador virtual, aparecen como ConnectedSubnets.
  * La conectividad de VPN usa la configuración basada en rutas e IKEv2.

### <a name="understanding-the-device-configuration-file"></a>Información sobre el archivo de configuración del dispositivo

El archivo de configuración de dispositivo contiene la configuración que se debe usar al configurar el dispositivo VPN local. Cuando visualice este archivo, tenga en cuenta la siguiente información:

* **vpnSiteConfiguration**: en esta sección se indica la configuración de los detalles del dispositivo como un sitio de conexión a la red virtual WAN. Incluye el nombre y la dirección IP pública del dispositivo de rama.
* **vpnSiteConnections**: en esta sección se proporciona información sobre lo siguiente:

    * **Espacio de direcciones** de la red virtual de concentradores virtuales.<br>Ejemplo:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Espacio de direcciones** de las redes virtuales que están conectadas al concentrador.<br>Ejemplo:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **Direcciones IP** de vpngateway del concentrador virtual. Dado que en el elemento vpngateway cada conexión consta de 2 túneles en configuración activa-activa, verá ambas direcciones IP enumeradas en este archivo. En este ejemplo puede ver "Instance0" e "Instance1" para cada sitio.<br>Ejemplo:

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

## <a name="default"></a>Directivas predeterminadas

### <a name="initiator"></a>Iniciador

Las secciones siguientes enumeran las combinaciones de directivas compatibles cuando Azure es el iniciador para el túnel.

**Fase 1**

* AES_256, SHA1, DH_GROUP_2
* AES_256, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* 3DES, SHA1, DH_GROUP_2
* 3DES, SHA_256, DH_GROUP_2

**Fase 2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256, SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE


### <a name="responder"></a>Servicio de respuesta

Las secciones siguientes enumeran las combinaciones de directivas compatibles cuando Azure es el respondedor para el túnel.

**Fase 1**

* AES_256, SHA1, DH_GROUP_2
* AES_256, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* 3DES, SHA1, DH_GROUP_2
* 3DES, SHA_256, DH_GROUP_2

**Fase 2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256, SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE
* CBC_DES, SHA_1, PFS_NONE 
* AES_256, SHA_1, PFS_1
* AES_256, SHA_1, PFS_2
* AES_256, SHA_1, PFS_14
* AES_128, SHA_1, PFS_1
* AES_128, SHA_1, PFS_2
* AES_128, SHA_1, PFS_14
* CBC_3DES, SHA_1, PFS_1
* CBC_3DES, SHA_1, PFS_2
* CBC_3DES, SHA_256, PFS_2
* AES_256, SHA_256, PFS_1
* AES_256, SHA_256, PFS_2
* AES_256, SHA_256, PFS_14
* AES_256, SHA_1, PFS_24
* AES_256, SHA_256, PFS_24
* AES_128, SHA_256, PFS_NONE
* AES_128, SHA_256, PFS_1
* AES_128, SHA_256, PFS_2
* AES_128, SHA_256, PFS_14
* CBC_3DES, SHA_1, PFS_14

### <a name="does-everything-need-to-match-between-the-virtual-hub-vpngateway-policy-and-my-on-premises-sdwanvpn-device-or-sd-wan-configuration"></a>¿Es preciso que coincidan todos los elementos de la directiva vpngateway del concentrador virtual y la configuración de mis dispositivos VPN o SDWAN locales?

La configuración de su dispositivo VPN o SDWAN local debe coincidir o contener los siguientes algoritmos y parámetros que se especifican en la directiva de IPsec o IKE de Azure. Las vigencias de SA solo son especificaciones locales y no es preciso que coincidan.

* Algoritmo de cifrado IKE
* Algoritmo de integridad de IKE
* Grupo DH
* Algoritmo de cifrado IPsec
* Algoritmo de integridad de IPsec
* Grupo PFS

## <a name="feedback"></a>Comentarios sobre la versión preliminar

Agradeceríamos su participación. Envíe un correo electrónico a <azurevirtualwan@microsoft.com> para informar sobre cualquier problema o para proporcionar comentarios (positivos o negativos) sobre Virtual WAN. Incluya el nombre de su compañía en la línea de asunto "[ ]". Incluya también su identificador de suscripción si está notificando un problema.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Virtual WAN, vea [Acerca de Azure Virtual WAN](virtual-wan-about.md) y [Preguntas más frecuentes sobre Azure Virtual WAN](virtual-wan-faq.md).
