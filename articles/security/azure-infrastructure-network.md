---
title: Arquitectura de red de Azure
description: En este artículo se proporciona una descripción general de la red de infraestructura de Microsoft Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 67781f196b445c9330e0dcb1fc7d8b0a1a53cbc0
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101690"
---
# <a name="azure-network-architecture"></a>Arquitectura de red de Azure
La arquitectura de red de Azure sigue una versión modificada del modelo núcleo/distribución/acceso estándar del sector, con distintas capas de hardware. Las capas incluyen:

- Núcleo (enrutadores de centros de datos)
- Distribución (enrutadores de acceso y agregación de L2): la capa de distribución separa el enrutamiento L3 de la conmutación L2
- Acceso (conmutadores de host L2)

La arquitectura de red tiene dos niveles de conmutadores de capa 2, una capa de agregación de tráfico de la otra y bucles de capa 2 para incorporar la redundancia. Esto proporciona ventajas, como una superficie de VLAN más flexible, y mejora el escalado de puertos. La arquitectura mantiene la distinción entre L2 y L3, lo que permite usar hardware en cada una de las distintas capas de la red y minimiza que los errores en una capa afecten a las otras capas. El uso de troncos permite compartir recursos, como la conectividad con la infraestructura de L3.

## <a name="network-configuration"></a>Network configuration (Configuración de red)
La arquitectura de red de un clúster de Microsoft Azure en un centro de datos consta de los siguientes dispositivos:

- Enrutadores (centro de datos, enrutador de acceso y enrutadores de hoja de borde)
- Conmutadores (conmutadores de agregación y parte superior del bastidor)
- Digi CM
- PDU o Nucleons

La ilustración siguiente proporciona una descripción general de la arquitectura de red de Azure dentro de un clúster. Azure tiene dos arquitecturas diferentes. Algunos clientes y servicios compartidos existentes de Azure residen en la arquitectura LAN predeterminada (DLA), mientras que a las nuevas regiones y clientes virtuales se accederá a través de la arquitectura de Quantum 10 (Q10). La arquitectura DLA es un diseño de árbol tradicional con enrutadores de acceso activo-pasivo y ACL de seguridad aplicadas en los enrutadores de acceso. La arquitectura de Quantum 10 tiene un diseño de CLOS/malla de enrutadores, donde las ACL no se aplican en los enrutadores, sino debajo de la ruta a través de equilibrio de carga de software (SLB) o VLAN definidas por software.

![Red de Azure][1]

### <a name="quantum-10-devices"></a>Dispositivos Quantum 10
En el diseño de Quantum 10, la conmutación de capa 3 se extiende a través de varios dispositivos en un diseño CLOS/mesh. Entre las ventajas del diseño Q10 se incluyen una mayor capacidad y una mayor posibilidad de escalar la infraestructura de red existente. El diseño emplea enrutadores de hoja de borde, conmutadores de tallo y enrutadores de la parte superior del bastidor para pasar el tráfico a los clústeres a través de varias rutas, lo que permite una tolerancia a errores. Los servicios de seguridad, como la traducción de direcciones de red, se controlan a través de equilibrio de carga de software, en lugar de dispositivos de hardware.

### <a name="access-routers"></a>Enrutadores de acceso
Los enrutadores de distribución/acceso L3 (AR) realizan la funcionalidad de enrutamiento principal para las capas de distribución y acceso. Estos dispositivos se implementan en pareja, y son la puerta de enlace predeterminada para las subredes. Cada par de AR puede admitir varios pares de conmutador de agregación de L2, según la capacidad. El número máximo depende de la capacidad del dispositivo, así como de los dominios de error. Un número típico basado en la capacidad esperada serían tres pares de conmutadores de agregación L2 por cada par de AR.

