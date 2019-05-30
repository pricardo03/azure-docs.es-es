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
ms.date: 05/25/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8794a93cecb50774f30746c22931db31a9fa9194
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239623"
---
# <a name="sap-hana-large-instances-network-architecture"></a>Arquitectura de red de SAP HANA (instancias grandes)

La arquitectura del servicio de red de Azure es un componente clave para implementar las aplicaciones de SAP de forma correcta en HANA (instancias grandes). Por lo general, las implementaciones de SAP HANA en Azure (Instancias grandes) tienen un entorno de SAP más amplio con varias soluciones SAP distintas y diversos tamaños de base de datos, consumo de recursos de CPU y utilización de memoria. Es probable que no todos los sistemas de TI ya se encuentran en Azure. El entorno de SAP a menudo es híbrida también desde un punto de DBMS y el punto de vista SAP aplicación con una combinación de NetWeaver y S/4HANA y SAP HANA y otros DBMS. Azure ofrece servicios diferentes que permiten ejecutar los distintos sistemas DBMS, NetWeaver y S/4HANA en Azure. Azure también ofrece que tecnología para que Azure el aspecto de la red como un centro de datos virtual para las implementaciones de software local

A menos que sus sistemas de TI completadas se hospedan en Azure. Funcionalidad de red de Azure se usa para conectar el mundo local con los recursos de Azure para que Azure el aspecto de un centro de datos virtual de la suya. La funcionalidad de red de Azure usa es: 

