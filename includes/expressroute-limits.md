---
title: archivo de inclusión
description: archivo de inclusión
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 05/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9f7f9b1504533e0ec503e97c086c380da71671a0
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299703"
---
| Recurso | Límite predeterminado y máximo |
| --- | --- |
| Circuitos ExpressRoute por suscripción |10 |
| Circuitos ExpressRoute por región y suscripción, con Azure Resource Manager |10 |
| Número máximo de rutas anunciadas a emparejamiento privado de Azure con estándar ExpressRoute |4.000 |
| Número máximo de rutas anunciadas a emparejamiento privado de Azure con complemento ExpressRoute Premium |10 000 |
| Número máximo de rutas que anuncia de emparejamiento privado de Azure desde el espacio de direcciones de red virtual de una conexión ExpressRoute |200 |
| Número máximo de rutas anunciadas a Microsoft emparejamiento con estándar ExpressRoute |200 |
| Número máximo de rutas anunciadas a Microsoft emparejamiento con el complemento ExpressRoute Premium |200 |
| Número máximo de vinculado a la misma red virtual en la misma ubicación de emparejamiento de circuitos de ExpressRoute |4 |
| Número máximo de circuitos ExpressRoute vinculado a la misma red virtual en distintas ubicaciones de emparejamiento |4 |
| Número de vínculos de red virtual permitidos por circuito ExpressRoute |Consulte la [número de redes virtuales por circuito de ExpressRoute](#vnetpercircuit) tabla.  |

#### <a name="vnetpercircuit"></a> Número de redes virtuales por circuito de ExpressRoute
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
| 40 GB/s * |10 |100 |
| 100 GB/s * |10 |100 |

* ExpressRoute Direct solo
