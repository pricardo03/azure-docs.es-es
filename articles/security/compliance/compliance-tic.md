---
title: Guía de conexiones a Internet de confianza para Azure
description: Guía de conexiones a Internet de confianza para Azure y servicios de SaaS
services: security
author: dlapiduz
ms.assetid: 09511e03-a862-4443-81ac-ede815bdaf25
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: dlap
ms.openlocfilehash: bb186ab2700b147bee3a7dd81474409ccafb76fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60608090"
---
# <a name="trusted-internet-connections-guidance"></a>Guía de conexiones a Internet de confianza

Este artículo describe cómo las administraciones públicas pueden utilizar Microsoft Azure Government para ayudar cumplir la iniciativa Conexiones a Internet de confianza (TIC). El artículo describe cómo usar Azure Government en las ofertas de infraestructura como servicio (IaaS) de Azure y de plataforma como servicio (PaaS) de Azure.

## <a name="trusted-internet-connections-overview"></a>Introducción a la iniciativa Conexiones a Internet de confianza

El objetivo de la iniciativa TIC es optimizar y estandarizar la seguridad de las conexiones de red externas individuales que usan las administraciones federales. La directiva se describe en el [memorándum M-08-05](https://georgewbush-whitehouse.archives.gov/omb/memoranda/fy2008/m08-05.pdf) de la Oficina de Administración y Presupuesto (OMB) de Estados Unidos.

En noviembre de 2007, la OMB estableció el programa TIC para mejorar la seguridad perimetral de la red federal y las funciones de respuesta a incidentes. La iniciativa TIC se diseñó para realizar análisis de redes de todo el tráfico entrante y saliente con el dominio .gov para identificar firmas específicas y datos basados en patrones. TIC descubre las anomalías de comportamientos, como las actividades de redes de robots (botnet). Las agencias están obligadas a consolidar sus conexiones de red externa y enrutar todo el tráfico a través de dispositivos de detección y prevención de intrusiones conocidos como EINSTEIN. Los dispositivos se hospedan en un número limitado de puntos de conexión de red, que se conocen como _conexiones a Internet de confianza_.

El objetivo de la iniciativa TIC es que las administraciones sepan:
- Quién se conecta a su red, con o sin autorización.
- Cuándo y por qué se accede a su red.
- A qué recursos se accede.

Actualmente, todas las conexiones externas de las administraciones deben enrutarse a través de una TIC aprobada por la OMB. Las administraciones federales deben participar en el programa TIC como proveedor de acceso de TIC (TICAP) o contratando los servicios de uno de los principales proveedores de servicios de Internet de nivel 1. Estos proveedores se llaman proveedores de servicios de protocolo de Internet de confianza administrados (MTIPS). TIC incluye funcionalidades obligatorias críticas que deben realizar la administración y el proveedor MTIPS. En la versión actual de TIC, los dispositivos de detección de intrusiones EINSTEIN versión 2 y de prevención de intrusiones EINSTEIN versión 3 acelerada (3A) se implementan en cada TICAP y MTIPS. La Agencia establece un "Memorándum de entendimiento" con el Departamento de Seguridad Nacional (DHS) para implementar las funcionalidades de EINSTEIN en los sistemas federales.

Como parte de su responsabilidad de proteger la red .gov, DHS requiere las fuentes de distribución de datos sin procesar de los datos del flujo de red de las administraciones para correlacionar incidentes en todo el organismo federal y realizar análisis con herramientas especializadas. Los enrutadores del DHS proporcionan la capacidad de recopilar el tráfico de red IP según entra o sale de una interfaz. Los administradores de red pueden analizar los datos del flujo de red para determinar el origen y el destino del tráfico, la clase de servicio, etc. Los datos del flujo de red se consideran "datos sin contenido" similares al encabezado, dirección IP de origen, dirección IP de destino, etc. Los datos sin contenido permiten al DHS obtener información sobre el contenido: quién estaba haciendo qué y durante cuánto tiempo.

La iniciativa también incluye directivas de seguridad, directrices y marcos específicos para infraestructuras locales. Dado que los organismos gubernamentales se mueven a la nube para lograr ahorros de costos, eficacia operativa e innovación, los requisitos de implementación de TIC pueden ralentizar el tráfico de red. Como resultado, se limitan la velocidad y agilidad con las qué los usuarios del gobierno pueden acceder a los datos en la nube.

## <a name="azure-networking-options"></a>Opciones de redes de Azure

Existen tres opciones principales para conectarse a los servicios de Azure:

- Conexión directa a Internet: conéctese a los servicios de Azure directamente mediante una conexión a Internet abierta. El medio y la conexión son públicos. Para garantizar la privacidad, se emplea cifrado a nivel de transporte y aplicación. El ancho de banda está limitado por la conectividad del sitio a Internet. Use más de un proveedor activo para garantizar la resistencia.
- Red privada virtual (VPN): conéctese a una red virtual de Azure de manera privada utilizando VPN Gateway.
El medio es público, ya que atraviesa una conexión a Internet estándar de un sitio, pero la conexión está cifrada en un túnel para garantizar la privacidad. El ancho de banda está limitado en función de los dispositivos VPN y la configuración seleccionados. Las conexiones de punto a sitio de Azure suelen estar limitadas a 100 Mbps y las conexiones de sitio a sitio están limitadas a 1,25 Gbps.
- Azure ExpressRoute: ExpressRoute es una conexión directa a los servicios de Microsoft. Puesto que la conectividad se realiza a través de un canal de fibra aislado, la conexión puede ser pública o privada, según la configuración utilizada. El ancho de banda suele estar limitado a un máximo de 10 Gbps.

Hay varias maneras de cumplir los requisitos del anexo H (Consideraciones de la nube) de TIC, como se especifica en el documento del Departamento de Seguridad Nacional "Trusted Internet Connections (TIC) Reference Architecture Document, Version 2.0" (Documento de arquitectura de referencia de conexiones a Internet de confianza, versión 2.0). Este artículo se refiere a la guía de TIC del DHS como **TIC 2.0**.

Para habilitar la conexión del **departamento o la administración (D/A)** a Azure u Office 365 sin enrutar el tráfico a través de la TIC del D/A, el D/A debe usar un túnel cifrado o una conexión dedicada con el proveedor de servicios en la nube (CSP). Los servicios del CSP pueden garantizar que no se ofrezca la conectividad a los recursos en la nube del D/A a la red pública de Internet para el acceso directo de personal de la administración.

Office 365 cumple las especificaciones del anexo H de TIC 2.0 al usar ExpressRoute con el [emparejamiento de Microsoft](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings) habilitado o una conexión a Internet que cifra todo el tráfico con TLS 1.2. Los usuarios finales de D/A en la red del D/A se pueden conectar mediante la red de su administración y la infraestructura de TIC mediante internet. Todo el acceso remoto de Internet a Office 365 se bloquea y se enruta a través de la administración. El D/A también puede conectarse a Office 365 a través de una conexión de ExpressRoute con el emparejamiento de Microsoft (un tipo de emparejamiento público) habilitado.  

Solo para Azure, la segunda opción (VPN) y la tercera (ExpressRoute) pueden cumplir estos requisitos cuando se usan junto con servicios que limitan el acceso a Internet.

![Conexión a Internet de confianza (TIC) de Microsoft](media/tic-diagram-a.png)

## <a name="azure-infrastructure-as-a-service-offerings"></a>Ofertas de infraestructura como servicio de Azure

El cumplimiento de la directiva de TIC al usar la infraestructura como servicio de Azure es relativamente sencillo, ya que los clientes de Azure administran su propio enrutamiento de red virtual.

El requisito principal para ayudar a garantizar el cumplimiento de la arquitectura de referencia de TIC es asegurarse de que la red virtual es una extensión privada de la red del D/A. Para ser una extensión _privada_, la directiva requiere que solo salga tráfico de su red a través de la conexión de red de TIC local. Este proceso se conoce como _tunelización forzada_. Para el cumplimiento de TIC, el proceso enruta todo el tráfico procedente de cualquier sistema del entorno del CSP mediante una puerta de enlace local en la red de la organización hacia Internet a través de la TIC.

El cumplimiento de TIC de la infraestructura como servicio de Azure se divide en dos pasos principales:

- Paso 1: Configuración.
- Paso 2: Auditoría.

### <a name="azure-iaas-tic-compliance-configuration"></a>Cumplimiento de TIC de la IaaS de Azure: Configuración

Para configurar una arquitectura compatible con TIC con Azure, debe impedir en primer lugar el acceso directo de Internet a la red virtual y, a continuación, forzar el tráfico de Internet a través de la red local.

#### <a name="prevent-direct-internet-access"></a>Impedir el acceso directo a Internet

La conexión de red de IaaS de Azure se realiza mediante redes virtuales que constan de subredes a las que están asociados los controladores de interfaz de red (NIC) de las máquinas virtuales.

El escenario más sencillo para garantizar el cumplimiento de TIC es asegurar que una máquina virtual o una colección de máquinas virtuales no pueda conectarse a ningún recurso externo. Garantice la desconexión de las redes externas mediante el uso de grupos de seguridad de red (NSG). Use grupos de seguridad de red para controlar el tráfico a una o varias NIC o subredes de la red virtual. Un grupo de seguridad de red contiene reglas de control de acceso que permitan o denieguen el tráfico según la dirección del tráfico, el protocolo, la dirección de origen y el puerto, y la dirección de destino y el puerto. Puede cambiar las reglas de un grupo de seguridad de red en cualquier momento y los cambios se aplican a todas las instancias asociadas. Para más información acerca de cómo crear un grupo de seguridad de red, consulte [Filtrado del tráfico de red con un grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

#### <a name="force-internet-traffic-through-an-on-premises-network"></a>Forzado del tráfico de Internet a través de una red local

Azure crea automáticamente rutas del sistema y las asigna a todas las subredes de una red virtual. No puede crear ni eliminar rutas del sistema, pero puede reemplazar algunas de ellas por rutas personalizadas. Azure crea rutas del sistema predeterminadas para cada subred. Azure agrega rutas predeterminadas opcionales a subredes específicas o a todas las subredes al usar funcionalidades específicas de Azure. Este tipo de enrutamiento garantiza que:
- El tráfico con destino dentro de la red virtual permanece dentro de la red virtual.
- Se eliminan los espacios de direcciones privadas designadas por IANA, como 10.0.0.0/8, a menos que estén incluidos en el espacio de direcciones de la red virtual.
- Hay enrutamiento de "último recurso" de 0.0.0.0/0 al punto de conexión de Internet de la red virtual.

![Tunelización forzada de TIC](media/tic-diagram-c.png)

Para garantizar que todo el tráfico pasa por la TIC del D/A, todo el tráfico que sale de la red virtual se debe enrutar mediante la conexión local. Para crear rutas personalizadas, cree rutas definidas por el usuario o intercambie rutas del protocolo de puerta de enlace de borde (BGP) entre su puerta de enlace de red local y una puerta de enlace de VPN de Azure. Para más información sobre las rutas definidas por el usuario, consulte [Enrutamiento del tráfico de redes virtuales: Rutas definidas por el usuario](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined). Para más información sobre BGP, consulte [Enrutamiento del tráfico de redes virtuales: Protocolo de puerta de enlace de borde](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol).

#### <a name="add-user-defined-routes"></a>Adición de rutas definidas por el usuario

Si usa una puerta de enlace de red virtual basada en rutas, puede forzar la tunelización en Azure. Agregue una ruta definida por el usuario (UDR) que establece que el tráfico de 0.0.0.0/0 se enruta al **próximo salto** de la puerta de enlace de red virtual. Azure da prioridad a las rutas definidas por el usuario sobre las rutas definidas por el sistema. Todo el tráfico de red no virtual se envía a la puerta de enlace de red virtual, que, a continuación, puede enrutar el tráfico al entorno local. Después de definir la UDR, asocie la ruta a las subredes existentes o a nuevas subredes dentro de todas las redes virtuales del entorno de Azure.

![Rutas definidas por el usuario y TIC](media/tic-diagram-d.png)

#### <a name="use-the-border-gateway-protocol"></a>Uso del protocolo de puerta de enlace de borde

Si usa ExpressRoute o una puerta de enlace de red virtual habilitada para BGP, BGP es el mecanismo preferido para el anuncio de rutas. Con una ruta de 0.0.0.0/0 anunciada por BGP, ExpressRoute y las puertas de enlace de red virtual compatibles con BGP garantizan que la ruta predeterminada se aplica a todas las subredes de las redes virtuales.

### <a name="azure-iaas-tic-compliance-auditing"></a>Cumplimiento de TIC de la IaaS de Azure: Auditoría

Azure ofrece varias formas de auditar el cumplimiento de TIC.

#### <a name="view-effective-routes"></a>Vista de las rutas eficaces

Confirme que la ruta predeterminada se propaga observando las _rutas efectivas_ de una máquina virtual determinada, una NIC específica o una tabla de rutas definidas por el usuario en [Azure Portal](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#diagnose-using-azure-portal) o en [ Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-powershell#diagnose-using-powershell). Las **rutas efectivas** muestran las rutas definidas por el usuario pertinentes, las rutas anunciadas por BGP y las rutas del sistema que se aplican a la entidad pertinente, tal como se muestra en la ilustración siguiente:

![Rutas eficaces](media/tic-screen-1.png)

> [!NOTE]
> No se pueden ver las rutas efectivas de una NIC a menos que esté asociada a una máquina virtual en ejecución.

#### <a name="use-azure-network-watcher"></a>Uso de Azure Network Watcher

Azure Network Watcher ofrece varias herramientas para auditar el cumplimiento de TIC. Para más información, consulte [esta introducción a Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).

##### <a name="capture-network-security-group-flow-logs"></a>Captura de los registros de flujo de los grupos de seguridad de red 

Utilice Network Watcher para capturar los registros de flujo de red que indican los metadatos que rodean el tráfico IP. Los registros de flujo de red contienen las direcciones de origen y destino de los destinos y otros datos. Puede usar estos datos con los registros de la puerta de enlace de red virtual, los dispositivos perimetrales locales o la TIC para supervisar que todo el tráfico se enruta de forma local. 

## <a name="azure-platform-as-a-service-offerings"></a>Ofertas de plataforma como servicio de Azure

Los servicios de PaaS de Azure, como Azure Storage, son accesibles mediante una dirección URL accesible desde Internet. Cualquier persona con credenciales autorizadas puede acceder a un recurso, como una cuenta de almacenamiento, desde cualquier ubicación sin pasar por una TIC. Por este motivo, muchos clientes de administraciones públicas creen equivocadamente que los servicios de PaaS de Azure no cumplen las directivas de TIC. Muchos servicios de PaaS de Azure pueden ser compatibles con la directiva de TIC. Un servicio es conforme cuando la arquitectura es la misma que la del entorno de IaaS compatible con TIC ([como se describió anteriormente](https://docs.microsoft.com/azure/security/compliance/compliance-tic#azure-infrastructure-as-a-service-offerings)) y el servicio está asociado a una red virtual de Azure.

Cuando los servicios de PaaS de Azure están integrados con una red virtual, el servicio es accesible de forma privada desde esa red virtual. Puede aplicar enrutamiento personalizado para 0.0.0.0/0 mediante rutas definidas por el usuario o BGP. El enrutamiento personalizado asegura que todo el tráfico vinculado a Internet se enruta de forma local para transcurrir por la TIC. Puede integrar los servicios de Azure en redes virtuales mediante el uso de los siguientes patrones:

- **Implementación de una instancia dedicada de un servicio**: cada vez son más los servicios PaaS que se pueden implementar como instancias dedicadas con puntos de conexión vinculados a la red virtual. Puede implementar App Service Environment para PowerApps en el modo "Aislado" para permitir que el punto de conexión de red se pueda restringir a una red virtual. App Service Environment puede hospedar así muchos servicios de PaaS de Azure, como Azure Web Apps, Azure API Management y Azure Functions.
- **Uso de puntos de conexión de servicio de red virtual**: cada vez son más los servicios PaaS que brindan la posibilidad de trasladar el punto de conexión a una dirección IP privada de una red virtual en lugar de a una dirección pública.

Los servicios que admiten la implementación de instancias dedicadas en una red virtual o el uso de puntos de conexión de servicio, a partir de mayo de 2018, se enumeran en las tablas siguientes.

> [!Note]
> El estado de disponibilidad se corresponde con los servicios de Azure que están disponibles comercialmente. El estado de disponibilidad para los servicios de Azure en Azure Government está pendiente.

### <a name="support-for-service-endpoints"></a>Compatibilidad con los puntos de conexión de servicio

|Servicio                        |Disponibilidad      |
|-------------------------------|------------------|
|Azure Key Vault                | Versión preliminar privada  |
|Azure Cosmos DB                | Versión preliminar privada  |
|Servicios de identidad              | Versión preliminar privada  |
|Azure Data Lake                | Versión preliminar privada  |
|Azure Database for PostgreSQL  | Versión preliminar privada  |
|Azure Database for MySQL       | Versión preliminar privada  |
|Azure SQL Data Warehouse       | Versión preliminar pública   |
|Azure SQL Database             | Disponibilidad general (GA) |
|Azure Storage                  | GA               |

### <a name="support-for-virtual-network-injection"></a>Compatibilidad con la inserción de red virtual

|Servicio                               |Disponibilidad      |
|--------------------------------------|------------------|
|Instancia administrada de Azure SQL Database   | Versión preliminar pública   |
|Azure Kubernetes Service (AKS)        | Versión preliminar pública   |
|Azure Service Fabric                  | GA               |
|Azure API Management                  | GA               |
|Azure Active Directory                | GA               |
|Azure Batch                           | GA               |
|Entorno de App Service               | GA               |
|Azure Cache for Redis                     | GA               |
|HDInsight de Azure                       | GA               |
|Conjunto de escalas de máquina virtual             | GA               |
|Azure Cloud Services                  | GA               |


### <a name="virtual-network-integration-details"></a>Detalles de la integración de red virtual

El siguiente diagrama muestra el flujo general de la red para el acceso a los servicios de PaaS. Se muestra el acceso tanto desde la inserción de red virtual como desde la tunelización del servicio de red virtual. Para más información acerca de las puertas de enlace de servicio de red, las redes virtuales y las etiquetas de servicio, consulte [Grupos de seguridad de redes y aplicaciones: Etiquetas de servicio](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

![Opciones de conectividad de PaaS para TIC](media/tic-diagram-e.png)

1. Se realiza una conexión privada a Azure mediante ExpressRoute. El emparejamiento privado de ExpressRoute con tunelización forzada se usa para forzar todo el tráfico de red virtual del cliente sobre ExpressRoute y de vuelta al entorno local. No es necesario el emparejamiento de Microsoft.
2. Es posible usar Azure VPN Gateway junto con ExpressRoute y el emparejamiento de Microsoft para superponer el cifrado IPSec de un extremo a otro entre la red virtual del cliente y el perímetro local. 
3. La conectividad de red a la red virtual del cliente se controla mediante grupos de seguridad de red, de manera que los clientes pueden permitir o denegar el acceso en función de la IP, el puerto y el protocolo.
4. La red virtual del cliente se extiende al servicio de PaaS mediante la creación de un punto de conexión de servicio para el servicio del cliente.
5. El punto de conexión de servicio de PaaS está protegido para **denegar todo de manera predeterminada** y permitir solo el acceso desde subredes especificadas dentro de la red virtual del cliente. La denegación predeterminada también incluye las conexiones que se originan desde Internet.
6. Otros servicios de Azure que necesiten acceder a recursos dentro de la red virtual del cliente deben:  
   - Implementarse directamente en la red virtual.
   - Autorizarse de manera selectiva en función de las instrucciones del servicio de Azure correspondiente.

#### <a name="option-a-deploy-a-dedicated-instance-of-a-service-virtual-network-injection"></a>Opción A: Implementación de una instancia dedicada de un servicio (inserción de red virtual)

La inserción de red virtual permite que los clientes puedan implementar de forma selectiva instancias dedicadas de un servicio de Azure determinado, como HDInsight, en su propia red virtual. Las instancias del servicio se implementan en una subred dedicada en la red virtual de un cliente. La inserción de red virtual permite el acceso a los recursos del servicio mediante direcciones enrutables que no son de Internet. Las instancias locales usan ExpressRoute o una red privada virtual de sitio a sitio para acceder directamente a las instancias del servicio mediante el espacio de direcciones de red virtual, en lugar de abrir un firewall al espacio de direcciones de Internet público. Cuando se conecta una instancia dedicada a un punto de conexión, puede usar las mismas estrategias en cuanto a cumplimiento de TIC de IaaS. El enrutamiento predeterminado garantiza que el tráfico de Internet se redirige a una puerta de enlace de red virtual que está limitada para el entorno local. Puede controlar aún más el acceso entrante y saliente mediante grupos de seguridad de red para la subred indicada.

![Introducción a la inserción de red virtual](media/tic-diagram-f.png)

#### <a name="option-b-use-virtual-network-service-endpoints-service-tunnel"></a>Opción B: Uso de puntos de conexión de servicio de red virtual (túnel de servicio)

Un número creciente de servicios multiinquilino de Azure ofrecen "puntos de conexión de servicio". Los puntos de conexión de servicio son un método alternativo para la integración con las redes virtuales de Azure. Los puntos de conexión de servicio de red virtual extienden el espacio de direcciones IP de la red virtual y la identidad de la red virtual a los servicios de Azure sobre una conexión directa. El tráfico desde la red virtual al servicio de Azure siempre permanece dentro de la red troncal de Azure. 

Después de habilitar un punto de conexión de servicio para un servicio, utilice las directivas expuestas por el servicio para restringir las conexiones al servicio a esa red virtual. Se aplican comprobaciones de acceso en la plataforma mediante el servicio de Azure. Solo se concede acceso a un recurso bloqueado si la solicitud se origina desde la red virtual o la subred permitidas o desde las dos direcciones IP que se usan para identificar el tráfico local si usa ExpressRoute. Utilice este método para impedir eficazmente que el tráfico entrante o saliente abandone directamente el servicio de PaaS.

![Introducción a los puntos de conexión de servicio](media/tic-diagram-g.png)

## <a name="cloud-native-tools-for-network-situational-awareness"></a>Herramientas nativas de la nube para conocer la situación de la red

Azure proporciona herramientas nativas de la nube para ayudarle a garantizar que conoce la situación de modo que comprende los flujos de tráfico de su red. Las herramientas no son necesarias para el cumplimiento de la directiva de TIC. Las herramientas pueden mejorar considerablemente las funcionalidades de seguridad.

### <a name="azure-policy"></a>Azure Policy

[Azure Policy](../../governance/policy/overview.md) es un servicio de Azure que proporciona a su organización una mejor capacidad de auditar y aplicar las iniciativas de cumplimiento. Los clientes pueden planear y probar sus reglas de Azure Policy ahora para asegurar el futuro cumplimiento de TIC.

Azure Policy está dirigido al nivel de suscripción. El servicio proporciona una interfaz centralizada en la que puede realizar tareas de cumplimiento, incluidas:
- Administración de iniciativas
- Configuración de las definiciones de directivas
- Auditoría de cumplimiento
- Exigir el cumplimiento
- Administración de excepciones

Junto con muchas definiciones integradas, los administradores pueden definir sus propias definiciones personalizadas mediante sencillas plantillas JSON. Microsoft recomienda dar prioridad a la auditoría sobre el cumplimiento, siempre que sea posible.

Las siguientes directivas de ejemplo se pueden utilizar para escenarios de cumplimiento de TIC:

|Directiva  |Escenario de ejemplo  |Plantilla  |
|---------|---------|---------|
|Aplicar tabla de rutas definida por el usuario. | Asegúrese de que la ruta predeterminada de todas las redes virtuales apunta a una puerta de enlace de red virtual aprobada para el enrutamiento al entorno local.    | Puede empezar a trabajar con esta [plantilla](../../governance/policy/samples/no-user-defined-route-table.md). |
|Auditar si Network Watcher no está habilitado para una región  | Asegúrese de que Network Watcher está habilitado para todas las regiones utilizadas.  | Puede empezar a trabajar con esta [plantilla](../../governance/policy/samples/network-watcher-not-enabled.md). |
|NSG en cada subred.  | Asegúrese de que un grupo de seguridad de red (o un conjunto de grupos de seguridad de red aprobados) con el tráfico de Internet bloqueado se aplica a todas las subredes de cada red virtual. | Puede empezar a trabajar con esta [plantilla](../../governance/policy/samples/nsg-on-subnet.md). |
|NSG en cada NIC. | Asegúrese de que un grupo de seguridad de red con el tráfico de Internet bloqueado se aplica a todas las NIC de todas las máquinas virtuales. | Puede empezar a trabajar con esta [plantilla](../../governance/policy/samples/nsg-on-nic.md). |
|Use una red virtual aprobada para las interfaces de red de máquina virtual.  | Asegúrese de que todas las NIC están en una red virtual aprobada. | Puede empezar a trabajar con esta [plantilla](../../governance/policy/samples/use-approved-vnet-vm-nics.md). |
|Ubicaciones permitidas. | Asegúrese de que todos los recursos se implementan en regiones con configuración de Network Watcher y redes virtuales compatibles.  | Puede empezar a trabajar con esta [plantilla](../../governance/policy/samples/allowed-locations.md). |
|Tipos de recursos no permitidos, como **direcciones IP públicas**. | Prohíba la implementación de tipos de recursos que no tengan un plan de cumplimiento. Utilice esta directiva para prohibir la implementación de recursos de direcciones IP públicas. Aunque se pueden usar reglas de grupos de seguridad de red para bloquear el tráfico entrante de Internet eficazmente, impedir el uso de direcciones IP públicas reduce aún más la superficie expuesta a ataques.   | Puede empezar a trabajar con esta [plantilla](../../governance/policy/samples/not-allowed-resource-types.md).  |

### <a name="network-watcher-traffic-analytics"></a>Análisis de tráfico de Network Watcher

El [análisis de tráfico](https://azure.microsoft.com/blog/traffic-analytics-in-preview/) de Network Watcher consume datos del registro de flujo y otros registros para proporcionar información general de alto nivel del tráfico de red. Estos datos pueden ser útiles para auditar el cumplimiento de TIC y para identificar aspectos problemáticos. Puede usar el panel de alto nivel para detectar rápidamente qué máquinas virtuales se comunican con Internet y obtener una lista focalizada para el enrutamiento de TIC.

![Análisis del tráfico](media/tic-traffic-analytics-1.png)

Use el **mapa geográfico** para identificar rápidamente los destinos físicos probables del tráfico de Internet de las máquinas virtuales. Puede identificar y evaluar las ubicaciones sospechosas o los cambios de patrón:

![Mapa geográfico](media/tic-traffic-analytics-2.png)

Use la **topología de redes virtuales** para analizar rápidamente las redes virtuales existentes:

![Mapa de topología de red](media/tic-traffic-analytics-3.png)

### <a name="network-watcher-next-hop-tests"></a>Pruebas de próximo salto de Network Watcher

Las redes de las regiones que se supervisan mediante Network Watcher pueden realizar pruebas de próximo salto. En Azure Portal, puede especificar un origen y un destino para un flujo de red de ejemplo para que Network Watcher resuelva el destino del próximo salto. Ejecute esta prueba en máquinas virtuales y direcciones de Internet de ejemplo para asegurarse de que el destino del próximo salto es la puerta de enlace de red virtual esperada.

![Pruebas de próximo salto](media/tic-network-watcher.png)

## <a name="conclusions"></a>Conclusiones

Puede configurar fácilmente el acceso de Microsoft Azure, Office 365 y Dynamics 365 para ayudar a cumplir la guía del apéndice H de TIC 2.0, según se define con fecha de mayo de 2018. Microsoft reconoce que la guía de TIC está sujeta a cambios. Microsoft intenta ayudar a los clientes a cumplir con la guía de manera oportuna cuando se publiquen nuevas instrucciones.

## <a name="appendix-trusted-internet-connections-patterns-for-common-workloads"></a>Apéndice: Patrones de las conexiones a Internet de confianza para cargas de trabajo comunes

| Categoría | Carga de trabajo | IaaS | PaaS dedicado / Inserción de red virtual  | Puntos de conexión de servicio  |
|---------|---------|---------|---------|--------|
| Proceso | Máquinas virtuales Linux en Azure | Sí | | |
| Proceso | Máquinas virtuales Windows en Azure | Sí | | |
| Proceso | Conjuntos de escalas de máquina virtual | Sí | | |
| Proceso | Azure Functions | | Entorno de App Service | |
| Web y móviles | Aplicación web interna | | Entorno de App Service| |
| Web y móviles | Aplicación móvil interna | | Entorno de App Service | |
| Web y móviles | Aplicaciones de API | | Entorno de App Service | |
| Contenedores | Azure Container Service | | | Sí |
| Contenedores | Azure Kubernetes Service (AKS) \* | | | Sí |
| Base de datos | Azure SQL Database | | Instancia administrada de Azure SQL Database \* | Azure SQL |
| Base de datos | Azure Database for MySQL | | | Sí |
| Base de datos | Azure Database for PostgreSQL | | | Sí |
| Base de datos | Azure SQL Data Warehouse | | | Sí |
| Base de datos | Azure Cosmos DB | | | Sí |
| Base de datos | Azure Cache for Redis | | Sí | |
| Almacenamiento | Azure Blob Storage | Sí | | |
| Almacenamiento | Archivos de Azure | Sí | | |
| Almacenamiento | Azure Queue Storage | Sí | | |
| Almacenamiento | Almacenamiento de tablas de Azure | Sí | | |
| Almacenamiento | Azure Disk Storage | Sí | | |

\* Versión preliminar pública en Azure Government con fecha de mayo de 2018
