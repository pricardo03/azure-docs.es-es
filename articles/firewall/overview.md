---
title: ¿Qué es Azure Firewall?
description: Conozca las características de Azure Firewall.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 3/29/2019
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: 7f313af75e78db8a60fe6864c41cd8e6c5a3ad9b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "60193037"
---
# <a name="what-is-azure-firewall"></a>¿Qué es Azure Firewall?

Azure Firewall es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de Azure Virtual Network. Se trata de un firewall como servicio con estado completo que incorpora alta disponibilidad y escalabilidad a la nube sin restricciones. 

![Información general de firewalls](media/overview/firewall-threat.png)

Puede crear, aplicar y registrar directivas de aplicaciones y de conectividad de red a nivel central en suscripciones y redes virtuales. Azure Firewall usa una dirección IP pública estática para los recursos de red virtual, que permite que los firewall externos identifiquen el tráfico procedente de la red virtual.  El servicio está totalmente integrado con Azure Monitor para los registros y análisis.

## <a name="features"></a>Características

Azure Firewall ofrece las siguientes características:

### <a name="built-in-high-availability"></a>Alta disponibilidad integrada

Gracias a la alta disponibilidad integrada, no se necesita ningún equilibrador de carga adicional y no es necesario configurar nada.

### <a name="unrestricted-cloud-scalability"></a>Escalabilidad a la nube sin restricciones

Azure Firewall puede escalarse verticalmente todo lo que sea necesario para acoger los flujos de tráfico de red cambiantes, por lo que no es necesario elaborar un presupuesto para el tráfico en su momento álgido.

### <a name="application-fqdn-filtering-rules"></a>Reglas de filtrado de FQDN de aplicación

Puede limitar el tráfico HTTP/S saliente a una lista especificada de nombres de dominio completos (FQDN), lo que incluye caracteres comodín. Esta característica no requiere terminación de SSL.

### <a name="network-traffic-filtering-rules"></a>Reglas de filtrado de tráfico de red

Puede crear reglas de filtrado de red para *permitir* o *denegar* por dirección IP de origen y destino, puerto y protocolo. Azure Firewall tiene estado completo, de modo que puede distinguir los paquetes legítimos de diferentes tipos de conexiones. Las reglas se aplican y se registran en varias suscripciones y redes virtuales.

### <a name="fqdn-tags"></a>Etiquetas FQDN

Con las etiquetas FQDN, puede permitir fácilmente el conocido tráfico de red del servicio de Azure a través del firewall. Por ejemplo, supongamos que quiere permitir el tráfico de red de Windows Update a través del firewall. Puede crear una regla de aplicación e incluir la etiqueta de Windows Update. Ahora, el tráfico de red de Windows Update puede fluir a través del firewall.

### <a name="service-tags"></a>Etiquetas de servicio

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad. No puede crear su propia etiqueta de servicio, ni especificar qué direcciones IP se incluyen dentro de una etiqueta. Microsoft administra los prefijos de direcciones que incluye la etiqueta de servicio y actualiza automáticamente esta a medida que las direcciones cambian.

### <a name="threat-intelligence"></a>Información sobre amenazas

El filtrado basado en inteligencia sobre amenazas puede habilitarse para que el firewall alerte y deniegue el tráfico desde y hacia los dominios y las direcciones IP malintencionados. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft.

### <a name="outbound-snat-support"></a>Compatibilidad con SNAT saliente

Todas las direcciones IP de tráfico de red virtual salientes se convierten a direcciones IP públicas de Azure Firewall (traducción de direcciones de red de origen). Puede identificar y permitir el tráfico que se origina en la red virtual y se dirige a los destinos de Internet remotos.

### <a name="inbound-dnat-support"></a>Compatibilidad con DNAT entrante

El tráfico de red entrante a la dirección IP pública de firewall se traduce (traducción de direcciones de red de destino) y se filtra para las direcciones IP privadas en las redes virtuales. 

