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
ms.date: 07/31/2019
ms.author: cherylmc
ms.openlocfilehash: b31a7c1cbd0f4f6cadc4ac139777d2cc3c451bcb
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706475"
---
# <a name="expressroute-partners-and-peering-locations"></a>Asociados de ExpressRoute y ubicaciones de emparejamiento

> [!div class="op_single_selector"]
> * [Ubicaciones por proveedor](expressroute-locations.md)
> * [Proveedores por ubicación](expressroute-locations-providers.md)


Las tablas de este artículo proporcionan información acerca de la cobertura geográfica y las ubicaciones de ExpressRoute, los proveedores de conectividad de ExpressRoute y los integradores de sistemas (SI) de ExpressRoute.

> [!Note]
> Las regiones de Azure y las ubicaciones de ExpressRoute son dos conceptos diferentes y conocer dicha diferencia es fundamental para explorar la conectividad de red híbrida de Azure. 
>
>

## <a name="azure-regions"></a>Regiones de Azure
Las regiones de Azure son centros de datos globales en los que se encuentran los recursos de proceso, red y almacenamiento de Azure. Al crear un recurso de Azure, los clientes deben seleccionar la ubicación del recurso. Esta ubicación determina el centro de datos (o la zona de disponibilidad) de Azure en que se crea el recurso.

## <a name="expressroute-locations"></a>Ubicaciones de ExpressRoute
Las ubicaciones de ExpressRoute (a las que a veces se denomina ubicaciones de emparejamiento o ubicaciones de punto de encuentro) son instalaciones de ubicación compartida en las que se encuentran los dispositivos de Microsoft Enterprise Edge (MSEE). Las ubicaciones de ExpressRoute son el punto de entrada a la red de Microsoft (y se distribuyen globalmente, lo que proporciona a los clientes la oportunidad de conectarse a la red de Microsoft en todo el mundo). Estas ubicaciones son donde los asociados de ExpressRoute y los clientes ExpressRoute Direct emiten conexiones cruzadas a la red de Microsoft. En general, no es preciso que la ubicación de ExpressRoute coincida con la región de Azure. Por ejemplo, un cliente puede crear un circuito ExpressRoute con la ubicación de recursos *Este de EE. UU.* , en la ubicación de emparejamiento*Seattle*.

Tendrá acceso a los servicios de Azure en todas las regiones dentro de una región geopolítica si se conectó al menos a una ubicación de ExpressRoute dentro de la región geopolítica. 

## <a name="locations"></a>Regiones de Azure para las ubicaciones de ExpressRoute dentro de una región geopolítica
La siguiente tabla proporciona un mapa de las regiones de Azure para las ubicaciones de ExpressRoute dentro de una región geopolítica.

| **Región geopolítica** | **Zona** | **Regiones de Azure** | **Ubicaciones de ExpressRoute** |
| --- | --- | --- | --- |
| **Australia Government** | 1 | Centro de Australia, Centro de Australia 2 |Canberra, Canberra 2 |
| **Europa** | 1 |Centro de Francia, Sur de Francia, Europa del Norte, Europa Occidental, Oeste de Reino Unido, Sur de Reino Unido |Ámsterdam, Ámsterdam2, Copenhague, Dublín, Fráncfort, Londres, Londres2, Marsella, Newport (Gales), París, Estocolmo, Zúrich |
| **Norteamérica** | 1 |Este de EE. UU., oeste de EE. UU., este de EE. UU. 2, oeste de EE.UU. 2, centro de EE. UU., centro-sur de EE. UU., centro-norte de EE. UU., centro-oeste de EE. UU., centro de Canadá y este de Canadá |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Ángeles, Miami, Nueva York, San Antonio, Seattle, Silicon Valley, Silicon Valley2, Washington DC, Washington DC2, Montreal, Ciudad de Quebec, Toronto |
| **Asia** | 2 |Este de Asia y Sudeste de Asia |RAE de Hong Kong, Kuala Lumpur, Singapur, Singapur2, Taipéi |
| **India** | 2 |India occidental, India central, India del Sur |Chennai (Madrás), Chennai (Madrás)2, Mumbai (Bombay), Mumbai (Bombay)2 |
| **Japón** | 2 |Oeste de Japón y Este de Japón |Osaka, Tokyo |
| **Oceanía** | 2 |Este de Australia y Sudeste de Australia |Auckland, Melbourne, Perth, Sídney | 
| **Corea del Sur** | 2 |Corea Central, Corea del Sur |Busan, Seúl|
| **Emiratos Árabes Unidos** | 3 | Centro de Emiratos Árabes Unidos, Norte de Emiratos Árabes Unidos | Dubai, Dubai2 |
| **Sudáfrica** | 3 |Oeste de Sudáfrica, Norte de Sudáfrica |Ciudad del cabo, Johannesburgo |
| **Sudamérica** | 3 |Sur de Brasil |Sao Paulo |

