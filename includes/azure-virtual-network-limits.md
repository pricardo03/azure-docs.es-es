---
title: archivo de inclusión
description: archivo de inclusión
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 08/16/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: 0394d1cc44dae270682691e2091543e5aab321f6
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513965"
---
<a name="virtual-networking-limits-classic"></a>Los límites siguientes se aplican solo a los recursos de redes administrados a través del modelo de implementación clásico por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Redes virtuales |50 |100 |
| Sitios de red local |20 |ponerse en contacto con el servicio de soporte técnico |
| Servidores DNS por red virtual |20 |20 |
| Direcciones IP privadas por red virtual |4096 |4096 |
| Conexiones TCP concurrentes o flujo UDP por NIC de una máquina virtual o instancia de rol |500 KB |500 KB |
| Grupos de seguridad de red (NSG) |100 |200 |
| Reglas de NSG por NSG |200 |1000 |
| Tablas de rutas definidas por el usuario |100 |200 |
| Rutas definidas por el usuario por tabla de ruta |100 |400 |
| Direcciones IP públicas (dinámicas) |5 |ponerse en contacto con el servicio de soporte técnico |
| Direcciones IP públicas reservadas |20 |ponerse en contacto con el servicio de soporte técnico |
| VIP pública por implementación |5 |ponerse en contacto con el servicio de soporte técnico |
| VIP privada (ILB) por implementación |1 |1 |
| Listas de control de acceso (ACL) de extremo |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Límites de redes - Azure Resource Manager
Los límites siguientes solo se aplican a los recursos de redes administrados a través de Azure Resource Manager por región y por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

> [!NOTE]
> Recientemente hemos aumentado todos los límites predeterminados a sus límites máximos. Si la columna **Límite máximo** no existe, el recurso no tiene límites ajustables. Si ha hecho que el soporte técnico incrementase estos límites en el pasado y no ve los límites actualizados como se muestra a continuación, [abra una solicitud de soporte técnico al cliente en línea sin cargo](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| Recurso | Límite predeterminado | 
| --- | --- |
| Redes virtuales |1000 |
| Subredes por red virtual |3000 |
| Emparejamientos de redes virtuales por red virtual |100 |
| Servidores DNS por red virtual |20 |
| Direcciones IP privadas por red virtual |65536 |
| Direcciones IP privadas por interfaz de red |256 |
| Direcciones IP privadas por máquina virtual |256 |
| Conexiones TCP concurrentes o flujo UDP por NIC de una máquina virtual o instancia de rol |500 KB |
| Interfaces de red (NIC) |65536 |
| Grupos de seguridad de red (NSG) |5000 |
| Reglas de NSG por NSG |1000 |
| Direcciones IP y rangos especificados para el origen o destino en un grupo de seguridad |4000 |
| Grupos de seguridad de aplicaciones |3000 |
| Grupos de seguridad de aplicaciones por configuración de IP, por NIC |20 |
| Configuraciones de IP por grupo de seguridad de aplicaciones |4000 |
| Grupos de seguridad de aplicaciones que se pueden especificar en todas las reglas de seguridad de un grupo de seguridad de red |100 |
| Tablas de rutas definidas por el usuario |200 |
| Rutas definidas por el usuario por tabla de ruta |400 |
| Certificados raíz de punto a sitio por VPN Gateway |20 |
| TAP de red virtual |100 |
| Configuraciones de TAP de la interfaz de red por TAP de red virtual |100 |

#### <a name="publicip-address"></a>Límites de dirección IP pública
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Direcciones IP públicas - dinámicas |(Básico) 1000 |ponerse en contacto con el servicio de soporte técnico |
| Direcciones IP públicas - estáticas |(Básico) 200 |ponerse en contacto con el servicio de soporte técnico |
| Direcciones IP públicas - estáticas |(Estándar) 200 |ponerse en contacto con el servicio de soporte técnico |
| Tamaño de prefijo de IP pública (versión preliminar) | /28 | /28 |

#### <a name="load-balancer"></a>Límites de Load Balancer
Los límites siguientes solo se aplican a los recursos de redes administrados a través de Azure Resource Manager por región y por suscripción. Aprenda a [ver el uso de los recursos actuales comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

| Recurso | Límite predeterminado |
| --- | --- | --- |
| Equilibradores de carga | 1000 | 
| Reglas por recurso, básico | 250 |
| Reglas por recurso, estándar | 1.500 | 
| Reglas por configuración de dirección IP | 299 |
| Configuraciones de direcciones IP de front-end, básico | 200 |
| Configuraciones de direcciones IP de front-end, estándar | 600 |
| Grupo de back-end, básico | 100, conjunto de disponibilidad único |
| Grupo de back-end, estándar | 1000, red virtual única |
| Recursos de back-end por Load Balancer, Estándar * | 150 |
| Puertos de alta disponibilidad, estándar | 1 por front-end interno |

** Hasta 150 recursos, cualquier combinación de máquinas virtuales independientes, conjuntos de disponibilidad y conjuntos de escalado de máquinas virtuales.

Si necesita que los límites sean superiores a lo establecido de manera predeterminada, [póngase en contacto con el soporte técnico](../articles/azure-supportability/resource-manager-core-quotas-request.md ).

