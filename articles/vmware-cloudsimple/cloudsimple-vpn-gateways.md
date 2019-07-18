---
title: Puertas de enlace de VPN en solución de VMware en Azure de CloudSimple
description: Obtenga información acerca de los conceptos de VPN de sitio a sitio y de VPN de punto a sitio de CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2eae81f357904bd5034d7409ef42b681d1085930
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67695219"
---
# <a name="vpn-gateways-overview"></a>Información general sobre puertas de enlace de VPN

Una puerta de enlace de VPN se usa para enviar tráfico cifrado entre la red de una región de CloudSimple en una ubicación local o en un equipo a través de Internet pública.  Cada región solo puede tener una puerta de enlace de VPN. Sin embargo, puede crear varias conexiones a la misma instancia. Al crear varias conexiones a la misma instancia de VPN Gateway, todos los túneles VPN comparten el ancho de banda disponible de la puerta de enlace.

CloudSimple proporciona dos tipos de puerta de enlace de VPN:

* Puerta de enlace de VPN de sitio a sitio
* Puerta de enlace de VPN de punto a sitio

## <a name="site-to-site-vpn-gateway"></a>Puerta de enlace de VPN de sitio a sitio

Una puerta de enlace de VPN de sitio a sitio se usa para enviar tráfico cifrado entre la red de una región de CloudSimple y un centro de datos local. Use esta conexión para definir las subredes o el intervalo de CIDR para la comunicación entre su red local y la red de la región de CloudSimple.

La puerta de enlace VPN permite consumir los servicios desde el entorno local en la nube privada, y los servicios en la nube privada, desde la red local.  CloudSimple proporciona un servidor VPN basado en directivas para establecer la conexión desde su red local.

Entre los casos de uso de VPN de sitio a sitio se incluyen:

* Accesibilidad de su vCenter de nube privada desde cualquier estación de trabajo en la red local.
* Uso de Active Directory local como origen de identidad de vCenter.
* Transferencia cómoda de plantillas de VM, imágenes ISO y otros archivos de los recursos locales a vCenter de la nube privada.
* Accesibilidad de las cargas de trabajo que se ejecutan en la nube privada desde la red local.

![Topología de conexiones VPN de sitio a sitio](media/cloudsimple-site-to-site-vpn-connection.png)

> [!IMPORTANT]
> Tiene que fijar el MSS de TCP en 1078 bytes o menos. O, si los dispositivos de VPN no admiten la fijación de MSS, también puede establecer el MTU en la interfaz de túnel en 1118 bytes. 

### <a name="cryptographic-parameters"></a>Parámetros criptográficos

Una conexión VPN de sitio a sitio usa los siguientes parámetros criptográficos predeterminados para establecer una conexión segura.  Cuando crea una conexión desde un dispositivo VPN local, use cualquiera de los siguientes parámetros admitidos por la puerta de enlace de VPN local.

#### <a name="phase-1-proposals"></a>Propuestas de la fase 1

| Parámetro | Propuesta 1 | Propuesta 2 | Propuesta 3 |
|-----------|------------|------------|------------|
| Versión de IKE | IKEv1 | IKEv1 | IKEv1 |
| Cifrado | AES 128 | AES 256 | AES 256 |
| Algoritmo hash| SHA 256 | SHA 256 | SHA 1 |
| Grupo Diffie Hellman (grupo DH) | 2 | 2 | 2 |
| Tiempo de vida | 28.800 segundos | 28.800 segundos | 28.800 segundos |
| Tamaño de datos | 4 GB | 4 GB | 4 GB |


#### <a name="phase-2-proposals"></a>Propuestas de la fase 2 

| Parámetro | Propuesta 1 | Propuesta 2 | Propuesta 3 |
|-----------|------------|------------|------------|
| Cifrado | AES 128 | AES 256 | AES 256 |
| Algoritmo hash| SHA 256 | SHA 256 | SHA 1 |
| Grupo de confidencialidad directa total (grupo PFS) | None | None | None |
| Tiempo de vida | 1800 segundos | 1800 segundos | 1800 segundos |
| Tamaño de datos | 4 GB | 4 GB | 4 GB |

## <a name="point-to-site-vpn-gateway"></a>Puerta de enlace de VPN de punto a sitio

Una VPN de punto a sitio se usa para enviar tráfico cifrado entre la red de una región de CloudSimple y un equipo cliente.  La VPN de punto a sitio es la manera más fácil de acceder a la red de la nube privada, incluido vCenter y las VM de carga de trabajo de su nube privada.  Use la conectividad de VPN de punto a sitio si se conecta a la nube privada de forma remota.

## <a name="next-steps"></a>Pasos siguientes

* [Configurar puerta de enlace de VPN](https://docs.azure.cloudsimple.com/vpn-gateway/)