### <a name="l2-aggregation-switches"></a>Conmutadores de agregación L2  
Estos dispositivos actúan como un punto de agregación para el tráfico de L2. Son la capa de distribución para el tejido de L2 y pueden controlar una gran cantidad de tráfico. Dado que estos dispositivos agregan el tráfico, se requieren la funcionalidad 802.1q y tecnologías de alto ancho de banda, como la agregación de puertos y 10GE.

### <a name="l2-host-switches"></a>Conmutadores de host L2
Los hosts se conectan directamente a estos conmutadores. Pueden ser conmutadores montados en bastidor o implementaciones de chasis. El estándar 802.1q permite la designación de una VLAN como VLAN nativa, por lo que esa VLAN se trata como una trama de Ethernet normal (sin etiquetar). En circunstancias normales, las tramas en la VLAN nativa se transmiten y reciben sin etiquetar en un puerto de enlace 802.1q. Esta característica se diseñó para la migración a 802.1q y compatibilidad con dispositivos no aptos para 802.1q. En esta arquitectura, solo la infraestructura de red utiliza la VLAN nativa.

Esta arquitectura especifica un estándar para la selección de VLAN nativa que garantiza, siempre que sea posible, que los dispositivos de AR tengan una VLAN nativa única para cada tronco y los troncos de L2Aggregation a L2Aggregation. Los troncos del conmutador de L2Aggregation a L2Host tienen una VLAN nativa no predeterminada.

### <a name="link-aggregation-8023ad"></a>Agregación de vínculos (802.3ad)
La agregación de vínculos (LAG) permite que varios vínculos individuales se agrupen y se traten como un único vínculo lógico. El número que se usa para designar las interfaces de puerto-canal debe estar estandarizado para facilita depuración operativa, el resto de la red utilizará el mismo número en ambos extremos de un puerto-canal. Esto requiere un estándar para determinar qué extremo del puerto-canal se utiliza para definir el siguiente número disponible.

Los números especificados para el conmutador de L2Agg a L2Host son los números de puerto-canal utilizados en el lado L2Agg. Dado que el rango de números es más limitado en el lado L2Host, el estándar es usar los números 1 y 2 en el lado L2Host para hacer referencia al puerto-canal que va a la parte "a" y la "b", respectivamente.

### <a name="vlans"></a>VLAN
La arquitectura de red utiliza redes VLAN para agrupar servidores en un único dominio de difusión. Los números de VLAN cumplen con los estándares 802.1q, lo que admite VLAN con numeración de 1 a 4094.

### <a name="customer-vlans"></a>VLAN de clientes
Los clientes tienen varias opciones de implementación de VLAN que pueden implementar a través de Azure Portal para satisfacer las necesidades de separación y arquitectura de su solución. Estas soluciones se implementan a través de máquinas virtuales. Para obtener ejemplos de arquitectura de referencia de clientes, visite [Arquitecturas de referencia de Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/).

### <a name="edge-architecture"></a>Arquitectura de borde
Los centros de datos de Azure se basan en infraestructuras de red altamente redundantes y bien aprovisionadas. Las redes en los centros de datos de Azure se implementan con arquitecturas de redundancia de "necesidad más uno" (N+1) o superior. Las características de conmutación por error completas dentro de los centros de datos y entre estos ayudan a garantizar la disponibilidad de las redes y de los servicios. Externamente, los centros de datos funcionan con circuitos dedicados de red de alto ancho de banda que conectan de manera redundante las propiedades con más de 1200 proveedores de servicios de Internet globalmente en varios puntos de emparejamiento que proporcionan más de 2000 Gbps de capacidad potencial de borde a lo largo de la red.

Los enrutadores de filtrado en el borde y la capa de acceso de la red de Microsoft Azure proporcionan seguridad bien establecida a nivel de paquete para evitar intentos no autorizados de conectarse a Azure. Ayudan a asegurar que el contenido real de los paquetes contenga datos en el formato esperado y se ajusten al esquema de comunicación de cliente/servidor esperado. Azure implementa una arquitectura en capas que consta de los siguientes componentes de segregación de redes y de control de acceso:

