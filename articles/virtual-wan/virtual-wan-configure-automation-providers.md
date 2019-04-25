---
title: Asociados de Azure Virtual WAN | Microsoft Docs
description: En este artículo se ayuda a los asociados a configurar la automatización de Azure Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: cherylmc
Customer intent: As a Virtual WAN software-defined connectivity provider, I want to set up a provisioning environment.
ms.openlocfilehash: a4664e628af5824b7b197cbdb5c5af602a3a4476
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60459944"
---
# <a name="virtual-wan-partners"></a>Asociados de Virtual WAN

Este artículo le ayudará a comprender cómo configurar el entorno de automatización para conectar y configurar un dispositivo de la rama (un dispositivo VPN de cliente en el entorno local o SDWAN CPE) para Azure Virtual WAN. Si es un proveedor que proporciona dispositivos de la rama que pueden dar cabida a la conectividad VPN a través de IPsec/IKEv2 o IPsec/IKEv1, este artículo es para usted.

Un dispositivo de la rama (un dispositivo VPN de cliente en el entorno local o SDWAN CPE) normalmente usa un panel de dispositivo o controlador para aprovisionarse. Los administradores de la solución SD-WAN a menudo pueden utilizar una consola de administración para aprovisionar previamente un dispositivo antes de conectarse a la red. Este dispositivo compatible con VPN obtiene su lógica de plan de control de un controlador. El controlador de dispositivo VPN o SD-WAN puede usar las API de Azure para automatizar la conectividad a Azure Virtual WAN. Este tipo de conexión requiere que el dispositivo local tenga una dirección IP pública asignada.

## <a name ="before"></a>Antes de iniciar la automatización

