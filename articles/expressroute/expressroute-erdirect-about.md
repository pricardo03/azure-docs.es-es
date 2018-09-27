---
title: Acerca de Azure ExpressRoute Direct | Microsoft Docs
description: En esta página se proporciona información general sobre ExpressRoute Direct (versión preliminar)
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: c33bec76fe17336221c873778c2993d75fec81e8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962239"
---
# <a name="about-expressroute-direct-preview"></a>Acerca de ExpressRoute Direct (versión preliminar)

ExpressRoute Direct ofrece a los clientes la capacidad para conectarse directamente a la red global de Microsoft en ubicaciones de emparejamiento distribuidas estratégicamente por todo el mundo. ExpressRoute Direct proporciona conectividad dual de 100 Gbps, que es compatible con la conectividad activa/activa a escala. 

Algunas de las características clave que ofrece ExpressRoute Direct incluyen:

* Ingesta de datos masivos en servicios como Storage y Cosmos DB 
* Aislamiento físico para sectores regulados y que necesitaron conectividades dedicadas y aisladas, como gubernamentales, con la banca y de venta directa 
* Control granular del circuito de distribución en función de la unidad de negocio

> [!IMPORTANT]
> ExpressRoute Direct se encuentra actualmente en versión preliminar.
>
> Esta versión preliminar pública se proporciona sin un contrato de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enroll-in-the-preview"></a>Inscripción en la versión preliminar

Para poder utilizar ExpressRoute Direct, primero hay que inscribir una suscripción en la versión preliminar. Para inscribirse, envíe un correo electrónico a <ExpressRouteDirect@microsoft.com> con su identificador de suscripción. ExpressRoute Direct es una característica de nivel empresarial. Proporcione detalles adicionales:

* Los escenarios que desea lograr con **ExpressRoute Direct**
* Las preferencias de ubicación: consulte el artículo [Asociados y ubicaciones de emparejamiento](expressroute-locations-providers.md) para obtener una lista completa de todas las ubicaciones
* Escala de tiempo de implementación
* Todas sus dudas con respecto a los servicios

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute con un proveedor de servicios y ExpressRoute Direct

| **ExpressRoute con un proveedor de servicios** | **ExpressRoute Direct** | 
| --- | --- | 
| Utiliza el proveedor de servicios para habilitar la incorporación rápida y la conectividad en la infraestructura existente | Requiere una infraestructura de 100 Gbps y administración completa de todas las capas
| Se integra con cientos de proveedores, incluidos Ethernet y MPLS | Capacidad dedicada o de Direct para los sectores regulados y la ingesta de datos masivos | 
| SKU de circuitos de 50 Mbps a 10 Gbps | SKU de circuitos de 1 Gbps a 100 Gbps
| Optimizado para un solo inquilino | Optimizado para un único inquilino, proveedores de servicios en la nube o varias unidades de negocio

## <a name="expressroute-direct-circuits"></a>Circuitos de ExpressRoute Direct

Microsoft Azure ExpressRoute le permite ampliar sus redes locales en la nube de Microsoft a través de una conexión privada que facilita un proveedor de conectividad. Con ExpressRoute, se pueden establecer conexiones con servicios en la nube de Microsoft, como Microsoft Azure, Office 365 y Dynamics 365.  

Cada ubicación de emparejamiento tiene acceso a la red global de Microsoft y puede acceder a cualquier región de una zona geopolítica de forma predeterminada, así como a todas las regiones globales con un circuito premium.  

En la mayoría de los escenarios, la funcionalidad es equivalente a circuitos que usan un proveedor de servicios de ExpressRoute para su funcionamiento. Para admitir más granularidad y nuevas capacidades que se ofrecen con ExpressRoute Direct, hay ciertas capacidades claves que existen en los circuitos de ExpressRoute Direct.

## <a name="circuit-skus"></a>SKU de circuitos

ExpressRoute Direct admite escenarios de ingesta de datos masivos en Azure Storage y otros servicios de macrodatos. Ahora, los circuitos ExpressRoute en ExpressRoute Direct también admiten SKU de circuitos de **40 G** y **100 G**. 

## <a name="vlan-tagging"></a>Etiquetado de VLAN

ExpressRoute Direct admite los etiquetados QinQ y Dot1Q de VLAN.

* El **etiquetado QinQ de VLAN** se permite para los dominios de enrutamiento aislados por circuito ExpressRoute. Durante la creación del circuito, Azure asigna de forma dinámica una etiqueta S que no se puede cambiar. Cada emparejamiento del circuito (privado y de Microsoft) utilizará una etiqueta C única como VLAN. No es necesario que la etiqueta C sea única en los circuitos de los puertos de ExpressRoute Direct. 

* El **etiquetado Dot1Q de VLAN** se permite para una única VLAN etiquetada por puerto de ExpressRoute Direct. Una etiqueta C que se use en un emparejamiento debe ser única en todos los circuitos y emparejamientos del par de puertos de ExpressRoute Direct.

## <a name="workflow"></a>Flujo de trabajo

![flujo de trabajo](./media/expressroute-erdirect-about/workflow1.png)

## <a name="sla"></a>Contrato de nivel de servicio

ExpressRoute Direct proporciona el mismo acuerdo de nivel de servicio de nivel empresarial con conexiones redundantes activa-activa a la red global de Microsoft. La infraestructura de ExpressRoute es redundante, así como la conectividad a la red global de Microsoft es redundante y diversa y se escala conforme a los requisitos del cliente. Durante la versión preliminar, no habrá ningún acuerdo de nivel de servicio y solo se debe considerar para las cargas de trabajo que no sean de producción.

## <a name="next-steps"></a>Pasos siguientes

[Configuración de ExpressRoute Direct](expressroute-howto-erdirect.md)