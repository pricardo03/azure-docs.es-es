---
title: archivo de inclusión
description: archivo de inclusión
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 8/13/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 5601f8d90f107636d2899a024772dccc8f75b69d
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2018
ms.locfileid: "40131066"
---
### <a name="what-is-azure-firewall"></a>¿Qué es Azure Firewall?

Azure Firewall es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de Azure Virtual Network. Se trata de un firewall como servicio con estado completo que incorpora alta disponibilidad y escalabilidad a la nube sin restricciones. Puede crear, aplicar y registrar directivas de aplicaciones y de conectividad de red a nivel central en suscripciones y redes virtuales. Azure Firewall actualmente está en su versión preliminar pública.

### <a name="what-capabilities-are-supported-in-the-azure-firewall-public-preview-release"></a>¿Qué funcionalidades admite la versión preliminar pública de Azure Firewall?  

* Firewall con estado como servicio
* Alta disponibilidad integrada con escalabilidad en la nube sin restricciones
* Filtrado de FQDN 
* Reglas de filtrado de tráfico de red
* Compatibilidad con SNAT saliente
* Creación, aplicación y registro de directivas de aplicación y de conectividad de red en las suscripciones y redes virtuales de Azure
* Totalmente integrado con Azure Monitor para registros y análisis 

### <a name="how-can-i-join-the-azure-firewall-public-preview"></a>¿Cómo me puedo unir a la versión preliminar pública de Azure Firewall?

Azure Firewall es en la actualidad una versión preliminar pública administrada a la que se puede unir utilizando el comando de PowerShell Register-AzureRmProviderFeature tal como se explica en la documentación de versión preliminar pública de Azure Firewall.

### <a name="what-is-the-pricing-for-azure-firewall"></a>¿Cuál es el precio de Azure Firewall?

Azure Firewall tiene un costo fijo y otro variable. Los precios se encuentran a continuación, a estos precios se aplica un descuento del 50 % durante la etapa de versión preliminar pública.

* Precio fijo: 1,25 USD/firewall/hora
* Tarifa variable: 0,03 USD/ GB procesado por el firewall (entrada o salida).

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>¿Cuál es el modelo de implementación típico para Azure Firewall?

Aunque Azure Firewall se puede implementar en cualquier red virtual, los clientes normalmente lo implementan en una red virtual central a la que se emparejan otras redes virtuales usando un modelo de concentrador y radio. A continuación, puede establecer la ruta predeterminada desde las redes virtuales emparejadas para que apunte a esta red virtual de Firewall central.

### <a name="how-can-i-install-the-azure-firewall"></a>¿Cómo puedo instalar Azure Firewall?

Azure Firewall se puede configurar con Azure Portal, PowerShell, API REST o con plantillas. Para instrucciones detalladas consulte [Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal](../articles/firewall/tutorial-firewall-deploy-portal.md).

### <a name="what-are-some-azure-firewall-concepts"></a>¿Cuáles son algunos de los conceptos de Azure Firewall?

Azure Firewall es compatible con las reglas y las colecciones de reglas. Una colección de reglas es una lista de reglas que comparten el mismo orden y prioridad. Las colecciones de reglas se ejecutan en su orden de prioridad, las colecciones de reglas de red tienen una prioridad más alta que las de aplicación. Todas las reglas provocan la terminación.
Existen dos tipos de colecciones de reglas:

* Reglas de aplicación: permiten configurar los nombres de dominio completo (FQDN) a los que se puede acceder desde una subred. 
* Reglas de red: permiten configurar reglas que contienen la dirección de origen, el protocolo, el puerto de destino y la dirección de destino. 

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>¿Admite Azure Firewall el filtrado del tráfico de entrada?

La versión preliminar pública de Azure Firewall admite solo el filtrado del tráfico de salida. La protección de entrada para los protocolos no HTTP/S (p. ej.: RDP, SSH, FTP) está prevista provisionalmente para proporcionarse con la disponibilidad general de Azure Firewall.  
 
### <a name="what-logginganalytics-is-supported-by-the-azure-firewall"></a>¿Qué análisis o registros son compatibles con Azure Firewall?

Azure Firewall se integra con Azure Monitor para ver y analizar los registros de Firewall. Los registros se pueden enviar a Log Analytics, Azure Storage o Event Hubs. Puede analizarse en Log Analytics o usando distintas herramientas, como Excel y Power BI. Para más detalles consulte [Tutorial: Supervisión de los registros de Azure Firewall](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-relative-to-existing-like-nvas-in-the-marketplace"></a>¿Cómo funciona Azure Firewall en relación con elementos ya existentes, como NVA en Marketplace?

Azure Firewall es un servicio de firewall básica que puede abordar determinados escenarios de cliente. Se espera que tenga una combinación de NVA de terceros y Azure Firewall. Trabajar mejor juntos es una prioridad fundamental.
 
### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>¿Cuál es la diferencia entre WAF de Application Gateway y Azure Firewall?

Firewall de aplicaciones web (WAF) es una característica de Application Gateway que proporciona a las aplicaciones una protección de entrada centralizada contra ataques y puntos vulnerables comunes. Azure Firewall proporciona protección de salida a nivel de red para todos los puertos y protocolos, y protección a nivel de aplicación para las salidas HTTP/S. La protección de entrada para los protocolos no HTTP/S (por ejemplo: RDP, SSH, FTP) está prevista provisionalmente para proporcionarse con la disponibilidad general de Azure Firewall.

### <a name="what-is-the-difference-between-network-security-groups-nsg-and-azure-firewall"></a>¿Cuál es la diferencia entre los grupos de seguridad de red (NSG) y Azure Firewall?

El servicio Azure Firewall complementa la funcionalidad de grupo de seguridad de red y juntos proporcionan una mejor seguridad de red de defensa en profundidad. Los grupos de seguridad de red proporcionan filtrado del trafico de capas de red distribuida para limitar el tráfico a los recursos dentro de las redes virtuales en cada suscripción.  Azure Firewall es un firewall de red centralizada como servicio con estado completo que proporciona protección a nivel de red y de aplicación en todas las distintas suscripciones y redes virtuales. 

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>¿Cómo puedo configurar Azure Firewall con mis puntos de conexión de servicio?

Para un acceso seguro a los servicios de PaaS, se recomiendan los puntos de conexión de servicio. Los clientes de Azure Firewall pueden habilitar puntos de conexión de servicio en la subred de Azure Firewall y deshabilitarlo en las redes virtuales de radios conectadas para de este modo beneficiarse de ambas características: seguridad de punto de conexión de servicio y registro central para todo el tráfico.

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

### <a name="what-are-the-known-service-limits"></a>¿Cuáles son los límites conocidos del servicio?

* Azure Firewall tiene un límite flexible de 1000 TB/firewall/mes. 
* La instancia de Azure Firewall que se ejecuta en una red virtual central está sujeta a limitaciones de emparejamiento de red virtual: máximo de 50 redes virtuales de radios.  
* Azure Firewall no funciona con el emparejamiento global, por lo que debe tener al menos una implementación de firewall por región.
* Azure Firewall admiten 10 000 reglas de aplicación y 10 000 reglas de red.