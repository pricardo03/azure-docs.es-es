---
title: archivo de inclusión
description: archivo de inclusión
services: networking
author: anavinahar
ms.service: networking
ms.topic: include
ms.date: 06/25/2019
ms.author: anavin
ms.custom: include file
ms.openlocfilehash: 1fd08efd12e723a16445eba1d341f017a86a212e
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68481559"
---
<a name="azure-resource-manager-virtual-networking-limits"></a>Límites de redes: Azure Resource Manager Los siguientes límites solo son válidos en los recursos de redes administrados mediante **Azure Resource Manager** por región y por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

> [!NOTE]
> Recientemente hemos aumentado todos los límites predeterminados a sus límites máximos. Si no hay ninguna columna de límite máximo, el recurso no tiene límites ajustables. Si el soporte técnico ha aumentado estos límites en el pasado y no ve los límites actualizados en las tablas siguientes, [abra una solicitud de soporte técnico al cliente en línea sin cargo](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| Recurso | Límite predeterminado/máximo | 
| --- | --- |
| Redes virtuales |1000 |
| Subredes por red virtual |3000 |
| Emparejamientos de redes virtuales por red virtual |500 |
| Servidores DNS por red virtual |20 |
| Direcciones IP privadas por red virtual |65 536 |
| Direcciones IP privadas por interfaz de red |256 |
| Direcciones IP privadas por máquina virtual |256 |
| Direcciones IP públicas por interfaz de red |256 |
| Direcciones IP públicas por máquina virtual |256 |
| Conexiones TCP concurrentes o flujo UDP por NIC de una máquina virtual o instancia de rol |500.000 |
| Tarjetas adaptadoras de red |65 536 |
| Grupos de seguridad de red |5\.000 |
| Reglas de NSG por NSG |1000 |
| Direcciones IP y rangos especificados para el origen o destino en un grupo de seguridad |4\.000 |
| Grupos de seguridad de aplicaciones |3000 |
| Grupos de seguridad de aplicaciones por configuración de IP, por NIC |20 |
| Configuraciones de IP por grupo de seguridad de aplicaciones |4\.000 |
| Grupos de seguridad de aplicaciones que se pueden especificar en todas las reglas de seguridad de un grupo de seguridad de red |100 |
| Tablas de rutas definidas por el usuario |200 |
| Rutas definidas por el usuario por tabla de ruta |400 |
| Certificados raíz de punto a sitio por Azure VPN Gateway |20 |
| TAP de red virtual |100 |
| Configuraciones de TAP de la interfaz de red por TAP de red virtual |100 |

#### <a name="publicip-address"></a>Límites de dirección IP pública
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Direcciones IP públicas - dinámicas | 1000 para Basic. |Póngase en contacto con el servicio de soporte técnico. |
| Direcciones IP públicas - estáticas | 1000 para Basic. |Póngase en contacto con el servicio de soporte técnico. |
| Direcciones IP públicas - estáticas | 200 para Estándar.|Póngase en contacto con el servicio de soporte técnico. |
| Longitud del prefijo de IP pública | /28 | Póngase en contacto con el servicio de soporte técnico. |

#### <a name="load-balancer"></a>Límites del equilibrador de carga
Los límites siguientes solo se aplican a los recursos de redes administrados a través de Azure Resource Manager por región y por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

| Recurso | Límite predeterminado/máximo |
| --- | --- |
| Equilibradores de carga | 1000 | 
| Reglas por recurso, básico | 250 |
| Reglas por recurso, estándar | 1500 | 
| Reglas por configuración de dirección IP | 299 |
| Reglas por NIC | 300 |
| Configuraciones de direcciones IP de front-end, Básico | 200 |
| Configuraciones de direcciones IP de front-end, Estándar | 600 |
| Grupo de servidores back-end, Básico | 100, conjunto de disponibilidad único |
| Grupo de servidores back-end, Estándar | 1000, red virtual única |
| Recursos del servidor back-end por equilibrador de carga, Estándar<sup>1</sup> | 150 |
| Puertos de alta disponibilidad, Estándar | 1 por front-end interno |

<sup>1</sup>El límite es de hasta 150 recursos, en cualquier combinación de recursos de máquinas virtuales independientes, recursos de conjuntos de disponibilidad y recursos de conjuntos de escalado de máquinas virtuales.

#### <a name="virtual-networking-limits-classic"></a>Los límites siguientes se aplican solo a los recursos de redes administrados a través del modelo de implementación **clásico** por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Redes virtuales |100 |100 |
| Sitios de red local |20 |Póngase en contacto con el servicio de soporte técnico. |
| Servidores DNS por red virtual |20 |20 |
| Direcciones IP privadas por red virtual |4 096 |4 096 |
| Conexiones TCP concurrentes o flujo UDP por NIC de una máquina virtual o instancia de rol |500 000, hasta 1 000 000 de dos o varias NIC. |500 000, hasta 1 000 000 de dos o varias NIC. |
| Grupos de seguridad de red (NSG) |200 |200 |
| Reglas de NSG por NSG |1000 |1000 |
| Tablas de rutas definidas por el usuario |200 |200 |
| Rutas definidas por el usuario por tabla de ruta |400 |400 |
| Direcciones IP públicas (dinámicas) |500 |500 |
| Direcciones IP públicas reservadas |500 |500 |
| VIP pública por implementación |5 |Ponerse en contacto con soporte técnico |
| VIP privada (equilibrio de carga interno) por implementación |1 |1 |
| Listas de control de acceso (ACL) de punto de conexión |50 |50 |