## <a name="azure-regions-and-geopolitical-boundaries-for-national-clouds"></a>Regiones de Azure y límites geopolíticos de las nubes nacionales
En la tabla siguiente se proporciona información sobre las regiones y los límites geopolíticos para nubes nacionales.

| **Región geopolítica** | **Regiones de Azure** | **Ubicaciones de ExpressRoute** |
| --- | --- | --- |
| **Nube del gobierno de Estados Unidos** |Arizona Gob. EE. UU., Iowa Gob. EE. UU., Texas Gob. EE.UU., Virginia Gob. EE.UU., US DoD Central, US DoD Este  |Chicago, Dallas, New York, Phoenix, San Antonio, Seattle, Silicon Valley, Washington DC |
| **Este de China** |Este de China, Este de China 2 |Shanghai, Shanghai2 |
| **Norte de China** |Norte de China, Norte de China 2 |Beijing, Beijing2 |
| **Alemania** |Centro de Alemania, Alemania oriental |Berlín+, Fráncfort |

No se admite la conectividad entre las regiones geopolíticas en el SKU de ExpressRoute estándar. Debe habilitar el complemento premium de ExpressRoute para admitir conectividad global. No se admite la conectividad con entornos de nube nacionales. Puede trabajar con su proveedor de conectividad si surge tal necesidad.

## <a name="partners"></a>Proveedores de conectividad ExpressRoute

La siguiente tabla muestra las ubicaciones de conectividad y los proveedores de servicios para cada ubicación. Si desea ver los proveedores de servicios y las ubicaciones en las que pueden prestar servicio, consulte [Ubicaciones por proveedor de servicios](expressroute-locations.md). 

Las **regiones de Azure locales** son a las que puede acceder [ExpressRoute Local](expressroute-faqs.md) en cada ubicación de emparejamiento. **n/d** indica que ExpressRoute Local no está disponible en esa ubicación de emparejamiento.


