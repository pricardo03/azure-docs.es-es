---
title: archivo de inclusión
description: archivo de inclusión
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 10/15/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 1d52c9b3b9b91873bd6f34443f162c74d724a65c
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49367017"
---
### <a name="what-is-azure-firewall"></a>¿Qué es Azure Firewall?

Azure Firewall es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de Azure Virtual Network. Se trata de un firewall como servicio con estado completo que incorpora alta disponibilidad y escalabilidad a la nube sin restricciones. Puede crear, aplicar y registrar directivas de aplicaciones y de conectividad de red a nivel central en suscripciones y redes virtuales.

### <a name="what-capabilities-are-supported-in-azure-firewall"></a>¿Qué funcionalidades son compatibles con Azure Firewall?

* Firewall con estado como servicio
* Alta disponibilidad integrada con escalabilidad en la nube sin restricciones
* Filtrado de FQDN
* Etiquetas FQDN
* Reglas de filtrado de tráfico de red
* Compatibilidad con SNAT saliente
* Compatibilidad con DNAT entrante
* Creación, aplicación y registro de directivas de aplicación y de conectividad de red en las suscripciones y redes virtuales de Azure
* Totalmente integrado con Azure Monitor para registros y análisis

### <a name="what-is-the-pricing-for-azure-firewall"></a>¿Cuál es el precio de Azure Firewall?

Azure Firewall tiene un costo fijo y otro variable:

* Precio fijo: 1,25 USD/firewall/hora
* Tarifa variable: USD 0,03/GB procesado por el firewall (entrada o salida)

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>¿Cuál es el modelo de implementación típico para Azure Firewall?

Puede implementar Azure Firewall en cualquier red virtual, pero los clientes habitualmente lo implementan en una red virtual central y lo emparejan con otras redes virtuales en un modelo de concentrador y radio. A continuación, puede establecer la ruta predeterminada desde las redes virtuales emparejadas para que apunte a esta red virtual de firewall central.

### <a name="how-can-i-install-the-azure-firewall"></a>¿Cómo puedo instalar Azure Firewall?

Puede configurar Azure Firewall mediante Azure Portal, PowerShell, API REST o a través del uso de plantillas. Para instrucciones detalladas consulte [Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal](../articles/firewall/tutorial-firewall-deploy-portal.md).

### <a name="what-are-some-azure-firewall-concepts"></a>¿Cuáles son algunos de los conceptos de Azure Firewall?

Azure Firewall es compatible con las reglas y las colecciones de reglas. Una colección de reglas es una lista de reglas que comparten el mismo orden y prioridad. Las colecciones de reglas se ejecutan en su orden de prioridad. Las colecciones de reglas de red tienen mayor prioridad que las colecciones de reglas de aplicación y se finalizan todas las reglas.

Existen dos tipos de colecciones de reglas:

* *Reglas de aplicación*: permiten configurar los nombres de dominio completos (FQDN) a los que se puede acceder desde una subred.
* *Reglas de red*: permiten configurar reglas que contienen direcciones de origen, protocolos, puertos de destino y direcciones de destino.

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>¿Admite Azure Firewall el filtrado del tráfico de entrada?

Azure Firewall es compatible con el filtrado entrante y saliente. La protección entrante es para los protocolos que no son HTTP/S. Por ejemplo, los protocolos RDP, SSH y FTP.

### <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>¿Qué servicios de registro y análisis son compatibles con Azure Firewall?

