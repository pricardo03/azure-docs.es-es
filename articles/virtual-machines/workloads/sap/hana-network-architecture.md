---
title: Arquitectura de red de SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Arquitectura de red para implementar SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 724a91b6ba0be030a2281bce366e4378892df59b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835813"
---
# <a name="sap-hana-large-instances-network-architecture"></a>Arquitectura de red de SAP HANA (instancias grandes)

La arquitectura del servicio de red de Azure es un componente clave para implementar las aplicaciones de SAP de forma correcta en HANA (instancias grandes). Por lo general, las implementaciones de SAP HANA en Azure (Instancias grandes) tienen un entorno de SAP más amplio con varias soluciones SAP distintas y diversos tamaños de base de datos, consumo de recursos de CPU y utilización de memoria. Es probable que no todos los sistemas SAP estén basados en SAP HANA. La infraestructura de SAP probablemente es un híbrido que usa:

- Sistemas SAP implementados localmente. Debido a sus tamaños, estos sistemas actualmente no se pueden hospedar en Azure. Un ejemplo es un sistema SAP ERP de producción que se ejecuta en SQL Server (como base de datos) y requiere más recursos de CPU o memoria de los que pueden proporcionar las máquinas virtuales.
- Sistemas SAP basados en SAP HANA implementados localmente.
- Sistemas SAP implementados en máquinas virtuales. Estos sistemas pueden ser instancias de desarrollo, pruebas, espacio aislado o producción para cualquiera de las aplicaciones basadas en SAP NetWeaver que se pueden implementar correctamente en Azure (en máquinas virtuales), en función de la demanda de memoria y el consumo de recursos. Estos sistemas también pueden basarse en bases de datos como SQL Server. Para obtener más información, consulte [SAP soporte Nota 1928533 – aplicaciones SAP en Azure: Tipos de máquina virtual de Azure y productos admitidos](https://launchpad.support.sap.com/#/notes/1928533/E). Y estos sistemas también pueden basarse en bases de datos como SAP HANA. Para más información, consulte [Plataformas de IaaS certificadas para SAP HANA](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).
- Servidores de aplicaciones de SAP implementados en Azure (en máquinas virtuales) que aprovechan SAP HANA en Azure (instancias grandes) en demarcaciones de instancias grandes de Azure.

Es normal una infraestructura de SAP con cuatro o más escenarios de implementación diferentes. Hay también muchos casos de cliente de infraestructuras de SAP completas que se ejecutan en Azure. A medida que las máquinas virtuales se vuelven más eficaces, aumenta el número de clientes que trasladan todas sus soluciones SAP a Azure.

Las redes de Azure en el contexto de los sistemas SAP implementados en Azure no son complicadas. Se basan en los siguientes principios:

- Las redes virtuales de Azure deben estar conectadas al circuito ExpressRoute que se conecta a la red local.
- Normalmente, un circuito ExpressRoute que conecta local y Azure necesitaría un ancho de banda de 1 Gbps o más. Este ancho de banda mínimo permite suficiente ancho de banda para la transferencia de datos entre los sistemas locales y los sistemas que se ejecutan en máquinas virtuales. También permite un ancho de banda adecuado para la conexión de los usuarios locales a los sistemas de Azure.
- Todos los sistemas SAP en Azure deben configurarse en redes virtuales para poder comunicarse entre sí.
- Active Directory y DNS hospedados localmente se extienden a Azure a través de ExpressRoute desde un entorno local.


> [!NOTE] 
> Desde un punto de vista facturación, solo se puede vincular una suscripción de Azure a un único inquilino en una demarcación de instancias grandes de una región de Azure específica. A la inversa, se puede vincular solo un inquilino de la demarcación de instancias grandes a una única suscripción de Azure. Este requisito es coherente con otros objetos facturables en Azure.

Si se implementa SAP HANA en Azure (instancias grandes) en varias regiones de Azure diferentes, se implementará un inquilino diferente en la demarcación de instancias grandes. Puede ejecutar las dos en la misma suscripción de Azure, siempre que estas instancias formen parte de la misma infraestructura de SAP. 

> [!IMPORTANT] 
> La única implementación compatible con SAP HANA en Azure (instancias grandes) es la de Azure Resource Manager.

 

## <a name="additional-virtual-network-information"></a>Información adicional sobre la red virtual

Para conectar una red virtual a ExpressRoute, se debe crear una puerta de enlace de Azure. Para más información, consulte [Puertas de enlace de red virtual para ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Se puede usar una puerta de enlace de Azure con ExpressRoute para una infraestructura fuera de Azure o para una demarcación de instancias grandes de Azure. Una puerta de enlace de Azure también se puede usar para la conexión entre redes virtuales. Para más información, consulte [Configuración de una conexión de red a red para el Administrador de recursos mediante PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Puede conectar la puerta de enlace de Azure a un máximo de cuatro conexiones de ExpressRoute diferentes, siempre que procedan de diferentes enrutadores perimetrales empresariales de Microsoft. Para más información, consulte [Infraestructura y conectividad con SAP HANA en Azure (instancias grandes)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> El rendimiento proporcionado por una puerta de enlace de Azure es distinto para ambos casos de uso. Para más información, consulte [Acerca de VPN Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). El rendimiento máximo que se puede lograr con una puerta de enlace de red virtual es de 10 Gbps, mediante una conexión de ExpressRoute. La copia de archivos entre una máquina virtual que resida en una red virtual y un sistema local (como una sola secuencia de copia) no consigue el rendimiento total de las diferentes SKU de puerta de enlace. Para aprovechar el ancho de banda completo de la puerta de enlace de red virtual, use varias secuencias. O bien, debe copiar archivos diferentes en secuencias paralelas de un único archivo.


## <a name="networking-architecture-for-hana-large-instance"></a>Arquitectura de redes de HANA (instancias grandes)
La arquitectura de redes de HANA (instancias grandes) se puede dividir en cuatro partes distintas:

- Redes locales y conexión de ExpressRoute a Azure. Esta parte es el dominio de los clientes y se conecta a Azure a través de ExpressRoute. Consulte la parte inferior derecha en la ilustración siguiente.
- Servicios de red de Azure, como se explicó anteriormente con las redes virtuales, que tienen de nuevo puertas de enlace. Esta parte es un área en la que debe buscar los diseños adecuados para los requisitos de las aplicaciones, de seguridad y de cumplimiento. Si usa HANA (instancias grandes) es otro punto a tener en cuenta en cuanto al número de redes virtuales y de SKU de puerta de enlace de Azure entre los que elegir. Observe la parte superior derecha de la ilustración.
- Conectividad de HANA (instancias grandes) a través de la tecnología de ExpressRoute en Azure. Microsoft implementa y controla esta parte. Todo lo que necesita hacer es proporcionar algunos intervalos de direcciones IP después de la implementación de los recursos de HANA (instancias grandes) que conectan el circuito ExpressRoute a las redes virtuales. Para más información, consulte [Infraestructura y conectividad con SAP HANA en Azure (instancias grandes)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- Redes en HANA (instancias grandes), las cuales son principalmente transparentes para usted.

![Red virtual conectada a SAP HANA en Azure (instancias grandes) y localmente](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

El hecho de que use HANA (instancias grandes) no cambia el requisito de tener que conectar los recursos locales a Azure a través de ExpressRoute. No cambia tampoco el requisito de tener una o varias redes virtuales que ejecutan las máquinas virtuales que hospedan el nivel de aplicación que se conecta a las instancias de HANA que se hospedan en unidades de HANA (instancias grandes). 

Las diferencias en las implementaciones de SAP en Azure son:

- Las unidades de HANA (instancias grandes) del inquilino del cliente se conectan a las redes virtuales través de otro circuito ExpressRoute. Con el fin de separar las condiciones de carga, los vínculos de ExpressRoute del entorno local a las redes virtuales y los vínculos entre las redes virtuales y HANA (instancias grandes) no comparten los mismos enrutadores.
- La naturaleza del perfil de carga de trabajo entre el nivel de aplicación de SAP y HANA (instancias grandes) es diferente y tiene muchas solicitudes pequeñas y transferencias de datos como ráfagas (conjuntos de resultados) desde SAP HANA al nivel de aplicación.
- La arquitectura de aplicación de SAP es más sensible a la latencia de la red que los escenarios típicos donde los datos se intercambian entre el entorno local y Azure.
- La puerta de enlace de red virtual tiene al menos dos conexiones de ExpressRoute. Las dos conexiones comparten el ancho de banda máximo para los datos entrantes de la puerta de enlace de red virtual.

La latencia de red que se experimenta entre las máquinas virtuales y las unidades de HANA (instancias grandes) puede ser mayor que una latencia de red de recorrido de ida y vuelta entre máquinas virtuales. Depende de la región de Azure, los valores medidos pueden superar la latencia de ida y vuelta de 0,7 ms clasificada como por debajo del promedio en [SAP nota Note #1100926: preguntas más frecuentes: Rendimiento de la red](https://launchpad.support.sap.com/#/notes/1100926/E). Depende de la región de Azure y de la herramienta para medir la latencia de ida y vuelta de la red entre una máquina virtual de Azure y la unidad de instancias grandes de HANA; la latencia medida puede ser de hasta 2 milisegundos. Sin embargo, los clientes han implementado correctamente aplicaciones de SAP de producción basadas en SAP HANA en SAP HANA (instancias grandes). Asegúrese de probar exhaustivamente los procesos empresariales en HANA en Azure (instancias grandes).
 
Para proporcionar latencia de red determinista entre las máquinas virtuales y HANA (instancias grandes), la elección de la SKU de la puerta de enlace de red virtual es esencial. A diferencia de los patrones de tráfico entre el entorno local y las máquinas virtuales, el patrón de tráfico entre las máquinas virtuales y HANA (instancias grandes) puede generar pequeñas pero elevadas ráfagas de solicitudes y volúmenes de datos para transmitir. Para poder controlar correctamente esas ráfagas, se recomienda el uso de la SKU de puerta de enlace UltraPerformance. Para las SKU de clase de tipo II de HANA (instancias grandes), es obligatorio el uso de la SKU de puerta de enlace UltraPerformance como puerta de enlace de red virtual.

> [!IMPORTANT] 
> Dado el tráfico de red general entre la capa de base de datos y la de la aplicación de SAP, solo se admiten las SKU de puerta de enlace HighPerformance o UltraPerformance para redes virtuales para la conexión a SAP HANA en Azure (instancias grandes). Para las SKU de la clase de tipo II de HANA (instancias grandes), solo se admite la SKU de puerta de enlace UltraPerformance como puerta de enlace de red virtual.



## <a name="single-sap-system"></a>Único sistema SAP

La infraestructura local mostrada anteriormente se conecta a través de ExpressRoute con Azure. El circuito ExpressRoute se conecta a un enrutador perimetral de empresa. Para más información, consulte [Información técnica sobre ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Una vez establecida, esa ruta se conecta a la red troncal de Azure y son accesibles todas las regiones de Azure.

> [!NOTE] 
> Para ejecutar infraestructuras de SAP en Azure, conéctese al enrutador perimetral de empresa más cercano a la región de Azure en la infraestructura de SAP. Las demarcaciones de instancias grandes de Azure están conectadas a través de dispositivos enrutadores perimetrales de empresa dedicados para minimizar la latencia de red entre las máquinas virtuales de IaaS de Azure y las demarcaciones de grandes instancias.

La puerta de enlace de red virtual de las máquinas virtuales que hospedan instancias de la aplicación de SAP está conectada al circuito ExpressRoute. La misma red virtual está conectada a un enrutador perimetral de empresa independiente dedicado para la conexión con las demarcaciones de instancias grandes.

Este sistema es un ejemplo sencillo de un único sistema SAP. El nivel de aplicación de SAP se hospeda en Azure. La base de datos de SAP HANA se ejecuta en SAP HANA en Azure (instancias grandes). Se supone que el ancho de banda de la puerta de enlace de red virtual de 2 Gbps o 10 Gbps de rendimiento no representa un cuello de botella.

## <a name="multiple-sap-systems-or-large-sap-systems"></a>Varios sistemas SAP o sistemas SAP grandes

Si se implementan varios sistemas SAP o sistemas SAP grandes para su conexión con SAP HANA en Azure (instancias grandes), el rendimiento de la puerta de enlace de red virtual podría convertirse en un cuello de botella. En ese caso, debe dividir los niveles de aplicación en varias redes virtuales. También puede crear una red virtual especial que se conecta a HANA (instancias grandes) para casos tales como:

- Realizar copias de seguridad directamente desde las instancias de HANA en HANA (instancias grandes) a una máquina virtual de Azure que hospeda los recursos compartidos de NFS.
- Copiar las copias de seguridad de gran tamaño u otros archivos de unidades de instancias grandes de HANA en espacio en disco administrado en Azure.

Usar una red virtual independiente para hospedar las máquinas virtuales que administran el almacenamiento. Esta disposición evita los efectos de las grandes transferencias de archivos o datos desde HANA (instancias grandes) a Azure sobre la puerta de enlace de red virtual que sirve las máquinas virtuales que ejecutan el nivel de aplicación de SAP. 

Para una arquitectura de red más escalable:

- Aproveche varias redes virtuales para un solo nivel de aplicación de SAP más grande.
- Implemente una red virtual distinta para cada sistema SAP implementado, en comparación con la combinación de estos sistemas SAP en subredes independientes en la misma red virtual.

  Una arquitectura de red más escalable para SAP HANA en Azure (Instancias grandes):

![Implementación del nivel de aplicación de SAP en varias redes virtuales](./media/hana-overview-architecture/image4-networking-architecture.png)

La ilustración muestra el nivel de aplicación de SAP y los componentes, que se implementan a través de varias redes virtuales. Esta configuración presenta una sobrecarga de latencia inevitable que se produce durante la comunicación entre las aplicaciones hospedadas en esas redes virtuales. De manera predeterminada, el tráfico de red entre máquinas virtuales ubicadas en diferentes redes virtuales discurrirá a través de enrutadores perimetrales de empresa en esta configuración. La manera de optimizar y reducir la latencia en la comunicación entre dos redes virtuales pasa por el emparejamiento de las redes virtuales que estén en la misma región. Este método funciona incluso si esas redes virtuales están en distintas suscripciones. Gracias al emparejamiento de redes virtuales, la comunicación entre máquinas virtuales en dos redes virtuales diferentes puede emplear la red troncal de Azure para permitir una comunicación directa entre ambas. La latencia se muestra como si las máquinas virtuales estuvieran en la misma red virtual. El tráfico que se dirige a los intervalos de direcciones IP que están conectados a través de la puerta de enlace de red virtual se enruta a través de la puerta de enlace de red virtual individual de dicha red. 

Para más información sobre el emparejamiento de redes virtuales, consulte [Emparejamiento de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


## <a name="routing-in-azure"></a>Enrutamiento en Azure

Hay tres consideraciones importantes sobre el enrutamiento de red para SAP HANA en Azure (instancias grandes):

* Las máquinas virtuales solo pueden acceder a SAP HANA en Azure (instancias grandes) en la conexión ExpressRoute dedicada, no directamente desde el entorno local. El acceso directo desde local a las unidades de HANA (instancias grandes), tal como lo entrega Microsoft, no es posible inmediatamente. Las restricciones de enrutamientos transitivas son debidas a la arquitectura de red de Azure actual utilizada para SAP HANA (instancias grandes). Los clientes de administración y las aplicaciones que necesiten acceso directo, como SAP Solution Manager ejecutado de forma local, no se pueden conectar a la base de datos de SAP HANA.

* Si tiene unidades de HANA (instancias grandes) implementadas en dos regiones de Azure distintas para la recuperación ante desastres, se aplican las mismas restricciones en el enrutamiento transitorio. En otras palabras, las direcciones IP de una unidad de HANA (instancias grandes) en una región (por ejemplo, Oeste de EE. UU.) no se enrutarán a una unidad de HANA (instancias grandes) implementada en otra región (por ejemplo, Este de EE. UU.). Esta restricción es independiente del uso del emparejamiento de redes de Azure entre regiones o de los circuitos ExpressRoute que conectan las unidades de HANA (instancias grandes) con las redes virtuales. Para ver una representación gráfica, consulte la ilustración de la sección "Uso de unidades de HANA (instancias grandes) en varias regiones". Esta restricción, que se incluye con la arquitectura implementada, prohíbe el uso inmediato de la replicación del sistema de HANA como funcionalidad de recuperación ante desastres.

* Las unidades de SAP HANA en Azure (instancias grandes) tienen una dirección IP asignada desde el intervalo de direcciones del grupo IP de servidor que ha enviado. Para más información, consulte [Infraestructura y conectividad con SAP HANA en Azure (instancias grandes)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Se puede acceder a esta dirección IP a través de las suscripciones de Azure y de la conexión ExpressRoute que conecta las redes virtuales a HANA en Azure (instancias grandes). La dirección IP asignada fuera del intervalo de direcciones del grupo IP de servidor se asigna directamente a la unidad de hardware. Ya *no* se asigna mediante NAT, como sucedía en las primeras implementaciones de esta solución. 

> [!NOTE]
> Para superar la limitación del enrutamiento transitorio como se explicó en los dos primeros elementos de la lista anterior, debe utilizar componentes adicionales para el enrutamiento. Los componentes que se pueden usar para superar la restricción pueden ser:
> 
> * Un servidor proxy inverso para enrutar los datos, desde y hacia. Por ejemplo, F5 BIG-IP, NGINX con Traffic Manager implementado en Azure como solución de enrutamiento de tráfico y firewall virtual.
> * Usar [reglas de IPTables](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) en una máquina virtual Linux para habilitar el enrutamiento entre las ubicaciones locales y las unidades de HANA (instancias grandes) o entre unidades de HANA (instancias grandes) en regiones diferentes.
> 
> Tenga en cuenta que Microsoft no proporciona la implementación y soporte técnico para las soluciones personalizadas que tienen dispositivos de red de terceros o IPTables. El proveedor del componente utilizado o el integrador es el que debe proporcionar el soporte técnico. 

## <a name="internet-connectivity-of-hana-large-instance"></a>Conectividad a Internet de HANA (instancias grandes)
HANA (instancias grandes) *NO* tiene conectividad directa a Internet. Por ejemplo, esta limitación puede restringir la capacidad de registrar la imagen del sistema operativo directamente con el proveedor del sistema operativo. Debe trabajar con la herramienta de administración de suscripciones de SUSE Linux Enterprise Server local o con el administrador de suscripciones de Red Hat Enterprise Linux.

## <a name="data-encryption-between-vms-and-hana-large-instance"></a>Cifrado de datos entre las máquinas virtuales y HANA (instancias grandes)
Los datos transferidos entre HANA (instancias grandes) y las máquinas virtuales no se cifran. Sin embargo, solo para el intercambio entre el DBMS de HANA y las aplicaciones basadas en JDBC y ODBC, puede habilitar el cifrado del tráfico. Para más información, consulte [esta documentación de SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

## <a name="use-hana-large-instance-units-in-multiple-regions"></a>Uso de unidades de HANA (instancias grandes) en varias regiones

Pueden existir razones para implementar SAP HANA en Azure (instancias grandes) en varias regiones de Azure además de la recuperación ante desastres. Es posible que desee acceder a HANA (instancias grandes) desde cada una de las máquinas virtuales implementadas en las diferentes redes virtuales en las regiones. Las direcciones IP asignadas a las diferentes unidades de HANA (instancias grandes) no se propagan más allá de las redes virtuales que están conectadas directamente a través de su puerta de enlace a las instancias. Como resultado, se presenta un pequeño cambio en el diseño de la red virtual. Una puerta de enlace de red virtual puede controlar cuatro circuitos ExpressRoute diferentes fuera de los diferentes enrutadores perimetrales empresariales. Cada red virtual que está conectada a una de las demarcaciones de instancias grandes puede estar conectada a la demarcación de instancias grandes en otra región de Azure.

![Redes virtuales conectadas a demarcaciones de instancias grandes de Azure en diferentes regiones de Azure](./media/hana-overview-architecture/image8-multiple-regions.png)

En la ilustración se muestra cómo las distintas redes virtuales en ambas regiones están conectadas a dos circuitos ExpressRoute diferentes que se utilizan para conectarse a SAP HANA en Azure (instancias grandes) en ambas regiones de Azure. Las conexiones recién introducidas son las líneas rojas rectangulares. Con estas conexiones, fuera de las redes virtuales, las máquinas virtuales que se ejecutan en una de esas redes virtuales pueden tener acceso a cada una de las diferentes unidades de HANA (instancias grandes) implementadas en las dos regiones. Como se muestra en la ilustración, se supone que tiene dos conexiones ExpressRoute desde local a las dos regiones de Azure. Esta disposición se recomienda por motivos de recuperación ante desastres.

> [!IMPORTANT] 
> Si se usan varios circuitos ExpressRoute, deben utilizarse las configuraciones de anteposición de AS Path y de preferencia local de BGP para garantizar el enrutamiento adecuado del tráfico.

**Pasos siguientes**
- Consulte [SAP HANA (Large Instances) storage architecture](hana-storage-architecture.md) (Arquitectura de almacenamiento de SAP HANA [instancias grandes]).