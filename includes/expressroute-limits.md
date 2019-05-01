---
title: archivo de inclusión
description: archivo de inclusión
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 06/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d067f1af3d5479aef28ddf2290cebe3fe45726b0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64733802"
---
| Recurso | Límite predeterminado y máximo |
| --- | --- |
| Circuitos ExpressRoute por suscripción |10 |
| Circuitos ExpressRoute por región y suscripción, con Azure Resource Manager |10 |
| Número máximo de rutas para emparejamiento privado de Azure con estándar ExpressRoute |4.000 |
| Número máximo de rutas para emparejamiento privado de Azure con complemento ExpressRoute Premium |10 000 |
| Número máximo de rutas para emparejamiento privado de Azure desde el espacio de direcciones de red virtual para una conexión de ExpressRoute |200 | 
| Número máximo de rutas para emparejamiento de Microsoft Azure con estándar ExpressRoute |200 |
| Número máximo de rutas para emparejamiento de Microsoft Azure con complemento ExpressRoute Premium |200 |
| Número máximo de vinculado a la misma red virtual en la misma ubicación de emparejamiento de circuitos de ExpressRoute |4 |
| Número máximo de circuitos ExpressRoute vinculado a la misma red virtual en distintas ubicaciones de emparejamiento |> 4, depende de tamaño de GatewaySubnet|
| Número de vínculos de red virtual permitidos por circuito ExpressRoute |Consulte la tabla siguiente. |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a>Número de redes virtuales por circuito de ExpressRoute
| **Tamaño del circuito** | **Número de vínculos de red virtual para estándar** | **Número de vínculos de red virtual con complemento Premium** |
| --- | --- | --- |
| 50 Mbps |10 |20 |
| 100 Mbps |10 |25 |
| 200 Mbps |10 |25 |
| 500 Mbps |10 |40 |
| 1 Gbps |10 |50 |
| 2 Gbps |10 |60 |
| 5 Gbps |10 |75 |
| 10 Gbps |10 |100 |

