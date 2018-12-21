---
title: Introducción a los grupos de seguridad de Azure | Microsoft Docs
description: Obtenga información sobre los grupos de seguridad de red y de aplicación. Los grupos de seguridad le ayudan a filtrar el tráfico de red entre los recursos de Azure.
services: virtual-network
documentationcenter: na
author: jimdial
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2018
ms.author: jdial
ms.openlocfilehash: ad5205bcf063cd3375e68ef609c71ced2f551b61
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53311769"
---
# <a name="security-groups"></a>Grupos de seguridad
<a name="network-security-groups"></a>

Puede filtrar el tráfico de red hacia y desde los recursos de Azure en una [red virtual](virtual-networks-overview.md) de Azure con un grupo de seguridad de red. Un grupo de seguridad de red contiene [reglas de seguridad](#security-rules) que permiten o deniegan el tráfico de red entrante o el tráfico de red saliente de varios tipos de recursos de Azure. Para obtener información acerca de qué recursos de Azure se pueden implementar en una red virtual y pueden tener grupos de seguridad de red asociados, consulte [Integración de redes virtuales para los servicios de Azure](virtual-network-for-azure-services.md). Para cada regla, puede especificar un origen y destino, un puerto y un protocolo.

En este artículo se explican los conceptos de los grupos de seguridad de red para ayudarle a usarlos de manera eficaz. Si nunca ha creado un grupo de seguridad de red, puede seguir un [tutorial](tutorial-filter-network-traffic.md) rápido para obtener alguna experiencia en la creación de uno. Si está familiarizado con los grupos de seguridad de red y necesita administrarlos, consulte [Administración de un grupo de seguridad de red](manage-network-security-group.md). Si tiene problemas con las comunicaciones y necesita solucionar problemas de los grupos de seguridad de red, consulte [Diagnóstico de un problema de filtro de tráfico de red de una máquina virtual](diagnose-network-traffic-filter-problem.md). Puede habilitar los [registros de flujo de los grupos de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) para [analizar el tráfico de red](../network-watcher/traffic-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hacia y desde los recursos que tienen un grupo de seguridad de red asociado.

## <a name="security-rules"></a>Reglas de seguridad

Un grupo de seguridad de red puede contener cero reglas, o tantas reglas como desee, siempre que esté dentro de los [límites](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) de la suscripción de Azure. Cada regla especifica las siguientes propiedades:

|Propiedad  |Explicación  |
|---------|---------|
|NOMBRE|Un nombre único dentro del grupo de seguridad de red.|
|Prioridad | Un número entre 100 y 4096. Las reglas se procesan en orden de prioridad. Se procesan primero las reglas con los números más bajos ya que estos tienen más prioridad. Si el tráfico coincide con una regla, se detiene el procesamiento. Como resultado, las reglas con menor prioridad (números más altos) que tengan los mismos atributos que las reglas con una prioridad mayor no se procesarán.|
|Origen o destino| Cualquiera, una dirección IP individual, un bloque CIDR de enrutamiento entre dominios sin clases (10.0.0.0/24, por ejemplo), una [etiqueta de servicio](#service-tags) o un [grupo de seguridad de aplicaciones](#application-security-groups). Si especifica una dirección para un recurso de Azure, especifique la dirección IP privada asignada al recurso. Las grupos de seguridad de red se procesan después de que Azure traduzca una dirección IP pública a una dirección IP privada para el tráfico de entrada y antes de que Azure traduzca una dirección IP privada a una dirección IP pública para el tráfico de salida. Más información sobre [direcciones IP](virtual-network-ip-addresses-overview-arm.md) de Azure. La especificación de un intervalo, una etiqueta de servicio o grupo de seguridad de aplicaciones le permite crear menos reglas de seguridad. La posibilidad de especificar varias direcciones IP individuales e intervalos (no puede especificar varias etiquetas de servicio ni grupos de aplicaciones) en una regla se conoce como [reglas de seguridad aumentada](#augmented-security-rules). Las reglas de seguridad aumentada solo se pueden generar en los grupos de seguridad de red creados mediante el modelo de implementación de Resource Manager. No puede especificar varias direcciones IP ni intervalos de ellas en grupos de seguridad de red creados mediante el modelo de implementación clásica. Más información acerca de los [modelos de implementación de Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|Protocolo     | TCP, UDP o Cualquiera, opción que incluye TCP, UDP e ICMP. No se puede especificar ICMP de forma independiente; si necesita usarlo, utilice la opción Cualquiera. |
|Dirección| Si la regla se aplica al tráfico entrante o al saliente.|
|Intervalo de puertos     |Puede especificar un puerto individual o un intervalo de puertos. Por ejemplo, puede especificar 80 o 10000-10005. La especificación de intervalos le permite crear menos reglas de seguridad. Las reglas de seguridad aumentada solo se pueden generar en los grupos de seguridad de red creados mediante el modelo de implementación de Resource Manager. No puede especificar varios puertos ni intervalos de ellos en la misma regla de seguridad de los grupos de seguridad de red creados mediante el modelo de implementación clásica.   |
|.     | Permitir o denegar        |

Las reglas de seguridad de los grupos de seguridad de red se evalúan por prioridad mediante información en tuplas de 5 elementos (origen, puerto de origen, destino, puerto de destino y protocolo) para permitir o denegar el tráfico. Se crea un registro de flujo para las conexiones existentes. Se permite o deniega la comunicación en función del estado de conexión del registro de flujo. El registro de flujo permite que un grupo de seguridad de red sea con estado. Por ejemplo, si especifica una regla de seguridad de salida para cualquier dirección a través del puerto 80, no será necesario especificar una regla de seguridad de entrada para la respuesta al tráfico saliente. Solo debe especificar una regla de seguridad de entrada si la comunicación se inicia de forma externa. Lo contrario también es cierto. Si se permite el tráfico entrante a través de un puerto, no es necesario especificar una regla de seguridad de salida para responder al tráfico a través del puerto.
No es posible interrumpir las conexiones existentes cuando se elimina una regla de seguridad que habilitó el flujo. Los flujos de tráfico se interrumpen cuando se detienen las conexiones y no fluye ningún tráfico en ambas direcciones durante al menos unos minutos.

Hay límites en el número de reglas de seguridad que puede crear en un grupo de seguridad de red. Para más información, consulte el artículo acerca de los [límites de Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="augmented-security-rules"></a>Reglas de seguridad aumentada

Las reglas de seguridad aumentada permiten simplificar la definición de seguridad para las redes virtuales, lo que le permitirá definir directivas de seguridad de red más grandes y complejas, con menos reglas. Puede combinar varios puertos y varias direcciones IP explícitas e intervalos en una única regla de seguridad de fácil comprensión. Use reglas aumentadas en los campos de origen, destino y puerto de una regla. Para simplificar el mantenimiento de la definición de la regla de seguridad, combine las reglas de seguridad aumentada con [etiquetas de servicio](#service-tags) o [grupos de seguridad de aplicaciones](#application-security-groups). Hay límites en el número de direcciones, intervalos y puertos que se pueden especificar en una regla. Para más información, consulte el artículo acerca de los [límites de Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="service-tags"></a>Etiquetas de servicio

 Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad. No puede crear su propia etiqueta de servicio, ni especificar qué direcciones IP se incluyen dentro de una etiqueta. Microsoft administra los prefijos de direcciones que incluye la etiqueta de servicio y actualiza automáticamente esta a medida que las direcciones cambian. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. 
 
 Puede descargar e integrar con un firewall local la lista de etiquetas de servicio con información de prefijos en las siguientes publicaciones semanales de las nubes [pública](https://www.microsoft.com/download/details.aspx?id=56519), [gobierno de Estados Unidos](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) y [Alemania](https://www.microsoft.com/download/details.aspx?id=57064).

 Las siguientes etiquetas de servicio están disponibles para su uso en la definición de reglas de seguridad. Sus nombres pueden variar ligeramente según el [modelo de implementación de Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

* **VirtualNetwork** (Resource Manager) (**VIRTUAL_NETWORK** para el modelo clásico): esta etiqueta incluye el espacio de direcciones de red virtual (todos los intervalos CIDR definidos para la red virtual), todos los espacios de direcciones locales conectados y las redes virtuales [del mismo nivel](virtual-network-peering-overview.md) o redes virtuales conectadas a una [puerta de enlace de red virtual](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** para el modelo clásico): esta etiqueta predeterminada denota el equilibrador de carga de la infraestructura de Azure. La etiqueta se traduce en la [dirección IP virtual del host](security-overview.md##azure-platform-considerations) (168.63.129.16) donde se originan los sondeos de mantenimiento de Azure. Si no usa el equilibrador de carga de Azure, puede reemplazar esta regla.
* **Internet** (Resource Manager) (**INTERNET** para el modelo clásico): esta etiqueta predeterminada denota el espacio de direcciones IP que se encuentra fuera de la red virtual y es accesible a través de la red pública de Internet. El intervalo de direcciones incluye el [espacio de direcciones IP públicas propiedad de Azure](https://www.microsoft.com/download/details.aspx?id=41653).
* **AzureCloud** (solo Resource Manager): esta etiqueta denota el espacio de direcciones IP de Azure e incluye todas las [direcciones IP públicas del centro de datos](https://www.microsoft.com/download/details.aspx?id=41653). Si especifica *AzureCloud* como valor, el tráfico a las direcciones IP públicas de Azure se permite o deniega. Si solo desea permitir el acceso a AzureCloud en una [región](https://azure.microsoft.com/regions) determinada, puede especificarla. Por ejemplo, si desea permitir el acceso a AzureCloud solo en la región este de EE. UU., puede especificar *AzureCloud.EastUS* como etiqueta de servicio. 
* **AzureTrafficManager** (solo Resource Manager): esta etiqueta denota el espacio de direcciones IP de las direcciones IP de sondeo de Azure Traffic Manager. En [Preguntas más frecuentes sobre Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs), puede encontrar más información acerca de las direcciones IP de sondeo de Azure Traffic Manager. 
* **Storage** (solo Resource Manager): esta etiqueta denota el espacio de direcciones IP del servicio Azure Storage. Si especifica *Storage* como valor, el tráfico al almacenamiento se permite o se deniega. Si solo desea permitir el acceso al almacenamiento de una determinada [región](https://azure.microsoft.com/regions), puede especificarla. Por ejemplo, si desea permitir el acceso a Azure Storage solo en la región este de EE. UU., puede especificar *Storage.EastUS* como etiqueta de servicio. La etiqueta representa el servicio, no instancias específicas del mismo. Por ejemplo, la etiqueta representa el servicio Azure Storage, pero no una cuenta de específica de este. Todos los prefijos de dirección representados por esta etiqueta también se representan mediante la etiqueta **Internet**. 
* **Sql** (solo Resource Manager): esta etiqueta denota los prefijos de direcciones de los servicios Azure SQL Database y Azure SQL Data Warehouse. Si especifica *Sql* como valor, el tráfico a Sql se permite o se deniega. Si solo desea permitir el acceso a SQL en una [región](https://azure.microsoft.com/regions) determinada, puede especificarla. Por ejemplo, si desea permitir el acceso a Azure SQL Database solo en la región este de EE. UU., puede especificar *Sql.EastUS* como etiqueta de servicio. La etiqueta representa el servicio, no instancias específicas del mismo. Por ejemplo, la etiqueta representa el servicio Azure SQL Database, pero no una cuenta de un servidor o base de datos SQL específicos. Todos los prefijos de dirección representados por esta etiqueta también se representan mediante la etiqueta **Internet**. 
* **AzureCosmosDB** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Cosmos Database. Si especifica *AzureCosmosDB* como valor, el tráfico a AzureCosmosDB se permite o deniega. Si solo desea permitir el acceso a AzureCosmosDB en una [región](https://azure.microsoft.com/regions), específica, puede especificar la región en el siguiente formato AzureCosmosDB.[nombre de región]. 
* **AzureKeyVault** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Key Vault. Si especifica *AzureKeyVault* como valor, el tráfico a AzureKeyVault se permite o deniega. Si solo desea permitir el acceso a AzureKeyVault en una [región](https://azure.microsoft.com/regions), específica, puede especificar la región en el siguiente formato AzureKeyVault.[nombre de región]. 
* **EventHub** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Event Hubs. Si especifica *EventHub* como valor, el tráfico a EventHub se permite o se deniega. Si solo desea permitir el acceso a EventHub en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región en el siguiente formato: EventHub.[nombre de región]. 
* **ServiceBus** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Service Bus. Si especifica *ServiceBus* como valor, el tráfico a ServiceBus se permite o se deniega. Si solo desea permitir el acceso a ServiceBus en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región en el siguiente formato: ServiceBus.[nombre de región]. 
* **MicrosoftContainerRegistry** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Microsoft Container Registry. Si especifica *MicrosoftContainerRegistry* como valor, el tráfico a MicrosoftContainerRegistry se permite o se deniega. Si solo desea permitir el acceso a MicrosoftContainerRegistry en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región en el siguiente formato: MicrosoftContainerRegistry.[nombre de región]. 
* **AzureContainerRegistry** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Container Registry. Si especifica *AzureContainerRegistry* como valor, el tráfico a AzureContainerRegistry se permite o deniega. Si solo desea permitir el acceso a AzureContainerRegistry en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región en el siguiente formato: AzureContainerRegistry.[nombre de región]. 
* **AppService** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure App Service. Si especifica *AppService* como valor, el tráfico a AppService se permite o se deniega. Si solo desea permitir el acceso a AppService en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región en el siguiente formato: AppService.[nombre de región]. 
* **AppServiceManagement** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure App Service Management. Si especifica *AppServiceManagement* como valor, el tráfico a AppServiceManagement se permite o se deniega. Si solo desea permitir el acceso a AppServiceManagement en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región en el siguiente formato: AppServiceManagement.[nombre de región]. 
* **ApiManagement** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure API Management. Si especifica *ApiManagement* como valor, el tráfico a ApiManagement se permite o se deniega. Si solo desea permitir el acceso a ApiManagement en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región en el siguiente formato: ApiManagement.[nombre de región]. 
* **AzureConnectors** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Connectors. Si especifica *AzureConnectors* como valor, el tráfico a AzureConnectors se permite o se deniega. Si solo desea permitir el acceso a AzureConnectors en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región en el siguiente formato: AzureConnectors.[nombre de región]. 
* **GatewayManager** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Gateway Manager. Si especifica *GatewayManager* como valor, el tráfico a GatewayManager se permite o se deniega. Si solo desea permitir el acceso a GatewayManager en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región en el siguiente formato: GatewayManager.[nombre de región]. 
* **AzureDataLake** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Data Lake. Si especifica *AzureDataLake* como valor, el tráfico a AzureDataLake se permite o se deniega. 
* **AzureActiveDirectory** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Active Directory. Si especifica *AzureActiveDirectory* como valor, el tráfico a AzureActiveDirectory se permite o se deniega.  

> [!NOTE]
> Las etiquetas de servicios de los servicios de Azure indican los prefijos de dirección de la nube específica que se va a usar. No se admiten etiquetas de servicio regionales en nubes nacionales, solo en formato global. Por ejemplo, *Storage* y *Sql*.

> [!NOTE]
> Si implementa un [punto de conexión de servicio de red virtual](virtual-network-service-endpoints-overview.md) para un servicio, como Azure Storage o Azure SQL Database, Azure agrega una [ruta](virtual-networks-udr-overview.md#optional-default-routes) a una subred de red virtual para el servicio. Los prefijos de dirección de la ruta son los mismos prefijos de dirección, o intervalos CIDR, que la etiqueta de servicio correspondiente.

## <a name="default-security-rules"></a>reglas de seguridad predeterminadas

Azure crea las siguientes reglas predeterminadas en cada grupo de seguridad de red que cree:

### <a name="inbound"></a>Entrada

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Prioridad|Origen|Puertos de origen|Destino|Puertos de destino|Protocolo|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Todo|PERMITIR|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Prioridad|Origen|Puertos de origen|Destino|Puertos de destino|Protocolo|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Todo|PERMITIR|

#### <a name="denyallinbound"></a>DenyAllInbound

|Prioridad|Origen|Puertos de origen|Destino|Puertos de destino|Protocolo|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Todo|Denegar|

### <a name="outbound"></a>Salida

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Prioridad|Origen|Puertos de origen| Destino | Puertos de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Todo | PERMITIR |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Prioridad|Origen|Puertos de origen| Destino | Puertos de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Todo | PERMITIR |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Prioridad|Origen|Puertos de origen| Destino | Puertos de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Todo | Denegar |

En las columnas **Origen** y **Destino**, *VirtualNetwork*, *AzureLoadBalancer* e *Internet* son [etiquetas de servicios](#service-tags), en lugar de direcciones IP. En la columna de protocolos, **Todos** abarca TCP, UDP e ICMP. Cuando se crea una regla, puede especificar TCP, UDP o Todos, pero no puede especificar ICMP de forma independiente. Por lo tanto, si la regla requiere ICMP, seleccione la opción *Todos* para el protocolo. *0.0.0.0/0* en las columnas **Origen** y **Destino** representa todas las direcciones.
 
Las reglas predeterminadas no se pueden quitar, pero puede reemplazarlas con reglas de prioridad más alta.

## <a name="application-security-groups"></a>Grupos de seguridad de aplicaciones

Los grupos de seguridad de aplicaciones le permiten configurar la seguridad de red como una extensión natural de la estructura de una aplicación, lo que le permite agrupar máquinas virtuales y directivas de seguridad de red basadas en esos grupos. Puede reutilizar la directiva de seguridad a escala sin mantenimiento manual de direcciones IP explícitas. La plataforma controla la complejidad de las direcciones IP explícitas y de varios conjuntos de reglas, lo que le permite centrarse en su lógica de negocios. Para entender mejor los grupos de seguridad de aplicaciones, considere el ejemplo siguiente:

![Grupos de seguridad de aplicaciones](./media/security-groups/application-security-groups.png)

En la imagen anterior, *NIC1* y *NIC2* son miembros del grupo de seguridad de aplicaciones *AsgWeb*. *NIC3* es miembro del grupo de seguridad de aplicaciones *AsgLogic*. *NIC4* es miembro del grupo de seguridad de aplicaciones *AsgDb*. Aunque cada interfaz de red de este ejemplo solo es miembro de un grupo de seguridad de aplicaciones, una interfaz de red puede ser miembro de varios grupos de seguridad de aplicaciones, hasta los [límites de Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Ninguna de las interfaces de red tiene un grupo de seguridad de red asociado. *NSG1* está asociado a ambas subredes y contiene las siguientes reglas:

### <a name="allow-http-inbound-internet"></a>Allow-HTTP-Inbound-Internet

Esta regla es necesaria para permitir el tráfico de Internet a los servidores web. Dado que la regla de seguridad predeterminada [DenyAllInbound](#denyallinbound) deniega el tráfico entrante desde Internet, no es necesaria ninguna regla adicional para los grupos de seguridad de aplicaciones *AsgLogic* y *AsgDb*.

|Prioridad|Origen|Puertos de origen| Destino | Puertos de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 100 | Internet | * | AsgWeb | 80 | TCP | PERMITIR |

### <a name="deny-database-all"></a>Deny-Database-All

Dado que la regla de seguridad predeterminada [AllowVNetInBound](#allowvnetinbound) permite todas las comunicaciones entre los recursos de la misma red virtual, se necesita esta regla para denegar el tráfico desde todos los recursos.

|Prioridad|Origen|Puertos de origen| Destino | Puertos de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 120 | * | * | AsgDb | 1433 | Todo | Denegar |

### <a name="allow-database-businesslogic"></a>Allow-Database-BusinessLogic

Esta regla permite el tráfico desde el grupo de seguridad de aplicaciones *AsgLogic* al grupo de seguridad de aplicaciones *AsgDb*. La prioridad de esta regla es mayor que la prioridad de la regla *Deny-Database-All*. Como resultado, esta regla se procesa antes que la regla *Deny-Database-All*, por lo que se permite el tráfico del grupo de seguridad de aplicaciones *AsgLogic*, mientras que el resto del tráfico es bloqueado.

|Prioridad|Origen|Puertos de origen| Destino | Puertos de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 110 | AsgLogic | * | AsgDb | 1433 | TCP | PERMITIR |

Las reglas que especifican un grupo de seguridad de aplicaciones como origen o destino solo se aplican a las interfaces de red que son miembros del grupo de seguridad de aplicaciones. Si la interfaz de red no es miembro de un grupo de seguridad de aplicaciones, la regla no se aplica a la interfaz de red aunque el grupo de seguridad de red esté asociado a la subred.

Los grupos de seguridad de aplicaciones presentan las siguientes restricciones:

-   Hay límites en el número de grupos de seguridad de aplicaciones que puede tener en una suscripción, así como otros límites relacionados con los grupos de seguridad de aplicaciones. Para más información, consulte el artículo acerca de los [límites de Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Puede especificar un grupo de seguridad de aplicaciones como origen y destino en una regla de seguridad. No puede especificar varios grupos de seguridad de aplicaciones en el origen o el destino.
- Todas las interfaces de red asignadas a un grupo de seguridad de aplicaciones deben existir en la misma red virtual en la que se encuentra la primera interfaz de red asignada a dicho grupo. Por ejemplo, si la primera interfaz de red asignada a un grupo de seguridad de aplicaciones llamado *AsgWeb* está en la red virtual llamada *VNet1*, todas las sucesivas interfaces de red asignadas a *ASGWeb* deben existir en *VNet1*. No se pueden agregar interfaces de red de distintas redes virtuales al mismo grupo de seguridad de aplicaciones.
- Si especifica grupos de seguridad de aplicaciones como origen y destino de una regla de seguridad, las interfaces de red de ambos grupos de seguridad de aplicaciones deben existir en la misma red virtual. Por ejemplo, si *AsgLogic* contiene interfaces de red de *VNet1* y *AsgDb* contiene interfaces de red de *VNet2*, no puede asignar *AsgLogic* como origen y *AsgDb* como destino en una regla. Todas las interfaces de red para los grupos de seguridad de aplicaciones de origen y de destino deben existir en la misma red virtual.

> [!TIP]
> Para minimizar el número de reglas de seguridad que necesita y la necesidad de cambiar las reglas, planee los grupos de seguridad de aplicaciones que necesita y cree reglas mediante etiquetas de servicio o grupos de seguridad de aplicaciones en lugar de direcciones IP individuales o intervalos de direcciones IP siempre que sea posible.

## <a name="how-traffic-is-evaluated"></a>Cómo se evalúa el tráfico

Puede implementar recursos de varios servicios de Azure en una red virtual de Azure. Para obtener una lista completa, consulte [Servicios que se pueden implementar en una red virtual](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Puede asociar cero o un grupo de seguridad de red a cada [subred](virtual-network-manage-subnet.md#change-subnet-settings) e [interfaz de red](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group) de la red virtual en una máquina virtual. El mismo grupo de seguridad de red se puede asociar a tantas interfaces de red y subredes como se desee.

La siguiente imagen ilustra los diferentes escenarios de cómo se podrían implementar grupos de seguridad de red para permitir el tráfico de red hacia y desde Internet a través del puerto TCP 80:

![Procesamiento del grupo de seguridad de red](./media/security-groups/nsg-interaction.png)

Observe la imagen anterior, junto con el siguiente texto, para entender cómo procesa Azure las reglas entrantes y salientes para los grupos de seguridad de red:

### <a name="inbound-traffic"></a>Tráfico entrante

Para el tráfico entrante, Azure procesa las reglas de un grupo de seguridad de red asociadas a una subred en primer lugar, si hay alguna y, a continuación, las reglas de un grupo de seguridad de red asociadas a la interfaz de red, si hay alguna.

- **VM1**: las reglas de seguridad de *NSG1* se procesan, ya que está asociado a *Subnet1* y *VM1* está en *Subnet1*. A menos que haya creado una regla que permita el puerto 80 de entrada, la regla de seguridad predeterminada [DenyAllInbound](#denyallinbound) deniega el tráfico y *NSG2* nunca lo evalúa, ya que *NSG2* está asociado a la interfaz de red. Si *NSG1* tiene una regla de seguridad que permite el puerto 80, *NSG2* procesa el tráfico. Para permitir el puerto 80 para la máquina virtual, tanto *NSG1* como *NSG2* deben tener una regla que permita el puerto 80 desde Internet.
- **VM2**: las reglas de *NSG1* se procesan porque *VM2* también está en *Subnet1*. Puesto que *VM2* no tiene un grupo de seguridad de red asociado a su interfaz de red, recibe todo el tráfico permitido por *NSG1* o se deniega todo el tráfico denegado por *NSG1*. El tráfico se permite o deniega a todos los recursos de la misma subred cuando un grupo de seguridad de red está asociado a una subred.
- **VM3**: dado que no hay ningún grupo de seguridad de red asociado a *Subnet2*, se permite el tráfico en la subred y *NSG2* lo procesa, porque *NSG2* está asociado a la interfaz de red conectada a *VM3*.
- **VM4**: se permite el tráfico a *VM4*, porque no hay un grupo de seguridad de red asociado a *Subnet3* ni a la interfaz de red de la máquina virtual. Si no tienen un grupo de seguridad de red asociado, se permite todo el tráfico de red a través de una subred y una interfaz de red.

### <a name="outbound-traffic"></a>Tráfico saliente

Para el tráfico saliente, Azure procesa las reglas de un grupo de seguridad de red asociadas a una interfaz de red en primer lugar, si hay alguna y, a continuación, las reglas de un grupo de seguridad de red asociadas a la subred, si hay alguna.

- **VM1**: se procesan las reglas de seguridad de *NSG2*. A menos que cree una regla de seguridad que deniegue el puerto 80 de salida a Internet, la regla de seguridad predeterminada [AllowInternetOutbound](#allowinternetoutbound) permite el tráfico de *NSG1* y *NSG2*. Si *NSG2* tiene una regla de seguridad que deniega el puerto 80, el tráfico se deniega y *NSG1* nunca lo evalúa. Para denegar el puerto 80 desde la máquina virtual, uno o ambos de los grupos de seguridad de red deben tener una regla que deniegue el puerto 80 a Internet.
- **VM2**: se envía todo el tráfico a través de la interfaz de red a la subred, ya que la interfaz de red conectada a *VM2* no tiene un grupo de seguridad de red asociado. Se procesan las reglas de *NSG1*.
- **VM3**: si *NSG2* tiene una regla de seguridad que deniega el puerto 80, también se deniega el tráfico. Si *NSG2* tiene una regla de seguridad que permite el puerto 80, dicho puerto tiene permitida la salida a Internet, ya que no hay un grupo de seguridad de red asociado a *Subnet2*.
- **VM4**: se permite todo el tráfico de red desde *VM4*, porque no hay un grupo de seguridad de red asociado a la interfaz de red conectada a la máquina virtual ni a *Subnet3*.

Puede ver fácilmente las reglas agregadas que se aplican a una interfaz de red mediante la visualización de las [reglas de seguridad vigentes](virtual-network-network-interface.md#view-effective-security-rules) de una interfaz de red. También puede usar la funcionalidad [Comprobación del flujo de IP](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de Azure Network Watcher para determinar si se permite la comunicación hacia una interfaz de red o desde esta. El flujo IP le indica si se permite o deniega la comunicación y qué regla de seguridad de red permite o deniega el tráfico.

> [!NOTE]
> Los grupos de seguridad de red se asocian a las subredes o a las máquinas virtuales y a los servicios en la nube que se implementan según el modelo de implementación clásica, en vez de a las interfaces de red del modelo de implementación de Resource Manager. Para más información sobre los modelos de implementación de Azure, consulte [Descripción de los modelos de implementación de Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

> [!TIP]
> A menos que tenga una razón concreta, se recomienda que asocie un grupo de seguridad de red a una subred o a una interfaz de red, pero no a ambas. Puesto que las reglas de un grupo de seguridad de red asociado a una subred pueden entrar en conflicto con las reglas de un grupo de seguridad de red asociado a una interfaz de red, puede tener problemas de comunicación inesperados que necesiten solución.

## <a name="azure-platform-considerations"></a>Consideraciones de la plataforma Azure

- **Dirección IP virtual del nodo de host**: los servicios de infraestructura básica, como DHCP, DNS, IMDS y seguimiento de estado se proporcionan a través de las direcciones IP de host virtualizadas 168.63.129.16 y 169.254.169.254. Estas direcciones IP pertenecen a Microsoft y son la únicas direcciones IP virtualizadas que se usarán en todas las regiones con este fin.
- **Licencias (Servicio de administración de claves)**: las imágenes de Windows que se ejecutan en máquinas virtuales deben tener licencia. Para garantizar que se usen licencias, se envía una solicitud a los servidores host del Servicio de administración de claves que administran dichas consultas. La solicitud de salida se realiza a través del puerto 1688. Para implementaciones que usan la configuración de la [ruta predeterminada 0.0.0.0/0](virtual-networks-udr-overview.md#default-route), se deshabilitará esta regla de plataforma.
- **Máquinas virtuales en grupos de carga equilibrada**: el puerto y el intervalo de direcciones de origen aplicados proceden del equipo de origen, no del equilibrador de carga. El puerto y el intervalo de direcciones de destino son los del equipo de destino, no los del equilibrador de carga.
- **Instancias de servicio de Azure**: instancias de varios servicios de Azure, como HDInsight, App Service Environment y Virtual Machine Scale Sets, implementados en subredes de la red virtual. Para ver una lista completa de los servicios que puede implementar en redes virtuales, consulte el artículo sobre la [Red virtual para los servicios de Azure](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Asegúrese de que conoce los requisitos de puertos de cada servicio antes de aplicar un grupo de seguridad de red a la subred en la que se implementa el recurso. Si deniega los puertos que el servicio requiere, este no funcionará correctamente.
- **Envío de correo electrónico saliente**: Microsoft recomienda usar servicios de retransmisión SMTP autenticados (que normalmente se conectan a través del puerto TCP 587, pero a menudo también de otros) para enviar correo electrónico desde Azure Virtual Machines. Los servicios de retransmisión SMTP se especializan en la reputación del remitente, con el fin de minimizar la posibilidad de que proveedores de correo electrónico de terceros rechacen los mensajes. Estos servicios de retransmisión de SMTP incluyen Exchange Online Protection y SendGrid, pero no se limitan a ellos. El uso de servicios de retransmisión de SMTP no tiene ninguna restricción en Azure, independientemente del tipo de suscripción. 

  Si ha creado la suscripción a Azure antes del 15 de noviembre de 2017, además de poder usar los servicios de retransmisión de SMTP, puede enviar el correo electrónico directamente a través del puerto TCP 25. Si ha creado la suscripción después del 15 de noviembre de 2017, es posible que no pueda enviar correo electrónico directamente a través del puerto 25. El comportamiento de la comunicación saliente a través del puerto 25 depende del tipo de suscripción que tenga, como se indica a continuación:

     - **Contrato Enterprise**: se permite la comunicación saliente a través del puerto 25. Puede enviar el correo electrónico saliente directamente desde las máquinas virtuales a los proveedores de correo electrónico externos, sin las restricciones de la plataforma Azure. 
     - **Pago por uso**: la comunicación saliente a través del puerto 25 está bloqueada en todos los recursos. Si necesita enviar correo electrónico desde una máquina virtual directamente a proveedores de correo electrónico externos (que no usan retransmisión SMTP autenticada), puede realizar una solicitud para quitar la restricción. Las solicitudes se revisan y aprueban a discreción de Microsoft y solo se conceden una vez que se han realizado las comprobaciones contra fraudes. Para realizar una solicitud, abra un caso de soporte técnico con el tipo de problema *Técnico*, *Conectividad de red virtual*, *No se puede enviar correo electrónico (SMTP/puerto 25)*. En su caso de soporte técnico, indique los motivos por los que su suscripción tiene que enviar correo electrónico directamente a los proveedores de correo electrónico, en lugar de pasar por una retransmisión SMTP autenticada. Si la suscripción está exenta, las únicas máquinas virtuales que pueden establecer comunicación saliente a través del puerto 25 son las creadas después de la fecha de exención.
     - **MSDN, Pase para Azure, Azure bajo licencia Open, Education, BizSpark y evaluación gratuita**: la comunicación saliente a través del puerto 25 está bloqueada en todos los recursos. No se pueden realizar solicitudes para quitar la restricción, ya que no se conceden solicitudes. Si necesita enviar correo electrónico desde la máquina virtual, debe usar un servicio de retransmisión SMTP.
     - **Proveedor de servicios en la nube**: los clientes que consumen recursos de Azure a través de un proveedor de servicios en la nube pueden crear una incidencia de soporte técnico con su proveedor de servicios en la nube y solicitar que el proveedor cree un caso de desbloqueo en su nombre si no se puede usar una retransmisión SMTP segura.

  Si Azure le permite enviar correo electrónico a través del puerto 25, Microsoft no garantiza de proveedores de correo electrónico vayan a aceptar correo electrónico entrante procedente de la máquina virtual. Si un proveedor específico rechaza el correo electrónico de la máquina virtual, tendrá que trabajar directamente con él para resolver los problemas de entrega de mensajes o de filtrado de correo no deseado o bien utilizar un servicio de retransmisión SMTP autenticado.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [crear un grupo de seguridad de red](tutorial-filter-network-traffic.md).
