---
title: Puertas de enlace VPN en la solución VMware CloudSimple - Azure
description: Obtenga información acerca de VPN de sitio a sitio CloudSimple y conceptos de VPN de punto a sitio
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9b2630614e549181f4dd2f4e79871c4594d09201
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496861"
---
# <a name="vpn-gateways-overview"></a>Información general sobre las puertas de enlace VPN

Una puerta de enlace VPN se usa para enviar tráfico cifrado entre una red de área CloudSimple en una ubicación local o en un equipo a través de Internet pública.  Cada región puede tener solo una puerta de enlace VPN. Sin embargo, puede crear varias conexiones a la misma instancia. Al crear varias conexiones a la misma instancia de VPN Gateway, todos los túneles VPN comparten el ancho de banda disponible de la puerta de enlace.

CloudSimple proporciona dos tipos de puertas de enlace de VPN:

* Puerta de enlace VPN de sitio a sitio
* Puerta de enlace de VPN de punto a sitio

## <a name="site-to-site-vpn-gateway"></a>Puerta de enlace de VPN de sitio a sitio

Una puerta de enlace VPN de sitio a sitio se usa para enviar tráfico cifrado entre una red de área CloudSimple y un centro de datos local. Utilice esta conexión para definir el intervalo de subredes y CIDR para el tráfico de red entre su red local y la red de área CloudSimple.

La puerta de enlace VPN permite consumir los servicios locales en la nube privada y servicios en la nube privada, desde la red local.  CloudSimple proporciona un servidor VPN basada en directivas para establecer conexión desde su red local.

Casos de uso de VPN de sitio a sitio se incluyen:

* Accesibilidad de su vCenter de nube privada desde cualquier estación de trabajo en la red local.
* Uso de Active Directory local como un origen de la identidad de vCenter.
* Transferencia cómoda de plantillas, imágenes ISO y otros archivos de máquina virtual de los recursos locales al nube privada de vCenter.
* Accesibilidad de las cargas de trabajo que se ejecutan en la nube privada de la red local.

![Topología de conexión de VPN de sitio a sitio](media/cloudsimple-site-to-site-vpn-connection.png)

### <a name="cryptographic-parameters"></a>Parámetros criptográficos

Una conexión VPN de sitio a sitio usa los siguientes parámetros cifrado predeterminada para establecer una conexión segura.  Cuando se crea una conexión de dispositivo VPN local, los parámetros deben coincidir.

Una conexión VPN de sitio a sitio usa los siguientes parámetros cifrado predeterminada para establecer una conexión segura.  Cuando se crea una conexión de dispositivo VPN local, use cualquiera de los siguientes parámetros admitidos por la puerta de enlace VPN local.

#### <a name="phase-1-proposals"></a>Fase 1 propuestas

| Parámetro | Propuesta 1 | Propuesta 2 | Propuesta de 3 |
|-----------|------------|------------|------------|
| Versión de IKE | IKEv1 | IKEv1 | IKEv1 |
| Cifrado | AES 128 | AES 256 | AES 256 |
| Algoritmo hash| SHA 256 | SHA 256 | SHA 1 |
| Grupo de Diffie Hellman (DH grupo) | 1 | 1 | 1 |
| Tiempo de vida | 28.800 segundos | 28.800 segundos | 28.800 segundos |
| Tamaño de los datos | 4 GB | 4 GB | 4 GB |


#### <a name="phase-2-proposals"></a>Fase 2 propuestas 

| Parámetro | Propuesta 1 | Propuesta 2 | Propuesta de 3 |
|-----------|------------|------------|------------|
| Cifrado | AES 128 | AES 256 | AES 256 |
| Algoritmo hash| SHA 256 | SHA 256 | SHA 1 |
| Grupo de confidencialidad directa perfecta (grupo PFS) | None | None | None |
| Tiempo de vida | 1800 segundos | 1800 segundos | 1800 segundos |
| Tamaño de los datos | 4 GB | 4 GB | 4 GB |

## <a name="point-to-site-vpn-gateway"></a>Puerta de enlace de VPN de punto a sitio

Una VPN de punto a sitio se usa para enviar tráfico cifrado entre una red de área CloudSimple y un equipo cliente.  VPN de punto a sitio es la manera más fácil de obtener acceso a la red privada en la nube, incluida su nube privada vCenter y carga de trabajo de máquinas virtuales.  Use la conectividad VPN punto a sitio si se conecta a la nube privada remota.

## <a name="next-steps"></a>Pasos siguientes

* [Configurar la puerta de enlace de VPN](https://docs.azure.cloudsimple.com/vpn-gateway/)