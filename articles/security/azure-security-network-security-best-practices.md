---
title: Procedimientos recomendados de seguridad de la red de Azure | Microsoft Docs
description: Este artículo proporciona un conjunto de procedimientos recomendados de seguridad de la red con funcionalidades de Azure integradas.
services: security
documentationcenter: na
author: TomShinder
manager: mbaldwin
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/17/2018
ms.author: TomSh
ms.openlocfilehash: d89972ff0f7e3035fa20f8d9ee2863b68fa52e9f
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124072"
---
# <a name="azure-network-security-best-practices"></a>Procedimientos recomendados de seguridad de la red de Azure
Puede conectar las [máquinas virtuales y los dispositivos de Azure](https://azure.microsoft.com/services/virtual-machines/) a otros dispositivos en red, colocándolos en [redes virtuales de Azure](https://azure.microsoft.com/documentation/services/virtual-network/). Esto es, puede conectar tarjetas de interfaz de red virtual a una red virtual para posibilitar las comunicaciones basadas en TCP/IP entre dispositivos habilitados para la red. Las máquinas virtuales conectadas a una red virtual de Azure pueden conectarse a dispositivos en la misma red virtual, en distintas redes virtuales, en Internet o, incluso, en sus propias redes locales.

En este artículo se trata un conjunto de procedimientos recomendados de seguridad de la red de Azure. Estos procedimientos recomendados se derivan de nuestra experiencia con las redes en Azure, y las experiencias de clientes como usted.

Para cada procedimiento recomendado, en este artículo se explica:

* Qué es el procedimiento recomendado
* Por qué le conviene habilitar este procedimiento recomendado
* Cuál podría ser el resultado si no habilita el procedimiento recomendado
* Alternativas posibles al procedimiento recomendado
* Cómo aprender a habilitar el procedimiento recomendado

Este artículo de Procedimientos recomendados de seguridad de la red de Azure se basa en las funcionalidades y los conjuntos de características de la plataforma Azure existentes cuando se redactó. Las opiniones y las tecnologías cambian con el tiempo, por lo que se actualizará de forma periódica para reflejar esos cambios.

En las secciones siguientes se describen los procedimientos recomendados de seguridad en la Red.

## <a name="logically-segment-subnets"></a>Segmentación lógica de subredes
Las redes virtuales de Azure son similares a una LAN de red local. La idea detrás de una red virtual de Azure es crear una sola red basada en espacios privados de direcciones IP en la que pueden colocar todas las máquinas virtuales de Azure. Los espacios de direcciones IP privados están en los intervalos de clase A (10.0.0.0/8), B (172.16.0.0/12) y C (192.168.0.0/16).

Los procedimientos recomendados para segmentar lógicamente las subredes son:

**Procedimiento recomendado**: segmentar el mayor espacio de direcciones en las subredes.   
**Detalle**: use los principios de subred basado en [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) para crear las subredes.

**Procedimiento recomendado**: crear controles de acceso de red entre subredes. El enrutamiento entre subredes se realizará automáticamente y no es necesario configurar manualmente las tablas de enrutamiento. Sin embargo, el valor predeterminado es que no hay ningún control de acceso a la red entre las subredes creadas en Azure Virtual Network.   
**Detalle**: use un [grupo de seguridad de red](../virtual-network/virtual-networks-nsg.md) (NSG). Los NSG son dispositivos de inspección de paquetes con estado simple que utilizan el enfoque tupla 5 (IP de origen, puerto de origen, dirección IP de destino, puerto de destino y el protocolo de nivel 4) para crear reglas de permiso o denegación del tráfico de la red. Puede permitir o denegar el tráfico hacia y desde una sola dirección IP, hacia y desde varias direcciones IP o, incluso, hacia y desde subredes enteras.

Al usar NSG para controlar el acceso a la red entre subredes, puede establecer recursos que pertenezcan a la misma zona de seguridad o rol en sus propias subredes.

## <a name="control-routing-behavior"></a>Control del comportamiento de enrutamiento
Cuando coloca una máquina virtual en una instancia de Azure Virtual Network, observará que la máquina virtual puede conectarse a cualquier otra máquina virtual de la misma red virtual, incluso si las otras máquinas virtuales están en subredes diferentes. Esto es posible porque hay una colección de rutas del sistema que están habilitadas de forma predeterminada y que permiten este tipo de comunicación. Estas rutas predeterminadas permiten que las máquinas virtuales de la misma red virtual inicien conexiones entre sí y con Internet (solo para comunicaciones salientes a Internet).

Si bien las rutas del sistema predeterminadas son útiles para muchos escenarios de implementación, habrá veces en las que preferirá personalizar la configuración de enrutamiento para las implementaciones. Puede configurar la dirección del próximo salto para que acceda a destinos específicos.

Igualmente le recomendamos que configure las [rutas definidas por el usuario](../virtual-network/virtual-networks-udr-overview.md) al implementar un dispositivo de seguridad para una red virtual. Trataremos este punto más adelante en la sección dedicada a [proteger los recursos de servicio de Azure críticos únicamente para las redes virtuales](azure-security-network-security-best-practices.md#secure-your-critical-azure-service-resources-to-only-your-virtual-networks).

> [!NOTE]
> Las rutas definidas por el usuario no son necesarias, y las rutas del sistema predeterminadas funcionan en la mayoría de lo casos.
>
>

## <a name="enable-forced-tunneling"></a>Habilitar la tunelización forzada
Para entender mejor la tunelización forzada, resulta útil entender qué es el "túnel dividido". El ejemplo más común de tunelización dividida se ve con las conexiones de red privada (VPN). Imagine que establece una conexión VPN de la habitación de hotel a su red corporativa. Esta conexión le permite acceder a los recursos corporativos. Todas las comunicaciones a la red corporativa pasan a través del túnel VPN.

¿Qué ocurre si quiere conectarse a recursos de Internet? Cuando se habilita el túnel dividido, esas conexiones van directamente a Internet, no a través del túnel VPN. Algunos expertos en seguridad consideran que esto es un riesgo potencial. Recomiendan desactivar la tunelización dividida y asegurarse de que todas las conexiones, incluyendo aquellas destinadas a Internet y aquellas destinadas a los recursos corporativos, pasen por el túnel VPN. La ventaja de deshabilitar la tunelización dividida es que las conexiones a Internet deben pasar forzosamente a través de los dispositivos de seguridad de la red corporativa. Tenga en cuenta que ese no es el caso si el cliente VPN conectado a Internet estuviera fuera del túnel VPN.

Ahora volvamos a las máquinas virtuales de la instancia de Azure Virtual Network. Las rutas predeterminadas de una instancia de Azure Virtual Network permiten a las máquinas virtuales iniciar el tráfico a Internet. Esto también puede representar un riesgo de seguridad, ya que estas conexiones salientes pueden aumentar la superficie de ataque de una máquina virtual, y los atacantes podrían aprovecharla. Por este motivo, se recomienda [habilitar la tunelización forzada](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) en las máquinas virtuales cuando tiene conectividad entre locales entre la red virtual de Azure y su red local. Hablaremos acerca de la conectividad entre locales más adelante en este documento de procedimientos recomendados de redes.

Si no tiene ninguna conexión entre locales, asegúrese de que puede aprovechar los grupos de seguridad de red (descritos antes) o las aplicaciones de seguridad de la red virtual de Azure (descritas a continuación) para impedir las conexiones salientes a Internet desde sus máquinas virtuales de Azure.

## <a name="use-virtual-network-appliances"></a>Uso de aplicaciones de red virtual
Los NSG y el enrutamiento definido por el usuario pueden proporcionar un cierto grado de seguridad de red en las capas de red y de transporte del [modelo OSI](https://en.wikipedia.org/wiki/OSI_model). Aún así, es posible que en algunas situaciones quiera o necesite habilitar la seguridad en los niveles altos de la pila. En tales situaciones, se recomienda implementar aplicaciones de seguridad de la red virtual proporcionadas por asociados de Azure.

Las aplicaciones de seguridad de la red de Azure pueden proporcionar niveles de seguridad mejorados que los que proporcionan los controles de nivel de red. Las funcionalidades de seguridad de red correspondientes a los dispositivos de seguridad de la red virtual incluyen:


* Firewalls
* Detección y prevención de intrusiones
* Administración de vulnerabilidades
* Control de aplicaciones
* Detección de anomalías basadas en la red
* Filtrado de web
* Antivirus
* Protección de redes de robots (botnets)

Para buscar dispositivos de seguridad de redes virtuales de Azure, vaya a [Azure Marketplace](https://azure.microsoft.com/marketplace/) y busque mediante las palabras clave "seguridad" y "seguridad de red".

## <a name="deploy-perimeter-networks-for-security-zones"></a>Implementar redes perimetrales para las zonas de seguridad
Una [red perimetral](https://docs.microsoft.com/azure/best-practices-network-security) (también conocida como DMZ) es un segmento de red físico o lógico que está diseñado para proporcionar un nivel de seguridad adicional entre los recursos e Internet. Los dispositivos de control de acceso de red especializados que se encuentran en el borde de una red perimetral solo permiten el tráfico deseado en la red virtual.

Las redes perimetrales son útiles porque permiten centrar la administración, supervisión, registro y generación de informes sobre los dispositivos del control de acceso a la red en el borde de la instancia de Azure Virtual Network. En este caso, por lo general, debe habilitar la prevención de denegación de servicio distribuido (DDoS), los sistemas de detección y prevención de intrusiones (IDS/IPS), las reglas y directivas de firewall, el filtrado web, el antimalware de la red, etc. Los dispositivos de seguridad de la red se sitúan entre Internet y la instancia de Azure Virtual Network, y tienen una interfaz en ambas redes.

Aunque este es el diseño básico de una red perimetral, existen muchos diseños diferentes, como la configuración opuesta, el triple alojamiento o el múltiple alojamiento.

En cuanto a todas las implementaciones de alta seguridad, se recomienda tener en cuenta la posibilidad de usar una red perimetral para mejorar el nivel de seguridad de la red para los recursos de Azure.

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Uso de vínculos WAN dedicados para evitar la exposición en Internet
Muchas organizaciones han elegido la ruta de TI híbrida. En la TI híbrida, algunos de los recursos de información de la compañía están en Azure, mientras que otros siguen siendo locales. En muchos casos, algunos componentes de un servicio se ejecutan en Azure, mientras que otros componentes siguen siendo locales.

En el escenario de TI híbrida, suele haber algún tipo de conectividad entre locales. Esta conectividad entre locales permite a la empresa conectar sus redes locales con las redes virtuales de Azure. Hay dos soluciones de conectividad entre locales:

* **VPN de sitio a sitio**: es una tecnología de confianza, confiable y bien establecida, pero que realiza la conexión a través de Internet. Además, el ancho de banda está limitado a un máximo de aproximadamente 200 Mbps. La VPN de sitio a sitio es una opción conveniente en algunos escenarios y se explica más en la sección [Deshabilitar el acceso RDP/SSH a las máquinas virtuales](#disable-rdpssh-access-to-virtual-machines).
* **Azure ExpressRoute**: se recomienda que use [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para la conectividad entre locales. ExpressRoute es un vínculo de WAN dedicada entre su ubicación local o un proveedor de hospedaje de Exchange. Se trata de una conexión de telecomunicaciones debido a la cual los datos no viajan a través de Internet y, por tanto, no se exponen a los posibles riesgos inherentes a las comunicaciones de Internet.

## <a name="optimize-uptime-and-performance"></a>Optimización del rendimiento y el tiempo de actividad
Si un servicio está inactivo, no puede accederse a la información. Si el rendimiento es tan bajo que no se pueden utilizar los datos, podemos considerar que los datos son inaccesibles. Por lo tanto, desde una perspectiva de seguridad, necesitamos hacer todo lo posible para asegurarnos de que nuestros servicios tienen un rendimiento y un tiempo de actividad óptimos.

Un método popular y eficaz para mejorar la disponibilidad y el rendimiento es usar el equilibrio de carga. El equilibrio de carga es un método para distribuir el tráfico de la red entre los servidores que forman parte de un servicio. Por ejemplo, si tiene servidores web front-end que forman parte de su servicio, puede usar el equilibrio de carga para distribuir el tráfico entre ellos.

Esta distribución del tráfico aumenta la disponibilidad, ya que si uno de los servidores web deja de estar disponible, el equilibrio de carga deja de enviarle tráfico y lo redirige a los servidores que aún están en línea. El equilibrio de carga también mejora el rendimiento, ya que la sobrecarga del procesador, la red y la memoria para atender a las solicitudes se distribuye entre todos los servidores con carga equilibrada.

Se recomienda usar el equilibrio de carga siempre que se pueda y, según sea adecuado para los servicios. Estos son los escenarios en el nivel de Azure Virtual Network y el nivel global, junto con las opciones de equilibrio de carga para cada uno.

**Escenario**: ahora tiene una aplicación que:

- Requiere solicitudes de la misma sesión de usuario o cliente para llegar a la misma máquina virtual de back-end. Ejemplos de esto serían las aplicaciones del carro de la compra y los servidores de correo web.
- Como solo acepta una conexión segura, la comunicación sin cifrar con los servidores no es una opción aceptable.
- Es necesario que varias solicitudes HTTP en la misma conexión TCP de ejecución prolongada se enruten a servidores de back-end diferentes o su carga se equilibre entre estos.

**Opción de equilibrio de carga**: use [Azure Application Gateway](../application-gateway/application-gateway-introduction.md), que es un equilibrador de carga del tráfico de web HTTP. Application Gateway admite el cifrado SSL de un extremo a otro y la [terminación SSL](../application-gateway/application-gateway-introduction.md) en la puerta de enlace. A continuación, los servidores web pueden librarse de la sobrecarga de cifrado y descifrado y del tráfico que fluye sin encriptar a los servidores de back-end.

**Escenario**: es necesario equilibrar la carga de las conexiones entrantes de Internet entre los servidores situados en una instancia de Azure Virtual Network. Los escenarios surgen cuando:

- Tiene aplicaciones sin estado que acepten solicitudes entrantes de Internet.
- No se requieren sesiones temporales o descargas de SSL. Las sesiones temporales son un método que se usa con el equilibrio de carga de la aplicación, para lograr la afinidad del servidor.

**Opción de equilibrio de carga**: use Azure Portal para [crear un equilibrador de carga externo](../load-balancer/quickstart-create-basic-load-balancer-portal.md) que distribuya las solicitudes entrantes entre varias máquinas virtuales para proporcionar un mayor nivel de disponibilidad.

**Escenario**: tendrá que equilibrar la carga de las conexiones de las máquinas virtuales que no estén en Internet. En la mayoría de los casos, los dispositivos se encargan de iniciar en una instancia de Azure Virtual Network las conexiones que se aceptan para el equilibrio de carga, como instancias de SQL Server o servidores web internos.   
**Opción de equilibrio de carga**: use Azure Portal para [crear un equilibrador de carga interno](../load-balancer/quickstart-create-basic-load-balancer-powershell.md) que distribuya las solicitudes entrantes entre varias máquinas virtuales para proporcionar un mayor nivel de disponibilidad.

**Escenario**: necesita conseguir un equilibrio de carga global ya que:

- Tiene una solución en la nube que se distribuye ampliamente en varias regiones y requiere el nivel más alto de tiempo de actividad (o disponibilidad) posible.
- Necesita el nivel más alto de tiempo de actividad para asegurarse de que el servicio está disponible incluso si todo un centro de datos deja de funcionar.

**Opción de equilibrio de carga**: use Azure Traffic Manager. Traffic Manager le permite equilibrar la carga de las conexiones a los servicios, en función de la ubicación del usuario.

Por ejemplo, si el usuario realiza una solicitud a su servicio desde la Unión Europea, la conexión se dirige a los servicios situados en un centro de datos de la Unión Europea. Esta parte del equilibrio de carga global del Administrador de tráfico ayuda a mejorar el rendimiento, ya que la conexión al centro de datos más cercano es más rápida que a los centros de datos que están lejos.

## <a name="disable-rdpssh-access-to-virtual-machines"></a>Deshabilitar el acceso RDP/SSH a las máquinas virtuales
Es posible obtener acceso a Azure Virtual Machines mediante los protocolos de [Escritorio remoto](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) y [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH). Estos protocolos le permiten administrar máquinas virtuales desde ubicaciones remotas y son los protocolos estándar que se usan en el procesamiento de los centros de datos.

El posible problema de seguridad al usar estos protocolos a través de Internet, es que los atacantes pueden utilizar diversas técnicas [de fuerza bruta](https://en.wikipedia.org/wiki/Brute-force_attack) para obtener acceso a Azure Virtual Machines. Una vez que los atacantes obtienen acceso, pueden utilizar la máquina virtual como punto de inicio para poner en peligro otros equipos de la red virtual o incluso atacar dispositivos en red fuera de Azure.

Por este motivo, se recomienda deshabilitar el acceso directo de RDP y SSH a Azure Virtual Machines desde Internet. Cuando se deshabilita el acceso directo de RDP y SSH desde Internet, tiene otras opciones que puede utilizar para acceder a estas máquinas virtuales y así administrarlas de forma remota.

**Escenario**: habilite una conexión de un solo usuario a una instancia de Azure Virtual Network a través de Internet.   
**Opción**: [VPN de punto a sitio](../vpn-gateway/vpn-gateway-point-to-site-create.md) es otro término para una conexión cliente/servidor de VPN con acceso remoto. Una vez establecida la conexión de punto a sitio, el usuario podrá usar RDP o SSH para conectarse a cualquier máquina virtual situada en la instancia de Azure Virtual Network a la que el usuario se conectó mediante la VPN de punto a sitio. Con esto, se supone que el usuario tiene permiso para obtener acceso a dichas máquinas virtuales.

La VPN de punto a sitio es más segura que las conexiones de RDP o SSH directas, ya que el usuario tiene que autenticarse dos veces antes de conectarse a una máquina virtual. En primer lugar, el usuario debe autenticarse (y ser autorizado) para poder establecer la conexión VPN de punto a sitio. En segundo lugar, el usuario debe autenticarse (y ser autorizado) para poder establecer la sesión RDP o SSH.

**Escenario**: permitir que los usuarios de la red local se conecten a máquinas virtuales en Azure Virtual Network.   
**Opción**: una [VPN de sitio a sitio](../vpn-gateway/vpn-gateway-site-to-site-create.md) conecta toda una red a otra a través de Internet. Puede usar una VPN de sitio a sitio para conectar su red local a una instancia de Azure Virtual Network. Los usuarios de su red local se pueden conectar mediante el protocolo RDP o SSH, a través de la conexión VPN de sitio a sitio. No debe permitir el acceso directo de RDP o SSH a través de Internet.

**Escenario**: use un vínculo WAN dedicado para ofrecer una funcionalidad similar a la VPN de sitio a sitio.   
**Opción**: use [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/). Proporciona funcionalidades similares a la VPN de sitio a sitio. Las principales diferencias son:

- El vínculo WAN dedicado no recorre Internet.
- Los vínculos WAN dedicados suelen ser más estables y eficaces.

## <a name="secure-your-critical-azure-service-resources-to-only-your-virtual-networks"></a>Proteja los recursos de servicio de Azure críticos únicamente en las redes virtuales
Use los puntos de conexión del servicio Virtual Network para extender el espacio de direcciones privadas de la red virtual y la identidad de la red virtual a los servicios de Azure, a través de una conexión directa. Los puntos de conexión permiten proteger los recursos de servicio de Azure críticos únicamente para las redes virtuales. El tráfico desde la red virtual al servicio de Azure siempre permanece en la red troncal de Microsoft Azure.

Los puntos de conexión de servicio proporcionan las siguientes ventajas:

- **Seguridad mejorada para los recursos de servicio de Azure**: con los puntos de conexión de servicio, se pueden proteger los recursos del servicio de Azure en la red virtual. Si protege los recursos del servicio en una red virtual, mejorará la seguridad al quitar totalmente el acceso público a través de Internet a estos recursos y al permitir el tráfico solo desde la red virtual.
- **Enrutamiento óptimo para el tráfico del servicio de Azure desde la red virtual**: las rutas de la red virtual que fuerzan el tráfico de Internet a las aplicaciones virtuales o locales, un proceso conocido como tunelización forzada, también fuerzan el tráfico del servicio de Azure para que realice la misma ruta que el tráfico de Internet. Los puntos de conexión de servicio proporcionan un enrutamiento óptimo al tráfico de Azure.

  Los puntos de conexión siempre toman el tráfico del servicio directamente de la red virtual al servicio en la red troncal de Azure. Si mantiene el tráfico en la red troncal de Azure, podrá seguir auditando y supervisando el tráfico saliente de Internet desde las redes virtuales, a través de la tunelización forzada, sin que ello afecte al tráfico del servicio. Obtenga más información sobre las [rutas definidas por el usuario y la tunelización forzada](../virtual-network/virtual-networks-udr-overview.md).

- **Fácil de configurar con menos sobrecarga de administración**: ya no es necesario tener direcciones IP públicas reservadas en las redes virtuales para proteger los recursos de Azure a través del firewall IP. No hay ningún dispositivo NAT o de puerta de enlace necesario para configurar los puntos de conexión de servicio. Los puntos de conexión de servicio se pueden configurar con un simple clic en una subred. No hay sobrecarga adicional para mantener los puntos de conexión.

Para obtener más información sobre los puntos de conexión de servicio y sobre los servicios de Azure y las regiones en las que están disponibles los puntos de conexión de servicio, consulte [Puntos de conexión del servicio de Virtual Network](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="next-step"></a>Paso siguiente
Consulte los [patrones y procedimientos recomendados de seguridad de Azure](security-best-practices-and-patterns.md) para obtener más procedimientos recomendados de seguridad que podrá usar cuando diseñe, implemente y administre las soluciones en la nube al usar Azure.