- Redes virtuales de Azure están conectadas a la [ExpressRoute](https://azure.microsoft.com/services/expressroute/) circuito que se conecta a los recursos de red local.
- Un circuito de ExpressRoute que conecta de forma local con Azure debe tener un ancho de banda mínimo de [1 Gbps o superior](https://azure.microsoft.com/pricing/details/expressroute/). Este ancho de banda mínimo permite suficiente ancho de banda para la transferencia de datos entre los sistemas locales y los sistemas que se ejecutan en máquinas virtuales. También permite un ancho de banda adecuado para la conexión de los usuarios locales a los sistemas de Azure.
- Todos los sistemas SAP en Azure se configuran en las redes virtuales se comuniquen entre sí.
- Active Directory y DNS hospedados localmente se extienden a Azure a través de ExpressRoute desde un entorno local, o que están ejecutando completa en Azure.

Para el caso de la integración de instancias grandes de HANA en el tejido de red del centro de datos de Azure, es también usan la tecnología de ExpressRoute de Azure


> [!NOTE] 
> Solo una suscripción de Azure se puede vincular a un único inquilino en un sello de instancia grande de HANA en una región de Azure específica. Por el contrario, un único inquilino del sello de instancia grande de HANA se puede vincular a una única suscripción de Azure. Este requisito es coherente con otros objetos facturables en Azure.

Si se implementa SAP HANA en Azure (instancias grandes) en varias regiones de Azure, un inquilino independiente se implementa en el sello de instancias grandes de HANA. Puede ejecutar las dos en la misma suscripción de Azure, siempre que estas instancias formen parte de la misma infraestructura de SAP. 

> [!IMPORTANT] 
> Solo el método de implementación de Azure Resource Manager es compatible con SAP HANA en Azure (instancias grandes).

 

## <a name="additional-virtual-network-information"></a>Información adicional sobre la red virtual

Para conectar una red virtual para ExpressRoute, se debe crear una puerta de enlace de ExpressRoute de Azure. Para obtener más información, consulte [Expressroute acerca de las puertas de enlace para ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Se utiliza una puerta de enlace de ExpressRoute de Azure con ExpressRoute hacia una infraestructura fuera de Azure o a un sello de instancias grandes de Azure. Puede conectar la puerta de enlace de ExpressRoute de Azure a un máximo de cuatro circuitos de ExpressRoute, siempre que procedan de diferentes enrutadores de perímetro empresarial de Microsoft. Para más información, consulte [Infraestructura y conectividad con SAP HANA en Azure (instancias grandes)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> El rendimiento máximo que puede lograr con una puerta de enlace de ExpressRoute es mediante el uso de una conexión de ExpressRoute de 10 Gbps. La copia de archivos entre una máquina virtual que resida en una red virtual y un sistema local (como una sola secuencia de copia) no consigue el rendimiento total de las diferentes SKU de puerta de enlace. Para aprovechar el ancho de banda completo de la puerta de enlace de ExpressRoute, use varias secuencias. O bien, debe copiar archivos diferentes en secuencias paralelas de un único archivo.


## <a name="networking-architecture-for-hana-large-instance"></a>Arquitectura de redes de HANA (instancias grandes)
La arquitectura de redes de HANA (instancias grandes) se puede dividir en cuatro partes distintas:

- Redes locales y conexión de ExpressRoute a Azure. Esta parte es el dominio de los clientes y se conecta a Azure a través de ExpressRoute. Este circuito de Expressroute está completamente pagado por el usuario como un cliente. El ancho de banda debe ser lo suficientemente grande como para controlar el tráfico de red entre los recursos locales y se conecta con la región de Azure. Consulte la parte inferior derecha en la ilustración siguiente.
- Servicios de red de Azure, como se explicó anteriormente, con las redes virtuales, lo que necesitan nuevo agregados de puertas de enlace de ExpressRoute. Esta parte es un área en la que debe buscar los diseños adecuados para los requisitos de las aplicaciones, de seguridad y de cumplimiento. Si usa HANA (instancias grandes) es otro punto a tener en cuenta en cuanto al número de redes virtuales y de SKU de puerta de enlace de Azure entre los que elegir. Observe la parte superior derecha de la ilustración.
- Conectividad de HANA (instancias grandes) a través de la tecnología de ExpressRoute en Azure. Microsoft implementa y controla esta parte. Todo lo que necesita hacer es proporcionar algunos intervalos de direcciones IP después de la implementación de los recursos de HANA (instancias grandes) que conectan el circuito ExpressRoute a las redes virtuales. Para más información, consulte [Infraestructura y conectividad con SAP HANA en Azure (instancias grandes)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). No hay ningún cargo adicional por usted como cliente para la conectividad entre el tejido de red del centro de datos de Azure y las unidades de instancia grande de HANA.
- Redes dentro de la marca de instancia grande de HANA, lo que resulta casi transparente para usted.

![Red virtual conectada a SAP HANA en Azure (instancias grandes) y localmente](./media/hana-overview-architecture/image1-architecture.png)

El hecho de que use HANA (instancias grandes) no cambia el requisito de tener que conectar los recursos locales a Azure a través de ExpressRoute. No cambia tampoco el requisito de tener una o varias redes virtuales que ejecutan las máquinas virtuales que hospedan el nivel de aplicación que se conecta a las instancias de HANA que se hospedan en unidades de HANA (instancias grandes). 

Las diferencias en las implementaciones de SAP en Azure son:

- Las unidades de HANA (instancias grandes) del inquilino del cliente se conectan a las redes virtuales través de otro circuito ExpressRoute. Para separar las condiciones de carga, la red local a circuitos de ExpressRoute de la red virtual de Azure y los circuitos entre redes virtuales de Azure e instancias grandes de HANA no comparte los mismos enrutadores.
- La naturaleza del perfil de carga de trabajo entre el nivel de aplicación de SAP y HANA (instancias grandes) es diferente y tiene muchas solicitudes pequeñas y transferencias de datos como ráfagas (conjuntos de resultados) desde SAP HANA al nivel de aplicación.
- La arquitectura de aplicación de SAP es más sensible a la latencia de la red que los escenarios típicos donde los datos se intercambian entre el entorno local y Azure.
- La puerta de enlace de ExpressRoute de Azure tiene al menos dos conexiones ExpressRoute. Un circuito que se ha conectado desde local y otro que está conectado a instancias grandes de HANA. Esto deja sólo espacio para otro dos circuitos adicionales desde msee diferentes para conectarse a la puerta de enlace de ExpressRoute. Esta restricción es independiente del uso de la ruta con Fast de ExpressRoute. Todos los circuitos conectados comparten el ancho de banda máximo para los datos entrantes de la puerta de enlace de ExpressRoute.

La latencia de red que se experimenta entre las máquinas virtuales y las unidades de HANA (instancias grandes) puede ser mayor que una latencia de red de recorrido de ida y vuelta entre máquinas virtuales. Depende de la región de Azure, los valores medidos pueden superar la latencia de ida y vuelta de 0,7 ms clasificada como por debajo del promedio en [SAP nota Note #1100926: preguntas más frecuentes: Rendimiento de la red](https://launchpad.support.sap.com/#/notes/1100926/E). Depende de la región de Azure y de la herramienta para medir la latencia de ida y vuelta de la red entre una máquina virtual de Azure y la unidad de instancias grandes de HANA; la latencia medida puede ser de hasta 2 milisegundos. Sin embargo, los clientes han implementado correctamente aplicaciones de SAP de producción basadas en SAP HANA en SAP HANA (instancias grandes). Asegúrese de probar exhaustivamente los procesos empresariales en HANA en Azure (instancias grandes). Una funcionalidad nueva, denominada ruta rápida de ExpressRoute, es capaz de reducir considerablemente la latencia de red entre instancias grandes de HANA y el nivel de aplicación de máquinas virtuales en Azure (ver abajo). 

SKU no es esencial para proporcionar latencia de red determinista entre las máquinas virtuales y de instancia grande de HANA, la elección de la puerta de enlace de ExpressRoute. A diferencia de los patrones de tráfico entre el entorno local y las máquinas virtuales, el patrón de tráfico entre las máquinas virtuales y HANA (instancias grandes) puede generar pequeñas pero elevadas ráfagas de solicitudes y volúmenes de datos para transmitir. Para poder controlar correctamente esas ráfagas, se recomienda el uso de la SKU de puerta de enlace UltraPerformance. Para la clase de tipo II de SKU de instancia grande de HANA, el uso de la SKU de puerta de enlace UltraPerformance como puerta de enlace ExpressRotue es obligatorio.

> [!IMPORTANT] 
> Dado el tráfico de red general entre la capa de base de datos y la de la aplicación de SAP, solo se admiten las SKU de puerta de enlace HighPerformance o UltraPerformance para redes virtuales para la conexión a SAP HANA en Azure (instancias grandes). Se admite solo la puerta de enlace de UltraPerformance SKU HANA grandes instancia SKU de tipo II, como una puerta de enlace de ExpressRoute. Las excepciones se aplican al usar ExpressRoute Fast Path (ver abajo)

### <a name="expressroute-fast-path"></a>Ruta de acceso rápido de ExpressRoute
Para reducir la latencia, ExpressRoute Fast Path introdujo y había publicado en mayo de 2019 para la conectividad específica de instancias grandes de HANA con redes virtuales de Azure que hospedan las máquinas virtuales de aplicación de SAP. La principal diferencia a la solución implanta hasta ahora, es que los flujos de datos entre máquinas virtuales e instancias grandes de HANA ya no se enrutan a través de la puerta de enlace de ExpressRoute. En su lugar, las máquinas virtuales asignadas en las subredes de la red virtual de Azure se comunican directamente con el enrutador perimetral de empresa dedicados. 

> [!IMPORTANT] 
> La funcionalidad de ruta con Fast de ExpressRoute requiere que las subredes que ejecutan la aplicación de SAP, las máquinas virtuales estén en la misma red virtual de Azure que tiene conectada a las instancias grandes de HANA. Máquinas virtuales ubicadas en redes virtuales de Azure que están emparejadas con la red virtual de Azure conectados directamente a las unidades de instancia grande de HANA no se benefician de ruta con Fast de ExpressRoute. Como resultado típico concentrador y radio diseño de red virtual, donde los circuitos de ExpressRoute se conectan con una red virtual de concentrador y están obteniendo emparejar redes virtuales que contiene la capa de aplicación de SAP (radios), la optimización de forma rápida de ExpressRoute Ruta de acceso no funcionará. Además, ExpressRoute Fast Path no admite reglas de enrutamiento definido por el usuario (UDR) hoy mismo. Para obtener más información, consulte [FastPath y puerta de enlace de red virtual de ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways). 


Para obtener más información sobre cómo configurar la ruta rápida de ExpressRoute, lea el documento [conectar una red virtual para instancias grandes de HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route).    

> [!NOTE]
> Una puerta de enlace de UltraPerformance ExpressRoute es necesario tener el trabajo de ruta con Fast de ExpressRoute


## <a name="single-sap-system"></a>Único sistema SAP

La infraestructura local mostrada anteriormente se conecta a través de ExpressRoute con Azure. El circuito de ExpressRoute se conecta a un enrutador perimetral de empresa de Microsoft (MSEE). Para más información, consulte [Información técnica sobre ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Una vez establecida la ruta, se conecta a la red troncal de Azure.

> [!NOTE] 
> Para ejecutar infraestructuras de SAP en Azure, conéctese al enrutador perimetral de empresa más cercano a la región de Azure en la infraestructura de SAP. Sellos de instancias grandes de HANA se conectan a través de dispositivos enrutadores perimetrales de empresa dedicados para minimizar la latencia de red entre las máquinas virtuales de IaaS de Azure y las marcas de instancia grande de HANA.

La puerta de enlace de ExpressRoute para las máquinas virtuales que hospedan las instancias de aplicación de SAP está conectada a un circuito de ExpressRoute que se conecta a locales. La misma red virtual está conectada a un enrutador perimetral de empresa independiente dedicado para la conexión con las demarcaciones de instancias grandes. Con ExpressRoute rápido ruta de acceso, el flujo de datos de instancias grandes de HANA al nivel de aplicación de SAP máquinas virtuales ya no se enrutan a través de la puerta de enlace de ExpressRoute y con la reducen la red de latencia de recorrido de ida y vuelta.

Este sistema es un ejemplo sencillo de un único sistema SAP. El nivel de aplicación de SAP se hospeda en Azure. La base de datos de SAP HANA se ejecuta en SAP HANA en Azure (instancias grandes). La suposición es que el ancho de banda de puerta de enlace de ExpressRoute de rendimiento de 2 Gbps o 10 Gbps no representa un cuello de botella.

## <a name="multiple-sap-systems-or-large-sap-systems"></a>Varios sistemas SAP o sistemas SAP grandes

Si se implementan varios sistemas SAP o sistemas SAP grandes para conectarse a SAP HANA en Azure (instancias grandes), el rendimiento de la puerta de enlace de ExpressRoute puede volverse un cuello de botella. O desea aislar la producción y los sistemas que no sea de producción en diferentes redes virtuales de Azure. En ese caso, debe dividir los niveles de aplicación en varias redes virtuales. También puede crear una red virtual especial que se conecta a HANA (instancias grandes) para casos tales como:

- Realizar copias de seguridad directamente desde las instancias de HANA en HANA (instancias grandes) a una máquina virtual de Azure que hospeda los recursos compartidos de NFS.
- Copiar las copias de seguridad de gran tamaño u otros archivos de unidades de instancias grandes de HANA en espacio en disco administrado en Azure.

Usar una red virtual independiente para hospedar las máquinas virtuales que administrar el almacenamiento para la transferencia masiva de datos entre instancias grandes de HANA y Azure. Esta disposición evita los efectos de los archivos de gran tamaño o la transferencia de datos de instancia grande de HANA en Azure en la puerta de enlace de ExpressRoute que sirve las máquinas virtuales que ejecutan el nivel de aplicación de SAP. 

Para una arquitectura de red más escalable:

- Aproveche varias redes virtuales para un solo nivel de aplicación de SAP más grande.
- Implemente una red virtual distinta para cada sistema SAP implementado, en comparación con la combinación de estos sistemas SAP en subredes independientes en la misma red virtual.

  Una arquitectura de red más escalable para SAP HANA en Azure (Instancias grandes):

![Implementación del nivel de aplicación de SAP en varias redes virtuales](./media/hana-overview-architecture/image4-networking-architecture.png)

Depende de las reglas y restricciones, que desea aplicar entre las distintas redes virtuales que hospeda máquinas virtuales de distintos sistemas SAP, se deben emparejar estas redes virtuales. Para más información sobre el emparejamiento de redes virtuales, consulte [Emparejamiento de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


## <a name="routing-in-azure"></a>Enrutamiento en Azure

Mediante la implementación predeterminada, los tres consideraciones de enrutamiento de red son importantes para SAP HANA en Azure (instancias grandes):

* SAP HANA en Azure (instancias grandes) puede tener acceso a través de máquinas virtuales de Azure y la conexión ExpressRoute dedicada, no directamente desde un entorno local. El acceso directo desde local a las unidades de HANA (instancias grandes), tal como lo entrega Microsoft, no es posible inmediatamente. Las restricciones de enrutamientos transitivas son debidas a la arquitectura de red de Azure actual utilizada para SAP HANA (instancias grandes). Los clientes de administración y las aplicaciones que necesiten acceso directo, como SAP Solution Manager ejecutado de forma local, no se pueden conectar a la base de datos de SAP HANA. Para las excepciones, compruebe la sección 'Enrutamiento directo a HANA instancias grandes'.

* Si tiene unidades de instancia grande de HANA implementadas en dos regiones de Azure para la recuperación ante desastres, las mismas restricciones de enrutamientos transitorio aplicadas en el pasado. En otras palabras, las direcciones IP de una unidad de instancia grande de HANA en una región (por ejemplo, oeste de EE) no se enrutan a una unidad de instancia grande de HANA implementada en otra región (por ejemplo, EE.UU). Esta restricción era independiente del uso de red de Azure emparejamiento entre regiones o de los circuitos ExpressRoute que conectan las unidades de instancia grande de HANA con las redes virtuales. Para ver una representación gráfica, consulte la ilustración de la sección "Uso de unidades de HANA (instancias grandes) en varias regiones". Esta restricción, que incluía la arquitectura implementada, prohíbe el uso inmediato de replicación del sistema de HANA como funcionalidad de recuperación ante desastres. Para los cambios recientes, consulte la sección "Unidades de instancia grande de HANA de uso en varias regiones". 

* SAP HANA en unidades de Azure (instancias grandes) tienen una dirección IP asignada desde servidor grupo intervalo de direcciones IP que haya enviado cuando se solicita la implementación de instancia grande de HANA. Para más información, consulte [Infraestructura y conectividad con SAP HANA en Azure (instancias grandes)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Esta dirección IP es accesible a través de las suscripciones de Azure y el circuito que conecta las redes virtuales de Azure a HANA instancias grandes. La dirección IP asignada fuera del intervalo de direcciones del grupo IP de servidor se asigna directamente a la unidad de hardware. Ya *no* se asigna mediante NAT, como sucedía en las primeras implementaciones de esta solución. 

### <a name="direct-routing-to-hana-large-instances"></a>Enrutamiento a HANA instancias grandes directo
De forma predeterminada no funciona el enrutamiento transitivo entre las unidades de instancia grande de HANA y de forma local o entre instancias grandes de HANA de enrutamiento que se implementan en dos regiones diferentes. Existen varias posibilidades para habilitar este tipo un enrutamiento transitivo.

- Un servidor proxy inverso para enrutar los datos, desde y hacia. Por ejemplo, F5 BIG-IP, NGINX con Traffic Manager implementado en Azure virtual network que se conecta a HANA instancias grandes y a nivel local como una solución de enrutamiento de tráfico y firewall virtual.
- Usar [reglas de IPTables](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) en una máquina virtual Linux para habilitar el enrutamiento entre las ubicaciones locales y las unidades de HANA (instancias grandes) o entre unidades de HANA (instancias grandes) en regiones diferentes. La máquina virtual ejecuta IPTables debe implementarse en la red virtual de Azure que se conecta a HANA instancias grandes y a nivel local. La máquina virtual debe ser ajustarse según corresponda, lo, que el rendimiento de la red de la máquina virtual es suficiente para el tráfico de red esperado. Para obtener más información sobre la máquina virtual de red de ancho de banda, consulte el artículo [tamaños de máquinas virtuales en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json).
- [Firewall de Azure](https://azure.microsoft.com/services/azure-firewall/) sería otra solución para permitir el tráfico directo entre local y en unidades de instancia grande de HANA. 

Todo el tráfico de estas soluciones se enrutaría a través de una red virtual de Azure y por lo tanto, el tráfico podría ser más limitado por los dispositivos de software usa o por Azure Network Security Group, por lo tanto, determinadas direcciones IP o la dirección IP comprendido entre local se podría bloqueado o había permitido explícitamente el acceso a las instancias grandes de HANA. 

> [!NOTE]  
> Tenga en cuenta que Microsoft no proporciona la implementación y soporte técnico para las soluciones personalizadas que tienen dispositivos de red de terceros o IPTables. El proveedor del componente utilizado o el integrador es el que debe proporcionar el soporte técnico. 

#### <a name="express-route-global-reach"></a>Alcance Global de ruta de Express
Microsoft presentó una nueva funcionalidad [alcance Global de ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach). Alcance global puede usarse para instancias grandes de HANA en dos escenarios:

- Habilitar el acceso directo desde local a las unidades de instancia grande de HANA implementadas en regiones distintas
- Habilitar la comunicación directa entre las unidades de instancia grande de HANA implementadas en regiones distintas


##### <a name="direct-access-from-on-premise"></a>Acceso directo desde local
En las regiones de Azure donde se ofrece el alcance Global, puede solicitar la habilitación de la funcionalidad de alcance Global para el circuito de ExpressRoute que conecta la red local a la red virtual de Azure que se conecta a las unidades de instancia grande de HANA así. Hay algunas implicaciones de coste para el lado local de su circuito de ExpressRoute. Para conocer los precios, consulte los precios de [complemento alcance Global](https://azure.microsoft.com/pricing/details/expressroute/). No hay ningún costo adicional para usted en relación con el circuito que se conecta a las unidades de instancia grande de HANA en Azure. 

> [!IMPORTANT]  
> En el caso de uso de alcance Global para habilitar el acceso directo entre sus unidades de instancia grande de HANA y los recursos locales, el flujo de datos y control de la red es **no se enruta a través de redes virtuales de Azure**, sino directamente entre Microsoft enrutadores de exchange Enterprise. Como resultado las reglas NSG o ASG o cualquier tipo de, NVA, proxy o firewall que implementó en una red virtual, no se Introducción modifican. **Si usa ExpressRoute de alcance Global para habilitar el acceso directo desde local a las restricciones de las unidades de instancia grande de HANA y los permisos para tener acceso a unidades de instancia grandes de HANA deben definirse en los firewalls en el lado local** 

##### <a name="connecting-hana-large-instances-in-different-azure-regions"></a>Conectar instancias grandes de HANA en diferentes regiones de Azure
En la misma manera, como alcance Global de ExpressRoute se pueden usar para conectarse localmente a unidades de instancia grande de HANA, se puede usar para conectarse a dos inquilinos de instancia grande de HANA que se implementan en dos regiones diferentes. El aislamiento es los circuitos ExpressRoute que usan los inquilinos de instancia grande de HANA para conectarse a Azure en ambas regiones. No hay ningún cargo adicional para conectar dos inquilinos de instancia grande de HANA que se implementan en dos regiones diferentes. 

> [!IMPORTANT]  
> El flujo de datos y el flujo de control del tráfico de red entre los inquilinos de la instancia no se enrutarán a través de redes de azure la grande de HANA diferentes. Como resultado no se puede usar la funcionalidad de Azure o aplicaciones virtuales de red para aplicar restricciones de comunicación entre los dos inquilinos de instancias grandes de HANA. 

Para obtener más detalles sobre cómo obtener ExpressRoute alcance Global habilitada, lea el documento [conectar una red virtual para instancias grandes de HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route).


## <a name="internet-connectivity-of-hana-large-instance"></a>Conectividad a Internet de HANA (instancias grandes)
HANA (instancias grandes) *NO* tiene conectividad directa a Internet. Por ejemplo, esta limitación puede restringir la capacidad de registrar la imagen del sistema operativo directamente con el proveedor del sistema operativo. Debe trabajar con la herramienta de administración de suscripciones de SUSE Linux Enterprise Server local o con el administrador de suscripciones de Red Hat Enterprise Linux.

## <a name="data-encryption-between-vms-and-hana-large-instance"></a>Cifrado de datos entre las máquinas virtuales y HANA (instancias grandes)
Los datos transferidos entre HANA (instancias grandes) y las máquinas virtuales no se cifran. Sin embargo, solo para el intercambio entre el DBMS de HANA y las aplicaciones basadas en JDBC y ODBC, puede habilitar el cifrado del tráfico. Para más información, consulte [esta documentación de SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

## <a name="use-hana-large-instance-units-in-multiple-regions"></a>Uso de unidades de HANA (instancias grandes) en varias regiones

Para obtener opciones de configuración de recuperación ante desastres, debe tener unidades de instancia grande de SHANA en varias regiones de Azure. Incluso con el uso de Azure [emparejamiento de Vnet Global], el enrutamiento transitivo de forma predeterminada no funciona entre los inquilinos de instancia grande de HANA en dos regiones diferentes. Sin embargo, alcance Global abre la ruta de comunicación entre las unidades de instancia grande de HANA que se ha aprovisionado en dos regiones diferentes. Este escenario de uso de alcance Global de ExpressRoute permite:

 - Replicación del sistema HANA sin necesidad de firewalls o servidores proxy adicionales
 - Copia las copias de seguridad entre las unidades de instancia grande de HANA en dos regiones diferentes para realizar copias del sistema o las actualizaciones del sistema


![Redes virtuales conectadas a demarcaciones de instancias grandes de Azure en diferentes regiones de Azure](./media/hana-overview-architecture/image8-multiple-regions.png)

En la ilustración se muestra cómo las distintas redes virtuales en ambas regiones están conectadas a dos circuitos ExpressRoute diferentes que se usan para conectarse a SAP HANA en Azure (instancias grandes) en ambas regiones de Azure (líneas en gris). Motivo de esta dos conexiones cruzadas es proteger contra una interrupción de los msee en ambos lados. El flujo de comunicación entre las dos redes virtuales en dos regiones de Azure se supone que debe controlarse a través de la [emparejamiento global](https://blogs.msdn.microsoft.com/azureedu/2018/04/24/how-to-setup-global-vnet-peering-in-azure/) de las dos redes virtuales en las dos regiones diferentes (línea azul de puntos). La línea roja gruesa describe la conexión de ExpressRoute de alcance Global, que permite que las unidades de instancia grande de HANA de los inquilinos en dos regiones diferentes se comuniquen entre sí. 

> [!IMPORTANT] 
> Si se usan varios circuitos ExpressRoute, deben utilizarse las configuraciones de anteposición de AS Path y de preferencia local de BGP para garantizar el enrutamiento adecuado del tráfico.

**Pasos siguientes**
- Consulte [SAP HANA (Large Instances) storage architecture](hana-storage-architecture.md) (Arquitectura de almacenamiento de SAP HANA [instancias grandes]).