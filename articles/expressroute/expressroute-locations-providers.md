---
title: 'Ubicaciones y proveedores de conectividad: Azure ExpressRoute | Microsoft Docs'
description: Este artículo ofrece una introducción detallada de ubicaciones en la que se ofrecen los servicios e información sobre cómo conectarse a regiones de Azure. Se ordenan por ubicación.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/20/2019
ms.author: pareshmu
ms.openlocfilehash: 8253610bd0c329c952442044586236acc56d4957
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978100"
---
# <a name="expressroute-partners-and-peering-locations"></a>Asociados de ExpressRoute y ubicaciones de emparejamiento

> [!div class="op_single_selector"]
> * [Ubicaciones por proveedor](expressroute-locations.md)
> * [Proveedores por ubicación](expressroute-locations-providers.md)


Las tablas de este artículo proporcionan información acerca de los proveedores de conectividad de ExpressRoute, la cobertura geográfica de ExpressRoute, los Servicios en la nube de Microsoft compatibles a través de ExpressRoute y los integradores de sistemas de ExpressRoute (SI).

## <a name="partners"></a>Proveedores de conectividad ExpressRoute
ExpressRoute es compatible con todas las ubicaciones y regiones de Azure. El siguiente mapa proporciona una lista de regiones de Azure y ubicaciones de ExpressRoute. Las ubicaciones de ExpressRoute hacen referencia a los emparejamientos de Microsoft con varios proveedores de servicio.

![Mapa de ubicación][0]

Tendrá acceso a los servicios de Azure en todas las regiones dentro de una región geopolítica si se conectó al menos a una ubicación de ExpressRoute dentro de la región geopolítica. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Regiones de Azure para ubicaciones de ExpressRoute dentro de una región geopolítica
La siguiente tabla proporciona un mapa de las regiones de Azure para las ubicaciones de ExpressRoute dentro de una región geopolítica.

| **Región geopolítica** | **Zona** | **Regiones de Azure** | **Ubicaciones de ExpressRoute** |
| --- | --- | --- | --- |
| **Australia Government** | 1 | Centro de Australia, Centro de Australia 2 |Canberra, Canberra 2 |
| **Europa** | 1 |Centro de Francia, Sur de Francia, Europa del Norte, Europa Occidental, Oeste de Reino Unido, Sur de Reino Unido |Ámsterdam, Amsterdam2, Dublín, Fráncfort, Londres, London2, Marsella, Newport (Wales), París, Zurich |
| **Norteamérica** | 1 |Este de EE. UU., oeste de EE. UU., este de EE. UU. 2, oeste de EE.UU. 2, centro de EE. UU., centro-sur de EE. UU., centro-norte de EE. UU., centro-oeste de EE. UU., centro de Canadá y este de Canadá |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Ángeles, Miami, Nueva York, San Antonio, Seattle, Silicon Valley, Silicon Valley2, DC2 de Washington, Washington, DC, Montreal, ciudad de Quebec, Toronto |
| **Asia** | 2 |Este de Asia y Sudeste de Asia |Hong Kong RAE, Kuala Lumpur, Singapur, singapur2, Taipei |
| **Australia** | 2 |Este de Australia y Sudeste de Australia |Melbourne, Perth, Sidney | 
| **India** | 2 |India occidental, India central, India del Sur |Chennai (Madrás), Chennai (Madrás)2, Mumbai (Bombay), Mumbai (Bombay)2 |
| **Japón** | 2 |Oeste de Japón y Este de Japón |Osaka, Tokyo |
| **Corea del Sur** | 2 |Corea Central, Corea del Sur |Busan, Seúl|
| **UAE** | 3 | Centro de eau, Norte de eau | Dubai, Dubai2 |
| **Sudáfrica** | 3 |Oeste de Sudáfrica, Norte de Sudáfrica |Ciudad del cabo, Johannesburgo |
| **Sudamérica** | 3 |Sur de Brasil |Sao Paulo |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regiones y límites geopolíticos para nubes nacionales
En la tabla siguiente se proporciona información sobre las regiones y los límites geopolíticos para nubes nacionales.