* Compruebe que el dispositivo es compatible con IPsec IKEv1 o IKEv2. Consulte las [directivas predeterminadas](#default).
* Consulte las [API REST](https://docs.microsoft.com/rest/api/azure/) que usará para automatizar la conectividad a Azure Virtual WAN.
* Pruebe la experiencia del portal de Azure Virtual WAN.
* A continuación, decida qué parte de los pasos de conectividad le gustaría automatizar. Como mínimo, se recomienda automatizar lo siguiente:

  * Control de acceso
  * Carga del dispositivo de la rama en Azure Virtual WAN
  * Descarga de la configuración de Azure y configuración de la conectividad del dispositivo de la rama en Azure Virtual WAN

* Comprenda la experiencia esperada del cliente conjuntamente con Azure Virtual WAN.

  1. Normalmente, un usuario de Virtual WAN iniciará el proceso mediante la creación de un recurso de Virtual WAN.
  2. El usuario configurará un acceso al grupo de recursos basado en la entidad de servicio para el sistema local (el controlador de rama o el dispositivo VPN que aprovisiona el software) para escribir información de la rama en Azure Virtual WAN.
  3. El usuario puede decidir en este momento iniciar sesión en la interfaz de usuario y configurar las credenciales de la entidad de servicio. Una vez completado, el controlador debe ser capaz de cargar la información de la rama con la automatización que proporcionará. El manual equivalente a este en Azure es "Crear sitio".
  4. Una vez que la información del sitio (dispositivo de la rama) esté disponible en Azure, el usuario asociará el sitio a un concentrador. Un concentrador virtual es una red virtual administrada por Microsoft. El concentrador contiene varios puntos de conexión de servicio para habilitar la conectividad de la red local (vpnsite). El concentrador es el núcleo de la red en una región. Solo puede haber un único concentrador por región de Azure y su punto de conexión de VPN (vpngateway) se crea durante este proceso. La puerta de enlace VPN es una puerta de enlace escalable cuyo tamaño se basa adecuadamente en las necesidades de ancho de banda y conexión. Puede automatizar la creación del concentrador virtual y vpngateway desde el panel de controlador del dispositivo de la rama.
  5. Una vez que el concentrador virtual se asocia al sitio, se genera un archivo de configuración para que el usuario lo descargue manualmente. En este momento la automatización entra en juego haciendo que el usuario obtenga una experiencia sin interrupciones. En lugar de que el usuario tenga que descargar y configurar el dispositivo de la rama manualmente, puede establecer la automatización y proporcionar una experiencia de click-through mínima en la interfaz de usuario. De este modo, se mitigan los típicos problemas de conectividad, como el error de coincidencia de la clave compartida o de los parámetros de IPSec, la legibilidad del archivo de configuración, etc.
  6. Al final de este paso de la solución, el usuario tendrá una conexión directa de sitio a sitio entre el dispositivo de la rama y el concentrador virtual. También puede configurar conexiones adicionales en otros concentradores. Cada conexión es un túnel activo-activo. El cliente puede utilizar otro ISP para cada uno de los vínculos del túnel.

## <a name ="understand"></a>Comprender los detalles de la automatización


###  <a name="access"></a>Control de acceso

Los clientes deben poder configurar el control de acceso adecuado para la red Virtual WAN en la interfaz de usuario del dispositivo. Se recomienda usar una entidad de servicio de Azure. El acceso basado en la entidad de servicio proporciona la autenticación adecuada de controlador de dispositivo para cargar la información de rama. Para obtener más información, consulte [Create service principal](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) (Creación de una entidad de servicio). Aunque esta funcionalidad no se incluye con la oferta de Azure Virtual WAN, a continuación se indican los pasos típicos para configurar el acceso en Azure, tras los cuales se especifican los detalles pertinentes en el panel de administración de dispositivos.

* Cree una aplicación de Azure Active Directory para el controlador de dispositivos local.
* Obtención del id. y la clave de autenticación de la aplicación
* Obtención del identificador de inquilino
* Asignación de la aplicación al rol "Colaborador"

###  <a name="branch"></a>Cargar la información del dispositivo de la rama

Diseñe la experiencia de usuario para cargar información de rama (sitio local) en Azure. Las [API REST](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) para VPNSite pueden usarse para crear la información del sitio en Virtual WAN. Puede proporcionar todos los dispositivos VPN o SDWAN de la rama o seleccionar las personalizaciones de dispositivo según corresponda.


### <a name="device"></a>Conectividad y descarga de la configuración del dispositivo

Este paso implica la descarga de la configuración de Azure y la configuración de la conectividad del dispositivo de la rama en Azure Virtual WAN. En este paso, un cliente que no está usando un proveedor podría descargar la configuración de Azure manualmente y aplicarla a su dispositivo SDWAN o VPN local. Como proveedor, debería automatizar este paso. El controlador del dispositivo puede llamar a la API REST "GetVpnConfiguration" para descargar la configuración de Azure, que normalmente tendrá un aspecto similar al siguiente archivo.

**Notas de la configuración**

  * Si las redes virtuales de Azure están conectadas al concentrador virtual, aparecen como ConnectedSubnets.
  * La conectividad de VPN usa la configuración basada en rutas e IKEv2 o IKEv1.

#### <a name="understanding-the-device-configuration-file"></a>Información sobre el archivo de configuración del dispositivo

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
  
#### <a name="example-device-configuration-file"></a>Archivo de configuración de dispositivo de ejemplo

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

## <a name="default"></a>Directivas predeterminadas para la conectividad de IPsec

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

La configuración de su dispositivo VPN o SDWAN local debe coincidir o contener los siguientes algoritmos y parámetros que se especifican en la directiva de IPsec o IKE de Azure.

* Algoritmo de cifrado IKE
* Algoritmo de integridad de IKE
* Grupo DH
* Algoritmo de cifrado IPsec
* Algoritmo de integridad de IPsec
* Grupo PFS

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Virtual WAN, vea [Acerca de Azure Virtual WAN](virtual-wan-about.md) y [Preguntas más frecuentes sobre Azure Virtual WAN](virtual-wan-faq.md).

Para obtener información adicional, envíe un correo electrónico a <azurevirtualwan@microsoft.com>. Incluya el nombre de su compañía en la línea de asunto "[ ]".