Azure Firewall se integra con Azure Monitor para ver y analizar los registros de firewall. Los registros se pueden enviar a Log Analytics, Azure Storage o Event Hubs. Puede analizarse en Log Analytics o usando distintas herramientas, como Excel y Power BI. Para más información, consulte [Tutorial: Supervisión de los registros de Azure Firewall](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>¿Cómo Azure Firewall funciona distinto de servicios existentes como NVA que ya están en el mercado?

Azure Firewall es un servicio de firewall básica que puede abordar determinados escenarios de cliente. Se espera que tenga una combinación de NVA de terceros y Azure Firewall. Trabajar mejor juntos es una prioridad fundamental.

### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>¿Cuál es la diferencia entre WAF de Application Gateway y Azure Firewall?

Firewall de aplicaciones web (WAF) es una característica de Application Gateway que proporciona a las aplicaciones una protección de entrada centralizada contra ataques y puntos vulnerables comunes. Azure Firewall proporciona protección de entrada para protocolos no HTTP/S (como RDP, SSH y FTP), protección de salida a nivel de red para todos los puertos y protocolos, y protección a nivel de aplicación para las salidas HTTP/S.

### <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>¿Cuál es la diferencia entre los grupos de seguridad de red (NSG) y Azure Firewall?

El servicio Azure Firewall complementa la funcionalidad de grupo de seguridad de red. Juntos proporcionan una mejor seguridad de red de "defensa en profundidad". Los grupos de seguridad de red proporcionan filtrado del trafico de capas de red distribuida para limitar el tráfico a los recursos dentro de las redes virtuales en cada suscripción. Azure Firewall es un firewall de red centralizada como servicio con estado completo que proporciona protección a nivel de red y de aplicación en todas las distintas suscripciones y redes virtuales.

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>¿Cómo puedo configurar Azure Firewall con mis puntos de conexión de servicio?

Para un acceso seguro a los servicios de PaaS, se recomiendan los puntos de conexión de servicio. Puede elegir habilitar puntos de conexión de servicio en la subred de Azure Firewall y deshabilitarlos en las redes virtuales de radios conectadas. De este modo se beneficia de ambas características: la seguridad de los puntos de conexión y el registro central de todo el tráfico.

### <a name="how-can-i-stop-and-start-azure-firewall"></a>¿Cómo puedo detener e iniciar Azure Firewall?

Puede usar los métodos *deallocate* y *allocate* de Azure PowerShell.

Por ejemplo: 

```azurepowershell
# Stop an exisitng firewall

$azfw = Get-AzureRmFirewall -Name "FW Name” -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzureRmFirewall -AzureFirewall $azfw
```

```azurepowershell
#Start a firewall

$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzureRmPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzureRmFirewall -AzureFirewall $azfw
```

> [!NOTE]
> Debe reasignar un firewall y una dirección IP pública al grupo de recursos y la suscripción originales.

### <a name="what-are-the-known-service-limits"></a>¿Cuáles son los límites conocidos del servicio?

* Azure Firewall tiene un límite flexible de 1000 TB por firewall por mes.
* Una instancia de Azure Firewall que se está ejecutando en una red virtual central tiene limitaciones de emparejamiento de red virtual, con un máximo de 50 redes virtuales de radio.  
* Azure Firewall no funciona con el emparejamiento global, por lo que debe tener al menos una implementación de firewall por región.
* Azure Firewall admite 10 000 reglas de aplicación y 10 000 reglas de red.

### <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>¿Azure Firewall en la red virtual de un concentrador puede reenviar y filtrar el tráfico de red entre dos redes virtuales de radio?

Sí, puede usar Azure Firewall en la red virtual de un concentrador para enrutar y filtrar el tráfico de red entre dos redes virtuales de radio. Las subredes de cada una de las redes virtuales de radio deben tener una ruta definida por el usuario (UDR) que apunte a la instancia de Azure Firewall como puerta de enlace predeterminada para que este escenario funcione correctamente.

### <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network"></a>¿Azure Firewall puede reenviar y filtrar el tráfico de red entre subredes de la misma red virtual?

El tráfico entre subredes de la misma red virtual o de una red virtual emparejada directamente se enruta de forma directa, incluso si la UDR apunta a la instancia de Azure Firewall como puerta de enlace predeterminada. El método recomendado para la segmentación de redes internas es utilizar grupos de seguridad de red. Para enviar tráfico de subred a subred al firewall en este escenario, el UDR debe contener el prefijo de red de la subred de destino de forma explícita en ambas subredes.

### <a name="are-there-any-firewall-resource-group-restrictions"></a>¿Existe alguna restricción de grupo de recursos de firewall?

Sí. El firewall, la subred, la red virtual y la dirección IP pública deben estar en el mismo grupo de recursos.
