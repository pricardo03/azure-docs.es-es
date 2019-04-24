---
title: Acerca de ExpressRoute Direct en Azure | Microsoft Docs
description: Esta página proporciona información general sobre ExpressRoute Direct
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: fb9dc5116ba23d57c7f2fe543e734759e8bbcc7b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60367648"
---
# <a name="about-expressroute-direct"></a>Acerca de ExpressRoute Direct

ExpressRoute Direct le ofrece la capacidad para conectarse directamente a la red global de Microsoft en ubicaciones de emparejamiento distribuidas estratégicamente por todo el mundo. ExpressRoute Direct proporciona conectividad dual de 100 Gbps, que es compatible con la conectividad activa/activa a escala.

Algunas de las características clave que ofrece ExpressRoute Direct incluyen:

* Ingesta de datos masivos en servicios como Storage y Cosmos DB
* Aislamiento físico para sectores regulados y que necesitan una conectividad dedicada y aislada, como: banca, gubernamentales y venta al por menor
* Control granular del circuito de distribución en función de la unidad de negocio

## <a name="onboard-to-expressroute-direct"></a>Incorporación a ExpressRoute directa

Antes de usar ExpressRoute Direct, se debe inscribir su suscripción. Para ello, envíe un correo electrónico a <ExpressRouteDirect@microsoft.com> con el identificador de la suscripción, incluidos los detalles siguientes:

* Los escenarios que desea lograr con **ExpressRoute Direct**
* Las preferencias de ubicación: consulte el artículo [Asociados y ubicaciones de emparejamiento](expressroute-locations-providers.md) para obtener una lista completa de todas las ubicaciones
* Escala de tiempo de implementación
* ¿Alguna otra pregunta?

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute con un proveedor de servicios y ExpressRoute Direct

| **ExpressRoute con un proveedor de servicios** | **ExpressRoute Direct** | 
| --- | --- |
| Usa proveedores de servicios para permitir la incorporación y la conectividad de forma rápida en la infraestructura existente | Requiere una infraestructura de 100 Gbps y administración completa de todas las capas
| Se integra con cientos de proveedores, incluidos Ethernet y MPLS | Capacidad dedicada o de Direct para los sectores regulados y la ingesta de datos masivos |
| SKU de circuitos de 50 Mbps a 10 Gbps | El cliente puede seleccionar una combinación de las SKU del circuito siguientes: 5 Gbps, 10 Gbps, 40 Gbps, 100 GB/s, limitados a un total de 200 GB/s
| Optimizado para un solo inquilino | Optimizado para un único inquilino, proveedores de servicios en la nube o varias unidades de negocio

## <a name="expressroute-direct-circuits"></a>Circuitos de ExpressRoute Direct

Microsoft Azure ExpressRoute le permite ampliar sus redes locales en la nube de Microsoft a través de una conexión privada que facilita un proveedor de conectividad. Con ExpressRoute, se pueden establecer conexiones con servicios en la nube de Microsoft, como Microsoft Azure, Office 365 y Dynamics 365.  

Cada ubicación de emparejamiento tiene acceso a la red global de Microsoft y puede acceder a cualquier región de una zona geopolítica de forma predeterminada, así como a todas las regiones globales con un circuito premium.  

En la mayoría de los escenarios, la funcionalidad es equivalente a circuitos que usan un proveedor de servicios de ExpressRoute para su funcionamiento. Para admitir más granularidad y nuevas capacidades que se ofrecen con ExpressRoute Direct, hay ciertas capacidades claves que existen en los circuitos de ExpressRoute Direct.

## <a name="circuit-skus"></a>SKU de circuitos

ExpressRoute Direct admite escenarios de ingesta de datos masivos en Azure Storage y otros servicios de macrodatos. Ahora, los circuitos ExpressRoute en ExpressRoute Direct también admiten SKU de circuitos de **40 Gbps** y **100 Gbps**. Los pares de puerto físico son solo de **100 Gbps** y pueden tener varios circuitos virtuales con anchos de banda de 5 Gbps, 10 Gbps, 40 Gbps, 100 Gbps y hasta 200 Gbps en cualquier combinación. 

## <a name="vlan-tagging"></a>Etiquetado de VLAN

ExpressRoute Direct admite los etiquetados QinQ y Dot1Q de VLAN.

* El **etiquetado QinQ de VLAN** se permite para los dominios de enrutamiento aislados por circuito ExpressRoute. Durante la creación del circuito, Azure asigna de forma dinámica una etiqueta S que no se puede cambiar. Cada emparejamiento del circuito (privado y de Microsoft) utilizará una etiqueta C única como VLAN. No es necesario que la etiqueta C sea única en los circuitos de los puertos de ExpressRoute Direct.

* El **etiquetado Dot1Q de VLAN** se permite para una única VLAN etiquetada por puerto de ExpressRoute Direct. Una etiqueta C que se use en un emparejamiento debe ser única en todos los circuitos y emparejamientos del par de puertos de ExpressRoute Direct.

## <a name="workflow"></a>Flujo de trabajo

[![workflow](./media/expressroute-erdirect-about/workflow1.png)](./media/expressroute-erdirect-about/workflow1.png#lightbox)

## <a name="sla"></a>Contrato de nivel de servicio

ExpressRoute Direct proporciona el mismo acuerdo de nivel de servicio de nivel empresarial con conexiones redundantes activa-activa a la red global de Microsoft. La infraestructura de ExpressRoute es redundante, así como la conectividad a la red global de Microsoft es redundante y diversa y se escala conforme a los requisitos del cliente. 

## <a name="next-steps"></a>Pasos siguientes

[Configuración de ExpressRoute Direct](expressroute-howto-erdirect.md)