### <a name="production-azure"></a>Azure para producción
| **Ubicación** | **Propietario de la ubicación de emparejamiento** | **Regiones de Azure locales** | **Proveedores de servicios** |
| --- | --- | --- | --- |
| **Ámsterdam** | Equinix | Europa occidental | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Telia Carrier, Verizon y Zayo |
| **Ámsterdam2** | Interxion | Europa occidental | CenturyLink Cloud Connect, DE-CIX, Interxion, Vodafone |
| **Atlanta** | Equinix | N/D | Equinix, Megaport |
| **Auckland** | Vocus Group NZ | N/D | Devoli, Kordia, Megaport, Spark NZ, Vocus Group NZ |
| **Busán** |LG CNS | Corea del Sur | LG CNS |
| **Canberra** | CDC | Centro de Australia | CDC |
| **Canberra 2** | CDC | Centro de Australia 2| CDC |
| **Ciudad del cabo** | Teraco | Oeste de Sudáfrica | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | Sur de la India | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | Sur de la India | Airtel |
| **Chicago** | Equinix | Centro-Norte de EE. UU | Aryaka Networks, AT&T NetBond, CenturyLink Cloud Connect, Cologix, Comcast, Coresite, Equinix, InterCloud, Internet2, Level 3 Communications, Megaport, PacketFabric, PCCW Global Limited, Sprint, Telia Carrier, Verizon, Zayo |
| **Copenhague** | Interxion | N/D | Interxion |
| **Dallas** | Equinix | N/D | Aryaka Networks, AT&T NetBond, Cologix, Equinix, Internet2, Level 3 Communications, Megaport, Neutrona Networks, Telmex Uninet, Telia Carrier, Transtelco, Verizon, Zayo|
| **Denver** | CoreSite | Centro occidental de EE.UU. | CoreSite, Megaport, Zayo |
| **Dubai** | Etisalat UAE | Norte de Emiratos Árabes Unidos | Etisalat UAE |
| **Dubai2** | du datamena | Norte de Emiratos Árabes Unidos | du datamena, Orixcom |
| **Dublín** | Equinix | Europa del Norte | Colt, eir, Equinix, Interxion, Megaport |
| **Fráncfort** | Interxion | N/D | DE-CIX, Interxion |
| **RAE de Hong Kong** | Equinix | Asia oriental | Aryaka Networks, British Telecom, CenturyLink Cloud Connect, Chief Telecom, China Telecom Global, Equinix, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telia Carrier, Verizon |
| **Johannesburgo** | Teraco | Norte de Sudáfrica | British Telecom, Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Kuala Lumpur** | TIME dotCom | N/D | TIME dotCom |
| **Las Vegas** | Switch | N/D | CenturyLink Cloud Connect, Megaport |
| **Londres** | Equinix | Sur de Reino Unido 2 | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **Londres2** | Telehouse | Sur de Reino Unido 2 | IX Reach, Equinix |
| **Los Ángeles** | CoreSite | N/D | CoreSite, Equinix, Megaport, Neutrona Networks, NTT, Zayo |
| **Marsella** |Interxion | Sur de Francia | DE-CIX, Interxion, Jaguar Network |
| **Melbourne** | NextDC | Sudeste de Australia | AARNet, Devoli, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation, TPG Telecom |
| **Miami** | Equinix | N/D | C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** | Cologix | N/D | Bell Canada, Cologix, Telus, Zayo |
| **Mumbai (Bombay)** | Tata Communications | Oeste de la India | Global CloudXchange (GCX), Reliance Jio, Sify, Tata Communications, Verizon |
| **Mumbai2** | Airtel | Oeste de la India | Airtel, Sify y Vodafone Idea |
| **Nueva York** | Equinix | N/D | CenturyLink Cloud Connect, Coresite, Equinix, InterCloud, Megaport, Packet, Zayo |
| **Newport (Gales)** | Datos de última generación | Oeste de Reino Unido | British Telecom, Colt, Level 3 Communications, Next Generation Data |
| **Osaka** | Equinix | Oeste de Japón | Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **París** | Interxion | Centro de Francia | CenturyLink Cloud Connect, Colt, Equinix, Intercloud, Interxion, Orange, Telia Carrier, Zayo |
| **Perth** | NextDC | N/D | Megaport, NextDC |
| **Quebec ciudad** | 4Degrees | Este de Canadá | Bell Canada, Megaport |
| **San Antonio** | CyrusOne | Centro-Sur de EE. UU | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | Equinix | Sur de Brasil | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica y UOLDIVEO |
| **Seattle** | Equinix | Oeste de EE. UU. 2 | Aryaka Networks, Equinix, Level 3 Communications, Megaport, Telus, Zayo |
| **Seúl** | KINX | Corea Central | KINX, LG CNS, Sejong Telecom |
| **Silicon Valley** | Equinix | Oeste de EE. UU. | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Comcast, Coresite, Equinix, InterCloud, IX Reach, Packet, PacketFabric, Level 3 Communications, Megaport, Orange, Sprint, Tata Communications, Telia Carrier, Verizon y Zayo |
| **Silicon Valley2** | Coresite | Oeste de EE. UU. | Coresite | 
| **Singapur** | Equinix | Sudeste asiático | Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon y Vodafone |
| **Singapur2** | Modificador global | Sudeste asiático | Colt, Epsilon Global Communications, Megaport, SingTel |
| **Estocolmo** | Equinix | N/D | Telia Carrier |
| **Sidney** | Equinix | Este de Australia | AARNet, AT&T NetBond, British Telecom, Devoli, Equinix, Kordia, Megaport, NEXTDC, NTT Communications, Optus, Orange, Spark NZ, Telstra Corporation, TPG Telecom, Verizon, Vocus Group NZ |
| **Taipéi** | Chief Telecom | N/D | Chief Telecom, FarEasTone |
| **Tokio** | Equinix | Este de Japón | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT EAST, Orange, Softbank, Verizon |
| **Toronto** | Cologix | Centro de Canadá | AT&T NetBond, Bell Canada, CenturyLink Cloud Connect, Cologix, Equinix, IX Reach Megaport, Telus, Verizon, Zayo |
| **Washington DC** | Equinix | Este de EE. UU., Este de EE. UU. 2 | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Cologix, Comcast, Coresite, Equinix, Internet2, InterCloud, Level 3 Communications, Megaport, Neutrona Networks, NTT Communications, Orange, PacketFabric, Sprint, Tata Communications, Telia Carrier, Verizon, Zayo |
| **Washington DC2** | Coresite | Este de EE. UU., Este de EE. UU. 2 |Coresite | 
| **Zúrich** | Interxion | N/D | Intercloud, Interxion |

 **+** indica próximamente

