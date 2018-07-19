---
title: Guía de conexión a Internet de confianza para Azure
description: Guía de conexión a Internet de confianza para Azure y servicios de SaaS
services: security
author: dlapiduz
ms.assetid: 09511e03-a862-4443-81ac-ede815bdaf25
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: dlap
ms.openlocfilehash: cf24810c0aa414e751e55df163563f013c1a0081
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969952"
---
# <a name="trusted-internet-connection-guidance"></a>Guía de conexión a Internet de confianza

## <a name="background"></a>Fondo

El objetivo de la iniciativa de conexiones a Internet de confianza (TIC) es optimizar y estandarizar la seguridad de las conexiones de red externas individuales que usan actualmente las administraciones federales. La directiva se describe en el [memorándum M-05-08](https://georgewbush-whitehouse.archives.gov/omb/memoranda/fy2008/m08-05.pdf) de la Oficina de Administración y Presupuesto (OMB) de Estados Unidos.

En noviembre de 2007, la OMB estableció el programa TIC para mejorar la seguridad perimetral de la red federal y las funciones de respuesta a incidentes. La TIC se diseñó para realizar análisis de redes de todo el tráfico entrante y saliente con el dominio .gov para identificar firmas específicas y datos basados en patrones, así como para detectar anomalías del comportamiento, por ejemplo, actividades de red de bots. Se solicitó a las administraciones que consolidaran sus conexiones de red externas y que dirigieran todo el tráfico mediante dispositivos de detección y prevención de intrusiones (conocidos como EINSTEIN) hospedados en un número limitado de puntos de conexión de red (denominados conexiones a Internet de confianza).

En pocas palabras, el objetivo de la TIC es que las administraciones sepan:
- Quién se conecta a su red, con o sin autorización.
- Cuándo y por qué se accede a su red.
- A qué recursos se accede.

Actualmente todas las conexiones externas de las administraciones deben enrutarse a través de una TIC aprobada por la OMB. Las administraciones federales deben participar en el programa TIC como proveedor de acceso de TIC (TICAP) o contratando los servicios de uno de los principales proveedores de servicios de Internet de nivel 1, denominados proveedores de servicios de protocolo de Internet de confianza administrados (MTIPS).  TIC incluye funcionalidades obligatorias críticas que actualmente realizan la administración y el proveedor MTIPS. En la versión actual de TIC, se implementa dispositivos de detección de intrusiones de la versión 2 de EINSTEIN y de prevención de intrusiones de la versión 3 acelerada (3A) de EINSTEIN en cada TICAP y MTIPS y la administración establece un memorándum de acuerdo con el Departamento de Seguridad Nacional (DHS) de Estados Unidos para implementar funcionalidades de EINSTEIN en sistemas federales.

Como parte de su responsabilidad de proteger la red .gov, DHS requiere las fuentes de distribución de datos sin procesar de los datos del flujo de red de las administraciones para correlacionar incidentes en todo el organismo federal y realizar análisis con herramientas especializadas. Los enrutadores del DHS proporcionan la capacidad de recopilar el tráfico de red IP según entra o sale de una interfaz. Al analizar los datos del flujo de red, un administrador de red puede determinar aspectos como el origen y el destino del tráfico, la clase de servicio, etc. Los datos de flujo de red se consideran "datos sin contenido" (por ejemplo, encabezado, IP de origen, IP de destino, etc.) y permiten al DHS conocer información acerca del contenido; es decir, quién ha hecho qué y durante cuanto tiempo.

La iniciativa también incluye directivas de seguridad, directrices y marcos específicos para infraestructuras locales. A medida que las administraciones públicas se cambian a la nube por razones de ahorro de costos, eficiencia operativa e innovación, los requisitos de implementación de TIC ralentizan en ciertos casos el tráfico de red y limitan la velocidad y agilidad con la que los usuarios de administraciones públicas pueden acceder a sus datos basados en la nube.

Este artículo describe cómo las administraciones públicas pueden utilizar Microsoft Azure Government para ayudar cumplir la directiva de TIC en servicios de IaaS y PaaS.

## <a name="summary-of-azure-networking-options"></a>Resumen de las opciones de redes de Azure

Existen tres opciones principales al conectarse a los servicios de Azure:

1. Conexión directa a Internet: conéctese con servicios de Azure directamente mediante una conexión a Internet abierta. El medio es público, así como la conexión. Para garantizar la privacidad, se emplea cifrado a nivel de transporte y aplicación. El ancho de banda está limitado por la conectividad de un sitio a Internet y se pueden usar varios proveedores activos para garantizar la resistencia.
1. Red privada virtual: conéctese a Azure Virtual Network de manera privada mediante una instancia de VPN Gateway.
El medio es público, ya que atraviesa una conexión a Internet estándar de un sitio, pero la conexión está cifrada en un túnel para garantizar la privacidad. El ancho de banda está limitado en función de los dispositivos VPN y la configuración seleccionados. Las conexiones de punto a sitio de Azure suelen estar limitadas a 100 Mbps, mientras que las conexiones de sitio a sitio están limitadas a 1,25 Gbps.
1. Microsoft ExpressRoute: ExpressRoute es una conexión directa a los servicios de Microsoft. Puesto que la conectividad se realiza a través de un canal de fibra aislado, la conexión puede ser pública o privada, según la configuración utilizada. El ancho de banda suele estar limitado a un máximo de 10 Gbps.

Hay varias maneras de satisfacer los requisitos del anexo H sobre conexiones a Internet de confianza (Consideraciones de la nube), que se encuentra en el documento del Departamento de Seguridad Nacional "Trusted Internet Connections (TIC) Reference Architecture Document, Version 2.0" (Documento de arquitectura de referencia de conexiones a Internet de confianza, versión 2.0). En este documento, se hace referencia a la TIC de DHS como TIC 2.0.

Para habilitar la conexión del departamento o la administraciones (D/A) a Azure u Office 365 sin enrutar el tráfico a través de la TIC del D/A, el D/A debe usar un túnel cifrado o una conexión dedicada para el proveedor de servicios en la nube (CSP). Los servicios del CSP pueden garantizar que no se ofrezca la conectividad a los recursos en la nube del D/A a la red pública de Internet para el acceso directo de personal de la administración.

Office 365 cumple las especificaciones del anexo H de TIC 2.0 al usar ExpressRoute con el [emparejamiento de Microsoft](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains) habilitado o una conexión a Internet que cifra todo el tráfico con TLS 1.2.  Los usuarios finales de D/A en la red del D/A se pueden conectar mediante la red de su administración y la infraestructura de TIC mediante internet. Todo el acceso remoto de Internet a O365 se bloquea y se enruta mediante la administración. El D/A también puede conectarse a O365 a través de una conexión de ExpressRoute con un emparejamiento de Microsoft (que es un tipo de emparejamiento público) habilitado.  

Solo para Azure, las opciones 2 (VPN) y 3 (ExpressRoute) pueden cumplir estos requisitos cuando se usan junto con los servicios que limitan el acceso a Internet.

![Diagrama de conexión a Internet de confianza (TIC) de Microsoft](media/tic-diagram-a.png)

## <a name="how-azure-infrastructure-as-a-service-offerings-can-help-with-tic-compliance"></a>Cómo puede ayudar la infraestructura como servicio de Azure con el cumplimiento de TIC

El cumplimiento de la directiva de TIC con IaaS es relativamente sencillo, ya que los clientes de Azure administran su propio enrutamiento de red virtual.

El requisito principal para ayudar a garantizar el cumplimiento de la arquitectura de referencia de TIC es asegurarse de que la red virtual se convierte en una extensión privada de la red del departamento o la administración. Para convertirse en una extensión _privada_, la directiva requiere que solo salga tráfico de su red a través de la conexión de red de TIC local. Este proceso se conoce como "tunelización forzada", que, al usarse para el cumplimiento de TIC, es el proceso de enrutamiento de todo el tráfico procedente de cualquier sistema del entorno del CSP para dirigirlo mediante una puerta de enlace local en la red de una organización hacia Internet a través de la TIC.

El cumplimiento de TIC de Azure IaaS puede dividirse en dos pasos principales:

1. Configuración
1. Auditoría

### <a name="azure-iaas-tic-compliance-configuration"></a>Configuración de cumplimiento de TIC de Azure IaaS

Para configurar una arquitectura compatible con TIC con Azure, tendrá que impedir en primer lugar el acceso directo a Internet a la red virtual y, a continuación, forzar el tráfico de Internet mediante la red local.

#### <a name="prevent-direct-internet-access"></a>Impedir acceso directo a Internet

La conexión de red de Azure IaaS se realiza mediante redes virtuales que constan de subredes, a las que están asociados los controladores de interfaz de red (NIC) de las máquinas virtuales.

El escenario más sencillo para garantizar el cumplimiento de TIC es asegurar que una máquina virtual o una colección de máquinas virtuales no pueda conectarse a ningún recurso externo. La desconexión de redes externas puede conseguirse mediante grupos de seguridad de red (NSG), que pueden usarse para controlar el tráfico a uno o varios NIC o subredes de la red virtual. Un grupo de seguridad de red contiene reglas de control de acceso que permitan o denieguen el tráfico según la dirección del tráfico, el protocolo, la dirección de origen y el puerto, y la dirección de destino y el puerto. Las reglas de un grupo de seguridad de red pueden cambiarse en cualquier momento; los cambios se aplican a todas las instancias asociadas.  Para más información sobre cómo crear un grupo de seguridad de red, consulte el artículo [Creación de un grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

#### <a name="force-internet-traffic-through-on-premises-network"></a>Forzado del tráfico de Internet mediante la red local

Azure crea automáticamente rutas del sistema y las asigna a todas las subredes de una red virtual. No puede crear ni eliminar rutas del sistema, pero puede reemplazar algunas de ellas por rutas personalizadas. Azure crea rutas del sistema predeterminadas para cada subred y agrega rutas predeterminadas opcionales adicionales a determinadas subredes, o a todas las subredes, al usar funcionalidades específicas de Azure. Este enrutamiento garantiza que el tráfico destinado a la red virtual se mantenga dentro de la red virtual, que se descarten los espacios de direcciones privados designados de IANA, como 10.0.0.0/8 (a menos que se incluyan en el espacio de direcciones de la red virtual) y el enrutamiento de "último recurso" de 0.0.0.0/0 al punto de conexión de Internet de la red virtual.

![Tunelización forzada de TIC](media/tic-diagram-c.png)

Para garantizar que todo el tráfico pase por la TIC del D/A, todo el tráfico que sale de la red virtual debe enrutarse mediante la conexión local.  Para crear rutas personalizadas, cree rutas definidas por el usuario o intercambie rutas del protocolo de puerta de enlace de borde (BGP) entre su puerta de enlace de red local y una puerta de enlace de red virtual de Azure. Para más información sobre las rutas definidas por el usuario, consulte https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined. También puede encontrar más información sobre el protocolo de puerta de enlace de borde en https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol.

#### <a name="user-defined-routes"></a>Rutas definidas por el usuario

Si usa una puerta de enlace de red virtual basada en rutas, la tunelización forzada puede realizarse en Azure mediante la adición de una ruta definida por el usuario (UDR) con el valor de tráfico 0.0.0.0/0 para enrutarlo a un "próximo salto" de la puerta de enlace de red virtual. Azure da prioridad a las rutas definidas por el usuario sobre las rutas definidas por el sistema, por lo que esto daría lugar a que todo el tráfico que no sea de redes virtuales se enviara a su puerta de enlace de red virtual, que puede enrutarlo a un entorno local. Una vez definida, esta ruta definida por el usuario debe asociarse a todas las subredes existentes o recién creadas en todas las redes virtuales de su entorno de Azure.

![Rutas definidas por el usuario y TIC](media/tic-diagram-d.png)

#### <a name="border-gateway-protocol"></a>Protocolo de puerta de enlace de borde

Si usa una puerta de enlace de red virtual compatible con un protocolo de puerta de enlace de borde (BGP) o ExpressRoute, BGP es el mecanismo preferido para el anuncio de rutas. Con una ruta anunciada de BGP de 0.0.0.0/0, las puertas de enlace de red virtual compatibles con ExpressRoute y BGP garantizarán que esta ruta predeterminada se aplica a todas las subredes de sus redes virtuales.

### <a name="azure-iaas-tic-compliance-auditing"></a>Auditoría de cumplimiento de TIC de Azure IaaS

Azure ofrece varias formas de auditar el cumplimiento de TIC.

#### <a name="effective-routes"></a>Rutas efectivas

Para confirmar que la ruta predeterminada se ha propagado, puede observar las "rutas efectivas" de una máquina virtual concreta, un NIC específico o una tabla de rutas definidas por el usuario. Esto puede hacerse mediante Azure Portal, como se describe en https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal, o mediante PowerShell, como se describe en https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-powershell. La hoja Rutas efectivas le permite ver las rutas pertinentes definidas por el usuario, las rutas anunciadas de BGP y las rutas del sistema que se aplican a la entidad pertinente, tal como se muestra a continuación.

![captura de pantalla de rutas](media/tic-screen-1.png)

**Nota**: No se pueden ver las rutas efectivas de un NIC a menos que esté asociado a una máquina virtual en ejecución.

#### <a name="network-watcher"></a>Network Watcher

Azure Network Watcher ofrece varias herramientas que se pueden usar para auditar el cumplimiento de TIC.  Más información sobre Network Watcher en https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview.

##### <a name="network-security-groups-flow-logs"></a>Registros de flujo de los grupos de seguridad de red 

Azure Network Watcher proporciona la posibilidad de capturar los registros de flujo de red que indican los metadatos alrededor del tráfico IP. Además de otros datos, los registros de flujo de red contienen las direcciones de origen y destino de los destinos. Esto, junto con los registros de la puerta de enlace de red virtual, los dispositivos perimetrales locales y la TIC, permitirá supervisar que todo el tráfico se está enrutando realmente por la red local. 

## <a name="how-azure-platform-as-a-service-offerings-can-help-with-tic-compliance"></a>Cómo pueden ayudar las ofertas de plataforma como servicio de Azure con el cumplimiento de TIC

Los servicios de PaaS de Azure, como Azure Storage, son accesibles mediante una dirección URL accesible desde Internet. Cualquier persona con credenciales autorizadas puede acceder a un recurso, como una cuenta de almacenamiento, desde cualquier ubicación sin pasar por una TIC. Por este motivo, muchos clientes de administraciones públicas creen equivocadamente que los servicios de PaaS de Azure no cumplen las directivas de TIC. De hecho, muchos servicios de PaaS de Azure pueden cumplir la directiva de TIC utilizando la misma arquitectura que un entorno de IaaS compatible con TIC descrita anteriormente si se pueden vincular a una red virtual. La integración de servicios de PaaS de Azure con una red virtual de Azure permite que el servicio sea accesible de forma privada desde esa red virtual y permite asimismo el enrutamiento personalizado para aplicar 0.0.0.0/0 mediante rutas definidas por el usuario o BGP, lo que garantiza que todo el tráfico dirigido a Internet se enrute a través de la red local para que pase por la TIC.  Algunos servicios de Azure se pueden integrar en redes virtuales mediante los siguientes patrones:

- **Implementación de una instancia dedicada del servicio**: un número creciente de servicios de PaaS se pueden implementar como instancias dedicadas con puntos de conexión vinculados a la red virtual. Por ejemplo, se puede implementar una instancia de App Service Environment (ASE) en modo aislado, lo que permite restringir su punto de conexión de red a una red virtual. Esta instancia de ASE puede hospedar muchos servicios de PaaS de Azure, como Web Apps, API y Functions.
- **Puntos de conexión de servicio de red virtual**: un número creciente de servicios de PaaS permite la opción de trasladar su punto de conexión a una dirección IP privada de red virtual en lugar de una dirección pública.

Los servicios que admiten la implementación de instancias dedicadas en una red virtual o puntos de conexión de servicio en mayo de 2018 se enumeran a continuación: *(La disponibilidad hace referencia a las nubes comerciales de Azure; la disponibilidad de las redes de Azure para administraciones públicas aún está pendiente).

### <a name="service-endpoints"></a>Puntos de conexión de servicio

|Servicio                   |Status            |
|--------------------------|------------------|
|Azure KeyVault            | Vista previa privada  |
|Cosmos DB                 | Vista previa privada  |
|Identidad                  | Vista previa privada  |
|Azure Data Lake           | Vista previa privada  |
|Sql Postgress/Mysql       | Vista previa privada  |
|Azure SQL Data Warehouse  | Vista previa pública   |
|Azure SQL                 | GA               |
|Storage                   | GA               |

### <a name="vnet-injection"></a>Inserción de red virtual

|Servicio                            |Status            |
|-----------------------------------|------------------|
|Instancia administrada de SQL               | Vista previa pública   |
|Azure Container Service (AKS)       | Vista previa pública   |
|Service Fabric                     | GA               |
|API Management                     | GA               |
|Azure Active Directory             | GA               |
|Azure Batch                        | GA               |
|ASE                                | GA               |
|Redis                              | GA               |
|HDI                                | GA               |
|Conjunto de escalado de máquinas virtuales de proceso  | GA               |
|Servicio de proceso en la nube              | GA               |

### <a name="vnet-integration-details"></a>Detalles de integración con red virtual

El diagrama siguiente le guía a través del flujo de red general para acceder a los servicios de PaaS mediante inserción de la red virtual y tunelización del servicio de red virtual.  Consulte https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags para más información sobre las puertas de enlace de servicio de red, las redes virtuales y las etiquetas de servicio.

![Opciones de conectividad de PaaS para TIC](media/tic-diagram-e.png)

1. Conexión privada a Azure mediante ExpressRoute. El emparejamiento privado de ExpressRoute con tunelización forzada se usa para forzar todo el tráfico de red virtual del cliente a través de ExpressRoute de vuelta hacia el entorno local. No se requiere emparejamiento de Microsoft.
2. Es posible usar Azure VPN Gateway junto con el emparejamiento de Microsoft de ExpressRoute para superponer el cifrado IPSec de un extremo a otro entre la red virtual del cliente y el perímetro local. 
3. La conectividad de red a la red virtual del cliente se controla mediante grupos de seguridad de red (NSG), de manera que los clientes pueden permitir o denegar el acceso en función de la IP, el puerto y el protocolo.
4. La red virtual del cliente se extiende al servicio de PaaS mediante la creación de un punto de conexión de servicio para el servicio del cliente.
5. El punto de conexión de servicio de PaaS está protegido al denegar todo de manera predeterminada y permitir solo el acceso desde subredes especificadas dentro de la red virtual del cliente.  La denegación predeterminada también incluye conexiones que se originan desde Internet.
6. Cualquier otro servicio de Azure que necesite acceder a recursos dentro de la red virtual del cliente debe:  
  a. Implementarse directamente en la red virtual  
  b. Autorizarse de manera selectiva en función de las instrucciones del servicio de Azure correspondiente.

#### <a name="option-1-dedicated-instance-vnet-injection"></a>Opción 1: "Inserción de red virtual" de instancia dedicada

Con la inserción de red virtual, los clientes pueden implementar de forma selectiva instancias dedicadas de un servicio de Azure determinado, como HDInsight, en su propia red virtual. Las instancias de servicio se implementan en una subred dedicada en la red virtual de un cliente. La inserción de red virtual permite que los recursos de servicio sean accesibles mediante direcciones enrutables que no son de Internet.  Las instancias locales pueden acceder a estas instancias de servicio mediante el espacio de direcciones de red virtual directamente a través de ExpressRoute o una VPN de sitio a sitio, en lugar de abrir los firewalls a un espacio de direcciones de Internet público. Con una instancia dedicada vinculada a un punto de conexión, se pueden utilizar las mismas estrategias usadas para el cumplimiento de TIC con IaaS, con el enrutamiento predeterminado que garantiza que el tráfico dirigido a Internet se redirija a una puerta de enlace de red virtual dirigida a la red local. El acceso de entrada y de salida se puede controlar aún más mediante grupos de seguridad de red (NSG) para la subred determinada.

![Diagrama de información general de la inserción de red virtual](media/tic-diagram-f.png)

#### <a name="option-2-vnet-service-endpoints"></a>Opción 2: Puntos de conexión de servicio de red virtual 

Un número creciente de servicios multiinquilino de Azure ofrece la funcionalidad "Punto de conexión de servicio", un método alternativo para la integración en redes virtuales de Azure. Los puntos de conexión del servicio de red virtual extienden el espacio de direcciones IP de la red virtual y la identidad de la red virtual al servicio a través de una conexión directa.  El tráfico desde la red virtual al servicio de Azure siempre permanece en la red troncal de Azure. Una vez habilitado un punto de conexión de servicio para un servicio, las conexiones con el servicio pueden restringirse a esa red virtual mediante directivas expuestas por el servicio. El servicio de Azure realiza comprobaciones de acceso en la plataforma y el acceso al recurso bloqueado solo se concede si la solicitud se origina de la red virtual o subred autorizada, o las dos direcciones IP usadas para identificar su tráfico local, si usa ExpressRoute. Esto puede usarse para impedir eficazmente que el tráfico entrante o saliente abandone directamente el servicio de PaaS.

![Diagrama de información general de los puntos de conexión de servicio](media/tic-diagram-g.png)

## <a name="using-cloud-native-tools-for-network-situational-awareness"></a>Uso de herramientas nativas de la nube para conocer la situación de la red

Azure proporciona herramientas nativas de la nube para ayudarle a garantizar que conoce la situación de modo que comprende los flujos de tráfico de su red. Estas herramientas no son necesarias para cumplir la directiva de TIC, pero pueden mejorar considerablemente las funcionalidades de seguridad.

### <a name="azure-policy"></a>Azure Policy

Azure Policy (https://azure.microsoft.com/services/azure-policy/) es un servicio de Azure que proporciona a su organización una mejor capacidad de auditar y aplicar las iniciativas de cumplimiento.  Disponible actualmente en versión preliminar pública en las nubes comerciales de Azure, pero aún no en la nube de Microsoft Azure para administraciones públicas, permite a los clientes interesados en TIC empezar a planear y probar sus reglas de directiva para garantizar el cumplimiento en el futuro. Azure Policy está dirigido a nivel de suscripción y proporciona una interfaz centralizada para la administración de iniciativas, definiciones de directiva, resultados de cumplimiento y auditoría y administración de excepciones. Además de las muchas definiciones de Azure Policy integradas, los administradores pueden definir sus propias definiciones personalizadas mediante sencillas plantillas JSON. Para muchos clientes, Microsoft recomienda dar prioridad a la auditoría sobre la aplicación, siempre que sea posible.

Las siguientes directivas de ejemplo pueden ser útiles para escenarios de cumplimiento de TIC:

|Directiva  |Escenario de ejemplo  |Plantilla inicial  |
|---------|---------|---------|
|Aplicar tabla de rutas definida por el usuario |     Asegúrese de que la ruta predeterminada en todas las redes virtuales señala a una puerta de enlace de red virtual aprobada para el enrutamiento a la red local. | https://docs.microsoft.com/azure/azure-policy/scripts/no-user-def-route-table |
|Auditar si Network Watcher no está habilitado para la región  | Asegúrese de que Network Watcher está habilitado para todas las regiones utilizadas.  | https://docs.microsoft.com/azure/azure-policy/scripts/net-watch-not-enabled |
|NSG X en cada subred  | Asegúrese de que un grupo de seguridad de red (o conjunto de grupos de seguridad de red aprobados) con el tráfico de Internet bloqueado se aplica a todas las subredes de cada red virtual. | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-subnet |
|NSG X en cada NIC | Asegúrese de que un grupo de seguridad de red con el tráfico de Internet bloqueado se aplica a todos los NIC en todas las máquinas virtuales. | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-nic |
|Usar red virtual aprobada para las interfaces de red de máquinas virtuales  | Asegúrese de que todos los NIC están en una red virtual aprobada. | https://docs.microsoft.com/azure/azure-policy/scripts/use-approved-vnet-vm-nics |
|Ubicaciones permitidas | Asegúrese de que todos los recursos se implementan en regiones con configuración de Network Watcher y redes virtuales compatibles.  | https://docs.microsoft.com/azure/azure-policy/scripts/allowed-locs |
|Tipos de recursos no permitidos, como direcciones IP públicas  | Prohíba la implementación de tipos de recursos que no tengan un plan de cumplimiento. Por ejemplo, esta directiva puede usarse para prohibir la implementación de recursos de direcciones IP públicas. Aunque se pueden usar reglas de grupos de seguridad de red para bloquear el tráfico entrante de Internet eficazmente, impedir el uso de direcciones IP públicas reduce aún más la superficie expuesta a ataques.    | https://docs.microsoft.com/azure/azure-policy/scripts/not-allowed-res-type  |

### <a name="azure-traffic-analyticshttpsazuremicrosoftcomen-inblogtraffic-analytics-in-preview"></a>[Análisis de tráfico](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/) de Azure

El análisis de tráfico de Azure Network Watcher consume datos de registro de flujo y otros registros para proporcionar información general de alto nivel de tráfico de red. Estos datos pueden ser útiles para auditar el cumplimiento de TIC y para identificar aspectos problemáticos. Es posible utilizar un panel de alto nivel para detectar rápidamente qué máquinas virtuales se comunican con Internet, lo que, a su vez, proporcionará una lista focalizada para el enrutamiento de TIC.

![Captura de pantalla de análisis de tráfico](media/tic-traffic-analytics-1.png)

Se puede usar un "mapa geográfico" para identificar rápidamente los probables destinos físicos del tráfico de Internet para las máquinas virtuales, lo que permite identificar y evaluar las ubicaciones sospechosas o los cambios de patrón.

![Captura de pantalla de análisis de tráfico](media/tic-traffic-analytics-2.png)

Se puede usar un mapa de topología de red para analizar rápidamente las redes virtuales existentes:

![Captura de pantalla de análisis de tráfico](media/tic-traffic-analytics-3.png)

### <a name="azure-network-watcher-next-hop"></a>Próximo salto de Azure Network Watcher

Las redes de las regiones supervisadas por Network Watcher pueden realizar pruebas de "Próximo salto", lo que facilita el acceso basado en Azure Portal para introducir un origen y un destino para un flujo de red de ejemplo, para el que Network Watcher resolverá el destino de "Próximo salto". Esto puede utilizarse con máquinas virtuales y con direcciones de Internet de ejemplo para asegurarse de que el "próximo salto" sea realmente la puerta de enlace de red virtual.

![Próximo salto de Network Watcher](media/tic-network-watcher.png)

## <a name="conclusions"></a>Conclusiones

El acceso de Microsoft Azure, Office 365 y Dynamics 365 se puede configurar fácilmente para ayudar a cumplir las instrucciones del apéndice H de TIC 2.0 según se define con fecha de mayo de 2018.  Microsoft es consciente de que estas instrucciones están sujetas a cambios y se esforzará por ayudar a los clientes a cumplirlas de manera oportuna conforme se vayan modificando.

## <a name="appendix-tic-patterns-for-common-workloads"></a>Apéndice: Patrones de TIC para cargas de trabajo comunes

| Categoría | Carga de trabajo | IaaS | PaaS dedicado/Inserción de red virtual  | Puntos de conexión de servicio  |
|---------|---------|---------|---------|--------|
| Compute | Máquinas virtuales Linux | Sí | | |
| Compute | Máquinas virtuales Windows | Sí | | |
| Compute | Virtual Machine Scale Sets | Sí | | |
| Compute | Azure Functions | | Mediante App Service Environment (ASE) | |
| Web y móvil | Aplicación web interna | | Mediante App Service Environment (ASE) | |
| Web y móvil | Aplicación móvil interna | | Mediante App Service Environment (ASE) | |
| Web y móvil | API Apps | | Mediante App Service Environment (ASE) | |
| Contenedores | Azure Container Service (ACS) | | | Sí |
| Contenedores | Azure Container Service (AKS)* | | | Sí |
| Base de datos | SQL Database | | Instancia administrada de Azure SQL Database* | Azure SQL |
| Base de datos | Azure Database for MySQL | | | Sí |
| Base de datos | Azure Database for PostgreSQL | | | Sí |
| Base de datos | SQL Data Warehouse | | | Sí |
| Base de datos | Azure Cosmos DB | | | Sí |
| Base de datos | Redis Cache | | Sí | |
| Storage | Blobs | Sí | | |
| Storage | Archivos | Sí | | |
| Storage | Colas | Sí | | |
| Storage | Tablas | Sí | | |
| Storage | Discos | Sí | | |

*: Versión preliminar pública en Azure Government con fecha de mayo de 2018  
**: Versión preliminar privada en Azure Government con fecha de mayo de 2018