### <a name="azure-monitor-logging"></a>Registro de Azure Monitor

Todos los eventos se integran en Azure Monitor, lo que permite archivar registros en una cuenta de almacenamiento, transmitir eventos al centro de eventos o enviarlos a los registros de Azure Monitor.

## <a name="known-issues"></a>Problemas conocidos

Azure Firewall presenta los siguientes problemas conocidos:

|Problema  |DESCRIPCIÓN  |Mitigación  |
|---------|---------|---------|
|Conflicto con la característica Just-in-Time (JIT) de Azure Security Center (ASC)|Si se accede a una máquina virtual mediante JIT, y está en una subred con una ruta definida por el usuario que apunta a Azure Firewall como puerta de enlace predeterminada, JIT de ASC no funcionará. Este es el resultado de un enrutamiento asimétrico: un paquete entra a través de la dirección IP pública de la máquina virtual (JIT abrió el acceso), pero la ruta devuelta se genera a través del firewall, que rechaza el paquete porque no se ha establecido ninguna sesión en el firewall.|Para solucionar este problema, coloque las máquinas virtuales JIT en una subred independiente que no tenga una ruta definida por el usuario al firewall.|
Las reglas de filtrado de red para protocolos que no son TCP/UDP (por ejemplo, ICMP) no funcionan con el tráfico enlazado a Internet|Las reglas de filtrado de red de protocolos que no son TCP/UDP no funcionan con la traducción SNAT a la dirección IP pública. Los protocolos que no son TCP/UDP no se admiten entre subredes de radio y redes virtuales.|Azure Firewall usa Standard Load Balancer, [que actualmente no admite SNAT para los protocolos IP](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#limitations). Se están examinando opciones para admitir este escenario en una versión futura.|
|Falta de compatibilidad entre PowerShell y CLI con ICMP|Azure PowerShell y CLI no admiten ICMP como protocolo válido en las reglas de red.|Aun así se puede usar ICMP como protocolo a través del portal y la API REST. Estamos trabajando para agregar pronto ICMP a PowerShell y CLI.|
|Las etiquetas FQDN requieren que se establezca una combinación protocolo: puerto|Las reglas de aplicaciones con las etiquetas FQDN requieren la definición de puerto: protocolo.|Puede usar **https** como valor de puerto: protocolo. Estamos trabajando para que este campo sea opcional cuando se usen etiquetas FQDN.|
|No se admite la posibilidad de mover un firewall a otro grupo de recursos o suscripción.|No se admite la posibilidad de mover un firewall a otro grupo de recursos o suscripción.|La compatibilidad con esta funcionalidad está en nuestro mapa de ruta. Para mover un firewall a otro un grupo de recursos o suscripción, debe eliminar la instancia actual y volver a crearla en el nuevo grupo de recursos o suscripción.|
|Intervalo de puertos en reglas de red y aplicación|Los puertos están limitados a 64.000, ya que los puertos altos están reservados para la administración y los sondeos de estado. |Estamos trabajando para reducir esta limitación.|
|Las alertas de inteligencia sobre amenazas pueden estar enmascaradas|Reglas de red con destino 80/443 para las alertas de inteligencia sobre amenazas de las máscaras de filtrado de salida cuando la configuración está establecida en el modo de solo alerta.|Cree filtrado de salida para 80/443 mediante reglas de aplicaciones. También puede cambiar el modo de inteligencia sobre amenazas a **Alertar y denegar**.|
|Azure Firewall usa Azure DNS para la resolución de nombres|Azure Firewall resuelve los FQDN solo mediante Azure DNS. No se admite un servidor DNS personalizado. No hay ningún impacto en la resolución de DNS de otras subredes.|Estamos trabajando para reducir esta limitación.
## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md)
- [Implementación de Azure Firewall mediante una plantilla](deploy-template.md)
- [Creación de un entorno de prueba de Azure Firewall](scripts/sample-create-firewall-test.md)