| **Región geopolítica** | **Regiones de Azure** | **Ubicaciones de ExpressRoute** |
| --- | --- | --- |
| **Nube del gobierno de Estados Unidos** |Arizona Gob. EE. UU., Iowa Gob. EE. UU., Texas Gob. EE.UU., Virginia Gob. EE.UU., US DoD Central, US DoD Este  |Chicago, Dallas, New York, Phoenix, San Antonio, Seattle, Silicon Valley, Washington DC |
| **Este de China** |Este de China, Este de China 2 |Shanghai, Shanghai2 |
| **Norte de China** |Norte de China, Norte de China 2 |Beijing, Beijing2 |
| **Alemania** |Centro de Alemania, Alemania oriental |Berlín+, Fráncfort |

No se admite la conectividad entre las regiones geopolíticas en el SKU de ExpressRoute estándar. Debe habilitar el complemento premium de ExpressRoute para admitir conectividad global. No se admite la conectividad con entornos de nube nacionales. Puede trabajar con su proveedor de conectividad si surge tal necesidad.

## <a name="locations"></a>Ubicaciones del proveedor de conectividad

La siguiente tabla muestra las ubicaciones de conectividad y los proveedores de servicios para cada ubicación. Si desea ver los proveedores de servicios y las ubicaciones en las que pueden prestar servicio, consulte [Ubicaciones por proveedor de servicios](expressroute-locations.md#locations). 

**Regiones de Azure local** son los que [ExpressRoute Local](expressroute-faqs.md) en cada ubicación de emparejamiento puede tener acceso. **n/d** indica que ExpressRoute Local no está disponible en esa ubicación de emparejamiento.


### <a name="production-azure"></a>Azure para producción
| **Ubicación** | **Propietario de la ubicación de emparejamiento** | **Regiones de Azure locales** | **Proveedores de servicios** |
| --- | --- | --- | --- |
| **Ámsterdam** | Equinix | Europa occidental | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Telia Carrier, Verizon y Zayo |
| **Ámsterdam2** | Interxion | Europa occidental | DE-CIX, Interxion |
| **Atlanta** | Equinix | N/D | Equinix, Megaport |
| **Busán** |LG CNS | Corea del Sur | LG CNS |
| **Canberra** | CDC | Centro de Australia | CDC |
| **Canberra 2** | CDC | Centro de Australia 2| CDC |
| **Ciudad del cabo** | Teraco | Oeste de Sudáfrica | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | Sur de la India | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | Sur de la India | Airtel |
| **Chicago** | Equinix | Centro-Norte de EE. UU | Aryaka Networks, AT & T NetBond, Cologix, Comcast, Coresite, Equinix, InterCloud, Internet2, nivel 3 PacketFabric Communications, Megaport, PCCW Global Limited, Sprint, Telia transportista, Verizon y Zayo |
| **Dallas** | Equinix | N/D | Aryaka Networks, AT & T NetBond, Cologix, Equinix, Internet2, Level 3 Communications, Megaport, Neutrona redes, Telmex Uninet, Telia transportista, Verizon y Zayo|
| **Denver** | CoreSite | Centro occidental de EE.UU. | CoreSite, Megaport y Zayo |
| **Dubai** | Etisalat eau | Norte de Emiratos Árabes Unidos | Etisalat eau |
| **Dubai2** | du datamena | Norte de Emiratos Árabes Unidos | du datamena |
| **Dublín** | Equinix | Europa del Norte | Colt, eir, Equinix, Interxion, Megaport |
| **Fráncfort** | Interxion | N/D | DE-CIX, Interxion |
| **Hong Kong SAR** | Equinix | Asia oriental | Aryaka Networks, British Telecom, China Telecom Global, Equinix, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications y Verizon |
| **Johannesburgo** | Teraco | Norte de Sudáfrica | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Kuala Lumpur** | TIME dotCom | N/D | TIME dotCom |
| **Las Vegas** | Cambiar | N/D | CenturyLink Cloud Connect, Megaport |
| **Londres** | Equinix | Sur de Reino Unido 2 | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **Londres2** | Telehouse | Sur de Reino Unido 2 | IX alcance, Equinix |
| **Los Ángeles** | CoreSite | N/D | CoreSite, Equinix, Megaport, Neutrona Networks, NTT, Zayo |
| **Marsella** |Interxion | Sur de Francia | Interxion |
| **Melbourne** | NextDC | Sudeste de Australia | AARNet, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation, TPG Telecom |
| **Miami** | Equinix | N/D | C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** | Cologix | N/D | Bell Canada, Cologix, Telus, Zayo |
| **Mumbai (Bombay)** | Tata Communications | Oeste de la India | CloudXchange global (GCX), dependencia Jio, Sify, Tata Communications y |
| **Mumbai2** | Airtel | Oeste de la India | Airtel, Sify y Vodafone Idea |
| **Nueva York** | Equinix | N/D | Conectar CenturyLink en la nube, Coresite, Equinix, InterCloud, Megaport, paquetes, y Zayo |
| **Newport (Gales)** | Datos de última generación | Oeste de Reino Unido | British Telecom, Level 3 Communications, Next Generation Data |
| **Osaka** | Equinix | Oeste de Japón | Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **París** | Interxion | Centro de Francia | Colt, Equinix, Intercloud, Interxion, naranja, Telia transportista, y Zayo |
| **Perth** | NextDC | N/D | Megaport, NextDC |
| **Quebec ciudad** | 4Degrees | Este de Canadá | Bell Canada, Megaport |
| **San Antonio** | CyrusOne | Centro-Sur de EE. UU | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | Equinix | Sur de Brasil | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica y UOLDIVEO |
| **Seattle** | Equinix | Oeste de EE. UU. 2 | Aryaka Networks, Equinix, Level 3 Communications, Megaport y Zayo |
| **Seúl** | KINX | Corea Central | KINX, LG CNS, Sejong Telecom |
| **Silicon Valley** | Equinix | Oeste de EE. UU. | Aryaka Networks, AT & T NetBond, British Telecom, CenturyLink en la nube conectarse, Comcast, Coresite, Equinix, InterCloud, llegar a IX, paquetes, PacketFabric, Level 3 Communications, Megaport, Orange, Sprint, Tata Communications, Verizon, y Zayo |
| **Silicon Valley2** | Coresite | Oeste de EE. UU. | Coresite | 
| **Singapur** | Equinix | Sudeste asiático | Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon y Vodafone |
| **Singapur2** | Modificador global | Sudeste asiático | Colt, épsilon Global Communications, Megaport, SingTel |
| **Sidney** | Equinix | Este de Australia | AARNet, AT & T NetBond, British Telecom, Devoli, Equinix, Kordia, Megaport, NEXTDC, NTT Communications, Optus, Orange, Telstra Corporation, TPG Telecom y Verizon |
| **Taipéi** | Chief Telecom | N/D | Jefe de telecomunicaciones, FarEasTone |
| **Tokio** | Equinix | Este de Japón | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT EAST, Orange, Softbank, Verizon |
| **Toronto** | Cologix | Centro de Canadá | AT & T NetBond, Bell Canada, CenturyLink en la nube conectarse, Cologix, Equinix, IX llegar Megaport, Telus, Zayo |
| **Washington DC** | Equinix | Este de EE.UU., este de EE.UU. 2 | Aryaka Networks, AT & T NetBond, British Telecom, Cologix, Comcast, Coresite, Equinix, Internet2, InterCloud, Level 3 Communications, Megaport, Neutrona redes, NTT Communications, Orange, PacketFabric, Sprint, Tata Communications, Telia transportista, Verizon y Zayo |
| **Washington DC2** | Coresite | Este de EE.UU., este de EE.UU. 2 |Coresite | 
| **Zurich** | Interxion | N/D | Interxion |

 **+** indica próximamente

### <a name="national-cloud-environments"></a>Entornos de nube nacionales

### <a name="us-government-cloud"></a>Nube del gobierno de Estados Unidos
| **Ubicación** | **Proveedores de servicios** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications y Verizon |
| **Dallas** |Equinix, Megaport y Verizon |
| **Nueva York** |Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | AT&T NetBond, CenturyLink Cloud Connect |
| **San Antonio** | CenturyLink Cloud Connect, Megaport |
| **Silicon Valley** | Equinix, Level 3 Communications y Verizon |
| **Seattle** | Equinix, Megaport |
| **Washington DC** |AT & T NetBond, Equinix, Level 3 Communications, Megaport y Verizon |

### <a name="china"></a>China
| **Ubicación** | **Proveedores de servicios** |
| --- | --- |
| **Beijing** |China Telecom |
| **Beijing2** | GDS |
| **Shanghai** |China Telecom |
| **Shanghai2** | GDS |

Para más información, consulte [ExpressRoute en China](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Alemania
| **Ubicación** | **Proveedores de servicios** |
| --- | --- |
| **Berlín** |e-shelter, Megaport+, T-Systems |
| **Fráncfort** |Colt, Equinix e Interxion |

## <a name="c1partners"></a>Conectividad a través de proveedores de intercambio
Si su proveedor de conectividad no aparece en la lista de las secciones anteriores, puede crear una conexión.

* Consulte con el proveedor de conectividad para ver si existe una conexión con cualquiera de los intercambios de la tabla anterior. Puede comprobar los vínculos siguientes para recopilar más información sobre los servicios ofrecidos por proveedores de Exchange. Varios proveedores de conectividad ya están conectados a los intercambios de Ethernet.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [Equinix Cloud Exchange](https://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](https://www.interxion.com/)
  * [NextDC](https://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)
  
* Haga que el proveedor de conectividad amplíe su red a la ubicación de emparejamiento que elija.
  * Asegúrese de que su proveedor de conectividad amplíe la conectividad con una alta disponibilidad para que no haya ningún punto único de error.
* Solicitar un circuito ExpressRoute con el intercambio como proveedor de conectividad para conectarse a Microsoft.
  * Siga los pasos de la sección [Creación de un circuito ExpressRoute](expressroute-howto-circuit-classic.md) para configurar la conectividad.

## <a name="c1partners"></a>Conectividad a través de proveedores de servicios
| **Ubicación** | **Exchange** | **Proveedores de conectividad** |
| --- | --- | --- |
| **Ámsterdam** | Equinix y Telecity | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf Bridge International, MainOne, Nianet, Post, Proximus, TDC Erhverv, Telecom Italia Sparkle, Telia |
| **Ciudad del cabo** | Teraco | MTN |
| **Chicago** | Equinix | Lightower, Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telecom, Cox Business, Data Foundry, Transtelco |
| **Fráncfort** | Telecity | BICS, Cinia, Nianet y QSC AG |
| **Hamburgo** | Equinix | Cinia |
| **Hong Kong SAR** | Equinix | Chief y Macroview Telecom |
| **Johannesburgo** | Teraco | MTN |
| **Londres** | BICS, Equinix, euNetworks, Telecity | Bezeq International Ltd., CoreAzure, Epsilon Telecommunications Limited, exponencial E, HSO, NexGen Networks, Proximus Tamares Telecom, Zain |
| **Los Ángeles** | Equinix |Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix y Equinix | Airgate Technologies, Inc. Cogeco Peer 1, Rogers y Zirro |
| **Nueva York** |Equinix, Megaport | Altice Business, Lightower, Webair |
| **París** | Equinix | Proximus |
| **Quebec ciudad** | Megaport | Fibrenoire |
| **Sao Paulo** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Silicon Valley** |Equinix | Cox Business, Windstream |
| **Singapur** |Equinix |1CLOUDSTAR, BICS, Epsilon Telecommunications Limited, LGA Telecom y United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sidney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix | Airgate Technologies, Inc. Cogeco Peer 1, IVedha Inc, Rogers, Thinktel y Zirro|
| **Washington DC** |Equinix | Altice Business, BICS, Cox Business, Gtt Communications Inc, Epsilon Telecommunications Limited, Lightower, Masergy y Windstream |

## <a name="expressroute-system-integrators"></a>Integradores de sistemas de ExpressRoute
Habilitar la conectividad privada para la adaptación a sus necesidades puede ser complicado según la escala de la red. Puede trabajar con cualquiera de los integradores de sistemas que aparecen en la tabla siguiente para ayudarle con la incorporación a ExpressRoute.

| **Continente** | **Integradores de sistemas** |
| --- | --- |
| **Asia** |Avanade Inc. y OneAs1a |
| **Australia** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Europa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Norteamérica** |Avanade Inc., Equinix Professional Services, FlexManage, Lightstream, Perficient y Presidio |
| **Sudamérica** |Avanade Inc., Venha Pra Nuvem |
## <a name="next-steps"></a>Pasos siguientes
* Para obtener más información acerca de ExpressRoute, consulte [P+F de ExpressRoute](expressroute-faqs.md).
* Asegúrese de que se cumplen todos los requisitos previos. Consulte [Requisitos previos de ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa de ubicación"
