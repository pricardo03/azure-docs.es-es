---
title: 'Red global de Microsoft: Azure'
description: Describe cómo Microsoft compila su red global rápida y confiable
services: networking
documentationcenter: ''
author: KumudD
manager: ''
ms.service: networking
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2019
ms.author: ypitsch,kumud
ms.openlocfilehash: 99650577b17874e61d1cede979d7ef2f4bfd491b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247295"
---
# <a name="microsoft-global-network"></a>Red global de Microsoft

Microsoft posee y administra una de las redes de la red troncal más grandes del mundo. Esta arquitectura global y sofisticada, que abarca más de 100.000 millas, conecta a nuestros centros de datos y los clientes. 
 
Cada día, los clientes en todo el mundo conexión y pasan billones de solicitudes a Microsoft Azure, Bing, Dynamics 365, Office 365, XBox y muchos otros. Independientemente del tipo, los clientes esperan instantánea confiabilidad y capacidad de respuesta de nuestros servicios. 
 
El [red global de Microsoft](https://azure.microsoft.com/global-infrastructure/global-network/) (WAN) es una parte fundamental de ofrecer una experiencia excelente en la nube. Conexión de nuestra Microsoft [centros de datos](https://azure.microsoft.com/global-infrastructure/) en 54 regiones de Azure y grande malla de nodos perimetrales de estratégicamente en todo el mundo, nuestra red global ofrece tanto la disponibilidad, capacidad y la flexibilidad para satisfacer cualquier demanda.

![Red global de Microsoft](./media/microsoft-global-network/microsoft-global-wan.png)
 
## <a name="get-the-premium-cloud-network"></a>Obtiene la red de la nube premium
 
Optar por la [mejor experiencia posible](https://www.sdxcentral.com/articles/news/azure-tops-aws-gcp-in-cloud-performance-says-thousandeyes/2018/11/) es fácil cuando se usa en la nube de Microsoft. Desde el momento cuando entra el tráfico de cliente en nuestra red global a través de nuestro nodos perimetrales estratégicamente, los datos se viajan a través de rutas optimizadas en cerca de la velocidad de la luz. Esto garantiza una latencia óptimo para un mejor rendimiento. Estos nodos perimetrales, todos conectados entre sí a más de 3500 único Internet asociados (elementos del mismo nivel) a través de miles de conexiones en más de 145 ubicaciones, proporcionan la base de nuestra estrategia de interconexión. 
 
Si se conecta de Londres a Tokio o de Washington DC en Los Ángeles, rendimiento de la red es cuantificado y se ve afectado por cosas como latencia, vibración, pérdida de paquetes y el rendimiento.  En Microsoft, es preferible y uso directo interconexiones en lugar de vínculos de tránsito, esto evita que el tráfico de respuesta simétrico y ayuda a mantener saltos, emparejamiento de entidades y las rutas de acceso más corto y sencillo posible. Este enfoque premium, suele denominarse [frío patata enrutamiento](https://en.wikipedia.org/wiki/Hot-potato_and_cold-potato_routing), garantiza que los clientes de red sigue siendo el tráfico dentro de la red de Microsoft siempre y cuando sea posible antes de que se entregan.  
 
Por lo tanto, ¿significa que cualquier tráfico al usar los servicios de Microsoft? Sí, los centros de todo el tráfico entre los datos, dentro de Microsoft Azure o entre los servicios de Microsoft, como las máquinas virtuales, Office 365, XBox, bases de datos SQL, almacenamiento, y se enrutan las redes virtuales dentro de nuestra red mundial y nunca a través de Internet, para garantizar una óptima rendimiento y la integridad.  
 
Grandes inversiones en capacidad de fibra y diversidad a través de rutas de acceso de metro, terrestre y submarino son fundamentales para que podamos mantener coherente y de alto nivel de servicio mientras las impulsando el crecimiento de extremo de la nube y servicios en línea. Son las adiciones recientes a nuestra red global nuestro [Ana](https://www.submarinecablemap.com/#/submarine-cable/marea) submarino cable, primero abrir línea del sistema (OLS del sector) a través de subsea entre Bilbao, España y Virginia Beach, Virginia, EE. UU., así como el [AEC](https://www.submarinecablemap.com/#/submarine-cable/aeconnect-1)entre Nueva York, Estados Unidos y Dublín, Irlanda y [New Cross del Pacífico (NCP)](https://www.submarinecablemap.com/#/submarine-cable/new-cross-pacific-ncp-cable-system) entre Tokio, Japón y Portland, Oregon, Estados Unidos. 
 

## <a name="our-network-is-your-network"></a>Nuestra red es la red

Hemos creado dos décadas de experiencia, junto con grandes inversiones en la red garantizar un rendimiento óptimo en todo momento. Las empresas pueden aprovechar al máximo nuestros recursos de red y crear arquitecturas de superposición avanzadas en la parte superior. 
 
Microsoft Azure ofrece la mayor cartera de servicios y capacidades, lo que permite a los clientes crear rápida y fácilmente, expanda y cumplan requisitos de red en cualquier lugar. Nuestra familia de servicios de conectividad abarcar el emparejamiento de redes virtuales entre regiones, híbridas y en la nube de punto a sitio y escenarios de tránsito de arquitecturas de sitio a sitio, así como IP global.  Para las empresas que desean conectarse a su propio centro de datos o la red en Azure o los clientes con necesidades de tránsito, o la ingesta de datos masivos [ExpressRoute](../expressroute/expressroute-introduction.md), y [ExpressRoute Direct](../expressroute/expressroute-erdirect-about.md) proporcionan opciones hasta 100 Gbps de ancho de banda, directamente en la red global de Microsoft en ubicaciones de emparejamiento en todo el mundo.  
 
[Alcance Global de ExpressRoute](../expressroute/expressroute-global-reach.md) está diseñado para complementar la implementación de WAN de su proveedor de servicios y conectar sus sitios locales en todo el mundo. Si ejecuta una operación global, se puede usar alcance Global de ExpressRoute junto con sus proveedores de servicios preferido y local para conectarse a todos los sitios globales con la red global de Microsoft. Expansión de la nueva red en la nube (WAN) para abarcar el gran número de sitios de sucursal puede realizarse a través de Azure red WAN Virtual, que ofrece la posibilidad de conectarse sin problemas sus ramas a red global de Microsoft con dispositivos SDWAN & VPN (es decir, Equipamiento para instalaciones de cliente o CPE) con integrados facilidad de uso y la administración de conectividad y la configuración automatizada. 
 
[Emparejamiento de VNet global](../virtual-network/virtual-network-peering-overview.md) permite a los clientes conectarse sin problemas dos o más redes virtuales de Azure entre regiones. Una vez emparejadas, las redes virtuales aparecen como una sola. El tráfico entre máquinas virtuales en las redes virtuales emparejadas se enruta a través de la infraestructura de red troncal de Microsoft, gran parte como el tráfico se enruta entre máquinas virtuales en la misma red virtual: a través de direcciones IP privadas solo. 
 

## <a name="well-managed-using-software-defined-innovation"></a>Administrar correctamente con innovación definidas por software

Ejecuta una de las nubes líderes en el mundo, Microsoft ha obtenido mucha información y experiencia en [crear y administrar](https://myignite.techcommunity.microsoft.com/sessions/66668) infraestructura global de alto rendimiento.  
 
Se debe seguir un sólido conjunto de principios operativos: 
 
- Usar hardware de conmutación de mejor entre los distintos niveles de la red.  
- Implementar nuevas características sin tener impacto en los usuarios finales.  
- Implementar actualizaciones de forma segura y confiable a través de la flota, tan rápido como sea posible. Horas en lugar de semanas.  
- Usar deep telemetría de escala de nube y automatizar completamente la mitigación de error.  
- Usar tecnología de red unificada y definidas por software para controlar todos los elementos de hardware de la red.  Eliminar la duplicación y reducir los errores. 
 
Estos principios se aplican a todas las capas de la red: desde el host de la interfaz de red, el cambio de plataforma, las funciones en el centro de datos, como los equilibradores de carga, hasta llegar a la red WAN con nuestra plataforma de ingeniería de tráfico y nuestras redes ópticos de red.  
 
El crecimiento exponencial de Azure y su red ha alcanzado un punto donde finalmente comprendimos que la intuición humana podría ya no se puede confiar en administrar las operaciones de red global. Para satisfacer la necesidad de validar long, medio y cambios a corto plazo en la red, hemos desarrollado una plataforma para crear el reflejo y emular sintética nuestra red de producción. La capacidad de crear entornos reflejados y ejecutar millones de simulaciones, nos permite probar los cambios de hardware, software y su impacto, antes de confirmarlos en nuestra plataforma de producción y la red. 

## <a name="next-steps"></a>Pasos siguientes
- [Más información sobre los servicios de red proporcionados en Azure](https://azure.microsoft.com/product-categories/networking/)
