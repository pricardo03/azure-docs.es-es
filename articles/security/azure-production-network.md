---
title: Red de producción de Azure
description: En este artículo se proporciona una descripción general de la red de producción de Microsoft Azure.
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
ms.openlocfilehash: 5c0bfae35464e73278a1efd9c04a03123bb9018a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101843"
---
# <a name="azure-production-network"></a>Red de producción de Azure
Los usuarios de la red de producción de Azure incluyen clientes externos que acceden a sus propias aplicaciones de Microsoft Azure, así como personal interno de soporte técnico de Microsoft Azure que administra la red de producción. En este artículo se describen los métodos de acceso de seguridad y los mecanismos de protección para establecer conexiones a la red de producción de Azure.

## <a name="internet-routing-and-fault-tolerance"></a>Enrutamiento de Internet y tolerancia a errores
Una infraestructura de Microsoft Azure Domain Name Service (WADNS) interna y externa globalmente redundante, junto con varios clústeres de servidor del Servicio de nombres de dominio (DNS) principales y secundarios proporcionan tolerancia a errores, mientras que controles de seguridad de red de Microsoft Azure adicionales, como NetScaler, se usan para evitar ataques de denegación de servicio distribuido (DDoS) y proteger la integridad de los servicios de Microsoft Azure DNS.

Los servidores WADNS se encuentran en varios centros de datos. La implementación de WADNS incorpora una jerarquía de servidores DNS principales o secundarios para resolver públicamente los nombres de dominio de cliente de Azure. Normalmente, los nombres de dominio se resuelven en una dirección CloudApp.net, que encapsula la dirección IP Virtual (VIP) para el servicio del cliente. Exclusiva de Azure, la VIP correspondiente a la dirección IP dedicada (DIP) interna de la traducción del inquilino se realiza mediante los equilibradores de carga de Microsoft responsables de dicha VIP.

Azure se hospeda en centros de datos de Azure distribuidos geográficamente en Estados Unidos y se basa en plataformas de enrutamientos de última generación que implementan estándares de arquitectura sólidos y escalables. Algunas de las características destacadas son:

- Ingeniería de tráfico basada en conmutación de etiquetas multiprotocolo (MPLS) que proporciona un uso eficaz de los vínculos y degradación correcta del servicio si se produce una interrupción.
- Las redes se implementan con arquitecturas de redundancia de "necesidad más uno" (N+1) o superior.
- Externamente, los centros de datos funcionan con circuitos dedicados de red de alto ancho de banda que conectan de manera redundante las propiedades con más de 1200 proveedores de servicios de Internet globalmente en varios puntos de emparejamiento que proporcionan más de 2000 gigabytes por segundo (Gbps) de capacidad de borde.

Dado que Microsoft posee sus propios circuitos de red entre los centros de datos, estos atributos ayudan a la oferta de Azure a lograr más de un 99,9 % de disponibilidad de red sin necesidad de proveedores externos de servicios de Internet tradicionales.

## <a name="connection-to-production-network-and-associated-firewalls"></a>Conexión a la red de producción y los firewalls asociados
La directiva de flujo de tráfico de Internet de la red de Azure dirige el tráfico a la red de producción de Azure ubicada en el centro de datos regional más cercano dentro de Estados Unidos. Puesto que los centros de datos de producción de Azure mantienen una arquitectura de red y hardware coherentes, la siguiente descripción del flujo de tráfico se aplica de manera uniforme a todos los centros de datos.

Una vez que el tráfico de Internet para Azure se enruta al centro de datos más cercano, se establece una conexión a los enrutadores de acceso. Estos enrutadores de acceso sirven para aislar el tráfico entre los nodos de Azure y las VM con instancias de cliente. Los dispositivos de la infraestructura de red en las ubicaciones de acceso y el borde son los puntos limítrofes donde se aplican los filtros de entrada y salida. Estos enrutadores se configuran a través de una ACL en capas para filtrar el tráfico de red no deseado y aplicar límites de velocidad de tráfico, si es necesario. El tráfico permitido por ACL se enruta a los equilibradores de carga. Los enrutadores de distribución están diseñados para permitir solo las direcciones IP aprobadas por Microsoft, proporcionar protección contra suplantación de identidad y conexiones TCP establecidas mediante ACL.

Dispositivos de equilibrio de carga externos se encuentran detrás de los enrutadores de acceso para realizar la traducción de direcciones de red (NAT) de direcciones IP enrutables por Internet a direcciones IP interna de Azure. También enrutan paquetes a puertos y direcciones IP de producción interna válidos, y funcionan como mecanismo de protección para limitar la exposición del espacio de direcciones de red de producción interna.

De manera predeterminada, Microsoft aplica el Protocolo de transferencia de hipertexto con cifrado de Capa de sockets seguros (HTTPS) para todo el tráfico que se transmite a los navegadores web del cliente, incluidos los inicios de sesión y todo el tráfico posterior. El uso de TLS v1.2 permite un túnel seguro para que el tráfico fluya. Las ACL en enrutadores de acceso y backbone aseguran que el origen del tráfico sea coherente con lo que se espera.

Una diferencia importante en esta arquitectura en comparación con la arquitectura de seguridad tradicional es que hay no hay firewalls de hardware dedicados, dispositivos especializados de detección y prevención de intrusiones, ni otros dispositivos de seguridad que se encuentran normalmente antes de que se establezcan las conexiones con el entorno de producción de Azure. Habitualmente, los clientes esperan estos dispositivos de firewall de hardware en la red de Azure; sin embargo, no se emplea ninguno en Azure. Casi exclusivamente, estas características de seguridad están integradas en el software que se ejecuta el entorno de Azure para proporcionar mecanismos sólidos de seguridad de varias capas, incluidas funcionalidades de firewall. Además, el ámbito de los límites y la expansión asociada de dispositivos de seguridad críticos es más fácil de administrar e inventariar, tal como se muestra en la ilustración anterior, ya que está administrado por el software que se ejecuta en Azure.