- Enrutadores perimetrales: separan el entorno de aplicaciones de Internet. Los enrutadores perimetrales están diseñados para proporcionar protección contra suplantación de identidad y limitar el acceso mediante listas de control de acceso (ACL).
- Enrutadores de distribución (acceso): los enrutadores de acceso están diseñados para permitir solo las direcciones IP aprobadas por Microsoft, proporcionar protección contra suplantación de identidad y conexiones establecidas mediante ACL.

### <a name="a10-ddos-mitigation-architecture"></a>Arquitectura de mitigación de DDOS A10
Los ataques por denegación de servicio siguen siendo una amenaza real para la confiabilidad de los servicios en línea de Microsoft. A medida que los ataques son cada vez más dirigidos y sofisticados y, a medida que los servicios de Microsoft abarcan cada vez geografías más diversas, la capacidad de proporcionar mecanismos eficaces para detectar y minimizar el impacto de estos ataques es una prioridad alta.

Los detalles siguientes explican cómo se implementa el sistema de mitigación de DDOS A10 desde una perspectiva de la arquitectura de red.  
Microsoft Azure usa dispositivos de red A10 en el DCR (enrutador del centro de datos) que proporcionan detección y mitigación automáticas. La solución A10 utiliza la supervisión de red de Azure para muestrear los paquetes de flujo de red y determinar si hay un ataque. Una vez que se detecta el ataque, se usan dispositivos A10 como limpiadores para mitigar los ataques. Después de la mitigación, se permite el tráfico limpio adicional desde el DCR al centro de datos de Azure directamente. La solución A10 se usa para proteger la infraestructura de red de Azure.

Entre las protecciones contra DDoS de la solución A10 se incluyen:

- Protección contra ataques “flood” UDP IPv4 e IPv6
- Protección contra ataques “flood” ICMP IPv4 e IPv6
- Protección contra ataques “flood” TCP IPv4 e IPv6
- Protección frente a ataques SYN de TCP para IPv4 e IPv6
- Ataque de fragmentación

> [!NOTE]
> La protección contra DDoS se proporciona de forma predeterminada para todos los clientes de Azure.
>
>

## <a name="network-connection-rules"></a>Reglas de conexión de red
Azure implementa enrutadores perimetrales en su red para proporcionar seguridad a nivel de paquete con el fin de evitar intentos no autorizados de conectarse a Microsoft Azure. Aseguran que el contenido real de los paquetes contenga datos en el formato esperado y se ajusten al esquema de comunicación de cliente/servidor esperado.

Los enrutadores perimetrales separan el entorno de aplicaciones de Internet. Los enrutadores perimetrales están diseñados para proporcionar protección contra suplantación de identidad y limitar el acceso mediante listas de control de acceso (ACL). Estos enrutadores perimetrales se configuran mediante un enfoque de ACL en capas para limitar los protocolos de red que tienen permitido transitar por los enrutadores perimetrales y los enrutadores de acceso.

Los dispositivos de red se encuentran en las ubicaciones de acceso y el borde, y funcionan como puntos limítrofes donde se aplican los filtros de entrada y salida.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre lo que hace Microsoft para proteger la infraestructura de Azure, consulte:

- [Azure facilities, premises, and physical security](azure-physical-security.md) (Instalaciones de Azure, locales y seguridad física)
- [Availability of Azure infrastructure](azure-infrastructure-availability.md) (Disponibilidad de la infraestructura de Azure)
- [Componentes y límites del sistema de información de Azure](azure-infrastructure-components.md)
- [Red de producción de Azure](azure-production-network.md)
- [Características de seguridad de Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](azure-infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure](azure-infrastructure-integrity.md) (Integridad de la infraestructura de Azure)
- [Protección de datos del cliente en Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-network/network-arch.png
