---
title: archivo de inclusión
description: archivo de inclusión
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: include
ms.date: 02/19/2018
ms.author: jaredro
ms.custom: include file
ms.openlocfilehash: 9e16c3bb4e4e5ba384bdc664e2c056acb0b70caf
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56448235"
---
### <a name="what-is-expressroute-direct"></a>¿Qué es ExpressRoute Direct?

ExpressRoute Direct ofrece a los clientes la capacidad para conectarse directamente a la red global de Microsoft en ubicaciones de emparejamiento distribuidas estratégicamente por todo el mundo. ExpressRoute Direct proporciona conectividad dual de 100 Gbps, que es compatible con la conectividad activa/activa a escala. 

### <a name="how-do-customers-connect-to-expressroute-direct"></a>¿Cómo se conecta a los clientes a ExpressRoute Direct? 

Los clientes tendrán que trabajar con sus operadores locales y proveedores de ubicación compartida para obtener conectividad a los enrutadores de ExpressRoute y aprovechar las ventajas de ExpressRoute Direct.

### <a name="what-locations-will-the-100-gbps-expressroute-direct-be-available-for-public-preview"></a>¿En qué ubicaciones estará disponible ExpressRoute Direct a 100 Gbps para la versión preliminar pública? 

Un determinado número de ubicaciones de emparejamiento de ExpressRoute lo admitirán en la versión preliminar pública. Los puertos disponibles serán dinámicos y estarán disponibles por PowerShell para ver la capacidad. Las ubicaciones incluyen y *están sujetas a cambios según disponibilidad*:

* Ámsterdam
* Canberra
* Chicago
* Washington DC
* Dallas 
* RAE de Hong Kong
* Los Ángeles
* Ciudad de Nueva York
* París
* San Antonio
* Silicon Valley
* Singapur 

### <a name="what-is-the-sla-for-expressroute-direct"></a>¿Qué es el Acuerdo de Nivel de Servicio de ExpressRoute Direct?

ExpressRoute Direct utilizará el mismo [nivel empresarial que ExpressRoute](https://azure.microsoft.com/support/legal/sla/expressroute/v1_3/). 

### <a name="what-scenarios-should-customers-consider-with-expressroute-direct"></a>¿Qué escenarios deben considerar los clientes con ExpressRoute Direct?  

ExpressRoute Direct proporciona a los clientes pares de puertos directos de 100 Gbps en la red troncal global de Microsoft. Los escenarios que proporcionarán a los clientes los mayores beneficios incluyen: ingesta de datos masiva, aislamiento físico para los mercados regulados y capacidad dedicada para el escenario de ráfaga, como la representación. 

### <a name="what-is-the-billing-model-for-expressroute-direct"></a>¿Qué es el modelo de facturación de ExpressRoute Direct? 

ExpressRoute Direct se facturará por el par de puertos a una cantidad fija. Los circuitos estándar se incluirán sin horas adicionales y los premium tendrán un ligero cargo adicional. La salida se facturará por circuito en función de la zona de la ubicación del emparejamiento.

### <a name="when-does-billing-state-for-the-expressroute-direct-port-pairs"></a>¿Cuándo cambia el estado de facturación para los pares de puertos directos de ExpressRoute?

Los pares de puertos directos de ExpressRoute se facturan 45 días después de la creación del recurso directo de ExpressRoute o cuando se activan uno o ambos vínculos, lo que suceda primero. Este período de gracia de 45 días se concede para permitir a los clientes completar el proceso de conexión cruzada con el proveedor de colocaciones.