## <a name="core-security-and-firewall-features"></a>Características principales de seguridad y firewall
Azure implementa sólidas características de seguridad y firewall de software en varios niveles para aplicar las características de seguridad que se esperan normalmente en un entorno tradicional para proteger el límite de autorización de seguridad principal.

### <a name="azure-security-features"></a>Características de seguridad de Azure
Azure implementa firewalls de software basados en host dentro de la red de producción. Varias características principales de seguridad y firewall residen dentro del entorno principal de Azure. Estas características de seguridad reflejan una estrategia de defensa en profundidad en el entorno de Azure. Los datos del cliente en Microsoft Azure están protegidos por los firewalls siguientes:

**Firewall de hipervisor (filtro de paquetes)**: este firewall se implementa en el hipervisor y se configura mediante un agente de controlador de tejido (FC). Este firewall protege al inquilino que se ejecuta dentro de la VM frente al acceso no autorizado. De forma predeterminada, cuando se crea una VM, se bloquea todo el tráfico y, a continuación, el agente de FC agrega reglas y excepciones al filtro para permitir el tráfico autorizado.

Existen dos categorías de reglas que se programan aquí:

- Reglas de configuración de la máquina o la infraestructura: de forma predeterminada, se bloquea toda la comunicación. Existen excepciones para permitir que una VM envíe y reciba comunicaciones del Protocolo de configuración dinámica de host (DHCP), información de DNS, envíe tráfico a Internet "pública", saliente a otras VM en el clúster de FC y el servidor de activación del sistema operativo. Dado que la lista de destinos de salida permitidos de VM no incluye subredes de enrutador y otras propiedades de Microsoft Azure, las reglas funcionan como capa de defensa.
- Archivo de configuración de roles: define las ACL de entrada según el modelo de servicio de los inquilinos. Por ejemplo, si un inquilino tiene un front-end web en el puerto 80 en una determinada VM, el puerto 80 está abierto a todas las direcciones IP. Si la VM tiene un rol de trabajo en ejecución, rol de trabajo está abierto únicamente para la VM dentro del mismo inquilino.

**Firewall de host nativo**: el tejido y el almacenamiento de Microsoft Azure se ejecutan en un sistema operativo nativo que no tiene hipervisor y, por tanto, el Firewall de Windows se configura con los dos conjuntos de reglas anteriores.

**Firewall de host**: el firewall del host protege la partición del host, que ejecuta el hipervisor. Las reglas se programan para permitir que solo el FC y los cuadros de salto se comuniquen con la partición del host en un puerto específico. Las otras excepciones son para permitir la respuesta DHCP y las respuestas DNS. Azure usa un archivo de configuración de la máquina, que tiene la plantilla de reglas de firewall para la partición del host. También hay una excepción de firewall de host que permite que las VM se comuniquen con los componentes del host, el servidor de conexión y el servidor de metadatos, a través de protocolos y puertos específicos.

**Firewall de invitado**: parte del Firewall de Windows del sistema operativo invitado, que el cliente puede configurar en las VM y en el almacenamiento del cliente.

Características adicionales de seguridad integradas en las funcionalidades de Azure:

- A los componentes de infraestructura se les asignan direcciones IP que son de IP dedicadas (DIP). Un atacante en Internet no puede dirigir tráfico a esas direcciones porque no podrían comunicarse con Microsoft. Los enrutadores de la puerta de enlace de Internet filtran los paquetes destinados únicamente a las direcciones internas, por lo que no ingresarían a la red de producción. Los únicos componentes que acepten el tráfico dirigido a VIP son los equilibradores de carga.
- Los firewalls que se implementa en todos los nodos internos tienen tres consideraciones sobre la arquitectura principal de seguridad para un escenario determinado:

   - Se colocan detrás del equilibrador de carga (LB) y aceptan paquetes desde cualquier lugar. Están diseñados para ser expuestos al exterior y corresponden a los puertos abiertos en un firewall perimetral tradicional.
   - Solo aceptan paquetes de un conjunto limitado de direcciones. Esto es parte de la estrategia de defensa exhaustiva contra ataques por denegación de servicio. Dichas conexiones se autentican criptográficamente.
   - Solo es posible acceder a los firewalls desde los nodos internos seleccionados, en cuyo caso, aceptan paquetes solo de una lista enumerada de direcciones IP de origen, que son todas DIP dentro de la red de Azure. Por ejemplo, un ataque en la red corporativa podría dirigir las solicitudes a estas direcciones, pero se bloquearía, a menos que la dirección de origen del paquete fuera una de las incluidas en la lista enumerada dentro de la red de Azure.
   - El enrutador de acceso en el perímetro bloquea los paquetes salientes dirigidos a una dirección que está dentro de la red de Azure, debido a sus rutas estáticas configuradas.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre lo que hace Microsoft para proteger la infraestructura de Azure, consulte:

- [Azure facilities, premises, and physical security](azure-physical-security.md) (Instalaciones de Azure, locales y seguridad física)
- [Availability of Azure infrastructure](azure-infrastructure-availability.md) (Disponibilidad de la infraestructura de Azure)
- [Componentes y límites del sistema de información de Azure](azure-infrastructure-components.md)
- [Arquitectura de red de Azure](azure-infrastructure-network.md)
- [Características de seguridad de Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](azure-infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure](azure-infrastructure-integrity.md) (Integridad de la infraestructura de Azure)
- [Protección de datos del cliente en Azure](azure-protection-of-customer-data.md)