### <a name="national-cloud-environments"></a>Entornos de nube nacionales

### <a name="us-government-cloud"></a>Nube del gobierno de Estados Unidos
| **Ubicación** | **Proveedores de servicios** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications y Verizon |
| **Dallas** |Equinix, Megaport y Verizon |
| **Nueva York** |Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | AT&T NetBond, CenturyLink Cloud Connect, Megaport |
| **San Antonio** | CenturyLink Cloud Connect, Megaport |
| **Silicon Valley** | Equinix, Level 3 Communications y Verizon |
| **Seattle** | Equinix, Megaport |
| **Washington DC** |AT&T NetBond, CenturyLink Cloud Connect, Equinix, Level 3 Communications, Megaport, Verizon |

### <a name="china"></a>China
| **Ubicación** | **Proveedores de servicios** |
| --- | --- |
| **Beijing** |China Telecom |
| **Beijing2** | China Telecom, GDS |
| **Shanghai** |China Telecom |
| **Shanghai2** | China Telecom, GDS |

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
| **Ámsterdam** | Equinix, Interxion, Level 3 Communications, Telecity | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf Bridge International, Kalaam Telecom Bahrain B.S.C, MainOne, Nianet, Post, Proximus, TDC Erhverv, Telecom Italia Sparkle, Telekom Deutschland GmbH, Telia |
| **Atlanta** | Equinix| Crown Castle
| **Ciudad del cabo** | Teraco | MTN |
| **Chicago** | Equinix| Crown Castle, Spectrum Enterprise, Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telecom, Cox Business, Crown Castle, Data Foundry, Spectrum Enterprise, Transtelco |
| **Fráncfort** | Interxion, Telecity | BICS, Cinia, Nianet, QSC AG, Telekom Deutschland GmbH |
| **Hamburgo** | Equinix | Cinia |
| **RAE de Hong Kong** | Equinix | Chief y Macroview Telecom |
| **Johannesburgo** | Teraco | MTN |
| **Londres** | BICS, Equinix, euNetworks, Telecity | Bezeq International Ltd., CoreAzure, Epsilon Telecommunications Limited, Exponential E, HSO, NexGen Networks, Proximus, Tamares Telecom, Zain |
| **Los Ángeles** | Equinix |Crown Castle, Spectrum Enterprise, Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix y Equinix | Airgate Technologies, Inc. Cogeco Peer 1, Rogers y Zirro |
| **Nueva York** |Equinix, Megaport | Altice Business, Crown Castle, Spectrum Enterprise, Webair |
| **París** | Equinix | Proximus |
| **Quebec ciudad** | Megaport | Fibrenoire |
| **Sao Paulo** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Silicon Valley** |Coresite, Equinix | Cox Business, Spectrum Enterprise, Windstream, X2nsat Inc. |
| **Singapur** |Equinix |1CLOUDSTAR, BICS, Epsilon Telecommunications Limited, LGA Telecom y United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sidney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix, Megaport | Airgate Technologies Inc., Beanfield Metroconnect, Cogeco Peer 1, IVedha Inc, Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice Business, BICS, Cox Business, Crown Castle, Gtt Communications Inc, Epsilon Telecommunications Limited, Masergy, Windstream |

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
